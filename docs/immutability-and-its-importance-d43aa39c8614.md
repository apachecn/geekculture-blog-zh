# 不变性及其重要性

> 原文：<https://medium.com/geekculture/immutability-and-its-importance-d43aa39c8614?source=collection_archive---------13----------------------->

![](img/890680e64105f791163fe2c858645966.png)

什么是不变性？

这个词在字典中的字面意思是——不会随着时间的推移而改变。在编程语言中，这个词也有相似的意思。在一些编程语言中，对象的不变性是隐含的，但对于 java 来说，情况并非如此。下面用一个例子来详细看看。

在本例中，我们将创建一个“用户”类的对象。用户由姓名、id、出生日期和地址等属性组成。似乎创建一个简单的类。如果我们不熟悉不变性，那么最有可能的是，我们最终会创建下面的类。

```
public class User {private String name;private Long id;
private Date dateOfBirth;
private StringBuilder address;public User(String name, Long id, Date dateOfBirth, StringBuilder address) {
this.name = name;
this.id = id;
this.dateOfBirth = dateOfBirth;
this.address = address;
}public String getName() { return name;    }
public void setName(String name) { this.name = name;    }
public Long getId() { return id;    }
public void setId(Long id) { this.id = id;    }
public Date getDateOfBirth() { return dateOfBirth;    }
public void setDateOfBirth(Date dateOfBirth) { this.dateOfBirth = dateOfBirth;    }
public StringBuilder getAddress() { return address;    }
public void setAddress(StringBuilder address) { this.address = address;    }
}
```

如果您不希望 User 的对象是不可变的，这是非常好的。但是如果你想要一个不可变的对象，那么你需要修改你的代码。在修改之前，我们先测试一下上面的代码。

```
@Test
void testImmutability() {User user = new User("TestName", 1234L, new Date(1990, 1, 1), new StringBuilder("India"));user.setName("AnotherName");
user.setId(3456L);
user.setDateOfBirth(new Date(2020, 1, 1));
user.setAddress(new StringBuilder("CountryDoesNotExist"));}
```

根据不变性的定义，对象“用户”不应该改变其状态，这意味着用户的所有属性都不应该是可编辑的，或者没有其他方法应该更新其值或引用。这么简单！删除 setter 方法，可以吗？让我想想…

用户类别版本— 2

```
private Long id;
private Date dateOfBirth;
private StringBuilder address;public User(String name, Long id, Date dateOfBirth, StringBuilder address) {
this.name = name;
this.id = id;
this.dateOfBirth = dateOfBirth;
this.address = address;
}public String getName() { return name;    }
public Long getId() { return id;    }
public Date getDateOfBirth() { return dateOfBirth;    }
public StringBuilder getAddress() { return address;    }

}
```

让我们再测试一次-

```
@Test
void testImmutability() {User user = new User("TestName", 1234L, new Date(1990, 1, 1), new StringBuilder("India"));assertEquals("TestName", user.getName());
assertEquals(1234L, user.getId());
assertEquals(new Date(1990, 1 ,1), user.getDateOfBirth());
assertEquals("India", user.getAddress().toString());}
```

没有 setter 方法，没有其他方法来更新值，如果你运行上面的测试将会成功！看来我们成功了！

但是在上面的代码中还有一个问题，让我们在下面修改后的测试中看看。

```
@Testvoid testImmutability() {User user = new User("TestName", 1234L, new Date(1990, 1, 1), new StringBuilder("India"));user.getAddress().append(" Some addition Address");
user.getDateOfBirth().setMonth(6);assertEquals("TestName", user.getName());
assertEquals(1234L, user.getId());
assertEquals(new Date(1990, 1 ,1), user.getDateOfBirth());
assertEquals("India", user.getAddress().toString());}
```

如果您观察，即使没有 setter 方法，地址和出生日期也会改变，因为“StringBuilder”和“date”对象本质上都是可变的。但是' Name '(字符串)和' Id '(整数)不是。因此，我们需要对本质上可变的属性做一些规定。让我们看看例子。

```
public Date getDateOfBirth() {
 return new Date(this.dateOfBirth.getYear(),    this.dateOfBirth.getMonth(), this.dateOfBirth.getDate());
}public StringBuilder getAddress() {
 return new StringBuilder(this.address);
}
```

我刚刚更新了上面代码片段中的 getter 方法，我们复制了对象，然后从 getter 方法返回副本。如果您在您的类中更新这些 getter 方法并运行测试，这一次您的测试将成功运行。

要使一个类不可变，还有一条规则，我们需要使这个类成为 final。但在我看来，这不是最重要的规则，因为我们不会为这类类创建子类。

# 不可变类的应用

主要的用例是计算对象的散列。散列只不过是返回整数值的数学函数。哈希的计算通常是为了给对象分配一个唯一的值，因此在计算哈希时，最好考虑所有的属性。

让我们考虑一下之前作为例子的用户类。我们有四个属性，我们将从这四个属性中创建一个散列。创建一个散列仅仅意味着从 Object 类中重写 hashcode 方法，并编写一个有意义的散列函数！

```
@Override
public int hashCode() {return Objects.hash(address, dateOfBirth, id, name);}
```

现在考虑您已经创建了您的用户类的五个对象，并且您的类没有被更新来创建不可变的对象。你把你所有的对象放在一个集合或者一个地图中(作为一个键)。现在，在代码的某个地方，您更新了刚刚放入 Set/Map 中的用户对象的状态。集合/映射的工作原理是散列，因此它根据将对象放入集合/映射时生成的散列码来存储对象。因此，当将对象放入 Set/Map 用户对象时，它具有不同的 hashcode，并且在更改对象的状态后，它可能具有不同的 hashcode，这可能会在您的程序中导致不需要的结果。

但是如果你让你的类不可变，然后在这样的数据结构中使用它，它会工作得更准确。