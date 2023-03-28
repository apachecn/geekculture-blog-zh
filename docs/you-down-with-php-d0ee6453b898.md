# 你懂 PHP 吗？

> 原文：<https://medium.com/geekculture/you-down-with-php-d0ee6453b898?source=collection_archive---------18----------------------->

## 第三部分

![](img/286b187ce4384ec365132fb7747266dd.png)

Photo by [James Harrison](https://unsplash.com/@jstrippa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这是我 PHP 之旅的第三部分。我的第一篇文章讲述了 PHP 的一点历史。我们深入到变量、赋值操作符以及与。)点运算符。你可以在这里阅读那篇文章。在第二个例子中，你可以在这里看到，我们深入到函数中，包括自定义函数和内置函数。在这一节中，我们将学习数组结构。我将举例说明如何创建它们，如何修改它们，以及几种不同的显示方式。

作为一个 referesher，一个数组只是一个数据集合，其中的每一部分都有一个数字索引号。这些索引从零开始，到最后一个元素结束。在 PHP 中，标准数组被称为有序数组。PHP 有两种不同的创建数组的方法。一个使用 array()，关键字，另一个使用括号标记[]。看看下面的例子。

```
$keyword_collection = array("First","Second","Third");
   echo $keyword_collection[0];   //"First" $bracket_collection = ["Fourth", "Fifth","Sixth"];
   echo $bracket_collection[2];   //"Sixth"
```

要访问数组中的一个元素，你只需要调用它的索引。所以对于第一个数组，我调用了索引 0，因为第一个索引中的 0，所以返回了" first"。在第二个数组中，我调用了 two，返回了" Sixth ",因为它是最后一个索引，即使它们是三个元素，索引也是从零开始的。

现在我们如何修改一个数组？他们有几种不同的方法来增加和减少数组中的元素。我们要学习的前两个函数是 array_pop()和 array_shift()，前者从数组中移除最后一个元素，后者从数组中移除第一个元素。

```
$keyword_collection = array("First","Second","Third");
    echo array_shift($keyword_collection);  
      //$keyword_collection("Second","Third")$bracket_collection = ["Fourth", "Fifth","Sixth"];
   echo array_pop($bracket_collection);   
      //$bracket_collection("Fourth", "Fifth")
```

与此相反，它们也有向数组添加元素的方法。我们有 array_push()将元素添加到数组的末尾，array_unshift()将元素添加到数组的开头。

```
$keyword_collection = array("First","Second","Third");
    echo array_unshift($keyword_collection, "First");  
      //$keyword_collection("First","Second","Third")$bracket_collection = ["Fourth", "Fifth","Sixth"];
   echo array_push($bracket_collection, "Sixth");   
      //$bracket_collection("Fourth", "Fifth", "Sixth")
```

PHP 有另一种形式的数组，即关联数组。这可能类似于 Ruby 中的散列。它们的主要区别是 key=>value dynamic，这时你有一个代表值的关键字，而不是一个索引。这里有一个例子。

```
$star_wars = ["jedi" => "Luke Skywalker", "Sith" => "Darth Vader"];
   echo $star_wars["jedi"];
     // "Luke Skywalker"$also_star_wars = ["scoundrel" => "Han Solo", "bounty hunter" => "Boba Fett"];
    echo $also_star_wars["bounty hunter"];
        //"Boba Fett"
```

我最后想讲的是如何组合数组。您可以使用联合运算符(+)。这里要注意的是，第二个数组总是被追加，这意味着它一直到最后。所以如果你想要特别的订单，一定要小心。

```
$star_wars = ["jedi" => "Luke Skywalker", "Sith" => "Darth Vader"];
   echo $star_wars["jedi"];$also_star_wars = ["scoundrel" => "Han Solo", "bounty hunter" => "Boba Fett"];$final_star_wars = $star_wars + $also_star_wars;$final_star_wars = ["jedi" => "Luke Skywalker", "Sith" => "Darth Vader", "scoundrel" => "Han Solo", "bounty hunter" => "Boba Fett"];
```

简而言之，这就是数组。我强烈建议深入研究 PHP 的文档。它有很多很棒的信息，还有很多细节，如果不写得很长，我就无法在这里塞进去。