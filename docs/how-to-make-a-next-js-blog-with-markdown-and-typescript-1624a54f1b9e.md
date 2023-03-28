# 如何用 Markdown 和 TypeScript 制作下一个 JS 博客

> 原文：<https://medium.com/geekculture/how-to-make-a-next-js-blog-with-markdown-and-typescript-1624a54f1b9e?source=collection_archive---------3----------------------->

## 使用 next js、markdown 和 typescript 创建服务器端呈现的博客

![](img/cbc7f3e618956849ffd4d08e58a52294.png)

Next js blog with markdown and typescript

# 设置

这篇中级教程将向你展示如何用 markdown 和 typescript 制作下一个 js 博客。下一个 js 是一个 React 框架，它将允许你进行 SSR(服务器端渲染)，提高它的 SEO 性能。SEO 优化将让你在谷歌搜索上增加你的社交存在。无论你是学生、自由职业者还是 web 2.0 开发人员，这都是成为专业 web 开发人员的必备技能。

启动项目最简单的方法是使用 create next app typescript 样板文件。

```
# with yarn
yarn create next-app blog --typescript# with npm
npx create-next-app blog --ts
```

之后，您将需要安装所有相关的依赖项。

灰质用于读取元数据，如缩略图、描述和标题。react-markdown 用于将 markdown 呈现为 HTML。react-syntax-highlighter 用于向呈现的 markdown 中的代码块添加语法突出显示。

```
# with yarn
yarn add gray-matter react-markdown react-syntax-highlighter
yarn add [@types/react-syntax-highlighter](http://twitter.com/types/react-syntax-highlighter) --dev# with npm
npm install gray-matter react-markdown react-syntax-highlighter
npm install  [@types/react-syntax-highlighter](http://twitter.com/types/react-syntax-highlighter) --save-dev
```

删除页面/api 目录，因为不需要它

# 创建文章

用一些模板降价文件创建一个名为 uploads 的目录。元数据由 3 个破折号包围，并且具有标题、描述和缩略图。下面是一篇文章的例子。该文件的名称将是 URL slug。

```
---
title: Eget Duis Sem Tincidunt Ac Ullamcorper Et Turpis Magna Viverradescription: risus eu lectus a consectetur aliquam nullam enim tellus urna nunc sagittis aenean aliquam ullamcorper consectetur dictumst sit, placerat eget lobortis eget elit nibh blandit scelerisque consectetur condimentum diam tempor. nisl erat semper gravida tempor aliquam suscipit a viverra molestie sit porta cras ultricies, fermentum habitasse sit semper cum eu eget lacus purus viverra cursus porttitor nisi nisl.thumbnail: [https://blogthing-strapi.cleggacus.com/uploads/0_d65573c0b9.jpg](https://blogthing-strapi.cleggacus.com/uploads/0_d65573c0b9.jpg)
---**# In Eu Sapien Tellus Id
## Ullamcorper Elit Semper Ultricies Morbi**sit at blandit cras id eu congue et platea massa lectus netus vulputate suspendisse sed, risus habitasse at purus nibh viverra elementum viverra arcu id vulputate vel. ipsum tincidunt lorem habitant dis nulla consectetur tincidunt iaculis adipiscing erat enim, ultrices etiam mollis volutpat est vestibulum aliquam lorem elit natoque metus dui est elit. mollis sit tincidunt mauris porttitor pellentesque at nisl pulvinar tortor egestas habitant hac, metus blandit scelerisque in aliquet tellus enim viverra sed eu neque placerat lobortis a. laoreet tempus posuere magna amet nec eget vitae pretium enim magnis, cras sem eget amet id risus pellentesque auctor quis nunc tincidunt tortor massa nisl velit tortor. a volutpat malesuada nisi habitasse id volutpat nibh volutpat suspendisse nunc justo elementum ac nec, elementum pulvinar enim sociis nunc eleifend malesuada platea nunc posuere aliquet ipsum.```ts
function someFunc(*thing*: *string*){
    const thing2 = thing[0];
    return thing2;
}
```
```

