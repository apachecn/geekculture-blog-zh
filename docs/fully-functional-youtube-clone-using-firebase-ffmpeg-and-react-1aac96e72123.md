# 使用 Firebase、FFmpeg 和 React 的全功能 Youtube 克隆

> 原文：<https://medium.com/geekculture/fully-functional-youtube-clone-using-firebase-ffmpeg-and-react-1aac96e72123?source=collection_archive---------6----------------------->

一个 youtube 的精确克隆，从查看计数到使用 Firebase、FFmpeg 和 React 订阅所有内容(没有 Youtube Api)的所有功能

![](img/df013137c267e6f04e5eeb6f66533c74.png)

Youtube Clone

# 遗憾的

首先，我要为我的文笔不一致说声抱歉。我的频道已经停播一年了，但是事实上，直到我今天查看时，我还一直有浏览量，这让我意识到。所以，是的，我又回来了:)带着我在过去一年里做的一些非常酷的项目。我希望再次得到支持

# GitHub 代码

Github 代码已经上传[这里](https://github.com/harsh317/Youtube-CLone-ReactJs)！你可以去看看！

[](https://github.com/harsh317/Youtube-CLone-ReactJs) [## GitHub-harsh 317/Youtube-CLone-react js:Youtube 的精确克隆，具有来自…

### youtube 的一个精确的克隆，具有从查看计数到订阅所有内容的所有功能(没有 Youtube Api)…

github.com](https://github.com/harsh317/Youtube-CLone-ReactJs) 

现在让我们来看看你们来做什么:)这是我的暑假，我花了很长时间来完成这个项目，我计划增加所有的功能。不开玩笑，这个项目包含了大量的功能。*不信我*？自己去看吧

**yooo, Ik editing sucks but don’t roast me on that ; — ;**

正如从视频中看到的，你可能会说 UI 也不太差，在这个项目中，我试图专注于每个部分(因为这是我很长时间后的第一个编码项目)。我让应用程序完全响应，动画可以看到， ***这里是我忘记在视频中告诉的附加功能:***

1.  **动画和用户界面**
2.  **和良好的认证系统** ( *忘记密码，重置密码，电子邮件验证系统*)例如，您可以在您的`ProtectedRoute`组件中传递一个`emailVerified`属性，就像您想要的任何页面上的这样:

![](img/18195dd98cdb08ccb323136e970ad437.png)

*3* 。**基于您的 Ip 的查看计数系统**

每个 IP 的浏览量都计算在内。所以，当你刷新你的页面时，不会产生额外的视图。您可以使用任何库/api 来获取 Ip，但我使用以下 api:

![](img/887efdf220942f11da983e4cd366e94e.png)

*4* 。**搜索功能**(您可以根据视频标题(而非标签)搜索视频标题)

*5* 。**根据标签获取相关视频**(根据您上传视频时上传的标签获取手表屏幕和主屏幕上的相关辅助视频……)

*6* 。**宣传功能**(将您的视频公开或保密(您也可以稍后编辑) )

*6。* **订阅自己喜欢的创作者和喜欢/不喜欢以及评论功能**(相当自明的 bruh)

*7* 。**视频所有者可以编辑他的视频**(只有视频所有者可以编辑他的视频，改变视频的可见性或其他事情)

*8* 。**使用 FFmpeg** 为您的视频生成缩略图(上传视频后，应用程序自动为您的视频生成缩略图，缩略图和视频存储在您的服务器中，如下所示: )

![](img/1c901ac11a67bcff67d22451c502311a.png)

How are videos and thumbnails stored on the server (Further file paths stored in firebase)

*9* 。**强大和一些高级的火焰基地规则**(我浪费了一整天来制定好的火焰基地规则，但最终它是值得的)

![](img/b96dd3a60b3cb3f750addd6089acbe2f.png)

Firebase rules (Yo It want transparent uwu)

我希望这个项目让你和我一样兴奋。和往常一样，这将是一个项目的一部分，我承诺每周张贴一篇文章(周六/周日)。但除此之外，我还有各种各样的项目，向你展示从 SOS react 本地应用到你自己的游戏等等。不骗你，这些项目会很有趣，充满了学习

所以我想让我们从下一篇文章开始继续设置我们的项目，直到完成这个项目。对于那些好奇想知道项目和文件结构的人，这里有这些:) :

