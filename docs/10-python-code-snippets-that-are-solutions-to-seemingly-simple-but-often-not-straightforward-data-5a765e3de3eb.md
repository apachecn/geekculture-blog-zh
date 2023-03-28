# 10 个 Python 代码片段，它们解决了看似简单但往往不简单的数据任务

> 原文：<https://medium.com/geekculture/10-python-code-snippets-that-are-solutions-to-seemingly-simple-but-often-not-straightforward-data-5a765e3de3eb?source=collection_archive---------9----------------------->

## 节省在 Stackoverflow 上查找解决方案的时间

# 1.将所有字符串转换为蛇皮箱样式

Snake case 是一种通过用' _ '替换空格并将每个单词的第一个字母转换为小写字母来编写字符串的样式。

```
**from** re **import** sub**def** snake(s):
  return '_'.join(
      sub('([A-Z][a-z]+)', r' \1'…
```