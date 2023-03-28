# 来自英超联赛 21/22 的见解

> 原文：<https://medium.com/geekculture/insights-from-premiere-league-21-22-e018430a3225?source=collection_archive---------16----------------------->

![](img/b86fcbb3fc5cd9e65c588e3deed0698e.png)

Photo by [Myriam Jessier](https://unsplash.com/@mjessier?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/sports-analytics?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 边做边学#2 (SQL 和 Tableau)

在我的**边做边学**系列中，我试图发布我个人项目的过程，通过这些项目我试图练习我的 Python 和 SQL 技能。

因此，在这个项目中，让我们探索并尝试找到关于 2021-2022 赛季英超联赛的统计数据，让我们看看我们可以获得什么样的见解。

数据集链接— [足球数据](https://www.football-data.co.uk/englandm.php)

数据显示几个属性:

*   Div =联赛部
*   日期=比赛日期(日/月/年)
*   主队=主队
*   客场队
*   FTHG =全职主队目标
*   FTAG =全职团队目标
*   FTR 和 Res =全职成绩(H =主场胜利，D =平局，A =客场胜利)
*   HTHG =半场主队进球
*   HTAG =半场休息时间团队目标
*   HTR =半场结果(H =主场胜，D =平局，A =客场胜)

匹配统计数据(如果可用)

*   HS =主队投篮
*   AS =客场球队投篮
*   HST =主队击中目标
*   AST =客队射门击中目标
*   HC =主队角球
*   AC =客场球队角球
*   HF =主队犯规
*   AF =客队犯规
*   HY =主队黄牌
*   AY =客队黄牌
*   HR =主队红牌
*   AR =客队红牌

表格:

![](img/7f3fba8f1616a1b59c157d65f4fe13b6.png)

snippet of the table(only the relevant columns are shown)

> 返回每支球队的主场总胜率，按降序排列。

![](img/bcd30722ec498ed5acdfaec2e97fdc12.png)

Snippet of the Query

![](img/d9f8956df5c92a59fc1e9e17e8dc2557.png)

Result of the query above

![](img/49740fd6c0330674c64ff451b2178d97.png)

Snippet of the Tableau Sheet

> 返回按降序排列的每支球队的客场胜利总数。

![](img/f084b1f23352eb8b8297bfb59ddfe719.png)

Snippet of the Query

![](img/a15098c1a7c60a0c2e577c0d17599c99.png)

Result of the query above

![](img/936d5240fa9ef02bfb8daa181f6b2cc0.png)

Snippet of the Tableau Sheet

> 整个锦标赛的总进球数按降序排列

![](img/ef0982667c4fd34c48882cc6bde7fe84.png)

Snippet of the Query

![](img/cbe6c207a14f3316511b764262395f4e.png)

Result of the query above

![](img/0f6b31cc8bf4f7e6da0c65c72a0fdf53.png)

Snippet of the Tableau Sheet

> 裁判和每场比赛的总裁判数，按降序排列

![](img/613c3cd9ab24fd63df2729da512d3f7d.png)

snippet of the query

![](img/a74eaeac28e0391d4fbe58423218f32d.png)

snippet of the result(some values are missed in the screenshot)

![](img/6f54deb6a1a3eeecee0e764b82ae8a44.png)

在主场输掉一场比赛本身就被视为对俱乐部地位的不尊重，有些比赛在半场结束前，比赛似乎控制了主队，但在下半场，客队迎头赶上，赢得或平了比赛，让我们看看哪些球队这样做得最多，

> 半场结束后让对手在主场迎头赶上的球队

![](img/393fbf48513576703771a43cdc8431e4.png)

snippet from the query

![](img/88486cd8921e27bd8810dcb6d56939c1.png)

snippet of the result

![](img/b96781a06e172f1af2affff1fa4eb311.png)

snippet from the tableau

> 整个比赛期间收到的球队和红牌总数

![](img/7c29453ed3fe812c9fd63083c417b179.png)

snippet of the query

![](img/28c335490b3969de1218c49b2a39c899.png)

snippet of the result

![](img/dcda62ab6bdb4d11652f681fc376903f.png)

snippet from the tableau

> 按降序排列，每个日期中发生了多少次匹配

![](img/a7e6302af86ddde8b12632e1213b882e.png)

snippet of the query

![](img/af28c8a5ebfe67734ed53cd3818c4dad.png)

snippet of part of result

![](img/0078dac055165b6b62eb8df43dc77adb.png)

snippet from the tableau

让我们为联盟创建积分表

积分系统如下:

如果一个队赢了——3 分

如果一个队打平— 1 分

如果一个队输了——0 分

> 点数表

![](img/27440d92d7ead3fe082b9ca4d23b3384.png)

snippet from the query

![](img/aa700a745a658b0e7cd9b6e2de184a11.png)

snippet of the result

![](img/0026e84a49caa39d3665d4eb70152b72.png)

snippet from the tableau

所以，现在让我们转到具体的球队分析，让我们分析利物浦的统计数据，我们已经看到了总进球，主场胜利，客场胜利，现在，让我们分析裁判 wrt。

> 裁判和为利物浦执法的比赛总数

![](img/12a7c17332924f29f1eeb658a71ed3c8.png)

snippet of the query

![](img/e3eb50a6d9a4dbe715f362cccadef514.png)

snippet of the result

![](img/bcbed45d63c8b716f867866f7d547335.png)

snippet from the tableau

让我们分析一下裁判和赢得比赛之间的关系

> 裁判 X 为利物浦获胜

![](img/1e56a1ddfafa459d9e67c077195facc4.png)

snippet of the query

![](img/16557c632a0e108a78ff47ed8e6201dd.png)

snippet of the result

![](img/b4fdf9af775f791e60445f06c95b350d.png)

snippet from the tableau

相同分析 wrt 损失

> 裁判 X 利物浦输球

![](img/954ebf79e87e270f8a9935c9c7cafb0a.png)

snippet of the query

![](img/3a0c60c1d78aef2af470aab4b1228123.png)

snippet of the result

![](img/0fd3a8383df656f927fb15cc14257c90.png)

snippet from the tableau

总结:

这是一个关于 PL 21/22 赛季的简短分析，我们可以找到更多的统计数据，比如总投篮次数和胜率之间的相关性，或者总命中率和胜率之间的相关性。如果您对查询有任何建议或对 tableau 表有任何建议，欢迎您联系，并对本文提出建议或意见。

谢谢你……..