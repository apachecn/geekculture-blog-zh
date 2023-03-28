# 在 WebdrioverIO 测试期间捕获详细信息和屏幕截图

> 原文：<https://medium.com/geekculture/capturing-details-and-screenshots-during-a-webdrioverio-test-2f4cc5879867?source=collection_archive---------12----------------------->

![](img/47511721fce1519ce20190367bb8d052.png)

**问题**

在您的项目中，您可能想要在一个文件中捕获每个已执行测试的指定操作。捕捉日期和时间以及名称将是识别每个测试用例的好方法，以便将来进行验证。此外，问题的一部分是，在执行时，如果测试失败，然后捕捉一个截图。

**一种解决方案**

一种解决方案是在执行周期中触发定制动作。自动化工具通常支持这些特性。WDIO 测试运行器允许在测试生命周期的特定时间触发挂钩。钩子是可以在执行周期的不同点运行的代码块。这允许自定义操作，比如在测试之前和之后捕获日期和时间以及名称。如果测试失败，可以使用相同的钩子来捕获屏幕截图。

解释从 webdriverIO 安装开始的解决方案的端到端流程的示例。

WebdriverIO 提供了一个单行命令来创建一个新的 WebdriverIO 项目。可在 macOS、Windows 和 Linux 上运行。

```
% npx create-wdio ./e2e
```

`npx`是一个命令行界面(CLI)工具，用于安装和管理托管在节点程序包管理器(`npm`)注册表中的依赖项。您将需要一个节点安装来成功运行`npx`命令。

```
% which npx% npx -v
```

各种支持命令可用上面的命令将显示当前位置的位置和`npx`命令的版本。

```
% npm install -g npx
```

默认情况下，安装`npm`时会出现`npx`命令，如果`npx`不可用，安装命令就派上用场了。

既然我们已经达成了共识`npx`让我们前往`create-wdio`指挥部。这个命令将执行一些任务，它将从创建一个`package.json`文件开始。如果当前目录中不存在该 JSON 文件。`package.json`是一个 JSON 文件，存在于 JavaScript/TypeScript 项目的根目录下。以最简单的形式，这个 JSON 文件保存了与管理项目依赖项相关的元数据。在下一阶段，将出现一个交互式向导，允许定制安装 WebdriverIO 软件包。

```
? Where is your automation backend located? On my local machine
? Which framework **do** you want to use? mocha
? Do you want to use a compiler? TypeScript (https://www.typescriptlang.org/)
? Where are your test specs located? ./test/specs/**/*.ts
? Do you want WebdriverIO to autogenerate some test files? Yes
? Do you want to use page objects (https://martinfowler.com/bliki/PageObject.html)? Yes
? Where are your page objects located? ./test/pageobjects/**/*.ts
? Which reporter **do** you want to use? spec
? Do you want to add a plugin to your test setup? 
? Do you want to add a service to your test setup? chromedriver
? What is the base url? [http://localhost](http://localhost)
? Do you want me to run `npm install` (Y/n) Yes
```

通过交互式向导安装的 wdio 软件包有

1.  *Local-runner* :-用最少的努力在本地环境中开始测试 Local-runner 是 WebdriverIO 解决方案。
2.  *Mocha-framework:*-web drivero 提供对各种测试框架的支持。Mocha 就是这样一个功能丰富的测试框架，运行在 Node.js 上和浏览器中，使得异步测试变得简单而有趣。
3.  就像框架一样，WebdriverIO 的强大之处在于支持各种报告工具。Spec-reporter 是一个 WebdriverIO 插件，以可读的语句表示结果，即 Spec 风格。
4.  *Chromedriver-service*:-包装 chrome driver，以便在使用 WDIO 测试运行器运行测试时无缝运行。这项服务不需要 Selenium 服务器，而是使用 ChromeDriver 直接与浏览器通信。因此也安装了 ChromeDriver。谈到 ChromeDriver，一个常见的问题是没有发现或者有时在 chrome 的支持版本和实际版本之间存在差异。这些错误消息中的大多数都是不言自明的。如果因为任何原因需要知道 chrome 的安装位置。项目的 ChromeDriver 二进制文件可以在位置`/node_modules/chromedriver/lib/chromedriver/chromedriver`找到
5.  *Ts-node* :-一个 typescript 执行引擎和 Node.js 的 read-evaluate-print 循环(REPL)，在这个过程中还会安装 TypeScript。Ts-node just in time 将 TypeScript 转换为 JavaScript，支持在 Node.js 上直接执行 TypeScript 而无需预编译。在每个项目的基础上设置 TypeScript 可以让您拥有许多项目，这些项目具有许多不同版本的 TypeScript。还更新了与支持 TypeScript 相关的包。

