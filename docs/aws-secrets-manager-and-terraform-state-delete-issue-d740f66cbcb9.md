# AWS 机密管理器和 Terraform 状态删除问题

> 原文：<https://medium.com/geekculture/aws-secrets-manager-and-terraform-state-delete-issue-d740f66cbcb9?source=collection_archive---------7----------------------->

![](img/5351286122d6b73facd4688aeb044258.png)

Photo by [Mahdi Bafande](https://unsplash.com/@mahdibafande?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我最近遇到了一个问题，推一些地形变化，并看到以下错误:

```
You can't perform this operation on the secret because it was marked for deletion
```

我完全糊涂了，如果我没有看错的话，我会看到它是在处理地形本身。所以让我们解释一下这个问题是如何发生的。由…