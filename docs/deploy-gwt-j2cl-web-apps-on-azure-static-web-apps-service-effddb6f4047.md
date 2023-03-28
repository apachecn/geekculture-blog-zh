# 在 Azure 静态 Web 应用服务上部署 GWT / J2CL Web 应用

> 原文：<https://medium.com/geekculture/deploy-gwt-j2cl-web-apps-on-azure-static-web-apps-service-effddb6f4047?source=collection_archive---------13----------------------->

在这个小故事中，我将向你展示如何在[微软 Azure 静态 web 应用服务](https://azure.microsoft.com/en-us/services/app-service/static/)上部署 GWT / J2CL web 应用。如果您使用[全 CSR(客户端渲染)](https://developers.google.com/web/updates/images/2019/02/rendering-on-the-web/infographic.png)来实现您的 web 应用程序，这样 web 应用程序就可以在您的 web 浏览器上运行，并充分利用客户端的计算能力，这会有一些优势。*云计算的成本就是其中之一*。在这种情况下，云提供商只需交付静态文件，而无需在他们这边做太多处理。

如果您想了解*为什么您应该拥抱网络浏览器的力量*,您可以…