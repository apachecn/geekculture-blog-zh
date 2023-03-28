# Jest 单元测试初学者指南

> 原文：<https://medium.com/geekculture/a-beginners-guide-to-unit-testing-with-jest-549a47edd3ea?source=collection_archive---------5----------------------->

## 开始使用 TDD 并编写自己的测试

![](img/85ca80d4bd530fef56b23b1d45b8e5da.png)

单元测试是测试驱动开发(TDD)不可或缺的一部分，TDD 是在我们开始实际的功能工作之前，定义一个功能的期望动作以及我们期望它做什么(或不做什么)的过程。以这种方式进行软件开发有很多目的:

*   该流程通过概述在职能过程中必须完成的任务，有助于定义成功之路。
*   这个过程可以帮助识别边缘情况，并确保您的代码在这些情况下继续按预期运行。
*   随着代码库的不断增长和修改，该过程还确保了对代码库其他部分的更改不会对测试功能的性能产生负面影响。

编程语言有自己的开发单元测试的框架。对于 Javascript 来说，Jest 是使用最广泛的测试框架之一，我希望这篇博客可以作为初学者指南，帮助那些希望开始编写自己的 Jest 测试的人。

我们将介绍设置基本 Jest 测试和文件的过程，但是您可以在这里查看包含所有代码的 repo

# 内容

