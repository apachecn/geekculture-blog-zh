# 从任何东西构建 3D！虚拟现实&元宇宙世界大厦

> 原文：<https://medium.com/geekculture/build-3d-from-anything-vr-metaverse-world-building-ba58ea5a4c14?source=collection_archive---------2----------------------->

![](img/a671b69491956142531cfb5f806bce56.png)

Photo by [DeepMind](https://unsplash.com/@deepmind?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在这篇文章中，我们将讨论一个使用人工智能的 3D 物体构建的新实现。我们将讨论这可能如何影响其他技术，如虚拟现实和视频动画，所以请继续阅读。在我们深入这个话题之前，如果你想看这篇文章的视频版本，那么看看下面的链接。

传统上，3D 建模需要繁琐的任务，包括艺术技巧和技术知识。建模专家通常会使用 Blender 之类的软件来构建用于视频动画或游戏设计的 3D 模型，这通常需要几个小时甚至几天的时间。流行的 3D 例子可以在你最喜欢的电影中看到。还记得你第一次看《复仇者联盟》中阿凡达不可思议的画面或者绿巨人逼真的动作吗？如前所述，这些结果通常需要昂贵的预算和专门的时间。

我们将从这个领域的前沿公司 Nvidia 开始。Nvidia 是一家跨国公司，主要以设计图形处理单元和高性能计算应用而闻名。该公司发布了一个人工智能解决方案，可以在几秒钟内快速建模 3D 对象！太棒了，对吧？他们称之为 InstantNerf 的人工智能产品是一个机器学习神经网络模型，它接受 2D 图像或现实世界物体或风景的视频作为输入。然后，这将在几秒钟内变成一个三维表示！用于此的专用神经网络模型被称为神经辐射场或简称为 Nerf。Nerf 的第一个版本很慢，可能需要几个小时的训练，InstantNerf 顾名思义，几乎可以立即进行训练。这绝对是一个游戏改变者！神经网络通过填充 2D 图像的缺失部分来产生完整的 3D 版本。当你想到人工智能模型必须学习的所有其他功能，如照明、纹理、阴影和视点时，这就更令人难以置信了。在电影行业，复杂的 3D 场景通常占用大量内存，可能会影响工作效率。另一方面，Nerf 模型以小得多的内存消耗存储整个场景。

![](img/57d1e97be5df1ca262687c146229d262.png)

Photo by [David Marcu](https://unsplash.com/@davidmarcu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在需要设计数字版本或修改真实对象的场景中，InstantNerf 使游戏设计师和动画艺术家的 3D 建模更加高效。该解决方案也吸引了那些想要创建数字对象但几乎没有设计技能的人。这种虚拟现实用例的快速 3D 建模非常方便，并且比现状成本更低。与其他 3D 模型不同，InstantNerf 还可以生成捕捉对象的环境的 3D 副本。这有无限的可能性。InstantNerf 实现的实际 VR 示例可以在博物馆或旅游景点完成，其中部分或全部场地以数字方式传输。这将确保这些机构能够接触到更广泛的受众。数字化真实物体的伟大之处在于，我们仍处于早期阶段，随着时间的推移，结果显然会有所改善。你可以想象 5 年后，当 3D 世界建筑和虚拟现实技术正在以瞬时的速度生产高质量的模型时，那将是惊人的，不是吗？另一个有趣的实现可能是定制游戏建模，玩家可以决定以可插拔的方式使用他们客厅的模型。显然，像 Rockstar Games 这样生产 GTA 系列或 Maxis 的大型游戏公司，模拟人生游戏的创造者，在未来将不得不适应这样的用例。最重要的是，你终于可以在几年内实现你的“一号玩家”的梦想，所以抓紧了。

除了 InstantNerf，还有其他基于人工智能的 3D 建模解决方案可能没有这么快，但它们在某些领域产生了最先进的 3D 模型结果。我们将在这里快速浏览其中的几个，这样你就能知道 3D 世代空间中的其他人工智能发展。列表中的第一个是 BANMo，一个用于渲染动物或人的运动的神经网络解决方案。它捕捉 3D 空间中的精确运动，与以前的尝试相比，这意味着尽可能真实。某些姿势或动作对于人工智能模型来说很难复制，BANMo 在这些方面达到了最先进的结果。另一个 AI 模型是 I M Avatar。该模型用于 3D 重建头部，产生高质量的纹理和逼真的面部运动。最难建模的部分是嘴部内部或头发纹理，这是《我的化身》所改进的。排名第三的是 SHAPY，这是一个基于人工智能的解决方案，用于估计一个人的实际体型。这也许可以用于时装业的虚拟试衣。最后，HumanNerf 是一个基于 Nerf 的人体运动模型，它也改进了复杂人体运动的 3D 渲染。它还完全模拟了在用作输入的真实世界版本中可能不可见的身体部分。随着所有这些令人兴奋的 3D 建模创新的出现，人们很难抑制对未来的兴奋。

您认为这些模型的后续版本还会有哪些其他实现？请在评论中告诉我你的想法。

谢谢大家！

**资源**:

instant nerf:[https://developer . NVIDIA . com/blog/getting-started-with-NVIDIA-instant-nerfs/](https://developer.nvidia.com/blog/getting-started-with-nvidia-instant-nerfs/)

班默:【https://banmo-www.github.io/】T4

我是 M 头像:[https://ait.ethz.ch/projects/2022/IMavatar/](https://ait.ethz.ch/projects/2022/IMavatar/)

沙皮:[https://shapy.is.tue.mpg.de/](https://shapy.is.tue.mpg.de/)

https://grail.cs.washington.edu/projects/humannerf/

内尔夫:[https://nvlabs.github.io/instant-ngp/](https://nvlabs.github.io/instant-ngp/)