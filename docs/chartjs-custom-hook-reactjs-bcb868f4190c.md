# ChartJs 自定义挂钩— ReactJs

> 原文：<https://medium.com/geekculture/chartjs-custom-hook-reactjs-bcb868f4190c?source=collection_archive---------17----------------------->

![](img/d53c12d00603e15b1cf57ef77ca2ab96.png)

Photo by [Steve Johnson](https://unsplash.com/@steve_j?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/hooks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

一个**自定义钩子**什么都不是，一个简单的带有**共享** **逻辑**的 javascript 文件。在这里，我们将编写一个自定义钩子，通过使用 [**ChartJs**](https://www.chartjs.org/docs/latest/) 库来呈现图表。

使用**纱线/npm** 命令安装**图表**

```
yarn add chart.js
or
npm install chart.js
```

# 让我们写一个钩子—使用图

钩子名称应该总是以 **use** 开头，这里我们将钩子命名为 **useChart** 。创建一个新文件 **useChart.js** 并添加以下代码片段

```
import React from "react";
import Chart from "chart.js/auto";const useChart = (nodeRef, options) => { React.useEffect(() => { const chart = Chart.getChart(nodeRef.current.id); if (chart) { chart.destroy(); } new Chart(nodeRef.current, options); console.log("chart rendered");}, [nodeRef, options]); return {};};export default useChart;
```

在上面的代码片段中， **useChart** 函数接受两个参数**ref**chart**options**。Ref 用于将**图表** **画布**传递给钩子&图表**选项**包括图表的**类型**和**数据**值。

# 现在，让我们看看如何调用 useChart 挂钩

在您的组件中，要呈现图表，请导入 **useChart** **钩子**并使用 React 的 **useRef** 创建一个 **ref** 。

```
import useChart from “./useChart”;
*.
.
.*function App() {const canvasRef = useRef(null);
*.
.
.*
}
```

**charts jss**使用 **DOM** 来渲染图表，所以我们要创建一个 **canvas** 元素并传递 **ref** 。

```
<canvas id=”weatherChart” ref={canvasRef} width=”400" height=”100" />
```

最后，用 **useChart** 钩子通过传递 **ref** & **图表选项来渲染**图表**。**

```
useChart(canvasRef, {type: "line", //type of chart (line, pie, bar)
data: {
labels: data.label,   //chart label eg: [SUN, MON, TUE]
datasets: [
{
label: "Temperature °C",
data: data.values,   //chart data eg: [10,50,11]backgroundColor: "rgba(255, 99, 132, 0.2)",
borderColor: "rgba(255, 99, 132, 1)",
borderWidth: 1,
  },
 ],
},
options: {
legend: {
display: true,
labels: {
 fontColor: "#ff0000",
},
},
scales: {
 yAxes: [
 {
  ticks: {
   beginAtZero: true,
    },
   },
  ],
  },
 },
});
```

并且我们已经成功创建了一个**自定义 chartJs 钩子**并渲染了图表。您可以调用**多个使用图表挂钩**来呈现不同类型的图表。
我已经用*[*https://api.weatherapi.com*](https://api.weatherapi.com)*来填充**数据** *。***

***代码回购—*[*https://github . com/RasikFareed/custom-hooks/tree/main/custom-hooks*](https://github.com/RasikFareed/custom-hooks/tree/main/custom-hooks)**

***试玩链接—*[*https://rasikfareed.github.io/custom-hooks/*](https://rasikfareed.github.io/custom-hooks/)**

# **摘要**

**总而言之，React 定制钩子更强大，并且使代码更少&更容易使用。**

**在本文中，我们为 ChartsJs 库构建了自己的定制挂钩，并呈现了图表**

**感谢阅读，编码快乐！**

**参考文献
1。[https://www.chartjs.org/docs/latest/](https://www.chartjs.org/docs/latest/)2
。【https://reactjs.org/docs/hooks-custom.html **