# 制作电影游戏:交互式摄像机系统

> 原文：<https://medium.com/geekculture/making-a-cinematic-game-interactive-camera-system-baac9cfc1507?source=collection_archive---------14----------------------->

![](img/30efa1a408f68f5465d0efd1a5653787.png)

现在关卡设计、灯光和后期处理都完成了，是时候进入相机系统了。

**摄像机**

当玩家按下 ***R*** *键*时，将会有两个摄像机在其间切换。这里在一个 *vCams* 父对象中，有一个*第三人跟随*摄像机和一个*固定房间*摄像机。