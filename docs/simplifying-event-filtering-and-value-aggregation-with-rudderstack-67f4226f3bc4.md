# 使用 RudderStack 简化事件过滤和值聚合

> 原文：<https://medium.com/geekculture/simplifying-event-filtering-and-value-aggregation-with-rudderstack-67f4226f3bc4?source=collection_archive---------60----------------------->

![](img/f56f17b6b62056739fe271f488939986.png)

处理事件数据有时很麻烦。由于开发人员所做的更改，开发人员可能会传输带有错误的事件。此外，如果数据工程团队决定更改数据仓库模式，有时可能会引入错误。由于对架构的这些更改，可能会发生数据类型冲突。人们如何处理生产环境中可能出现的所有不同的事件数据问题？这篇博客讨论了 [RudderStack](http://www.rudderstack.com/) 如何处理事件过滤和值聚合而不引入手动错误。

RudderStack 的解决方案是一个复杂的机制。在这里，您可以使用 JavaScript 实现自定义逻辑来定义转换。您可以将这些转换应用于传入的事件。

拥有一个像 RudderStack 这样的表达环境为数据工程团队如何与数据交互提供了无限的可能性。在这篇博文中，我们将只探讨我们在 RudderStack 社区中遇到的两个最常见的用例。事件过滤和值聚合是通用的，易于实现，但非常强大。

# 用于事件过滤和值聚合的用户转换

您可以在 RudderStack 设置的配置平面中定义用户转换。在我们的 [GitHub](https://github.com/rudderlabs/sample-user-transformers) 上几乎没有可用的用户转换示例。这篇博客提供了对这样一个示例转换的深入了解，您可以将它用于:

*   **事件过滤:**阻止事件传递到目的地。您可能需要过滤组织使用多种工具/平台来满足不同业务需求的事件。此外，您可能希望仅将特定事件路由到特定工具/平台目的地。
*   **值聚合:**这允许聚合特定事件类型的特定属性的值。当组织不打算使用工具/平台来执行事务级记录保存和/或分析时，您可能需要汇总值。相反，他们想要整合的记录/分析。因此，这种转换有助于减少网络流量和请求/消息量。这是因为系统可以用具有聚合值的同一类型的单个事件替换特定类型的多个事件。这种转变也有助于降低成本，因为目的地平台按事件/消息的数量收费。

您可以在我们的 [GitHub](https://github.com/rudderlabs/sample-user-transformers/) 页面上查看示例转换。

# 履行

您需要在`transform`函数中包含所有逻辑，该函数将一组事件作为输入，并返回一组转换后的事件。`transform`函数是所有用户转换的入口点函数。

```
function transform(events) {
	const filterEventNames = [
    		// Add list of event names that you want to filter out
   	 "game_load_time",
   		 "lobby_fps"
	]; //remove events whose name match those in above list
	const filteredEvents = events.filter(event => {
    	const eventName = event.event;
    		return !(eventName && filterEventNames.includes(eventName));
	});
```

上面的代码片段展示了如何使用 JavaScript 数组的`filter`函数根据事件名称过滤出事件。

该代码的变体也是可能的。在这里，事件名称数组中的值是您希望*保留的值，您从倒数第二行的`return`语句中删除了 not ( `!`)条件。*

下面的代码显示了基于简单检查(如事件名称匹配)的事件删除，但更复杂的逻辑涉及检查相关属性值的存在。

```
//remove events of a certain type if related property value does not satisfy the pre-defined condition
//in this example, if 'total_payment' for a 'spin' event is null or 0, then it would be removed.
    	//Only non-null, non-zero 'spin' events would be considered
	const nonSpinAndSpinPayerEvents = filteredEvents.filter( event => {
    		const eventName = event.event;
    	// spin events
    		if(eventName.toLowerCase().indexOf('spin') >= 0) {
        		if(event.userProperties && event.userProperties.total_payments 
&& event.userProperties.total_payments > 0) {
            		return true;
        	} else {
            			return false;
        		}
    		} else {
        			return true;
    	}
	});
```

从上面的例子可以看出，您可以使用过滤后的数组作为一个步骤的输出，作为下一个步骤的输入。因此，您可以将转换条件以菊花链形式连接起来。

最后，下面的代码展示了如何为批处理中特定类型事件的特定属性准备聚合。此后，代码返回相关类型的单个事件。此外，代码返回相应属性的聚合值。

```
//remove events of a certain type if related property value does not satisfy the pre-defined condition
//in this example, if 'total_payment' for a 'spin' event is null or 0, then it would be removed.
    	//Only non-null, non-zero 'spin' events would be considered
	const nonSpinAndSpinPayerEvents = filteredEvents.filter( event => {
    		const eventName = event.event;
    	// spin events
    		if(eventName.toLowerCase().indexOf('spin') >= 0) {
        		if(event.userProperties && event.userProperties.total_payments 
&& event.userProperties.total_payments > 0) {
            		return true;
        	} else {
            			return false;
        		}
    		} else {
        			return true;
    	}
	});
```

# 结论

在上面的片段中:

*   首先，代码将`spin_result`事件收集到一个数组中。
*   然后，代码通过迭代上述数组的元素来聚合三个属性的值— `bet_amount`、`win_amount`和`no_of_spin`。
*   此后，系统将聚合值分配给数组中第一个`spin_result`事件的相应属性。
*   现在，代码将非目标类型的事件(在本例中为`spin_result`)分离到另一个数组中。如果没有这样的事件，则创建一个空数组。
*   最后，系统将`single spin_result`事件添加到上一步创建的数组中，并返回结果。

# 免费注册并开始发送数据

测试我们的事件流、ELT 和反向 ETL 管道。使用我们的 HTTP 源在不到 5 分钟的时间内发送数据，或者在您的网站或应用程序中安装我们 12 个 SDK 中的一个。[入门](https://app.rudderlabs.com/signup?type=freetrial)。

本博客最初发表于
[https://rudder stack . com/blog/simplified-event-filtering-and-value-aggregation-with-rudder stack](https://rudderstack.com/blog/simplifying-event-filtering-and-value-aggregation-with-rudderstack)