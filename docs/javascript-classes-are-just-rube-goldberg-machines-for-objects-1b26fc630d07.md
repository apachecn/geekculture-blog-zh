# JavaScript 类只是对象的 Rube Goldberg 机器

> 原文：<https://medium.com/geekculture/javascript-classes-are-just-rube-goldberg-machines-for-objects-1b26fc630d07?source=collection_archive---------12----------------------->

![](img/0360598129719cbae466ef7812b6605d.png)

Rube Goldberg machines are overly complicated devices for simple tasks ([Image source](https://www.therapidian.org/sites/default/files/imagecache/article_main/article_images/rube.jpg))

一个类的全部工作就是最终创建一个对象。但是 JavaScript 有一种最简单、最清晰、最简洁的方法来创建对象，并且有一种熟悉的非常简单的方法来以一种特定的方式反复创建对象:

```
{} // an object() => ({}) // a function that returns the same object shape every time.
```

那么我们能合理地做到什么程度呢？嗯，套用一个体育参考，它可以去所有的方式！

```
// Why do all of this
class SubClass extends BaseClass {
  #state = {} 

  constructor(arg) {
    super(arg);

    this.#state = arg
  } get state() {
    return this.#state
  } set state(newState) {
    this.#state = newState
  }

  gimmeAThing() { 
    return `here's a thing`
  } goDoAThing() {
    console.log('did a thing')
  } 
} // When you can to this
const factory = (state) => ({
   ...baseFactory(), 
   state,
   gimmeAThing: () => `here's a thing`,
   goDoAThing: () => { console.log('did a thing') } 
})
```

# 偏好组合而非继承

就像，我是说，每个人都知道这个，对吗？很有名。这是你向别人暗示你实际上已经读过四人组的[设计模式书的引语，这本书已经在你的书架上放了很多年了，你会不时地告诉自己你真的应该坐下来读一读，因为当你想到它的时候，你可能已经从你在网上阅读的文章中引用了更多的内容，而不是从你自己图书馆的书中引用的内容。不是我。但是，你知道…其他人😳。](https://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612/ref=sr_1_4?crid=1G1VPOL7OIZF0&dchild=1&keywords=design+patterns&qid=1618624417&sprefix=practical+microser%2Caps%2C244&sr=8-4)

组合显然比继承更加灵活多样。据我所知，没有一个严肃的软件工程师质疑一个人应该更喜欢组合而不是继承的想法。

所以让我问你这个问题。对于 JavaScript 类，如何选择组合而不是继承呢？那看起来像什么？JavaScript 类没有允许组合的特征或混合或类似的东西。如果你想在 JavaScript 中扩展一个类的功能，你需要子类化它。对于 JavaScript 类来说，组合并不是一个真正的选项。

那么，如果组合从一开始就不存在，我们为什么还要使用类呢？！？！

我怀疑部分原因是一个[吸引新奇谬论](https://en.wikipedia.org/wiki/Appeal_to_novelty)的元素。类是语言的一个新特性，因此必须优于先前存在的类。但也许不止如此，还有一个错误的假设，即类比对象更通用或提供更好的人机工程学。它们的方法，getter，setter，private members，static members，constructor 等等，难道不比我们以前用函数构造函数和闭包做的更好吗？

嗯，因为我们使用旧的 JavaScript 特性来模拟类，是的。上课更好。但是如果我们一开始就不应该试图模仿类呢？

看，在 JavaScript 中使用类是有正当理由的…可能吧。或者至少我愿意承认，我还没有经历过要解决的所有问题，至少对其中一些来说，类是一个更好的解决方案。只是…除了处理 DOM(在这种情况下，人们不得不处理类，因为这是规范作者半武断地决定的)，我还没有碰到它们中的任何一个。在我遇到的每一个现实世界的案例中，简单的对象至少和类一样通用和符合人体工程学，而且大多数时候更受欢迎。事实上，在我看来，类只是对象的 Rube Goldberg 机器。在课堂上有更多的概念和仪式，只是为了做我们已经可以做得更简单的事情。不相信我？让我们看一些例子。

## 构造给定形状的物体

表面上看，这是一个经典案例。你需要一个对象？使用一个类。让我们来看看。

*上课模式*

```
class BeautifulObjectClass {
  isMagnificent = true  

