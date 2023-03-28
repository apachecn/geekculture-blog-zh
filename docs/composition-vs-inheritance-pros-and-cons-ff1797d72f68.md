# 合成与继承:利弊

> 原文：<https://medium.com/geekculture/composition-vs-inheritance-pros-and-cons-ff1797d72f68?source=collection_archive---------7----------------------->

## 组合和继承在面向对象编程中的位置？

![](img/ffde06ddba5940625a61633a56a1ed1a.png)

Photo by [GR Stocks](https://unsplash.com/@grstocks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/battle?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

本教程将查看`inheritance` 和`composition`，这是正确的用例。示例代码是用 Java 编写的，但是基本原理对于所有支持面向对象编程(OOP)的编程语言都是一样的。在开始之前，我们首先要理解这些术语。先说继承。

## 定义

> 在面向对象编程中，**继承**是将一个对象或类基于另一个对象(基于原型的继承)或类(基于类的继承)的机制，保留了类似的实现。— [维基百科](https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming))

有大量的英语在进行，但就其简单的意义而言，继承使得一个类将其功能扩展到其他类成为可能。它模仿了我们在现实世界中所称的遗传，即孩子从父母那里获得特征。

不出所料，扩展另一个类的类被称为*子类*或*子类*，被扩展的类是*父类*或*超类*(都是双关语)。继承有一个*“is-a”*关系。

例如，狮子来自猫科动物，所以狮子是一只猫。鬣狗也一样。它们都有爪子，并继承了捕食的本能。然而，每一种都有区别于其他产品的独特特征。它是 OOP 中的典型子类，有不同的方法专门针对它们。

构图包括组成整体的部分。没有方向盘、引擎或座椅的汽车是不完整的。为此，我们说一辆汽车*“有——一个”*方向盘。继承和组合之间第一个值得注意的区别是“is-a”和“has-a”的关系。

让我们创建三个类，看看它们是如何通过继承相互关联的。

## 诉讼继承

```
public abstract class Employee {
    private String firstName;
    private String lastName;
    private String accountNumber;
    private Double salary;
    private Date hireDate; // constructor
    public Employee (
        String firstName,
        String lastName, 
        String accountNumber,
        Double salary, 
        Date birthDate,
        Date hireDate
    ) {
      this.firstName = firstName;
      this.lastName = lastName;
      this.accountNumber = accountNumber;
      this.salary = salary;
      this.hireDate = hireDate;
  }
  // setters and getter are omitted for brevity
   abstract Double earnings();
}
```

上面的代码是一个简单的抽象 Plain Old Java Object (POJO ),有五个实例变量、一个初始化所有变量的构造函数和一个抽象的收益方法。Java 要求子类有一个构造函数，以便在对象实例化期间调用它。我们显式地创建了构造函数来传递所有的实例变量作为参数。

请密切注意我们在这里使用的私有访问修饰符以及它们对应的 setters 和 getters。我们选择 private 作为 OOP 最佳实践的一部分，以避免破坏封装。

**注意**:这不是关于 OOP 的教程，所以我将把详细的解释推迟到后面的博客。

现在让我们创建两个子类，FullTimeEmployee 和 ContractEmployee。

## 全职员工类别

```
class FullTimeEmployee extends Employee {
    private Double overtimeAllowance; public FullTimeEmployee(
        String firstName,
        String lastName,
        String accountNumber,
        Double salary,
        Date hireDate
    ) {
        super(
            firstName, 
            lastName,
            accountNumber,
            salary, 
            birthDate, 
            hireDate
        );
        this.overtimeAllowance = overtimeAllowance;
  } @Override
    public Double earnings() {
        return getSalary() + getOvertimeAllowance();
   }
}
```

上面的代码片段是一个子类，它扩展了 Employee 类并覆盖了 code 方法，这里没什么特别的。它还有一个额外的实例变量，*“overtime allow ance”，*，它使用这个变量的 getter 来计算收益。

现在，让我们看看另一个子类。

## 合同雇员类

```
class ContractEmployee extends Employee {
    public ContractEmployee(
        String firstName,
        String lastName,
        String accountNumber,
        Double salary) {
        super(
            firstName,
            lastName,
            accountNumber,
            salary
        );
  } @Override
    public Double earnings() {
        return getSalary();
    }
}
```

这段代码看起来与*全职雇员*类几乎相同。唯一的区别是，它没有额外的领域和计算收入的方法不同。

## 抽象类中一个简单变化的连锁反应

让我们假设在一次例行的财务审计后，审计人员发现公司因为虚名而亏损，所以他们决定补救这种情况。软件架构师想出了一个绝妙的策略，通过引入另一个实例变量*“is account expired”*来防止向已经离开公司的员工持续支付工资，从而帮助解决这个问题。