*   [设置笑话](https://dev.to/dsasse07/a-beginner-s-guide-to-unit-testing-with-jest-128c-temp-slug-3762656?preview=2dba20ce7c9586e51298cb9d4910fab010345bd39ef973ca3fca041c76c33d5c90e1b400c2848d38ecbad924980da96fa9a292f5faf10fdd2c9f8261#setting-up-jest)
*   [确定期望的行动](https://dev.to/dsasse07/a-beginner-s-guide-to-unit-testing-with-jest-128c-temp-slug-3762656?preview=2dba20ce7c9586e51298cb9d4910fab010345bd39ef973ca3fca041c76c33d5c90e1b400c2848d38ecbad924980da96fa9a292f5faf10fdd2c9f8261#identify-desired-actions)
*   [初始化测试文件](https://dev.to/dsasse07/a-beginner-s-guide-to-unit-testing-with-jest-128c-temp-slug-3762656?preview=2dba20ce7c9586e51298cb9d4910fab010345bd39ef973ca3fca041c76c33d5c90e1b400c2848d38ecbad924980da96fa9a292f5faf10fdd2c9f8261#initializing-the-test-file)
*   [写作测试](https://dev.to/dsasse07/a-beginner-s-guide-to-unit-testing-with-jest-128c-temp-slug-3762656?preview=2dba20ce7c9586e51298cb9d4910fab010345bd39ef973ca3fca041c76c33d5c90e1b400c2848d38ecbad924980da96fa9a292f5faf10fdd2c9f8261#writing-tests)
*   [运行测试](https://dev.to/dsasse07/a-beginner-s-guide-to-unit-testing-with-jest-128c-temp-slug-3762656?preview=2dba20ce7c9586e51298cb9d4910fab010345bd39ef973ca3fca041c76c33d5c90e1b400c2848d38ecbad924980da96fa9a292f5faf10fdd2c9f8261#running-the-tests)
*   [编写函数](https://dev.to/dsasse07/a-beginner-s-guide-to-unit-testing-with-jest-128c-temp-slug-3762656?preview=2dba20ce7c9586e51298cb9d4910fab010345bd39ef973ca3fca041c76c33d5c90e1b400c2848d38ecbad924980da96fa9a292f5faf10fdd2c9f8261#writing-the-functions)
*   [结论](https://dev.to/dsasse07/a-beginner-s-guide-to-unit-testing-with-jest-128c-temp-slug-3762656?preview=2dba20ce7c9586e51298cb9d4910fab010345bd39ef973ca3fca041c76c33d5c90e1b400c2848d38ecbad924980da96fa9a292f5faf10fdd2c9f8261#conclusion)
*   [资源](https://dev.to/dsasse07/a-beginner-s-guide-to-unit-testing-with-jest-128c-temp-slug-3762656?preview=2dba20ce7c9586e51298cb9d4910fab010345bd39ef973ca3fca041c76c33d5c90e1b400c2848d38ecbad924980da96fa9a292f5faf10fdd2c9f8261#resources)

# 设置笑话

**步骤:**

*   创建一个新的目录，并`cd`进入那个目录。
*   设置 NPM 环境

```
mkdir jest-example && cd jest-example 
npm init -y
```

*   安装 Jest

```
npm i jest --save-dev
```

*   通过修改前面创建的`package.json`文件，将 NPM 环境配置为使用 Jest。这个编辑将导致命令`npm test`运行我们将要构建的测试。

```
// In package.json
"scripts": {
  "test": "jest"
}
```

# 确定期望的行动

为了开始编写测试，我们必须定义我们将要构建的函数*应该*做什么，以及当函数被调用时*预期的*结果应该是什么。

对于我们的示例，让我们考虑一个包含用户博客帖子信息的对象:

```
const user = {
    username: "user1",
    blogs: [
      {
        title: "Entry 1"
        likes: 130,
        content: "Blog 1 Content..."
      },
      {
        title: "Entry 2"
        likes: 100,
        content: "Blog 2 Content..."
      }
    ]
  }
```

我们将编写两个函数，

*   `getTotalLikes`为了获得给定用户帖子的总点赞数，
*   `getMostPopularBlog`返回指定用户最喜欢的博客对象。

遵循 TDD 过程，我们将在为功能本身制定逻辑之前为这些功能开发测试。

# 初始化测试文件

通常，测试被写在应用程序的`tests`或`__tests__`子目录中，我们将遵循同样的惯例。从我们的示例项目的根目录，让我们创建一个`tests`目录和包含我们的测试的文件。

```
mkdir tests && cd tests && touch exampleFunctions.test.js
```

在这个新文件中，我们必须做的第一件事是导入我们将要测试的函数(没关系，它们还没有被编写。)为了这篇博客，我们将把两个示例函数写入同一个`.js`文件，并且我们将在导入中使用析构来访问这两个函数。

```
// jest-example/tests/exampleFunctions.test.js
const { getTotalLikes, getMostPopularBlog } = require('../exampleFunctions')
```

上面讨论的两个示例函数都将使用前面提到的同一个 sample `user`对象进行测试，所以我们也可以为我们的测试文件全局地定义它。

```
// jest-example/tests/exampleFunctions.test.js
const { getTotalLikes, getMostPopularBlog } = require('../exampleFunctions')
const user = {
    username: "user1",
    blogs: [
      {
        title: "Entry 1"
        likes: 130,
        content: "Blog 1 Content..."
      },
      {
        title: "Entry 2"
        likes: 100,
        content: "Blog 2 Content..."
      }
    ]
  }
```

# 写作测试

测试通常包含以下常规组件:

*   调用一个接受两个参数的`describe`函数:
*   一个字符串(测试运行时将出现在终端中的描述，它“描述”了测试块)
*   包含单个测试的回调函数..
*   一个(或多个)`test`函数接受两个参数:
*   描述特定测试操作的字符串，
*   一个回调函数，包含一个`expect`函数和一个`matcher`函数。
*   `expect`函数接受被测试的函数调用，并链接到描述预期结果的`matcher`。

在`getTotalLikes`函数中，我们**期望**当函数被传递给一个用户对象时，返回值**将是一个整数**，它是该用户所有博客上的`likes`的总和。将它包含到我们的测试文件中会是这样的:

```
// jest-example/tests/exampleFunctions.test.js
const { getTotalLikes, getMostPopularBlog } = require('../exampleFunctions')
const user = {
    username: "user1",
    blogs: [
      {
        title: "Entry 1",
        likes: 130,
        content: "Blog 1 Content..."
      },
      {
        title: "Entry 2",
        likes: 100,
        content: "Blog 2 Content..."
      }
    ]
  }describe('getTotalLikes', () => {
  test('should return the total likes of a user', () => {
    expect( getTotalLikes(user) ).toBe(230)
  })
})
```

这里，`.toBe`匹配器用于定义在前面的`expect`语句中编写的函数调用的预期输出。如果函数的输出等于传递给匹配器的值，则`.toBe`匹配器返回真值。Jest 框架有许多已定义的匹配器，例如:

*   `toBeNull`仅匹配空值
*   `toBeUndefined`仅匹配未定义的
*   `toBeDefined`是 toBeUndefined 的反义词
*   `toBeTruthy`匹配 if 语句视为真的任何内容
*   `toBeFalsy`匹配 if 语句视为假的任何内容
*   `toBeGreaterThan`或`toBeLessThan`用于数值比较
*   `toMatch`接受正则表达式模式来匹配字符串输出
*   `toContain`可以用来查看一个数组中是否包含一个值

更多常见的笑话匹配器可以在[官方介绍中找到](https://jestjs.io/docs/using-matchers)或者完整的列表可以在[官方文档中找到](https://jestjs.io/docs/expect)

对于我们的第二个函数，我们可以在`describe`块的范围内定义期望的输出对象，并将该对象传递给我们的匹配器。这样做，我们将再次检查是否相等；然而，当处理对象时，我们必须使用`.toEqual`来代替，它遍历对象的所有值来检查相等性。

考虑到这一点，我们必须将最后的`describe`块添加到我们的测试文件中:

```
describe('getMostPopularBlog', () => {
  test('should return the most popular blog of a user', () => {
    const output = {
        title: "Entry 1",
        likes: 130,
        content: "Blog 1 Content..."
    }
    expect( getMostPopularBlog(user) ).toEqual(output)
  })
})
```

# 运行测试

我们编写的测试显然会失败，因为我们还没有编写函数；但是，我们可以运行测试来确保它们被正确设置。

要运行测试，运行`npm test`(它匹配我们在`package.json`中定义的命令)。我们惊奇地发现，我们的函数没有被定义，这表明我们的测试文件已经准备好了。

```
FAIL  tests/exampleFunctions.test.js
  getTotalLikes
    ✕ should return the total likes of a user (1 ms)
  getMostPopularBlog
    ✕ should return the most popular blog of a user ● getTotalLikes › should return the total likes of a user TypeError: getTotalLikes is not a function
```

# 编写函数

在`/jest-example`中创建一个包含我们函数的新文件。文件名应该与测试文件的文件名相匹配，减去扩展名`.test`。

在`/jest-example`

```
touch exampleFunctions.js
```

在这个文件中，我们需要定义两个函数，并确保我们导出这些函数，以便我们的测试文件可以访问它们。

```
function getTotalLikes(user){}
function getMostPopularBlog( user){}
module.exports = { getTotalLikes, getMostPopularBlog }
```

如果我们保存并再次运行我们的测试，我们将会看到所有四个测试仍然失败(这是意料之中的)，但是 Jest 向我们提供了一个 ne 消息，表明发生了什么。

```
getTotalLikes
    ✕ should return the total likes of a user (3 ms)
  getMostPopularBlog
    ✕ should return the most popular blog of a user (1 ms) ● getTotalLikes › should return the total likes of a user expect(received).toBe(expected) // Object.is equality Expected: 230
    Received: undefined
```

这条消息表明我们的测试能够找到匹配的函数，与以前不同，但是现在不是获得传递给`matcher`的预期值，而是没有从我们的函数返回任何值。让我们实现这两个函数的逻辑，如下所示:

```
function getTotalLikes( user ){
  // iterate through the blog entries and sum the like values
  const totalLikes = user.blogs.reduce( (total, blog) => {
    return total += blog.likes
  }, 0) return totalLikes
}function getMostPopularBlog( user ){
  // Iterate through users blogs, and update the tracking object to
  // continually have the index of the blog with most likes, and the 
  // number of likes for comparison
  const maxLikes = user.blogs.reduce( (max, blog, index) => {
      if (blog.likes > max.likes) {
        return {
          index: index, 
          likes: blog.likes
        }
      } else {
        return max
      }
  }, {index: undefined, likes: 0} ) //Select get the blog object by looking up the index stored in the tracker
  const topBlog = user.blogs[ maxLikes.index ]
  return topBlog
}module.exports = { getTotalLikes, getMostPopularBlog }
```

现在，如果我们最后一次运行测试，迎接我们的是通过指标:

```
PASS  tests/exampleFunctions.test.js
  getTotalLikes
    ✓ should return the total likes of a user (1 ms)
  getMostPopularBlog
    ✓ should return the most popular blog of a user (1 ms)Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        0.713 s, estimated 1 s
```

# 结论

测试是强大的。即使有这些有限的测试，我们也将能够看到开发过程中进一步的变化是否会对我们已经完成的工作产生负面影响。例如，如果我们用来构建`user`对象的 API 响应的结构发生了变化，那么运行测试文件将会在变化生效之前指出一个问题。这在开发团队中尤其重要，在开发团队中，多个开发人员在同一个代码库上工作。测试有助于确保新代码与代码库和其他开发人员的代码库保持兼容和功能。

然而，测试的可靠性和能力受到测试场景的全面性的限制。在构建测试时，记得考虑可能破坏应用程序功能的边缘情况，并编写测试来模拟这些情况。例如:

*   如果找不到用户，我们会期望发生什么？
*   如果两个帖子有相同的赞数，预期的行为是什么？
*   如果用户没有博客，预期的行为是什么？

测试的主题非常深入，但是希望这能帮助你开始理解测试过程和开发你自己的测试。

# 资源:

*   [玩笑入门](https://jestjs.io/docs/getting-started)
*   [常见笑话撮合者](https://jestjs.io/docs/using-matchers)
*   [笑话文档](https://jestjs.io/docs/expect)
*   [笑话教程](https://www.valentinog.com/blog/jest/)
*   [博客回购](https://github.com/dsasse07/jest-example)