# 使用 Node Express、Elasticsearch 和 ErdLab DRM 构建电子书 API

> 原文：<https://medium.com/geekculture/build-an-ebook-api-with-node-express-elasticsearch-and-erdlab-drm-709f2551d76d?source=collection_archive---------19----------------------->

![](img/eeab9b50b536d167dcd77233c4506310.png)

Democratizing eBook DRM

小型电子书商店面临的挑战之一是托管和运营数字版权管理(DRM)基础设施的成本。市场上几乎没有专有的解决方案(无需在此提及名称),它们要么使用成本很高，要么在它们的平台上吸你的血。

[ErdLab](https://www.edrlab.org/) 凭借其开源的镭 LCP 技术，试图通过提供服务器技术和一些开发工具来使 DRM 空间民主化，以便能够建立运营成本低廉的电子书商店。

下面的示例 API，涵盖了一些您期望从 eStore 中获得的基本许可操作。

它使用:

*   承载 API 的节点快车
*   确保 API 安全的 Okta Node Express 中间件
*   [ErdLab 许可证服务器](https://github.com/readium/readium-lcp-server/wiki)和 API 来(d)加密内容并提供 DRM 许可
*   作为后端 eStore 数据库的 Elasticsearch

这里涉及的一些主要模式包括:

*   使用 Okta 中间件为 Node Express 保护您的 API
*   在 API 中启用文件上传
*   使用后端 ErdLab 服务器自动化 DRM 活动
*   使用弹性搜索的 CRUD 操作
*   使用 sentry.io 进行日志记录
*   处理 ePub 格式

在我们开始之前:

*   假设熟悉 Okta 的设置并理解 jwt 令牌等认证机制
*   假设熟悉 Node Express
*   假设熟悉 [ErdLab](https://www.edrlab.org/) DRM 许可技术
*   假设 ErdLab 许可证服务器安装并运行在部署此 API 的同一台计算机上。检查安装文件[此处](https://github.com/readium/readium-lcp-server/wiki)
*   假设对 Elasticsearch CRUD 操作和节点客户端有很好的理解
*   假设 nodejs > 10 已经安装并运行，并且熟悉 npm init
*   访问整个 git repo [这里](https://gitlab.com/fujkani/ebookapi)

有意思？让我们开始吧:

*   创建名为 eBookAPI 的新文件夹
*   打开一个终端窗口，转到上面的文件夹并运行“npm init”
*   安装以下软件包:

`npm i @elastic/elasticsearch @okta/jwt-verifier [@sentry/node](https://npmjs.com/package/@sentry/node)" @sentry/tracing @axios @body-parser @cors @dotenv @epub @express @ express-fileupload @log4js @request-promise @uuid @util @morgan`

*   创建新的 index.js 文件并粘贴:

```
//Entry point index.js
//Exposes main REST endpoints

require('dotenv').config();
const express = require('express');
const fileUpload = require('express-fileupload');
const bodyParser = require('body-parser');
const { promisify } = require('util');

const cors = require('cors');
const morgan = require('morgan');
const _ = require('lodash');
const { v4: uuidv4 } = require('uuid');
var crypto = require('crypto');

const authMiddleware = require('./auth');
const helperLCPencrypt = require('./helperLCPencrypt');
const helperES = require('./helperES');
const helperLCPServer = require('./helperLCPServer');
const helperEPUB = require('./helperEPUB');

const Sentry =  require("@sentry/node");
const Tracing = require("@sentry/tracing");
const SENTRY_DSN = process.env.SENTRY_DSN;

const app = express();

Sentry.init({
  dsn: SENTRY_DSN,
  integrations: [
    // enable HTTP calls tracing
    new Sentry.Integrations.Http({ tracing: true }),
    // enable Express.js middleware tracing
    new Tracing.Integrations.Express({ app }),
  ],
  tracesSampleRate: 1.0,
});

app.use(Sentry.Handlers.requestHandler());
app.use(Sentry.Handlers.tracingHandler());

// enable files upload
app.use(fileUpload({
  createParentPath: true,
  limits: { 
      fileSize: 200 * 1024 * 1024 * 1024 //200MB max file(s) size
  },
  abortOnLimit: true
}));

//add other middleware
app.use(cors());
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({extended: true}));
app.use(morgan('dev'));
app.use(authMiddleware)

app.get('/hello', (req, res) => {
  res.send('Hello World!')

  console.log(authMiddleware)
});

//#region publisher endpoints

//Uploads and stores a new .epub
app.post('/publisher/contents/uploadepub', async (req, res) => {
  var ret = ''
  const BookClubReaderId = authMiddleware.BookClubReaderId

  try {
      if(!req.files) {
          res.send({
              status: false,
              message: 'No file uploaded'
          });
      } 
      else {
        //Using name of the input field to retrieve the uploaded file
        let publicationFile = req.files.publication;

        //console.log(req.body.publicationinfo);
        const contentid = uuidv4()
        const fileName = contentid + '.epub';
        const baseUploadFolder = process.env.UPLOAD_FOLDER
        const ePubFolder = baseUploadFolder + 'epub/'

        const inputFile = ePubFolder + fileName

        const outputFile = baseUploadFolder + 'epub/out' + fileName //+ publicationFile.name
        console.log('About to move From: ' + inputFile + ' To: ' + outputFile)

        const coverPageFileName = contentid //Will set extension latter when processing the ePub
        const JPEGFolder = baseUploadFolder + 'jpeg/'
        const coverPageFile = JPEGFolder + coverPageFileName

        let pubInfoJSON = JSON.parse(req.body.publicationinfo);
        pubInfoJSON['content-id'] = contentid
        pubInfoJSON['BookClubReaderId'] = BookClubReaderId

        publicationFile.mv(inputFile, coverPageFile)                                                       //MOVE FILE TO PREFERRED LOCATION
        .then(async (respublicationFile) => {
          console.log('File MOVED  Successfully')

          await helperEPUB.readEPUBAsync(inputFile, coverPageFile, pubInfoJSON)                                                       //READ EPUB FILE
          .then(async (reshelperEPUB) => {

            console.log('EPUB read Successfully')
            pubInfoJSON = reshelperEPUB

            await helperLCPencrypt.encryptAsync(inputFile, outputFile, contentid)                         //ENCRYPT THE EPUB
            .then(async (reshelperLCPencrypt) => {
              console.log('Encrypted Successfully')
              const LCPEncryptJSON = JSON.parse(reshelperLCPencrypt)

              //store the Publication
              await helperLCPServer.storePublicationAsync(LCPEncryptJSON)                                 //STORE THE PUBLICATION WITH THE LCP SERVER
              .then( async (reshelperLCPServer) => {
                console.log('Store Pub successful')
                await helperES.addPublicationAsync(pubInfoJSON)                                           //STORE ON ELASTIC
                .then( helperES => {
                  console.log('Add Pub to ES successfull')
                })
                .catch(err => {
                  console.log('Add Pub to ES failed')
                  console.error(err)
                  //res.status(500).send(err);
                });
              })
              .catch(err => {
                console.log('Store Pub Failed')
                res.status(500).send(err);
              });

            })
            .catch(err => {
              console.log('LCPEncrypt failed')
              console.error(err)
              //res.status(500).send(err);
            });

          })
          .catch(err => {
            console.log('EPUB read failed')
            console.error(err)
            res.send({
              status: 500,
              message: err
            });

          });

        })

        console.log('wrapping up again')

        res.send({
        status: true,
        message: {
            'content-id': contentid ,// publicationFile.name,
            publicationURL: process.env.CLIENT_BASE_URL +':' + process.env.SERVER_PORT + '/publisher/contents/?content-id=' + pubInfoJSON['content-id']
        }
      });

      }

  } catch (err) {
      console.log("ERROR999: " + err)
      res.status(500).send(err);
  }
});

//#endregion publisher endpoints

//#region eStore endpoints

app.post('/estore/contents/generatelicense', async (req, res) => {
  var ret = ''
  console.log(ret)

  const BookClubReaderId = authMiddleware.BookClubReaderId
  const BookClubReaderEmail = authMiddleware.email

  try {

    if(!req.body.contentid || !req.body.licenserequestinfo) {
      res.send({
          status: 500,
          message: 'Either content id or licenserequestinfo is missing on request body'
      });
    } 
    else {

      console.log('BookClubReaderId: ' + BookClubReaderId)

      let licenseRequestInfoJSON = JSON.parse(req.body.licenserequestinfo)

      let contentid = req.body.contentid

      const secret = licenseRequestInfoJSON['encryption']['user_key']['hex_value'];
      const hash = crypto.createHmac('sha256', secret)
                   .update(licenseRequestInfoJSON['encryption']['user_key']['text_hint'])
                   .digest('hex');

      licenseRequestInfoJSON['encryption']['user_key']['hex_value'] = hash
      console.log(licenseRequestInfoJSON['encryption']['user_key']['hex_value']);

       await helperLCPServer.generatePublicationLicenseAsync(contentid, licenseRequestInfoJSON)
      .then( async reshelperLCPServer => {
        console.log('LICENSE Retrieval from LCP Server successful')
        console.log(reshelperLCPServer.status)
        console.log(reshelperLCPServer.data)

        let publicationLicenseJSON = {license_data: reshelperLCPServer.data}
        await helperES.addPublicationLicenseAsync(publicationLicenseJSON)
        .then(resHelper =>{
          console.log('Store new license in ES successful')
          res.send({
            status: true,
            message: {
                'content-id': contentid,
                publicationURL: process.env.CLIENT_BASE_URL +':' + process.env.SERVER_PORT + '/estore/pub/?id=' + contentid
            },
            license_data: reshelperLCPServer.data
          });
        })
        .catch(err => {
          console.log('Store new license in ES failed')
          console.error(err)
          res.send({
            status: 500,
            message: 'License generation failed'
          });
        });

      })
      .catch(err => {
        console.log('LICENSE Retrieval from LCP Server failed')
        console.error(err)
        res.send({
          status: 500,
          message: 'License generation failed'
        });
      });

    }

  } catch (err) {
      //res.status(500).send(err);
      res.send({
        status: 500,
        message: 'Error occured',
        data: err
    });
  }
});
//#endregion eStore endpoints

app.get("/debug-sentry", function mainHandler(req, res) {
  throw new Error("Fake Sentry error!");
});

app.use(
  Sentry.Handlers.errorHandler({
    shouldHandleError(error) {
      // Capture all 404 and 500 errors
      if (error.status === 404 || error.status === 500) {
        return true;
      }
      return false;
    },
  })
);

app.use(function onError(err, req, res, next) {
  // The error id is attached to `res.sentry` to be returned
  // and optionally displayed to the user for support.
  res.statusCode = 500;
  res.end(res.sentry + "\n");
});

const startServer = async () => {

  const port = process.env.SERVER_PORT || 3000
  await promisify(app.listen).bind(app)(port)
  console.log(`Listening on port ${port}`)
}

startServer()
```

让我们从上面看一些关键的东西。js 文件:

*   Okta 认证中间件

在 index.js 中，我们需要 auth.js 文件，然后将其用作中间件:

```
const authMiddleware = require('./auth');
app.use(authMiddleware)
```

创建一个 auth.js 文件并粘贴以下内容:

```
const OktaJwtVerifier = require('@okta/jwt-verifier')const oktaJwtVerifier = new OktaJwtVerifier({ issuer: process.env.ISSUER })// Define which Okta user group can access this end point
const oktaGroup = "BookClubGroup"module.exports = async (req, res, next) => {
  try {
    const { authorization } = req.headers
    if (!authorization) throw new Error('You must send an Authorization header') const [authType, token] = authorization.trim().split(' ')
    if (authType !== 'Bearer') throw new Error('Expected a Bearer token') const { claims } = await oktaJwtVerifier.verifyAccessToken(token) console.log(claims)

    if (!claims.scp.includes(process.env.SCOPE)) {
      throw new Error('Could not verify the proper scope')
    } if (!claims.BookClubReaderId){
      throw new Error('BookClubReaderId missing')
    }
    module.exports.BookClubReaderId = claims.BookClubReaderId
    module.exports.email = claims.sub next()
  } catch (error) {
    next(error.message)
  }
}
```

请注意，身份验证中间件不仅检查提供的令牌的有效性，还验证该令牌是否包含一个名为 BookClubReaderId 的声明。

*   上传新电子书的电子书发布者端点

```
/publisher/contents/uploadepub
```

这个 POST 端点检查是否存在 file 类型的请求头，然后使用 express file uploader 将文件放在本地文件夹中，以便进一步处理。

然后通过`helperEPUB.readEPUBAsync(inputFile, coverPageFile, pubInfoJSON)`将提供的 ePub 文件读入一个 JSON 对象，该对象包含重要的发布元数据(有些是通过 web 调用提供的，有些是从 ePub 中发现的)。封面。jpeg 也是从 ePub 中自动提取的。

然后使用`helperLCPencrypt.encryptAsync(inputFile, outputFile, contentid)`对 ePub 文件进行加密。

ePub 文件加密成功后，我们使用`helperLCPServer.storePublicationAsync(LCPEncryptJSON)` 将出版物存储在 ErdLab 许可证服务器上，最后将电子书信息存储到 Elasticsearch 索引`helperES.addPublicationAsync(pubInfoJSON)`中。

由于操作的依赖性，Promises 模式用于同步运行。

一旦电子书被加密并存储在 ErdLab 服务器中，我们现在就可以根据它生成许可证(这是购买书籍的用例)。

这是在`/estore/contents/generatelicense`端点下处理的

*   现在让我们看看其他的助手模块
*   crypto.js 非常简单:

```
var crypto = require('crypto');

const secret = 'abcdefg';
const hash = crypto.createHmac('sha256', secret)
                   .update('I love cupcakes')
                   .digest('hex');

console.log(hash);
```

handlerEPUB.js

```
//Module handles all communications with Elasticsearch backend
require('dotenv').config()

var fs = require('fs')
const EPub = require("epub2/node");

const log4js = require("log4js");
log4js.configure({
  appenders: { cheese: { type: "file", filename: "helperEPUB.log" } },
  categories: { default: { appenders: ["cheese"], level: "info" } }
});

module.exports =  {

    readEPUBAsync: async function(inputFile, coverPageFile, pubInfoJSON) {return await module.exports.readEPUB(inputFile, coverPageFile, pubInfoJSON);},

    readEPUB: async function(inputFile, coverPageFile, pubInfoJSON){
        return new Promise( async (resolve, reject) => {

            try{
                var coverPageFileWrite = coverPageFile
                console.log(coverPageFile)
                const logger = log4js.getLogger("cheese");

                //test opening ePub and listing some of the metadata
                let inFile = inputFile
                let imagewebroot = '/images/'
                let chapterwebroot = '/chapter/'

                await EPub.createAsync(inFile, imagewebroot, chapterwebroot)
                .then(function (epub)
                {
                    //console.log(epub.manifest.cover);

                    logger.info(epub)

                    if (epub.metadata.description) {console.log('CHANGING ABOUT');pubInfoJSON['mainEntity']['about'] = epub.metadata.description}
                    if (epub.metadata.publisher) {console.log('CHANGING PUBLISHER'); pubInfoJSON['mainEntity']['publisher'] = epub.metadata.publisher}
                    if (epub.metadata.date) {console.log('CHANGING DatePublished'); pubInfoJSON['mainEntity']['datePublished'] = epub.metadata.date}
                    if (epub.metadata.language) {console.log('CHANGING inLanguage'); pubInfoJSON['mainEntity']['inLanguage'] = epub.metadata.language}
                    if (epub.metadata.title) {console.log('CHANGING NAME'); pubInfoJSON['mainEntity']['name'] = epub.metadata.title}

                    pubInfoJSON['ePUBMETA'] = epub.metadata
                    pubInfoJSON['ePUBTOC'] = epub.toc
                    if (epub.metadata.cover)
                    {

                        epub.getImage(epub.metadata.cover, function(error, img, mimeType){
                            console.log('GOT IMAGE')

                            let fileExt  = '.jpeg'
                            if (!epub.manifest[epub.metadata.cover]['media-type'] == 'image/jpeg')
                            {
                                let fileExt  = '.png'
                            }
                            coverPageFileWrite = coverPageFile + fileExt
                            console.log(coverPageFileWrite)

                            fs.writeFile(coverPageFileWrite, img, function(err) {
                                // If an error occurred, show it and return
                                if(err) {console.error(err); reject()};
                                console.log('WRITING COVER PAGE')
                                resolve (pubInfoJSON)
                                // Successfully wrote binary contents to the file!
                            });

                        })

                    }
                    else {resolve (pubInfoJSON)}                    

                })
                .catch(function (err)
                {
                    console.log("ERROR\n-----");
                    reject()
                });

            }
            catch (error) {
                console.log('helperEPUB: ' + error)
                reject()
            }
        });
    },

};
```

handlerES.js:

```
//Module handles all communications with Elasticsearch backend
require('dotenv').config()

require('array.prototype.flatmap').shim()
const { Client } = require('@elastic/elasticsearch')
const client = new Client({
  auth: {username: 'elastic', password: process.env.ESPWD},
  cloud: {id: process.env.ESID}
})

const publicationIndexName  = process.env.ES_PUBLICATION_INDEX_NAME
const licenseIndexName = process.env.ES_LICENCE_INDEX_NAME

module.exports =  {

    //Insert a publication in elastic

    addPublicationAsync: async function(publicationJSON) {return await module.exports.addPublication(publicationJSON);},

    addPublication: function(publicationJSON){
        return new Promise((resolve, reject) => {
            try{

                //force an index create if not there already
                client.indices.create({
                    index: publicationIndexName
                }, function(error, response, status) {
                    if (error) {
                        console.log('Index already exists');
                    } else {
                        console.log("created a new index", response);
                    }
                });

                //index the document
                res = client.index({
                    index: publicationIndexName,
                    id: publicationJSON['content-id'],
                    type: 'publication',
                    body: publicationJSON
                }, function(err, resp, status) {
                    console.log(resp);
                    resolve(resp)
                });

                //resolve(res)
            }
            catch (error) {
                console.log('helperES: ' + error)
                reject()
            }
        });

    },

    getPublicationByContentIdAsync: async function(contentid) {

        try{

            var query = {bool: { must: [] }}
            query.bool.must.push({ match: { 'content-id': contentid } })
            console.log(query)
            const sort = [{'mainEntity.datePublished': {order : 'desc'}}]
            const _source = ["mesmainEntitysage"]

            const { body } = await client.search({ index: publicationIndexName, body: { size: 1, sort, query  } })

            const res = body.hits.hits.map(e => ({ _id: e._id, ...e._source }))

            console.log(res)

            return res
        }
        catch (error) {
            console.log('helperES: ' + error)
        }

    },

    addPublicationLicenseAsync: async function(publicationLicenseJSON) {return await module.exports.addPublicationLicense(publicationLicenseJSON);},

    addPublicationLicense: function(publicationLicenseJSON){
        return new Promise((resolve, reject) => {
            try{

                //force an index create if not there already
                client.indices.create({
                    index: licenseIndexName
                }, function(error, response, status) {
                    if (error) {
                        console.log('Index already exists');
                    } else {
                        console.log("created a new index", response);
                    }
                });

                //index the document
                res = client.index({
                    index: licenseIndexName,
                    id: publicationLicenseJSON['license_data']['id'],
                    type: 'license',
                    body: publicationLicenseJSON
                }, function(err, resp, status) {
                    console.log(resp);
                    resolve(resp)
                });

                //resolve(res)
            }
            catch (error) {
                console.log('helperES: ' + error)
                reject()
            }
        });

    },

};
```

handlerLCPServer.js

```
//Module handles REST calls to LCP Server
require('dotenv').config()

const axios = require('axios'); 

const crypto = require('crypto');

require('array.prototype.flatmap').shim()
const { Client } = require('@elastic/elasticsearch')
const client = new Client({
  auth: {username: 'elastic', password: process.env.ESPWD},
  cloud: {id: process.env.ESID}
})

module.exports =  {

    //Insert a publication LCP Server

    storePublicationAsync: async function(LCPEncryptJSON) {return await module.exports.storePublication(LCPEncryptJSON);},

    storePublication: function(LCPEncryptJSON){
        return new Promise((resolve, reject) => {
          try {
            const res = axios.put( process.env.LCP_SERVER_URL + '/contents/' + LCPEncryptJSON['content-id'], LCPEncryptJSON, {  
              headers: {
                'Content-Type': 'application/json'
              },
              auth: {
                username: process.env.LCP_SERVER_USERNAME,
                password: process.env.LCP_SERVER_PASSWORD
              }
            })
            .then(res => {
                console.log(res)
                resolve(res)
            })
            .catch(err => {
                console.log(err)
                reject(err)
            });

          }

          catch (error) {
              console.log('helperES: ' + error)
          }    
        });

    },

    generatePublicationLicenseAsync: async function(contentId, licenseJSON) {return await module.exports.generatePublicationLicense(contentId, licenseJSON);},

    generatePublicationLicense: function(contentId, licenseJSON){
      return new Promise((resolve, reject) => {
        try {

          let hash = crypto.createHash("sha256")
          .update(licenseJSON['encryption']['user_key']['hex_value'])
          .digest("hex");

          console.log('HASH: ' + hash)

          licenseJSON['encryption']['user_key']['hex_value'] = hash
          const res = axios.post( process.env.LCP_SERVER_URL + '/contents/' + contentId + '/license', licenseJSON, { 
            headers: {
              'Content-Type': 'application/json'
            },
            auth: {
              username: process.env.LCP_SERVER_USERNAME,
              password: process.env.LCP_SERVER_PASSWORD
            }
          })
          .then(res => {
              resolve(res)
          })
          .catch(err => {
              console.log(err)
              reject(err)
          });

        }

        catch (error) {
            console.log('helperES: ' + error)
        }    
      });

  },

};
```

最后是 handlerLCPEncrypt.js

```
require('dotenv').config()

const { spawn } = require('child_process');

module.exports =  {

    encryptAsync: async function(inputFile, outFile, contentid) {return await module.exports.encrypt(inputFile, outFile, contentid);},

    encrypt: function(inputFile, outFile, contentid){
        return new Promise((resolve, reject) => {

        var result = ''

        try {
            const lcpencrypt = spawn(process.env.LCPENCRYPT_LOCATION, ['-input', inputFile, '-contentid', contentid, '-output', outFile]);

            const ret = lcpencrypt.stdout.on('data', (data) => {

                const stdOutput = `${data}`
                jsonPart = stdOutput.substring(
                    stdOutput.lastIndexOf("{"), 
                    stdOutput.lastIndexOf("}") + 1
                );
                result= jsonPart
            });

            lcpencrypt.stderr.on('data', (data) => {
                console.error(`child stderr:\n${data}`);
                 reject({
                    status: 500,
                    headers: {
                      "Content-Type": "application/json"
                    },
                    body: {data}
                  })
                  resolve()
            });

            lcpencrypt.on('exit', function (code, signal) {
                console.log('child process exited with ' +
                    `code ${code} and signal ${signal}`);  //you want code 0 and signla null basically
                    return resolve(result)
            });

        }
        catch (error) {
            console.log('helperES: ' + error)
        }

    });

    }
};
```

这个示例 API 涵盖了很多内容。我希望这是有用的和有益的。

一些额外的信息给到目前为止还没有感到害怕的读者。

*   弹性搜索索引映射[此处](https://gitlab.com/fujkani/ebookapi/-/blob/master/elasticmappings.txt)
*   样本卷曲在这里调用