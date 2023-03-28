# 在 Docker 容器上部署 Jenkins

> 原文：<https://medium.com/geekculture/deploy-jenkins-on-docker-container-aaa710a33572?source=collection_archive---------2----------------------->

💥在这篇文章中，我将在 **Dockerfile** 的帮助下**自动化**一个 **docker 容器**内的 Jenkins，然后**创建并执行**不同的 **Jenkins 作业**在一个容器内运行。

![](img/4eb1ec8f25f288cda88c10f663578b9a.png)

# ⚡Task Description⚡

**1。使用 Dockerfile** 创建安装了 Jenkins 的容器映像

**2。当我们启动这个映像时，它应该会自动启动容器中的 Jenkins 服务。**

**3。使用 Jenkins** 中的构建管道插件创建作业 1、作业 2、作业 3 和作业 4 的作业链

**4。Job1:部分开发者向 GitHub 推送回购时自动拉取 GitHub 回购。**

**5。Job2:通过查看代码或程序文件，Jenkins 应该自动启动相应的语言解释器安装映像容器来部署代码(例如，如果代码是 PHP 的，那么 Jenkins 应该启动已经安装了 PHP 的容器)。**

6。工作 3:测试你的应用程序是否工作正常。

7。Job4:如果应用程序不工作，然后发送电子邮件给错误信息的开发人员。

8。为 monitor : If 容器创建一个额外的 job job5，其中应用程序正在运行。由于任何原因失败，则该作业应自动再次启动容器。

![](img/e55d396a01b097107a657d5594e9b259.png)

# 那么，让我们开始吧🤩

⏩**创建一个 docker 文件**

👉首先为 docker 文件创建一个单独的工作空间。我创建了一个名为 **js-ws** 的目录。在这个工作区创建一个 **Dockerfile** 。

![](img/eb6e556435da6cc8e2e32475a7977c6e.png)![](img/73a06a3fd6d95fed0da7fed26fd52aef.png)

**Dockerfile**

**⏩建立一个 Dockerfile 文件**

## docker build-t<image_name>:<tag>/工作台</tag></image_name>

![](img/d8d6eb807188912a674e2a20f92d95b5.png)![](img/bba55b517b22bc79cc799ab9ee3e6179.png)

**docker images**

## ⏩启动 docker 容器

启动 docker 容器，它将自动启动**Jenkins**服务。

![](img/95ed40789533d689b39291fd24304f2d.png)![](img/9361d3c96c9f12d9f408fe6158439b18.png)

**Here we got Password**

于是，我们得到了詹金斯的**初始密码**。这是第一次解锁詹金斯所需要的。

![](img/cc526b4bde86bc0d54abd91b1a8445f8.png)

**Paste the password here to unlock Jenkins**

⏩现在点击**安装建议插件**。这将自动安装重要的插件..

![](img/e2d4b1d6eeba929f2ccba3c2f7019b5d.png)

⏩我们可以看到下面👇那个**插件开始下载** …..

![](img/9fcf44494dcdf7a4e455bb6b3105f558.png)

⏩在这里，我们创建**用户名和密码**用于登录 jenkins

![](img/432667bd7ba1a35564aa211694b2aaa2.png)![](img/3c3f937fd4c311daf8181aeef3f332a8.png)![](img/3c92bb5cb98a2819c6c15486a84c61d6.png)

**所以现在，詹金斯准备使用**😀✌

⏩ ***首先，创建一个新的 GitHub repo，这样我们就可以把 GitHub 的 url 放到 jenkins 的作业中..***

# 🔆现在，让我们创造就业机会

## ⏩创建工单 1

***这个作业会在某个开发者向 GitHub 推送回购时自动拉取 GitHub 回购。***

👉首先在**源代码管理**中给出你的 GitHub 库 url

![](img/882308e1e70a647aaf95b258ae51ee7e.png)

👉在 Build Triggers 中，选择 **GitHub hook trigger 进行 GITScm 轮询**，这样只有当 GitHub 代码发生变化时， **jenkins job 才会自动启动**。

👉接下来这段代码将帮助我们把文件从 **Jenkins 工作区复制到我们想要的文件夹。**

![](img/6314d333d981b9312a288106fc2f7c3c.png)![](img/c2db7983f81175c199dc03262f945b40.png)

**Console Output of J0B-1**

***这样，JOB-1 就成功完成了*** 😀

# ⏩创建工单 2

***通过查看代码或程序文件，Jenkins 应该自动启动相应的语言解释器安装镜像容器来部署代码(例如，如果代码是 PHP 的，那么 Jenkins 应该启动已经安装了 PHP 的容器)。***

👉该作业将在作业 1 的作业构建后**触发**，因此在**构建触发**中选择**其他项目构建后**构建，在**项目查看**中，将**作业-1**