这个单一的变化需要修改超类及其所有伴随的子类。让我们根据新要求进行如下更改:

**注意**:这段代码中只有类定义、方法签名和新的变化。各处的省略号*“…”*表示代码延续。

新*员工*班

```
// superclass
public class Employee {
    ...
    private Boolean isAccountExpired; // new instance variable here // constructor
    public Employee (
        ... // ellipse denote continuation
        Boolean isAccountExpired // change here
    ) {
        ... 
        this.isAccountExpired = isAccountExpired; // change here
}
```

新的*全职员工类别*

```
// subclass 1
public class FullTimeEmployee extends Employee {
      // constructor
    public FullTimeEmployee (
        ...
        Boolean isAccountExpired // change here super(
            firstName,
            lastName,
            accountNumber,
            salary,
            birthDate,
            hireDate,
           isAccountExpired
        );
}
```

新的 *ContractEmployee* 子类

```
// subclass 2
public class ContractEmployee extends Employee {
      // constructor
    public FullTimeEmployee (
        ...
        Boolean isAccountExpired // change here super(
             firstName,
             lastName,
             accountNumber,
             salary,
             birthDate,
             hireDate,
             isAccountExpired
           ); // change here
}
```

你明白意思了吗？超类中的一个简单变化会影响层次链中的所有子类。我们的用例只有两个子类，所以没什么大不了的。想象一下，从 Employee 类继承的大型代码库中有数百个子类。你的猜测和我的一样好。

让我们以不同的方式做事。我们将 accountNumber 提取到它的类中，并将其传递给 Employee 类。有了这个变化，我们就可以深入到*“has-a”*关系的领域了。

## 动作合成

```
class Account {
    private final String id;
    private Double balance;
    private Boolean isExpired; public Account(
        String id,
        Double balance,
        Boolean isExpired
    ) {
       this.id = id;
       this.balance = balance;
       this.isExpired = isExpired;
    }
    // setters and getters left for brevity
}
```

上面的代码是一个简单的 account 类，有三个实例变量和一个构造函数。

现在，我们相应地修改雇员类。在下面查找具有最新更改的源代码:

```
public abstract class Employee {
    private String firstName;
    private String lastName;
    private Double salary;
    private Date hireDate;
    private Account account; public Employee (
        String firstName,
        String lastName,
        Double salary,
        Date hireDate,
        Account account
    ) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.salary = salary;
        this.hireDate = hireDate;
        this.account = account;
    }
  // setters and getter are omitted for brevity
  abstract Double earnings();
}
```

事后看来，我们写的代码比我们已经有的要多。那么有什么问题呢？为什么我们试图通过引入整个 *Account* 类来解决继承问题？

如果你沿着这些思路思考，你的担心是有道理的。您最初编写了相当多的代码，但是随着需求的不断变化，这就是组合的闪光点。

在我们上次的 Sprint 规划中，产品负责人告诉我们，用户希望在他们的 *Account* 类中有一个 *name* 字段。我们只修改 account 类，其他类保持不变。

## 继承的优势

假设你能保证你设置数据的唯一地方是从一个对应的 *set* 方法和 get 方法获取数据。在这种情况下，您可以减少调试代码所花费的时间，因为您知道只有那些方法负责操作这样的数据。

当您决定修改方法的实现细节时，这也很方便。你只需要改变方法，而不是对代码库的超类和子类中的许多不同地方进行修改。

最后但同样重要的是，如果你确定你永远不会修改你的超类，继承可能是正确的选择。它减少了写作所需的额外课程的数量。

## 继承的缺点

> “继承是实现代码重用的一种强有力的方法，但它并不总是这项工作的最佳工具。使用不当，会导致软件脆弱。”——《有效的 Java 》,作者约书亚·布洛克。

从上面的例子中，很明显子类与超类紧密耦合，这违反了*中的 *O* 固体*原则。超类中的更改会中断更改，直到它的所有后代都被相应地修改。避免不必要的紧耦合的最好方法是尽可能引入组合。

同样，组合极大地增强了代码重用。任何想要使用 Account 类的类都可以自由地这样做，因为它具有松耦合的特性。将它作为引用传递，我们可以调用 account 的所有公共方法，这些方法以前是不存在的。

另一个支持组合胜过继承的好理由是单元测试。如果一个 *Account 类的实例是未知的，我们可以用一些假数据模拟它，然后继续我们的测试。这种方法比继承方法更容易。*

## 结论

这个教程告诉我们什么是继承和合成，以及它们的区别。总之，尽可能使用复合，只保守地使用继承。合成的主要缺点是你要写的代码行数。然而，从长远来看，代码的松散耦合和灵活性弥补了这一点。

如果你喜欢这篇文章，请关注我的 handle [Bright](/@jessebrite) 了解更多关于软件开发和一般技术的故事。