  constructor(marvelousness = 11) {
    this.marvelousness = marvelousness
  }
}const myBeautifulObject = new BeautifulObjectClass()
myBeautifulObject.isMagnificent // true
myBeautifulObject.marvelousness // 11
```

令人愉快。不能再简单了。可能吗？嗯，实际上，是的！

*工厂模式*

```
const beautifulObjectFactory = (marvelousness = 11) => ({
  isMagnificent: true,
  marvelousness 
})const myBeautifulObject = beautifulObjectFactory()
myBeautifulObject.isMagnificent // true
myBeautifulObject.marvelousness // 11
```

更短、更清晰、更多信号、更少噪音、更少需要考虑的事情。这只是一个返回对象的函数。仅此而已。但这叫工厂模式。它能做类能做的任何事情。

## 方法？当然可以。

*班级模式*

```
class BedazzlerClass {
  dazzle() {
    console.log('Look at how dazzling I am!')
  }
}const bedazzled = new BedazzlerClass()
bedazzled.dazzle() // Look at how dazzling I am!
```

*工厂模式*

```
const bedazzlerFactory = () => ({
  dazzle() {
    console.log('Look at how dazzling I am!')
  } 
})const bedazzled = bedazzlerFactory()
bedazzled.dazzle() // Look at how dazzling I am!
```

## 私人会员？来吧儿子。

*班级模式*

```
class Shhh {
  _secrete = ''

  constructor(secrete) {
    this._secrete = secrete
  } 

  revealYourSecretes: () => this._secrete

}const shh = new Shhh('Mischief managed')
shh.revealYourSecretes() // 'Mischief managed'
shh._secrete // 'Mischief managed' 🤥
shh._secrete = 'password123' // 🤭
shh._secrete  // 😱
```

你听到了吗？对于类，你有这个“私有约定”的问题，你必须在一个公共的属性中存储你的值，但是我们让用户知道我们希望他们通过以“_”开始名字来假装它是私有的。那不太安全。但是这很酷，JavaScript 类有了一种拥有真正私有成员的方法！

```
class Shhh {
  #secrete = ''

  constructor(secrete) {
    this.#secrete = secrete
  } 

  revealYourSecretes: () => this.#secrete

}const shh = new Shhh('Mischief managed')
shh.revealYourSecretes() // 'Mischief managed'
shh.#secrete // Uncaught SyntaxError: Private field '#name' must be declared in an enclosing class 😅
```

所以这很好。这当然是一个进步，但是我们现在又增加了一件需要记住的事情。要记住的东西越多，人为错误的足迹就越多。越简单越好。尤其是如果你没有失去任何功能。现在让我们看看工厂模式解决方案。

*工厂模式*

```
const shhhFactory = (secrete = '') => ({
  revealYourSecretes: () => secrete
})const shh = shhhFactory('Mischief managed')
shh.revealYourSecretes() // 'Mischief managed'
shh.secrete = 'password123'
shh.revealYourSecretes() // 'Mischief managed' 🙌
```

## getter/setter？没错。

*班级模式*

```
class NameManagerClass { constructor (name = '') {
    this.#name = name  
  } get name() {
    return this.#name
  }

  set name(newName) {
    this.#name = newName
  }}const nameManager = new NameManagerClass()
