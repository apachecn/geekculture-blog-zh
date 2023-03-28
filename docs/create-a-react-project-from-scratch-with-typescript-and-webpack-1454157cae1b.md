# 使用 TypeScript 和 Webpack 从头开始创建 React 项目

> 原文：<https://medium.com/geekculture/create-a-react-project-from-scratch-with-typescript-and-webpack-1454157cae1b?source=collection_archive---------24----------------------->

如何使用 TypeScript 和 Webpack 从头开始创建 React 项目的分步指南。

你可以在这里找到完整的源代码:[https://github . com/Alex Adam/project-templates/tree/master/projects/react-app](https://github.com/alexadam/project-templates/tree/master/projects/react-app)

# 设置

先决条件:

*   结节
*   故事

创建项目的文件夹:

```
mkdir react-app 
cd react-app
```

用 yarn 生成一个默认的 **package.json** 文件:

```
yarn init -y
```

安装 React、TypeScript 和 Webpack:

```
yarn add react react-dom

yarn add --dev @types/react \
        @types/react-dom \
        awesome-typescript-loader \
        css-loader \
        html-webpack-plugin \
        node-sass \
        sass-loader \
        style-loader \
        typescript \
        webpack \
        webpack-cli \
        webpack-dev-server
```

在 **package.json** 文件中添加构建、开发和清理脚本:

```
....
  },
  "scripts": {
    "clean": "rm -rf dist/*",
    "build": "webpack",
    "dev": "webpack serve"
  }
```

通过使用以下内容创建文件 **tsconfig.json** 来配置 TypeScript:

```
{
  "compilerOptions": {
    "incremental": true,
    "target": "es5",
    "module": "commonjs",
    "lib": ["dom", "dom.iterable", "es6"],
    "allowJs": true,
    "jsx": "react",
    "sourceMap": true,
    "outDir": "./dist/",
    "rootDir": ".",
    "removeComments": true,
    "strict": true,
    "moduleResolution": "node",            
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "experimentalDecorators": true
  },
  "include": [
    "client"
  ],
  "exclude": [
    "node_modules",
    "build",
    "dist"
  ]
}
```

要配置 Webpack，创建一个文件 **webpack.config.js** 并粘贴:

```
const path = require("path");

const app_dir = __dirname + '/client';

const HtmlWebpackPlugin = require('html-webpack-plugin');
const HTMLWebpackPluginConfig = new HtmlWebpackPlugin({
  template: app_dir + '/index.html',
  filename: 'index.html',
  inject: 'body'
});

const config = {
  mode: 'development',
  entry: app_dir + '/app.tsx',
  output: {
    path: __dirname + '/dist',
    filename: 'app.js',
    publicPath: '/'
  },
  module: {
    rules: [{
        test: /\.s?css$/,
        use: [
          'style-loader',
          'css-loader',
          'sass-loader'
        ]
      }, {
        test: /\.tsx?$/,
        loader: "awesome-typescript-loader",
        exclude: /(node_modules|bower_components)/
      },
      {
        test: /\.(woff|woff2|ttf|eot)(\?v=[0-9]\.[0-9]\.[0-9])?$/,
        exclude: [/node_modules/],
        loader: "file-loader"
      },
      {
        test: /\.(jpe?g|png|gif|svg)$/i,
        exclude: [/node_modules/],
        loader: "file-loader"
      },
      {
        test: /\.(pdf)$/i,
        exclude: [/node_modules/],
        loader: "file-loader",
        options: {
          name: '[name].[ext]',
        },
      },
    ]
  },
  plugins: [HTMLWebpackPluginConfig],
  resolve: {
    extensions: [".ts", ".tsx", ".js", ".jsx"]
  },
  optimization: {
    removeAvailableModules: false,
    removeEmptyChunks: false,
    splitChunks: false,
  },
  devServer: {
    port: 8080,
    // open: true,
    hot: true,
    inline: true,
    historyApiFallback: true,
  },
};
module.exports = config;
```

# 示例应用程序

创建一个名为 **client** 的文件夹(在项目的文件夹中):

```
mkdir client
cd client
```

在文件 **numbers.tsx** 中制作一个简单的 React 组件:

```
import React, {useState} from 'react';

interface INumberProps {
  initValue: number
}

const Numbers = (props: INumberProps) => {
  const [value, setValue] = useState(props.initValue)

  const onIncrement = () => {
    setValue(value + 1)
  }

  const onDecrement = () => {
    setValue(value - 1)
  }

  return (
    <div>
      Number is {value}
        <div>
          <button onClick={onIncrement}>+</button>
          <button onClick={onDecrement}>-</button>
        </div>
    </div>
  )
}
export default Numbers
```

在文件 **app.tsx** 中创建主 React 组件(入口点):

```
import * as React from 'react';
import * as ReactDOM from 'react-dom';
import Numbers from './numbers';ReactDOM.render(
    <Numbers initValue={42} />,
    document.getElementById('app') as HTMLElement
  );
```

接下来，添加**index.html**:

```
<!DOCTYPE html>
<html><head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>React TypeScript</title>
</head><body>
  <div id="app"></div>
</body>
</html>
```

然后，运行`yarn dev`并在浏览器中打开`http://localhost:8080/`。

# 将此项目用作模板

您可以将*设置*步骤保存为 shell 脚本:

```
#!/bin/sh

rm -rf node_modules
rm package.json

yarn init --yes

yarn add react react-dom

yarn add --dev @types/react \
        @types/react-dom \
        awesome-typescript-loader \
        css-loader \
        html-webpack-plugin \
        node-sass \
        sass-loader \
        style-loader \
        typescript \
        webpack \
        webpack-cli \
        webpack-dev-server

# Remove the last line
sed -i.bak '$ d' package.json && rm package.json.bak

# append the scripts commads
cat <<EOT >> package.json
   ,"scripts": {
      "clean": "rm -rf dist/*",
      "build": "webpack",
      "dev": "webpack serve"
   }
}
```

删除 **node-modules** 文件夹，当您想要启动一个新项目时，您可以将 *react-app* 的内容复制到新位置:

```
mkdir new-project
cd new-project

# copy the react-app folder content to the new project
rsync -rtv /path/to/../react-app/ .

./init.sh
```

*原载于*[*https://Alex Adam . dev*](https://alexadam.dev/blog/create-react-project.html)*。*