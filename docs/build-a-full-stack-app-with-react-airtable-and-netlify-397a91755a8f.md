# 利用 React、Airtable 和 Netlify 构建全栈应用

> 原文：<https://medium.com/geekculture/build-a-full-stack-app-with-react-airtable-and-netlify-397a91755a8f?source=collection_archive---------6----------------------->

![](img/c9bda24ea440a6681b7525087fe47bb1.png)

XPS

在本帖中，我们将使用 React、Airtable 和 Netlify 无服务器函数构建一个课程跟踪器。它将有完整的 CRUD 操作，并将跟踪所有的课程，我们已经和我们希望在未来购买的课程。

我们要做的第一件事是进入 Airtable 并建立我们的数据库。在 Airtable 中注册后，单击**添加基本信息**图标。它将打开一个菜单，并在其中点击**从头开始**按钮。

![](img/1eb76ff268f23ef97b0afad982e20bb3.png)

Base

它将打开一个弹出窗口，在其中输入数据库名称，我已经输入了**航向跟踪器**并按下回车键。

![](img/362f15a40793fc350770446538340f6c.png)

Course Tracker

我们将重命名它附带的默认四列。因此，首先点击名称栏中的小箭头，它将打开一个菜单。点击其中的**自定义字段类型**链接。

![](img/cffc2bbc756914db2eae457a4143cfc2.png)

Customize

这将打开一个弹出窗口，我们需要在文本字段中给出**名称**，并点击**保存**按钮。

![](img/80bf4343cb3641c091a228483079662a.png)

Field

接下来，将**注释**改为**链接**和一个**单行文本**。之后点击**保存**按钮。

![](img/b88dc62ec0a7a65a4f44ad1863b09be8.png)

Save

接下来，我们将添加**标签**，这将是**多选**。在这里，点击**添加一个选项**,为您想要参加的课程添加各种标签。

![](img/205ac81a04259ec78f841712c90f2c4b.png)

tags

最后会是**购买**，会是**复选框**。

![](img/4904214313dde715d1e6eb6c956e6035.png)

Checkbox

接下来，在 excel 表格中添加您已经参加或想要参加的任何课程。此外，我还通过右键单击它并更改名称，将它重命名为 courses。

![](img/2481a6fc735b3f376543a4819590ed64.png)

Table

