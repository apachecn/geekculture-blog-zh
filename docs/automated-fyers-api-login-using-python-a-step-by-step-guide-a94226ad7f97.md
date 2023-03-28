# 使用 Python 自动登录 Fyers API:分步指南

> 原文：<https://medium.com/geekculture/automated-fyers-api-login-using-python-a-step-by-step-guide-a94226ad7f97?source=collection_archive---------5----------------------->

本文假设您已经做好了以下准备:

1.  fyers 帐户
2.  使用网站 [API](https://api-dashboard.fyers.in/v2/dashboard) 创建了一个应用。
3.  已安装的 fyers python 包

## 步骤 1:创建一个 JSON 文件，如“appid_key.json ”,并将机密信息存储在该文件中，如下所示:

## 第二步:进行必要的导入。

```
from fyers_api import fyersModel
from fyers_api import accessToken
from urllib.parse import urlparse, parse_qs
```

## 步骤 3:从 json 文件中检索凭证。

确保您的 redirect_url 与您在 Fyers 应用程序中创建的 url 相同

## 步骤 4:创建会话

使用上面创建的会话登录 Fyers 页面。
它需要你的认证数据。之后，我们有一个包含 access_token 的重定向 URL，它可以用于您希望完成的不同任务。

## 步骤 5:将 auth_code 设置为 session 并生成访问令牌。

```
session.set_token(auth_code)
response = session.generate_token()
print(response)access_token = response["access_token"]log_path = "/home/tushar/Documents/Logs_now/" # Your Logs folder
fyers = fyersModel.FyersModel(client_id=client_id, token=access_token,log_path= log_path)is_async = True  # Set to true in you need async API calls 
```

最后，如果你已经到达这里，如果你跑:

```
print(fyers.get_profile())
# {'s': 'ok', 'code': 200, 'message': '', 'data': .....
```

并获取您的详细信息作为输出。恭喜🙌🎉您已成功登录 Fyers API。