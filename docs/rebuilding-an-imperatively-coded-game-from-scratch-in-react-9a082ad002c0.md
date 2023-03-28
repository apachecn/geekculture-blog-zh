# 在 React 中从头开始重建强制编码的游戏

> 原文：<https://medium.com/geekculture/rebuilding-an-imperatively-coded-game-from-scratch-in-react-9a082ad002c0?source=collection_archive---------26----------------------->

![](img/6769860e7c1e44c0612802e2efea5610.png)

The Guessing Game ([https://shapirodaniel.github.io/guessing-game-react/](https://shapirodaniel.github.io/guessing-game-react/))

*我们将重新打造的游戏！* [*猜谜游戏，命令式*](https://shapirodaniel.github.io/guessing-game-imperative/)

*重构后的基于 React 的游戏！* [*猜谜游戏，功能性+反应性*](https://shapirodaniel.github.io/guessing-game-react/)

*Github repos for both:*[*命令式代码库*](https://github.com/shapirodaniel/guessing-game-imperative) *|* [*功能性+反应式代码库*](https://github.com/shapirodaniel/guessing-game-react)

如果你和我一样，你至少有一个有趣的副业，当你还是个新手的时候，就已经写好了。当我 2020 年加入 Fullstack Academy 时，我刚刚为面包师傅编写了一个食谱管理工具(查看一下！面包师的朋友)，而且对框架一无所知，我用我所有的新的命令式技巧构建了我的下一个 web app——一个在 1 到 100 之间的 10 x 10 数字网格上玩的猜谜游戏。

虽然游戏逻辑肯定可以重构，但游戏机制——你可以作为对用户交互的响应而锁定的一切——这是一个— *插入太阳镜迷因—* 反应框架的真正好处将会显现出来。

**告诉我你想要什么(你真的真的想要什么)**

命令式编程的困难之处在于，您必须详尽地记录实现某个效果所需的步骤。React 通过允许您描述*您希望发生什么*，而不是*您希望它如何发生*，释放了精神资源——所以让我们将 DOM 的微观管理转向 React，以便我们可以专注于更高层次的问题，如游戏逻辑和多个相关 DOM 节点或*组件的交互*。

但是在我们开始 React 之前，让我们先来看看这个游戏场的命令式版本。尽管 React 允许我们使用 JSX 并编写 HTML、CSS 和 Javascript 的混合代码，但命令式代码库使标记在很大程度上与控制器(JS)分离。

首先，我们构建一个“奇数行”模板，用数字填充它。(也有一个“偶数行”模板，其中的`darkSquare, lightSquare`类是交替的，我们将把它留给想象……)

Direct link to gist (with more reasonable spacing!): [https://tinyurl.com/rowtemplate](https://tinyurl.com/rowtemplate)

接下来，我们指定一个容纳十行的游戏场容器。如果你曾经使用过 React，这种策略反映了框架从顶级容器呈现应用程序的方法，通常被标识为*根*或*应用程序*。

Direct link to gist: [https://tinyurl.com/imperativeplayingfield](https://tinyurl.com/imperativeplayingfield)

最后，在我们的游戏类中，我们调用`buildPlayingField()`来构建网格，调用`assignNodeVals()`来给每个网格分配一个从 1 到 100 的数字。

Direct link to gist (with more reasonable spacing!): [https://tinyurl.com/buildplayingfield](https://tinyurl.com/buildplayingfield)

将我们强制性编码的游戏场转换成面向功能的 React 组件，我们可能会得到类似这样的东西——首先，一个名为`Row`的子组件呈现棋盘模式的单行。

Direct link to gist (with more reasonable spacing!): [https://tinyurl.com/reactrow](https://tinyurl.com/react-row-impl)

接下来，我们创建一个`PlayingField`组件，它将映射一个“标量”列表——这些是我们希望每行从其开始的数字——并将标量传递给`Row`子组件。

Direct link to gist (with more reasonable spacing!): [https://tinyurl.com/react-playingfield](https://tinyurl.com/react-playingfield)

编写函数式 React 组件而不是命令式代码的好处在于它们固有的可维护性和可扩展性——简洁不是主要问题，因为我们编写了更多的代码来完成相同的任务。在我们的 React 游戏领域中，棋盘的全部逻辑都包含在一个函数中，我们没有详尽地记录应用程序应该如何构建 DOM，而是描述我们希望看到的内容，将其余的留给 React。

> **国家是巨大的发动机，缓慢地运转着。(弗朗西斯·培根)**

React 在状态管理方面真的大放异彩——跟踪用户当前经历的“情况”的所有值 Bacon 的格言让我们知道为什么:状态是一个庞大的事物，从用户加载我们的应用程序的那一刻起，它就向许多方向爬行和分支。报道州内可能出现的每一个案件就相当于旋转盘子的把戏——为了避免一堆碎陶瓷，每个事件都必须精确地按顺序处理。

如果没有状态管理系统，命令式状态管理会很快*分离*:以这个处理所有点击事件及其相关逻辑的单片功能`clickHandler`为例。

Direct link to gist (with more reasonable spacing!): [https://tinyurl.com/react-clickhandler](https://tinyurl.com/react-clickhandler)

如果我们有办法将**游戏逻辑**从 **DOM 元素本身**中分离出来，这样我们就可以分别对它们进行推理，会怎么样？如果以这种方式分离关注点，我们会得到什么好处呢？

让我们通过提取状态逻辑来重构`clickHandler`，将机械信息(“当用户点击 X，Y 发生”)发送给反应组件，并让它们接收来自单一真实来源的状态更新。React 生态系统有几个可用的状态管理系统:考虑到我们适度的需求，React 的上下文 API 非常合适。

**用 React 上下文 API + useReducer 管理状态**

React 上下文是提供者组件，它包装 React 的部分(或全部)虚拟 DOM，并使全局状态上下文对上下文提供者下游的所有子元素可用，这些子元素通过 React 的`useContext`钩子访问上下文提供的值。

我们已经看到，我们的状态逻辑经常涉及几个不同的 DOM 元素同时改变。例如，当用户请求提示时:

*   生成提示并分配类来反映它们
*   用户的剩余猜测减少
*   代表用户选择的活动区块被重置
*   如果提示由于剩余猜测次数少或难度高而不可用，则触发与提示相关的条件逻辑

React 允许我们颠倒这种命令式的数据流模型，将状态传递给元素，而不是在 DOM 中操纵状态。让我们利用 React 的`useReducer`钩子，通过向一个 reducer 发出**动作**来修改我们的全局状态，这个 reducer 只是一个函数，它返回一个更新的状态，给出一个动作类型和一个状态修改的有效负载。

我们的应用程序让用户采取五个不同的行动:设置难度，选择一个方块，提交选择的方块作为猜测，获得提示，重置游戏。如果我们限制用户在游戏中改变难度等级的能力，那么我们实际上只有四个交互。

Direct link to gist: [https://tinyurl.com/react-reducer-actions](https://tinyurl.com/react-reducer-actions)

我们的 reducer 将接受当前状态，初始化如下(其中以 ***Lib** 结尾的对象是保存映射到难度、进度和玩家消息的变量的字符串库，以及保存大量提示和猜测的数字库):

Direct link to gist: [https://tinyurl.com/react-reducer-initstate](https://tinyurl.com/react-reducer-initstate)

每当用户采取动作时，一个对象被*分派*或发送给 reducing 函数，后者将打开动作类型——这是一个由 DOM 元素转发给 reducer 的消息，DOM 元素捕获了由分派的消息描述的用户动作。有了我们的动作类型，让我们把所有的部分放在一起。

Direct link to gist (with more reasonable spacing!): [https://tinyurl.com/react-reducer](https://tinyurl.com/react-reducer)

我们将用来把`state`和`dispatch`方法传递给下游组件的提供者组件依赖于提供者包装它的子树，我们允许它用`props.children.`来做这件事

Direct link to gist (with more reasonable spacing!): [https://tinyurl.com/react-context-provider](https://tinyurl.com/react-context-provider)

现在，我们将组件及其子组件包装在一个`GameProvider`中，无论我们将提供者放在什么级别，所有的子组件都可以访问它的`GameContext`，而**提供了对`state`和`dispatch`的**访问——我们将使用这些来修改实际执行该工作的组件内部的状态！(把`useReducer's` `state`对象和`dispatch`方法看作是`useState`更健壮的版本)。

Direct link to gist (with more reasonable spacing!): [https://tinyurl.com/react-provider-wrapped-app](https://tinyurl.com/react-provider-wrapped-app)

我们回到了我们的`PlayingField`，在这里我们将看到`Row`子组件现在渲染游戏状态，而不知道*如何*或什么导致状态改变——它只是渲染新的状态——并且它不是通过道具训练，而是直接在`GameContext`上访问状态。

> 顺便说一下，我们可以放心地将 useReducer 的`dispatch` 方法从 React 上下文中取出，因为每当提供者本身发生变化时，都会触发提供者子树的呈现:
> 
> " React 保证`dispatch`函数身份是稳定的，不会在重新渲染时改变."——[https://reactjs.org/docs/hooks-reference.html#usereducer](https://reactjs.org/docs/hooks-reference.html#usereducer)

Direct link to gist (with more reasonable spacing!): https://tinyurl.com/reactrow

`PlayerButtons`组件展示了我们从分离关注点中获得的清晰性:按钮分派动作，而不需要知道那些动作将如何传递新状态。

Direct link to gist (with more reasonable spacing!): [https://tinyurl.com/react-playerbtns](https://tinyurl.com/react-playerbtns)

我们可以很容易地将`START_GAME`逻辑扩展到我们的`DifficultySwitches`，允许我们在没有任何额外逻辑的情况下阻止用户在游戏中重置游戏难度！我们可以访问我们的`GameContext`，这将让我们与组件共享 ***Lib** 常量库，减少重复定义的需要。

Direct link to gist (with more reasonable spacing!): [https://tinyurl.com/react-difficulty-switches](https://tinyurl.com/react-difficulty-switches)

**外卖**

React 是一个强大的前端框架，它直接内置了一些很棒的状态管理 API，特别是它的`useReducer`钩子和`useContext`钩子，允许向你的整个应用程序提供一个复杂的类似 Redux 的单一事实来源，一个特定的特性，或者一个完全本地的环境，用于管理你的应用程序的其余部分可以愉快地忽略的小块状态。

虽然 Redux 可能有些过头，但 React 的上下文系统是一种低调、灵活的替代方案，特别适合小空间中的大状态——就像本教程使用 React 自然的面向功能的设计对强制编码的猜谜游戏进行的演练重构。感谢阅读，并享受使用 React 和 React Context API + useReducer 构建有状态应用程序的乐趣！

Daniel Shapiro 是一名全栈软件工程师，毕业于全栈学院，目前是该学院的助教。当他不制作酷的东西时，你通常会发现他沉迷于他以前的职业生涯，在芝加哥的许多工匠面包店担任首席面包师，烘焙一批英式松饼，带着他的小猎犬百合在沙滩上跑步，或者在密歇根湖划船。联系 shapirodanieladam@gmail.com 的丹尼尔或者连接 LinkedIn 上的[](http://linkedin.com/in/shapirodanieladam)**，一定要访问* [*面包师的朋友*](http://breadbakersfriend.com) *，这是一个专为业余和专业面包师设计的网络应用程序！**