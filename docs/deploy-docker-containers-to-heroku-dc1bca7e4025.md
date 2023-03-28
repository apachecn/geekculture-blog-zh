# 将 Docker 容器从 GitHub 部署到 Heroku

> 原文：<https://medium.com/geekculture/deploy-docker-containers-to-heroku-dc1bca7e4025?source=collection_archive---------6----------------------->

![](img/1806c85a6f824dd1f665639296dce7b5.png)

你是 Heroku 的粉丝吗，我个人很喜欢在 Heroku 上部署我的原型，并且它使用 GitHub 库开箱即用。

我的 **docker** 应用呢，最近我用 docker 给 Heroku 部署了**前端** + **后端** Monolith 应用。

*观众是谁？*

你应该熟悉`**Modern JavaScript Frameworks**`、 **GitHub** —我们将以 **NodeJs** 应用为例。

# 你可以通过三个简单的步骤做到这一点

1.  创建您的节点应用程序—(快速)

*   创建应用程序文件夹
*   `npm i express`
*   创建文件`main.js`并复制下面的代码

```
const express = require('express')
const app **=** express()
const port = 3000app.get('/', (req, res) => {
  res.send('Hello World!')
})app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```

通过运行`node main.js`进行测试。如果你得到以上信息，一切都好了。

# **第二步**——容器化你的应用程序

*   创建 Docker 文件— `Dockerfile`(无文件扩展名)

```
FROM node:14-alpine3.12
ADD ./app /app # we copied our app to container# change working directory
WORKDIR /app
RUN npm install# start application
CMD [ "node", "main.js" ] 
```

# 步骤 3:在 github 上推送您的代码

*   如果您不熟悉 GitHub，请遵循 GitHub 文档
*   转到 GitHub 存储库中的 Actions 选项卡。

![](img/8cba7b85501086624684db026f62dffe.png)

click actions tab

*   创建新工作流

![](img/7ee600194b9cfe7968304a442606a099.png)

click new workflow button

*   将以下代码添加到 action.yml 文件中

```
name: Deployon:
  push:
    branches:
      - mainjobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: akhileshns/heroku-deploy@v3.12.12 # This is the action
        with:
          heroku_api_key: ${{secrets.HEROKU_API_KEY}}
          heroku_app_name: "app-name" // name of app
          heroku_email: "[mohit.XXXX@gmail.com](mailto:mohit.official497@gmail.com)"
          usedocker: true  // make sure to be true
          docker_build_args: |
            NODE_ENV
        env:
          NODE_ENV: production
          SECRET_KEY: ${{ secrets.MY_SECRET_KEY }} 
```

*   在 **env:** 部分中，您可以为受保护的信息(如 Heroku 令牌)提供 GitHub 机密的名称
*   您需要登录 Heroku 并生成安全令牌。[此处](https://help.heroku.com/login)

```
heroku_api_key: ${{secrets.HEROKU_API_KEY}}
```

注意:—您需要从 Heroku 创建一个要在 GitHub 中使用的令牌，以便 GitHub 可以部署您的应用程序。

## **怎么考？**

只需做一些更改并提交，它应该会直接部署到 Heroku 服务器。

> https://app-name.herokuapp.com/

如果您遇到困难，请留下您的评论，我们将努力改进对初学者来说可能太短的部分。