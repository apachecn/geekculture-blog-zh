# Node.js 设置:Babel，Jest，Nodemon

> 原文：<https://medium.com/geekculture/node-js-setup-babel-jest-nodemon-2fed147bade5?source=collection_archive---------8----------------------->

## 我的个人 Node.js 设置指南。

我写 Node.js 代码。我如何思考我写的东西？我创造了哪些关卡让入门更容易？这篇文章将打破我对开始一个新项目的理解和考虑。

Node.js 提供了几种处理异步代码的方法:

*   复试
*   承诺
*   `async/await`语法

没有理由不能用纯回调来编写代码库。我个人觉得 async/await 语法很容易阅读。这完全取决于项目的复杂性和所涉及的个人。当我在做一个项目时，这些是我开始的步骤。

# 文件系统设置

> 创建根文件夹和`index.js`文件。初始化 NPM。安装依赖项。

```
mkdir my_app && cd my_app
echo "import fs from 'fs'" > index.js
npm init -y
npm install --save-dev @babel/core @babel/cli @babel/node
npm install --save-dev @babel/preset-env
npm install --save-dev jest
npm install --save-dev nodemon
```

## 属国

巴别塔负责将新的语言特征转换成旧的

Jest 是一个测试框架

Nodemon 是一个 CLI 界面，它包装您的节点应用程序，监视文件系统，并自动重新启动进程。

# 更新 package.json

> 配置通天塔和设置脚本。

将以下内容添加到`package.json`

```
"babel": {
  "presets": ["@babel/preset-env"]
}
```

更新`package.json`中的`scripts`

```
"scripts": {
  "test": "jest",
  "dev": "nodemon --exec babel-node index.js"
}
```

# 结论

> 简单明了，这个设置支持最新最棒的 ES6 JavaScript。

当我开始一个新的 Node.js 项目时，这个设置是我的必经之路。