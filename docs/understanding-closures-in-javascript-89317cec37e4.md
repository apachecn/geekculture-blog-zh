# 理解 JavaScript 中的闭包

> 原文：<https://medium.com/geekculture/understanding-closures-in-javascript-89317cec37e4?source=collection_archive---------12----------------------->

![](img/a149b42e3b2de16001a4cb6dc499be10.png)

Photo credits : Code Geek

闭包的概念是 JavaScript 中可能很难理解的主题之一。然而，一旦你掌握了它们，它们会非常有用！它们可以帮助您向任何 web 应用程序添加许多可重用的功能。这有助于我们在应用程序编译和运行时节省时间。你问什么是结束，我会先把它分解成小部分来解释，然后一步一步去理解它到底是什么。

# 什么是终结？

MDN Web Docs 的解释:

> 闭包是一个函数的组合，该函数被捆绑在一起(被封闭),并引用其周围的状态(词法环境)。换句话说，闭包允许您从内部函数访问外部函数的范围。在 JavaScript 中，闭包是在每次创建函数时创建的。”

# 让我们来谈谈示波器

范围决定了变量的可访问性(或可见性)。有两种类型:

*   本地范围
*   全局范围

# 局部范围

JavaScript 函数中声明的变量属于它的局部范围。它们只能在该函数中访问。局部变量在函数启动时创建，在函数完成时删除。

```
function myFunction() { var count = 0; console.log(count);};myFunction(); => 0 console.log(count); => ReferenceError: count is not defined
                       count is declared within myFunction's 
                       scope, therefore it is not eligible 
                       outside of that function.
```

您甚至可以在不同的作用域中重用公共变量名称(计数、索引、当前、值等)而不会导致错误:

```
function myFunction() {

    // 'myFunction" function scope
    // This count variable belongs here, and it will log 0

    let count = 0;

    console.log(count);
};

function someFunction(){

    // 'someFunction" function scope
    // This count variable belongs here, and it will log 1

    let count = 1;

    console.log(count);
};

myFunction(); => 0
someFunction(); => 1
```

# 全球范围

在函数外部声明的变量属于全局范围。网页上的所有脚本和函数都可以访问它。

```
let carName = "Volvo";

console.log(carName); => "Volvo"

function myFunction() {

  // Code written inside this function can also use carName, 
     because carName belongs to Global Scope

  console.log(carName);

};

myFunction(); => "Volvo"
```

这很好，但是我们可以用望远镜做更多的事情！

# 嵌套范围

让我们稍微玩一下，把一个范围放在另一个范围内！这称为嵌套:

```
function outerFunction(){

    // The outer scope

    let outerVar = "Outer variable";

   function innerFunction(){

       // The inner scope
       // The variables of the outer scope are accessible inside the inner scope 

       console.log(outerVar);

   }

   innerFunction();

}

outerFunction(); => "Outer variable"
```

让我们看一下上面的代码，一步一步地理解代码是如何运行的:

*   一个名为`outerFunction()`的函数是用变量`outerVar`和函数`innerFunction()`定义的。
*   `innerFunction()`运行我们`outerVar`的控制台日志。
*   由于`innerFuction()`中没有`outerVar`的声明，JavaScript 会认为`outerVar`与`outerFunction()`中声明的变量相同。然而，如果我们要在我们的`innerFunction()`中创建另一个`outerVar`，我们的`innerFunction()`将首先遵从那个变量。

这是另一个例子:

```
let myGlobal = 0;

function myFunction() {

  let myVar = 1;

  console.log(myGlobal); // => It will log 0

  function innerOfFunction() {

    let myInnerVar = 2;

    console.log(myVar, myGlobal); // => I will log 1 0

    function innerOfInnerOfFunction() {

      console.log(myInnerVar, myVar, myGlobal); // => It will log 
                                                      2 1 0 

    }

    innerOfInnerOfFunction();

  }

  innerOfFunction();

}

myFunction(); => 0 
                 1 0
                 2 1 0
```

让我们一行一行地看看上面的代码:

1.  变量`myGlobal`在全局范围内声明，其值为 0。
2.  声明下一个`myFunction()`。
3.  在`myFunction()`的范围内，变量`myVar`被声明为值 1。
4.  在`myFunction()`内部，另一个函数`innerOfFunction()`是用一个变量`myInnerVar`声明的，其作用域的值为 2。
5.  然后我们在`innerofFunction()`中声明另一个名为`innerOfinnerOfFunctoion()`的函数(例子名称很复杂！)哪个控制台记录变量:`myGlobal`、`myVar`和`myInnerVar`。因为它嵌套在`myFunction()`和`innerOfFunction(),`下面，所以它可以访问它上面的所有变量。
6.  `innerOfFunction()`然后通过调用`innerOfInnerOfFunction()`结束。
7.  最后`myFunction()`通过调用`innerOfFunction()`关闭，这将运行嵌套在其中的所有代码。

*外部作用域不知道内部作用域中的变量，而内部作用域(如果嵌套的话)知道其外部作用域中的变量。*

# 那么什么是真正的终结呢？

词法环境允许我们静态地访问外部作用域的变量。我们可以继续嵌套变量和函数，但是我们还没有创建闭包。离关闭只有一步之遥了！

看一下这段代码:

```
function outerFunction(){

  let counter = 0;

  function innerFunction(){

    console.log(counter+=1); 

   }

  innerFunction(); 
}outerFunction(); // => 1 
outerFunction(); // => 1 counter didn't change because
                       everytime. outerFunction() is called,
                       it sets the counter to 0, and adds 1 to it.
```

