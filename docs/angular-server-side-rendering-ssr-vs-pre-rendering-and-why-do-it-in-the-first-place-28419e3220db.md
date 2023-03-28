# 角度服务器端渲染(SSR)与预渲染，以及为什么要这样做。

> 原文：<https://medium.com/geekculture/angular-server-side-rendering-ssr-vs-pre-rendering-and-why-do-it-in-the-first-place-28419e3220db?source=collection_archive---------7----------------------->

首先简单解释一下为什么——对于用 Angular/ React 编写的应用程序，javascript 在确保你的应用程序超级高效和无缝地工作方面做得很好。然而，这对于 SEO 来说并不好，因为大多数搜索引擎并不理解 JS。生成需要爬行的 html 页面花费了太多时间。因此，服务器端渲染和预渲染都通过预先为特定 URL 生成 HTML 代码来解决这个问题，并使爬虫程序和整个网络上特定 URL 的一般共享变得容易。基本上-你应该在任何公开的页面上使用它-你的登陆页面，博客等。

所以我一直在研究服务器端的 Angular 渲染。这通常与预渲染结合使用。尽管它们使用构建在 node(universal)上的相同引擎，但它们并不相同，服务它们的方式也完全不同。

所以服务器端渲染实际上需要你在 node 上为你的前端托管一个服务器。预渲染创建了一组可直接用于客户端浏览器的 HTML 页面。

这也不应该与 prerender.io 等预呈现器提供的服务混淆，尽管它们确实试图完成类似的事情。

使用 universal 进行预渲染和使用 prerender.io 之类的服务进行预渲染之间的主要区别在于，当您自己使用 universal 进行预渲染时，您需要负责网页的托管和缓存。Prerender.io 是一项自动缓存现有页面集的服务，是应用程序和外部世界(包括客户端浏览器和机器人)之间的中间层。除此之外，像 prerender.io 这样的服务会给你提供访问过你网站的机器人的统计数据。

如果你使用过像尖叫青蛙这样的 SEO 服务，你就会知道 JavaScript 爬行是一种增值服务。JavaScript 很难爬。因此，比起依靠机器人来更好地理解你的站点，大多数开发 angular/react 的公司采用的更好的解决方案是预渲染他们的应用程序或进行服务器端渲染，以便在 HTML 页面到达客户端浏览器之前生成 HTML 页面。这使得网站加载速度更快，网站更容易被机器人抓取，在更小的移动设备上加载速度更快。

所以下一个问题是我们应该在哪里使用服务器端渲染，我们应该在哪里使用预渲染。

要点中的答案是，当你有主要的静态页面，如联系人、关于、登录页面等时，使用预渲染。这通常是一组有限的页面，可以清楚地维护和记录。当链接是动态的，尤其是处理 IDs 时，应该使用服务器端。比方说，你有一个网站，允许用户上传狗的图片，你在不同的 id 下显示这些图片，比如“/dog/:id”。在这种情况下，预渲染将要生成的每个 ID 是没有意义的，因为它可能是一个无限列表，并且为了预渲染的目的而维护大量链接的成本太高，维护起来太麻烦。注意，预渲染比服务器端渲染更快，因为最终即使在服务器端渲染的情况下，HTML 内容仍然需要在服务器端计算。在预渲染的情况下，它已经保存到您的服务器或 pre-render.io 等缓存服务中。预渲染博客可能仍然有意义，即使它可能被视为动态的。这是假设博客不会改变太频繁，将有一个有限的博客条目列表。因此，使用预渲染使它们快速加载可能很有意义。因为服务的目标是确保机器人爬行，所以不必为私有链接做预渲染或服务器端渲染也是有意义的。也就是说，如果你热衷于努力，你仍然可以让你的网站更快。

附加资源:

棱角分明的官方文档:[https://angular.io/guide/universal](https://angular.io/guide/universal)

https://www.cnc.io/en/blog/angular-server-side-rendering

[](https://christianlydemann.com/server-side-rendering-ssr-with-angular-universal/) [## 使用 Angular Universal 的服务器端渲染(SSR)

### 顾名思义，单页应用程序(SPA)是最初可以提供给客户端的单个 HTML 文档。任何…

christianlydemann.com](https://christianlydemann.com/server-side-rendering-ssr-with-angular-universal/) 

结束本文的一个快速补充说明——您可以使用 NGINX/ Apache routing 来确保您可以预先呈现和 SSR 您需要 SEO/预先生成页面标题/元数据(用于在社交媒体等上共享)的特定路径，这些路径预先以 HTML 的形式提供。对于在登录后运行的大多数应用程序来说，你并不真的需要 SSR/预渲染，可以使用 JS 作为一个单独的页面应用程序来运行。

好了，现在把你这个可怕的东西编码掉，快点！

关于我自己，我是 Bharadwaj，一个 Saas 产品的联合创始人——[https://tobu . ai](https://tobu.ai)。

我做了大量的前端工作，也涉足了很多后端工作，对新技术、人工智能、游戏、体育和音乐充满热情。