# Ruby 中的功能成分

> 原文：<https://medium.com/geekculture/functional-composition-in-ruby-a9929055899a?source=collection_archive---------20----------------------->

## 让我们来探索一下函数式思维，以及它在 wrt Ruby 中意味着什么。

![](img/56c6edb04d48b79381379259ebe14b9b.png)

Photo by [Valeria Bold](https://unsplash.com/@valeriabold?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/functional-composition?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我们都知道 Ruby 是一种面向对象的语言，虽然这是事实，但是我们仍然可以以一种纯函数的方式使用 Ruby。如果我们那样做，就不会太 Ruby 了，所以让我们看看我们可以从函数式编程中得到什么启发。

函数式编程本质上就像编写一个数学程序。让我们通过一个例子来理解这一点，假设我想找出数组中的第一个奇数。现在我们将写两个目标相同的函数，一个用 Ruby 方式，另一个用函数方式。

```
# Ruby way
def first_odd(array)
  for i in 0...array.size
    break i if i.odd?
  end
end
```

在 Ruby 函数中，我们在循环的每次迭代中重新分配 I 的值，这在数学函数中是不可能的，因为我们定义了一个变量，这是定义而不是赋值。这是函数式编程和非函数式编程的主要区别，函数式编程强调不变性和引用透明性(意思是如果你一次又一次地传递相同的值给一个函数，输出总是相同的。)现在，让我们看看如何以函数方式编写相同的方法:

```
# Functional way
def first_odd(array)
  i = array.shift 
  return nil if array.empty?
  if i.odd?
    return i
  else
    first_odd(array) 
  end 
end
```

用函数的方式我们写了一个递归程序，它就像一个数学函数，即它检查第一个元素是否是奇数，如果不是，它用剩余的元素调用自己。由此我们可以得出关于函数式编程的三个要点，我们需要记住:

1.  函数是定义，而不是指令列表。
2.  我们不分配变量，我们定义事物。
3.  我们可以在另一个函数中传递函数，就像变量一样。

让我们更深入地探讨功能组合如何在日常生活问题中拯救我们。假设我们有一个狗收养申请，我们被要求提供一份关于这些狗被收养情况的报告。你已经有了一个狗及其属性的列表。

```
# We have:
$ dogs.sample
=> #<struct Dog adoption_fees: 12, rating: 4, breed: :golden, adopted: 22>
```

我们可以在报告中要求的第一件事非常简单，只是平均收养费用。为此，我们可以创建一个报告类，然后添加一个 inititalize 方法来获取并存储所有狗的列表，然后添加一个 run 方法来计算平均收养费用。

```
class Report
  def initialize(dogs)
    @dogs = dogs
  end # [inject](https://apidock.com/ruby/Enumerable/inject) is an enumerable function
  def run
    money_taken = @dogs.inject(0) do |total, dog| 
      (dog.adoption_fees * dog.adopted) + total
    end

    total_adoption =  @dogs.inject(0) do |total, dog| 
      dog.adopted + total
    end

    money_taken / total_adoption
  end 
end$ Report.new(dogs).run
=> 20
```

最近，由于 COVID，许多 Golden 被收养，现在我们需要一份新的报告来显示组织从中获得的平均收养费用。我们可以调整前面的代码来添加一个新的参数，这个参数只选择一个给定的品种并进行计算。接下来我们需要被收养的前三个品种。这些请求会使我们报告类变得更长，随着新请求的到来，这种方法将变得更难维护，并将引入许多标志和条件逻辑。有很多方法可以重构这种情况，但是因为我们在看函数组合，所以让我们看看用 Ruby 的函数编程思想可以做些什么。

#注意:这部分会大量使用 lambdas，所以如果你需要修改: [Ruby Blocks，Procs 和 Lambdas](https://anjali-jaiswal.medium.com/block-proc-and-currying-in-ruby-6c00f91959f6)

```
# lambda for summing up using inject
sum = -> list { list.inject(&:+) } # lambda for calculating average adoption fee
total_adoption_fee = -> dogs do
  adoption_fee = dogs.map { |dog| dog.adoption_fees * dog.adopted }
  sum[adoption_fee]
end

# lambda for calculating average adoption fee
avg_adoption_fee = -> dogs do
  total_adoption_fee[dogs]/ total_adoptions[dogs]
end# lambda for getting number of dogs adopted
total_adoption = dogs { sum[dogs.map(&:adopted)] }$ avg_adoption_fee[DOGS] 
=> 20# now, we need to get adoption fees only for golden's
# hence we will add a new lambda for getting jut one breed
golden = -> dogs { dogs.select { |dog| dog.breed == :golden }}
```

让我们重写我们的 Reports 类，在这里我们可以使用我们刚刚创建的 lambdas。

```
class FunctionalReport

  # splated argument will take a list of functions generated at
  # each stage of report
  def initialize(dogs, *fns)
    @dogs = dogs
    @fns = fns
  end def run
    @fns.inject(@dogs) do |last_result, fn|
      fn[last_result]
    end
  end
end$ FunctionalReport.new(DOGS, avg_adoption_fee).run
=> 20
$ FunctionalReport.new(DOGS, golden, avg_adoption_fee).run
=> 12# We can create more lambdas and work accordingly, lets say someone # wants to know adoption fees of dogs which have rating of three and # higherhigh_rating = -> dogs { dogs.select { |d| d.rating >= 3 }}$ FunctionalReport.new(DOGS, high_rating, avg_adoption_fee).run
=> 15
```

如上所述，通过使用函数式编程，我们可以安全地推断出:

1.  我们可以在不修改现有代码的情况下添加新功能
2.  用很少的代码创建报告的每一步
3.  不需要支持来自其他步骤的无关代码

但问题是这不是很 Rubish，并且忽略了 Ruby 必须提供的很多表现力和能力。那么，我们可以从 Ruby 的函数式编程中得到什么呢？

> 我们可以使用像 map、select、inject、blocks 这样的方法来创建数据列表，lambdas 用于以后运行代码..

现在从这篇文章中得到的应该是方法，我们编写了实现一些小的特定任务的 lambdas，并找到了让它们一起工作的方法。我们可以对实现小的特定规则的对象做同样的事情，并找到将它们组合在一起的方法。

```
class BreedFilter
  def initialize(breed)
    @breed = breed
  end def apply(dogs)
    dogs.select { |dog| dog.breed == @breed }
  end
end
```

这种面向对象的编码方式更容易阅读和思考，在编写 Ruby 时感觉更自然，最重要的是它将数据视为不可变的。函数式编程有很多东西要教我们，我们不必编写纯粹的函数式代码来获得好处。希望这有助于您理解如何编写更好的代码。再见了，稍后我们会带来新的花絮。保持快乐，保持安全。