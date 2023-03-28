# 用 Cube.js、BigQuery 和 Chart.js 可视化航空公司数据

> 原文：<https://medium.com/geekculture/visualizing-airline-data-with-cube-js-bigquery-and-chart-js-8866a09972a2?source=collection_archive---------6----------------------->

![](img/c6532821264e7e20965c1c17fc9e0645.png)

可视化或表示您的数据是添加到项目优先级列表中的一个非常重要的项目，因为我们可以更清晰地了解图形、图表等形式的信息，有了它，不仅可以帮助我们解释大块数据，还可以帮助我们分析数据的趋势和模式。

因为权力越大，责任越大！我们将通过利用一些惊人的工具使我们的生活变得更容易，这些工具可以将我们的开发时间减少到仅仅几分钟。

在本教程中，我们将学习:

*   我们如何在 Google 的 BigQuery 上设置我们自己的自定义数据，而不需要任何额外的成本
*   了解如何连接 Cube.js，为我们提供 BigQuery 和我们的前端应用程序之间的后端支持
*   添加 Chart.js 作为依赖项，在 Web 前端构建漂亮的可视化效果

# 什么是 Google BigQuery？

BigQuery 是一个企业数据仓库，它通过使用 Google 基础设施的处理能力实现超快速的 SQL 查询来解决这个问题。它是无服务器的，意味着存储数据更便宜，扩展数据更快。BigQuery 能够以很低的成本快速处理大量 T2 数据。除此之外，它还具有内置的集成，使得在 BigQuery 中构建一个**数据湖**变得简单、快速且经济高效。

集中您的数据，以允许与谷歌云的机器学习工具自动集成，获得高级数据科学报告。它还与 Data Studio 一键式集成，这意味着可视化处理后的表既简单又快速。像 DataFlow 和 DataProc 这样的 ETL 解决方案减少了数据转换的开销。

BigQuery 还有一个免费使用层:在最初的 90 天里，你每月可以获得高达 1 TB 的已处理数据和一些免费积分，用于在谷歌云上消费。

# 在 BigQuery 上设置航空公司分析数据

尽管 BigQuery 为我们提供了大量免费的公共数据集，对谷歌云用户没有任何限制，但在本教程中，我们将使用我们自己的数据集。