目前，`innerFunction()`正在`outerFunction()`内被调用。让我们改变这一点，不要调用函数，而是简单地返回函数。

让我们添加更改:

```
function outerFunction(){

    let counter = 0;

    function innerFunction(){

      console.log(counter+=1); // Let's increment the counter

    }

    return innerFunction; // Note that we are not invoking the
                             function, but just returning it. 

}
```

通过返回`innerFunction()`，我们现在可以将外部函数保存到不同的变量中，这些变量将是该函数的实例:

```
let invokeFirst = outerFunction() // => ƒ innerFunction(){

      console.log(counter+=1); // Le…

// outerFunction() invoked the first time

let invokeSecond = outerFunction() // => ƒ innerFunction(){

      console.log(counter+=1); // Le…

// outerFunction() invoked the first time

// By setting the invokeFirst and invokeSecond variables to
   outerFunction, its value becomes result of innerFunction.

invokeFirst() // => 1 invokeFirst() invoked the first time
invokeFirst() // => 2 invokeFirst() invoked the second time
invokeFirst() // => 3 invokeFirst() invoked the third time

invokeSecond() // => 1 invokeSecond() invoked the first time
```

让我们检查一下第一次执行`invokeFirst()`时会发生什么:

1.  创建变量计数器，其值在`outerFunction()`内设置为 0。
2.  `outerFunction()`保存到变量`invokeFirst()`和`invokeSecond()`时，仅调用和运行一次。
3.  JavaScript 知道变量 counter 不再存在。由于`counter`是`outerFunction()`的一部分，`counter`只会在`outerFunction()`执行时存在。由于`outerFunction()`在我们调用`invokeFirst()`之前很久就完成了执行，所以外部函数范围内的任何变量都不复存在，因此变量`counter`也不复存在。

# 但是，它实际上是如何工作的呢？

由于 JavaScript 中的闭包，内部函数可以访问封闭函数的变量。换句话说，内部函数在封闭函数执行时保留了封闭函数的作用域链，因此可以访问封闭函数的变量。

还在迷茫？更简单的说法是，当我们将我们的`outerFunction()`保存到一个新变量中时，它运行一次，设置它的变量并执行变量中的内容，然后返回`innerFunction()`。现在，当我们调用新变量作为函数时，它只运行内部函数。`innerFunction()`记住了`outerFunction()`传递给它的所有东西，然后执行自己的代码。没有传递给`innerFunction()`的任何东西现在都不在它的范围内。让我们来看看这意味着什么:

```
function outerFunction(){

    let counter = 0;

    let anotherVar = "something" // This won't be remembered by
                                    innerFunction(), because 
                                    outerFunction() returns the 
                                    function of innerFunction() 
                                    and innerFunction() doesn't 
                                    use anotherVar. Therefore, 
                                    it will be forgotten.

    function innerFunction(){

      console.log(counter+=1); // Let's increment the counter

   }

    return innerFunction;

}
```

# 闭包的流行用例

*   处理事件侦听器
*   私有方法
*   函数式编程

# 处理事件侦听器

```
const myButton = document.getElementById("myButton");
const button = document.getElementById("button");

const myfunc = () => {

  let clicked = 0;

  const inner = () => {

    return clicked +=1;

  };

  return inner;

};

let callback = myfunc();

myButton.addEventListener("click", function handleClick() {

  myText.innerText = `You clicked ${callback()} times`;

});
```

**为什么在这里有用？**

在这里，每次我们将`myFunc()`设置为一个新变量时，它都会为我们提供一个从 0 开始的新的`clicked`值。这使得它可以被各种不同的事件侦听器重用。想象一下在社交媒体页面上添加赞，或者一个鼓掌功能。

# 私有方法

我们可以使用闭包来模拟私有方法。私有方法为我们提供了一种管理全局名称空间和限制代码访问的好方法。

```
let counter = (function() {

  let privateCounter = 0;

  function changeBy(val) {

    privateCounter += val;

  }

  return {

    increment: function() {

      changeBy(1);

    },

    decrement: function() {

      changeBy(-1);

    },

    value: function() {

      return privateCounter;

    }

  };

})();

console.log(counter.value());  // 0.

counter.increment();
counter.increment();
console.log(counter.value());  // 2.

counter.decrement();
console.log(counter.value());  // 1.
```

在上面的代码中，我们做了一些事情。首先，我们创建一个私有变量(`privateCounter`的值为 0)和一个私有函数(`changeBy()`向`privateCounter`变量添加一个值)。这两个都是私有的，因为我们没有将它们传递给我们的语句。第二，我们的返回语句我们正在创建一个匿名包装器，它包含三个公共对象:`increment`、`decrement`和`value`。增量和减量调用`changeBy()`函数将`privateCounter`的值改变 1 或-1。值只是返回我们的`privateCounter`的值。这里返回的对象可以通过调用我们的计数器并调用我们希望运行的函数的一个键来访问(例如:`counter.value()`)

# 函数式编程

这是一个非常有用的概念，我们可以用它来帮助我们避免重复编写相同的代码。我们可以创建函数式编程，反过来，我们可以在任何时候发现自己重复编写类似的代码时使用函数。

```
function division(a) {

  return function runDivision(b) {

    return b / a;

  }

}

const divideByTwo= division(2);
divideByTwo(4); // => 2
divideByTwo(6); // => 3

const divideByThree = division(3);
tridivideByThree(3); // => 1
tridivideByThree(9); // => 3
```

# 结论

如果您发现自己一次又一次地重用相同的代码块，那么闭包是创建可重用函数的好方法，这些函数可以放在您需要的地方！

# 资源

*   [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
*   [W3Schools](https://www.w3schools.com/js/js_function_closures.asp)