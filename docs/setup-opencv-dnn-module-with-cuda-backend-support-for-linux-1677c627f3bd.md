# 使用 CUDA 后端支持设置 OpenCV-DNN 模块(适用于 Linux)

> 原文：<https://medium.com/geekculture/setup-opencv-dnn-module-with-cuda-backend-support-for-linux-1677c627f3bd?source=collection_archive---------0----------------------->

![](img/9a9adfde7199682fda23f381320ea36e.png)

# 在本教程中，我们将使用 CUDA 后端支持从源代码构建 OpenCV(OpenCV-DNN-CUDA 模块)。

**重要提示:**OpenCV-DNN 模块只支持推理，所以尽管你可以从中获得更快的推理，但训练将与我们在没有 CUDA 后端支持的情况下设置的 OpenCV 相同。

# 步伐

1.安装 CUDA & cuDNN。

2.如果尚未安装，请安装必要的 Ubuntu 软件包。

3.下载并解压缩 opencv & opencv_contrib

4.设置 python 环境

5.使用 CMake 命令配置 OpenCV 库

6.为你的 NVidia GPU 从源代码构建 OpenCV。

# 我在 YouTube 上的视频！

# 我们开始吧！

## 步骤 1)安装 CUDA 和 cuDNN

为您的系统下载并安装 CUDA 及其相应的 cuDNN 归档文件。去 https://developer.nvidia.com/cuda-downloads 的[下载最新的 CUDA 工具包。您也可以从以前的 CUDA 版本的](https://developer.nvidia.com/cuda-downloads)[档案中下载以前的版本](https://developer.nvidia.com/cuda-toolkit-archive),或者从上面给出的 cuda-downloads 链接的参考资料部分下载。接下来，下载并复制 cuDNN 文件夹及其内容到我们安装 CUDA 的地方。你可以从 https://developer.nvidia.com/cudnn[下载 cuDNN 的最新版本。你也可以从 https://developer.nvidia.com/rdp/cudnn-archive 的 cudnn 档案下载以前的版本。](https://developer.nvidia.com/cudnn)

接下来，您需要在 bashrc 脚本中添加这些文件的路径。

之后，重新启动您的系统，并通过键入以下两个命令在终端中验证您的 CUDA 安装:

```
**nvidia-smi****nvcc -V**
```

