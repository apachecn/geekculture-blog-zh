# 测试异步任务

> 原文：<https://medium.com/geekculture/testing-asynchronous-tasks-ebd0dbc310ec?source=collection_archive---------2----------------------->

![](img/7ba44102b4c40f70a339b2462a661d6c.png)

Photo by [Startup Stock Photos](https://www.pexels.com/@startup-stock-photos?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/man-wearing-black-and-white-stripe-shirt-looking-at-white-printer-papers-on-the-wall-212286/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

**学习成果**

1.  学习如何使用`XCTestExpectation`类
2.  利用`XCTestExpectation`类清理测试的源代码

## XCTestExpectation 类

当测试驾驶或者为一些任务编写测试时，我们通常有我们的给定，我们的时间，最后是我们的时间。当我们为同步任务创建测试时，这很容易描述。当我们不得不测试行为方式不同的驾驶任务时，会发生什么？如果一个任务是异步的，我们如何在测试中得到我们断言的结果？答案相当简单，苹果为我们提供了`XCTestExpectation`类。根据 Apple 文档，该类代表:

> 异步测试中的预期结果。

简而言之，这个类将帮助我们知道一个异步任务是否完成或**完成。**期望在以下情况下实现:

1.  他们会收到通知，
2.  它们接收一个用谓词成功评估的对象，
3.  他们观察一个值，直到它与期望值相匹配，或者；
4.  作为开发人员，当你明确地实现它们时(我们将在本教程中讨论这些)🙂).

`XCTestCase`类为我们提供了创建`XCTestExpectation`对象的`expectation`方法。让我们看一个例子:

```
**func** test_asyncFunction_didFulfill() {
   // Given
   //Any initialization //When
   **let** exp = expectation(description: "Wait for asycn function to complete") //Then
   //Any assertion
}
```

在上面的例子中，我们创建了一个必须明确实现的期望。为什么？因为我们只传递了`description`参数。`expectation`方法被重载以涵盖我们之前见过的实现期望的四种方式。

现在你可能想知道，显式实现是什么意思？这意味着当异步任务完成时，您将调用`fulfill`方法。这样做是为了让测试知道一个异步任务已经完成了它的执行。

```
exp.fulfill()
```

你可能也想知道:嗯，我从一个同步函数调用一个异步任务，如果我的异步任务需要一段时间会发生什么？我的测试函数可以在我的异步任务之前结束吗？如果我没有按时得到异步任务结果，我该如何做一些断言？这个问题也可以用`XCTestCase`类提供的`wait`方法来解决。这个方法将接收一个期望数组和一个以秒为单位的超时，这意味着您的测试函数将一直等待，直到您传递的期望得到满足。我们根据将要执行的任务来定义超时值。对于需要网络操作(如 URL 请求)的异步任务，可能需要 3-5 秒。

```
**func** test_asyncFunction_didFulfill() {
   **let** exp1 = 
      expectation(description: "Wait for async function to complete")
   **let** exp2 = 
      expectation(description: "Wait for another async function to complete") //Async actions call // Our function is on hold and waiting for the expectations to be   fulfilled when it reaches this line
   **wait**(for: [exp1, exp2], timeout: 2.0) //Once our expectations are fulfilled our function continues its execution to this block}
```

如果由于任何原因，我们在前面的例子中声明的期望没有实现，我们的测试将失败，我们将收到下一条消息:

> 异步等待失败:超过 2 秒的超时，未达到预期:“等待异步函数完成”、“等待另一个异步函数完成”。

关于等待方法的另一个重要的事情是，期望实现的顺序并不重要。如果您的测试要求期望以特定顺序实现，您可以使用下一个方法重载:

```
func **wait**(for expectations: [[XCTestExpectation](https://developer.apple.com/documentation/xctest/xctestexpectation)], 
  timeout seconds: [TimeInterval](https://developer.apple.com/documentation/foundation/timeinterval), 
enforceOrder enforceOrderOfFulfillment: [Bool](https://developer.apple.com/documentation/swift/bool))
```

## Authenticator 类示例

`Authenticator`类将帮助我们更好地理解之前描述的动力学。我们的类非常简单，它只有一个`signIn`方法。`signIn`方法将执行一个异步任务，它有一个悲伤路径和一个快乐路径，这意味着我们将为它们中的每一个写一个测试。

```
**class** Authenticator { **enum** Result: Equatable {
      **case** success
      **case** fail(Error)
   } **enum** Error: Swift.Error {
      **case** imcompleteData
   } **func** signIn(email: String, password: String, completion: **@escaping** (Result) -> Void) { **guard** !email.isEmpty, !password.isEmpty **else** {
         completion(.fail(.imcompleteData))
         **return** } completion(.success)
   }}
```

让我们从 sad 路径测试开始，如果电子邮件或密码为空，`signIn`方法会产生一个`incompleteData`错误。

```
**func** test_signIn_withEmptyEmailOrPasswordDeliversIncompleteDataError() { //Given
   **let** sut = Authenticator()
   **let** email = ""
   **let** password = "" //When
   authenticator.signIn(email: email, 
                        password: password) { result **in** }}
```

在前面的例子中，到目前为止我们已经给出了我们的给定和我们的何时，但是我们仍然缺少一些东西。我们的`signIn`函数是异步的，这意味着我们需要添加一个期望并等待这个期望实现。现在的问题是:

1.  我们在哪里实现我们的期望？和
2.  我们在哪里等待我们的期望完成？

我们知道，当我们作为参数传递的闭包被执行时，`singIn`函数就完成了，因此，闭包就是我们实现预期的地方。

