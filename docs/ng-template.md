# 使用<ng-template>避免在角度组件中出现大量重复的 HTML</ng-template>

> 原文：<https://medium.com/geekculture/avoiding-large-chunks-of-repetitive-html-in-an-angular-component-633c19d18c89?source=collection_archive---------8----------------------->

您一定遇到过这种情况，在这种情况下，15 行或更多的 HTML 需要在组件中重复，但使用不同/相同的数据。

每次需要的时候都写那些 15 行甚至更多的 HTML，真的很傻。相反，我们可以使用 Angular 提供的资源来优化我们的 HTML 代码，每次只有 3 行。让我们看看我们如何能做到这一点。

考虑一个必须在组件的多个位置填充不同数据的表。

**组件**:我定义了 2 个列表，它们将以表格的形式显示在组件中。为了简单起见，两个列表的列名是相同的。

```
import { Component} from ‘@angular/core’;@Component({
selector: ‘my-app’,
templateUrl: ‘./app.component.html’,
styleUrls: [ ‘./app.component.css’ ]
})export class **AppComponent** {public **secondList**=[{
“userId”: 1,
“id”: 6,
“title”: “qui ullam ratione quibusdam voluptatem quia omnis”,
“completed”: false
},
{
“userId”: 1,
“id”: 7,
“title”: “illo expedita consequatur quia in”,
“completed”: false
},
{
“userId”: 1,
“id”: 8,
“title”: “quo adipisci enim quam ut ab”,
“completed”: true
},
{
“userId”: 1,
“id”: 9,
“title”: “molestiae perspiciatis ipsa”,
“completed”: false
},
{
“userId”: 1,
“id”: 10,
“title”: “illo est ratione doloremque quia maiores aut”,
“completed”: true
},
{
“userId”: 1,
“id”: 11,
“title”: “vero rerum temporibus dolor”,
“completed”: true
},
{
“userId”: 1,
“id”: 12,
“title”: “ipsa repellendus fugit nisi”,
“completed”: true
}];public **firstList**=[{
“userId”: 1,
“id”: 1,
“title”: “delectus aut autem”,
“completed”: false
},
{
“userId”: 1,
“id”: 2,
“title”: “quis ut nam facilis et officia qui”,
“completed”: false
},
{
“userId”: 1,
“id”: 3,
“title”: “fugiat veniam minus”,
“completed”: false
},
{
“userId”: 1,
“id”: 4,
“title”: “et porro tempora”,
“completed”: true
},
{
“userId”: 1,
“id”: 5,
“title”: “laboriosam mollitia et enim quasi adipisci quia provident illum”,
“completed”: false
}]
}
```

现在让我们来看看模板。

```
**<ng-template #table let-userData=”data”>**
<table>
<tr>
<th>UserId</th>
<th>Id</th>
<th>Title</th>
<th>Completed</th>
</tr><tr *ngFor=”let row of userData”>
<td>{{row.userId}}</td>
<td>{{row.id}}</td>
<td>{{row.title}}</td>
<td>{{row.completed}}</td>
</tr>
</table>
</ng-template><ng-container **[ngTemplateOutlet]=”table” [ngTemplateOutletContext]=”{data:firstList}”**>
</ng-container><ng-container **[ngTemplateOutlet]=”table” [ngTemplateOutletContext]=”{data:secondList}”**>
</ng-container>
```

正如你所看到的，我只定义了这个表一次(14 行),并且用了两次 2 组数据，每次只用了 3 行 HTML。

我们已经向< ng-template >传递了一个名为**表**的引用，并在< ng-container >的 **ngTemplateOutlet** 中引用它来访问模板。

在 **ngTemplateOutletContext** 中，我们已经将要在表中显示的数据作为键值对进行了传递。键的值是我们已经定义的两个列表。

该键在<ng-template>中用于将列表数据注入到变量 **userData** 中。userData 用于迭代表中的行</ng-template>

```
**<ng-template #table let-userData=”data”>**
```

这是一个非常简单的例子，其中只有 14 行 HTML 代码被优化为 3 行。但也可以更多。

查看下面的链接，获取上面示例的 Stackblitz 演示:

[](https://stackblitz.com/edit/angular-ivy-p16w8r?file=src/app/app.component.html) [## angular-ivy-p16w8r - StackBlitz

### 导出到 Angular CLI 的 Angular 应用程序的启动项目

stackblitz.com](https://stackblitz.com/edit/angular-ivy-p16w8r?file=src/app/app.component.html) 

您还可以在子组件之间共享同一个模板。

考虑一个父组件 a 及其子组件 b。

```
export class **ComponentA** implements OnInit {constructor() { }ngOnInit() {}**public data:string=”hello world”;**
}
```

**组件模板:**

```
<**ng-template** **#templateFromParent** let-data=”data” let-component=”component”>
<h4>Same ng-template with different content in {{ component }}</h4>
<p>{{ data }}</p>
</ng-template><**ng-container**
**[ngTemplateOutlet]=”templateFromParent”**
**[ngTemplateOutletContext]=”{ data: data, component: ‘Parent Component’}”**></ng-container>**<app-component-b [templateFromParent]="templateFromParent"></app-component-b>**
```

在组件 a 中，我定义了引用 **templateFromParent** 的可重用模板。我想在子组件 ComponentB 中重用相同的模板，但使用不同的数据。

如您所见，我已经通过属性绑定将引用传递给了子组件。

```
**<app-component-b [templateFromParent]="templateFromParent"></app-component-b>**
```

**ComponentB 类:**

```
export class **ComponentB** implements OnInit {
constructor() { }**@Input() templateFromParent:TemplateRef<any>;**
**public data:string=”good morning”;**ngOnInit() {}
}
```

**组件 b 模板:**

```
<ng-container **[ngTemplateOutlet]=”templateFromParent”**
**[ngTemplateOutletContext]=”{data:data,component:’Child Component’}”**></ng-container>
```

ngTemplateOutlet 现在已经被分配了从父组件 ComponentA 传来的模板引用。

因此，<ng-template>是在同一个组件中或者跨具有相同/不同数据的子组件重用同一个模板的绝佳选择。</ng-template>