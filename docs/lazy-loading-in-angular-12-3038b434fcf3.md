# 什么是懒加载，Angular 12 怎么做懒加载？

> 原文：<https://medium.com/geekculture/lazy-loading-in-angular-12-3038b434fcf3?source=collection_archive---------5----------------------->

![](img/52b04a52820feaa2ed3fe0f0014d6c3f.png)

Lazy Loading in Angular (Bhanu Maniktahla)

延迟加载并不是一个新概念。自从我们 JavaScripter 开始开发库和框架以来，它就存在了。在这篇文章中，我们将从角度探索
延迟加载，我们将创建一个非常简单的 web 应用程序来实现延迟加载的概念。

首先要做的事。什么是懒装。我们的单页应用程序(SPA)主要以两种不同的方式加载到浏览器中。

*   急切地装载
*   延迟加载

在 ***急切加载*** 中，所有的组件/模块被一次加载，因此它们增加了网络负载。对于小型应用程序来说，这是一个很好的方法，但是当应用程序增长时，它会降低加载速度，严重影响页面的性能。在 ***延迟加载的*** 应用中，所有页面不会一次加载，而是按需加载。每当请求一个特定的路由时，该组件或模块的包就作为一个块加载。通过这种方式，应用程序的性能得到了显著增强。

让我们看一个简单的例子。我们有一个 Angular 应用程序，当我们点击它的 URL(比如说— Localhost:4200)时就会加载它。现在，当我们点击 URL 加载具有多个页面的应用程序时，很有可能应用程序中的一些页面占用了大量带宽&内容，并且可能是用户现在最不需要的。我们从中了解到什么。不知何故，我们在某处犯了一个错误。我们正在加载所有无用的页面(有些页面也很重)，但是这会影响应用程序的加载时间和响应时间。

**我们如何避免？**

angular 中有一个非常复杂的特性，可以帮助我们实现延迟加载。惰性加载允许我们不在点击应用程序时加载应用程序的某些部分或组件，而是当我们点击该组件或
时，仅当用户需要/要求时才加载。

例如，假设我们有一个应用程序，该应用程序的一个页面是一个图片库，这表明它是一个沉重的页面，将需要更多的带宽来加载它。如果不延迟加载，当我们点击应用程序时，这个页面也将被加载到用户的浏览器中，即使有可能它从来没有被要求过，当然这将花费更多的时间。既然可以避免，为什么还要增加加载时间呢？？？

该页面只有在用户点击页面时才会加载。
让我们看看惰性加载概念的实际应用。考虑具有三条路线的示例:
i)书店
ii)电子商店
iii)家庭
我们将使书店延迟加载，电子组件将首先被急切地加载，稍后我们将把它改变为延迟加载，以便
如果你有一个具有某些组件的应用程序，你可以将它们改变为延迟加载。

```
 ng new lazyLoading // create a new Angular application

 cd lazyLoading // step into the application

 ng g m books --route books -m app 
```

**app.component.html**

```
 <button routerLink='books'> Books Shop </button>
 <button routerLink="electronics">Electronics Shop </button>
 <button routerLink='/'> Home </button>
 <router-outlet></router-outlet>
```

**book.module.ts :**

```
 import { NgModule } from '@angular/core';
 import { CommonModule } from '@angular/common';

 import { BooksRoutingModule } from './books-routing.module';
 import { BooksComponent } from './books.component';

 @NgModule({
   declarations: [
     BooksComponent
   ],
   imports: [
     CommonModule,
     BooksRoutingModule
   ]
 })
 export class BooksModule { }
```

**books-routing.module.ts**

```
 import { NgModule } from '@angular/core';
 import { RouterModule, Routes } from '@angular/router';
 import { BooksComponent } from './books.component';

 const routes: Routes = [{ path: 'books', component: BooksComponent }];

 @NgModule({
   imports: [RouterModule.forChild(routes)],
   exports: [RouterModule]
 })
 export class BooksRoutingModule { }
```

