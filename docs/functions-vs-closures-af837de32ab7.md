# 函数与闭包

> 原文：<https://medium.com/geekculture/functions-vs-closures-af837de32ab7?source=collection_archive---------5----------------------->

![](img/7003a90d29b533a5590f49b206a9df16.png)

如果你对编程感兴趣，那么当然，你以前听说过函数，但也许闭包表达式对你来说是新的，或者你会对此有点困惑，所以在思考任何事情之前，让我们深入这两个伟大的概念，检查一下它们的区别。

# 1-功能

函数被认为是一个代码块，你可以通过提及它的名字来调用它，函数可以带参数也可以不带参数，你也可以返回值或不返回值，这取决于你的代码。让我们举一个非常简单的函数的小例子:

让我们假设你是一名跑步者，你知道你可以在 7 分钟内跑完 1 公里，所以你想计算你在特定时间内可以跑多少公里

```
countKilometers(totalTimeInMinutes: 100,timePerKilo: 7)
```

如果你没听清前面的内容，不要担心，现在一切都会得到详细解释。

前一行代码包含两个变量，或者更具体地说，包含您的参数，但这一行代码称为“函数调用”，因为每个程序员都知道函数有两个部分:

> 1.定义函数
> 2。调用函数

如果你稍微想一想，你就会发现，在选修他的课之前，你不能参加考试，当然你会不及格，这里也一样，在调用函数之前，你必须先定义它，所以现在的问题是，我们如何定义它？

让我们定义 count 公里函数:

```
func countKilometers(totalTimeInMinutes: Int ,timePerKilo: Int) {
    print("You run about \(totalTimeInMinutes/timePerKilo) kms")
}
countKilometers(totalTimeInMinutes: 100,timePerKilo: 7)
```

> 输出:**你跑了大约 14 公里**

所以我们确实在前两行中定义了我们的函数，当我们在最后一行调用它时，就像之前一样，我们将得到 **14** ，如果有人问我为什么我们得到 **14，**而不是 **14.28，**我会问你“我们的**参数**的数据类型是什么？”所以我们会注意到，这里我们的参数的数据类型是 **Int** ，它不能保存任何分数类型。

这样做之后，我们发现我们的函数可以由以下内容组成:

> 1.名为 **func** 的关键字
> 2。函数名为**count 公里**3。参数“可选”可以添加也可以不添加:
> 3.1 第一个参数:totalTimeInMinutes
> 3.2 第二个参数:timePerKilo
> 4 .Return type“我们现在就来提一下”
> 5。{函数体}

现在我们将尝试添加一个返回类型，并使用 **Float** 而不是 **Int** 作为参数的数据类型

```
func countKilometers(totalTimeInMinutes: Float ,timePerKilo: Float) -> Float{
    return totalTimeInMinutes/timePerKilo
    //    print("You run about \(totalTimeInMinutes/timePerKilo) kms")}
countKilometers(totalTimeInMinutes: 100,timePerKilo: 7)
```

> 输出:**你跑了大约 14.285714 公里**

现在你有了你已经跑了多少公里的确切数字，我们使用 Float 代替 Int，print 语句 if 注释，因为我们已经使用 return 语句返回我们的结果

> ***注:*** *你可以用****/****来做一个* ***单行*** *注释或者如果你想多行***注释哟可以用这个* ***** /** 前的注释和 **/ ** .****

*所以我们试着做一个有两个参数的函数，让我们试着做一个没有任何参数的函数*

```
*func printPlayersNames(){
    let playerNames = ["Mo Salah", "Messi", "Ronaldo", "Mane", "Ibrahimovic"]
    print(playerNames)
}
printPlayersNames()*
```

*我们可以做同样的功能，但是通过传递一个字符串数组作为参数:*

```
*func printPlayersNames(playerNames: [String]){
    print(playerNames)
}let myPlayerNames = ["Mo Salah", "Messi", "Ronaldo", "Mane", "Ibrahimovic"]
printPlayersNames(playerNames: myPlayerNames)*
```

