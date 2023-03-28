# 停止使用构造函数

> 原文：<https://medium.com/geekculture/stop-using-constructors-5ae7c91e1502?source=collection_archive---------9----------------------->

![](img/c1afbd526f5b6d59fe512680340b81d6.png)

P.C. [https://cutt.ly/80034QC](https://cutt.ly/80034QC)

创建类的对象的传统方法是什么？我们为一个类提供构造方法来创建它的实例。但是软件工程师或开发人员应该遵循另一种技术。那是什么？一个类可以提供一个公共静态工厂方法，该方法可以返回该类的实例。

> 考虑静态工厂方法而不是构造函数

这都是关于这篇文章的。一个类可以为它的客户提供带有或不带有公共构造函数的静态工厂方法。但是提供公共静态工厂方法而不是公共构造函数既有优点也有缺点。

## **使用静态工厂方法的优势**

1.  静态工厂方法有名字，而构造函数没有。

您可以给静态工厂方法起一个自描述性的名称，这样更容易使用，也更容易阅读客户端代码。

```
// TypeScript or JavaScript
Array.from("foo");

// PHP
Closure::fromCallable("strlen");

// Java
Date date = Date.from(instant);
```

这里我们不需要构造函数来创建对象。所有的类都有一个名为`from`的静态工厂方法。我们可以很容易地猜测对象是如何从其他类型创建的；例如，这个`[‘f’, ‘o’, ‘o’]`数组是从字符串`foo`创建的。

2.与构造函数不同，静态工厂方法不需要在每次被调用时创建一个新对象。

通常，当我们创建一个类的一些对象时，每个实例都变得独立于其他实例。如果我们不允许一个类总是创建一个新对象呢？使用构造函数不可能一次又一次地返回同一个对象，只能返回静态工厂方法。

```
// TypeScript/JavaScript
static singleton(): Database {
  if (! Database.instance) {
    return new Database();
  }

  return Database.instance;
}

// PHP
static function singleton(): Database
{
  if (! static::$instance) {
    return new Database();
  }

  return static::$instance;
}

// Java
static Database singleton() {
  if (null == instance) {
    return new Database();
  }

  return instance;
}
```

现在我们可以保证在一个脚本的生命周期中不存在两个实例，并且这个类将是一个完全不可变的类。但是使用构造函数你不能做到这一点。否则，如果创建对象的成本很高，那么通过重复调用返回同一个实例可以提高性能。

3.与构造函数不同，静态工厂方法可以返回其返回类型的任何子类型的对象。

这意味着如果一个方法的返回类型是一个基类/父类或接口，那么该方法可以使它返回该父类或接口的子类的对象。这是不可能的。

```
// TypeScript/JavaScript
static newInstance(target: String): Parent {
  if ('specified type' === target)
    return new Child();
  else
    return new OtherChild();
}

// PHP
static function newInstance(string $target): Parent
{
  if ('specified type' === $target)
    return new Child();
  else
    return new OtherChild();
}

// Java
static Parent newInstance(String target) {
  if ('specified type' == target)
    return new Child();
  else
    return new OtherChild();
}
```

注意方法的返回类型。该方法返回子对象，而不是父对象。因为父类可能是抽象类或接口。所以使用静态工厂来选择返回对象的类是非常灵活的。

4.**作为输入参数的函数，返回对象的类可以随调用而变化。**

通过查看这个`EnumSet.of()`方法的 Java OpenJDK 实现可以清楚地理解这一点，它使用这个`EnumSet.noneOf()`方法来使其工作。请注意，我已经省略了这两个方法的部分签名，以使它们尽可能简单。

```
static EnumSet of(E e1, E e2, E e3) {

  EnumSet result = noneOf(...); // see below

  result.add(e1);
  result.add(e2);
  result.add(e3);

  return result;
}
```

```
static EnumSet noneOf(...) {
  ...

  if (universe.length <= 64)
      return new RegularEnumSet(...); // note this line and 
  else
      return new JumboEnumSet(...); // this line
}
```

将返回`EnumSet`的哪个实现取决于`EnumSet.of()`方法参数的长度。如果长度小于或等于 64 `RegularEnumSet`则返回对象，否则返回`JumboEnumSet`。

5.**编写包含方法的类时，返回对象的类不需要存在。**

这个优势是关于服务提供商的服务访问 API。这个访问 API 可以通过静态工厂方法来实现。这种优势的目的是在缺少搜索服务的标准的情况下获得服务的默认实现。

```
// Expect a specific implementation
Service.get('ge me this service', 'with this implementation');

// Without specifying a particular implemenation
Service.get('ge me this service');
```

请注意第二个示例代码。这里缺少标准。因此，在没有特定标准的情况下，它应该返回预期服务的默认实现的对象。

## 使用静态工厂方法的缺点

1.  **没有公共或受保护构造函数的类不能被子类化。**

如果您提供静态工厂方法，通常您会将该类的构造函数设为私有。这意味着该类没有公共的或受保护的构造函数。因此，您不能从类继承。对于一些编程语言来说确实如此。

但这可能不是问题。因为有一个原则——**重合成轻继承**。

2.静态工厂方法的第二个限制是程序员很难找到它们。

如果你给一个类提供静态工厂方法，那么这个类的用户可能不知道名字是什么。所以他们需要这门课的文档。但是现在这不再是一个问题了，因为 ide 帮助很大。

我强烈建议您遵循社区标准和最佳实践。跟随他们，你首先帮助自己，然后是别人。

现在，我们可以通过遵循如下常见命名约定来解决第二个限制:

1.  **from** —一种类型转换方法，采用单个参数并返回该类型的相应实例。以字符串返回数组为例。

```
Array.from("foo");
```

2. **of** —一种聚合方法，采用多个参数并返回一个包含这些参数的此类型的实例，例如。

```
Array.of(1, 2, 3); // [1, 2, 3]

Set<Rank> faceCards = EnumSet.of (JACK, QUEEN, KING);
```

3.**value of**——一种替代`from`和`of`的更详细的方法。

```
BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE);
```

4.**实例**或 **getInstance** —返回由其参数(如果有的话)描述但不能说具有相同值的实例。

```
Registry::getInstance(); // Returns an registry object

StackWalker luke = StackWalker.getInstance(options);
```

5.**创建**或**new instance**——类似于**实例**或 **getInstance** ，除了该方法保证每个调用返回一个新实例。

```
ReflectionClass::newInstance($param1, $param2, ...);

Object newArray = Array.newInstance(classObject, arrayLen);
```

6. **getType** —类似于 **getInstance** ，但是在工厂方法在不同的类中时使用。**类型**是工厂方法返回的对象类型。

```
// Here FileStore is the target type
FileStore fs = Files.getFileStore(path);
```

7. **newType** —类似于 **newInstance** ，但是如果工厂方法在不同的类中使用。**类型**是工厂方法返回的对象类型。

```
// Here BufferedReader is the target type
BufferedReader br = Files.newBufferedReader(path);
```

8.**类型**——getType 和 **newType** 的简洁替代。

```
// Here List is the target type
List<Complaint> litany = Collections.list(legacyLitany);
```

## 决定

现在的问题是:我应该总是使用静态工厂方法而不是构造函数吗？看情况。静态工厂方法和构造函数都有自己的用途。但是在使用构造函数之前，您应该首先考虑静态工厂方法。

**注意**本出版物是来自 [Effective Java](https://www.amazon.com/Effective-Java-Joshua-Bloch/dp/0134685997) 一书的“项目 1:考虑静态工厂方法而不是构造函数”的简短版本，作者是 [Joshua Bloch](https://en.wikipedia.org/wiki/Joshua_Bloch) 。我强烈推荐这本书。然后你看“第一项”。

在 Medium、 [Github、](https://github.com/unclexo) [Twitter、](https://twitter.com/unclexo)或 [LinkedIn](https://www.linkedin.com/in/unclexo) 上关注我。