nameManager.name // ''
nameManager.name = 'Rumplestiltskin'
nameManager.name // 'Rumplestiltskin'
```

*工厂模式*

```
const nameManagerFactory = (name = '') => ({

  get name() {
    return name
  },

  set name(newName) {
    name = newName
  }}const nameManager = new NameManager()
nameManager.name // ''
nameManager.name = 'Rumplestiltskin'
nameManager.name // 'Rumplestiltskin'
```

## 作文

在对象的上下文中，组合是指一个对象可以由两个或更多其他对象的所有部分组成。让我们看看这个类是什么样子的。

*班级模式*

```
class AClass {
  propA: 'propA'  
}class BClass {
  propB: 'propB'
}class CompositeClass {
   // 🤷 
}
```

据我所知，在 JavaScript 中没有规范的方法来组合类。虽然是编程，对吧。我相信我们可以一起破解一些东西…

```
class APrimeClass {
  propAPrime = 'propAPrime'  
}class AClass extends APrimeClass {
  propA = 'propA'  
}class BClass {
  propB = 'propB'
}class CompositeClass {
  constructor() {
    const a = new AClass()
    const b = new BClass()

    const ab = {
      ...a,
      ...b
    } Object.entries(ab).forEach(([key, val]) => {
      this[key] = val
    }) }
}new CompositeClass()
// { propAPrime: 'propAPrime', propA: 'propA', propB: 'propB' }
```

这大概是我能想到的做这件事最简单的方法了。即使一个类继承了另一个类(尽管我们都知道应该避免这种情况，对吗？).但是那里发生了很多事情。这不是公理。有粗糙的边缘。工厂的情况怎么样？

*工厂模式*

```
const aFactory = () => ({
  propA: 'propA'  
})const bFactory = () => ({
  propB: 'propB'
})const compositeFactory = () => ({
  ...aFactory(),
  ...bFactory()
})compositeFactory()
// { propA: 'propA', propB: 'propB' }
```

该死的。那很容易。

> 哦，但是等等！你说用类，它仍然可以和继承一起工作。所以即使这是应该避免的事情，它仍然是我们不能用普通物体做的事情…是吗？

*——一些持反对意见、目光敏锐的读者:*

坚持我的饮食胡椒博士…

```
const aPrimeFactory = () => ({
 propAPrime: 'propAPrime'
})

const aFactory = () => ({
  ...aPrimeFactory(),
  propA: 'propA'  
})const bFactory = () => ({
  propB: 'propB'
})const compositeFactory = () => ({
  ...aFactory(),
  ...bFactory()
})compositeFactory()
// { propAPrime: 'propAPrime', propA: 'propA', propB: 'propB' }
```

现在你们中的一些人会说:

![](img/271c3dd690eb0f28efac3e90579073cf.png)

Oh snap indeed. ([Image source](https://media.giphy.com/media/26AHLBZUC1n53ozi8/giphy.gif))

其他人会说:

![](img/82334221506d6ee9cf24a9c65fea1067.png)

That’s not inheritance, that’s just more composition. ([Image source](https://media.giphy.com/media/TbRwmI2fHg6ELPObHH/giphy.gif))

嗯，好吧，是的。我只是用了更多的构图。但是，呃，这就是遗传。功能上，只是线性构图。

好吧，小把戏，我承认。但是，为了完整起见，我将用简单的对象和工厂模式向您展示真正的、适当的继承。只是，请不要用。重点是避免继承。总有更好的办法。

```
// Never do this!!const superObjectFactory = () => ({
  superProp: 'superProp'  
})const grossInheritanceFactory = () => {
  const obj = ({
    subProp: 'subProp'
  })

  obj.__proto__ = superObjectFactory() return obj
}
```

啊！我觉得好恶心！🤢

## 类型检查

我们需要讲的最后一件事。当你深入到这一切的本质时，这就是 JavaScript 中使用类的真正原因。这并不是说它们启用了以前缺失的任何功能。而是它们在编程中启用了一种特殊的模式。类型检查。问题的关键是，到目前为止我给你展示的所有东西都是围绕着这样一个想法:一个对象 ***有*** 一个属性或者方法或者其他什么。我们把对象和它们的目的之间的这种关系称为一种 ***一种*** 关系。它基本上是面向功能的。

但是基于类的编程是面向对象 ***是*** 某种类型的想法。我们把一个对象和它的目的之间的这种关系叫做***is——一种*** 关系。它从根本上是以身份为导向的。

这两种思维方式之间实际上有一些重大的范式转变。其中最明显的是痴迷于类型检查和错误抛出的代码库:

```
if (obj instanceof SomeClass ) {
  // do something I know I can do with SomeClass
} else {
  //Crap! I don't know what to do! Uhhhhh
  throw new Error('obj is not and instance of SomeClass') 
}
```

现在我个人觉得这种编程很怪诞，过于复杂。引发错误——像其他形式的副作用一样——应该完全避免，或者尽可能地推到系统的边缘。将异常、错误抛出、类型和实例检查杂乱地放在代码库中，会使一切都难以遵循，并且对用户来说充满了地雷。快速失败对于开发来说很好，但是对于用户来说一点帮助都没有，而且从头到尾读完都是一场噩梦(实际上，任何分支逻辑都会使理解代码变得更加困难，但是那是另一篇文章的内容)。但是这就是现代面向对象编程的样子，所以如果你没有接受过编写它的训练，你几乎肯定会碰到它。

所以工厂模式的普通对象不允许实例检查，这是一个优点。有许多，[许多](https://en.wikipedia.org/wiki/Pure_function)模式避免了面向类编程产生的问题，面向类编程已经为这些问题创造了许多概念来纠正它们。我认为你对设计模式考虑得越多，你就越不会考虑手头的问题。在 JavaScript 中使用简单的函数和对象确实简化了事情，让您能够专注于您试图解决的实际问题。

为了好玩，让我们把类提供的所有好的特性都放在一个类中，并与工厂模式进行比较

*班级模式*

```
class SuperClass {
  superProp = '🦸‍♀️';

