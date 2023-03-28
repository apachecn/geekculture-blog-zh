# 使用 React Native (3)构建图像浏览应用程序—持久存储

> 原文：<https://medium.com/geekculture/build-an-image-browsing-app-with-react-native-3-persistent-storage-2acdf01f3c62?source=collection_archive---------28----------------------->

> *React Native 为 iOS 和 Android 带来了* [*React 的*](https://reactjs.org/) *声明式 UI 框架。使用 React Native，您可以使用原生 UI 控件，并拥有对原生平台的完全访问权限。*[*(https://github.com/facebook/react-native*](https://github.com/facebook/react-native)*)*

我们已经在建立了一个图像浏览应用程序

[](/geekculture/build-an-image-browsing-app-with-react-native-df303d222a0d) [## 使用 React Native 构建图片浏览应用程序

### React Native 为 iOS 和 Android 带来了 React 的声明式 UI 框架。使用 React Native，您可以使用本机 UI 控件…

medium.com](/geekculture/build-an-image-browsing-app-with-react-native-df303d222a0d) 

和

 [## 使用 React Native(2)-Redux 构建图片浏览应用程序

### React Native 为 iOS 和 Android 带来了 React 的声明式 UI 框架。使用 React Native，您可以使用本机 UI 控件…

kxie0124.medium.com](https://kxie0124.medium.com/build-an-image-browsing-app-with-react-native-2-redux-68f2d7a6744f) 

在这两篇文章中，我们实现了所有必要的功能，比如从后端 API 获取图像数据、在列表中显示图像、在用户从列表中单击图像时导航到单个图像的详细页面，以及在 Redux store 中保存和读取数据。

但你可能会注意到，当你启动应用程序时，即使你已经运行了应用程序，输入了关键字并浏览了一些图片，应用程序也不会显示任何内容。这是因为我们没有持久存储来保存数据和这些图像，当你关闭应用程序时，一切都消失了。

因此，在本教程中，我们将添加持久存储并保存其中的数据，然后当应用程序启动时，应用程序将显示保存的图像，并等待用户的关键字。

实际上，从技术上讲，我们不会保存图像本身，因为它们可能相当大。我们将保存来自后端 API 的数据，包括图像的所有 URL。

使用 React Native 在移动设备上实现持久存储有很多方法。最简单的方法可能是使用`[React Native AsyncStorage](https://reactnative.dev/docs/asyncstorage)`。有了`AsyncStorage`，我们可以很容易地在移动设备上保存一些带有键值对的数据。但是在本教程中，我将介绍另一种方法，用`[WatermelonDB](https://github.com/Nozbe/WatermelonDB)`来实现本地存储。

`WatermelonDB`是一个反应式数据库框架，在 React Native 和 React web 应用中实现了一种处理用户数据的新方法。

那我们开始吧。

# 安装 WatermelonDB

请按照[https://nozbe.github.io/WatermelonDB/Installation.html](https://nozbe.github.io/WatermelonDB/Installation.html)安装`WatermelonDB`。

# 创建模式

让我们首先为`WatermelonDB`创建模式。我建议在`src`文件夹中创建一个文件夹`database`，把所有数据库相关的代码放在这个文件夹中。

让我们在这个文件夹中创建一个新文件`Schema.ts`，并添加一个新的模式如下:

```
import {appSchema, tableSchema} from '@nozbe/watermelondb';export default appSchema({
  version: 1,
  tables: [
    tableSchema({
      name: 'photos',
      columns: [
        {name: 'photo_id', type: 'number'},
        {name: 'width', type: 'number', isOptional: true},
        {name: 'height', type: 'number', isOptional: true},
        {name: 'url', type: 'string', isOptional: true},
        {name: 'photographer', type: 'string', isOptional: true},
        {name: 'photographer_url', type: 'string', isOptional: true},
        {name: 'photographer_id', type: 'number', isOptional: true},
        {name: 'avg_color', type: 'string', isOptional: true},
        {name: 'src', type: 'string', isOptional: true},
        {name: 'liked', type: 'boolean', isOptional: true},
        {name: 'keyword', type: 'string', isOptional: true},
      ],
    }),
  ],
});
```

在这个模式中，我们在数据库中定义了一个表`photos`，并且在这个表中定义了几个具有适当类型的列，如果它是可选的。

# 创建模型

我们还需要相应地创建一个新的数据模型。因此，让我们创建一个新文件`Photo.ts`，并添加以下数据模型:

```
import {Model} from '@nozbe/watermelondb';
import {field} from '@nozbe/watermelondb/decorators';export default class PhotoModel extends Model {
  static table = 'photos'; @field('photo_id') photo_id: number;
  @field('width') width: number;
  @field('height') height: number;
  @field('url') url: string;
  @field('photographer') photographer: string;
  @field('photographer_url') photographerUrl: string;
  @field('photographer_id') photographerId: number;
  @field('avg_color') avgColor: string;
  @field('src') src: string;
  @field('liked') liked: boolean;
  @field('keyword') keyword: string;
}
```

这个模型类代表了我们应用程序中的一类东西。

# 实现数据库操作

然后我们需要创建一个数据库实例并实现一些 CRUD 方法。

因此，让我们创建一个新文件`Database.ts`，并将以下代码放入其中

```
import {Database} from '@nozbe/watermelondb';
import SQLiteAdapter from '@nozbe/watermelondb/adapters/sqlite';
import schema from './Schema';
import PhotoModel from './Photo';// First, create the adapter to the underlying database:
const adapter = new SQLiteAdapter({
  schema,
  // experimental JSI mode, a more advanced version of synchronous: true
  jsi: false,
  // Optional, but you should implement this method:
  onSetUpError: error => {
    // Database failed to load -- offer the user to reload the app or log out
  },
});// Then, make a Watermelon database from it!
const database = new Database({
  adapter,
  modelClasses: [PhotoModel],
  actionsEnabled: true,
});export const fetchAll = async (): Promise<Photo[]> => {
  const photosCollection = database.get('photos');
  const photos = (await photosCollection.query().fetch()) as Array<PhotoModel>;
  return photos.map((photoModel: PhotoModel) => {
    const photo = {
      id: photoModel.photo_id,
      width: photoModel.width,
      height: photoModel.height,
      url: photoModel.url,
      photographer: photoModel.photographer,
      photographer_url: photoModel.photographerUrl,
      photographer_id: photoModel.photographerId,
      avg_color: photoModel.avgColor,
      src: JSON.parse(photoModel.src),
      liked: photoModel.liked,
      keyword: photoModel.keyword,
    };
    return photo;
  });
};export const insertAll = async (photos: Array<Photo>) => {
  const photosCollection = database.get('photos');
  await database.action(async () => {
    await database.batch(
      ...photos.map((photo: Photo) => {
        const newPhoto = photosCollection.prepareCreate(
          (photoModel: PhotoModel) => {
            photoModel.photo_id = photo.id;
            photoModel.width = photo.width;
            photoModel.height = photo.height;
            photoModel.url = photo.url;
            photoModel.photographer = photo.photographer;
            photoModel.photographerUrl = photo.photographer_url;
            photoModel.photographerId = photo.photographer_id;
            photoModel.avgColor = photo.avg_color;
            photoModel.src = JSON.stringify(photo.src);
            photoModel.liked = photo.liked;
            photoModel.keyword = photo.keyword;
          },
        );
        return newPhoto;
      }),
    );
  });
};export const clearAll = async () => {
  const photosCollection = database.get('photos');
  const photos = await photosCollection.query().fetch();
  await database.action(async () => {
    photos.forEach(async (item: {markAsDeleted: () => any}) => {
      await item.markAsDeleted(); // syncable
      // await item.destroyPermanently(); // permanent
    });
  });
};
```

如这段代码所示，我们实现了一个适配器，一个根据我们之前定义的模式和模型的数据库实例，并实现了 3 个方法，`fetchAll`，用它们我们可以从数据库中获取所有图片；`insertAll`，我们可以添加一些新的图片到数据库中，和`clearAll`，我们可以删除数据库中的所有图片。

到目前为止，我们已经用`WatermelonDB`实现了从本地数据库保存/获取数据的基本部分，所以我们将在本章中用我们的数据库模型实现持久存储。

# 分离数据提取

在继续下一步之前，我想介绍一下软件开发中的一个重要原则，真理的单一来源(SSOT)。

> 在信息系统设计和理论中，单一的事实来源是构造信息模型和相关数据模式的实践，这样每个数据元素都只在一个地方被掌握。此数据元素的任何可能链接仅供参考。

但是在引入持久存储之后，我们将有两个数据源，后端 API 和数据库，所以我将修改我们的代码，使数据库成为唯一的真实数据源。

根据上面的解释，我想把数据获取函数从`Home`组件中分离出来，因为根据单一的事实来源，我们不应该从任何组件中直接调用这个函数。

您可能会注意到，我们在`Home`组件中实现了`fetchData`方法，并将其与当前代码中的 React 挂钩混合在一起，因此为了更好的结构，让我们将它与`Home`组件分开。

让我们在`src`文件夹中创建一个新文件夹`network`，并在其中创建一个新文件`FetchData.ts`，将`fetchData`方法移动到该文件中，并做一些小的修改，如下所示

```
const requests = new Set();
const fetchData = async (
  ......
) => {
  ......
    const json = await response.json();
 **return json;**  } catch (error) {
 **throw error;**  } finally {
    requests.delete(url);
  }
};
**export default fetchData;**
```

正如我们所看到的，我们将`fetchData`函数更改为一个纯函数，因为它接受特定的参数并返回一个 JSON 对象或抛出一个异常。它与任何组件或生命周期都不相关。我们将把这些部分移动到一个抽象的存储库模块中。

# 添加存储库

接下来让我们实现这个抽象存储库模块。

因此，让我们在`src`文件夹中创建一个新文件夹`repository`，并在该文件夹中创建一个新文件`Repository.ts`，并向其中添加以下代码

```
import {insertAll, clearAll, fetchAll} from '../database/Database';
import fetchData from '../network/FetchData';
import {FETCH_SUCCEED, FETCH_FAILED, GET_SUCCEED} from '../redux/Action';
import {store} from '../App';export default class Repository {
  static data: Data;
  static cachedData?: Photo[];

  static get = async () => {
    Repository.cachedData = await fetchAll();
    console.log(
      'get',
      Repository.cachedData.map(item => item.id),
    );
    store.dispatch({
      type: GET_SUCCEED,
      payload: {...Repository.data, photos: Repository.cachedData},
    });
  }; static refresh = async (query: string = '', pageIndex: number = 0) => {
    if (query === '') {
      return;
    }
    try {
      const ids = new Set(Repository.cachedData?.map(item => item.id));
      Repository.data = await fetchData(query, pageIndex);
      Repository.data?.photos.forEach(item => (item.keyword = query));
      // console.log('refresh, ids', ids);
      const newPhotos =
        Repository.data &&
        Repository.data?.photos &&
        Repository.data.photos.filter(item => !ids.has(item.id));
      if (newPhotos && newPhotos.length > 0) {
        console.log(
          'insert',
          newPhotos.map(item => item.id),
        );
        insertAll(newPhotos);
      }
      store.dispatch({
        type: FETCH_SUCCEED,
        payload: {
          ...Repository.data,
          photos: Repository.cachedData,
        },
      });
    } catch (error) {
      store.dispatch({type: FETCH_FAILED, payload: error})
    }
  }; static clear = async () => {
    await clearAll();
  };
}
```

正如我们看到的，我们在这个`Repository`类中实现了三个方法，`get()`从持久存储中获取保存的数据，`refresh()`从后端 API 获取新数据并保存到持久存储中，`clear()`清除数据库。

您可能还注意到这段代码中有一个新的动作，`GET_SUCCEED`。这是因为我们需要分开两个动作来区分从后端 API 获取数据的成功和从持久存储获取数据的成功。

所以我们需要将下面的代码添加到`Action.d.ts`中

```
**interface GetAction {
  type: string;
  payload: Data | Error | undefined;
}**
```

并将下面的代码放入`Action.ts`

```
**export const GET_SUCCEED = 'GET_SUCCESS';**......
**export const getSucceed = (data: Data): GetAction => ({
  type: GET_SUCCEED,
  payload: data,
});**
```

并将`Reducer.ts`更新为

```
import {FETCH_FAILED, FETCH_SUCCEED, **GET_SUCCEED**} from '../redux/Action';const INITIAL_STATE: ReducerStateType = {
  data: undefined,
  error: undefined,
};
const isData = (object: any): object is Data => object;const fetchReducer = (
    state = INITIAL_STATE,
    action: FetchAction,
  ): ReducerStateType => {
  switch (action.type) {
    case **GET_SUCCEED**:
      if (isData(action.payload)) {
 **state = {
          ...state,
          data: {
            ...action.payload,
            photos: action.payload?.photos,
          },**        };
      }
      break;
 **case FETCH_SUCCEED:
      if (isData(action.payload)) {
        state = {
          ...state,
          data: {
            ...action.payload,
            photos: state.data?.photos,
          },
          error: undefined,
        };
      }
      break;**    case FETCH_FAILED:
      ......
  }
  return state;
};
```

如我们所见，我们会以不同的方式处理`GET_SUCCEED`和`FETCH_SUCCEED`。

# 更新主页组件以从 SSOT 数据库获取数据

最后一步是将我们上面实现的所有模块组合在一起。所以让我们更新`Home`组件如下:

```
**import Repository from '../repository/Repository';** ......const Home = ({navigation}) => {
  const data: Data = useSelector(state => state.fetch.data);
  const error: Error = useSelector(state => state.fetch.error); const nextPage = async () => {
    const key = 'page';
    if (data?.next_page) {
      const page = decodeURIComponent(
        ......
      );
 **await Repository.refresh(data.photos[0].keyword, parseInt(page, 10) || 0);
      Repository.get();**    }
  }; const onSearch = async (query: string) => {
 **await Repository.clear();
    await Repository.refresh(query, 0);
    await Repository.get();**  }; useEffect(() => {
 **if (!data && !error) {
      (async () => {
        await Repository.get();
      })();
    }**  });
  ......
```

当应用程序启动时，您会发现我们正在调用`Home`组件中的`Repository.get()`。并且储存库将从永久存储器中获取一些保存的数据，并分派一个`GET_SUCCEED`动作。并且一旦缩减器接收到这个动作，它将更新状态。

在另一个场景中，如果用户输入一个搜索关键字，那么`Repository.refresh()`将被触发，然后存储库将清除持久存储并从后端 API 获取新数据，并将新数据保存到持久存储中。在这之后，另一个`Repository.get()`将被触发，然后状态将被更新，这与应用程序启动相同。

第三种情况是，一旦用户滚动到列表的底部，就会触发`Repository.refresh()`，然后存储库将获取下一页图片并将其附加到当前列表，并更新状态。

现在持久存储是唯一的事实来源，来自后端 API 的任何新数据都将首先保存到持久存储中，然后更新状态，然后更新视图。

现在让我们运行应用程序，你会看到如下

![](img/badd483d67ff4281f301814d02d20c47.png)

我们获取了一些大猩猩的图片并关闭了应用程序，然后再次启动应用程序，我们会看到大猩猩的图片在没有输入任何搜索关键字的情况下显示出来。

现在，我们已经在 React Native 上使用`WatermelonDB`实现了持久存储，这使得您能够显示一些旧数据，而不是一个空视图，即使网络不可访问或后端 API 关闭。在某些情况下，它可能会改善用户体验。

还有一些其他地方可以改进，比如我们使用平面列表来显示图像，但平面列表可能会消耗大量内存，所以我们将在中使用 RecyclerListView 来优化它

 [## 使用 React Native (4)构建图像浏览应用程序— RecyclerListView

### React Native 为 iOS 和 Android 带来了 React 的声明式 UI 框架。使用 React Native，您可以使用本机 UI 控件…

kxie0124.medium.com](https://kxie0124.medium.com/build-an-image-browsing-app-with-react-native-4-recyclerlistview-b3cb465e1ef7) 

欢迎阅读本文，欢迎任何评论。