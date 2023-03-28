# 如何在 MongoDB 上存储图像

> 原文：<https://medium.com/geekculture/how-to-store-images-on-mongodb-71081a1da96f?source=collection_archive---------6----------------------->

## 使用 ExpressJS、Mongoose 和 Multer

![](img/0fdd6146a6c34587a221f0caeb1c2b47.png)

图像已经成为互联网的重要组成部分。不仅仅是网络应用程序需要图像，社交媒体已经确保用户不仅消费数据，还生产和分享数据。像 WhatsApp、Telegram 和 Discord 这样的应用也支持共享文档。因此，作为后端开发人员，处理图像并将它们存储在数据库中是必须的。对于本教程，我假设您对 ExpressJS 相当熟悉，并且能够使用 Mongoose，或者至少知道如何使用 NodeJS 的 MongoDB 本地驱动程序。我还假设您的 Express 服务器已经安装了 Mongoose，或者您正在使用 NodeJS 的原生 MongoDB 驱动程序

# 表单编码

当发出一个`POST`请求时，您需要对传递给 backed 的数据进行编码，以便它可以被容易地解析。HTML 表单提供了三种编码方法:

*   **application/x-www-form-urlencoded**:默认编码方式。创建了一长串名称-值，其中每个名称-值对由一个`=`分隔，每个对由一个`&`分隔，以便它可以被服务器解析。
*   **multipart/form-data** :当需要将文件上传到服务器时，使用这种编码。
*   **text/plain** :它们是作为 HTML 5 规范的一部分引入的，一般不会被广泛使用。

# 为什么 Express 上的图像处理不同？

当您将表单数据发送到 express 后端时，express 配备了处理`application/x-www-form-urlencoded`和`text/plain`编码的功能，但它不能处理主要用于上传文件的`multipart/form-data`编码。这就是 Multer 的用武之地。一个 node.js 中间件，将为我们处理多部分编码的表单。

# 设置您的模式

您需要为将要存储图像的集合定义一个模式`Upload.js`。如果您使用的是原生 MongoDB 驱动程序，可以跳过这一部分。

```
// Upload.js
const mongoose = require("mongoose");

const UploadSchema = new mongoose.Schema({
  fileName: {
    type: String,
    required: true,
  },
  file: {
    data: Buffer,
    contentType: String,
  },
  uploadTime: {
    type: Date,
    default: Date.now,
  },
});

module.exports = Upload = mongoose.model("upload", UploadSchema);
```

在上面的模式中，`file`块是最重要的一个，其余的可以方便地忽略以满足您的需求。

# 设置乘法器

为您的应用程序安装 Multer:

## 使用 npm:

`npm i multer`

## 使用纱线:

`yarn add multer`

现在，让我们创建一个处理文件上传的路由。但在此之前，让我们在`upload.js`中启用我们的应用程序使用 multer。

```
// upload.js
const express = require('express')
const multer  = require('multer')
//importing mongoose schema file
const Upload = require("../models/Upload");
const app = express()
//setting options for multer
const storage = multer.memoryStorage();
const upload = multer({ storage: storage });
```

这个代码片段确保文件被解析并存储在内存中。**警告**:确保你采取措施确保上传的文件不是很大，否则你可能面临拒绝服务的威胁。您可以在 multer 中使用一些选项。请看这里的[和](https://github.com/expressjs/multer#multeropts)。

# 在您的路线中使用 multer 中间件

现在您已经成功地设置了 multer，是时候在您的请求中将它作为中间件使用了。

```
app.post("/upload", upload.single("file"), async (req, res) => {
  // req.file can be used to access all file properties
  try {
    //check if the request has an image or not
    if (!req.file) {
      res.json({
        success: false,
        message: "You must provide at least 1 file"
      });
    } else {
      let imageUploadObject = {
        file: {
          data: req.file.buffer,
          contentType: req.file.mimetype
        },
        fileName: req.body.fileName
      };
      const uploadObject = new Upload(imageUploadObject);
      // saving the object into the database
      const uploadProcess = await uploadObject.save();
    }
  } catch (error) {
    console.error(error);
    res.status(500).send("Server Error");
  }
});
```

如果您希望从前端接收多个文件，也可以使用`upload.array()`代替`upload.single()`。更多关于那个[这里](https://github.com/expressjs/multer#usage)。

## 代码解释

中间件`upload.single("image")`用于告诉服务器浏览器只需要一个文件。`upload.single()`中的参数告诉 HTML 表单中文件字段的名称。使用这个中间件使我们能够在路由定义中使用`req.file`来访问接收到的文件。我们使用`req.file.buffer`和`req.file.mimetype`将文件保存在数据库中。`buffer`是接收到的文件的原始二进制数据，我们将按原样将其存储在数据库中。`req.file.mimetype`对我们来说也非常重要，因为它将告诉浏览器如何解析原始二进制数据，也就是说，将数据解释为什么，是 png 图像还是 jpeg，或者其他什么。如需了解可从`req.file`获取的其他信息，点击[此处](https://github.com/expressjs/multer#file-information)。我们必须将文件对象分成两个属性，即包含原始二进制文件的**数据**，以及包含 mimetype 的**内容类型**。

# 从前端发送数据

记住，multer 只接受文件的`multipart/form-data`。这就是为什么我们需要在前端设置相同的编码类型。

```
<form action="/profile" method="post" enctype="multipart/form-data">
  <input type="file" name="avatar" />
</form>
```

# 如何将其转换回图像？

嗯，基本上有两种方法可以做到这一点。您可以在后端将二进制数据转换为映像，然后将其发送到前端，或者将二进制数据发送到前端，然后将其转换为映像。这完全取决于你的喜好和你的用例。怎么做？嗯，那篇文章是为另一个 WebDev 周一准备的。

*最初发表于*[*【https://blogs.yasharyan.com】*](https://blogs.yasharyan.com/store-images-on-mongodb)*。*