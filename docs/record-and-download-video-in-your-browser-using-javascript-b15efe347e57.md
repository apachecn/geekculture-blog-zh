# 使用 Javascript 在浏览器中录制和下载视频

> 原文：<https://medium.com/geekculture/record-and-download-video-in-your-browser-using-javascript-b15efe347e57?source=collection_archive---------15----------------------->

## 在浏览器上录制视频和音频并使用 Javascript 下载的简单方法

![](img/04b74548f64c69833999c3b36025e885.png)

视频在我们的日常互联网活动中变得非常流行。你想知道一个网站支持视频需要做多难吗？公司很难将媒体技术引入他们的技术产品，因为这需要媒体和软件开发方面的知识。但是在过去的几年里，事情发生了变化；当 [WebRTC](https://webrtc.org/) 作为浏览器和移动设备的开放标准被引入时，软件开发人员可以轻松地使用它的 API，而不需要先进的媒体知识。

这篇文章将介绍一个简单的例子，通过你的浏览器录制视频和音频，然后回放或下载到你的本地驱动器。该代码将支持除 Safari 之外的大多数主流浏览器(Chrome、Firefox、Opera、Edge)。

我已经在我的 [GitHub repos](https://github.com/huynvk/webrtc_demos/tree/master/record_by_browser) 中使用 ReactJS 将代码放入一个示例中，但是您可以根据自己的喜好将代码片段自由集成到任何 Javascript 框架中。

app 的主要工作流程为:`initialize media stream`>`record media stream to blobs`>`combine recorded blobs into single video blob`>`play or download video`>`clean up`。

# 初始化媒体流

第一部分是初始化[媒体流](https://developer.mozilla.org/en-US/docs/Web/API/MediaStream)。一个流可以包含不同的[轨道](https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamTrack)。在这个示例中，我请求一个包含来自用户设备的视频和音频的流。

`initMediaStream`函数将从用户设备请求视频和音频，遵循我们在`constraints`对象中的规范。浏览器可能会要求用户在返回流之前获得许可。如果用户不允许，代码将抛出一个异常。我将在调用者处处理异常，不需要在这里担心它。

```
const initMediaStream = async () => {
  const constraints = {
    audio: {
      echoCancellation: { exact: true },
    },
    video: {
      with: 1280,
      height: 720,
    },
  };
  const stream = await navigator.mediaDevices.getUserMedia(
    constraints,
  );
  return stream;
};
```

# 将媒体流记录到 Blobs

为了录制媒体流，我们需要使用[媒体记录器](https://developer.mozilla.org/en-US/docs/Web/API/MediaRecorder)。它支持不同的视频编解码器。目前最新的是视频的`vp9`和音频的`opus`。下面的功能`detectMimeType`是检测我们将使用哪种类型的编解码器。我们不必指定 mimeType，但是设置最好的一个可以帮助我们拥有最好的质量和大小。您可能已经在代码中注意到，输出视频类型将是 [webm](https://en.wikipedia.org/wiki/WebM) 。

```
const detectMimeType = () => {
  const mimeTypes = [
    'video/webm;codecs=vp9,opus',
    'video/webm;codecs=vp8,opus',
    'video/webm',
  ];for (let mimeType of mimeTypes) {
    if (MediaRecorder.isTypeSupported(mimeType)) {
      return mimeType;
    }
  }return '';
};
```

现在是时候创建一个 MediaRecorder 来记录我们的视频了。在下面的函数`beginRecord`中，我们将:

*   初始化一个流(如前一节所述通过调用`initMediaStream`)
*   使用流作为输入创建 MediaRecorder 对象，并提供由`detectMimeType`函数检测到的`mimeType`。
*   支持两个回调函数`onStreamReady`和`onFinished`，这样调用者就可以为这些事件触发一些动作，比如在录制过程中在屏幕上显示视频或者将录制的数据捕获到内存中。
*   返回 MediaRecorder 对象。

使用这种实现，对于从流接收的每个数据，媒体记录器将把它放入 Blob 的数组中。我们在这里需要一个数组的原因是流可能会因为很多原因而被中断。每当它再次可用时，MediaRecorder 将记录一个新的 Blob。

```
export const beginRecord = async (onStreamReady, onFinished) => {
  const stream = await initMediaStream();
  onStreamReady(stream);
  const options = { mimeType: detectMimeType() };
  const recordedBlobs = [];const mediaRecorder = new MediaRecorder(stream, options);
  mediaRecorder.ondataavailable = (event) => {
    if (event.data && event.data.size > 0) {
      recordedBlobs.push(event.data);
    }
  };
  mediaRecorder.onstop = () => {
    onFinished(recordedBlobs);
    stopMediaStream(stream);
  };mediaRecorder.start();return mediaRecorder;
};
```

# 将 Blobs 合并到一个视频中

下面是一个简单的代码片段，您可以将一个 Blob 数组合并成一个 Blob。输出对象已准备好下载或回放。

```
const combineBlobs = (recordedBlobs) => {
  return new Blob(recordedBlobs, { type: 'video/webm' });
};
```

# 播放和下载视频

要播放或下载视频 Blob，我们需要为它创建一个 URL。

```
const createBlobURL = (blob) => {
  const url = window.URL.createObjectURL(blob);
  return url;
};
```

要播放录制的 blobs，我们需要:

*   将这些 blob 合并成一个博客(`combineBlobs`)
*   为它创建一个 URL(`createBlobURL`)
*   将 URL 设置为网站上视频元素的来源
*   播放视频元素

```
export const playRecordedBlobs = (videoElement, recordedBlobs) => {
  const blob = combineBlobs(recordedBlobs);
  const url = createBlobURL(blob);stopPlaying(videoElement);videoElement.controls = true;
  videoElement.src = url;
  videoElement.play();
};
```

我使用第三方库`file-saver`下载记录的 Blob，如下面的函数`download`所示。也可以用[普通 Javascript 代码](https://stackoverflow.com/questions/19327749/javascript-blob-filename-without-link)代替。

```
import FileSaver from 'file-saver';export const download = (
  recordedBlobs,
  fileName = 'RecordedVideo.webm',
) => {
  const blob = combineBlobs(recordedBlobs);
  return FileSaver.saveAs(blob, fileName);
};
```

如果想直接实时播放 stream，就把它作为一个视频元素的`srcObject`。

```
export const playStream = (videoElement, stream) => {
  stopPlaying(videoElement); videoElement.srcObject = stream;
  videoElement.play();
};
```

# 打扫

清理步骤不是强制性的，但建议这样做，尤其是对`stopMediaStream`。否则，您的网站将继续访问用户设备上的视频和音频。

```
const stopMediaStream = async (stream) => {
  stream.getTracks().forEach((track) => track.stop());
};export const stopPlaying = (videoElement) => {
  videoElement.pause();
  videoElement.src = null;
  videoElement.srcObject = null;
};
```

# 把东西放在一起

如前所述，我已经在 ReactJS 应用程序中将所有东西放在一起。app 有三个按钮:`Record`、`Play`和`Download`。这些按钮有处理程序，如下面的代码片段所述。您可以自己定制它，使其最适合您的 JS 框架。

`Record`按钮的处理程序:

```
const btnRecord_onClick = async () => {
  try {
    if (!recorder) {
      const mediaRecorder = await beginRecord(
        (stream) =>
          playStream(recordingVideoEl.current, stream),
        (recordedBlobs) => setData(recordedBlobs),
      );
      setRecorder(mediaRecorder);
    } else {
      recorder.stop();
      stopPlaying(recordingVideoEl.current); setRecorder(undefined);
      setRecorded(true);
    }
  } catch (err) {
    console.error(err);
  }
}
```

`Play`按钮处理程序:

```
const btnPlay_onClick = () => {
  try {
    if (!playing) {
      setPlaying(true);
      playRecordedBlobs(playingVideoEl.current, data);
    } else {
      stopPlaying(playingVideoEl.current);
      setPlaying(false);
    }
  } catch (err) {
    console.error(err);
  }
}
```

`Download`按钮的处理器:

```
const btnDownload_onClick = () => {
  try {
    download(data);
  } catch (err) {
    console.error(err);
  }
}
```

# 结论

在这篇文章中，我给出了一个使用普通 Javascript API 录制和下载视频的小例子。在我看来，使用内置函数非常简单，不需要任何其他第三方 API。虽然该脚本还不支持 Safari，但它支持所有其他主流浏览器，并且已经覆盖了很大一部分用户。

*原发布于*[*https://huynvk . dev*](https://huynvk.dev/blog/record-and-download-video-in-your-browser-using-javascript)*。*