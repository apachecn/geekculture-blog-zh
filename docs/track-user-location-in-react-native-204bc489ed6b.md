# 在 react native 中跟踪用户位置

> 原文：<https://medium.com/geekculture/track-user-location-in-react-native-204bc489ed6b?source=collection_archive---------3----------------------->

大家好！在本文中，我们将介绍如何使用 react-native-maps 库跟踪 react native/expo 中的用户位置。

在上一篇文章中，我们解释了如何开始使用 react native 的 react-native-maps 以及基本用法。如果你没有读过，现在就去读，然后回到这一页。

[https://medium . com/geek culture/mapview-in-Expo-react-native-5aa 69 EB 81519](/geekculture/mapview-in-expo-react-native-5aa69eb81519)

我们将从基本设置开始

```
import React, { useState } from 'react';
import { View, StyleSheet } from 'react-native';
import MapView from 'react-native-maps';
const App = () => {
  const [mapRegion, setmapRegion] = useState({
    latitude: 37.78825,
    longitude: -122.4324,
    latitudeDelta: 0.0922,
    longitudeDelta: 0.0421,
  }); 
 return (
    <View style={styles.container}>
      <MapView
        style={{ alignSelf: 'stretch', height: '100%' }}
        region={mapRegion}
      />
    </View>
  );
};
export default App;
const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
});
```

这段代码用变量 mapRegion 中的坐标在屏幕上显示一幅地图，并占据设备的整个屏幕。

然后，我们需要有权限观看当前用户的位置。为此，我们需要安装包 expo-location

```
expo install expo-location
```

为了在屏幕加载时请求权限，我们需要实现在屏幕加载时执行的钩子 useEffect。

```
useEffect(() => {
    (async () => {
      let { status } = await  Location.requestForegroundPermissionsAsync();
     if (status !== 'granted') {
        setStatus('Permission to access location was denied');
        return;
     } else {
       console.log('Access granted!!')
       setStatus(status) }

    })();
  }, []);
```

我们询问权限，如果我们获得授权的状态，我们就可以监视用户的位置。

现在我们需要实现这个函数 watchPositionAsync

```
const watch_location = async () => { if (status === 'granted') { let location = await Location.watchPositionAsync({
       accuracy:Location.Accuracy.High,
       timeInterval: 10000,
       distanceInterval: 80,
       }, 
       false ,(location_update) => { console.log('update location!', location_update.coords)
     } })}}
```

使用这个函数，我们传递我们想要的精度，每次更新之间的时间间隔(以毫秒计)，用户必须移动的距离间隔(以米计)来更新位置。然后我们在 mayShowUserSettingsDialog 中传递 false，最后我们有一个返回位置的回调函数。

我们需要知道，这个功能只在前台工作，如果用户关闭应用程序，将不会工作这个功能。为此，我们需要实现任务管理器库。

```
import React from 'react';
import { Text, TouchableOpacity } from 'react-native';
import * as TaskManager from 'expo-task-manager';
import * as Location from 'expo-location';

const LOCATION_TASK_NAME = 'background-location-task';TaskManager.defineTask(LOCATION_TASK_NAME, ({ data, error }) => {
  if (error) {
    // Error occurred - check `error.message` for more details.
    return;
  }
  if (data) {
    const { locations } = data;
    // do something with the locations captured in the background
  }
});const requestPermissions = async () => {
 const {status} = awaitLocation.requestBackgroundPermissionsAsync();
  if (status === 'granted') {
    await Location.startLocationUpdatesAsync(LOCATION_TASK_NAME, {
     accuracy:Location.Accuracy.High,
       timeInterval: 10000,
       distanceInterval: 80,
    });
  }
};

const PermissionsButton = () => (
  <TouchableOpacity onPress={requestPermissions}>
    <Text>Enable background location</Text>
  </TouchableOpacity>
);

export default PermissionsButton;
```

在这个例子中，我们首先需要请求在背景中观看位置的许可。然后，我们执行函数 startLocationUpdatesAsync，并传递任务的名称以及精度、时间间隔和距离间隔的参数。

一旦我们关闭应用程序，defineTask 将执行，我们可以在后台获得用户的位置，请记住，我们需要始终拥有位置权限。

在下一部分中，我们将把用户正在做的路线添加到 mapview 中。

敬请期待！