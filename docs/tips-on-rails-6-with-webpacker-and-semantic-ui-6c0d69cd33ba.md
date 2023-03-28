# 关于具有 Webpacker 和语义 UI 的 Rails 6 的提示

> 原文：<https://medium.com/geekculture/tips-on-rails-6-with-webpacker-and-semantic-ui-6c0d69cd33ba?source=collection_archive---------18----------------------->

![](img/d0755ce250cea87811b0a0f32f6c30f1.png)

Image by [Andrew Martin](https://pixabay.com/users/aitoff-388338/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1351022) from [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1351022)

在我们最近为客户开发的内部系统中，我们大量使用 Fomantic UI——语义 UI 的一个社区分支——来构建我们的后端管理。在本文中，我将向您展示将这个 UI 框架集成到 Rails 6 的技巧，并希望您不必经历我遇到的麻烦。

如果你想了解更多关于语义 UI 的知识，可以参考我之前的介绍。

[](/swlh/plugins-and-frameworks-for-your-next-ruby-on-rails-project-5793d8dee3eb) [## 下一个 Ruby on Rails 项目的插件和框架

### 对于 web 开发项目，你必须有使用不同开源插件和前端框架的经验…

medium.com](/swlh/plugins-and-frameworks-for-your-next-ruby-on-rails-project-5793d8dee3eb) 

## 软件版本

*   Ruby 2.7.3
*   Rails 6.1

# 我们开始吧

假设您已经准备好了新的或现有的 Rails 应用程序:

1.  `yarn add fomantic-ui`
2.  `yarn --cwd node_modules/fomantic-ui run install.`我已经在`app/javascript.`下安装了我的语义目录
3.  `cd app/javascript/semantic`和`gulp build.`这个过程需要一段时间。
4.  然后将以下配置添加到`config/webpack/environment.js.`

```
const path = require('path')environment.config.merge({
  resolve: {
    alias: {
      './themes': path.resolve(__dirname, '../../app/javascript/semantic/dist/themes'),
      'semantic': path.resolve(__dirname, '../../app/javascript/semantic')
    }
  }
})
```

5.将以下几行添加到`app/javascript/packs/application.js`

```
import 'semantic/dist/semantic.js'
```

6.将以下几行添加到`app/javascript/stylesheets/application.scss`

```
@import "../semantic/dist/semantic.css";
```

7.转到`cd app/javascript/semantic`和`gulp watch.`

8.现在你可以改变你的`app/javascript/semantic/src/theme.config`，gulp 会自动编译文件。一旦完成，你必须强制 webpacker 重新构建(更新`app/javascript/packs`下的任何 js 文件)。

## 最后的想法

Fomantic UI 或 Semantic UI 确实提供了优秀的 UI 组件，具有结构化和语义命名约定。使用内置 API 的 UI 组件的状态管理和数据源集成特别有用，并使我们的代码更加整洁。如果你想知道更多这方面的内容，请在下面的评论中告诉我。下次我会写一篇关于这个的新文章。

尽情享受吧！