# Java 又回来了

> 原文：<https://medium.com/geekculture/java-and-back-again-fc0747a0ad3a?source=collection_archive---------14----------------------->

## 初学者的故事…第二部分

![](img/71fe53b67fd685ff35882cd5fe4e2a57.png)

Get it, it’s two cups because it’s part two.

所以这是我学习 Java 跋涉的继续。在第一部分，我谈了一点 Java 的历史，谁创造了它，以及它目前的所有权。我还谈了一点语法，并举例说明了布局。我还举例说明了原始变量以及一个有用的对象。你可以在这里阅读这篇文章。

在这篇文章中，我们将进入状态和实例齐头并进。我将描述对象是如何被创建的，以及一旦它们被创建，它们可以访问什么。我将讨论构造函数，它们的签名是什么，以及它的作用。所以事不宜迟，我们开始吧。

所以，我们知道 Java 中有类，就像其他面向对象的语言一样，我们可以创建该类的实例。一个类的每个实例都是一个对象。每个对象都以实例字段和方法的形式继承状态和行为。

在下面的代码示例中已经创建了一些变量。在它下面是一个构造函数，我们用它来创建我们的对象。然后是 main 方法，这是我们在上一篇文章中学到的 java 程序必备的方法。我将分解每个部分如何创建一个对象。

```
CreateJedi.javapublic class CreateJedi { int age;
    int forceSensitivity;
    String name; public CreateJedi(int age, int forceSensitivity, String name){
       this.age = age;
       this.forceSensitivity = forceSensitivity;
       this.name = name;
    } public static void main(String [] args){

       CreateJedi luke = new CreateJedi(18, 9, "Luke");

       System.out.println("My name is " + luke.name + " I'm " + 
       luke.age + " my force sensitivty is " + 
       luke.forceSensitivity); }
}
```

在代码示例的第一部分，您将看到我声明了几个变量。我们将用这些变量来创造我们的绝地目标。每个绝地都会有一个年龄，对原力的敏感程度，以及一个名字。

```
CreateJedi.javapublic class CreateJedi { int age;
    int forceSensitivity;
    String name;
```

代码的下一部分是构造函数。这是用于创建我们的实例的方法。构造函数总是以类命名。如果没有定义构造函数，将使用默认的空构造函数。您会注意到“this”关键字的使用。我们使用“this”来帮助消除类属性和同名参数之间的混淆。在构造函数方法中尤其如此，其中参数通常隐藏类属性，但这只是惯例，您可以使用不同的方法。

```
CreateJedi.javapublic class CreateJedi { int age;
    int forceSensitivity;
    String name;public CreateJedi(int age, int forceSensitivity, String name){
       this.age = age;
       this.forceSensitivity = forceSensitivity;
       this.name = name;
    }
```

现在我们有了一些属性，并把它们分配给我们的实例。让我们看看如何实际建立一个绝地。我们将使用“new”关键字，后跟类构造函数。然后，您可以使用构造函数添加初始值。

```
CreateJedi.javapublic class CreateJedi { int age;
    int forceSensitivity;
    String name; public CreateJedi(int age, int forceSensitivity, String name){
       this.age = age;
       this.forceSensitivity = forceSensitivity;
       this.name = name;
    } public static void main(String [] args){

       CreateJedi luke = new CreateJedi(18, 9, "Luke");
```

然后我们可以使用点符号来调用属性。

```
CreateJedi.javapublic class CreateJedi { int age;
    int forceSensitivity;
    String name; public CreateJedi(int age, int forceSensitivity, String name){
       this.age = age;
       this.forceSensitivity = forceSensitivity;
       this.name = name;
    } public static void main(String [] args){

       CreateJedi luke = new CreateJedi(18, 9, "Luke");

       System.out.println("My name is " + luke.name + " I'm " + 
       luke.age + " ,my force sensitivty is " + 
       luke.forceSensitivity); }
}
```

这就是在 java 程序中创建实例的方式。我想指出的是，一个程序有多个构造函数并不罕见。现在，这是可能的，只要它们都有不同的参数。编译器使用一种叫做构造函数签名的东西，它由构造函数的名字和它的参数组成。在这种情况下，签名应该是“CreateJedi(int age，int forceSensitivity，String name)”。

```
public CreateJedi(int age, int forceSensitivity, String name){
       this.age = age;
       this.forceSensitivity = forceSensitivity;
       this.name = name;
    }
```

# 结论

这个帖子到此为止。在这篇文章中，我们讨论了如何创建一个类的实例，以及这些实例是如何成为对象的。我们讨论了构造函数方法和构造函数方法签名。我们看了如何使用点符号来访问属性。在下一部分中，我们将会看到数组和数组列表，以及为什么它们和其他语言有一点不同。