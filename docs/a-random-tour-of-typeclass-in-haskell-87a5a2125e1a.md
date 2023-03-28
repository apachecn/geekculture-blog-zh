# Haskell 中 Typeclass 的随机漫游

> 原文：<https://medium.com/geekculture/a-random-tour-of-typeclass-in-haskell-87a5a2125e1a?source=collection_archive---------8----------------------->

这篇文章的灵感来自 Vitaly Bragilevsky 关于[通向 Haskell 复杂性](https://www.youtube.com/watch?v=n3H_YipBDrY)的演讲。这不是一个单子教程。

**单子的疯狂** 自从函数式编程得势以来，人们开始编写[单子教程](https://wiki.haskell.org/Monad_tutorials_timeline)。Monad 可以说是函数式编程领域最著名的流行语，Monad 上也有模因。

![](img/015b233f25e464c2de093cb10819144c.png)

Image is taken from [http://www.quickmeme.com/meme/3ufzp1](http://www.quickmeme.com/meme/3ufzp1)

我既不精通范畴理论，也不精通任何编程语言，但在 Haskell 的背景下，我非常同意 Vitaly Bragilevsky 的建议，即没有理解单子的问题，只有定义(也称为类型、类型类和法则)。我在我的旧帖子[中强调了术语、类型和种类的定义。在这篇文章中，我想谈谈 typeclass。](https://fpbyintuition.medium.com/term-type-and-kind-in-functional-programming-8fbd47be9728)

**特定多态性** type classes 的目的是定义约束构造函数，这些约束构造函数具有相关的函数/数据类型/类型同义词，可以为不同的类型提供不同的实现(类型可以是具体的，也可以不是具体的)。

**规则** 先从著名的 Monad typeclass 说起。我们可以遵循两条规则。

```
**class** Monad m **where**
  (>>=)  :: m a -> (a -> m b) -> m b
  return :: a -> m a
```

**规则 1:**`**->**`**前后的任何类型变量都必须是具有 kind signature** `**Type**` **的具体类型。**
1。我们从查看函数`return`开始，我们知道`a`是一个具体类型，所以`a`的类型是`Type`。
2。`m a`也是一个具体类型，既然`a`的种类是`Type`，那么`m`的种类就是`Type -> Type`。
3。我们检查这个假设是否也适用于函数`>>=`。

**规则二:a 类型约束的种类为** `**Constraint**`
1。从规则 1 中，我们知道`m`的种类是`Type -> Type`。
2。类型约束`Monad m`的种类是`Constraint`，由此我们推导出约束构造器`Monad`的种类是`(Type -> Type)-> Constraint`。

通过检查类型签名，我们可以知道哪些类型可能有一个特定类型类的实例。然而，拥有匹配的 kind 签名并不能保证一个类型拥有一个 typeclass 的实例，例如[的](https://www.fpcomplete.com/blog/2016/11/covariance-contravariance/)，检查仿函数、协变仿函数和逆变仿函数。

类型构造器`Maybe`的种类是`Type -> Type`，我们可以为`Maybe`写一个实例。

```
**instance** Monad Maybe **where**
  Nothing  >>= f **=** Nothing
  (Just x) >>= f **=** f x
  return         **=** Just
```

我们不能为`Either`写一个实例，因为`Either`的种类是`Type -> Type -> Type`。相比之下，`Either a`的类型是`Type -> Type`，我们可以为`Either a`编写一个`Monad`实例。

```
**instance** Monad (Either e) **where**
  Left  l >>= _ **=** Left l
  Right r >>= k **=** k r
```

Monad typeclass 的所有书面实例都应该满足三个定律，但这里不讨论它们。有兴趣就看这个的[。值得注意的是，可以编写满足类型签名的非法单子实例](https://wiki.haskell.org/Monad_laws)[。](https://www.reddit.com/r/haskell/comments/16iakr/what_happens_when_a_monad_violates_monadic_laws/)

**类型约束** 

```
f :: (Monad m) => String -> m [Char]
f x = return x
```

这是因为函数`return`只对带有`Monad`实例的类型有效。

但是为什么在下面的例子中我们不需要任何签名呢？注意以下功能中`>>=`和`return`的使用。

```
g :: a -> Maybe a
g x = Just x >>= return
```

**等式约束** 这是因为`m`在这个场景中是已知的。为了保持一致，通过启用扩展`TypeFamilies`，我们可以将函数`g`重写如下。在这个函数中，我们明确地将`m`等于`Maybe`。这是一个过于简单的例子，对于真实世界的例子，检查[这个](https://journal.infinitenegativeutility.com/haskell-type-equality-constraints)出来。

```
g :: (Monad m,m ~ Maybe) => a -> m a
g x = Just x >>= return
```

**类型应用**

```
**>** :t return 
return :: Monad m => a -> m a**>** :t return @Maybe
return @Maybe :: a -> Maybe a**>** :t return @Maybe "hello"
return @Maybe "hello" :: Maybe [Char]**>** return @Maybe "hello"
Just "hello"
```

**灵活实例** 函数`id`绝对是最简单的函数，它只是简单地取一个 term 值，并返回相同的 term 值。`id :: a -> a`
我天真的以为可以用 typeclass 重写函数`id`。如果我们尝试实现以下内容

```
**class** Id m **where**
  identity :: m -> m**instance** Id m **where**
  identity m = m 
```

编译器会给我们抛出一个错误

```
 • Illegal instance declaration for ‘Id m’
        (All instance types must be of the form (T a1 ... an)
         where a1 ... an are *distinct type variables*,
         and each type variable appears at most once in the instance head.
         Use FlexibleInstances if you want to disable this.)
    • In the instance declaration for ‘Id m’
```

这是因为这个实例声明太普通了，为了解决这个问题，我们可以启用扩展`FlexibleInstances`。之后，`identity "1"`应该会返回`"1"`。

**参数化多态性**
现在，即使我们不知道`x`的类型是什么，我们也可以编写没有类型约束的标识函数，因为实例适用于所有类型。

```
h :: a -> a
h x = identity x
```

**重叠实例**
在前面的例子中，我们说的实例声明太泛是什么意思？想象一下，如果有人编写如下的另一个实例。

```
**class** Id m **where**
  identity :: m -> m**instance** Id m **where**
  identity m = m**instance** Id Bool **where**
  identity m = not m
```

如果我们尝试运行`identity Bool`，我们会得到以下错误

```
 • Overlapping instances for Id Bool
        arising from a use of ‘identity’
      Matching instances:
        instance Id m -- Defined at Main.hs:24:10
        instance Id Bool -- Defined at Main.hs:27:10
    • In the expression: identity True
      In an equation for ‘it’: it = identity True
```

出现这个错误是因为`True`既匹配类型`Bool`又匹配更通用的类型`a`，GHC 运行时不知道选择哪一个。为了克服这个问题，我们可以重写`Bool`的实例。

```
**class** Id m **where**
  identity :: m -> m**instance** Id m **where**
  identity m = m**instance** {-# OVERLAPPING #-} Id Bool **where**
  identity a = not a
```

现在，如果我们尝试运行`identity True`，我们将得到值`False`。当然，这里的例子没有实际意义。

事实上，如果没有启用扩展`FlexibleInstances`，那么实例声明中必须有且只有一个具体类型构造函数，比如`Maybe`、`Either`。这是因为如果有一个以上的具体类型构造函数，另一个总是可以被泛化。在下面的示例中，任何第二个具体类型构造函数都可以被泛化为类型变量。

```
**instance** Id (Either Bool b) **where** 
  *... implementation -- not allowed when FlexibleInstances is not enabled* 
**instance** Id (Either a Bool) **where** 
 * ... implementation -- not allowed when FlexibleInsances is not enabled*
*-- the most generic form would be* 
**instance** Id (Either a b) **where**
 *... implementation*
```

**多参数类型类** 通过启用扩展`MultiParamTypeClasses`，我们可以定义接受多个类型参数的类型类。

```
**class** Transform a b | a -> b **where**
  transform :: a -> b
```

这`| a -> b`是什么东西？在解释这个之前，我们先来展示一个没有它的例子。

```
**class** Transform a b **where**
  transform :: a -> b **instance** Transform String (Maybe Bool) **where**
  transform "True" = Just True
  transform "False" = Just False
  transform _ = Nothing**instance** Transform String (Maybe Int) **where**
  transform "zero" = Just 0
  transform _ = Nothing
```

当我们运行 write `transform "True"`时，GHC 运行时无法选择实例，因为它没有足够的关于`b`是什么类型的信息。

```
 • Non type-variable argument in the constraint: Transform [Char] b
      (Use FlexibleContexts to permit this)
    • When checking the inferred type
        it :: forall b. Transform [Char] b => b
```

为了克服这一点，我们可以明确地告诉 GHC 什么是我们想要的类型。

```
**>** transform "1" :: Maybe Bool
Nothing
**>** transform @String @(Maybe Bool) "1"
Nothing
```

**函数依赖** 实例声明是开放的，有人可能会添加另一个实例，比如`instance Transform String (Either a b)`，而你可能没有意识到。为了限制这种可能性并帮助编译器推断类型，我们可以通过启用扩展`FunctionalDependencies`来使用[函数依赖](https://stackoverflow.com/questions/20040224/functional-dependencies-in-haskell)。

```
**class** Transform a b | a -> b **where**
  transform :: a -> b**instance** Transform String (Maybe Bool) **where**
  transform "True" = Just True
  transform "False" = Just False
  transform _ = Nothing**instance** Transform String (Maybe Int) **where**
  transform "zero" = Just 0
  transform _ = Nothing
```

如果我们像那样写代码，编译器会抱怨

```
 Functional dependencies conflict between instance declarations:
      instance Transform String (Maybe Bool) -- Defined at Main.hs:55:10
      instance Transform String (Maybe Int) -- Defined at Main.hs:59:10
```

这是因为`| a -> b`意味着对于一个类型`a`，对应的类型`b`只有一个，因为我们用两个不同的类型`b` ( `Maybe Int`和`Maybe Bool`)声明了两个实例，编译器会拒绝接受。然而，反过来是不正确的，它并不阻止你编写如下的实例。

```
**class** Transform a b | a -> b **where**
  transform :: a -> b**instance** Transform String (Maybe Bool) **where**
  transform "True" = Just True
  transform "False" = Just False
  transform _ = Nothing**instance** Transform Bool String **where**
  transform True = "True"
  transform False = "False"
```

**关联类型同义词** 功能依赖的替代方法是使用关联类型同义词。

```
**class** Transform a **where
  type** F a transform :: a -> F a
```

我们可以如下声明实例，它们相当于上一节中的函数依赖的例子。

```
**instance** Transform String **where**
  **type** F String = Maybe Bool 
 * -- transform :: String -> Maybe Bool 
  -- we substitute (F String) with (Maybe Bool)*
  transform "True" = Just True 
  transform "False" = Just False
  transform _ = Nothing**instance** Transform Bool **where**
  **type** F Bool = String 
  *-- transform :: Bool -> String 
  -- we substitute (F Bool) with (String)*
  transform True = "True"
  transform False = "False"
```

**关联数据类型** 原来，你也可以在 typeclass 定义里面定义数据构造函数！下面的例子摘自论文 [Fun with type functions](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/07/typefun.pdf?from=http%3A%2F%2Fresearch.microsoft.com%2F%7Esimonpj%2Fpapers%2Fassoc-types%2Ffun-with-type-funs%2Ftypefun.pdf) 。

```
**class** Memo a **where**
  **data** Table a :: Type -> Type
  toTable :: (a -> w) -> Table a w
  fromTable :: Table a w -> (a -> w)
```

这里我们来考察一下种类
1。`w`的那种是`Type`
2。`a`的那种是`Type`
3。`Table`的那种是`Type -> Type -> Type`
4。`Memo`的种类是`Type -> Constraint`

这里的关键理解是，对于每个实例，将声明一个新的数据构造函数。适合这些签名的一个可能的例子是

```
**instance** Memo Bool **where** 
  **data** Table Bool w = TBool w w 
  toTable f = TBool (f True) (f False) 
  fromTable (TBool x y) b = if b then x else y
```

在这个例子中，声明了一个数据构造函数`TBool`，它有一个类型签名`w -> w -> Table Bool w`

为了完整起见，我认为值得一提的是，我们可以在一个类型类定义中同时拥有关联的数据类型、关联的类型同义词和函数。下面是来自同一篇论文的另一个例子。

```
**class** Graph g **where** 
  **type** Vertex g 
 ** data** Edge g 
  src, tgt :: Edge g -> Vertex g 
  outEdges :: g -> Vertex g -> [Edge g]
```

请随意编写一个实例作为练习。

令人惊讶的是，你可以这样定义 Typeclass

```
**class** Trivial a *-- No further definition*
**instance** Trivial a *-- No futher implementation*
```

这个 typeclass 很简单，因为它适用于所有类型，包括具体类型和非具体类型。
这里的目的是创建一个可在别处使用的约束构造器。

**聚种类** 上例中的`a`是什么种类？说`a`的种类是`Type`所以`Trivial`的种类是`Type -> Constraint`不会错，但是太具体了。`a`的那种也可以是`Type -> Type`的！通过启用扩展`PolyKinds`，我们可以执行以下操作

```
**>** :k Trivial (Either) *-- the kind of Trivial is (Type -> Type -> Type) -> Constraint*
Constraint
**>** :k Trivial (Either Int) *-- the kind of Trivial is (Type -> Type) -> Constraint*
Constraint
**>** :k Trivial (Either Int Int) *-- the kind of Trivial is Type -> Constraint*
Constraint
```

**约束类型** 读到这里，你可能想知道约束构造函数拥有一个类型签名有什么意义，这是因为通过启用扩展`ConstraintKinds`，我们实际上可以使用约束构造函数作为类型！
以爱德华·克米特关于[类型阶级与世界](https://www.youtube.com/watch?v=hIZxTQP1ifo&t=5046s&ab_channel=BostonHaskell)的演讲为例

```
**data** Dict (p :: Constraint) **where**
  Dict :: p => Dict p
```

区分哪些`Dict`是类型构造函数，哪些`Dict`是数据构造函数是很重要的。在这个例子中，第一个和第三个`Dict`是类型构造函数，而第二个`Dict`是数据构造函数。

```
**>** :t Dict @(Show Int)
Dict (Show Int)
**>** :t Dict @(Eq Int)
Dict (Eq Int)
**>** :t Dict @(Trivial Int)  -- Let's not forget our trivial typeclass
Dict (Trivial Int)
```

除此之外，您还可以在类型同义词中使用它。`Cons Int`的那种就是`Constraint`。

```
**type** Cons a = (Eq a , Ord a)
```

深入探究如何在实践中使用约束类型。我推荐看 Andres Lö关于[数据类型——泛型编程](https://www.youtube.com/watch?v=pwnrfREbhWY&t=4612s&ab_channel=Z%C3%BCrichFriendsofHaskell)的演讲。

**派生实例**
编写 typeclass 的实例可能容易出错且重复。幸运的是，编译器可以自动为一些类型类生成实例。让我们尝试为下面的数据类型自动派生一个`Ord`实例。

```
**data** Direction = North | West | South | East
  **deriving (Ord)**
```

这样做会给我们带来以下错误。

```
 • No instance for (Eq Direction)
        arising from the 'deriving' clause of a data type declaration
      Possible fix:
        use a standalone 'deriving instance' declaration,
          so you can specify the instance context yourself
    • When deriving the instance for (Ord Direction)
```

如果我们检查`Ord`的定义，我们会注意到 typeclass `Ord`的定义如下，我们可以看到还有另一个类型约束`Eq a =>`。这意味着类型`a`要有一个`Ord`实例，它必须首先有一个`Eq`实例。

```
**class** Eq a => Ord a **where**
  compare :: a -> a -> Ordering
  (<) :: a -> a -> Bool
  (<=) :: a -> a -> Bool
  (>) :: a -> a -> Bool
  (>=) :: a -> a -> Bool
  max :: a -> a -> a
  min :: a -> a -> a
```

修复相当简单，我们只需要为数据类型`Direction`一起派生`Eq`实例。

```
**data** Direction = North | West | South | East
  **deriving (Ord,Eq)**
```

如果我们看看单子、应用和函子的现代 typeclass 定义。我们会观察到等级制度。类型约束告诉我们，如果一个类型有单子实例，它也必须有应用实例，因此函子实例。然而，反过来并不总是正确的。

```
**class** Applicative m => Monad m **where** 
   *... definition*
**class** Functor f => Applicative f **where** 
   *... definition*
**class** Functor f **where**
   *... definition*
```

代码示例作为[要点](https://gist.github.com/ongyiren1994/30d406f5b3ec936b2f8ff4b765620a15)提供。
*PS:如有错误，敬请指正:)*

**进一步阅读** [GHC 用户手册](https://downloads.haskell.org/ghc/latest/docs/html/users_guide/index.html)是了解更多关于 GHC Haskell 类型系统的极好材料，关于[语言扩展的章节](https://downloads.haskell.org/ghc/latest/docs/html/users_guide/exts.html#language-extensions)令人兴奋不已。

**参考文献:**
1。通往哈斯克尔复杂性的清晰道路 2。[单子教程时间线](https://wiki.haskell.org/Monad_tutorials_timeline)
3。[协方差-逆变](https://www.fpcomplete.com/blog/2016/11/covariance-contravariance/)4。[单子定律](https://wiki.haskell.org/Monad_laws)
5。[当一个“单子”违反单子定律会发生什么？](https://www.reddit.com/r/haskell/comments/16iakr/what_happens_when_a_monad_violates_monadic_laws/)⑥
⑥。H [askell 类型等式约束](https://journal.infinitenegativeutility.com/haskell-type-equality-constraints)7。[An-introduction-to-type class-元编程](https://lexi-lambda.github.io/blog/2021/03/25/an-introduction-to-typeclass-metaprogramming/)
8。[Haskell 中的函数依赖](https://stackoverflow.com/questions/20040224/functional-dependencies-in-haskell)
9。[趣味同类型功能](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/07/typefun.pdf?from=http%3A%2F%2Fresearch.microsoft.com%2F%7Esimonpj%2Fpapers%2Fassoc-types%2Ffun-with-type-funs%2Ftypefun.pdf)
10。[爱德华·克迈特——类型类与世界](https://www.youtube.com/watch?v=hIZxTQP1ifo&t=3559s)
11。[数据类型-泛型编程作者 Andres LH—高级跟踪@ ZuriHac 2020](https://www.youtube.com/watch?v=pwnrfREbhWY&t=4612s&ab_channel=Z%C3%BCrichFriendsofHaskell)
12。[非函子/函子/适用/单子的好例子？](https://stackoverflow.com/questions/7220436/good-examples-of-not-a-functor-functor-applicative-monad)