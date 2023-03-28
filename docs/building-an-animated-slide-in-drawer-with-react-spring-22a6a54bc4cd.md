# 用反作用力弹簧制作一个动画滑入式抽屉

> 原文：<https://medium.com/geekculture/building-an-animated-slide-in-drawer-with-react-spring-22a6a54bc4cd?source=collection_archive---------10----------------------->

![](img/12937cf7ad67020fb6820ccbf47fb7e1.png)

Photo by [Monstera](https://www.pexels.com/@gabby-k?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/wood-pattern-mailbox-row-7794456/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

# 目标

作为一名开发人员，我喜欢简单的解决方案，当我在寻找构建它们的步骤时，我觉得像抽屉这样的常见 UI 模式从来没有向我传达过“哇，这很容易”的信息。此外，安装一个有预建解决方案的 npm 包是很好的，但有时，这比它的价值更麻烦，最好只是构建自己的东西。如果这些痛点中的任何一个适用于你，那么希望这篇文章能帮助你。

这样做的目的是通过使用 [react-spring](https://react-spring.io/hooks/use-spring) 动画库，在 react 中构建一个简单而优雅的动画滑入式抽屉。我将介绍几行代码，您将需要这些代码来使滑入功能正常工作，到此结束时，您应该有一个功能完整的动画抽屉组件，您可以将它插入到任何 react 项目中，为您的 web 应用程序添加一点趣味🌶。

# TLDR

如果您不想阅读代码块解释，而是只想打开代码沙箱来查看最终产品，请一直滚动到底部🙂

# 代码块

## 触发器

```
<button className="openButton" onClick={handleToggleDrawer}>{isDrawerShowing ? "Close" : "Open"}</button>
```

我们将用来打开和关闭抽屉的触发器是一个简单的按钮。我们将使用`isDrawerShowing`状态变量来切换按钮文本。当按钮被点击时，所有的`handleToggleDrawer`功能就像它的名字一样，在真和假之间切换`isDrawerShowing`状态。

## 抽屉组件

```
<Drawer show={isDrawerShowing} />
```

抽屉组件接受一个名为`show`的道具，它的值再次是`isDrawerShowing`状态变量。我们将使用这个布尔值让`react-spring`知道何时重新渲染动画。我可以把抽屉的代码和按钮的代码放在同一个文件里，但是我认为如果每样东西都有自己的家，阅读和理解起来会更简单。写干净代码的人！

## 动画道具

```
const props = useSpring({left: show ? window.innerWidth - 300 : window.innerWidth,position: "absolute",top: 0,backgroundColor: "#806290",height: "100vh",width: "300px"});
```

这是魔法真正发生的地方。我们使用来自`react-spring`的`useSpring`钩子，但是他们有各种各样的其他选项，允许你实现不同的动画效果。通读他们的文档，看看这个库还能做些什么！

因此，大多数 CSS 属性是静态的，我们实际上用来更新动画的唯一值是`left`属性。当我们传入的`show`属性是`true`时，我们让我们的抽屉从屏幕右侧滑出 300 像素。当它是`false`时，我们希望 left 的值是`window.innerWidth`，这基本上意味着抽屉将隐藏在浏览器屏幕右侧的右侧。(看起来比写起来容易，所以只要玩玩代码沙箱，你就能明白我的意思了🙃)

注意:如果你想知道我在`left`属性中看到的`— 300`是从哪里来的，那是因为我把抽屉的宽度指定为`300px`。你不需要硬编码这个值，我只是为了简单起见。您可以将宽度设为屏幕总可用宽度的一个百分比，这样可以更具动态性。这真的取决于您的用例以及您正在构建的内容。

## 动画 div

```
<animated.div style={props}><div className="drawer">Animated Drawer!</div></animated.div>
```

最后一步是将上面定义的道具传递到动画视图中，在本例中是`animated.div`，它将常规的`div`元素转化为能够接收和理解`react-spring`提供的动画属性的元素

# 成品！

Ooo ahhh 😍

# 结论

这是一个关于如何使用`react-spring.`创建滑入式抽屉的超级简单的例子，你应该能够使用这个代码作为起点，并扩展它以获得更复杂的功能，并真正使抽屉在你的个人项目和网络应用中变得生动。我希望你喜欢，甚至可能学到了一些东西！

如果你最终尝试了`react-spring`并创造了很酷的动画，我很想看看它们！你可以在评论中给我链接你的代码沙箱，或者发邮件给我，地址是*atriana@protonmail.com。*