# Firebase Scheduler 的云功能的时区不变

> 原文：<https://medium.com/geekculture/time-zone-of-cloud-functions-for-firebase-scheduler-does-not-change-5680cc8491a7?source=collection_archive---------19----------------------->

【Firebase Scheduler 的云功能时区不变

我目前正在创建和部署一个函数，该函数将在 Firebase 调度程序的云函数中定期执行，如下所示。关键是我希望它在日本时间每天 0:05 被执行。时区(“亚洲/东京”)。

部署本身工作正常，但当我查看 GCP 云调度程序中的内容时，时区被设置为(美洲/洛杉矶)as…