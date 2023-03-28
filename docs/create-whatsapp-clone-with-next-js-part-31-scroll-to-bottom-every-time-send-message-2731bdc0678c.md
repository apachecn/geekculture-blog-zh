# 使用 Next.js Part 31 创建 WhatsApp 克隆每次发送消息时滚动到底部

> 原文：<https://medium.com/geekculture/create-whatsapp-clone-with-next-js-part-31-scroll-to-bottom-every-time-send-message-2731bdc0678c?source=collection_archive---------8----------------------->

## 使用 React useRef 标记底部

在这篇文章中，我们将讨论如何在大量消息的情况下使 app 滚动到底部。我们希望该应用程序将自动滚动到底部时，消息框没有足够的空间。

从 react 导入 useRef。

```
import { useRef } from 'react';
```

创建 messageEndRef

```
const messagesEndRef =…
```