# 在 PHP 中使用 AWS SDK

> 原文：<https://medium.com/geekculture/using-aws-sdk-in-php-dcbd5513b6dc?source=collection_archive---------5----------------------->

关于使用凭据的概述指南

![](img/d2cbbd82158cea576c961aedd0ddf346.png)

Photo by [Sunrise King](https://unsplash.com/@sunriseking?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

AWS 提供了一个 SDK 来调用它的各种服务，并且它需要凭证来触发这些方法。有几种方法可以做到这一点，但这里我只记录我做了什么。

# 步骤 0a:检查区域

根据我们将使用的方法，如果区域设置不正确，SDK 调用可能会失败。检查资源在哪里，并记下相应的区域代码。

# 步骤 0b:创建访问密钥

凭据需要访问密钥才能工作。我们可以通过转到 **IAM >用户>安全凭证**来创建一个。

请注意，这个秘密只显示一次。如果它丢失或遗忘，我们将需要创建一个新的密钥。如果已经有两个现有的键，我们需要先删除其中一个。

# **第一步:获取 SDK**

有几种方法可以获得 SDK，如 AWS [这里](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/getting-started_installation.html)所提供的。我用的是. phar 文件。

```
<?php
   require '/path/to/aws.phar';
?>
```

# 步骤 2a:创建凭据—外部文件

我在这个特定的场景中使用 Docker，所以我没有设置要求我将凭证文件放在根目录下的操作系统环境。

然而，“更好”的方法仍然是将凭证放在另一个地方，而不是与主项目的其余部分编码在一起。在我参与的一个项目中，有人不小心将凭证上传到了代码库中。它将永远留在那里。对他们来说幸运的是，这是一个相对而言的私有仓库。

以下是 AWS 给出的文件外观示例(摘自[这里是](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/loading-node-credentials-shared.html)):

```
[default]
aws_access_key_id = <YOUR_ACCESS_KEY_ID>
aws_secret_access_key = <YOUR_SECRET_ACCESS_KEY>
```

我想补充两点:

1.  密钥名必须分别为 **aws_access_key_id** 和 **aws_secret_access_key** 。在我找到这个页面之前，我最初只是输入了 access_key 和 access_secret，当然，事情失败了。
2.  您的字符串必须在**双撇号**中，而不是单引号中。我是在 PHP 环境中，所以我根本没有注意到它的影响。(参见[维基百科](https://en.wikipedia.org/wiki/INI_file)中 INI 文件的示例)

换句话说，您的凭证文件应该如下所示:

```
[default]
aws_access_key_id = "key_value"
aws_secret_access_key = "secret_value"
```

然后我们可以创建 **CredentialProvider** 对象。我在这里使用的是 **ApiGatewayClient** ，但它也可以是 SDK 中的任何其他方法，例如 SESClient:

```
use Aws\Credentials\CredentialProvider;
use Aws\ApiGateway\ApiGatewayClient;$profile = 'default'; // this matches the [default] in the cred file
$path = 'path_to_cred';
$region = 'region_code'; //change this to our region$provider = CredentialProvider::ini($profile, $path);
$provider = CredentialProvider::memoize($provider);$apiGatewayClient = new ApiGatewayClient([
    'region' => $region, 
    'version' => 'latest',
    'credentials' => $provider
]);
```

# 步骤 2b:创建凭证—硬编码

只是为了比较，我也将描述硬编码的方法。这既不需要外部凭证文件，也不需要 **CredentialProvider** 对象:

```
use Aws\ApiGateway\ApiGatewayClient;
$region = 'region_code'; //change this to our region$key = 'key_value';
$secret = 'secret_value';$apiGatewayClient = new ApiGatewayClient([
    'region' => $region, 
    'version' => 'latest',
    'credentials' => [
        'key' => $key,
        'secret' => $secret,
    ]
]);
```

请注意，对于实际的“实时”项目，不推荐使用这种方法。

# 步骤 3:测试 API 调用

最后是检验的时候了。SDK 的官方 AWS 文档是这里的。我为 APIGateway 选择了(我认为是)最简单的方法: **getRestApis** 。它应该只列出我在该地区的所有 API，下面是描述:

```
$result = $client->getRestApis([
    'limit' => <integer>,
    'position' => '<string>',
]);
```

参数**极限**很容易理解，但是**位置**有点神秘。我试图将它保留为空字符串' '(毕竟它说预期类型是<字符串>)，但这不起作用。经过一番搜索，我发现我可以简单地使用一个空值:

```
$result = $apiGatewayClient->getRestApis([
    'limit' => 10,
    'position' => null,
]);
```

瞧啊。如果我们执行$result 的 var_dump，我们应该会看到列表的打印输出。

感谢您读到这里，如果我有任何错误或者可以进一步简化，请告诉我。如果你感兴趣，也请查看我的其他文章或在 Twitter 上关注我。再次感谢！