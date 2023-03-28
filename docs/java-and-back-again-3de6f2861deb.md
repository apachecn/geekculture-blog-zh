# Java 又回来了

> 原文：<https://medium.com/geekculture/java-and-back-again-3de6f2861deb?source=collection_archive---------24----------------------->

## 初学者的故事…第 4 部分

![](img/0542ee7bbf246063f079b8cccff564e6.png)

Photo by [Brooke Lark](https://unsplash.com/@brookelark?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

欢迎来到我的 java 之旅的第四部分。在[第一部分](/geekculture/java-and-back-again-e7663acba4d5)中，我讲了一点历史和一些基本原理。在[的第二部分](/geekculture/java-and-back-again-fc0747a0ad3a)，我进入了状态和实例，花了一些时间解释了一些关于面向对象的编程。在《T4》第三部中，我们谈到了数组和数组列表。在这一部分，我们将讨论封装，以及访问方法的不同方式。

封装的目的是通过对其他类隐藏实现细节来创建小的逻辑束。有几种不同的方法可以做到这一点。我要讲的是公共和私有关键字。现在我们都在以前的帖子中看到过 public 关键字。它允许来自任何类的访问。与此同时，private 将访问权限仅限于做出声明的类。

```
public class dontPanic {

    private String name;
    private int number;}
```

访问这些私有变量的一种方法是使用访问器方法。访问器方法返回私有变量的值。这将允许类访问变量的值，而实际上并不直接访问变量本身。这些方法不带参数，返回类型与它们所访问的变量类型相匹配。你会注意到我使用了*这个*关键字。稍后我会解释。

```
public class dontPanic {

    private String question;
    private int answer; public int getAnswer() {
      return this.answer;
}}
```

如果我们想要改变私有变量的值，我们可以使用一个 mutator 方法。这些允许其他类在没有直接访问变量的情况下修改变量。mutator 方法接受一个类型与它所修改的变量类型相匹配的参数。没有返回任何东西。

```
public class CheckingAccount{
  private int question;

  public void setQuestion(int newQuestion){
    this.question = newQuestion;
  }
}
```

这个关键字用来区分一个实例变量和一个局部变量。变量用*这个*表示实例。您也可以在方法调用中使用 *this* 关键字。

```
public class dontPanic {

    private String question;
    private int answer; public int getAnswer() {
      System.out.print("42");
} public String askQuestion() { this.getAnswer();
       System.out.println("What is the meaning of Life, the  
       Universe, and Everything"); 
  }}
```

我最后想提的是实例变量和局部变量的区别。实例变量可以在类中声明，但在方法之外。而局部变量只能在方法中使用。

## 结论

这个帖子到此为止。我希望您对封装有所了解。下周回来看看，这是本系列的第五部分，也是最后一部分。