# 接口

在添加代码之前，最好创建一个接口目录并添加一些接口，这样我们就知道所获取的数据的结构。这些接口将利用一篇文章的元数据和信息遵循一个固定的结构。

# 成分

我们现在可以创建一个组件目录来存储项目中使用的所有组件。这将包括一个 card 组件和一个 markdown 组件，它们将保存我们的代码，以便用语法高亮显示来呈现我们的 markdown。

## 卡片组件

卡组件将接受 ArticleMeta 类型的属性 article。这是在接口 IProps 中声明的。

components/card.tsx

卡片被设计成可以放在用 CSS flex 制作的网格中。

style/card . module . CSS

## 降价成分

降价组件将获取适当的内容。内容是保存要呈现的降价代码的字符串。

为了设置 markdown 的样式，它被一个 div 标签包围，标签的类名是 markdown-body。从[https://github . com/cleggacus/next-blog-medium-tutorial/blob/master/styles/markdown.css](https://github.com/cleggacus/next-blog-medium-tutorial/blob/master/styles/markdown.css)中复制 CSS 文件并保存为 styles/markdown . CSS

将下面一行添加到 your _app.tsx 文件中，以导入 CSS 文件。

```
import '../styles/markdown.css'
```

# 页

有 2 页是需要的:一个索引页和一篇文章页。索引页将显示一个网格中的所有文章，文章页将显示文章的所有内容。

## 索引页

从删除 pages/index.tsx 中的所有内容开始。

商品被传递到主页组件并映射到卡组件。

然后我们可以用 getStaticProps 获取文章。Get static props 是一个异步函数，它将使用从该函数返回的获取数据静态生成页面。

readdirSync("uploads ")用于获取 uploads 目录中所有文件的数组。

```
const files = fs.readdirSync("uploads");
```

然后这些文件被读取并映射到一个 ArticleMeta 数组。使用 readFileSync 读取文件，并将其转换为字符串。

```
const data = fs.readFileSync(`uploads/${file}`).toString();
```

物质(字符串)。数据将返回降价的元数据。然后通过在“.”处分裂来产生废料 char 并获取索引 0 处的字符串。这将删除。md '扩展名的文件名。

```
return {
    ...matter(data).data,
    slug: file.split('.')[0]
}
```

getStaticProps 的完整代码如下。

最终的 index.tsx 文件如下面的代码所示

样式/Home.module.scss

![](img/9b343b2767766c3e0fd8934250ab3951.png)

## 文章页面

文章文件位于“pages/article/[slug]位置。tsx '

文章组件采用 ArticleInfo 类型的文章属性来创建文章页面。

文件名中的方括号用于动态路由。为了静态地生成文章页面，使用了 getStaticPaths 函数。getStaticProps 将返回包含有页面的所有路由的数组。

上传目录中的每个文件都映射到一个路由数组。路线是文章的鼻涕虫。slug 的生成方式与它在主页上的生成方式相同。

生成路径后，每个页面都会被渲染。通过 ctx 参数获取该段塞。

```
const {slug} = ctx.params;
```

文件名是通过添加'找到的。md '延伸回到段塞的末端。文件中的信息然后通过使用灰质进行分析。

物质(字符串)。数据将返回降价的元数据。

物质(字符串)。内容将返回减价的正文。

数据和内容被添加到一个名为 article 的对象中，该对象的类型为 ArticleInfo。

pages/article/[slug]的完整代码。tsx 在下面。

文章页面的 CSS 位于 styles/aricle.css

![](img/090e4a83ee202ba4f641465edace9455.png)

总之，next js 可以很容易地用作服务器端呈现 react 代码的方法。我们已经使用了 getStaticProps 和 getStaticPaths 来处理带有静态和动态路由的一般静态页面。

在[https://github.com/cleggacus/next-blog-medium-tutorial](https://github.com/cleggacus/next-blog-medium-tutorial)获得该项目的完整源代码