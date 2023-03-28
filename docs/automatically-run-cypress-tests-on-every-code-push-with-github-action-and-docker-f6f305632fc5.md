# 使用 GitHub Action 和 Docker 在每次代码推送时自动运行 Cypress 测试

> 原文：<https://medium.com/geekculture/automatically-run-cypress-tests-on-every-code-push-with-github-action-and-docker-f6f305632fc5?source=collection_archive---------5----------------------->

![](img/d7e88c80fc69a964b48f8bb40cf0648d.png)

自从我在 Amazon Mechanical Turk 上发布了用于众包图像分割注释的 Turkey [知识库](https://github.com/yanfengliu/turkey)以来，我一直在思考如何使用软件工程的最佳实践来不断改进它。我最近实现的一个关键改进是让 GitHub repo 在每次代码推送时运行完整的系统测试。它是完全自动化的、一致的、不受环境限制的、免费的。

# 1.火鸡

我正在测试的产品叫做`Turkey`。这是一个 HTML 文件，你可以粘贴到亚马逊机械土耳其人作为一个自定义的工作。它允许在同一作业中众包图像注释数据，如实例分段、边界框和线条。它支持撤消、重做、放大、缩小、拖动和重置。我在大学时做了这个，因为没有一个好的免费的在线图像注释众包解决方案，我们需要一个研究项目。

# 2.柏树

Cypress 非常适合系统测试。我计划单独写一篇关于编写好的 Cypress 测试的文章。就本文的范围而言，我将只谈论我使用它的目的。随着`Turkey`功能的扩展，很难跟踪它做的所有事情。手动可靠地测试所有特性更加困难。Cypress 以编程方式模拟点击、拖动和输入。用户在使用我的产品时通常会做的所有事情。由于图像注释工具的性质，仅仅知道 Cypress 找到了渲染组件并与它们成功交互是不够的。我还需要确保图形用户界面(GUI)看起来完全符合我的预期。所以我用一个名为`cypress-image-snapshot` ( [npm 链接](https://www.npmjs.com/package/cypress-image-snapshot))的 Cypress 插件包截图，并与存储版本进行对比。`Turkey`的典型测试如下所示:

命令`matchImageSnapshot`将只对`cy.get("canvas")`返回的内容进行截图，将其与存储的同名截图进行比较，并检查不同像素的百分比。如果百分比高于可定制的阈值，则失败。如果没有以该名称存储的截图，那么它会将新捕获的截图存储为正确答案。

要运行测试，首先需要在 localhost 上服务网站，然后运行`cypress run`。它将自动运行在 cypress 文件夹中找到的所有测试。您也可以使用`cypress open`来观看它的运行，但是要注意，除非被覆盖，否则这种模式可能会使用与 headless 模式不同的视口。分辨率差异将导致快照比较失败。

# 3.GitHub 操作

GitHub 在 2018 年发布了他们的动作功能。很高兴在 GitHub 上看到对 CI/CD 的原生支持。通过在 repo 中指定一个`\\.github\\workflows\\github-actions.yml`文件，GitHub 会自动选择动作定义。然后，每次请求拉或推时都会触发该操作。Cypress 现在有了他们官方的 GitHub 动作，选项[相当令人印象深刻](https://github.com/cypress-io/github-action)。所以我开始尝试最基本的版本:

让我们一行一行地检查文件:

*   `name: Turkey end-to-end tests chrome headless`:动作名称。除了帮助您记住它的作用之外，不会影响任何功能
*   `on: [push]`:此动作每隔`git push`触发一次
*   `jobs:`定义触发动作时运行的内容。
*   这也只是工作的一个名字。您可以定义多个作业。默认情况下，作业并行运行，但也可以按顺序运行。因此，在我们的例子中，我们可以运行多个作业，每个支持的浏览器一个，或者设置略有不同。
*   `runs-on: ubuntu-20.04`:动作运行的操作系统。事实证明，这将对我们的系统测试结果产生影响。稍后会详细介绍。
*   `steps`:工作步骤。这些按顺序运行。
*   `- uses: actions/checkout@v2`:使用版本 2 的社区操作，将您的回购下载到主机。
*   `- uses: cypress-io/github-action@v2`:使用运行作业的 cypress 动作。它运行`cypress run`。注意，要使其工作，repo 中必须有一个`package.json`文件，并且在其中有一个`cypress`作为依赖项。
*   `with:`我们传递给 cypress 动作的额外设置
*   `start: npm start`:在运行`cypress run`之前，我们先运行`npm start`开始托管网站
*   `wait-on: "<http://localhost:5000>"`:等到网站上线
*   `browser: chrome`:我们在 chrome 上运行测试，不管 cypress 软件包附带的是什么版本。
*   `headless: true`:我们使用无头模式是为了和我们在本地做的事情保持一致，也是因为我们无论如何都看不到它

我推送代码，看到动作运行，大声欢呼，看到测试失败。哪里出了问题？为了能够调试这个，我需要 GitHub 来保存工件，以便我可以下载截图 diffs 并亲自查看。所以在最后对 yml 文件做一点补充:

这告诉 GitHub，如果测试失败，可以使用`cypress-screenshots`文件夹中的任何内容。它还会一直提供 cypress 视频，每次测试运行时都会保存下来。所以我下载了这些差异，它们看起来像这样:

![](img/967087f10f370575db36634bec44eb3c.png)

前后看起来和人眼一模一样，那么这是怎么回事呢？事实证明，操作系统之间的渲染是非常不同的。当测试在本地运行时，它在我的 Windows 10 机器上，但在 GitHub Actions 上，它在 Ubuntu 上。网站在不同的浏览器或机器上看起来也不同，即使它们运行在相同的操作系统上。物理监视器也会影响用于渲染的颜色配置文件。因此，唯一绝对确定的方法是在一致、隔离的环境中运行测试。你猜对了——我们需要码头工人。

# 4.码头工人

Cypress GitHub 动作本身支持 Docker，并且 Cypress 也在 Docker Hub 上托管了他们的官方 Docker 映像，所以只需一行代码，我们就可以非常容易地运行 Cypress 测试的 Docker 版本:

这与其余的定义配合得出奇地好。工件仍然被上传，即使它们是由 Docker 中运行的测试生成的。现在的重点是让相同的设置在本地工作。

事实证明，我们不必使用《奔跑吧》所处的确切环境。我们只需要 Docker 默认使用的 Linux 环境。我们在`cypress/included:8.0.0`的基础上编写了自己的`Dockerfile`，并添加了我们在本地托管网站时使用的其他依赖项:

`package.json`中的脚本如下所示:

所以`npm run test:chrome`启动网站，等待它在端口 5000 上准备好，然后在 chrome 上运行测试。我们清空了`cypress/screenshots`文件夹，这样第一次运行将会生成基本事实截图，然后我们将它们推送到回购上。这一次，测试通过了。当一切最终都运转正常时，这是一种非常令人满意的感觉。

# 参考

*   [https://github.com/yanfengliu/turkey](https://github.com/yanfengliu/turkey)
*   [https://docs . cypress . io/guides/continuous-integration/github-actions](https://docs.cypress.io/guides/continuous-integration/github-actions)
*   https://docs.github.com/en/actions

(这是我最初的想法[链接](https://yanfeng.notion.site/Automatically-run-Cypress-tests-on-every-code-push-with-GitHub-Action-and-Docker-6283469aa9414540b982ff25156a213f)为了更好的阅读体验)