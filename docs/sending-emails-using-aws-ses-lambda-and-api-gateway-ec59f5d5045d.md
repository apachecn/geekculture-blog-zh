# 使用 AWS SES、Lambda 和 API 网关发送电子邮件

> 原文：<https://medium.com/geekculture/sending-emails-using-aws-ses-lambda-and-api-gateway-ec59f5d5045d?source=collection_archive---------2----------------------->

Python 中的概要指南

![](img/c187c6d688fadf69ed44dc58ed2cb3cd.png)

我正在建立一些 SaaS，我将使用 AWS SES 发送“重置密码”功能的电子邮件。周围还有其他选择，我选择这个是因为我对它最熟悉(几年前就已经为客户这么做了)。

尽管如此，我仍然发现自己忘记了大部分步骤。此外，虽然我以前在 Node.js 中这样做过，但这一次我将使用 Python(因为我最近一直在使用 Python)。像往常一样，我在这里记录这个过程，以防有人(或我自己)需要。

# **步骤 0a:确保你在正确的区域**

我实际上忘记做这个了。我的帐户创建于很久以前，那时 AWS 还没有那么多地区，所以我实际上在他们的默认地区切换了几个东西——us-east-1(北弗吉尼亚)。当我最近重新登录时，我没有意识到我的默认设置现在被设置为我的地理区域，我继续验证身份并请求我当前区域的限制。

AWS 接着问我为什么要在一个新的地区申请限额。

哎呀。除了身份验证和限制之外，我们稍后需要使用的 SDK(在 Lambda 中使用)也是区域敏感的。因此，请检查区域(在菜单栏中)是否正确。

# 步骤 0b:验证身份

我们需要一些经过验证的身份来做测试——至少两个经过验证的电子邮件(一个用于发送，一个用于接收)或一个经过验证的域。如有需要，请查看[我对 DNS DKIM](https://loistsnippets.hashnode.dev/a-whole-week-delayed-due-to-dns-settings) 的咆哮进行域名验证。

# 步骤 1:创建 Lambda 函数

在λ中，点击**创建函数**。我从零开始用了**作者**。

输入一个**函数名**，对于**运行时**，我用的是 **Python 3.9** (最新支持)。

在**更改默认执行角色**下，我选择了**创建一个具有基本 Lambda 权限的新角色**。

点击**创建功能**。

# 步骤 Lambda 角色的权限

转到 IAM(如果你知道角色的名字)或者转到 Lambda 函数的页面，**配置**选项卡，点击角色名。

在**权限**下，点击**附加策略**。搜索**amazonsefull access**并将其添加到角色的权限中。

如果没有此步骤，您将在控制台中看到“…无权执行…”消息。

# **第三步:函数体**

在**代码**选项卡下，将下面的基本代码(大部分摘自 [AWS 的文档](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/send-using-sdk-python.html))粘贴到函数体中。请注意更改发件人和收件人地址以及 AWS_REGION。在这里，我设置电子邮件地址，从**事件**参数中获取输入值。

```
import json
import boto3
from botocore.exceptions import ClientErrordef lambda_handler(event, context):

    # This address must be verified with Amazon SES.
    SENDER = event['from_name']+' <'+event['from_address']+'>'

    # If your account is still in the sandbox, this address must be verified.
    RECIPIENT = event['to_address']

    # the AWS Region you're using for Amazon SES.
    AWS_REGION = "us-west-2"

    # The subject line for the email.
    SUBJECT = event["email_subject"]

    # The email body for recipients with non-HTML email clients.
    BODY_TEXT = ("Amazon SES Test (Python)\r\n"
                 "This email was sent with Amazon SES using the "
                 "AWS SDK for Python (Boto)."
                )

    # The HTML body of the email.
    BODY_HTML = """<html>
    <head></head>
    <body>
      <h1>Amazon SES Test (SDK for Python)</h1>
      <p>This email was sent with
        <a href='[https://aws.amazon.com/ses/'](https://aws.amazon.com/ses/')>Amazon SES</a> using the
        <a href='[https://aws.amazon.com/sdk-for-python/'](https://aws.amazon.com/sdk-for-python/')>
          AWS SDK for Python (Boto)</a>.</p>
    </body>
    </html>
                """            

    # The character encoding for the email.
    CHARSET = "UTF-8" # Create SES client (edited)
    ses = boto3.client('ses', region=AWS_REGION) try:
        #Provide the contents of the email.
        response = ses.send_email(
            Destination={
                'ToAddresses': [
                    RECIPIENT,
                ],
            },
            Message={
                'Body': {
                    'Html': {
                        'Charset': CHARSET,
                        'Data': BODY_HTML,
                    },
                    'Text': {
                        'Charset': CHARSET,
                        'Data': BODY_TEXT,
                    },
                },
                'Subject': {
                    'Charset': CHARSET,
                    'Data': SUBJECT,
                },
            },
            Source=SENDER
        )
    # Display an error if something goes wrong. 
    except ClientError as e:
        print(e.response['Error']['Message'])
    else:
        print("Email sent! Message ID:"),
        print(response['MessageId'])

    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda!')
    }
```

点击**部署**保存更改。

# 步骤 4:测试 Lambda 函数

仍在**代码**选项卡内，点击**测试**按钮。选择**配置测试事件**。

在**创建新的测试事件**下，输入新的**事件名称**。我的活动模板如下所示(请注意，我使用的是经过验证的电子邮件地址):

```
{
  "from_address": "sender@verifiedidentity.com",
  "from_name": "Sender Name",
  "to_address": "receiver@verifiedidentity.com",
  "email_subject": "Test Lambda Function"
}
```

点击**创建**。

返回**代码**选项卡，点击**测试**。一个新的标签“执行结果”将在 lambda_function 标签旁边打开。

此时，邮件应该已经成功发送到您的邮箱了。如果您在收件箱中没有看到垃圾邮件，您可能需要检查垃圾邮件文件夹。

# 步骤 5:触发 Lambda 函数

现在 Lambda 函数开始工作了，我们需要在它前面放置一个 API 网关，这样它就可以作为 API 被触发。

在服务下，转到 **API 网关**。点击**创建 API** 。

对于**选择 API 类型**，我选择了 **REST API** (表示“对请求和响应的完全控制”)。

输入 **API 名称、描述**并点击**创建 API** 。

在 API 页面内，点击**动作**。我选择了**创建方法>发布**。在**集成类型**下，选择 **Lambda 函数**，并输入上面创建的 Lambda 函数的名称。点击**保存**。

出现一个弹出窗口，上面写着“向 Lambda 函数添加权限”。点击**确定**。

现在我们准备测试我们的 API 触发器。因为我们选择了 POST，所以为**请求主体**出现了一个文本字段。

```
{
  "from_address": "sender@verifiedidentity.com",
  "from_name": "Sender Name",
  "to_address": "receiver@verifiedidentity.com",
  "email_subject": "Test API Gateway"
}
```

点击**测试**。

可能有其他方法可以做到这一点，比如使用 HTTP API 选项。请随意尝试替代方案(如果有更好的或错误的地方请告诉我)，我总是从错误中学习。谢谢你读到这里，我希望这有所帮助！