我们将通过调用`wait`方法，在异步函数调用之后立即等待我们的期望实现。`wait`方法“冻结”我们的源代码，直到您作为参数传递的期望实现。如果我们在异步函数之前调用这个方法，我们将在测试中收到一个超时错误。

```
**func** test_signIn_withEmptyEmailOrPasswordDeliversIncompleteDataError() { *//Given*
   **let** sut = Authenticator()
   **let** email = ""
   **let** password = "" * //When
   //Expectation declaration*
   **let** exp = 
      expectation(description: "Wait for async signIn to complete")
   authenticator.signIn(email: email, 
                        password: password) { result **in** *//Fulfill expectation from signIn completion block*exp.fulfill()
   } *//Freeze the function execution until we fulfill our expectation
   //This could take a second or less*wait(for: [exp], timeout: 1.0)
}
```

太好了！我们有一个通过测试，我们确保我们不会得到任何超时错误，因为我们正在满足我们的期望。我们仍然缺少我们的断言来确保我们有一个有效的测试。我们将在`singIn`完成块中添加断言(目前可以工作)🙂).

```
**func** test_signIn_withEmptyEmailOrPasswordDeliversIncompleteDataError() { *//Given*
   **let** sut = Authenticator()
   **let** email = ""
   **let** password = "" *//When*
   **let** exp = 
      expectation(description: "Wait for async signIn to complete")
   authenticator.signIn(email: email, 
                        password: password) { result **in** *//Then*
      XCTAssertEqual(result, .fail(.imcompleteData)) exp.fulfill()
   } wait(for: [exp], timeout: 1.0)
}
```

太好了！我们用这个测试覆盖了悲伤的道路。现在，让我们添加一个测试来覆盖幸福之路。

```
**func** test_signIn_withEmailAndPasswordAuthenticatesTheUser() {
   **//Given
   let** sut = Authenticator()
   **let** email = "my-email@my-domain.com"
   **let** password = "my-password" **//When
   let** exp = 
      expectation(description: "Wait for async signIn to complete")
   authenticator.signIn(email: email, 
                        password: password) { result **in** //Then
      XCTAssertEqual(result, .success) exp.fulfill()
   } wait(for: [exp], timeout: 1.0)
}
```

太好了，现在我们已经通过了两条路径的测试！但在我看来，我们在重复自己，我们可以改进这两项测试。如您所见，两个测试函数都是:

1.  宣布一个期望，
2.  调用`signIn`方法，
3.  等待期望的实现，最后，
4.  做一些断言。

这意味着我们可以在一个单独的函数中提取所有这些任务。我们的函数将接收我们的 SUT(被测系统)，在本例中是一个`Authenticator`实例、`email`、`password`，最后是预期的结果。由于我们将在这个函数中做一些断言，我们也必须传递`file`和`line`，以便 XCode 向我们显示可能失败的正确行。

```
**func** test_signIn_withEmailAndPasswordAuthenticatesTheUser() {
   **let** email = "my-email@my-domain.com"
   **let** password = "my-password" expect(Authenticator(), 
          email: email, 
          password: password,   
          toCompleteWith: .success)
}**func** test_signIn_withEmptyEmailOrPasswordDeliversIncompleteDataError() {
   **let** email = ""
   **let** password = "" expect(Authenticator(), 
          email: email, 
          password: password, 
          toCompleteWith: .fail(.imcompleteData))
}// MARK: **- Helper****private** **func** expect(**_** sut: Authenticator, 
                    email: String, 
                    password: String, 
                    toCompleteWith expectedResult: Authenticator.Result, 
                    file: StaticString = **#filePath**, 
                    line: UInt = **#line**) { **let** exp = 
      expectation(description: "Wait for async signIn to complete")
   sut.signIn(email: email, password: password) { receivedResult **in** XCTAssertEqual(expectedResult, 
                     receivedResult, 
                     "Expecting \(expectedResult) got \(receivedResult) instead", 
                     file: file, 
                     line: line) exp.fulfill()
   } wait(for: [exp], timeout: 1.0)
}
```

现在我们的测试更干净，更容易理解他们在做什么！

我们还可以遵循另一种方法。假设我们想从测试函数中得到断言。我们可以创建一个助手方法来返回`signIn`结果。这听起来像是一个不可能的任务，不是吗？调用异步函数，然后返回它的值。你还记得我们提到过调用`wait`方法会冻结你的代码吗？这将帮助我们获得`signIn`结果并返回它。

```
**func** test_signIn_withEmailAndPasswordAuthenticatesTheUser() {
   **let** email = "my-email@my-domain.com"
   **let** password = "my-password" **let** receivedResult = 
      resultFor(Authenticator(), email: email, password: password) XCTAssertEqual(receivedResult, .success)
}**func** test_signIn_withEmptyEmailOrPasswordDeliversIncompleteDataError() {
   **let** email = ""
   **let** password = "" **let** receivedResult = 
      resultFor(Authenticator(), email: email, password: password) XCTAssertEqual(receivedResult, .fail(.imcompleteData))
}// MARK: **- Helper****private** **func** resultFor(**_** sut: Authenticator,
                       email: String,
                       password: String) -> Authenticator.Result? { **let** exp = 
      expectation(description: "Wait for async signIn to complete") **var** result: Authenticator.Result?
   sut.signIn(email: email, password: password) { receivedResult **in** result = receivedResult
      exp.fulfill()
   } wait(for: [exp], timeout: 1.0) **return** result}
```

我们仍然通过了两条路径的测试！如您所见,`resultFor`函数调用`singIn`函数，然后等待期望实现。一旦期望实现，函数可以继续执行到`return result`行。

这两种方法都可以有效地使用，并帮助我们保持测试源代码的整洁。