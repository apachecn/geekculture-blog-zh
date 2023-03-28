# 使用 RudderStack 将表单数据从 Marketo 发送到多个目的地

> 原文：<https://medium.com/geekculture/send-form-data-from-marketo-to-multiple-destinations-using-rudderstack-d5932c68f56c?source=collection_archive---------25----------------------->

![](img/b350d096e61c182e044a8078389f1043.png)

# 将匿名变为已知

您是否正在使用 Marketo 表单，并努力将用户表单提交与他们在您的网站和产品中采取的其他行动联系起来？然后继续读。在本帖中，我们将展示如何利用 RudderStack 来跟踪 Marketo 表单提交，而不干扰 Marketo *或*您的营销团队。

通过使用 RudderStack 来了解用户是如何找到您的网站并与之交互的，然后将其与您的 Marketo 表单收集的数据相结合，您将更深入地了解您的潜在客户，并为您的销售团队提供更高质量的线索。

要设置这个，首先你需要让 RudderStack 在你的网站上运行。你可以在几分钟内完成。去[这里](https://docs.rudderstack.com/stream-sources/rudderstack-sdk-integration-guides/rudderstack-javascript-sdk)，按照指示，你将很快收集数据。

一旦完成了这些工作，并且对 Marketo 进行了检测，就可以使用这个代码片段从您网站上的每个 Marketo 表单中捕获所有表单值，并通过 RudderStack 发送它们:

```
<script>
MktoForms2.whenReady(function (form) {
    form.onSuccess(function() {         
        if (typeof(rudderanalytics) !== 'undefined') {
            var formVals = form.getValues();
            rudderanalytics.track("marketo_form_submit", formVals);
        }
    });
});
</script>
```

有了 RudderStack 捕获的数据，您现在将拥有数据仓库中的所有数据，因此您可以将从*页面*和*跟踪*请求中捕获的数据与 *marketo_form_submit、*结合起来，为您提供更多关于您的客户的信息，并将他们从匿名客户转变为您可以接洽的已知客户。

本博客最初发表于:
[https://rudder stack . com/blog/send-form-data-from-marketo-to-multi-destinations-using-rudder stack](https://rudderstack.com/blog/send-form-data-from-marketo-to-multiple-destinations-using-rudderstack)