# 如何在玩笑中模仿网络请求

> 原文：<https://medium.com/geekculture/how-to-mock-network-request-in-jest-40958bb8d8e2?source=collection_archive---------1----------------------->

## 测试

## 模仿网络请求更加容易

![](img/7136bb0c42fda2231a0ffbb1a99eaed7.png)

Photo by [Taylor Vick](https://unsplash.com/@tvick?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/@tvick?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

现在需要修改一个比较老的库到`TS`，进行单元测试。如果库修改为`TS`，还是有一点点好的。单元测试纯粹是一个当前的研究，现在出售。对于初学者学习`Jest`框架，我认为单元测试中比较麻烦的是测试网络请求。所以记录下`Mock`丢弃`Axios`来发起网络请求的一些方式。这是我的第 39 篇中型文章。

# 介绍

文中提到的例子都在 [jest-mock-server](https://github.com/sabesansathananthan/jest-mock-server) 存储库中。可以通过安装包管理器直接启动例子，比如通过`yarn`安装:

```
$ yarn install
```

一些命令在`package.json`中指定，如下:

*   `npm run build`:`rollup`的打包命令。
*   `npm run test:demo1`:简单来说就是`mock`封装的网络请求库。
*   `npm run test:demo2`:通过重新执行`hook`来完成`mock`。
*   `npm run test:demo3`:使用`Jest`中的库完成`demo2`的实现。
*   `npm run test:demo4-5`:启动`node`服务器，`proxy`通过`axios`的代理将网络请求转发给已启动的`node`服务器。通过设置相应的单元测试请求和响应数据，利用对应关系来实现测试，这就是`jest-mock-server`完成的工作。

这里我们封装了一层`axios`，更接近真实场景。可以查看`test/demo/wrap-request.ts`文件。事实上，它只是在内部创建一个`axios`实例，并转发响应数据。

`test/demo/index.ts`文件简单地导出了一个`counter`方法，在发起网络请求之前，这两个参数被处理到一定程度。然后对响应数据也进行一定程度的处理，只是为了模拟相关操作。

这里`Jest`使用`JSDOM`模拟的浏览器环境，启动文件`test/config/setup.js`配置在`jest.config.js`配置的`setupFiles`属性中，`JSDOM`在这里初始化。

# 演示 1:简单的模拟网络请求

简单的`mock`处理是在`test/demo1.test.js`中执行的，你可以尝试通过 npm `run test:demo1`来运行。事实上，在包装了`axios`的`wrap-request`库上执行了一个`mock`操作。`wrap-request`将在`Jest`启动时编译。在这里模拟库之后，所有后来导入到库中的文件都将得到被模拟的对象。换句话说，我们可以认为这个库被重写了，重写后的方法都是`JEST`的`Mock Functions`。您可以使用`mockReturnValue`等功能进行数据模拟。`Mock Functions`请参见此[链接](https://www.jestjs.cn/docs/mock-functions)。

这里我们完成了返回值的`Mock`，也就是说我们可以控制`wrap-request`库中的`request`返回的值。但是，前面提到过，传入参数也有特定的过程。我们还没有对这部分内容做任何断言，所以我们也需要尝试处理这一点。

# 演示 2:挂钩网络请求

`demo2`可以通过`npm run test:demo2`试运行。如上所述，我们可以处理返回值，但是没有办法断言输入参数是否被正确处理，所以我们需要处理这种情况。幸运的是，`Jest`提供了一种直接实现被嘲笑的函数库的方法。因此，`Jest`也提供了一个`mockImplementation`方法，在`demo3`中使用。这里我们重写了被模仿的函数库。我们也可以用`jest.fn`来完成`Implementations`。这里，我们在返回之前编写一个`hook`函数，然后在每个`test`中实现断言或指定返回值。这样就可以解决上面的问题，其实就是`Jest`中`Mock Functions`的`mockImplementation`的实现。

# 演示 3:使用 Jest 的模拟实现

`demo3`可以通过`npm run test:demo3`试运行。`demo2`中的例子其实写起来很复杂。在`Jest`中，`Mock Functions`有`mockImplementation`的实现，可以直接使用。

# 演示 4–5:真正发起网络请求

`demo4`和`demo5`可以通过`npm run test:demo4–5`试运行。这样，就提出了一个真实的数据请求。这里，`axios` proxy 将用于将内部数据请求转发到指定的服务器端口。因此，服务器也在本地启动，通过指定与相应的`path`相关的请求和响应数据来执行测试。如果请求的数据不正确，则相关的响应数据将不能正常匹配。所以请求会直接返回`500`。如果返回的响应数据不正确，也将在断言期间被捕获。在`jest-mock-server`库中，首先我们需要指定三个文件，分别对应每个单元测试文件在启动前要执行的三个生命周期。`Jest`测试在三个生命周期之前执行，三个生命周期在`Jest`测试完成之后执行。我们需要指定的三个文件是`jest.config.js`配置文件的`setupFiles`、`globalSetup`和`globalTeardown`配置项。

首先我们将从`setupFiles`开始。除了初始化`JSDOM`，我们还需要操作`axios`的默认代理。因为采用的解决方案是用`axios`的`proxy`来转发数据请求。因此，有必要将代理值设置在单元测试的最前面。

一旦我们在`test/config`文件夹中设置了上面的文件，那么我们需要在那里再添加两个文件，分别是`globalSetup`和`globalTeardown`。这两个文件是指在`Jest`单元测试开始之前和所有测试完成之后所执行的操作。我们将服务器启动和关闭操作放在这两个文件中。

> 请注意，这两个文件中运行的文件是一个单独独立的`*contex*`，与任何单元测试的`*contex*`无关，包括 setupFiles 配置项指定的文件。所以这里所有的数据要么在配置文件中指定，要么就是通过网络在服务器端口之间传输。

对于配置端口和域名信息，直接放在`jest.config.js`中的`globals`字段。对于`debug`配置项，建议配合`test.only`使用。

现在，可能会有人提出为什么服务器不应该在每个单元测试文件的`beforeAll`和`afterAll`生命周期中启动和关闭。因此，我尝试了这个解决方案。在这个解决方案中，对于每个测试文件，服务器启动，然后关闭。因此，这种解决方案比较耗时。但理论上，这种解决方案是合理的。毕竟，数据隔离是必要的，这是事实。但是`afterAll`关闭的时候有一个问题。它实际上并没有关闭服务器和端口占用，因为在关闭`node`服务器时会调用`close`方法。当`afterAll`关闭时，它停止处理请求，但是端口仍然被占用。当第二个单元测试文件启动时，将抛出一个异常，表明端口正在被使用。虽然我尝试了一些解决方案，但它们并不理想，因为有时端口仍然被占用。尤其是`node`开启后第一次运行时，出现异常的概率比较高。所以效果不是很理想。最终采用完全隔离方案。具体相关问题请参考此[链接](https://stackoverflow.com/questions/14626636/how-do-i-shutdown-a-node-js-https-server-immediately)。

由于我们采用了完全隔离的解决方案，当我们想要传输测试请求的请求和响应数据时，只有两种选择。这两种解决方案要么是服务器启动时所有数据都在`test/config/global-setup.js`文件中指定，要么是服务器运行时数据通过网络传输，路径被指定，路径的网络请求将携带数据，数据请求将在服务器的关闭中指定。因此，这里支持这两种选项。我觉得在每个单元测试文件中指定自己的数据比较合适，所以这里只举一个在单元测试文件中指定要测试的数据的例子。关于要测试的数据，指定了一个`DataMapper`类型，以减少由类型错误引起的异常。因此，这里以两个数据集为例。另外，匹配`query`和`data`时支持正则表达式。`DataMapper`型的结构比较标准。

在下面两个单元测试中，要测试的数据在`beforeAll`中指定。注意`beforeAll`是返回 setSuitesData(data)因为单元测试是在数据置位且响应成功后执行的，后面是正常的请求和响应断言测试是否正确。