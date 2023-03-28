# 打字稿的基础知识

> 原文：<https://medium.com/geekculture/the-abcs-of-typescript-97e1917869b3?source=collection_archive---------14----------------------->

## 如何增强您的 JavaScript 开发

如果您是一名 JavaScript 开发人员，您绝对应该学习 TypeScript。这不仅会让你成为更好的开发人员，还会节省你调试的时间。不仅仅是我——[TypeScript 是 2021 年第二大最想学的语言](https://insights.stackoverflow.com/survey/2021#most-loved-dreaded-and-wanted-language-want)。

本文旨在简要概述您开始接触 TypeScript 世界所需的一切，以及为什么应该将它添加到您的开发工具包中。

TypeScript **(TS)** 是 JavaScript **(JS)** 的超集语言。TS 本身不是一门语言，而是 JS 的一个附件，它增强了 JS 的有用性和功能。TS 通过向 JS 添加“强类型”来做到这一点。

JS 是一种动态类型的语言，这意味着我们可以反复无常地使用变量声明，并且可以随意地将变量重新分配给其他变量。这对于灵活性来说非常好，但是也带来了难以管理的问题。此外，JS 可能会在运行时出现类型错误，而 TS 甚至会在您编译代码之前就发现它们。从长远来看，这会节省你大量的时间。

TS 的主要吸引力在于它能够帮助我们在开发过程中避免错误。有些 TS 代码甚至根本没有被编译成 JS！这不是浪费代码，因为通过可怕的红色下划线的力量，编写 TS 本质上使您的代码更不容易出错。

![](img/8ff5a7645014576cfd43a3967b6bbec7.png)

Source: [https://unsplash.com/photos/u_3rD02dmkw](https://unsplash.com/photos/u_3rD02dmkw)

最重要的是，TypeScript *就是* JavaScript。任何有效的 JavaScript 代码在 TypeScript 中都是有效的。这降低了学习 TypeScript 的门槛，前提是您已经掌握了 JavaScript 的基础知识。

# 是啊，但这有什么意义呢？

TS 的流行还源于它如何增强开发人员的体验，因为它允许代码编辑器通过智能代码完成来提高效率，智能代码完成为函数、对象和语法错误查询提示提供自动完成的弹出窗口。基本上，TS 的工作就像一个文字处理器，在你犯常见错误时通知你，并根据上下文自动补全单词。这反过来会为你节省大量的时间和(谷歌搜索！)写代码的时候。还有，完全开源！

# 如何设置 TypeScript

*   安装类型脚本`npm install -g typescript`
*   创建一个 tsconfig.json 文件，并插入以下内容:

```
{
  "compilerOptions": {
    "module": "commonjs",
    "rootDir": "./src",
    "outDir": "./dist"
  },
  "exclude": [
    "node_modules"
  ]
}
```

或者，使用 TSC-init 生成一个 tsconfig 文件

*   创建一个 src 目录，并在其中创建 index.ts 文件
*   写点代码。
*   从根目录使用`tsc`命令将其编译成 JS。

恭喜你！你已经写了你的第一个打字稿程序！

![](img/eece3e27ee5a6a8147716d5eb453098c.png)

Source: [https://giphy.com/gifs/sesamestreet-sesame-street-50th-anniversary-kyLYXonQYYfwYDIeZl](https://giphy.com/gifs/sesamestreet-sesame-street-50th-anniversary-kyLYXonQYYfwYDIeZl)

或者，使用 VSCode，您可以将`// @ts-check`添加到任何 JS 文件的顶部，以便为该文件启用 TypeScript 检查。

需要注意的是，你写的任何 TS 代码都会被编译成 JS，因为浏览器不能直接理解 TS。

# 键入变量

TypeScript 最强大的功能是强类型变量的能力。这可以是隐式的(TS 足够聪明，可以在赋值时推断出变量的类型)，但显式声明类型通常是一种好的做法。

以下是基本类型:

```
let num: number = 5; 
let str: string = "Hello"; 
let isTrue: boolean = false; 
let x:any = "Whatever you want";
```

当使用' any '作为类型时，变量可以被重新分配，TS 不会对你发火。这就是 JS 的自然工作方式。

您也可以使用竖线“|”将一个变量分配给多种类型。

```
let age: string | number;
```

年龄变量现在可以分配(和重新分配)给字符串或数字。

当声明一个常量时，它的类型将是任何相关的值。

```
const dog = 'fido'; 
//const dog is of type 'fido', rather than type string
```

# 对象也可以有类型:

您可以指定数组中的值；

```
let nums: number[] = [1,2,3,4,5];
```

number[]表示这是一个数字数组。

您还可以使用输入功能在 JS 中创建元组:

```
const myTuple :[number, string][] = [[1, "Max"], [2, "Jim"], [3, "Cathy"]]
```

# 对象和接口

您可以用同样的方式强类型化对象:

```
const myCat: {name:string, age:number} = { name: "Mittens", age: 6 }
```

您还可以使用接口为对象类型创建一个可重用的模板。接口是指定对象结构的一种更简洁的方式。

```
interface Cat { 
  name:string, 
  age:number 
} const ourCat: Cat = { 
  name:"Smiley", 
  age: 2 
};
```

# 下面是一个实际使用的类型示例:

TypeScript 将隐式地将类型设置为 number。

```
let houseNo = 221;
```

如果您随后尝试将变量重新分配给字符串:

```
houseNo = "Baker Street";
```

TS 将抛出一个错误:“ ***类型‘string’不可赋给类型‘number***’注意，运行`tsc`，即使 TS 在抱怨，仍然会生成一个 JS 文件。从技术上讲，没有什么能阻止你忽略所有这些错误(当然，除了你燃烧的良心)。

起初，将类型一致性强加到松散类型的 JS 上似乎是一件痛苦的事情。之前一切都很好，对吧？为什么我现在要做更多的工作？

嗯，TS 实际上帮了你一个大忙。确定一个变量是某种类型的，或者一个函数返回一种特定的类型，可以确保你的输出总是你所期望的。

这里有一个例子:

```
function addFive(num){
  return num + 5;
}
```

上面的函数非常简单，你会期望输出是输入加 5。

```
addFive(50) // --> 55
```

看起来不错，但是如果输入的不是数字呢？

```
addFive("50") // --> "505"
```

不完全是我们想要的。

![](img/7d4a97b54cd2598f1c4ebbf374a71a47.png)

Source: [https://unsplash.com/photos/i4vGvr0YU4c](https://unsplash.com/photos/i4vGvr0YU4c)

确保我们的变量是我们所期望的，可以防止这样的错误发生。这可能看起来微不足道，但对于安全操作，如金融交易，您需要确保您正在执行的任务完全按照您预期的方式进行。相信我，人们对自己的财务状况很敏感。

我们可以更进一步，对函数参数和输出使用类型检查。

```
function addFive(input: number): number{
  return input + 5;
}
```

通过明确列出预期的输入和输出类型，您可以确定您的代码不会在您不知道的情况下发出任何奇怪的东西。很可爱，对吧？

但是等等？如果你不知道你的预期输出应该是什么呢？然后，重新评估你正在编写的代码试图完成什么是一个好主意。

通常，您会希望代码在给定相同输入的情况下返回完全相同的结果。这是函数式编程的一个基本方面。不管怎样，这是使用 ts 的好处之一，它会让你更清楚你程序的内部结构。您可以在发布之前发现这些错误，而不是匆忙修复一个根本不需要的 bug。

# 可选参数和返回:

通常，您会发现自己的函数带有一个并不明确需要的参数:

```
const adoptCat = (name: string, breed: string, age?: number):string => { 
  return `Name: ${name}\\nBreed: ${breed}\\nAge: ${age ? age : "unknown"}\\nWelcome home!`}
```

无论您是否向它传递年龄参数，该函数都会工作，因此如果您不提供参数，TS 不会报错，但是如果您省略了姓名或品种参数，它就会报错。

函数既可以返回值，也可以空值。当函数不返回显式值时(使用 return 关键字，或 arrow 函数的隐式返回)，它将返回 void。

```
const greeting = (name: string):void => {
  console.log(`What's poppin' ${name}?`) 
}
```

同样，我们不需要显式声明返回类型，但这是一个很好的做法。

还有更多的概念要涉及，但是理解原语、对象和函数的显式类型化的基础是涉水的良好基础。

# 后续步骤:

尝试使用 TS 构建一个简单的项目。回到你的初学者时代，尝试使用 TS 重构一个旧的 JS 项目。您将很快了解 TS 的代码完成功能有多强大。尽情享受吧，欢迎来到 JavaScript 的新时代。