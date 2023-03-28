# [2022]如何在 Vue CLI 的路由器中使用子目录进行托管(针对空白页问题)

> 原文：<https://medium.com/geekculture/2022-how-to-use-subdirectories-for-hosting-in-vue-clis-router-for-blank-page-problem-df764fc7f4a3?source=collection_archive---------5----------------------->

使用默认配置，当您运行 npm run build 命令时，会在 dist 目录中生成以下文件

*   index.html
*   css 目录
*   js 目录

**然而**，如果这个 dist 目录被直接部署到一个托管环境，比如 Firebase，将会显示一个空白页，如下所示。