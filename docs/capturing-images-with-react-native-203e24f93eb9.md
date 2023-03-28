# 使用 React Native 捕捉图像

> 原文：<https://medium.com/geekculture/capturing-images-with-react-native-203e24f93eb9?source=collection_archive---------0----------------------->

对于第一个博客，我想我应该从分享我在开发我的一个应用程序的 [V2](https://play.google.com/store/apps/details?id=com.syookpro) 时的一个重要的学习和经历开始。我只在 android Studio 上为 Android 开发了 V1，很快[我们](http://www.syook.com)意识到我们也需要一个 iOS 版本。由于对 Swift 一无所知，我不喜欢支持 2 种不同的代码库，并开始搜索跨平台应用程序开发选项。经过大量的搜索后，我被我的一个朋友说服去钻研 React Native。

我开始探索 React Native 是否允许我构建一些东西，可以与我在 android 生产中作为 V1 已经拥有的东西相匹配。对于大多数功能来说，它可以无缝地工作，但当它访问硬件时就变得棘手了。我们让用户录制音频片段并捕捉图像。React Native 没有这方面的内置功能，所以我不得不寻找一个外部库来做这件事。

[react-native-camera](https://github.com/react-native-community/react-native-camera) 是我唯一能找到的捕捉图像的库。[react-native-audio-toolkit](https://github.com/futurice/react-native-audio-toolkit)是我用来录制音频的工具(如果需要的话，我以后会在博客上写这个)

我想要的是捕捉图像并保存在一个自定义的目录中。关于 [react-native-camera](https://github.com/react-native-community/react-native-camera) 的文档足以让我捕捉图像，但默认选项保存到相机胶卷。为了将图像移动到自定义的应用程序目录，我使用了 [react-native-fs](https://github.com/itinance/react-native-fs) 。

这里是我的 2 美分的代码，允许这一点(希望这能帮助别人！)

创建 dirStorage.js，它将包含您想要存储图像的目录。这里我以 iOs 版的 DocumentDirectoryPath/myapp name 和 android 版的 externalstreatirectorypath/myapp name 为例。文件夹 myAppName 将在 iOS 的根目录和 android 的外部存储目录中可用。这里面的子目录是我存储所有文件的地方。

我已经创建了一个基本的相机组件，它将捕获图像并将其移动到从 dirStorage 文件导入的文件夹中。有一个摄像头图标来捕捉图像，还有一个十字图标来取消并返回到上一个屏幕(为了导航，我使用了 [react-navigation](https://github.com/react-navigation/react-navigation) )。作为奖励，我为相机添加了一些基本的样式，并处理方向的变化，以适应按钮及其样式。

为了捕获图像，在初始化照相机组件时保存的照相机 ref 上调用 capture 方法。它返回包含保存在承诺响应中的图像的文件路径的数据对象。

```
takePicture() { this.camera .capture() .then(data => this.saveImage(data.path)) .catch(err => console.error('capture picture error', err));}
```

然后，我使用 moveAttachment 方法将该图像移动到所需的文件路径。

```
const moveAttachment = async (filePath, newFilepath) => { return new Promise((resolve, reject) => { RNFS.mkdir(dirPicutures) .then(() => { RNFS.moveFile(filePath, newFilepath) .then(() => resolve(true)) .catch(error => reject(error)); }) .catch(err => reject(err)); });};
```

摄像头组件:

作为我探索 React-Native 的一部分，解决这个问题是我面临的许多有趣挑战之一。发现最终的解决方案非常简洁。

希望有人会发现这有助于他们在 react-native 上捕捉和处理图像。

# 感谢阅读！😃