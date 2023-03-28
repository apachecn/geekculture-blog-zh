# 在 Flutter 中将推送通知重定向到相应的屏幕

> 原文：<https://medium.com/geekculture/on-tap-push-notification-redirect-to-corresponding-screen-in-flutter-9f184068e4f8?source=collection_archive---------7----------------------->

从之前的[文章](/geekculture/push-notification-in-flutter-in-background-as-well-as-foreground-using-firebase-1e6e7ecad292)中，你将学习如何通过 firebase 在前台和后台显示推送通知。仅显示推送通知是不够的。我们需要在点击后将其重定向到某个屏幕。所以问题来了:我们如何在 flutter 中重定向到点击推送通知的特定屏幕。

我们开始吧:

**先决条件:**
-如果你知道，自己在 flutter 中集成推送通知
-如果不知道，跟随这篇[文章](/geekculture/push-notification-in-flutter-in-background-as-well-as-foreground-using-firebase-1e6e7ecad292)关于如何使用 Firebase 在 Flutter 的后台和前台添加推送通知

**程序(点击通知时重定向至特定屏幕):**

在上述文章的 ***步骤 VIII*** 中，只需编辑"*FLT notification . init setting(init setting)；"。* 粘贴下面的代码代替这段代码:

```
fltNotification.initialize(initSetting,onSelectNotification: (String payload)async{
//function to navigate to the page you want to show after tapping //push notification
});
```

根据有效载荷，您将导航到不同的屏幕，例如:

```
fltNotification.initialize(initSetting,onSelectNotification: (String payload)async{
   if(payload=="<Condition A>"){
      // show page A
   }else{
      //show page B
   }
});
```

在这之后，你必须像这样使用有效载荷:

```
fltNotification.show(
notification.hashCode, notification.title, notification.body, generalNotificationDetails,**payload: “Notification”**);
```

因此，完成这些操作后，您的所有代码将看起来像这样:

```
void initMessaging() {  
var androiInit = AndroidInitializationSettings(‘@mipmap/ic_launcher’);//for logo  
var iosInit = IOSInitializationSettings();  
var initSetting=InitializationSettings(android: androiInit,iOS:
   iosInit);  
fltNotification = FlutterLocalNotificationsPlugin(); **//fltNotification.initialize(initSetting);**fltNotification.initialize(initSetting,onSelectNotification: (String payload)async{
//function to navigate to the page you want to show after tapping //push notification
}); var androidDetails =
   AndroidNotificationDetails(‘1’, ‘channelName’, ‘channel 
    Description’);  
var iosDetails = IOSNotificationDetails();  
var generalNotificationDetails =
   NotificationDetails(android: androidDetails, iOS: iosDetails);FirebaseMessaging.onMessage.listen((RemoteMessage message) {     RemoteNotification notification=message.notification;
   AndroidNotification android=message.notification?.android;
   if(notification!=null && android!=null){
      **//**  **fltNotification.show(
      // notification.hashCode, notification.title, notification.
      // body, generalNotificationDetails);** fltNotification.show(
notification.hashCode, notification.title, notification.body, generalNotificationDetails,**payload: “Notification”**);}
});
}
```

这些都是将推送通知重定向到某个页面所需的步骤。

欲了解更多信息，请购买书籍"**让自己成为软件开发人员:让我们深入了解 Flutter & MNCs** " [购买](https://www.amazon.com/dp/B09NNXNT6X/ref=sr_1_1?keywords=make+yourself+the+software&qid=1639582180&sr=8-1)。

继续编码&继续学习
也给予反馈
谢谢