# 如何在 Gradle 插件中测试 POST 请求文件上传

> 原文：<https://medium.com/geekculture/wiremock-how-to-test-a-post-request-file-upload-435f60517b57?source=collection_archive---------4----------------------->

本周，我不得不创建一个 Gradle 插件，用 POST 端点上传一个 zip 文件到服务器。这个插件有更多的功能，但是上传是我遇到最多问题的部分。尤其是对上传的测试证明是很难创建的。因此，我将只向你展示这个插件的上传部分。

![](img/b2f123cc4142b7a7263b3369b67d6418.png)

# 插件

首先，我在根项目中创建了一个新的子项目，并将其命名为“document-publish”。然后我将`document-publish.gradle.kts`文件添加到项目中，并创建了插件类本身。这些文件看起来像这样:

如您所见，我们的类实现了`Plugin<Project>`接口。这样我们可以用`project`参数覆盖`apply()`函数。因为我们在上传 zip 文件之前需要一个 AsciiDoctor 插件，所以我们需要在我们的插件中应用它。相应的依赖项需要在之前的构建文件中设置。

之后，我们创建一个带有类型的自定义扩展，该类型没有在这个文件中定义，稍后我们将会看到它。

这里最后也可能是最重要的一点是，我们用`DocumentPublish`类型注册了一个新任务。这种类型暂时不存在，需要以后再创建。

`dependsOn()`函数表示每当执行 documentPublish 任务时，asciidoctor 任务将首先被触发，然后 documentPublish 任务本身将运行。它类似于对另一个任务的函数调用。

该扩展将在下一节中定义。它采用在`apply()`函数中定义的值。

## 添加逻辑

我们现在可以创建一个名为`DocumentPublish.kt` *的新文件。*这个文件包含了我们插件的逻辑，看起来像这样:

1.  首先，在文件底部创建一个属性为`url`和`id`的`PublishPluginExtension`类。
2.  创建一个类型为`PublishPluginExtension`的`lateinit`变量。不要忘记添加`@get:Input`注释。没有它就不行。
3.  用`@TaskAction`注释创建一个`publishDocumentation()`函数。这将是任务被触发时执行的函数。
4.  将 POST 请求逻辑添加到函数中，并使用文件名和 URL 的扩展名。内容类型必须是`MultiPart.FormData`，因为我们想要上传一个文件到服务器。

这个插件现在功能齐全，可以上传一个 zip 文件到服务器。但是如果我们不能正确地测试它，它就没有任何价值。

# 测试

我们将使用 WireMock 和 Gradle TestKit 来测试插件。

1.  创建一个名为`functional-test`的新子项目
2.  将一个`functional-test.gradle.kts`文件添加到项目中，并将以下依赖项添加到文件中:

3.在项目的新文件中创建一个名为`DocumentPublishPluginFunctionalTest.kt`的类。

4.将以下内容添加到文件中:

首先，我们必须用。`@WireMockTest`。然后我们用相应的 Gradle 文件创建一个新的临时 Gradle 项目。

我们可以创建一个为我们创建 Gradle runner 的函数，这样我们就不必为我们编写的每个新测试单独创建它。

我们的测试函数将 WireMockRuntimeInfo 作为一个参数，我们可以立即使用它。服务器会自动被模仿。我们唯一要做的就是为它创建一个存根并读出`httpBaseUrl`。该 URL 用于随后与被模仿的服务器进行通信。

下一步是用任务的名称调用我们之前构建的 Gradle runner。

现在我们可以验证服务器收到了具有正确的`content-type`和`filename`的请求。我们还可以遍历 post 请求，检查我们的文件是否真的是请求体的一部分。

请记住，我们必须将 ByteArray 转换成一个字节列表才能使用`containsAll()`方法。

# 反射

## 什么进展顺利

插件本身的创建非常顺利。我没有更大的问题，除了一开始我想为 HTTP 请求使用另一个插件，最后不得不切换到 Ktor。我在很短的时间内就让插件工作了，但是我仍然需要为它编写测试。我知道我们都应该在实现之前编写测试，但在这一点上，我没有知识来测试这样的 POST 请求，除了等待帮助，这是我唯一能做的事情。

## 什么需要改进

我在测试我的代码时遇到了一些问题。我真的不确定 WireMock 是否有能力进行这种测试，因为他们文档中的大多数例子都指向 GET 请求。下一次，我会投入更多的时间去发现我想用 WireMock 做的事情是否真的有可能实现。最后，我找到了一个变通办法，但我相信对于这个问题会有更好的解决方案。