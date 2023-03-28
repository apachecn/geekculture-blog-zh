# JavaScript 代理

> 原文：<https://medium.com/geekculture/javascript-proxy-45efdd1f50f7?source=collection_archive---------9----------------------->

![](img/b7835aa313f54224c386499bf50e07d9.png)

Javascript Proxy

JavaScript 代理是包装另一个对象并拦截目标对象的基本操作的对象。

***语法*** *:*

```
let proxy = new Proxy(target, handler);
```

*   `target`–是要包装的对象，可以是任何东西，包括函数。
*   `handler`–代理配置:带有“陷阱”的对象，拦截操作的方法。–例如用于读取`target`属性的`get`陷阱，用于将属性写入`target`的`set`陷阱，等等。

让我们通过定义一个名为 user 的对象来看一个简单的例子:

```
const user = {
    firstName: 'suprabha',
    lastName: 'supi',
}// defining a handler function
const handler = {}// now, create a proxy
const userProxy = new Proxy(user, handler);
```

`userProxy`对象使用`user`对象存储数据。`userProxy`可以访问`user`对象的所有属性。

让我们看看输出:

```
console.log(userProxy.firstName); // suprabha
console.log(userProxy.lastName); // supi
```

如果你修改了原来的对象`user`，这种改变会反映在`userProxy`中

```
user.firstName = 'sam';
console.log(userProxy.firstName); // sam
```

类似地，`userProxy`对象的变化将反映在原始对象`user`中:

```
proxyUser.lastName = 's';
console.log(user.lastName); // s
```

代理中有方法，这里我们将介绍最重要的**方法**:

1.  得到
2.  设置
3.  应用

# 1️.获取:

`**handler.get()**`方法是获取属性值的陷阱。

您也可以使用`get`进行更改:

```
const user = {
    firstName: 'suprabha',
    lastName: 'supi',
}// defining a handler function
const handler = {
    get(target, prop, receiver) {
    return "sumi";
  }
}// now, create a proxy
const userProxy = new Proxy(user, handler);console.log(userProxy.firstName) // sumi
console.log(userProxy.lastName)  // sumi
```

到目前为止，我们在用户对象中还没有`fullUserName`，所以让我们使用`get`陷阱在代理中创建它:

```
const user = {
    firstName: 'suprabha',
    lastName: 'supi',
}const handler = {
    get(target, property) {
        return property === 'fullUserName' ?
            `${target.firstName} ${target.lastName}` :
            target[property]
    }
};const userProxy = new Proxy(user, handler)console.log(userProxy.fullUserName) // suprabha supi
```

# 2️.设置:

`set` trap 控制设置`target`对象属性时的行为。

所以，假设你必须添加一些条件，这样你就可以在`set`陷阱中完成。

```
const user = {
    firstName: 'suprabha',
    lastName: 'supi',
        age: 15
}const handler = {
    set(target, property, value) {
        if (property === 'age') {
            if (typeof value !== 'number') {
                throw new Error('Age should be in number!');
            }
            if (value < 18) {
                throw new Error('The user must be 18 or older!')
            }
        }
        target[property] = value;
    }
};const userProxy = new Proxy(user, handler);// if you try to set age to bool, you will get error
userProxy.age = true;
// Error: Age must be a number.userProxy.age = '16';
// The user must be 18 or older.userProxy.age = '20'
// no errors would be found
```

# 3️.应用

`handler.apply()`方法是函数调用的陷阱。下面是语法:

```
let proxy = new Proxy(target, {
    apply: function(target, thisArg, args) {
        //...
    }
});
```

现在，让我们按照上面的例子，将名字和姓氏大写。

```
const user = {
    firstName: 'suprabha',
    lastName: 'supi'
}const getFullName = function (user) {
    return `${user.firstName} ${user.lastName}`;
}const getFullNameProxy = new Proxy(getFullName, {
    apply(target, thisArg, args) {
        return target(...args).toUpperCase();
    }
});console.log(getFullNameProxy(user)); // SUPRABHA SUPI
```

# 参考🧐

*   [代理 MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

🌟[推特](https://twitter.com/suprabhasupi) |📚[电子书](https://gum.co/css-pseudo-class-elements) |🌟 [Instagram](https://www.instagram.com/suprabhasupi/)