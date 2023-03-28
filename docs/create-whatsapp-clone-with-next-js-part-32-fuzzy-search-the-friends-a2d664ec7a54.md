# 用 Next.js 创建 WhatsApp 克隆第 32 部分模糊搜索朋友

> 原文：<https://medium.com/geekculture/create-whatsapp-clone-with-next-js-part-32-fuzzy-search-the-friends-a2d664ec7a54?source=collection_archive---------24----------------------->

## 导入 Fuse.js 进行模糊搜索

这是 WhatsApp 克隆项目的一个额外的部分，当你搜索你的朋友时，而不是只显示所有的朋友。你想根据你的输入显示，我们可以通过使用一个名为 fuse.js 的 npm 包来做一些模糊搜索。

我们可以通过打字来添加

```
yarn add fuse.js
```

转到顶部并导入保险丝

```
import Fuse from 'fuse.js'
```