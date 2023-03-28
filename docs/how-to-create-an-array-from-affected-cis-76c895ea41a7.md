# 创建包含受影响配置项的阵列

> 原文：<https://medium.com/geekculture/how-to-create-an-array-from-affected-cis-76c895ea41a7?source=collection_archive---------48----------------------->

## **使用** JavaScript **push()方法将 CI 名称列表添加到一个数组中。**

![](img/40be9260f484781a6e6c5b7fa834ff31.png)

Photo by [Pawel Czerwinski](https://unsplash.com/@pawel_czerwinski?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/array?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

当处理与第三方应用程序的集成时，知道如何从 ServiceNow 中的表创建元素数组会很方便。

假设我们有一个事件记录和该事件的受影响配置项。我们希望用事件的受影响配置项列表填充字符串类型字段`u_affected_cis`。

为了实现这一点，我们在 task_ci 表中的之后创建一个*业务规则，它在 *create* 和 *delete 上运行。*因此，对于每个新的或删除的受影响配置项，系统将使用刷新的配置项名称列表更新事件字段 u _ affected CIs。*

首先，我们使用数组文字语法声明 ciList。

```
var ciList = [];
```

然后，我们创建一个 GlideRecord 来查询 task_ci 表。GlideRecord 为 task_ci 表创建一个 GlideRecord 类的实例。

```
var gr = new GlideRecord('task_ci'); 
```

然后，我们查询表 task_ci。

```
gr.addQuery('task', current.task);
```

我们从查询结果中遍历受影响的配置项列表。toString()方法返回一个 String 对象的值，push()方法将新的项添加到数组的末尾。

```
 var gr = new GlideRecord('task_ci'); 
    gr.addQuery('task', current.task); 
    gr.query(); 
 while (gr.next()) {
     var affectedCIs = gr.ci_item.name.toString();
            ciList.push(affectedCIs);
          }
```

一旦我们用来自 CI 名称的元素填充了 ciList 数组。我们查询事件表，其中 sys_id =当前删除或添加的 task_ci 记录，并填充字段 u_affected_cis

```
var gr2 = new GlideRecord('incident');
   gr2.addQuery('sys_id', current.task);
   gr2.query();
        if (gr2.next()) {
            gr2.u_affected_cis = ciList.toString(); 
            gr2.update();
         }
```

完整的业务规则。

```
(function executeRule(current, previous /*null when async*/ ) {
 var ciList = [];
 var gr = new GlideRecord(‘task_ci’); 
 gr.addQuery(‘task’, current.task); 
 gr.query(); 
 while (gr.next()) {
 var affectedCIs = gr.ci_item.name.toString(); 
 ciList.push(affectedCIs); //Add new items to the end of the array
 }
 var gr2 = new GlideRecord(‘incident’);
 gr2.addQuery(‘sys_id’, current.task);
 gr2.query();
 if (gr2.next()) {
 gr2.u_affected_cis = ciList.toString(); //populate u_affected_cis
 gr2.update();
 }
 })(current, previous);
```

# 关键注意事项

`push()` 方法向数组中添加新的项并返回新的长度。方法在数组末尾添加新项。此方法更改数组的长度。