根据交互式向导完成安装后，将创建配置文件并添加脚本。并且在控制台中出现 WebdriverIO 项目完成消息的设置。既然我们已经在`e2e`里面建立了项目，wdio 建议改变目录和命令来执行第一个测试。

由于我们已经选择了自动生成一些测试文件，我们可以通过指向刚刚创建的 WebdriverIO 配置来启动测试执行。配置文件指定了应该运行的测试文件。这些规范被定义为一组规范文件，并在同一个或单独的工作进程中运行。为了让一组规范文件在同一个工作进程中运行，只需将它们放在规范数组中的一个数组中。该模式与调用“wdio”的目录相关。

```
e2e % cd e2e 
e2e % npm run wdio
```

`npm`，run wdio 从软件包的`scripts`对象运行任意命令。默认脚本将指向 WebdriverIO 配置。如果没有提供`command`，它将列出可用的脚本。

```
​​”wdio”: “wdio run test/wdio.conf.ts”
```

如果您试图在没有`node_modules`目录的情况下运行一个脚本，并且命令失败，那么您将会得到对`run npm install`的警告。

现在我们已经看到了用一个强大的命令安装 WebdriverIO 是多么容易。让我们看看如何完成手头问题的解决方案。正如在解决方案部分已经提到的，WebdriverIO 提供了几个钩子来干扰测试过程，以增强钩子的使用并围绕钩子构建服务。挂钩可以是一个函数，也可以是一组方法。挂钩具有特定于测试套件或测试生命周期的参数。`wdio.conf.ts`是所有钩子都能找到的地方。尽管我们已经选择了设置 wdio 项目的所有选项，但当我们打开`wdio.conf.ts`文件时，仍然有一个问题。这可以通过删除类型的 import 语句轻松解决。

```
import type { Options } from ‘@wdio/types’
```

由于删除了类型的导入语句，导出语句必须更新如下。

```
**export** **const** config = {
```

通过挂钩`beforeTest`和`afterTest`，可以轻松捕捉每次测试前后的姓名、日期和时间等细节。钩子`afterTest`也可以被重用来捕获失败测试的截图。

*测试前*[](https://webdriver.io/docs/options/#beforetest)

是在测试前执行的功能，只能在摩卡/茉莉中使用。cucumber 框架不能依赖这个函数。

```
beforeTest: **function** (test, context) {
 displayDateAndTimeWithTitle(test, “Entry”);
 },
```

`displayDateAndTimeWithTitle()`是一个自定义函数，用于捕获文件中的测试细节。配置不变后，自定义函数放在`wdio.conf.ts`文件中。将指定数据异步更新到外部文件由`fsPromises.writeFile()`方法负责。默认情况下，如果文件存在，它将被替换。该方法接受三个参数，即文件名、将被写入文件的数据以及最后将影响输出的可选参数。这个方法返回一个承诺，WebdriverIO 将等待，直到这个承诺得到解决，然后继续。函数`beforeTest`的测试参数提供了当前执行测试的标题。从`new Date()`中提取指定格式的日期和时间

```
**const** fsPromises = require(‘fs’).promises;
**const** RESULTS_FOLDER = “screenshots”;
**function** **displayDateAndTimeWithTitle**(test, stage) {
 **const** today = **new** Date();
 fsPromises.writeFile(
 `${RESULTS_FOLDER}/data.txt`,
 `${today.getDate()}-${today.getMonth() + 1}-${today.getFullYear().toString().slice(-2)} ${today.getHours()}:${today.getMinutes()}:${today.getSeconds()} ${stage}: ${test.title}\n`,
 { flag: “a+” }
 );
 }
```

*事后*[](https://webdriver.io/docs/options/#aftertest)

在 Mocha/Jasmine 框架中测试结束后要执行的函数。除了捕获测试的细节，这个函数还检查执行结果，并在失败时捕获屏幕截图。

```
afterTest: **function** (
 test,
 context,
 { error, result, duration, passed, retries }
 ) {
 displayDateAndTimeWithTitle(test, “Exit”);
 **const** f = **async** () => {
 **if** (passed === **false**) {
 **await** browser.saveScreenshot(`${RESULTS_FOLDER}/${test.title}.png`);
 }
 };
 **return** f();
 },
```

方法`saveScreenshot()`保存当前浏览上下文的截图。在 Firefox 中使用 Geckodriver 时，`saveScreenshot()`方法对整个文档进行截图。在 Chromedriver 和 Chrome 一起使用的情况下，它只捕获当前的视口。

我打算突然停止听到，因为这已经成为一个长期的职位，因为我已经花了一些时间通过初始设置。下一次，我将把这篇文章作为参考文章，并写一些完成特定任务的短文。