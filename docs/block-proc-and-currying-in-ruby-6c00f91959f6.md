# Ruby 中的 Block、Proc 和 curry

> 原文：<https://medium.com/geekculture/block-proc-and-currying-in-ruby-6c00f91959f6?source=collection_archive---------9----------------------->

## 了解更多关于 Ruby 中的 block、proc、lambda 和 currying 的知识。

![](img/085367db3ce022c6d3587b8c3dede810.png)

Photo by [emy](https://unsplash.com/@grimnoire?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/curry?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

让我们深入了解什么是 blocks、procs 和 lambdas，以及如何使用它们。

# **块**

在 Ruby 中，块是传递函数引用的最常见方式。他们通过使函数作为参数传递变得超级容易和易读，为一些看起来很优雅的 DSL 打开了大门，例如:

您可以将 block 作为参数传递，方法是将它传递到花括号{}中，或者将其封装在 do 中..结束。

块格式 1:

```
array = [1,2,3,4] 
array.map do |ele|
 do_something
end
```

块格式 2:

```
array.map { |ele| do_something }
```

当你在一个方法中时，你怎么知道一个块是否被传递了呢？我们实际上可以调用一个方法`block_given?`，如果它返回 true，那么我们可以访问这个块吗？是的，我们可以，打电话给`yield`:

```
def called_with_a_block?
  if block_given?
    puts "Block was given"
    yield_and_print
 else
    puts "No block given"
  end
enddef yield_and_print
  puts "Before calling yield"
  yield
  puts "After calling yield"
end$ called_with_a_block? 
=> "No block given"
$ called_with_a_block? { puts "I am the block" }
=> "Block was given"
=> "Before calling yield"
=> "I am the block"
=> "After calling yield"# Note: yield stops the execution of method at the point of calling and runs the block of code which was passed and once that block finishes it resumes the method where it left off.
```

仅仅运行一个块似乎没有多大用处，我们希望能够向它传递值，并获得一些值作为回报:

```
# let's make something like Ruby's built-in method each
def each_in_array(array)
  for i in 0...array.size
    yield array[i]
  end
endeach_in_array([1,2,3,4]) do |x|
  puts x * 2
end
```

当我们编写一个采用块的方法时，我们可以使用一些特殊的语法将该块赋给一个参数。您可以指定一个可选的参数来保持以&号为前缀的块。

```
def modify_prices(prices, &block)
  block.inspect
endprices = [10, 20, 30]$ modify_prices(prices) { |x| x * 0.2 }
```

# PROCS

“proc”是`Proc`类的一个实例，它包含一个要执行的代码块，并可以存储在一个变量中。要创建一个 proc，您调用`Proc.new`并传递给它一个块。因为过程可以存储在变量中，所以它也可以像普通参数一样传递给方法。

```
# Proc Format 1
cheap = Proc.new do |price|
  price < 50
end# Proc Format 2
$ cheap = Proc.new { |price| price < 50 }# Call Format 1
$ cheap.call(100)# Call Format 2
$ cheap[100]# Call Format 3
$ cheap.(100)# Proc can be used to call a symbol on the blocks:
$ array.map(&:to_s) is similar to array.map { |x| x.to_s }
```

# 希腊字母的第 11 个

与 proc 对象类似的是 lambdas，你可以通过使用关键字`lambda`后跟一个块或者通过指向一个箭头并传递一个块来创建它，你可以像 procs 一样调用 lambdas。

```
cheap = lambda { |price| price < 30 }Orcheap = -> price { price < 30 }Orcheapest = -> price1, price2 { [price1, price2].min }
```

Lambdas 本质上是具有一些区别因素的过程。它们在两个方面更像“常规”方法:它们在被调用时强制传递参数的数量，并且它们使用“常规”返回。

当调用一个期望参数而没有参数的 lambda 时，或者如果你将一个参数传递给一个不期望它的 lambda，Ruby 会抛出一个`ArgumentError`。

```
# Proc
p = Proc.new { |x| "You called me with #{x.inspect}" }
$ p.call
=> "You called me with nil"# Lambda
l = -> x { "You called me with #{x.inspect}" }
$ l.call
=> ArgumentError: wrong number of arguments (given 0, expected 1)
```

此外，lambda 处理 return 关键字的方式与方法相同。当调用 proc 时，程序将控制权交给 proc 中的代码块。因此，如果 proc 返回，则当前作用域返回。如果在函数内部调用 proc 并调用`return`，函数也会立即返回。

```
# Proc
def return_from_proc
  a = Proc.new { return 10 }.call
  puts "This will never be printed."
end$ return_from_proc.call
=> 10
```

这个函数将把控制权交给 proc，所以当它返回时，函数返回。在这个例子中调用函数永远不会打印输出并返回 10。

```
# Lambda
def return_from_lambda
  a = lambda { return 10 }.call
  puts "The lambda returned #{a}, and this will be printed."
end

$ return_from_lambda.call
=> The lambda returned 10, and this will be printed.
```

当使用λ时，它*将*被打印。在 lambda 中调用`return`将像在方法中调用`return`一样，因此用`10`填充`a`变量，并将该行打印到控制台。

# 加脂操作

Currying 是部分应用程序的概念，我们现在可以用一些参数调用一个 Proc，然后用其余的参数调用它。我们用一个例子来理解一下。

```
# Let's create a proc which multiplies two numbers
mult = -> x, y { x* y }$ mult[2,3]
=> 6# calling proc with curry; whatever argument we provide to curry now will become the default first argument.
$ double = mult.curry[2]# calling double
$ double[2]
=> 4$ double[4]
=> 8
```

既然我们已经了解了什么是 currying，让我们来看一些真实的例子。假设我们有一个电子商务平台，我们希望不时地运行一些促销活动，提供不同的交易，为此，我们将创建一个促销类，调用该类时会附带一个描述和一个计算促销金额的过程。

```
class Promotion
  def initialise(description, calculator)
    @description = description
    @calculator = calculator
  end def apply(total)
    total - @calculator[total]
  end
end# 15% discount
discount = Promotion.new(
    "20% off everything",
    -> total { total * 0.20 }  
)$ discount.apply(100)
=> 80# 10% discount if spend over 50
ten_pc = Promotion.new(
  "10% off if you spend over 50",
  -> total { total > 100 ? total * 0.10 : 0 } 
)$ ten_pc(45)
=> 45$ ten_pc(100)
=> 90.0
```

我们可以在创造促销的同时使用咖喱。提升班不在乎他们是否被咖喱化。它只需要知道它们是可以调用折扣的 procs。

```
calc = -> threshold, discount, total do
    total > threshold ? total * discount : 0
end ten_pc_calc = calc.curry[50, 0.1]
$ ten_pc_calc[45]
=> 45
$  ten_pc_calc[100]
=> 90.0

fifteen_pc_calc = calc.curry[100, 0.15]# creating a promotion using currying proc
ten_pc = Promotion.new(
  "10% if you spend over $50",
  ten_pc_calc
)$ ten_pc.apply(100)
=> 90.0
```

因此，currying procs 是一种消除重复的有效方法，它给我们提供了一种编写通用函数的方法，并通过重用其他函数或专门使用这些函数来做一些在我们的问题领域有意义的事情。因为 curry 过程存储在一个变量中，所以我们可以给这个特殊的功能起一个在代码中有意义的名字。

目前就这些。会带着一些新的话题和更多的见解回来。保持快乐，保持安全。