![](img/3fa38ecaab3aace0c9c03d48e5f8e06e.png)

👉现在在下面的脚本中，我写了一些步骤。 **chroot 改变当前根目录**。下一行脚本将**启动一个 docker 容器，其中安装了 php。**

![](img/a99f5dd041dcf4c2d25215fd58e4e3d0.png)![](img/86602cef4d513964ec24ecb3345ccbbb.png)

**Console Output of JOB-2**

***这样，JOB-2 就成功完成了*** 😀

# ⏩创建工单 3

***作业 3 会检查我们的网站是否在运行。***

该作业将在作业 2 的作业构建之后由**触发**，因此在**构建触发**中选择**构建在其他项目构建之后**并在**项目中查看**，将**作业-2**

![](img/6605e2afdbe031bc7038f7e9d251e21b.png)

👉现在下面的脚本将检查我们的网站是否给出 200 状态码。**状态代码 200 表示网站运行正常。**

![](img/8210e7155add0f32ca92142bccd4d297.png)![](img/61f755c9867ce7f08550c650eacd3870.png)

**Console Output of JOB-3**

## ***这样，JOB-3 就成功完成了*** 😀

# ⏩创建工单 4

***如果应用程序不工作，那么发送电子邮件给开发者的错误信息。***

👉该作业将在作业 3 的作业构建之后由**触发**，因此在**构建触发**中选择**构建在其他项目构建之后**并且在**项目中观看**，将**作业-3**

![](img/4f0b157c866647f0069004b1708b54d7.png)

👉现在这个脚本将首先检查我们的网站是否**没有给出 200 状态码**，然后**它将发送电子邮件(通知开发者)**

![](img/0102ccbcd15bccd08a602aeec78cfe39.png)![](img/990a386475312e11aeb98970a366768f.png)

**Console output of JOB-4**

## 因此，JOB-4 成功完成😀

# ⏩创建工单 5

***该作业用于监控 docker 容器。如果我们的容器在任何时候失败，那么这个作业将再次启动容器。***

👉在构建中定期触发-给出“* * * * *”。这个“*** * * * * ***”意味着 jenkins 会每秒检查一次 ***docker 容器。*** *如果我们的容器在任何时候失败，那么这个作业将再次启动容器。*

![](img/97418f7a67d301f42005cdd9b1558236.png)![](img/725d14d7b4b02dd3f066bc03d69b4af1.png)

## 因此，作业 5 成功完成😀

![](img/31ca655e10eef1c78c96e1b55bf0199c.png)

**All jobs**

![](img/59e1563a2062353021babb0f4ef9d322.png)

**GitHub files**

***最后所有的工作都完成了，现在让我们创建构建管道***

# ⏩创建生成管道

👉要创建构建管道**首先安装构建管道插件。**

👉然后点击**新建视图**来添加一个管道。

👉命名并**启用构建管道视图选项，然后**选择初始作业作为**作业 1**

# 现在让我们看看我们的管道:

![](img/aaab675c3bc902e34a38cc84ec280a61.png)

**👉到网站上键入我们服务器的网址，就会看到页面运行良好。**

![](img/4492123463dcc328adf47b6aa0b2cf28.png)

**Webpage working fine**

👉现在**让我们通过在 php 代码**中犯一个错误来检查一下。所以，现在我按错了代码。

![](img/086429c6982d9dbf515133b283131813.png)

**wrong PHP CODE**

现在你会看到你的作业 3 会自动失败

![](img/387e56a1f522c1906154de51502ac002.png)

***👉在这里你可以看到作业 3 已经失败，所以现在作业 4 将工作并向开发者发送通知。***

![](img/8eae41b48dcdfc02689150d019c510b5.png)

**Mail notification**

👉此外，如果你进入**页面，你会发现它不工作**。

![](img/0febda14e46ad69df27234c494bdb9a0.png)

**Web page is not working**

## 💥成功完成任务😀✌

我希望这篇文章能帮助你整合 GitHub、Jenkins 和 Docker。

**🔅GitHub 链接**:[https://github.com/Anujakumari/Jenkins_Task_2.git](https://github.com/Anujakumari/Jenkins_Task_2.git)

**🔅**𝐃𝐨𝐜𝐤𝐞𝐫𝐡𝐮𝐛𝐮𝐫𝐥:[https://hub . docker . com/repository/docker/anuja 9431/Jenkins-image](https://hub.docker.com/repository/docker/anuja9431/jenkins-image)

# 感谢阅读！！

# 🔰继续学习！！继续分享🧾🔰

## https://www.linkedin.com/in/anuja-kumari-4a62581aa/领英简介:[](https://www.linkedin.com/in/anuja-kumari-4a62581aa/)

## **https://github.com/Anujakumari github 简介:[](https://github.com/Anujakumari)**