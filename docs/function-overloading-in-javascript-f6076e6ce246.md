# JavaScript 中的函数重载

> 原文：<https://medium.com/geekculture/function-overloading-in-javascript-f6076e6ce246?source=collection_archive---------0----------------------->

![](img/987728149f77477b598e038844cc2e80.png)

Function Overloading in JavaScript (Made with [Excalidraw](https://excalidraw.com/))

# **编程语言中的函数重载**

函数重载意味着用不同的实现创建多个函数。函数重载的规则是

1.  同一函数名用于多个函数定义。
2.  这些函数的参数类型一定不同。

# **JavaScript 中的函数重载**

JavaScript 的 ***不*** 支持函数重载吗？

```
/**
* Display Name
* @param {string} name Name
*/
function displayName(name) {
	return `Hi ${name}`;
}/**
* Display Name
* @param {string} fName First Name
* @param {string} lName Last Name
* The above function will be overwritten by this function.
*/
function displayName(fName, lName) {
	return `Hi ${fName} ${lName}`;
}displayName('Ana');    // 'Hi Ana undefined'
```

在上面的代码示例中，第二个函数将重写第一个函数。这是因为[函数声明在 JavaScript 中被提升](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function#function_declaration_hoisting)。

> *JavaScript 中的函数声明被提升到封闭函数或全局作用域的顶部。*

# **在 JavaScript 中处理相同的函数名**

第一个建议是为函数使用不同的、不言自明的名称。

否则，我们可以使用以下方法来处理同名函数。

## 默认值

使用参数的默认值。

```
function displayName(fName, lName = 'Anonymous') {
   return `Hi ${fName} ${lName}`;
}displayName('Ana');    // 'Hi Ana Anonymous'
```

## **论据**

检查参数的数量和类型。

```
function displayName(fName, lName) {
   switch(arguments.length) {
       case 2:
           return `Hi ${fName} ${lName}`;
      default:
          return `Hi ${fName}`;
     }
}displayName('Ana');    // 'Hi Ana'displayName('Ana Jhonson');    // 'Hi Ana Jhonson'
```

## 操作类型的选项

传递最后一个参数来描述函数应该执行的选项类型。我们可以使用`swtich`或`if...else`来执行所需的操作。

```
/** 
* *Simple code example to illustrate the point 3.* * Display Name 
* [@param](http://twitter.com/param) {string} param1 
* [@param](http://twitter.com/param) {string} param2 
* [@param](http://twitter.com/param) {string} option Kind of operation to be performed. 
* [@return](http://twitter.com/return) {string} 
*/function displayName(param1, param2, option) {  
    switch(option) {
       case 'JUST_NAME':
           return `Hi ${param1}`; 
       case 'FULL_NAME':
           return `Hi ${param1} ${param2}`;
      default:
          return 'No option provided.';
     }
} displayName('Ana', '', 'JUST_NAME');    // 'Hi Ana' displayName('Ana', 'Jhonson', 'FULL_NAME');    // 'Hi Ana Jhonson' displayName('Anonymous');    // 'No option provided.
```

# **结论**

JavaScript 中因提升而出现 ***无*** 函数重载。有很多方法可以处理同名函数。但是最好的方法是不要有同名的函数。

您可以将同样的方法扩展到 JavaScript 中的方法重载。**JavaScript**中没有方法重载，但是有[种方式](https://johnresig.com/blog/javascript-method-overloading/)来处理同名的方法。

# 资源

1.  [函数重载—维基百科](https://en.wikipedia.org/wiki/Function_overloading)
2.  [JavaScript 中的函数重载—geekforgeks](https://www.geeksforgeeks.org/function-overloading-in-javascript/)
3.  [JavaScript 中的函数重载—最佳实践—堆栈溢出](https://stackoverflow.com/questions/456177/function-overloading-in-javascript-best-practices)
4.  [如何在 JavaScript 中重载函数？—堆栈溢出](https://stackoverflow.com/questions/10855908/how-to-overload-functions-in-javascript)
5.  [JavaScript 方法重载—约翰·瑞西](https://johnresig.com/blog/javascript-method-overloading/)