要开始，请访问[谷歌云控制台](https://console.cloud.google.com/)，如果您还没有帐户，请创建一个帐户。创建帐户后，访问[新项目创建页面](https://console.cloud.google.com/projectcreate)并创建一个新项目，在该项目中我们将拥有航空公司分析数据集。

完成基本设置后，建议[创建一个服务帐户](https://console.cloud.google.com/iam-admin/serviceaccounts/create)来限制您的 API 运行工作负载和添加验证的权限。创建服务帐户时，您需要为其指定角色，对数据集进行读访问所需的两个角色是`BigQuery Data Viewer`和`BigQuery Job User`。完成后，最后一步是向这个新创建的用户添加一个身份验证密钥。点击右侧`Actions`下的`...`按钮，选择`Manage keys`。从这个部分添加一个新的 JSON 键，它会自动下载到您的本地系统中。一定要保管好，不要弄丢了。

现在您已经完成了创建和读取 BigQuery 数据集所需的所有配置，是时候从[这里](https://tmpfiles.org/7359/airlines.csv)实际下载航空公司数据集了。

下载完成后，访问 [BigQuery 仪表板](https://console.cloud.google.com/bigquery)并在浏览器中点击您的项目 ID。在右边展开的部分，您会看到一个`Create Dataset`选项。选择该选项，并提供数据集 ID 给你想要命名的数据集，最好将其命名为`airline`以更好地识别它，保留其他设置为默认，仅在需要时调整它。

您将能够在 explorer 部分看到新创建的空数据集，关注刚刚创建的新空数据集，并单击`+`图标向其添加表。点击后，您将能够看到`Create table`选项，从源下拉列表中选择`Upload`作为首选项，并上传您下载的 CSV 航空公司数据集。交叉检查项目名称和数据集名称是否设置为您新创建的项目。给表格命名，点击底部的蓝色按钮`Create table`，完成表格创建。

就这样，你的数据都设置好了。

# 使用 Cube.js 引导您的项目

既然我们在 BigQuery 中有了数据集，我们就需要能够使用这些数据，这意味着要与 BigQuery 和 SQL 查询直接交互。SQL 没有错；这是一种很棒的特定于领域的语言，但是让 SQL 查询遍布于您的代码库听起来像是一种泄漏的抽象——您的应用层将知道您的数据库中的列名和数据类型。

为了改进这一点，我们将使用 Cube.js 作为我们选择的开源分析 API 平台。Cube.js 提供了一种称为“语义层”或“数据模式”的抽象，它封装了特定于数据库的东西，为您生成 SQL 查询，并允许您使用高级的、特定于域的标识符来处理数据。

导航到您的项目目录，使用 Cube.js 引导您的项目，以安装所有必要的驱动程序和分析依赖项，并连接数据库

```
npx cubejs-cli create airline-analysis -d bigquery
```

Cube.js 利用 Cube.js CLI 创建带有-d 标志的新项目，以连接相应的数据库。这里是 cube.js 支持的数据库的列表[和](https://cube.dev/docs/connecting-to-the-database)。在引导的同时，它还会创建一个`.env`文件，其中包含后端所需的项目配置，包括数据库类型、项目 id、项目环境等。

下面是我们的 BigQuery 项目的`.env`文件的样子。

```
CUBEJS_DB_BQ_PROJECT_ID=PROJECT_ID
CUBEJS_DB_BQ_KEY_FILE=key.json
CUBEJS_DEV_MODE=true
CUBEJS_DB_TYPE=bigquery
CUBEJS_API_SECRET=SECRET
```

将你的`PROJECT_ID`替换为谷歌云平台新创建的项目 ID，点击页面顶部的项目切换按钮即可找到。此外，将先前下载到您系统中的服务帐户密钥复制到您的项目文件夹中，并将其重命名为`key.json`以便于理解。

在本地运行 Cube.js 后端服务，在控制台中键入以下内容*(确保导航到根项目目录)*

```
npm run dev
```

> 您会注意到一条消息，指明服务器默认运行在 localhost:4000 上。继续操作并打开它以查看您的数据集以及 Cube.js 开发人员游乐场。

# 定义数据模式

数据模式是对数据的特定于领域的高级描述，我们需要首先定义我们的数据模式，以便浏览数据。使用数据模式，您不需要编写 SQL 查询，而是依赖于 Cube.js 查询生成引擎，这使得处理数据更加容易。

我们的数据模式将作为`schema/Airline.js`出现在`schema`文件夹中

> 如果已经手动更改，请务必将`sql: 'SELECT * FROM <Your Dataset>.<Table Name>'`更改为您自己的数据集和表格名称。

在这个模式中，`measures`是要计算的数值，而`dimensions`是要计算度量的属性。尺寸可以有`string`或`time`类型。像`CAST`和`TIMESTAMP`这样的 BigQuery 函数也可以用在度量和维度中。

# 摆弄数据可视化

考虑到您的服务器已经在运行，您可以访问`localhost:4000`查看 Cube.js 开发者园地，在那里您可以找到`Build`选项卡。该选项卡包含一些令人惊叹的数据选择选项，在这些选项的顶部可以看到可视化效果，只需点击`Measure`并选择一个度量标准来测量您的数据，然后选择您想要的数量`Dimensions`，您应该能够看到/过滤您自己的可视化效果以及所有不同的选项。

现在让我们以 Cube.js 模板为基础来生成我们的前端应用程序。只需导航到`Dashboard`选项卡，选择`Create custom application`，使用 React 和 Ant Design，并将 Chart.js 作为我们的图表库。创建`dashboard-app`文件夹并安装所有基本的依赖项需要几分钟时间。一旦完成，我们将添加我们自己的图表，并定制我们的前端应用程序。

导航到`dashboard-app`文件夹，使用以下命令启动前端服务器:

```
npm start
```

> 您将能够看到在 localhost:3000 中运行的服务器

我们的应用程序将具有以下结构

*   `App`将成为应用程序的主控制器
*   `ChartRenderer`将负责渲染不同的图表
*   `DashboardPage`将是主 UI 页面，其中包含所有图表数据*(该页面将调用* `*ChartRenderer*` *来呈现不同类型的图表)*

## App.js

`App`组件将是主控制器，看起来像这样，我们使用`CubeProvider`在整个应用程序中提供 cube.js API，以便在任何地方查询/使用数据。

## 仪表板页面

对于这个示例应用程序，我将仪表板划分为网格，以显示`Line`、`Area`、`Pie`图形和`Table`。因此，我们可以使用蚂蚁设计的网格来实现这一点。

## 呈现图表

`ChartRenderer`组件负责呈现您的图表，在本例中，我将向您展示仅呈现折线图类型的代码，以使其简单易懂。

`WhichQueryRenderer`实用程序找出图表类型，并返回带有适当的`measures`和`dimensions`的特定查询。它在内部使用从 Chart.js 中呈现折线图的`renderChart`方法。我们将它`labels`和`datasets`作为从查询中获得的`resultSet`的参数传递。

就这样，我们准备在仪表板中查看我们的第一个折线图。

> 确保从`DashboardPage`组件中移除其他图表类型及其网格。

我们将使用相同的图表呈现器组件来查询更多的图表类型，并将图表提供给 DashboardPage 组件，该组件将在 UI 中呈现它们。

GitHub 上有[的完整源代码](https://github.com/S-ayanide/Airline-Analysis)。

# 结论

如果我错过了任何一点，或者你想讨论什么，请在下面留下评论，我会尽快加入。🌟

最后，感谢您能深入到本文中，并对 React 表现出兴趣。你很了不起，每天都在做出积极的改变。再见。✌🏼