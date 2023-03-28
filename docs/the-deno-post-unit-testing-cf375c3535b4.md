# 发现 Deno——单元测试

> 原文：<https://medium.com/geekculture/the-deno-post-unit-testing-cf375c3535b4?source=collection_archive---------17----------------------->

![](img/a5676a5641d31479da9fe9f44182f4a4.png)

Image Credit: Dimitrij Agal

Deno 是 2020 年发布的 JavaScript 和 TypeScript 的新运行时，专注于安全性、可靠性和高效开发。因为它相对较新，而且许多节点模块不能直接用于 Deno，所以找到合适的库和框架可能是一个真正的挑战。这篇文章将介绍如何开始 Deno 中的单元测试。我们将从检查一些内置的开始，并继续研究一些正在开发的测试框架。

# 标准图书馆

Deno 的目标之一是以最少的设置提供一系列开箱即用的开发工具。因此，它提供了一个名为`testing`的模块，可以用作基本的断言库。我们可以通过它的 URL 获取这个模块:

```
import * as testing from "https://deno.land/std@0.97.0/testing/asserts.ts";
```

很多时候，我们并不是真的想要`testing`模块中的所有东西，而是只想要我们需要的东西，例如`assertEquals`:

```
import { assertEquals } from "https://deno.land/std@0.97.0/testing/asserts.ts";
```

现在我们可以在代码中使用这个断言来执行测试。假设我们有以下函数:

```
function sum(a: number, b: number): number {
    return a + b;
}
```

出于演示目的，我将这个`sum`函数保持得非常简单。在 Deno 中，我们可以选择在单独的文件中编写测试，或者在我们测试的代码旁边编写测试。在任何情况下，我们都需要使用内置的`Deno.test`方法，它有两种形式:一种是简写语法，另一种是完整语法，在完整语法中，我们向测试传递一个更具可配置性的对象。让我们从速记语法开始，为`sum`编写一个测试:

```
Deno.test("sum - should return the sum of two numbers correctly", function(): void {
    assertEquals(sum(1, 2), 3);
});
```

当使用完整表单时，我们将测试名放在`name`属性中，将测试函数放在`fn`属性中。当使用这个表单时，有更多的选项可用，例如每个测试的可配置权限，如果您想要为某些测试用例覆盖它们的话。

```
Deno.test({
    name: "sum - should return the sum of two numbers correctly",
    fn(): void {
        assertEquals(sum(1, 2), 3);
    }
});
```

对于这些基本的断言，Deno 标准库`testing`模块工作得非常好，但是为了进行更高级的测试，我们将需要一些第三方代码。

# 拉姆

如果你曾经为 Node 编写过测试，你可能有使用 Jest、Jasmine 和 Mocha 等框架的经验。Rhum 是一个非常相似风格的 Deno 测试框架。它允许您编写测试用例，并在逻辑上将它们分组到测试套件和测试计划中。从导入 Rhum 开始:

```
import { Rhum } from "https://deno.land/x/rhum@v1.1.10/mod.ts";
```

注意，我正在为这篇文章导入版本`1.1.10`，但是最新的版本可能会更高。在导入中使用显式版本控制是一个很好的做法，尽管您也可以省略它并导入最新的版本。

用`Rhum`给`sum`写个测试吧！我们可以使用`Rhum.asserts`来访问 Deno 标准库中的所有断言，并使用 Deno 测试运行程序运行它们，同时以 Rhum 自己的格式显示测试输出。我们现在可以用一个`testCase`写一个`testSuite`:

```
Rhum.testSuite("sum", () => {
    Rhum.testCase("should sum two numbers correctly", () => {
        Rhum.asserts.assertEquals(sum(1, 2), 3);
    });
});Rhum.run();
```

如果我们有更高级的功能要测试，我们可以在套件中编写所有的测试。此外，如果我们有各种以某种方式逻辑相关的功能，我们可以用`testPlan`增加一个分组级别:

```
Rhum.testPlan("utils.test.ts", () => {
    ... 
});
```

## 嘲弄

除了测试套件，Rhum 还提供了模仿对象和类的能力。这有助于在没有外部依赖的情况下测试特定的代码，这是一个好的单元测试的要素之一。

Rhum 文档提供了一个非常清晰的使用模拟的例子。下面你会发现这个例子，但略有改动。`sum`函数已经作为方法添加到类`MathService`中，然后作为依赖注入到另一个类`Calculator`中。

```
class MathService {
    sum(a: number, b: number): number {
        return a + b;
    }
}class Calculator {
    constructor(private service: MathService) {} sum(a: number, b: number): number {
        return this.service.sum(a, b);
    }
}const mock = Rhum.mock(MathService).create();const calculator = new Calculator(mock);Rhum.asserts.assertEquals(mock.calls.sum, 0);calculator.sum(1, 1);
Rhum.asserts.assertEquals(mock.calls.sum, 1);Rhum.run();
```

**挂钩**

Rhum 提供了四个钩子，可以用来在测试之前或之后运行一些代码:`beforeEach`、`beforeAll`、`afterEach`和`afterAll`。这些对于将值重置为某个状态很有用，或者，如果您计划编写集成测试，它们对于启动/停止 HTTP 服务器或测试数据库很有用。

# **测试 _ 套件**

Deno 的另一个测试选择是`test_suite`，这个模块也提供了在套件中组织测试的能力(谁会想到呢！:) )和挂钩。它处于比`Rhum`更早的开发阶段，不提供模拟，但是它采用了更接近 Deno 本地测试模块的语法和风格。下面是一个将`test_suite`与 Deno 标准库一起使用的例子:

```
import {
    test,
    TestSuite
} from "https://deno.land/x/test_suite@v0.7.0/mod.ts";
/asserts.ts";
import {
    getUser,
    resetUsers,
    User
} from "https://deno.land/x/test_suite@v0.7.0/example/user.ts";
import { assertEquals } from "https://deno.land/std@0.93.0/testinginterface UserSuiteContext {
    user: User;
}const userSuite: TestSuite<UserSuiteContext> = new TestSuite({
    name: "user",
    beforeEach(context: UserSuiteContext) {
        context.user = new User("Alice");
    },
    afterEach() {
        resetUsers();
    }
});test(userSuite, "creates a user correctly", () => {
    const user = new User("Bob");
    assertEquals(user.name, "Bob");
};
```

为了将多个测试组合在一起，`test_suite`支持使用`describe/it`语法，与许多预先存在的 JavaScript 测试框架一致。

这篇文章涵盖了 Deno 的几个单元测试解决方案:`std/testing`、`Rhum`和`test_suite`。尽管它们都还在开发中，但它们已经为 Deno 开发者提供了相当多的测试能力。与配置测试框架以使用 Node.js 中的 ES 模块和 TypeScript 的麻烦相比，Deno 体验似乎是一大进步，因为设置起来确实很容易(不需要 npm、package.json、tsconfig)。然而，像`Rhum`这样的框架并不能提供目前`Jest`所能提供的一切。尽管如此，仍然有足够的东西可以使用测试驱动开发，自信地重构，或者仅仅是产生更好质量的代码。