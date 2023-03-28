# 对 Typescript 中的语义版本数组进行排序

> 原文：<https://medium.com/geekculture/sorting-an-array-of-semantic-versions-in-typescript-55d65d411df2?source=collection_archive---------10----------------------->

![](img/393c0bb4e02e14964b0d58010e8cb33a.png)

Photo by [Jess Bailey](https://unsplash.com/@jessbaileydesigns?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/object-sizes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在本文中，我将向您展示如何使用定制的比较函数对语义版本字符串进行排序，我们可以将该函数用作数组排序中的回调函数

# 什么是语义版本化？

“语义版本化”是一个版本化软件的系统，其中版本由三个数字定义:主要版本号、次要版本号和补丁版本号。

这些值以字符串形式出现，如“MAJOR”。MINOR.PATCH”，所以“5.2.1”是指第五个主版本，第二个次版本，第一个补丁。从技术上讲，您可以添加更多的点和版本，但是层次结构必须保持不变。

您可能已经使用过它了——每个 package.json 都有一个——但是从来不知道它有名字。

# 对语义版本数组进行排序。

如果您发现自己必须使用语义版本字符串进行排序(比如，按时间顺序对发布列表进行排序)，我们首先需要 ***将*** 字符串拆分成其值，然后比较每个值。在这种情况下，我们将首先按主要版本排序，然后按次要版本排序，最后按补丁号排序。

```
const compareSemanticVersions = (a: string, b: string) => {

    // 1\. Split the strings into their parts.
    const a1 = a.split('.');
    const b1 = b.split('.'); // 2\. Contingency in case there's a 4th or 5th version
    const len = Math.min(a1.length, b1.length); // 3\. Look through each version number and compare.
    for (let i = 0; i < len; i++) {
        const a2 = +a1[ i ] || 0;
        const b2 = +b1[ i ] || 0;

        if (a2 !== b2) {
            return a2 > b2 ? 1 : -1;        
        } }

    // 4\. We hit this if the all checked versions so far are equal
    //
    return b1.length - a1.length;};
```

我们首先将字符串拆分成各自的部分，因此从“1.2.3”中我们会得到[“1”、“2”、“3”]。

因为我们不能假设所有传入的值都有相同数量的版本类型(例如“1.2.3”对“1.2.3.4”)，所以我们对重叠的值进行循环，所以我们需要最短的长度。

然后我们遍历每个版本号，并比较每个版本号。

最终的返回将比较版本标签的数量，并返回最小的一个，这假设没有提到的每个“版本”都是 0。

然后，我们可以通过将它传递给数组来使用它进行排序。

```
const versions = [ '1.0.0', '5.0.0', '0.2.0', '2.4.1', '1.0.1' ];const sorted = versions.sort(compareSemanticVersions);console.log(versions);
```

这会打印出以下结果:

```
> [ '0.2.0', '1.0.0', '1.0.1', '2.4.1', '5.0.0' ]
```

如果我们想反转这种排序，我们可以在排序后调用`.reverse()`。

# 按语义版本属性对对象进行排序

如果我们有一个需要排序的对象数组，并且语义版本现在是每个对象的属性，那该怎么办？我们可以更新我们的排序函数，使其更加健壮，并处理这个问题:

```
const compareSemanticVersions = (key?: string) => (a: any, b: any) => { // 1\. Split the strings into their parts.
    let a1;
    let b1;

   if (key) {
        a1 = a[ key ].split('.');
        b1 = b[ key ].split('.');
    } else {
        a1 = a.split('.');
        b1 = b.split('.');
    } // 2\. Contingency in case there's a 4th or 5th version
    const len = Math.min(a1.length, b1.length); // 3\. Look through each version number and compare.
    for (let i = 0; i < len; i++) {
        const a2 = +a1[ i ] || 0;
        const b2 = +b1[ i ] || 0;

        if (a2 !== b2) {
            return a2 > b2 ? 1 : -1;        
        } }

    // 4\. We hit this if the all checked versions so far are equal
    return b1.length - a1.length;};
```

我们所做的唯一改变是在第 1 部分，现在我们检查是否有一个键被传入，否则它只是试图分割值。我们还为函数`... = (key?: string) => (a: any, b: any) => ...`添加了一个额外的参数，这样我们现在可以传入一个键。

```
const objs = [ 
    { version: "1.0.0", label: "Apple" },
    { version: "5.0.0", label: "Orange" },
    { version: "0.2.0", label: "Banana" },
    { version: "2.4.1", label: "Pomegranate" },
    { version: "1.0.1", label: "Grape" } 
];const sorted = objs.sort(compareSemanticVersions('version'));console.log(sorted);
```

这会打印出以下结果:

```
> [
  { version: '0.2.0', label: 'Banana' },
  { version: '1.0.0', label: 'Apple' },
  { version: '1.0.1', label: 'Grape' },
  { version: '2.4.1', label: 'Pomegranate' },
  { version: '5.0.0', label: 'Orange' }
]
```

将它粘贴到一个模块中并导出，这样您就可以在需要的时候调用它。

编码快乐！