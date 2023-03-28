# 为 Vim 使用完整备忘录

> 原文：<https://medium.com/geekculture/usefull-memo-for-vim-741b25a745f4?source=collection_archive---------14----------------------->

![](img/aecad833774b0d8c0db346ad00556b43.png)

Photo by [David Travis](https://unsplash.com/@dtravisphd?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/notes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 检查打开文件的换行符

```
:set fileformat?
:se ff?
```

# 换行代码的可视化

## 显示换行代码

```
:se list
```

## 隐藏换行符

```
:se nolist
```

# 指定要打开的换行代码

## 低频

```
:edit ++fileformat=unix
```

## CRLF

```
:e ++ff=dos
```

## 中国国家铁路（China Railway）

```
:e ++ff=mac
```

# 指定换行代码

```
:se ff=dos
:se ff=mac
:se ff=unix
```

# 删除换行代码

```
:%s/\r//g
```

# 用指定的字符代码重新打开文件

## 在 UTF-8 中重新打开文件

```
**#Option 1**
:e ++enc=utf-8**#Option 2**
:edit ++enc=utf-8
```

# 用指定的字符代码保存文件

## 在 UTF-8 中保存文件

```
**#Option 1**
:set fileencoding=utf-8**#Option 2**
:se fenc=utf-8
```

# 检查当前编码

## Vim 编码

```
:se enc?
```

## 文件编码

```
:se fenc?
```