# 文件结构

Bruh Lmao 这些文件太多了，甚至没有出现在一个屏幕上

![](img/09c20456cfc65f67012491a6302279b9.png)

File Structure

# Firestore 视频收藏结构

```
{"data": {"GVSA4od75f5BOq2CfmBK": {"Categories": ["Ballcat", "Cats", "#meme"],"Userpfp": "https://lh3.googleusercontent.com/a-/AOh14Gg6y6DrYdtYNYl-p_6eV3OyUAARBD0J4H4bTgML=s96-c","Usersname": "harshvardhan jain","comments": [{"Comment": "Good video","VideoId": "GVSA4od75f5BOq2CfmBK","name": "harshvardhan jain","timestamp": {"__datatype__": "timestamp","value": "2022-06-13T09:58:16.376Z"},"userId": "EK9nslWIRFgwjnhPuFS9GnPgMQN2","userPfp": "https://lh3.googleusercontent.com/a-/AOh14Gg6y6DrYdtYNYl-p_6eV3OyUAARBD0J4H4bTgML=s96-c"}],"descriptions": "This is how you make slime....I am gonna right a long description for testing, so you have to cope.......Me trying my best to make it long but still short aaaaaaaa GOD help me.....Should i just copy paste some dummy text? hmmmm SUS aaaaaaaaa","dislikes": [],"duration": 14.84,"filePath": "Uploads\\file-1655114264218-cute_cat_relax_on_outdoor_ground_6892533.mp4","likes": ["EK9nslWIRFgwjnhPuFS9GnPgMQN2"],"ownerId": "EK9nslWIRFgwjnhPuFS9GnPgMQN2","publicity": "Public","thumbnail": "http://localhost:5000/Uploads/thumbnails/thumbnail-file-1655114264218-cute_cat_relax_on_outdoor_ground_6892533.png","timestamp": {"__datatype__": "timestamp","value": "2022-06-13T09:57:45.800Z"},"title": "This Is How Cat Relax Uwu","views": ["122.173.27.25"],"__collections__": {}},"UAge5Pgww7Z1vWsYro4l": {"Categories": ["Tankster.io", "Online games", "#gaming"],"Userpfp": "https://lh3.googleusercontent.com/a-/AOh14Gg6y6DrYdtYNYl-p_6eV3OyUAARBD0J4H4bTgML=s96-c","Usersname": "harshvardhan jain","comments": [{"Comment": "A comment from new user","VideoId": "UAge5Pgww7Z1vWsYro4l","name": "A NEW USER","timestamp": {"__datatype__": "timestamp","value": "2022-06-13T03:47:49.003Z"},"userId": "MmQFFNPBNgW2BNiHH1oJYxas1F82","userPfp": "https://cdn-icons-png.flaticon.com/512/149/149071.png"}],"descriptions": "Tankster.io A pretty nice game that I found online !! It's discord is good place Yea changed description","dislikes": [],"duration": 34.835,"filePath": "Uploads\\file-1655053918168-Tankster.io - Google Chrome 2022-05-15 21-59-05 (online-video-cutter.com).mp4","likes": ["EK9nslWIRFgwjnhPuFS9GnPgMQN2", "MmQFFNPBNgW2BNiHH1oJYxas1F82"],"ownerId": "EK9nslWIRFgwjnhPuFS9GnPgMQN2","publicity": "Public","thumbnail": "http://localhost:5000/Uploads\\thumbnails\\file-1655114375973-Screenshot 2022-03-02 165903.png","timestamp": {"__datatype__": "timestamp","value": "2022-06-12T17:12:00.247Z"},"title": "Tankster.io An Online Game pretty nice Game","views": ["122.173.31.158", "122.173.27.25"],"__collections__": {}}}}
```

至此，我想我们的文章就到此为止了。但下一篇不算太远；)希望看到你们兴奋，我们将在下一部分与你们见面

在那之前保持安全，保持健康

**谢谢**