# JavaScript 中的调用、应用和绑定方法是什么？

> 原文：<https://medium.com/geekculture/what-are-call-apply-and-bind-methods-in-javascript-7f485c4a79eb?source=collection_archive---------6----------------------->

# 目标

了解 JavaScript 中 **Function.prototype** 的 **call()** 、 **apply()** 和 **bind()** 方法的用法。

# 先决条件

JavaScript 语言基础

# 让我们开始…

call()、apply()和 bind()方法是 Function.prototype 上可用的方法。这意味着无论何时创建函数，这些方法在默认情况下都作为函数属性可用。

> 但是这些方法的需要是什么呢？他们解决什么目的？

让我们举一个例子。看看下面的代码。

```
let addressObj = {
  state: 'Haryana',
  country: 'India',
  pincode: '123456', getAddress: function() {
    console.log(`${this.state}\n${this.country}\n${this.pincode}`);
  }
}addressObj.getAddress();
```

这段代码的输出是—

```
Haryana
India
123456
```

在执行`getAddress()`方法时，我们观察到它与`addressObj`紧密耦合，后者充当方法中`this`的上下文。

# 调用()

> 如果我们想要得到相同的输出，但是对于一些其他的上下文呢？

然后，要么我们必须创建另一个具有相同键但不同值的对象，以及相同的方法实现，如下所述—

```
let addressObjTwo = {
  state: 'Texas',
  country: 'United Stated of America',
  pincode: '342567', getAddress: function() {
    console.log(`${this.state}\n${this.country}\n${this.pincode}`);
  }
}
```

或者..

我们可以用 **call()** 的方法，用下面的方式——

```
let addressObj = {
  state: 'Haryana',
  country: 'India',
  pincode: '123456',
  getAddress: function() {
    console.log(`${this.state}\n${this.country}\n${this.pincode}`);
  }
}let addressObjTwo = {
  state: 'Texas',
  country: 'United Stated of America',
  pincode: '342567'
}// Strange thing that we are using the property of one object
// and using it to output the parameters of other object
addressObj.getAddress.call(addressObjTwo);
```

这个的输出是—

```
Texas
United Stated of America
342567
```

当然，更好的构建代码的方式是这样的，这样方法就可以重用，而不需要耦合到任何对象。

```
let addressObj = {
  state: 'Haryana',
  country: 'India',
  pincode: '123456',
}let addressObjTwo = {
  state: 'Texas',
  country: 'United Stated of America',
  pincode: '342567'
}function getAddress() {
  console.log(`${this.state}\n${this.country}\n${this.pincode}`)
}getAddress.call(addressObj);
getAddress.call(addressObjTwo)
```

它的输出是这样的—

```
Haryana
India
123456Texas
United Stated of America
342567
```

至于 call()方法的语法，则是

```
[**Function.prototype.call(thisArg[, arg1, arg2, ...argN])**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
```

# 应用()

就功能而言，`call()`和`apply()`是相同的。唯一的区别在于它的语法。`call()`期望函数的参数按序列化顺序排列，但是`apply()`期望这些参数在数组中。

看看下面的代码—

```
let country = {
  country: 'India',
}function getAddress(state, pincode) {
  console.log(`${state}\n${this.country}\n${pincode}`)
}getAddress.call(country, "Haryana", "123456");
getAddress.apply(country, ["Gujarat", "332122"])
```

**apply()** 方法的语法是这样的

```
[**Function.prototype.apply(thisArg [, argsArray])**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
```

# **bind()**

`bind()`方法使用提供的上下文和参数，并返回另一个函数供以后调用。

从下面的例子可以理解——

```
let country = {
  country: 'India',
}function getAddress(state, pincode) {
  console.log(`${state}\n${this.country}\n${pincode}`)
}let newFunc = getAddress.bind(country, "Kerala", "478381");newFunc();
```

这个的输出是—

```
Kerala
India
478381
```

# 结论

我们看到 **call()** 、 **apply()** 和 **bind()** 是 Function.prototype 上的方法，可以用来操纵函数执行的上下文。

# 参考

1.  [https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Global _ Objects/Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function)
2.  [https://www.youtube.com/watch?v=75W8UPQ5l7k](https://www.youtube.com/watch?v=75W8UPQ5l7k)