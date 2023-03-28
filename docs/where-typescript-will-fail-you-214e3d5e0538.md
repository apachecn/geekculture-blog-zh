# TypeScript 会让你失望的地方

> 原文：<https://medium.com/geekculture/where-typescript-will-fail-you-214e3d5e0538?source=collection_archive---------59----------------------->

![](img/e2cfec9e1ddd34c8bd91620a58d287f3.png)

Photo by [Gavin Allanwood](https://unsplash.com/@gavla?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

TypeScript 很棒。几年前，当我开始一个新项目时，我做了这个改变。从那以后我再也没有回头。我对我生成的代码更有信心，它将(大部分)按照我的预期工作。有时候，这种自信会反咬我一口。

今天，我给我们现有的一个`clearFilters`功能添加了一些功能。它看起来像这样:

```
const [activeFilters, setActiveFilters] = useState<Filter[]>(initialFilters)const clearFilters = () => {
   setActiveFilters([])
}
```

很简单。我们有一个过滤系统，这个可以重置过滤器。它用在我们的`Filters`组件中，也作为一个共享函数，所以我们可以在需要的时候清除`Filters`组件之外的过滤器。

在`Filters`中，看起来是这样的:

```
interface Props {
  filters: Filter[]
  clearFilters: () => void
}const Filters: React.FC<Props> = ({
  return (
    <div>
      ... the filters <button
        onClick={clearFilters}
      >
        Clear All
      </button>
    </div>
  )
})
```

同样，非常简单。单击`Clear All`按钮清除所有过滤器，这将触发对更高级组件中 API 的新查询。如果我想让它变得更复杂呢？我希望能够清除所有的过滤器**，除了我想保留的几个**。

太好了！让我们更新我们的`clearFilters`功能

```
const clearFilters = (except?: FilterType[]) => {
  if(except){
    setActiveFilters(activeFilters.filter((f) => except.includes(f.type))  
  } else {
    setActiveFilters([])
  }
}
```

我们已经更新了图书馆。一切编译。没有类型脚本错误！万岁！把它推到戳！

# 除...之外

![](img/87c035ed3c34f77ff5c6ac4270369c7c.png)

Photo by [Conor Samuel](https://unsplash.com/@csbphotography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> 每次我试图清除过滤器时，都会得到一个异常，说`except.includes`不是一个函数！什么什么？！

我想我真的用这个错误战胜了 TypeScript。原来，这是一个非常简单的错误。但是就像所有的错误一样，一旦你想通了，事情就简单了。

## 我的(简单？)错误

我们再来看看`Filters`。

```
interface Props {
  filters: Filter[]
  clearFilters: () => void
}const Filters: React.FC<Props> = ({
  return (
    <div>
      ... the filters <button
        onClick={clearFilters}
      >
        Clear All
      </button>
    </div>
  )
})
```

看到了吗？我的道具不再有效。我需要更新它们来反映新的`clearFilters`功能。

```
interface Props {
  filters: Filter[]
  clearFilters: (except?: FilterType[]) => void
}
```

一旦我这样做了，我的`onClick={clearFilters}`行将抛出一个 TypeScript 编译错误。类型不匹配！**将 onClick 设置为不带任何参数的函数意味着来自单击的鼠标事件将被传递给该函数。**

为了解决这个问题，我们做了另一个小小的更新:

```
<button onClick={() => clearFilters()}>All Clear</button>
```

现在，我们实际上都是在明确的，一切都在工作，因为它应该！

## 经验教训

1.  **将函数作为道具传递时，更新函数时更新类型**

TypeScript 很棒，但是它只会做我让它做的事情。如果我直接导入了`clearFilters`函数，这里就不会有问题了。因为我把它作为参数传入，所以我需要确保 prop 类型与我传入的函数相匹配。

2.**编写自动化测试**

在这种情况下，我的错误被构建服务器上的 Cypress 测试捕获。如果不是这样，很可能这些代码已经发布到产品中，而没有人意识到它被破坏了。这些自动化测试防止了大麻烦，尤其是在周五晚些时候的部署之前。

希望我的错误能让你在将来免于一些！在 TypeScript 中，您在哪里遇到过与您预期不同的事情？一路走来，你学到了什么教训？