> *输出:**【“莫萨拉赫”、“梅西”、“c 罗”、“马内”、“伊布”】***

*在这两种情况下，我们的输出仍然相同，因为我们只是没有让函数不带参数并在其中打印一个字符串数组，而是在第一行**中添加了一个名为 **playerNames** 的**字符串数组作为参数，然后在第三行**中初始化一个常量并将其命名为**my playerNames****第三行中将其传递给最后一行中的函数。*****

*您还可以使用一种叫做参数标签的东西，让我们来看一个例子，并详细解释一下:*

```
*func playGame(withPlayerName userName: String, andGameLevel gameLevel: String) {     
     print("Welcome \(userName) in your first \(gameLevel) level") 
} 
playGame(withPlayerName: "Menaim", andGameLevel: "Hard")*
```

> *输出:**在你的第一关**欢迎梅纳姆*

*在 PlayerName 和**和 GameLevel** 中使用类似**的参数标签的好处是，当你试图像在最后一行中那样调用函数时，你可以看到我们不必再添加参数，我们所需要做的就是像调用函数时那样使用我们强大的参数标签。***

*你必须注意参数和参数标签之间的区别，因为参数标签更多地用于编码，因为它在实际的 iOS 中相当常见，并且描述函数比仅仅传递参数更容易，可能这些参数在函数的意义上没有意义，所以参数标签对任何函数来说都更具描述性。*

*讲了函数，了解了函数的一切，现在的问题是我们为什么要用函数？函数有什么好处？*

*如果你必须通过 50 个楼梯大约 5 次，每次你必须在这些楼梯上上下下，但同时你可以选择只通过这些楼梯一次，你不需要再走 4 次…*

*在函数中也是一样，如果你有一段代码，这段代码可能会在你的代码中重复很多次，所以即使你在复制它，也不要增加代码的行数或者重复同样的事情，而是把这段代码放到你的函数中。*

# *2-关闭*

*闭包是自包含的功能块，可以在代码中传递和使用(Apple)*

*因此，苹果将闭包定义为之前的定义，但你仍然不理解它的含义，这就是为什么我们将详细解释闭包中的一切，所以让我们从一个例子开始:*

```
*func backward (_ S1: String, _ S2: String) -> Bool {
    return S1 > S2
}
let names = ["Ahmed", "Mohamed","Rose","Maya","Noah"]
var arrangedNames = names.sorted(by: backward)
print(arrangedNames)*
```

*名为 S1 和 S2 的参数和返回类型 **Bool，**所以现在我们确实使用了闭包，但作为一个函数，所以现在我们有一个排序闭包，写为一个函数，之后我们初始化了一个名为 **names** 的字符串数组，然后我们将开始使用苹果提供的一个名为 **sorted(by:)** 的函数*

> *输出: **["罗斯"、"诺亚"、"穆罕默德"、"玛雅"、"艾哈迈德"]***

*但是这种形式并不常见，或者不是大多数开发人员都在使用它，所以让我们看看闭包的另一种形式:*

```
*let myNames = ["Ahmed", "Mohamed","Rose","Maya","Noah"]
var arrangedNames: [String] = []
arrangedNames = myNames.sorted(by:
            { (S1: String, S2: String) -> Bool in
    return S1 > S2
})
print(arrangedNames)*
```

> *输出: **[“罗斯”、“诺亚”、“穆罕默德”、“玛雅”、“艾哈迈德”]***

*当然，正如我们注意到的，结果还是一样的，闭包体的开始是由关键字中的**引入的。这个关键字表明闭包的参数和返回类型的定义已经完成，闭包的主体即将开始，为了增强我们的代码，我们可以从前面的代码中删除一些东西，让我们来看看我们可以删除什么来增强它。***

```
*let myNames = ["Ahmed", "Mohamed","Rose","Maya","Noah"]
var arrangedNames: [String] = []
arrangedNames = myNames.sorted(by: {  s1, s2 in
     return s1 > s2
})
print(arrangedNames)*
```

*我们发现了 swift 的强大之处，我们甚至不需要提及参数的数据类型或返回类型，因为 Swift 有能力自己发现它，但进展如何呢？？*

