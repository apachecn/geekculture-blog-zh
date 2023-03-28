# 避免 TypeScript 后端中的相对路径地狱

> 原文：<https://medium.com/geekculture/avoid-relative-path-hell-in-typescript-backend-41417b0086b7?source=collection_archive---------5----------------------->

当我开始用 TypeScript 创建一个新的应用程序时，我倾向于在开始时使用相对导入。但是随着应用程序的增长，目录结构变得更深更复杂。有一天，我注意到地狱的相对路径如下所示。

```
import * as dbService from "../../../../services/db";
import * as routes from "../../../routes";
import * as graphql from "../../../../graphql";
....
```

幸运的是，在像 React 和 Vue 这样的 TypeScript 前端中使用导入别名是众所周知的。`tsconfig.json`捆绑器配置(如 webpack)是一个目标…