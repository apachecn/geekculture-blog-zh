# Github 和 GitLab 解决方案

> 原文：<https://medium.com/geekculture/github-and-gitlab-solutions-76946d3c967a?source=collection_archive---------14----------------------->

## ##将 github 中超过 100MB 的文件添加到。gitignore

github 的默认设置不允许推送超过 100MB 的文件，提交和推送文件会比较麻烦。

超过 100MB

```
find . -size +100M -ls
```

增加

```
find . -size +100M | sed -e 's/^\.\///' >> .gitignore
```

移除缓存

```
git rm -r --cached .
```

## ##未反映在中。gitignore…