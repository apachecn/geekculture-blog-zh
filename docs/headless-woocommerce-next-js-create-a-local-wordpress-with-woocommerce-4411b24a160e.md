# Headless WooCommerce & Next.js:用 WooCommerce 创建一个本地 WordPress

> 原文：<https://medium.com/geekculture/headless-woocommerce-next-js-create-a-local-wordpress-with-woocommerce-4411b24a160e?source=collection_archive---------5----------------------->

![](img/660ad24832e2b4406f92c91c0796d0b9.png)

Photo by [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这是我如何使用 WooCommerce 的 REST API 并使用 Next.js 创建一个无头网站的系列文章的第一篇，该网站可以显示产品，添加到购物车，并通过 Stripe 用卡支付结账。

我以前用 WooCommerce 创建过一个标准的 Wordpress 网站，它和模板配合得很好，但是我发现它有局限性而且很慢。我喜欢创建一个无头解决方案的想法，用 WooCommerce 处理后端，Next.js 弥合差距，提供一个快如闪电的定制前端。

理论是，如果我决定离开 WooCommerce，使用一个不同的电子商务平台，我就不需要彻底修改 UI。相反，在基本层面上，我可以重构应用程序，以使用新的 API 并将数据传递到 UI 组件中，这样它看起来相同，但从不同的来源获取数据。同样，如果我们想使用不同于 Next.js 的框架——也许我们想创建一个本地移动应用程序——那么我们可以创建新的 UI，并在不同的平台上使用相同的 API 和数据。

# 在你设置 WordPress 之前

作为一名开发人员，老实说，我正在寻找免费的东西。我想尝试 WooCommerce，但我不想支付托管费来建立一个 WordPress 网站。

我在我的另一篇文章中提到了这一点，但是 Wordpress.org 和 Wordpress.com 一开始真的让我很困惑。Wordpress 是开源软件，可以免费使用。鉴于`.com`是最常见的顶级域名，大多数人的第一反应会是看看 Wordpress.com。然而，Wordpress.com 是 Wordpress 的商业分支，因此试图从你身上赚钱。如果你使用他们的免费账户，你可以很快建立一个 Wordpress 网站，但是你必须付费使用插件——即使插件通常是免费的！

相反，去 Wordpress.org 吧，在那里你可以免费下载 Wordpress 并免费使用插件。是的，会有你必须付费的插件，但至少你有选择。

在你继续下一步之前，你需要决定在哪里托管你的网站。许多网络主机包括一键 Wordpress 安装程序，可以很容易地用它们建立一个 Wordpress 网站，但是你首先需要向网络主机提供商付费。这里的优势是你将有一个可以浏览的实时网站。

免费选项是在您的计算机上创建一个本地开发网站——您将无法在您的网络之外访问该网站，但它对于测试和开发目的来说非常方便。

使用 Stripe 时，有一个缺点需要注意。稍后，您可能希望使用 Stripe webhooks 在卡支付成功处理后执行某项功能(例如，更新 WooCommerce 订单状态)。Stripe 只允许发送 webhooks 来保护`https://`URL，而您的本地开发网站将达不到这一要求。

![](img/631087d764eb22268d599f730c4e00fb.png)

Photo by [Braden Collum](https://unsplash.com/@bradencollum?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 建立一个本地 Wordpress 站点

你需要设置一个服务器和一个数据库来启动并运行 Wordpress。有不同的选项可用，但我只使用了一种组合，我会告诉你我是如何做到的。

**设置服务器和数据库**

我们知道我们需要一台服务器和一个数据库，而 XAMPP 帮助我们实现了这两点。它是免费的，你可以在 apachefriends.org[下载并安装。单击安装程序提示，直到选择要安装的组件。您需要勾选的唯一组件是:](https://www.apachefriends.org/index.html)

```
Server:
[/] Apache
[/] MySQLProgram Languages:
[/] PHP
[/] phpMyAdmin
```

Apache 是服务器，MySQL 是数据库，PHP 是软件使用的语言(不一定要懂 PHP)，phpMyAdmin 允许你交互和管理你的数据库。

安装程序可能会要求您安装 Bitnami，但您可以忽略这一点。

安装完成后，您需要运行 XAMPP(记得重启或关闭计算机后也要运行 XAMPP ),然后在 Apache 服务器上单击 start，在 MySQL 数据库上单击 start。服务器和数据库启动后，您可以通过打开浏览器并导航到`[http://localhost/](http://localhost/.)`来测试它的工作情况。您应该会看到来自 XAMPP 的欢迎屏幕。

**设置 Wordpress 文件**

有了服务器和数据库，我们现在可以把注意力转向 Wordpress 本身。在 wordpress.org[下载 Wordpress 的最新版本。](https://wordpress.org/)

在 Windows 中，导航到安装 XAMPP 的文件夹(通常是`C:\xampp`)并找到`\htdocs`文件夹。它在`\htdocs`文件夹中，你将在那里保存所有本地的 Wordpress 站点。继续在`\htdocs`中创建一个新的子文件夹，并将其命名为相关的东西——我将我的命名为`\htdocs\woocommercenextjs`。现在，您需要将最近下载的 Wordpress zip 文件的内容提取到`\woocommercenextjs`子文件夹中。注意:`extract all`通常会将所有内容保存在自己的`wordpress`文件夹中。您不需要这个文件夹—您只需要直接在您的`\woocommercenextjs`文件夹中的内容。

**配置数据库**

下一步是创建一个数据库，并将其指向您的新项目。

打开 XAMPP 控制面板，点击 MySQL 的`Admin`按钮。这将打开 phpMyAdmin，帮助您管理您的数据库。这里有很多选项，但我们只想创建一个新的数据库。为此，单击`Databases`选项卡，并在“创建数据库”下的输入字段中添加您最近创建的 Windows 子文件夹的名称—在本例中为`woocommercenextjs`—在下拉框(在列表的最顶端)中选择`Collation`，然后单击`Create`。注意:您不必使用与您的 Windows 子文件夹相同的名称，但是这样做是有意义的，并且可以创建一些一致性。

**安装 Wordpress**

我们快到了。有了为我们的新项目创建的数据库，我们现在可以安装 Wordpress 了。

为此，我们打开浏览器并导航到`http://localhost/woocommercenextjs`，其中`woocommercenextjs`是您在`\htdocs`中为子文件夹选择的确切名称。从现在开始，只要你的服务器和数据库通过 XAMPP 运行，这将是你可以用来访问你的 Wordpress 站点的本地 URL 地址。

你将看到 Wordpress 安装程序，你可以点击直到你需要输入你的数据库细节。使用下面的细节，只需将数据库名称更改为您在 phpMyAdmin 中使用的名称。注意:密码字段故意留空。

```
Database Name: woocommercenextjs
Username: root
Password:
Database Host: localhost
Table Prefix: wp_
```

运行安装的其余部分，就这样！现在你可以导航到`http://localhost/woocommercenextjs`来查看你的本地 Wordpress 站点。如果你需要访问 Wordpress admin，那么你可以导航到`http://localhost/woocommercenextjs/wp-admin`。

# 安装 WooCommerce

在 WordPress 仪表盘中点击插件标签->添加新插件，然后搜索 WooCommerce。安装并激活插件。我想现在可以了。

让我们在下一篇文章中解决添加产品和配置的问题。

![](img/18ae4206bc08d57230b92e1d6bdee916.png)

Photo by [Spencer Bergen](https://unsplash.com/@shutterspence?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

[第 1 部分:用 WooCommerce 创建一个本地 WordPress】](https://leojbchan.medium.com/headless-woocommerce-next-js-create-a-local-wordpress-with-woocommerce-4411b24a160e)

[第 2 部分:用邮递员](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-woocommerce-test-with-postman-5e7859c65626)设置 WooCommerce &测试

[第 3 部分:设置 Next.js](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-next-js-b8d2193b2688)

[第 4 部分:用 TypeScript 和 Next.js 设置样式组件](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-styled-components-with-typescript-and-next-js-18cc047ccd79)

[第 5 部分:展示产品并创建订单](https://leojbchan.medium.com/headless-woocommerce-next-js-display-products-and-create-orders-593a6c5aee86)

[第 6 部分:为状态管理建立 Redux 工具包](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-redux-toolkit-for-state-management-c605adda58fe)

[第 7 部分:创建购物车](https://leojbchan.medium.com/headless-woocommerce-next-js-create-a-cart-8a3e49b90076)

[第 8 部分:设置条带并创建检验](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-stripe-and-create-checkout-d01333607a66)