  constructor(name) {
   this.name = name;
  }; someSuperMethod() {
    console.log(`I'm ${this.name}!`)
  }

  someOverrideMethod() {
    console.log(`I get overridden`)
  }
}class FinalClass extends SuperClass {
  finalProp = '🪦';

  #privateProp = '🔐';
  #configProp = null; 

  constructor(configProp) {
    super('super'); 
    this.#configProp = configProp;
  };  

  someMethod() {
    console.log(`I'm only a member of FinalClass`);
  };

  someOverrideMethod() {
    console.log(`I override the super class method of the same name`);
  };

  get finalProp() {
    return this.#privateProp;
  };

  get configProp() {
    return this.#configProp;
  };

  set configProp(newVal) {
    this.#configProp = newVal;
  };  

}
```

*工厂模式*

```
const superFactory = (name) => ({
  superProp: '🦸‍♀️',

  someSuperMethod() {
    console.log(`I'm ${name}!`)
  },

  someOverrideMethod() {
    console.log(`I get overridden`)
  }
});const finalFactory = (configProp) => {
  const privateProp = '🔐';

  return {
    ...superFactory('super'), 
    finalProp: '🪦',

    someMethod() {
      console.log(`I'm only a member of the finalFactory object`);
    },

    someOverrideMethod() {
      console.log(`I override the method of the same name in superFactory`)
    },

    get finalProp() {
      return privateProp;
    },

    get configProp() {
      return configProp;
    },

    set configProp(newVal) {
      configProp = newVal;
    }
  }   
}
```

我认为在这两个例子中最重要的是要认识到工厂函数是多么的正常。这里没有不寻常的语法。在你的日常工作中，没有什么是你不会写几百遍的。这只是一个常规功能。您可能不太熟悉对象上的 getter/setter，但是它们早于类。JavaScript 类不需要做任何不同的事情来支持它。简而言之，一个返回对象的函数比类简单得多——而且能做的和类一样多。为什么不选择更简单的情况。至少这样你就可以有构图了。一些你不能从 JavaScript 类中得到的东西。

# 进一步阅读和推荐阅读

[设计模式:可重用面向对象软件的元素](https://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612/ref=sr_1_4?crid=1G1VPOL7OIZF0&dchild=1&keywords=design+patterns&qid=1618624417&sprefix=practical+microser%2Caps%2C244&sr=8-4) *作者:Erich Gamma、Richard Helm、Ralph Johnson 和 John Vlissides*

[物体构图](/javascript-scene/the-hidden-treasures-of-object-composition-60cd89480381)隐藏的宝藏*埃里克·艾略特*

[JavaScript 工厂函数与 ES6+](/javascript-scene/javascript-factory-functions-with-es6-4d224591a8b1) *作者 Eric Elliott*

[优雅的错误处理](https://jrsinclair.com/articles/2019/elegant-error-handling-with-the-js-either-monad/) *由 JR 辛克莱*

Alex Jover Morales 的 JavaScript *中的类组合——总的来说这是一篇很棒的文章，但在他热情地展示如何通过 JavaScript 类进行组合时，作者无意中展示了一个人必须经历的长度，看起来有点像你正在做，但实际上这只是非常非常深入的继承，看起来像组合。*