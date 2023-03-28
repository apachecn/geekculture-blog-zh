# 原生反应:部署一次，无限运输

> 原文：<https://medium.com/geekculture/to-react-native-306e399dbb2?source=collection_archive---------68----------------------->

![](img/8d10ffa81a68b3dbbf9efa3bc732fb97.png)

React Native 是一个用于在 React 中构建本机、iOS 和 Android 应用程序的框架。与许多基于应用程序的语言一样，React Native 在视图方面进行了移动旋转，并将 DOM 扩展为移动状态。

Expo 是一个框架，用于建立 React 本地应用程序，启动、测试和捆绑这些应用程序，以便通过网络提供给用户。从我记事起，Expo 就一直是展示开发功能的领先解决方案，具有快速托管和可视的 QR 已建立环境。

[](https://reactnative.dev/docs/environment-setup) [## 设置开发环境

### 该页面将帮助您安装和构建您的第一个 React 本机应用程序。如果您是移动开发的新手，那么…

反应性发展](https://reactnative.dev/docs/environment-setup) 

## 对本地目标和模式做出反应

![](img/0cff76ab4a2e86f243714361e2c45b34.png)

简单地说，我们的目标是使用 React Native 和 Expo 将一个有效的 React X Next web 应用程序迁移到移动环境中。从基于 web 的对象到基于移动的对象的交换是最简单的变化。

[](https://docs.expo.io/distribution/building-standalone-apps/) [## 构建独立应用程序—世博会文档

### 本指南的目的是帮助您为 iOS 和 Android 创建 Expo 应用程序的独立二进制文件，它们可以是…

docs.expo.io](https://docs.expo.io/distribution/building-standalone-apps/) 

当 Div 变成视图时，safeareaviewing 变成容器，这些元素的编排组合在一起就创建了一个非常熟悉的模式来进行响应性聚焦开发。参考文献是上一篇文章中的 [React 应用程序，将该解决方案的范围扩展到新的方式是手头的想法。](https://collectedview.medium.com/minimum-viable-application-33a245fff225)

# 解决 React Native 中的挑战

![](img/2c627ef6ea1ddbf04c5718308da59cb8.png)

P 在应用程序的初始架构中，如果没有 UI 库来支持快速迭代和样式化，React 原生应用程序的功能就成了眼前的挑战。

使用 React 挂钩、效果、事件和函数进行扩展——构建所有核心功能需要通过状态、库和定制解决方案将 React 本地组件关联起来。

[](https://reactnative.dev/docs/handling-text-input) [## 处理文本输入反应自然

### TextInput 是允许用户输入文本的核心组件。它有一个 onChangeText 属性，该属性接受一个函数来…

反应性发展](https://reactnative.dev/docs/handling-text-input) 

这个案例研究中最大的不同是基于 HTML 的输入的交换 React 本地“TextInput”组件的默认值。基于 DOM 的输入设置了格式化类型，React Native 设置了输入类型的键盘。

最初这是一个挑战，表单输入的流动性和处理 jQuery 的 DOM 的缺乏意味着重新编写整个基于状态的应用程序。在 MVP 和简单性的理念中，这是以前没有开发过的&为应用程序输入本身如何处理输入创造了新的途径。

[](https://collectedview.medium.com/minimum-viable-application-33a245fff225) [## 最小可行应用程序(MVP):计划的成功和用户

### 你有一个想法，一项创新，一项发明——创造东西是你的目标，接触用户是方法，还有…

collectedview.medium.com](https://collectedview.medium.com/minimum-viable-application-33a245fff225) 

## 时间值格式的用户本机输入

![](img/0a4f9ace062380f5eeb7a68ba05b4b57.png)

当格式化输入成为输入本身被特别设置的解决方案的核心时，它成为一个有趣的焦点。如前所述，[<input type = " time ">](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/time)对于 web 来说是相当方便的，尽管没有它，功能会发生变化。

在首先，头脑会用替代方案复制手头的解决方案，以模拟体验并简化结构。有几个库实现了基于时间的输入，工作方式类似，但失去了功能，没有反应网络视图。

[](https://github.com/antonfisher/react-simple-timefield) [## Anton fisher/react-simple-time field

### 简单的反应时间输入字段，看看演示。NPM install-save React-simple-time field # for React<16 use: npm…

github.com](https://github.com/antonfisher/react-simple-timefield) 

Additionally, there are solutions that extend the input in a time format, though use scroll and date picker type functionality that is more or less arduous when inputting multiple time variables, in a repetitive cycle for a result.

[](https://github.com/react-native-datetimepicker/datetimepicker) [## react-native-datetimepicker/datetimepicker

### See this issue This repository was moved out of the react native community GH organization, in accordance to this…

github.com](https://github.com/react-native-datetimepicker/datetimepicker) 

## Event Driven Procedures with React Native

![](img/c6f6895fdc90b2e2820af25ef4230d93.png)

What became useful, upon reaching this point — were the event driven handlers that are outlined in the React Native documentation. *文档有助于导航基本的，直到编排晦涩的，这里是一个主要的例子。*

[](https://reactnative.dev/docs/button) [## 按钮反应本机

### 一个基本的按钮组件，应该可以在任何平台上很好地呈现。支持最低级别的自定义。如果这个…

反应性发展](https://reactnative.dev/docs/button) 

```
<Button
    title="Left button"
    onPress={() => Alert.alert('Left button pressed')}
 />
```

在onPress 事件中，您可以处理作为处理程序构建的自定义函数，在这种意义上，将两个输入的状态计算为总和。接下来的逻辑类似于现有的 React 应用程序，不过使用状态和简单的对数来输出总和。

```
<TextInput                            
    keyboardType="numeric"
    onChangeText={(text) => setTime(+text)}                                     />
```

文本的<textinputs>的 [onChange](https://reactnative.dev/docs/textinput) 设置状态中的入口数据点，以存储算法的数据，然后由 onPress 事件处理，并在<文本>组件中作为有状态文本重复。</textinputs>

> 函数 calculate total(){ settime total((time three—time one)* 100+(time four—time two))/100)；}

自定义函数处理所有存储的状态，并设置新版本的状态来输出这些数据。与[初始应用](https://collectedview.medium.com/minimum-viable-application-33a245fff225)一样，这些解决方案可以扩展到持续更新存储中的状态总数，或者向作为用户存储的数据库发送 API POST 请求。

[](https://github.com/collectedview/react-native-hourly-time-calculator) [## collected view/react-native-hour-time-计算器

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/collectedview/react-native-hourly-time-calculator) [](https://expo.io/@collectedview/react-native-hourly-time-calculator) [## 世博会

### Expo 是一个开源平台，使用 JavaScript 和……为 Android、iOS 和 web 开发通用的本地应用程序

世博会](https://expo.io/@collectedview/react-native-hourly-time-calculator) 

# 感谢阅读，继续订阅！

寻找更多的应用程序开发建议？在[推特](https://twitter.com/collectedview)、 [GitHub](https://github.com/collectedview) 和 [LinkedIn](https://www.linkedin.com/in/collectedview) 上关注。在 [collectedview.io](https://collectedview.io/) 上在线访问最新更新、新闻和信息。