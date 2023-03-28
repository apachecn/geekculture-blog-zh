# 如何使用“react-native-chart-kit”在 React Native 中制作动态图表

> 原文：<https://medium.com/geekculture/how-to-make-dynamic-charts-in-react-native-using-react-native-chart-kit-48e9e5f41485?source=collection_archive---------4----------------------->

图表是数据可视化的图形表示，其中“数据由符号表示，如条形图中的条、折线图中的线或饼图中的切片”。

如今，不同的移动和网络应用程序都以图表的形式呈现数据。图表可读性强，并且用漂亮的图形向用户传达信息。应用程序中的图表还有几个其他的用例。

让我们进入主题，我看到许多程序员正在努力在他们的应用程序中嵌入动态图表，所以在这里我将通过使用库" **react-native-chart-kit** "来分享动态图表的代码配方，这是 react native 中最受欢迎的图表库之一。我正在分享“*折线图”*的信息，您可以相应地将其用于不同的图表，但请记住每个图表有不同的输入，例如折线图与饼图相比有不同的输入。 ***数据*** ***变量*** 从库中负责取输入。

本库链接:
[*https://www.npmjs.com/package/react-native-chart-kit*](https://www.npmjs.com/package/react-native-chart-kit)

**先决条件:**

1.  *在您的机器上建立 react-native 环境*

*2。对 react-native 的基本了解*

**步骤 1:** 通过 npm 安装 library — react-native-chart-kit

在终端或 cmd 上运行以下命令进行安装。

```
npm i react-native-chart-kit
```

**第二步:**导入库— react-native-chart-kit

```
import {
 LineChart,
  BarChart,
  PieChart,
  ProgressChart,
  ContributionGraph,
  StackedBarChart
} from "react-native-chart-kit";
```

**第三步:**制作一个状态—数组来存储值

```
state = {
datasource:[]
  };
```

**第四步:**创建一个函数，从数据库中获取动态数据。这个函数将负责从数据库的响应变量中获取信息，并将数据放入数据源数组。

**注**:在这里你必须使用你自己的信息来访问数据，我展示这些信息只是为了便于理解。

```
//fetch your own data from here get_chart=()=>{
        //change the url for fetching your data 
              fetch('http://192.168.18.7:8000/api/linechart', {
                method: 'GET',
                headers: {
                  Accept: 'application/json',
                  'Content-Type': 'application/json',
                },

              })
                .then(response => response.json())

                .then(response => {
                 this.setState({datasource:response})

                })
                .catch(error => {

                });

}
```

**第五步:**为动态折线图或任何其他你想使用的图表制作一个函数。

```
LineChart_Dynamic=()=>{

if (this.state.datasource){
if(this.state.datasource.length){

return(
<View>
  <Text>Bezier Line Chart</Text>
  <LineChart
    data={{
//this is x-axis data labels: ["January", "February", "March", "April", "May", "June"], datasets: [
        {
          data:  this.state.datasource.map(item=>{
            return(
//this is y-axis item.students /*you need to add your data here from JSON, and remember the data you are requesting should be integer because it is line chart*/.
            )
          })

        }
      ]
    }}
    width={Dimensions.get("window").width} // from react-native
    height={220}
    yAxisLabel="students"
    yAxisSuffix="k"
    yAxisInterval={1} // optional, defaults to 1
    chartConfig={{
      backgroundColor: "#e26a00",
      backgroundGradientFrom: "#fb8c00",
      backgroundGradientTo: "#ffa726",
      decimalPlaces: 2, // optional, defaults to 2dp
      color: (opacity = 1) => `rgba(255, 255, 255, ${opacity})`,
      labelColor: (opacity = 1) => `rgba(255, 255, 255, ${opacity})`,
      style: {
        borderRadius: 16
      },
      propsForDots: {
        r: "6",
        strokeWidth: "2",
        stroke: "#ffa726"
      }
    }}
    bezier
    style={{
      marginVertical: 8,
      borderRadius: 16
    }}
  />
</View>
)

} }} 
```

当 get_chart()函数获取数据时，您还可以添加额外的东西向用户显示。

**完整代码**

```
import React, { Component } from "react";
import {
  Alert,
  StyleSheet,
  Text,
  View,
  ActivityIndicator,
  Dimensions,

} from "react-native";

import {
 LineChart,
  BarChart,
  PieChart,
  ProgressChart,
  ContributionGraph,
  StackedBarChart
} from "react-native-chart-kit";

class Linedchart extends Component {
  state = {
datasource:[]
  };

LineChart_Dynamic=()=>{

if (this.state.datasource){
if(this.state.datasource.length){

return(
<View>
  <Text>Bezier Line Chart</Text>
  <LineChart
    data={{
      labels: ["January", "February", "March", "April", "May", "June"],
      datasets: [
        {
          data:  this.state.datasource.map(item=>{
            return(
              item.students 
//you need to add your data here from JSON, and remember the data you are requesting should be integer.
            )
          })

        }
      ]
    }}
    width={Dimensions.get("window").width} // from react-native
    height={220}
    yAxisLabel="students"
    yAxisSuffix="k"
    yAxisInterval={1} // optional, defaults to 1
    chartConfig={{
      backgroundColor: "#e26a00",
      backgroundGradientFrom: "#fb8c00",
      backgroundGradientTo: "#ffa726",
      decimalPlaces: 2, // optional, defaults to 2dp
      color: (opacity = 1) => `rgba(255, 255, 255, ${opacity})`,
      labelColor: (opacity = 1) => `rgba(255, 255, 255, ${opacity})`,
      style: {
        borderRadius: 16
      },
      propsForDots: {
        r: "6",
        strokeWidth: "2",
        stroke: "#ffa726"
      }
    }}
    bezier
    style={{
      marginVertical: 8,
      borderRadius: 16
    }}
  />
</View>
)
} else {
return(

<View style={{justifyContent:"center",alignItems:'center',flex:1}}>

<ActivityIndicator size="large"/>

</View>

)
  }

}else {

  return(

  <View style={{justifyContent:"center",alignItems:'center',flex:1}}>

 <Text>no data found</Text>

  </View>
  )}

  }

//***************************************************************//fetch your own data from here

 get_chart=()=>{

              fetch('http://192.168.18.7:8000/api/linechart', {
                method: 'GET',
                headers: {
                  Accept: 'application/json',
                  'Content-Type': 'application/json',
                },

              })
                .then(response => response.json())

                .then(response => {
                 this.setState({datasource:response})

                })
                .catch(error => {

                });
}

/*componentDidMount will execute the function when the screen is mounted.*/ componentDidMount=()=>{
    this.get_chart()
  }

  render() {

return(
<View>

{this.LineChart_Dynamic()}

</View>

)}}

export default Linedchart;
```