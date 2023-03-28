# 使用 React Native (4)构建图像浏览应用程序— RecyclerListView

> 原文：<https://medium.com/geekculture/build-an-image-browsing-app-with-react-native-4-recyclerlistview-b3cb465e1ef7?source=collection_archive---------36----------------------->

> *React 原生为 iOS 和 Android 带来了* [*React 的*](https://reactjs.org/) *声明式 UI 框架。使用 React Native，您可以使用原生 UI 控件，并拥有对原生平台的完全访问权限。*[*(https://github.com/facebook/react-native*](https://github.com/facebook/react-native)*)*

这是本教程关于如何用 React Native 构建图片浏览应用的第 4 篇文章。所以我想你已经读过它的前几部分了。

这是前面章节的链接。

[](/geekculture/build-an-image-browsing-app-with-react-native-df303d222a0d) [## 使用 React Native 构建图片浏览应用程序

### React Native 为 iOS 和 Android 带来了 React 的声明式 UI 框架。使用 React Native，您可以使用本机 UI 控件…

medium.com](/geekculture/build-an-image-browsing-app-with-react-native-df303d222a0d)  [## 使用 React Native(2)-Redux 构建图片浏览应用程序

### React Native 为 iOS 和 Android 带来了 React 的声明式 UI 框架。使用 React Native，您可以使用本机 UI 控件…

kxie0124.medium.com](https://kxie0124.medium.com/build-an-image-browsing-app-with-react-native-2-redux-68f2d7a6744f) [](https://kxie0124.medium.com/build-an-image-browsing-app-with-react-native-3-persistent-storage-2acdf01f3c62) [## 使用 React Native (3)构建图像浏览应用程序—持久存储

### React Native 为 iOS 和 Android 带来了 React 的声明式 UI 框架。使用 React Native，您可以使用本机 UI 控件…

kxie0124.medium.com](https://kxie0124.medium.com/build-an-image-browsing-app-with-react-native-3-persistent-storage-2acdf01f3c62) 

实际上，我们已经实现了这样一个图像浏览应用程序的所有功能，包括在一个`FlatList`中显示图像，从后端 API 获取数据，在持久存储中保存数据，用 Redux 在全局状态存储中保存数据，用`react-navigation`从一条路线导航到另一条路线。所以在这篇文章中，我只是做了一些改进，比如使用`RecyclerListView`来显示图片，而不是使用 FlatList。

> 什么是 RecyclerListView？这是一个支持复杂布局的 React Native 和 Web 的高性能 listview。受 Android 上的 RecyclerView 和 iOS 上的 UICollectionView 的启发，只有 JS，没有原生依赖。
> 
> RecyclerListView 使用“单元回收”来重用不再可见的视图以呈现项目，而不是创建新的视图对象。对象的创建是非常昂贵的，并且伴随着内存开销，这意味着当你在列表中滚动时，内存占用会不断增加。释放内存中的不可见项是另一种技术，但是这会导致创建更多的对象和大量的垃圾收集。回收是呈现无限列表的最佳方式，不会影响性能或内存效率。
> 
> —[https://github.com/Flipkart/recyclerlistview](https://github.com/Flipkart/recyclerlistview)

它可以做 FlatList 能做的所有事情，但是性能更好。

# 添加 RecyclerListView 包

首先我们需要添加一个新的包来支持`RecyclerListView`,方法是运行

```
yarn add recyclerlistview
```

您会发现一个新的依赖项被添加到项目的`Package.json`中。

# 创建回收列表组件

然后，让我们在`home`文件夹中创建一个新文件`RecyclerList.tsx`，并添加以下代码

```
import React from 'react';
import {
  RecyclerListView,
  DataProvider,
  LayoutProvider,
  Dimension,
} from 'recyclerlistview';
import {StyleSheet, StatusBar, Dimensions} from 'react-native';
import Item from './Item';const getWindowWidth = () =>
  Math.round(Dimensions.get('window').width * 1000) / 1000 - 6;
const RecyclerList = ({
  data,
  nextPage,
  navigation,
}: {
  data: Array<Photo>;
  nextPage: () => void;
  navigation: any;
}): JSX.Element => {
  console.log(
    'RecyclerList',
    data.map(item => item.id),
  );const renderItem = (type: string | number, data: any) => (
    <Item photo={data} navigation={navigation} />
  );
  const dataProvider = new DataProvider((r1, r2) => {
    return r1 !== r2;
  }).cloneWithRows(data);
  const layoutProvider = new LayoutProvider(
    index => {
      return JSON.stringify({
        width: data[index].width,
        height: data[index].height,
      });
    },
    (type: string | number, dim: Dimension) => {
      switch (type) {
        case 'VSEL':
          dim.width = getWindowWidth() / 2;
          dim.height = 150;
          break;
        default:
          if (typeof type === 'string') {
            const {width, height} = JSON.parse(type);
            dim.width = getWindowWidth() / 2;
            dim.height = 250;
          }
      }
    },
  );
  return (
    <RecyclerListView
      style={styles.container}
      dataProvider={dataProvider}
      layoutProvider={layoutProvider}
      rowRenderer={renderItem}
      onEndReached={nextPage}
    />
  );
};const styles = StyleSheet.create({
  container: {
    flex: 1,
    paddingTop: StatusBar.currentHeight || 0,
    marginHorizontal: 0,
  },
});
export default RecyclerList;
```

然后我们用`RecyclerListView`实现了一个组件`RecyclerList`。我们可以用这个新组件来呈现图像列表。稍后，我们将替换`Home`组件上原来的`List`组件。

其实大部分代码都是标准的。我们需要做的是实现我们自己的`DataProvider`，用它来消费图像数组，还有`LayoutProvider`，用它我们可以定制布局。但是仍然有一些限制，例如,`RecyclerListView`不支持每个元素的动态高度，所以我们不能用砖石列表来呈现元素。

# 更新主页组件

最后一步是更新`Home`组件中的下面一行

```
import List from './List';
```

随着

```
import {default as List} from './RecyclerList';
```

然后再次运行该应用程序。但是您会发现没有什么变化，因为使用`RecyclerListView`代替 FlatList 只是性能增强。用户没有任何变化。

好的。这一章简短明了。谢谢你阅读它。