接下来，我们需要获取我们的 API 密钥，因此请转到[https://airtable.com/api](https://airtable.com/api)，我们可以在这里看到新项目。

![](img/08e83da09fbeb23aaefd0b99cb7432f6.png)

Course tracker

点击**课程跟踪器**数据库，我们将被带到另一个页面，在那里我们需要首先记下基本 ID。单击右上方的 show API 键，我们可以获得 API 键。

![](img/8c308ee1737e3c7a10385a425db311c8.png)

Base id

最后，是时候创建我们的 React 项目了。因此，用下面的命令创建一个新项目。

```
npx create-react-app course-tracker
```

更改目录后，用 VS 代码打开。我们也在安装**气动工作台**、 **dotenv** 和**书夹 4** 的软件包。

之后创建一个**。env** 文件，并放入前面步骤中的基本 ID 和 API 键。另外，输入表名，在我们的例子中是 courses。

![](img/70bb0415ed5d3c5343766c6aa84b7a5b.png)

.env

接下来，我们将把引导程序添加到我们的 **index.js** 文件中。

![](img/713c10c0c4a46a367b75a4b2746bac09.png)

index.js

接下来，更新 **App.js** 来呈现一个 **CourseForm** 组件。我们正在传递一个道具 **courseAdded** 给它，这基本上是一个回调函数。

![](img/0d2d1eef84ce9b438e93ba15106e0983.png)

App.js

在 **src** 文件夹中创建一个 **components** 文件夹，并在其中创建一个文件 **CourseForm.js** 。这里，我们基本上有两个输入字段，分别是**名称**和**链接**，当我们提交表单时，将调用一个函数 **submitCourse** 。

```
import React, { useState } from 'react';export default function CourseForm({ courseAdded }) {
    const [name, setName] = useState('');
    const [link, setLink] = useState('');
    const submitCourse = () => {};return (
        <div className="card">
            <div className="card-header">Add a New Course</div>
            <div className="card-body">
                <form className="" onSubmit={submitCourse}>
                    <div className="form-group">
                        <label htmlFor="name">Name</label>
                        <input
                            type="text"
                            name="name"
                            value={name}
                            className="form-control"
                            onChange={(e) => setName(e.target.value)}
                        />
                    </div>
                    <div className="form-group">
                        <label htmlFor="link">Link</label>
                        <input
                            type="text"
                            name="link"
                            value={link}
                            className="form-control"
                            onChange={(e) => setLink(e.target.value)}
                        />
                    </div>
                    <button type="submit" className="btn btn-primary">
                        Submit
                    </button>
                </form>
            </div>
        </div>
    );
}
```

现在，当您通过 **npm start** 启动 react 项目时，它将在 localhost 中显示表单。

![](img/4065532b4a2ac4a92ea9b686891a0b90.png)

localhost

接下来，我们将从 **CourseForm.js** 文件渲染**标签**组件。这里，我们正在传递更新的标签和**钥匙**道具。

![](img/e3e8c0cff080b3117171882fb1690d09.png)

CourseForm.js

接下来，在**组件**文件夹中创建一个文件 **Tags.js** 。在这里，我们展示了我们的标签，我们已经把它放在了飞行表中。在完成后端之后，我们将再次回到这个文件。

![](img/b90d7992aabe47f07f91e1654ee3212f.png)

Tags.js

现在，我们将创建**课程列表**组件。因此，我们将首先从 **App.js** 文件中渲染它，并将两个道具**课程**和**刷新课程**传递给它。

![](img/e1b1a4968cb7335d014d53d14a8cb99c.png)

App.js

接下来，在**组件**文件夹中创建一个文件 **CourseList.js** 。这里，我们根据购买的课程进行过滤，并将其传递给另一个组件**课程**。

![](img/73617bcd5ee7d6ef584b32fa5073abba.png)

CourseList.js

现在，在同一个**组件**文件夹中创建一个文件 **Course.js** 。在其中添加下面的代码。在这里，我们只是显示个别课程的细节。

![](img/ab208ff71c25cb6fe341e3a66c3ab29d.png)

Course.js

现在，是时候去后端了，后端是 netlify 内部的无服务器功能。为此，我们必须在根目录下创建一个 **functions** 目录，并在其中创建一个名为 **courses.js** 的 JavaScript 文件。

这个文件有一个处理程序，并且正在使用一些文件来执行 CRUD 操作。

![](img/5a883614fb44c9261e93059384032501.png)

courses.js

接下来，在根目录下创建一个文件 **netlify.toml** 并将以下内容放入其中。这是为了告诉 netlify，你的无服务器函数在哪里。

另外，在**函数**目录下创建了一个**帮助器**文件夹并放了五个文件，要求放在 **courses.js** 里面。

![](img/3ce4507985cf6397b0af260620df1cd9.png)

nelify.toml

接下来，我们需要安装 netlify cli，全局使用下面的命令。

```
npm install netlify-cli -g
```

现在，在**助手**文件夹中创建一个文件 **airtable.js** ，并在其中添加以下内容。这里，我们从环境文件中访问库和表。

![](img/57c561ea8e6bb1478678ea077d19dde0.png)

airtable.js

现在，我们将为 **formattedReturn.js** 文件编写代码。在这里，我们只是在处理尸体。

![](img/11187d23825d9a9d4293bcc0f40e695f.png)

formattedReturn.js

现在，是时候创建 **getCourses.js** 文件了。这里，我们导入 aie 表和 formattedReturn，然后从表中选择第一页。之后，我们通过 formattedReturn 返回它，状态为 200。

如果我们遇到任何错误，我们也会以格式 Return 返回错误。

![](img/faa0a11dfcd0e48331e45fed1ff12f2c.png)

getCourses.js

现在，我们需要从终端运行下面的命令来启动与 netlify 的反应。

```
netlify dev
```

如果以上成功，我们的 React app 将运行在 [http://localhost:8888/](http://localhost:8888/) 上，与后端无服务器功能对话。

![](img/309506d2e7bc4cf4b069be99ad2c2dae.png)

React app

现在，我们的后端将在[http://localhost:8888/API/courses](http://localhost:8888/api/courses)上运行，我们可以看到来自 Airtable 的数据。

![](img/0d79adc93dfddbc417043acc7b3d0182.png)

API

这里我们只关心 id 和 fields 属性，所以我们将在我们的 **getCourses.js** 文件中格式化这个数组。

![](img/57845e023d0c67fbb1ece230dd155a2a.png)

getCourses.js

现在，当我们到达我们的 API 端点并刷新时，数据将看起来不错。

![](img/e9f5ec36389c81cab58cb8d8e5161668.png)

API endpoint

接下来，我们将完成我们的 **createCourse.js** 文件。这里，我们从 body 中提取字段，并使用 table.create 在 airtable 中创建新字段。

![](img/4a42065aa9109ad555cecc579119e0c0.png)

createCourse.js

现在，我们将通过 postman 检查这个 POST 端点，因为我们的前端并不完整。这里，我们通过主体给出名称、链接和标签，并发送请求，这是成功的。

![](img/a3995e8f7f9ebaaa386f53682e7b4c82.png)

Postman

接下来，我们将编写代码来更新课程。所以，在 **updateCourse.js** 文件中放入下面的代码。这里，我们期望在请求中提供 id 和字段。

![](img/eb2a8da8cebbd0db0e9950cab468794a.png)

updateCourse.js

我们将通过 postman 再次检查它，但通过 PUT 请求并将 id 传递，我们收到了来自上一个请求的返回。

![](img/b064d084240abaf7a51ff30768799b49.png)

PUT request

我们将执行最后一个 CRUD 操作，即删除。所以，在 **deleteCourse.js** 文件中放入下面的代码。这里，我们只是使用 table.destroy 方法获取 id 并删除它。

![](img/f4bc299862ba156a7f2a884a47777fa4.png)

deleteCourse.js

接下来，在 postman 中发送一个删除请求，只有一个 id 和 hot Send。

![](img/3a3ac51880d952a538d7570e63cf27f5.png)

Postman

现在，我们有了连接到 React 前端的所有东西。首先，在我们的 **App.js** 中，我们需要完成所有的课程。这里，我们使用 fetch api 来获取课程，并将其设置为课程数组。

![](img/12e7d62a5aaa46fb11aa36fe89a8630f.png)

App.js

接下来，在 **CourseForm.js** 文件中，我们将添加代码，用表单中的所有数据发出 POST 请求。同样，我们使用 feth api 进行调用。

![](img/bb8169390eadb36e76ecc4b22ac7e79c.png)

CourseForm.js

我们之前错过的一件事是更新 **Tags.js** 文件。这里，当有人点击标签时，我们只是更新标签的值。

![](img/d9d8747930c39e12900e480ac3ae66b7.png)

Tags.js

接下来，我们必须从 localhost 提交另一门课程，所有课程都将从 airtable 中检索并显示出来。

![](img/96b6890d062b2614f21860c0071e3d96.png)

All courses

最后，我们需要完成从 **Course.js** 文件中购买和删除的功能。它们是简单的 PUT 和 DELETE 请求，同样来自 fetch api。

![](img/cdfe8de3d94857a47a1a02f8a7ac235b.png)

Course.js

现在，返回到 localhost，单击“purchase ”,它将移动它。此外，删除工作正常。

![](img/fd4ccf08090811cac8af11d7715669d5.png)

POST and DELETE

接下来，我们将把代码推送到 github 并进行部署。但是首先包括。env 文件在**中。gitignore** 否则会推送到 github。

![](img/5fd2fc01ef90d31cdd798600eb5a783a.png)

.gitignore

现在，登录你的 netlify 账户，点击 Git 按钮中的**新站点**

![](img/862043a0eec87a8ef314777a97dae420.png)

Netlify console

在这个屏幕中点击 **Github** ，因为我的代码部署在 Github 中。

![](img/56fa50153111da99914f8f011ede32b4.png)

Github

因为，我有很多回购，我需要搜索回购并点击它。

![](img/69ed30b7389fddcebb730b42a5d65630.png)

Course tracker

我们需要更新目录中的构建命令，并在此屏幕中给出环境变量。

![](img/645290f28c494f018d3977e9d0256fe1.png)

Next

在点击**部署站点**时，该端得到了部署，但并没有像在 localhost 情况下那样从 **/api/courses** 中获取数据，而是从 **/中获取数据。网络/功能/课程**

![](img/4ccc4895f8159bfd6e7f7cddbb5e3975.png)

Correct endpoint

现在，在我们的代码中，我们需要更新所有对 **/api/courses** 到**的引用。netlify/functions/courses** 推送到 github，触发 netlify 中的部署。

此外，我似乎错过了 App.js 中自动加载所有现有代码的一小部分。

![](img/5b250a65de0c4443c6ff2e94d06d179a.png)

App.js

我们的应用程序现在运行良好。你可以在这里找到相同[的代码。](https://github.com/nabendu82/course-tracker)

![](img/ab0a6882c8d92c02a40e94b25048f196.png)

Working