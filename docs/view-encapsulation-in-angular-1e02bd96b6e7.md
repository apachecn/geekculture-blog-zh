# 以角度方式查看封装

> 原文：<https://medium.com/geekculture/view-encapsulation-in-angular-1e02bd96b6e7?source=collection_archive---------12----------------------->

## 了解 Angular 中视图封装的工作原理？

![](img/102e4ae91198ced91f552f3d7e3fac66.png)

View Encapsulation in Angular

角形部件主要由三部分组成:

1.  组件类别
2.  模板
3.  风格

我已经提到我的 AppComponent 具有来自 app.component.css 文件的样式。另外，提到视图封装也是用户的选择。模拟为 not，这是 Angular 提供的默认属性。

```
import { Component, ViewEncapsulation } from '[@angular/core](http://twitter.com/angular/core)';[@Component](http://twitter.com/Component)({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css'],
    encapsulation: ViewEncapsulation.Emulated
})
export class AppComponent {
    title = 'parent component';
}
```

在下面的 appcomponent.css 文件中，我们为 p 标记添加了蓝色，这样在 AppComponent HTML 模板中使用的所有 p 标记都将以蓝色显示。

```
p {
        color: blue;
    }
```

现在，我在下面添加了一个 AppChildComponent，它将是我的 AppComponent 的子组件。您可能认为添加到父组件的样式也必须反映在子组件中，因为我们没有向它添加任何特定的样式。因此，如果我在 AppChildComponent 中使用 p 标记，它也应该显示为蓝色。

```
import { Component } from '[@angular/core](http://twitter.com/angular/core)';[@Component](http://twitter.com/Component)({
    selector: 'app-child',
    template: `
  <h1>{{title}}</h1>
  `
})
export class AppChildComponent {
    title = 'child app';
}
```

但这不是 Angular 中的默认情况。默认情况下，*样式只作用于它们特定的组件，不会应用于其他组件*。

为了将样式应用于其他组件，而不仅仅局限于其各自的组件，我们通常在 Angular 中调整默认的封装属性。

```
[@Component](http://twitter.com/Component)({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css'],
    encapsulation: ViewEncapsulation.Emulated
})
```

封装:视图封装。Emulated 是默认属性，由我们决定是否编写它，这使得样式的作用范围局限于组件。

如果我们把它调整到视图封装。None，然后再用蓝色提供样式，比如说一个 p 标记，它同时应用于 AppComponent 和 AppChildComponent，因此不会停留在特定组件的范围内。

```
[@Component](http://twitter.com/Component)({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css'],
    encapsulation: ViewEncapsulation.None
})
export class AppComponent {
    title = 'parent component';
}
```

总结一下:

**查看封装。**仿真，在此选项中:

1.  该样式的范围将是组件。
2.  这是封装的默认值，可以写入或忽略。

**查看封装。无，本选项中的**:

1.  样式不在组件范围内。
2.  可以改，但不得不提。