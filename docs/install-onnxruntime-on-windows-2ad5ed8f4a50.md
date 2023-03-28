# 在 Windows 上安装 OnxxRuntime

> 原文：<https://medium.com/geekculture/install-onnxruntime-on-windows-2ad5ed8f4a50?source=collection_archive---------5----------------------->

视频版:[https://youtu.be/Tiq_vea5Eqg](https://youtu.be/Tiq_vea5Eqg)

如果您在 Python 中使用 OnnxRuntime 时遇到这个错误，那是因为您缺少 Cuda 依赖项或其他东西，

```
ImportError: cannot import name 'get_all_providers' from 'onnxruntime.capi._pybind_state
```

OnnxRuntime 并没有使它超级显式，但是要在 GPU 上运行 OnnxRuntime，你需要已经安装了 Cuda 工具包和 CuDNN 库。

首先检查你的机器，确保你有一个支持 Cuda 的卡。如果你有…