# 举例说明协变和逆变

> 原文：<https://medium.com/geekculture/covariance-and-contravariance-by-example-99c2bc29fd40?source=collection_archive---------1----------------------->

![](img/0799f8b4cbe8ded1a9a52843b664c859.png)

协变和逆变是非常怪异的词。

如果我想让自己听起来很聪明，我会用这些词——这通常很有效。没有人真正知道这到底是什么。

因为我想尽可能简单地解释这个话题(但不是更简单)，我会尽量避免这些听起来很聪明的词。

我将使用泛型实现一个存储库模式来解释这个主题。

我将使用 C#作为我的例子，它与 Java 或 Kotlin 等语言非常相似。

# 你已经知道一半了！

让我们定义几个简单的类。

```
record Person(string Name);
record Employee(string Name) : Person(Name);
record RemoteEmployee(string Name, string location) : Employee(Name);
```

创建几个雇员，并将他们放入一个集合中。

```
IEnumerable<Employee> people = new List<Employee>
{
    new Employee("Andrew"),
    new RemoteEmployee("Karen","USA")
};
```

因为一个`Employee`也是一个`Person`我应该可以这样做。

```
IEnumerable<Person> people = new List<Employee> // It's not <Employee> anymore.
{
    new Employee("Andrew"),
    new RemoteEmployee("Karen","USA")
};
```

所以如果一个`Employee`也是一个`Person`，我可以在赋值的左边使用一个更通用的类型。又名协方差！

简单吧？

![](img/e79c84c6585f4d6f8c417b2a1da1d57b.png)

既然我提到了一个*泛型类型*让我们做一些可以利用泛型的东西。让我们把数据保存在数据库里。

有趣的事实:泛型是在 2005 年与 Visual Studio 200 一起发布的 C# 2.0 中引入的。

应该用 MySQL，Excel，SQL Server，SQLite 还是 Postgres？答案是——我们不必在意。我想做的只是插入一些数据，稍后再取回。这里，存储库模式来拯救我们了！

```
interface IRepository<T>     // "T" stands for "data of some type"
{
    void Insert(T item);     // I want to insert some data.
    T Get(string id);        // and get it 
    IEnumerable<T> GetAll(); //            back later
}
```