*让我们来计算一下:*

*开始时**排序(by:)** 正如我们之前提到的，它是 swift 中的一个**内置**函数，所以如果我们深入研究它，我们会发现它已经接受了两个 **String** 参数并返回了 **Bool** 因此，如果您没有编写它们，swift 将默认使用原始参数，因此它将接受 s1 & s2 作为字符串，返回类型将为 Bool*

> *在这里，我们可以越来越多地缩短我们的闭包，但是我们怎么做呢？通过使用 ***速记论证名称****

```
*let myNames = ["Ahmed", "Mohamed","Rose","Maya","Noah"]
var arrangedNames = myNames.sorted(by: { $0 > $1 } )
print(arrangedNames)*
```

*那么，为什么我们使用$0 & $1、$0 和$1 来指代闭包的第一个和第二个字符串参数呢？这意味着 swift 可以为您提供速记参数名称，这意味着如果您有两个以上的参数，我们可以使用$0、$1、$2 等等*

> **还有一种更短的方式来编写闭包，我们可以在* ***“操作方法”*** 中使用*

```
*let myNames = ["Ahmed", "Mohamed","Rose","Maya","Noah"]
var arrangedNames = myNames.sorted(by: >)
print(arrangedNames)*
```

*所以这里我们只使用了>,这向我们展示了 swift 是多么有用和强大的语言，因为它发现您只需要通过使用排序运算符>,就可以对两个字符串进行排序并返回布尔值*

*我们也可以使用所谓的尾随闭包，这意味着我们将把闭包作为参数传入函数，以便能够使用和调用它*

> **注:最有用也是用得最多的是* ***尾随闭包****

```
*func functionTakesAClosure(closure: () -> Void) {
    // function body goes here
}*
```

*下面是不使用尾随闭包调用该函数的方法:*

```
*functionTakesAClosure(closure: {
    // closure's body goes here
})*
```

*下面是如何使用尾随闭包来调用该函数:*

```
*functionTakesAClosure() {
    // trailing closure's body goes here
}*
```

*因此，让我们将它应用到我们的示例中:*

```
*let myNames = ["Ahmed", "Mohamed","Rose","Maya","Noah"]
var arrangedNames = myNames.sorted(){
    $0 > $1
}
print(arrangedNames)*
```

*在调用函数时，我们甚至不需要在函数或方法名后写或使用一对括号()*

```
*let myNames = ["Ahmed", "Mohamed","Rose","Maya","Noah"]
var arrangedNames = myNames.sorted { $0 > $1 }
print (arrangedNames)*
```

*您可以使用带有两个或更多闭包的函数，在这种情况下，我们将在最后一个函数中使用尾随闭包，如下所示:*

*假设我们有下面的函数为照片库加载一张照片:*

*所有的变量和数据类型都是未声明的，你可以在你的代码中声明它们，比如“服务器”，“图片”，func download & someView*

```
*func loadMyPicture(from server: Server, completion: (Picture) -> Void, onFailure: () -> Void) {
    if let picture = download("photo.jpg", from: server) {
        completion(picture)
    } else {
        onFailure()
    }
}*
```

*当您调用这个函数来加载图片时，您提供了两个闭包。第一个闭包是一个完成处理程序，它在成功下载后显示一张图片。第二个闭包是一个向用户显示错误的错误处理程序。*

```
*let anyServer = Server()
loadMyPicture(from: anyServer) { picture in
    someView.currentPicture = picture
} onFailure: {
    print("Couldn't download the next picture.")
}*
```

*在本例中，load my picture(from:completion:on failure:)函数将其网络任务分派到后台，并在网络任务完成时调用两个完成处理程序之一。通过这种方式编写函数，您可以清楚地将负责处理网络故障的代码与成功下载后更新用户界面的代码分开，而不是只使用一个闭包来处理这两种情况。*

> **你必须注意闭包是* ***引用*** *类型**

```
*func createIncrementer(forIncrement amount: Int) -> () -> Int {
    var myTotal = 0
    func incrementer() -> Int {
        myTotal += amount
        print(myTotal)
        return myTotal
    }
    return incrementer
}*
```

