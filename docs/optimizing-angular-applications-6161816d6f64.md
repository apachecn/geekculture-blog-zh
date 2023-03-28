# 优化角度应用

> 原文：<https://medium.com/geekculture/optimizing-angular-applications-6161816d6f64?source=collection_archive---------5----------------------->

![](img/ef635d79ae506fa196da9537ab26e3c5.png)

在这篇文章中，你可以找到一些可选的技巧，帮助你优化你的角度应用，我希望你在你的项目中受益。我们开始吧。

# 关键的 CSS 内联:

这是一个很酷的新功能。在构建过程中，Angular 将解析 CSS 选择器，并尝试找到与呈现页面的匹配。因此 Angular 将决定该样式是否是关键路径的一部分，并将这些样式内联到 HTML 中。启用此功能将改善页面的第一次内容绘制指标。

您可以通过调整`angular.json`文件中的`optimization`浏览器构建器选项来启用关键的 CSS 内联:

```
"optimization": {
    "styles": {
        "minify": true,
        "inlineCritical": true // enable critical css inlining
    }}
```

如果你正在使用 Angular Universal 来 SSR 你的应用程序，并且想要启用关键的 CSS 内联，你需要在`server.ts`中启用它

```
server.engine(‘html’, ngExpressEngine({
    bootstrap: AppServerModule,
    inlineCriticalCss: true
}));
```

# 字体内嵌

这是另一种方式来改善[第一个内容丰富的油漆](https://web.dev/first-contentful-paint/)。如果你使用的是谷歌字体，你可以启用这个优化选项，angular 会在 index.html 文件中嵌入谷歌字体。这将为您节省一个额外的请求，浏览器需要这样做，以获得和渲染字体。

您也可以从`angular.json`文件中的`optimization`浏览器构建器选项启用字体内嵌:

```
"optimization": {
    "fonts": {
        "inline": **true**,// enable font inlining
    }
}
```

你可以在这里了解更多关于关键 CSS 内联和字体内联的信息[https://angular . io/guide/workspace-config # styles-optimization-optimization-options](https://angular.io/guide/workspace-config#styles-optimization-options)。

# 使用 OnPush 更改检测策略

这种策略防止 angular 在整个组件子树上运行变化检测，从而导致更快的执行。您可以通过如下方式更改`changeDetection`属性，在组件配置元数据中启用 OnPush 更改检测:

```
@Component({
    selector: 'my-company',
    templateUrl: './my-company.component.html',
    styleUrls: ['./my-company.component.scss'],
    changeDetection: ChangeDetectionStrategy.OnPush,
})
```

这将停用该组件及其子指令的自动变更检测，但保留明确调用变更检测的可能性。在这种情况下，只有三种情况会检测到元件的角度变化:

1.  组件@输出引用之一已被更改。
2.  组件或其子指令中的任何 Dom 事件。
3.  明确触发变化检测。

在这里，您可以找到两篇关于 OnPush 变更检测的详细文章:

[https://net basal . com/a-comprehensive-guide-to-angular-on push-change-detection-strategy-5 BAC 493074 a4](https://netbasal.com/a-comprehensive-guide-to-angular-onpush-change-detection-strategy-5bac493074a4)

[https://blog . angular-university . io/on push-change-detection-how-it-works/](https://blog.angular-university.io/onpush-change-detection-how-it-works/)

# 使用 Angular Universal 进行服务器端渲染

在[的上一篇文章](https://nichola.dev/user-experience-core-web-vitals-optimization-angular-universal/)中，我使用 Angular Universal 在服务器端呈现了一个示例 web 应用程序，以改善用户体验并提高核心 Web 生命体征分数。

默认情况下，角度应用程序在客户端呈现。换句话说，浏览器加载所有的应用程序 javascript 文件，引导应用程序，进行必要的 API 调用以获得要显示的数据，然后呈现最终的页面。Angular Universal 将在服务器端执行应用程序，同时生成一个静态 HTML，该 HTML 将被传递给客户端以直接呈现给用户。之后，浏览器将加载 javascript 文件，并在客户端重新整合应用程序。因此，应用程序的呈现速度会快得多，用户也能更早地看到呈现的页面。

你可以在这里找到关于如何启用和使用 Angular Universal 的全部细节[https://angular.io/guide/universal](https://angular.io/guide/universal)

# 使用延迟加载

## 模块延迟加载

默认情况下，所有的 Angular 代码都打包发送给客户端。随着您不断向应用程序添加新功能，这个包的大小可能会增加。幸运的是 Angular 支持延迟加载部分应用程序的能力。这将改善应用程序的初始加载，因此[最大内容绘制(LCP)](https://web.dev/first-contentful-paint/) 。同时 Angular 通过路由支持延迟加载。这意味着您可以延迟加载应用程序的一部分，只要它有一个单独的路由。

你可以在官方角度文档[https://angular.io/guide/lazy-loading-ngmodules](https://angular.io/guide/lazy-loading-ngmodules)中找到更多关于如何惰性加载你的角度模块的细节

## 组件延迟加载

有了新的 Ivy 渲染器，现在可以延迟加载角度组件，这是一个很棒的特性。但是，这需要一些额外的手工工作来完成，尤其是当您的组件包含一些子指令或子组件时。

在这里你可以找到一篇关于组件延迟加载如何工作的文章[https://johnpapa.net/angular-9-lazy-loading-components/](https://johnpapa.net/angular-9-lazy-loading-components/)

# 使用可用的工具

最后，您可以使用许多工具来测量应用程序的性能，这是优化过程的第一步。这些工具中的许多还为您提供了如何修复应用程序中潜在问题的建议。这些工具中最重要的可能是:

*   [Lighthouse](https://developers.google.com/web/tools/lighthouse) :开源项目，附带 chrome dev 工具，目的是提高网页质量。
*   [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/) :一个在线工具，可以生成桌面和移动设备上页面的性能报告，并为您提供如何改进和修复现有问题的广泛建议。