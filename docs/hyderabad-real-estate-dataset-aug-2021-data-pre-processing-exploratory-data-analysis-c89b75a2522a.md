# 探索性数据分析和数据预处理:海德拉巴房价数据集(2021 年 8 月)

> 原文：<https://medium.com/geekculture/hyderabad-real-estate-dataset-aug-2021-data-pre-processing-exploratory-data-analysis-c89b75a2522a?source=collection_archive---------36----------------------->

![](img/f16ece89bb5304cbad4630c7a0bb8f77.png)

Source: Timesproperty.com

这篇文章对印度第四大城市海得拉巴(按人口计算)的房价数据集进行了数据预处理、特征提取、特征工程和探索性数据分析。我从网上搜集了这些数据，过程在我的 [**上一篇**](https://vijayv500.medium.com/how-i-web-scraped-10-000-property-listings-of-hyderabad-city-4c731c74f1f0) 中有详细描述。访问我的 [**github**](https://github.com/vijayv500/Hyderabad-Housing-Prices) 获取完整代码&数据集。

# 文件的串联:

我的[上一篇文章](https://vijayv500.medium.com/how-i-web-scraped-10-000-property-listings-of-hyderabad-city-4c731c74f1f0)解释了我是如何收集数据的——它是以两种类型的 csv 文件收集的——***第一种类型*** & ***第二种类型***——它们将被合并起来进行分析。我一次收集了这两种类型文件的 100 页数据(大约 2400 个清单)。

有六个第一类型的文件(即来自 600 页的数据),它们连接如下。

首先，所有文件都作为 pandas 数据帧加载。

![](img/ff05ac6ea4191fc5493d26d771a11757.png)![](img/6ba881557700812ff1edc339f4c99c4e.png)

并连接在一起。我们还删除重复的行并重置索引。

![](img/2765dbec05c7b97ef1d7bca47bcda375.png)

对于第二种类型的文件，我们重复相同的过程。

![](img/e4771007c6925ba0ae56d837b1821c5d.png)![](img/f23f4e295ee6aa8a4ab1c8c886619ee3.png)![](img/68b8afe156bf65f2b5e5dc9d6c276643.png)

我们现在有两个第一和第二类型的连接文件:***‘concat _ first . CSV’***&***‘concat _ second . CSV’***。

# 内部合并:

让我们基于一个公共列*【Prop ID】*将上述两个连接在一起的文件进行“内部”合并，并保存它。

![](img/83f4ef498f36a706cd4eaff08412ebf1.png)

# 数据预处理、特征提取和特征工程:

在本节中，我们将对合并后的文件执行数据预处理、特征提取和特征工程。

为了方便起见，加载文件并重新排列列的顺序:

![](img/97506359fb25752d49971b319127c840.png)

让我们来看看列'*builder*'&'*builder 2*'，它们分别来自两个不同的容器，正如我在上一篇文章中解释的那样。

![](img/9b408c15150f99ae024d7aeb92b729b3.png)

列'*生成器*'有 293 个值，而'*生成器 2* '有 1854 个值。因此，如果'*构建器*中缺少一个值，我们可以用'*构建器 2* '中的相应值来替换(前提是它没有丢失)。这可以通过使用 numpy 的 *where* 方法来完成。因此，我们添加了一个新列' *builder_final* '，它有 2018 个值，其中 797 个是唯一的(高于' *builder2* ')。

![](img/aec85907374471a760cc00cdb3357c2a.png)

现在转到“ *other* ”列，我们收集的数据是非结构化格式的。

![](img/059490e58229ddba21ca1c87ef520938.png)

如果我们浏览几行“ *other* ”列，我们可以观察到与超面积、物业类型、准备入住、家具状况、停车场等相关的信息。我们的目标是提取对我们有用的信息，并为此添加一个专栏。

![](img/0346f0538ffc5b4d16959758283ec9cc.png)

首先，我们将在我们创建的两个不同的列中提取关于“*准备移动*和“*拥有*”的信息，然后基于它们创建一个新列。

![](img/875676a87161fe115885b85a559188de.png)

区域信息可以从*链接*栏提取。

![](img/2c25fafbde39d4a83b4b2de51bcda6ec.png)![](img/fea800989a5c17817fe0d8d0f27c78f7.png)

日期字符串应该转换成正确的格式。

![](img/5b8e690414a14a11b6d59a26bdb5a94d.png)

对位置数据进行一些预处理:

![](img/b715cbe5cb226351cb895727430f8224.png)

我们将提取的*‘area _ sqft’*列转换为 float，并添加了一个新列*‘price _ pers qft’。*

![](img/dc7eb7b449400f9754afad476bce9613.png)

删除我们不需要的列，重命名并重新排列它们以方便我们使用。

![](img/1d30f65b3c3f9567f95cb2a2ef9b20c5.png)

在这一步之后，我们遇到了' *studio apartments* '房产类型的问题——它们的面积没有被正确提取，并在上述预处理步骤之后返回 *NaN* 。我们需要稍微不同的字符串切片来提取这些数据。

首先，我们将从数据框中删除包含“工作室公寓”的行。

![](img/f0779d5aa3e43b8827b61afb441c17a1.png)

接下来，我们加载原始的合并文件作为新的数据帧，并过滤与“工作室公寓”相关的数据。可以通过使用以下字符串切片来提取区域。

![](img/5ff150b86dfff723f778edbb8a16bbb7.png)

其他预处理步骤与上述数据帧“df”相同(此处不包括代码。).

现在，两个数据帧— **df** 和 **df_studio** 被连接起来。

![](img/8dbeb39fc0f003def123d9b338fd90a4.png)

# 探索性数据分析:

在这一部分，我们将探索和可视化我们的数据。由于特性列中有许多缺失值，图表下方提供了样本大小。请记住，总共有 **10，907 个列表**供参考。

下面的图表很简单，不言自明。必要时会提供解释。

请注意，我在这里只提供了 plotly 代码的关键片段。完整代码可以在我的 [**github**](https://github.com/vijayv500/Hyderabad-Housing-Prices) 上找到。

## 新上市与转售上市:

![](img/2d1f97b6a50e45667a430c577fd661fa.png)![](img/65512f667ff93e016234093b164eeedb.png)![](img/ce67a5f28e565a75b8fb7346689a7745.png)

*   *样本量:10906 人*

## 地点:

![](img/44693dacb553bca29a213d7f163a9b13.png)![](img/08ffdf29bbb51df8589d23c9fb4b3e1e.png)

*   *样本量:10162 人*
*   *唯一位置值:1085*

## 建筑商:

![](img/6690c03d45483abc21d99ace5de15f69.png)![](img/d522b519a6426cc9e1667f7da39aae5c.png)

*   *样本量:2018*
*   *唯一构建器值:797*

![](img/68aa73dd3596499856f58091377a3d6a.png)![](img/f309429f6711cc3f2011846551d84e50.png)

**Builders: Bigger the font, more the no. of listings**

## 项目:

![](img/dfb528ffa9ac4127603195c16e92a535.png)![](img/3680ac434899fe377d6ed1e844d26322.png)

*   *样本量:2520*
*   *唯一项目值:1342*

## 占有时间:

![](img/02d1fdd63c31b642ec5206b482bbb657.png)![](img/f37dd5c4683b85c15e246e37544877f5.png)

*   *样本量:1217*

![](img/9446d234c1c73d9811056d6455edf01b.png)![](img/96967adfbfaeb90e2f01340d9ee373b0.png)

*   *样本量:8829*

## 家具状态:

![](img/07e8675b78916a5f9a2002d094d2c9ad.png)![](img/31d9fee11478e34c1208e8cea1eeb104.png)

*   *样本数量:10422*

## 属性类型:

![](img/c214f008a6e8605a729e93cd4e7d9d18.png)![](img/ca1e9eb5c33a1e516b79910e9f89dd87.png)

*   *样本量:10，907(无空值)*

## 楼层:

![](img/a99ad79b73972755429f093e283d6535.png)![](img/b7e90b73bef264bb910566c406a8f3da.png)

*   *样本数量:9212*

## 用户:

![](img/207a8bb19dcb055b2d56dd685d5f6138.png)![](img/63f4d018e3ff9598309df848aad36489.png)

*   *样本量:10907(无空值)*

## 价格/平方英尺:

![](img/2aa998037da7ae9cec97fb254ae37c97.png)![](img/b0be77838e7476e768312c323672513e.png)

*   *样本量:10374*

## 价格:

![](img/cf08dd036637c68eeb5e1ab4a55bc603.png)![](img/caea7ada23734f70d75cc5597e0919fb.png)

*   *样本量:10413*

## 价格/平方英尺与家具状态:

![](img/1bb6869c4791527b909110f02a5d1c37.png)![](img/617398376fcafb92c7bdbfdd666fe211.png)

*   *样本量:10374*

## 价格/平方英尺与月:

![](img/52b0537d46b2d695604936eb3ec4fc2a.png)![](img/be2812b53a57eb4b6ef8ce639bad17d0.png)

*   *样本量:10374*

## 最贵和最便宜的地点:

*   为了更准确，这里我们只考虑那些有 10 个以上列表的位置。为了增加位置的计数， *transform()* 函数使用如下。

![](img/571836607edf49a986bfa1d6bf02a978.png)![](img/55c3a757bc395f831719eef1c6140a8f.png)

*   *样本量:10374*

![](img/a64b126a47369b5dd8ee777cc1469b47.png)![](img/3208622d90e76bb5645cd40eaff366bf.png)

*   *样本量:10374*

## 公寓:

***新增:***

![](img/5fc4b5e718e59490ac2d128893c83735.png)![](img/e8e62c883f88c8420344e9d7bf30da6e.png)

*   *样本量:970*

***转售:***

![](img/ebaa48dd3f2c8b987e3b8611489930e9.png)![](img/4cb6bc39eb38229e7d604daea41ef456.png)

*   *样本量:8243*

**住宅房屋:**

![](img/a96d1b253b81dbf2f8f2e870a0488b06.png)![](img/1b3069bdf60f8288f72f21765b3a3ff8.png)

*   *样本量:2310*

## 建筑楼层公寓:

![](img/457902dce7625a1e3b786ede1e3dd2c3.png)![](img/e7ae94ae545fc8461be23cb2e7362ea7.png)

*   *样本量:634*

## 别墅:

![](img/9ddaa652d448eed799c10c0af2468d36.png)![](img/40541b43d682415a14769268368325e7.png)

*   *样本数量:392*

## 顶层公寓:

![](img/2ff487b54b0ecf906562ee67ed458923.png)![](img/09b25d1d57f95e881ca26e0eb993667b.png)

*   *样本量:42*

## 一室公寓:

![](img/40a6562187279e1869f358989ab8160b.png)![](img/58ac2a6dd7109c260b5b8f44bff3383d.png)

*   *样本量:13*

我的 [**github**](https://github.com/vijayv500/Hyderabad-Housing-Prices) **上的完整代码&数据集。**

结账[我是怎么刮到以上数据的。](https://vijayv500.medium.com/how-i-web-scraped-10-000-property-listings-of-hyderabad-city-4c731c74f1f0)

另请参见: [**理解 3300 万行 Instacart 数据:使用 Plotly 进行探索性数据分析**](/geekculture/making-sense-of-33-million-rows-of-instacart-data-exploratory-data-analysis-using-plotly-3f0461a56811)