**关注** [**本博客**](https://techzizou.com/?p=424) **学习如何在 Linux 上安装和设置 CUDA 和 CUDNN。**

## 步骤 2)安装必要的 Ubuntu 软件包，如果还没有安装的话。

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install build-essential cmake unzip pkg-config
sudo apt-get install libjpeg-dev libpng-dev libtiff-dev
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install libv4l-dev libxvidcore-dev libx264-dev
sudo apt-get install libgtk-3-dev
sudo apt-get install libblas-dev liblapack-dev gfortran
sudo apt-get install python3-dev
```

## 步骤 3)下载并解压缩 opencv & opencv_contrib

导航到主文件夹，下载 OpenCV 归档文件，将其解压缩，并将文件夹重命名为 *opencv* 。

```
cd ~
wget -O opencv-4.5.5.zip [https://github.com/opencv/opencv/archive/4.5.5.zip](https://github.com/opencv/opencv/archive/4.5.5.zip)
unzip -q opencv-4.5.5.zip
mv opencv-4.5.5 opencv
rm -f opencv-4.5.5.zip
```

接下来，下载 opencv_contrib 文件，解压并重命名为 *opencv_contrib* 。

**注意**:确保下载的 opencv_contrib 版本与之前下载的 opencv 档案完全相同。

```
wget -O opencv_contrib-4.5.5.zip [https://github.com/opencv/opencv_contrib/archive/4.5.5.zip](https://github.com/opencv/opencv_contrib/archive/4.5.5.zip)
unzip -q opencv_contrib-4.5.5.zip
mv opencv_contrib-4.5.5 opencv_contrib
rm -f opencv_contrib-4.5.5.zip
```

## 步骤 4)设置 python 环境

如果您的系统上还没有 virtualenv 和 virtualenvwrapper，请安装它们。

```
sudo pip install virtualenv virtualenvwrapper
```

使用`nano`打开 bashrc 脚本文件。

```
nano ~/.bashrc
```

输入以下导出路径，在底部设置 virtualenvwrapper 的路径并保存文件。(按`ctrl+x`、`y`和`Enter`保存文件)

```
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
source /usr/local/bin/virtualenvwrapper.sh
```

运行 source 来断言 bashrc 脚本在系统中的更改。

```
source ~/.bashrc
```

在终端中创建虚拟环境。我们将在虚拟环境中安装 OpenCV-DNN-CUDA 模块，而不是我们的基础环境。

```
mkvirtualenv opencv_dnn_cuda -p python3
```

安装 NumPy 包。

```
pip install numpy
```

通过输入以下命令激活虚拟环境:

```
workon opencv_dnn_cuda
```

## 步骤 5)使用 cmake 命令配置 OpenCV 以安装一些库

导航到我们在步骤 3 中创建的 *opencv* 文件夹。在其中创建另一个名为 *build* 的文件夹，并在其中导航。

```
cd ~/opencv
mkdir build
cd build
```

运行 CMake 命令来配置 OpenCV 的设置，您希望使用您选择的自定义库和函数从源代码构建 OpenCV。

将下面命令中的 CUDA_ARCH_BIN 参数更改为您的 GPU 的计算能力。在这里找到你的 GPU 的 cc。此外，如果使用不同的名称，请更改 PYTHON_EXECUTABLE 参数中的虚拟环境名称，否则保持不变。

```
cmake -D CMAKE_BUILD_TYPE=RELEASE \
	-D CMAKE_INSTALL_PREFIX=/usr/local \
	-D INSTALL_PYTHON_EXAMPLES=ON \
	-D INSTALL_C_EXAMPLES=OFF \
	-D OPENCV_ENABLE_NONFREE=ON \
	-D WITH_CUDA=ON \
	-D WITH_CUDNN=ON \
	-D OPENCV_DNN_CUDA=ON \
	-D ENABLE_FAST_MATH=1 \
	-D CUDA_FAST_MATH=1 \
	-D CUDA_ARCH_BIN=6.1 \
	-D WITH_CUBLAS=1 \
	-D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
	-D HAVE_opencv_python3=ON \
	-D PYTHON_EXECUTABLE=~/.virtualenvs/opencv_dnn_cuda/bin/python \
	-D BUILD_EXAMPLES=ON ..
```

在上面的 CMake 命令中，我们正在编译 OpenCV，同时启用了 CUDA 和 cuDNN 支持(分别使用 _CUDA 和 _CUDNN)。我们还启用了 OPENCV_DNN_CUDA 参数来构建具有 CUDA 后端支持的 DNN 模块。出于优化目的，我们还设置了 ENABLE_FAST_MATH、CUDA_FAST_MATH 和 WITH_CUBLAS。

# 第 6 步)最后，从源代码中构建 OpenCV-DNN 模块，为您的特定 NVidia GPU 提供 CUDA 后端支持

运行 make 命令，用上面配置的设置构建 OpenCV。(在终端中运行`nproc`，打印您的处理器号。在 j8 中用下面的 8 代替。)

```
make -j8
```

这将使用 CUDA 后端从源代码构建 OpenCV-DNN 模块。这个过程可能需要一个小时左右。

接下来，一旦在上面的 make 命令中完成了 OpenCV 的构建，就在您的系统上安装 OpenCV。运行以下命令:

```
sudo make install
sudo ldconfig
```

接下来，最后一步是在新安装的 OpenCV 库和我们的 python 虚拟环境之间创建一个符号链接。为此，您需要 OpenCV 绑定的安装路径。您可以通过 CMake 命令步骤输出**中安装路径配置来确定路径。**

在我的例子中，安装路径是***lib/python 3.6/site-packages/cv2/python-3.6***，这意味着我的 OpenCV 绑定在***/usr/local/lib/python 3.6/site-packages/cv2/python-3.6***中。检查你的”。所以“文件是。

您可以通过列出其内容来确认上面的路径。运行下面的`ls`命令:

```
ls -l /usr/local/lib/python3.6/site-packages/cv2/python-3.6
total 10104
-rw-r--r-- 1 root staff 10345488 Mar 23 07:06 cv2.cpython-36m-x86_64-linux-gnu.so
```

您将看到类似上面的输出，确认 cv2 绑定在这里。我有 Python 3.6 和解释文件名的 64 位架构。根据您的 Python 和系统架构，您的可能会有所不同。

接下来，导航到虚拟环境“site-packages”文件夹，最后使用`ln -s`命令为我们在上面找到的 cv2 绑定创建一个链接。这将在当前工作目录中创建一个 cv2.so [符号链接](https://devdojo.com/devdojo/what-is-a-symlink)，该目录是我们虚拟环境中的站点包文件夹。

```
cd ~/.virtualenvs/opencv_dnn_cuda/lib/python3.6/site-packages/
ln -s /usr/local/lib/python3.6/site-packages/cv2/python-3.6/cv2.cpython-36m-x86_64-linux-gnu.so cv2.so
```

注意:请确保仔细检查所有内容，因为即使路径不正确，该命令也会无声地失败，这意味着您不会收到警告或错误消息，并且输出也不会工作。所以要注意这一步。

就是这样！我们已经在 CUDA 后端支持下成功构建了 OpenCV-DNN 模块。

检查 cv2 是否安装正确。

```
workon opencv_dnn_cuda
pythonPython 3.6.9 (default, July 02 2019, 17:25:39)
[GCC 6.3.0 20170516] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import cv2
>>> cv2.__version__
'4.5.5'
>>>
```

# 使用下面的 test_DNN_CV.py 脚本检查 OpenCV 是否正确安装了 CUDA 后端支持。下面下载。

```
import numpy as np
import cv2 as cv
import timenpTmp = np.random.random((1024, 1024)).astype(np.float32)npMat1 = np.stack([npTmp,npTmp],axis=2)
npMat2 = npMat1cuMat1 = cv.cuda_GpuMat()
cuMat2 = cv.cuda_GpuMat()
cuMat1.upload(npMat1)
cuMat2.upload(npMat2)start_time = time.time()
cv.cuda.gemm(cuMat1, cuMat2,1,None,0,None,1)print("CUDA using GPU --- %s seconds ---" % (time.time() - start_time))start_time = time.time()
cv.gemm(npMat1,npMat2,1,None,0,None,1)print("CPU --- %s seconds ---" % (time.time() - start_time))
```

## 下载下面的脚本来测试 OpenCV-DNN。

[**下载测试 _DNN_CV.py**](https://techzizou.com/setup-opencv-dnn-cuda-module-for-linux/#test_dnn_cv)

# 下一个教程:

## 在 Linux 上使用 OpenCV-DNN 模块运行 YOLOv4 推理

[](https://techzizou007.medium.com/yolov4-inference-using-opencv-dnn-cuda-on-linux-904c16d2aea7) [## Linux 下基于 OpenCV-DNN-CUDA 的 YOLOv4 推理

### 在 Linux 上使用 OpenCV-DNN-CUDA 模块运行 YOLOv4 推理。

techzizou007.medium.com](https://techzizou007.medium.com/yolov4-inference-using-opencv-dnn-cuda-on-linux-904c16d2aea7) 

## 别忘了留下👏

## 祝您愉快！！！✌

# ♕·特奇佐·♕