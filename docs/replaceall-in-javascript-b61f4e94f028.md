# JavaScript 中的 replaceAll

> 原文：<https://medium.com/geekculture/replaceall-in-javascript-b61f4e94f028?source=collection_archive---------0----------------------->

![](img/2c87ade6cbb16db8c18e637c133bbad6.png)

replaceAll in Javascript

String.prototype.replaceAll()用另一个字符串值替换所有出现的字符串。

**语法:**

```
const newStr = str.replaceAll(regexp|substr, newSubstr|function)
```

有几种方法可以替换所有出现的字符串:

1.  正则表达式
2.  拆分和合并
3.  全部替换

# 1.正则表达式🙅‍♀️

```
const info = "Hi All, suprabha's account is @suprabha";
const newInfo = info.replace(/suprabha/g, "suprabha supi");
console.log(newInfo); 
// "Hi All, suprabhasupi's account is @suprabhasupi"
```

# 2.分裂并加入䷖ ⊞

使用`split`和`join`，替换所有出现的字符串。

```
const info = "Hi All, suprabha's account is @suprabha";
const newInfo = info.split('suprabha').join('suprabhasupi');
console.log(newInfo); 
// "Hi All, suprabhasupi's account is @suprabhasupi"
```

到目前为止，你已经可以用以上两种方法完全替代。现在我们有`replaceAll`帮助我们做同样的事情。

# 3.全部替换🚀

Mathias bynens 建议解决了这些问题，并给出了一种非常简单的方法来进行子串替换，即使用“replaceAll()”用另一个字符串值替换一个字符串中的所有子串实例，而不使用全局 regexp。

```
const info = "Hi All, suprabha's account is @suprabha";
const newInfo = info.replaceAll('suprabha','suprabhasupi');
console.log(newInfo); 
// "Hi All, suprabhasupi's account is @suprabhasupi"
```

也可以将 RegEx 传递给`replaceAll`中的第一个参数。

```
const info = "Hi All, suprabha's account is @suprabha";
const regex = /suprabha/ig;
const newInfo = info.replaceAll(regex,'suprabhasupi');
console.log(newInfo); 
// "Hi All, suprabhasupi's account is @suprabhasupi"
```

**注:🧨**

使用 regexp 时，必须设置全局(“g”)标志；否则会抛出一个*type error:“replace all 必须用全局 RegExp 调用”*。

还有`replace()`方法，如果输入模式是一个字符串，它只替换第一个匹配项。

```
const info = "Hi All, suprabha's account is @suprabha";
const newInfo = info.replace("suprabha", "suprabhasupi");
console.log(newInfo);
// "Hi All, suprabhasupi's account is @suprabha"
```

# 参考🧐

*   [replaceAll MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll)

🌟推特 |👩🏻‍💻 [Suprabha.me](https://www.suprabha.me/) 🌟 [Instagram](https://www.instagram.com/suprabhasupi/)