**应用模块 ts**

```
import { NgModule } from '@angular/core';
 import { BrowserModule } from '@angular/platform-browser';

 import { AppRoutingModule } from './app-routing.module';
 import { AppComponent } from './app.component';
 import { BooksModule } from './books/books.module';

 @NgModule({
   declarations: [
     AppComponent,

   ],
   imports: [
     BrowserModule,
     AppRoutingModule,
     BooksModule
   ],
   providers: [],
   bootstrap: [AppComponent]
 })
 export class AppModule { }
```

**app.routing.module.ts**

```
 import { NgModule } from '@angular/core';
 import { RouterModule, Routes } from '@angular/router';

 import { HomeComponent } from './home/home.component';

 const routes: Routes = [{ path: 'books', loadChildren: () =>          import('./books/books.module').then(m => m.BooksModule) }];

 @NgModule({
   imports: [RouterModule.forRoot(routes)],
   exports: [RouterModule]
 })
 export class AppRoutingModule { }
```

现在，让我们通过生成命令再创建 2 个组件:

```
 ng generate component electronics
 ng generate component home
```

现在，我们首先将家庭和电子设备添加为急切加载的组件
，然后将电子设备设置为延迟加载。

**app-routing.module.ts**

```
import { NgModule } from '@angular/core';
 import { RouterModule, Routes } from '@angular/router';

 import { HomeComponent } from './home/home.component';

 const routes: Routes = [{ path: 'books', loadChildren: () => import('./books/books.module').then(m => m.BooksModule) },

  {
    path:'electronics',
    component: ElectronicsComponent
  },
 {
   path:'',
   component: HomeComponent
 }];

 @NgModule({
   imports: [RouterModule.forRoot(routes)],
   exports: [RouterModule]
 })
 export class AppRoutingModule { }
```

**app.module.ts**

```
import { NgModule } from '@angular/core';
 import { BrowserModule } from '@angular/platform-browser';

 import { AppRoutingModule } from './app-routing.module';
 import { AppComponent } from './app.component';
 import { BooksModule } from './books/books.module';
  import { ElectronicsComponent } from './electronics/electronics/electronics.component';
 import { HomeComponent } from './home/home.component';

 @NgModule({
   declarations: [
     AppComponent,
     ElectronicsComponent,
     HomeComponent
   ],
   imports: [
     BrowserModule,
     AppRoutingModule,
     BooksModule
   ],
   providers: [],
   bootstrap: [AppComponent]
 })
 export class AppModule { }
```

让我们创造一个新的模块:电子学

```
ng generate module electronics
```

新创建的电子模块的代码应该是这样的。

**电子模块 ts**

```
 import { NgModule } from '@angular/core';
 import { CommonModule } from '@angular/common';
 import { ElectronicsComponent } from './electronics/electronics.component';

 import { RouterModule, Routes } from '@angular/router';

 const routes: Routes = [{ path: 'electronics', component: ElectronicsComponent }];
 @NgModule({
   declarations: [
     ElectronicsComponent
   ],
   imports: [
     CommonModule,
     RouterModule.forChild(routes)
   ]
 })
 export class ElectronicsModule { }
```

现在让我们在 app.routing.module 中延迟加载电子组件。

**app.routing.module.ts**

```
import { NgModule } from '@angular/core';
 import { RouterModule, Routes } from '@angular/router';
 import { HomeComponent } from './home/home.component';

 const routes: Routes = [{ path: 'books', loadChildren: () => import('./books/books.module').then(m => m.BooksModule) },
 { path: 'electronics', loadChildren: () => import('./electronics/electronics.module').then(m => m.ElectronicsModule) },

 {
   path:'',
   component: HomeComponent
 }];

 @NgModule({
   imports: [RouterModule.forRoot(routes)],
   exports: [RouterModule]
 })
 export class AppRoutingModule { }
```

这是一个非常基本的初学者友好的惰性加载概念的介绍。