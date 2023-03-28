# 用 Dockerfile 为 Ubuntu image 制作一个别名

> 原文：<https://medium.com/geekculture/make-an-alias-with-dockerfile-for-ubuntu-image-16454256ffef?source=collection_archive---------3----------------------->

## 为什么我的化名没有粘性？；_;

您不能简单地在 docker 文件中设置别名:

```
# syntax=docker/dockerfile:1FROM ubuntu:18.04RUN alias dir='ls -halp'
```

他们不坚持。你可能会奇怪为什么，既然你看起来可以用 RUN 命令轻松运行“apt-get”等命令行工具，而且那些 apt-get 的安装没有消失，那我的别名怎么会消失呢？它正在消失，因为当你…