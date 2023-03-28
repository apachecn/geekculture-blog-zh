# 用 Next.js Part 24 创建 WhatsApp 克隆获取好友数据

> 原文：<https://medium.com/geekculture/create-whatsapp-clone-with-next-js-part-24-get-friend-data-69cca5232937?source=collection_archive---------27----------------------->

## 应用 JavaScript es6 过滤功能获取朋友的 firestore 文档

上次，我们已经获取了聊天数据，在聊天项中，我们还想显示朋友的信息，如他的个人资料图片和显示名称。

在根文件夹下，创建“utils”文件夹。并在其中创建 getFriendData.js。

```
import { getAuth } from "@firebase/auth"import { doc, getDoc } from…
```