我实现了一个非常简单的存储库，它将对象存储在磁盘上的 Json 文件中。这里可以看到[。](https://filerepository.cs/)

当你实现这样一个库时，你会在实现`Insert`方法时遇到一个问题。
如何插入数据，以便日后检索？
在`Get`方法中有一个`id`，但是你会在哪个`id`下插入数据呢？

这里你有多种选择。让数据库来处理它——数据库可以生成一些 Guid(比如 Mongo ),或者关系数据库可以生成一个主键。
2。使用对象的哈希代码或 ToString 但是您可能永远也不会得到对象，所以千万不要这样做。
3。在将 Id 添加到数据库之前对其进行强制。
4。一些其他的想法。

你是负责人，而不是数据库或哈希函数，所以我选择第三个选项。

![](img/3c3eff75ce3e1e9128d4874b464bc13f.png)

我希望我所有的持久对象都实现一个`Entity`接口。

```
interface Entity { string Id { get; } }record Person(string Name) : Entity { public string Id => Name; } // Name is not the best Id of course.
```

我还必须在接口本身上实施规则。

```
interface IRepository<T> where T : Entity  // "T" stands for "data of some type" where the data also has an Id.
```

现在我可以在数据库中存储我的员工名单了！

```
static void Main(string[] args)
{
    var employeesRepository = new FileRepository<Employee>();
    AddEmployees(employeesRepository);  // Add a list of employees to a repository
    ReadAndPrintRepository(employeesRepository);     // Read the repository and print users.
}static void AddEmployees(IRepository<Employee> repository) 
    => new List<Employee>
        {
            new RemoteEmployee("Karen","Usa"),
            new Employee("Karen")
        }.ForEach(repository.Insert);static void ReadAndPrintRepository(IRepository<Employee> repository) 
    => repository
        .GetAll()
        .ToList()
        .ForEach(Console.WriteLine);
```

这一切都工作，这是伟大的。但是员工也是人，所以我想做些小小的改变。

```
static void ReadAndPrintRepository(IRepository<Person> repository)
```

突然你得到一个错误

```
cannot convert from 'FileRepository<Employee>' to 'IRepository<Person>'csharp(CS1503)
```

![](img/a62db655518d42d6548c15efb12d7069.png)

但是为什么呢？它对上面的例子起作用！

引用我的话“所以如果一个`Employee`也是一个`Person`，我可以在赋值的左边使用一个更抽象的类型。又名协方差！

有什么区别？如果你看看 IEnumerable(或者 Java 中的集合)接口，它是这样定义的。

```
public interface IEnumerable<out T> : System.Collections.IEnumerable
```

`out`这个关键词在这里至关重要。它对编译器说——使用类型`T`或更抽象/派生的类型。类型`T`是一个*协变*参数。它允许一些变化，差异。

但是为什么要用`out`这个关键词呢？为什么不是`in`或者`covariant`或者`leave me alone and work plz`？

让我们在我们的界面上添加一个`out`关键字，让我们看看。

```
Invalid variance: The type parameter 'T' must be contravariantly valid on 'IRepository<T>.Insert(T)'. 'T' is covariant. csharp(CS1961)
```

(╯°□°）╯︵ ┻━┻

怎么了？

好吧。当你指定`out`关键字时，意味着接口只能**输出**类型`T`。它不能像在 Insert 方法中那样接受类型为`T`的输入。

我将从`IRepository`接口中移除`Insert`方法，并将其移动到`IWriteOnlyRepository`。为了在代码库中保持一些对称性，我将引入一个`IReadOnlyRepository`并将它们合并到`IRepository`中。

```
interface IWriteOnlyRepository<T>
{
    void Insert(T item);
}interface IReadOnlyRepository<out T>
{
    T Get(string id);
    IEnumerable<T> GetAll();
}interface IRepository<T> : IWriteOnlyRepository<T>, IReadOnlyRepository<T> where T : Entity
{
}
```

现在，当您使用`IReadOnlyRepository`接口作为`ReadAndPrintRepository`方法中的参数时，您将不会再收到错误！

┬─┬ノ( º _ ºノ)

```
static void Main(string[] args)
{
    var employeesRepository = new FileRepository<Employee>();
    AddEmployees(employeesRepository);
    ReadAndPrintRepository(employeesRepository);
}static void ReadAndPrintRepository(IReadOnlyRepository<Person> repository) => repository.GetAll().ToList().ForEach(Console.WriteLine);static void AddEmployees(IRepository<Employee> repository) => new List<Employee>
    {
        new Employee("Bjork"),
        new RemoteEmployee("Karen","Usa")
    }.ForEach(repository.Insert);
```

一切按预期运行。生活是美好的。但是如果我添加一个只处理远程雇员的特殊方法呢？

```
static void AddRemoteEmployees(IRepository<RemoteEmployee> repository)
    {
        //some operations only valid for RemoteEmployee
        //OnlyRemoteEmployeeMethod(remoteEmployee)
        repository.Insert(new RemoteEmployee("Andrew","Canada"));
        repository.Insert(new RemoteEmployee("Carol","UK"));
    }
```

不幸的是，你会得到这个错误

```
cannot convert from 'FileRepository<Employee>' to IRepository<RemoteEmployee>'
```

`FileRepository`实现了接口`IRepository<Employee>`，所以只有一个雇员可以被添加到存储库中。你可以说那是非常刻板的。它不允许任何变化，因此它是**不变的**。

`IWriteOnlyRepository`目前存在与`IRepository`相同的问题，它不允许任何差异。既然我们在`IReadOnlyRepository`上有一个`out`修改器，难道我们没有一个`in`修改器可以使用吗？

是的，我们有！当您指定`in`关键字时，这意味着接口只能接受类型为`T`的**输入**。因为我们只使用类型`T`作为输入，并且我们从不返回，所以我们可以使用关键字`in`将输入类型标记为逆变。

```
interface IWriteOnlyRepository<in T>
{
    void Insert(T item);
}
```

最后要做的事情是改变方法中的接口。由于`IRepository`不允许任何变化，我们必须使用更灵活的接口，即`IWriteOnlyRepository`。`RemoteEmployee`比`Employee`更具体，但是因为我们的`IWriteOnlyRepository`被标记了一个`in`关键字，所以它不介意更具体的类型。又名逆变。

```
static void AddRemoteEmployees(IWriteOnlyRepository<RemoteEmployee> repository) => new List<RemoteEmployee>
    {
        new RemoteEmployee("Andrew","Canada"),
        new RemoteEmployee("Carol","UK")
    }.ForEach(repository.Insert);
```

有用！
(☞ﾟヮﾟ)☞

# 摘要

当你的接口允许你使用单一类型时，它没有灵活性，没有变化，因此它是**不变的**。

你可以浏览一份雇员名单，把他们当作`Person`的，因为他们有一个**co**common trait-** *协方差*

您不能将员工列表视为经理列表，但是您应该能够将经理添加到员工列表中。这样你就从相反的一面来处理问题了——矛盾

我希望我把这个奇怪的话题说得更清楚一点！