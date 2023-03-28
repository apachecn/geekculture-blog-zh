# Java 又回来了

> 原文：<https://medium.com/geekculture/java-and-back-again-231012ae113f?source=collection_archive---------29----------------------->

## 初学者的故事…第 3 部分

![](img/b060296ccef7d91423c98442faaace51.png)

Photo by [Nathan Dumlao](https://unsplash.com/@nate_dumlao?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

欢迎阅读我的 Java 学习之旅系列文章的第三部分。在[第一部分](/geekculture/java-and-back-again-e7663acba4d5)中，我讲了一点历史和一些基本原理。在[的第二部分](/geekculture/java-and-back-again-fc0747a0ad3a)，我进入了状态和实例，花了一些时间解释了一些关于面向对象的编程。在这一部分，我们将进入数组和数组列表。我们会谈到两者的区别，我会举两者的例子。

## 数组

与我所熟悉的其他编程语言不同，java 中的数组只能保存相似数据类型的集合。所以你不能在同一个数组里保存一个字符串和一个 int。像大多数其他语言一样，元素的位置被称为索引。索引从零开始，一直到最后一个元素。这意味着最后一个元素的索引将比元素的实际数量少 1。例如，如果数组中有四个元素，那么最后一个元素的索引将是三。还有一点我想指出的是，Java 数组是固定大小的。一旦它们被声明，它们的大小就不能改变。

下面是如何创建数组的例子，有两种不同的方法。我还将展示引用索引的例子，以便您可以添加或更改数组中的元素。

```
String[] words = {"The", "One", "Ring", "to", "Rule", "Them","All"};
//You can use bracket notation and add the all. String[] precious = new String[2] //The two represents how large the  
                                  array is. This array only has two 
                                  elements. precious[0] = "My";
precious[1] = "Precious";
//Or you can use the new keyword and assign them seperatly
//Notice that when I assigned the first string I placed it at zero, //which is the first index. //If you would wan to change an elemnt you can also use bracket notation. precious[1] = "Fishes"
precious = {"My", "Fishes"};
```

## 数组列表

当你想保存不同数据类型的集合时，可以使用数组列表。与数组不同，数组列表的大小可以调整，元素可以添加和删除。数组列表不是 java 自带的，所以您必须将一个包导入到 java 文件中才能使用它们。数组和 ArrayList 的另一个区别是不能用值声明 ArrayList。

```
import java.util.ArrayList;       //Import ArrayList packageArrayList<String> fellowship = new ArrayList<String>();
//create an ArrayList called fellowship
```

有几种不同的方法可以用来修改和数组列表，这取决于你想完成什么。

```
import java.util.ArrayList; public class Fellowship {
  public static void main(String[] args) { ArrayList<String> fellowship = new ArrayList<String>(); fellowhip.add("Frodo");     //add memebers(elements) to the 
      fellowhip.add("Sam");         fellowship. 
      fellowhip.add("Mary");
      fellowhip.add("Pippin");
      fellowhip.add("Aragorn");
      fellowhip.add("Boromir");
      fellowhip.add("Legolas");
      fellowhip.add("Gimli");
      fellowhip.add("Gandalf"); fellowship.remove(0);      //remove Frodo, Sam, and Boromir
      fellowship.remove("Sam");  
      fellowship.remove(3);      //Notice that after I removed Frodo     
                                   and Sam, Boromir's index changed 
                                   to three from five. Keep that in 
                                   mind when removing elements using 
                                   their index.   }}
```

## 结论

正如你所看到的，数组和数组列表不仅和其他语言有一点不同，而且它们本身也有所不同。数组只适用于相似数据类型的元素，而数组列表适用于动态组。我希望这能对你有所帮助。请注意下一部分。