# 用于编程快捷方式的原型 JS

> 原文：<https://medium.com/geekculture/prototype-js-for-programming-shortcuts-96a7e74f6e5e?source=collection_archive---------51----------------------->

## 原型 JS 通过快捷方式消除了客户端 web 编程的复杂性。

![](img/e288c23ff1e7bfa64415ef9d17d0745c.png)

Photo by [Alvaro Reyes](https://unsplash.com/@alvarordesign?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/ux?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

[Prototype JS](http://prototypejs.org/) 是一个开源的 JavaScript 框架。Sam Stephenson 在 2005 年 2 月首次开发了这个项目。根据 w3techs 的数据，截至 2021 年 3 月， [0.6%的网站](https://w3techs.com/technologies/details/js-prototype)使用了 Prototype。这 0.6 %包括 [ServiceNow](https://en.wikipedia.org/wiki/ServiceNow) ，我目前正在使用的应用开发云平台。当使用原型 JS 时，我们消除了客户端 web 编程的复杂性，因为它包含了处理 XMLHttpRequest 的主要函数的快捷方式。在 JavaScript 中，基本的数据类型是对象。对象是属性的集合，每个属性都有一个名称和值。但是除了维护自己的一组属性之外，在 JavaScript 中，对象继承另一个对象的属性；这就是我们所说的“原型”，这种原型继承是 JavaScript 的一个基本特性。Prototype JS 进一步发展了这个原型概念，包括模拟面向对象编程传统的语法——这很有意思。当我在 2013 年第一次开始使用 ServiceNow 平台时，我发现了原型 JS。那时，我正在处理一个客户的需求，涉及到创建一个向服务器发送参数的客户端脚本。GlideAjax 类允许我从客户端执行服务器端代码，然后我偶然发现了`Class.create().`

创建一个类，然后为同一个类的实例返回一个构造函数。然后，我们可以传递一个类作为参数。在这种情况下，它被用作新类的超类，并将继承它的所有方法。我们作为参数传递的任何其他元素都被视为对象。

在 ServiceNow 中，客户端脚本代码使用 [*GlideAjax*](https://docs.servicenow.com/bundle/paris-application-development/page/script/ajax/topic/p_AJAX.html) *。脚本查询表事件并将简短描述作为警报窗口返回。*

```
function onChange(control, oldValue, newValue, isLoading, isTemplate) {
    if (isLoading || newValue === '') {
        return;
    }var ga = new GlideAjax('HelloWorld');
    ga.addParam('sysparm_name', 'helloWorld');
    ga.addParam('sysparm_number', g_form.getValue('number'));
    ga.getXML(HelloWorldParse);function HelloWorldParse(response) {
        var answer = response.responseXML.documentElement.getAttribute("answer");
        alert(answer);
    }}
```

该脚本包括我们从客户端调用的脚本。注意第 1 行，使用 Class.create()。

```
var HelloWorld = Class.create();
HelloWorld.prototype = Object.extendsObject(AbstractAjaxProcessor, {
    helloWorld: function() {
        var incNum = this.getParameter('sysparm_number');
        var gr = GlideRecord('incident');
        gr.addQuery('number', incNum);
        gr.query();
        if (gr.next()) {
   var incDesc =gr.short_description;
            return incDesc;
        }
    },
    _privateFunction: function() {     
    }
});
```

在这个[链接](https://github.com/prototypejs/prototype/blob/5fddd3e/src/prototype/lang/class.js#L5)中，您可以访问 Github 存储库中 Class.create()的完整代码。