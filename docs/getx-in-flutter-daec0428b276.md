# 抖动中的 GetX

> 原文：<https://medium.com/geekculture/getx-in-flutter-daec0428b276?source=collection_archive---------14----------------------->

![](img/3df099788e1e2247bf5109e61625f761.png)

etX 是颤振中管理状态的多种方式之一。 [Getx](https://pub.dev/packages/get) 是管理状态最简单、最容易的方式，因为它使用内联代码或更少的代码来管理状态。

让我们首先讨论什么是真正的状态管理，然后我们将讨论各种状态管理方式。
**状态管理:**类似于与项目中的其他屏幕共享状态或数据。当一个屏幕中状态发生变化且必须在另一个屏幕中使用更新后的状态时，就出现了术语“[状态管理](https://flutter.dev/docs/development/data-and-backend/state-mgmt/intro)”。

**状态管理方式列表:** 1。GetX
2。供应商
2。Redux
4。阻挡
5。GetIt
6 活页夹
7。Riverpod
8。MobX
9。你喜欢哪个就选哪个，这完全取决于你。但最可取的是 **GetX** ，**BLoC**&**Provider**。我大多用 GetX。

> 现在，我将分享所有关于 GetX 的内容，从我所经历的最简单的方式开始。:)

**使用 GetX:** 使用 **GetMaterialApp** 代替 main.dart 文件中的 MaterialApp。

**用于导航:**

```
//For navigating to Page.
**Get.to(Page());
Get.to(()=>Page());**//For popping page or context.
**Get.back();**// For popping to the desired screen
**Get.off(Page());**//For clearing all navigations
**Get.offAll(Page());**
```

**用于改变状态:**

```
//make a controller
**class DemoController extends GetxController{
var demo;****...
}**// For using this controller in other dart file
//Initialize the controller**Get.put(DemoController());**//For changing the variable**Get.find<DemoController>().demo=2;**//For using controller as a widget**GetBuilder(builder:(DemoController cont)=>Text(cont.demo));**
```

**对于 Snackbar:**

```
Get.snackbar("title", "message",
 duration: Duration(seconds: 3),
 colorText: Colors.red,
 snackPosition: SnackPosition.BOTTOM);
```

**换主题:**

```
Get.changeTheme(ThemeData.light());
```

**用于显示对话框:**

```
Get.dialog(Container(child: Text("Dialog"),));
```

这些是我使用 GetX 进行状态管理的各种方式。我觉得绰绰有余了。如果我了解更多，我会更新这篇文章，或者你可以访问这里的。这就是 getx 的全部内容。

欲了解更多信息，请购买《让自己成为软件开发者:让我们深入了解 Flutter & MNCs》一书。

希望你喜欢，并可能得到帮助。
随时回复，关注我更多文章。
在这里连接我👇👇
[LinkedIn](https://www.linkedin.com/in/lakshydeep-14/)
[Github](https://github.com/lakshydeep-14)
保持学习
保持编码