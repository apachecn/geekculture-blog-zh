# 用 Next.js 构建一个 YouTube 队列:第 1 部分

> 原文：<https://medium.com/geekculture/build-a-youtube-queue-with-next-js-part-1-2d1d8954c705?source=collection_archive---------16----------------------->

![](img/e76d98c0abd01d019615ad57361ebc22.png)

Photo by [Alexander Shatov](https://unsplash.com/@alexbemore?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

最近几个月，我迷上了共享的在线空间。像 Clubhouse、Discord 这样的应用程序，以及 Beatsense 这样不太为人所知的平台，对于维护整个疫情的社区意识是不可或缺的。

在本教程中，我们将创建一个共享空间，您可以在其中添加和观看 Youtube 视频，并与其他用户实时聊天。在第 1 部分中，我们将关注视频。

# 先决条件

*   熟悉 React
*   熟悉 Next.js
*   Node.js 10.13 或更高版本
*   创建下一个应用程序(`npm i create-next-app`)

# 入门指南

首先，您需要初始化一个新的 Next.js 项目并设置 TailwindCSS。在命令行中，导航到您选择的位置并运行:

```
npx create-next-app -e with-tailwindcss youtube-social
```

要安装 Tailwind 及其依赖项，请在 CLI 中导航到`youtube-social`文件夹并运行:

```
npm install -D tailwindcss@latest postcss@latest autoprefixer@latest
```

然后生成默认[配置文件](https://tailwindcss.com/docs/configuration) `tailwind.config.js`和`postcss.config.js`:

```
npx tailwindcss init -full -p
```

要启动开发服务器运行:

```
npm run dev
```

当您转到 [http://localhost:3000](http://localhost:3000/) 时，您应该会看到默认的“欢迎使用 Next.js”页面。

# 播放器组件

我们将使用由皮特·库克创建的漂亮的 [react-player/youtube](https://github.com/CookPete/react-player) 库来轻松处理与 youtube 视频队列相关的视频事件。

首先，使用 CLI 在您的项目中安装`react-player`:

`npm install react-player`

在项目的根级别创建一个名为`components`的新文件夹。我们将在这个文件夹中存储应用程序的独立“组件”,使它们可以重复使用并易于修改。

将名为`player.js`的新文件添加到`components`文件夹中。这将包含 Youtube 嵌入。

在`player.js`中，像这样导入`react-player/youtube`:

```
import ReactPlayer from 'react-player/youtube'
```

接下来，创建一个名为`Player`的接受道具的功能组件。我们将传入的道具是一个 Youtube 视频 ID 和一个在视频结束时执行的回调函数。

在其中，声明一个名为 videoURL 的变量，该变量保存基本的 Youtube 视频 URL 和视频 ID，并返回一个包含一个`<ReactPlayer>`的`<div>`:

```
export default function Player(props) {
    const videoURL = "https://www.youtube.com/watch?v=" + props.videoId
  return(
    <div>
      <ReactPlayer />
    </div>
  )
}
```

ReactPlayer 接受各种各样的[道具](https://github.com/CookPete/react-player#props)，这些道具可以帮助我们管理与玩家相关的事件。我们将使用:

`url`:(String)Youtube 视频的 URL`playing`:(Boolean)视频自动播放`onEnded`:(回调)视频结束时执行的函数`config`:(JSON)Youtube embed 的选项

```
export default function Player(props) {
    const videoURL = "https://www.youtube.com/watch?v=" + props.videoId
    return(
        <div>
            <ReactPlayer
                url={videoURL}
                playing={true}
                onEnded={props.onEnd}
                config={{
                    youtube: {
                        playerVars: {
                            autoplay: 1,
                            controls: 1
                        }
                    }
                }}
            />
        </div>
  )
}
```

将`Player`组件导入`index.js`。删除外层`<div>`中的所有样板代码，将播放器放在页面的`<main>`部分下。作为占位符，我们将为一个 [Kurzgesagt](https://www.youtube.com/watch?v=JXeJANDKwDc) 视频传入一个硬编码的`videoId`:

```
import Head from 'next/head'
import Player from '../components/player'export default function Home() {
  return (
    <div className="flex flex-col items-center justify-center min-h-screen py-2">
      <Head>
        <title>Youtube Social</title>
        <link rel="icon" href="/favicon.ico" />
      </Head> <main>
        <Player videoId={"JXeJANDKwDc"}/>
      </main>
    </div>
  )
}
```

在编辑器中保存您的工作，页面应该会重新加载并开始播放视频。

# 队列组件

现在，让我们设置队列组件。我们希望从 Youtube API 中获取 JSON 格式的视频数据，提取视频 id、标题和频道标题，并将它们显示为队列的一部分。

我们将从一些模拟 Youtube API 响应的硬编码数据开始。在项目的根目录下创建一个名为`data`的文件夹。然后在其中添加一个名为`test-queue.json`的文件，并复制以下测试数据:

```
{
    "items": [
        {
            "id": {
                "videoId": "3qqzz9a8pMQ"
            },
            "snippet": {
                "title": "Grimes - Aeon / Kill V. Maim / Di-Li-Do (Acapella) [Mixed]",
                "channelTitle": "sef",
                "thumbnails": {
                    "default": {
                        "url": "https://i.ytimg.com/vi/3qqzz9a8pMQ/default.jpg",
                        "width": 120,
                        "height": 90
                    }
                }
            }
        },
        {
            "id": {
                "videoId": "HlLx7oE7q3I"
            },
            "snippet": {
                "title": "Hozier - Dinner & Diatribes (Official Video)",
                "channelTitle": "HozierVEVO",
                "thumbnails": {
                    "default": {
                        "url": "https://i.ytimg.com/vi/Iq5gesj6kmw/default.jpg",
                        "width": 120,
                        "height": 90
                    }
                }
            }
        },
        {
            "id": {
                "videoId": "M9teCJVTr_s"
            },
            "snippet": {
                "title": "Yves Tumor - Licking An Orchid (ft. James K)",
                "channelTitle": "Yves Tumor",
                "thumbnails": {
                    "default": {
                        "url": "https://i.ytimg.com/vi/M9teCJVTr_s/default.jpg",
                        "width": 120,
                        "height": 90
                    }
                }
            }
        }
,
        {
            "id": {
                "videoId": "p2Rro6TQgpU"
            },
            "snippet": {
                "title": "FKA twigs - home with you",
                "channelTitle": "FKA twigs",
                "thumbnails": {
                    "default": {
                        "url": "https://i.ytimg.com/vi/p2Rro6TQgpU/default.jpg",
                        "width": 120,
                        "height": 90
                    }
                }
            }
        },
        {
            "id": {
                "videoId": "286jXjwdst0"
            },
            "snippet": {
                "title": "Apashe ft. Instasamka - Uebok (Gotta Run) [Official Video]",
                "channelTitle": "Kannibalen Records",
                "thumbnails": {
                    "default": {
                        "url": "https://i.ytimg.com/vi/gM4xZy39kNE/default.jpg",
                        "width": 120,
                        "height": 90
                    }
                }
            }
        },
        {
            "id": {
                "videoId": "xqYFU1_SiBo"
            },
            "snippet": {
                "title": "Untitled (2012 Remaster)",
                "channelTitle": "Interpol",
                "thumbnails": {
                    "default": {
                        "url": "https://i.ytimg.com/vi/xqYFU1_SiBo/default.jpg",
                        "width": 120,
                        "height": 90
                    }
                }
            }
        }
    ]
}
```

将名为`queue.js`的新组件添加到`components`文件夹中。`queue`组件应该接受一个名为`data`的包含视频数据的道具。数据将被映射到唯一的行，如下所示:

```
export default function Queue(props) { return (
            <div className="max-w-sm max-h-96 overflow-y-scroll">
                { props.data.map((item, index) => {
                        return (
                            <div key={index} className="flex flex-row items-center w-60">
                                <h1 className="text-lg">{index + 1}</h1> <div className="text-sm">
                                    <h3 className="font-bold">{item.snippet.title}</h3>
                                    <p>{item.snippet.channelTitle}</p>
                                </div>
                            </div>
                        )
                    })
                }
            </div>
        )
}
```

我们将使用`useState` [钩子](https://reactjs.org/docs/hooks-intro.html)来跟踪和管理视频数据的状态。一旦视频从视频数据列表中删除，`useState`将帮助我们动态更新`Queue`组件。

在`index.js`文件中，导入`TestQueue`、`Queue`组件和`useState`:

```
import TestQueue from '../data/test-queue.json'
import Queue from '../components/queue'
import { useState } from React
```

在`Home`组件中，添加带有变量`videoData`和`updateVideoData`的`useState`钩子。将`videoData`的初始状态设置为`TestQueue.items`:

```
const [videoData, updateVideoData] = useState(TestQueue.items)
```

我们将使用带顺风的[柔性盒](https://tailwindcss.com/docs/flex-direction)并排放置`Player`和`Queue`组件。我们还将把`videoData`作为道具传递给`Queue`组件。这将使`Queue`组件随着`videoData`的变化而更新。

```
<main>
        <div className="flex flex-row">
            <Player videoId={"JXeJANDKwDc"}/>
            <Queue data={videoData}/>
        </div>
</main>
```

# 玩家事件

您可能会注意到，第一个视频播放完毕，但是队列中的下一个视频在结束后不会自动开始播放。为了实现这个行为，我们需要使用`Player`的`onEnd`属性来传入一个回调，该回调在队列中启动一个移位，以及当前视频。

让我们从在`index.js`文件中为`currentVideoId`添加一个`useState`钩子开始。将默认值设置为队列中的第一个视频:

```
const [currentVideoId, updateCurrentVideoId] = useState(videoData[0].id.videoId)
```

现在将`currentVideoId`向下传递到`Player`组件:

```
<Player videoId={currentVideoId}/>
```

接下来是`onEnd`回调。我们需要创建一个函数来更新队列和当前视频，然后通过`onEnd`属性将它传递给`Player`:

```
const playNext = () => {
    videoData.shift()
    if (videoData.length > 0) {
      updateCurrentVideoId(videoData[0].id.videoId)
    }
  }<Player videoId={currentVideoId} onEnd={playNext}/>
```

# 结论

瞧，你现在有一个基本的 YouTube 播放列表应用程序！在下一部分，我将向您展示如何使用 [YouTube 数据 API](https://developers.google.com/youtube/v3) 来实现搜索。这个代码在 [Github](https://github.com/khalea/youtube-social) 上也有。