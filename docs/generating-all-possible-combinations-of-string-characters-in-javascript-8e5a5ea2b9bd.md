# 在 JavaScript 中生成字符串字符的所有可能组合

> 原文：<https://medium.com/geekculture/generating-all-possible-combinations-of-string-characters-in-javascript-8e5a5ea2b9bd?source=collection_archive---------8----------------------->

![](img/d84686f8533e01f364486158d99b3fbb.png)

Password Combination

不久前，我无法访问我制作的包含一些重要文档的压缩 zip 文件。我一定是在设置 zip 文件的密码时出错了。

我尝试了各种破解 zip 文件的免费软件，但它们似乎都不起作用，它们都有问题。所以我想到了另一个解决方案:如果我能生成所有可能的密码并全部尝试，会怎么样？

所有可能的密码列表将是无限的，除了密码的长度和可能的字符数是有限的。我知道我不可能输入超过 3 个字符的密码，而且所有的字符都是小写字母。换句话说，我需要生成一个不超过 3 个字符的密码列表，并且不包含小写字母 a 到 z 之外的任何其他字符— 26 个字符。

现在，给定一个字符列表以及最小和最大长度，生成的密码总数将是:

> *n^a+n^(a+1)+n^(a+2)+……+n^(b-2)+n^(b-1)+n^b*

其中:
- **n** 为可用字符数(n ≥ 1)
- **a** 为最小密码长度(**b**≥**a**≥1)
-**b**为最大密码长度(**n**≥**b**≥**a**

例如:
为一个列表人物“ ***abcd*** ”、***n****=****4***、 **a** = ***2、*** 和**b**=***4****、*

> 4² + 4³ + 4⁴ = 16 + 64 + 256 = 336

这可以用一个 JavaScript 函数来演示

```
countCombinations = (n, a, b) => {
 a = parseInt(a) || 0;
 b = Math.max(parseInt(b) || 0, a);
 let count = 0;
 if(a) {
  for (i = a; i <= b; i++) {
   count += Math.pow(n, i);
  }  
 }
 return count;
}
```

在我的例子中，对于 26 个字符的列表，***n****= 26*， **a** = 1 ***，*** 和 **b** = 3 *，*生成的密码总数将是:

> 26¹ + 26² + 26³ = 18278

如果我在 200 毫秒内测试每个密码，我将在大约一个小时内得到正确的密码。现在我需要编写一个 JavaScript 函数来生成密码

该函数接受 3 个参数:字符列表**字符**，最小密码长度 **minLength** ，最大密码长度 **maxLength** 。将使用发生器函数*。这样，我们可以像数组一样迭代密码，而不会耗尽分配的内存，即使密码的大小是无限的。你可以在[https://developer . Mozilla . org/en-US/docs/Web/Javascript/Reference/Global _ Objects/Generator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator)上阅读更多关于 Javascript 生成器函数的内容

```
function* charCombinations (chars, minLength, maxLength) {
  // Yield the passwords.
}
```

将为每个密码长度(**minLength**->**maxLength)**生成密码

```
for (i = minLength; i <= maxLength; i++) {
  // Generate all possible combinations for the current length
}
```

当前密码长度的第一个密码由第一个字符 **chars[0]** 的重复产生。

```
word = (chars[0] || '').repeat(i);
yield word;
```

当前密码长度的其他可能组合是通过总可能组合的迭代生成的，总可能组合可以通过字符总数的当前密码长度的幂来计算。

```
for (j = 1; j < Math.pow(chars.length, i); j++) {
  // Yeild a word for each iteration
}
```

密码被视为数字。设想一个字符列表，包含 10 个字符 0–9，长度为 3。下面是前 12 种组合的一个例子。

000
001
002
003
004
005
006
007
008
009
010
011

将每一行视为一个可能组合的迭代，每个组合都是根据以下规则生成的。
-最后一位数字在每次迭代时翻转。
-当一个数字翻转了所有字符后，右边的下一个数字被翻转。到迭代完成时，我们已经为当前长度 3 生成了所有可能的组合。同样的逻辑也适用于我们的函数，但是字符不必局限于 0-9。它可以是任何字符列表。

我们可以用下面的代码在 Javascript 中实现上面的逻辑。

```
//Make iteration for all indices of the word
for(k = 0; k < i; k++) {
  //check if the current index char need to be flipped
  if(!(j % Math.pow(chars.length, k))) {
    // Flip matched index char to the next
    let charIndex = chars.indexOf(word[k]) + 1;
    char = chars[charIndex < chars.length ? charIndex : 0];
    word = word.substr(0, k) + char + word.substr(k + char.length);
  }
}
// yeild the combination
yield word
```

您可以在下面的 Github Gist 中找到完整的代码

这种方法的一个常见应用是破解弱密码。下面的例子将破解所有不超过 3 个字符的密码。

```
let passwords = charCombinations('abcdefghijklmnopqrstuvwxyz1234567890ABCDEFGHIJKLMNOPQRSTUVWXYZ !"#$%&\'()*+,-./:;<=>?@[\\]^7_`{|}~', 3);let password = {};while(password = passwords.next()) {
  //tryPassword(password.value);
}
```

破解更长的密码将导致更长的密码列表，最终需要更长的时间来迭代列表。虽然上面的示例将生成 894048 个密码的列表，但是将最大密码长度增加到 4 将生成 85828704 个密码。可以调整最小密码长度和字符列表，以优化特定目标的密码列表。

该函数还可以进一步扩展，以支持升序/降序排序以及跳到特定索引的迭代。

感谢阅读。