***incrementer()** 函数没有任何参数，但它从其函数体内引用 myTotal 和 amount。它通过从周围的函数中获取对 myTotal 和 amount 的引用，并在自己的函数体中使用它们来实现这一点。通过引用捕获确保 myTotal 和 amount 不会在调用 **createIncrementer** 结束时消失，同时也确保 myTotal 在下次调用 **incrementer** 函数时可用。*

```
*let addTenIncrementer = createIncrementer(forIncrement: 10)
addTenIncrementer() // return 10
addTenIncrementer() // return 20
addTenIncrementer() // return 30*
```

*等等，这意味着如果我们创建一个常量并调用它，它应该不会改变，但是因为我们使用了一个闭包，它是一个引用类型，常量的值将会改变，就像我们以前做的那样，每次调用闭包，以确保下一步将会证明它。*

```
*let addTenIncrementeragain = addTenIncrementer
addTenIncrementeragain() // return 40
addTenIncrementer() // return 50*
```

*这意味着每一个都不独立于另一个，但是每一个都依赖于第二个，因为这里我们在 **addTenIncrementeragain()** 中有 40 个，然后在 **addTenIncrementer()** 中有 50 个，这使我们非常确定闭包是一个**引用**类型，而不是**值**类型。*

*我们有另一种类型的闭包，叫做 **Autoclosures** ，这种类型在你想要解开作为参数传递给函数的表达式时使用，让我们举一个例子来解释它:*

```
*var studentsQueue  = ["May", "Nolan","Haidy","Amal","Mohamed"]
print(studentsQueue)
// ["May", "Nolan","Haidy","Amal","Mohamed"]
let remove1stStudent = {studentsQueue.remove(at: 0)}
print(studentsQueue)
// ["May", "Nolan","Haidy","Amal","Mohamed"]
print("first student in line was \(remove1stStudent())")
// first student in line was May
print(studentsQueue)
// ["Nolan","Haidy","Amal","Mohamed"]*
```

*如果我们检查最后一段代码，我们会发现在声明变量**remove 1 student**时，我们在第三行使用了闭包，尽管删除了第三行的第一个元素，它仍然打印了所有的数组，但是当我们在第五行调用闭包时，我们会发现它打印了没有第一个元素的数组，所以我们发现变量保持原样，直到闭包被触发。*

*最后一种闭包是转义闭包，它是:*

*当闭包作为参数传递给函数，但在函数返回后被调用时，闭包被称为对函数进行转义(苹果)*

*前面的意思是，如果你声明一个以闭包作为参数的函数，并且如果你在参数的类型前写@escaping，这意味着这个闭包可以被跳过。*

```
*var completionHandlers = [() -> Void]()
func functionUsingEscaping(completionHandler: [@escaping](http://twitter.com/escaping) () -> Void) {
    completionHandlers.append(completionHandler)
}*
```

*我们这里有一个函数**functionUsingEscaping()**，它将一个闭包作为其参数，并将其添加到一个名为 **completionHandlers** 的数组中，如果我们没有提到 **@escaping** ，这段代码将会给出一个编译时错误。*

> ****注:*** *转义闭包无法捕获一个可变引用****self****为结构体**

*感谢 reading️！帮忙宣传一下&别忘了查看其他关于 Swift 中[数组 VS 集合](/swlh/array-vs-set-in-swift-aabb3b6f5930)、[类 VS 结构](https://menaim.medium.com/class-vs-struct-in-swift-5c4f9b44a5a9)、[可选的](https://levelup.gitconnected.com/optionals-in-swift-3831f1ca5dfb) & [弧的文章。](https://menaim.medium.com/strong-vs-weak-unowned-references-arc-in-swift-171c06a93933)*

*对即将发布的博客文章有问题、建议、评论或想法吗？在 [LinkedIn](https://www.linkedin.com/in/ahmed-menaim-22cm/) 上联系我或者写评论！你也可以在 [GitHub 上关注我。](https://github.com/CryptoOo/)*