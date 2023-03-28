# 面向 ARM64/AMD64 的 Python 3.9 docker 容器

> 原文：<https://medium.com/geekculture/docker-container-with-python-3-9-for-arm64-amd64-f2cdf167230f?source=collection_archive---------12----------------------->

这是基于 Alpine Linux 的 Python 3.9，没有 Gnome-Keyring。也没有`-anaconda`版本或者`-slim`版本，只有一个版本。我稍微改变了操作系统级& python 包的顺序。它只包含绝对必要的依赖项，如`cffi`。它还包含`Git` & `Twine`以便于开发开箱即用的 Python 项目。它还包含`*lapack*` 包，`Fortran, gcc, etc.`来编译 numpy/scipy/etc ( `pip install numpy scipy`)将会工作，所以你可以安装它们。你甚至可以安装木星笔记本，类似于

```
docker run -i -t -p 8888:8888 alexberkovich/alpine-python39 /bin/bash -c "pip install jupyter -y --quiet && \
mkdir -p /opt/notebooks && \
jupyter notebook \
--notebook-dir=/opt/notebooks --ip='*' --port=8888 \
--no-browser --allow-root"
```

**注意:** Gnome-Keyring 被移除是因为[https://gitlab.gnome.org/GNOME/gnome-keyring/-/issues/77](https://gitlab.gnome.org/GNOME/gnome-keyring/-/issues/77)

**注意:**如果您想要 Python 3.8 或基于 Anaconda 的 docker 容器，请参见我的故事[“Docker 容器与 Python for arm 64/AMD64”](/geekculture/docker-container-with-python-for-arm64-amd64-779c3e90d293)

因此，GitHub 代码可在此处获得[https://github.com/alex-ber/alpine-python39](https://github.com/alex-ber/alpine-python39)
Docker 图片可在此处获得[https://hub . Docker . com/repository/Docker/alexberkovich/alpine-python 39](https://hub.docker.com/repository/docker/alexberkovich/alpine-python39)