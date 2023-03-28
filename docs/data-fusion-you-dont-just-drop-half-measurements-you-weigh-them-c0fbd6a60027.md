# 数据融合:您不只是丢弃一半的测量值，而是对它们进行称重

> 原文：<https://medium.com/geekculture/data-fusion-you-dont-just-drop-half-measurements-you-weigh-them-c0fbd6a60027?source=collection_archive---------59----------------------->

更新:每个手机中工作数据融合的非常简单的例子:全景照片。拼接图像是一个困难的图像处理问题，增加陀螺仪测量使它变得简单。

Comet-ML 的时事通讯发到了我的邮箱，差点让我把咖啡洒了，其中一段引起了我的注意:

[***来自 CVPR 2021 年:特斯拉的安德烈·卡帕西关于基于视觉的自动驾驶汽车的方法***](https://www.youtube.com/watch?v=g6bOwQdCJrc&list=PLvXze1V52Yy2OY67mz2Jy-JcnEw8GUZEl&index=11&utm_campaign=Monthly%20newsletter&utm_source=hs_email&utm_medium=email&utm_content=136820874&_hsenc=p2ANqtz-_77kjQ26nVBwSMN5ryfLcLh1OElbw-VSukWSRV3EVQHIhmNFmp-4tJ8tBVioVoI6FStxcT7MCHwRxO7vduGez-3xA55Q)

*特斯拉正在加倍努力实现其视觉优先的自动驾驶汽车，并将在未来的版本中完全停止使用雷达传感器。*

*从一开始，特斯拉就采取了与大多数其他公司不同的方法来开发不依赖激光雷达的自动驾驶汽车。相反，他们使用了雷达传感器和放置在车辆周围的 8 个摄像头的组合。*

*在 CVPR 2021 的这次演讲中，Andrej Karpathy 解释了这种方法面临的挑战，以及* [*为什么特斯拉决定专注于摄像头*](https://is.t.hubspotemail.net/e2t/tc/VWrRkc108hLtW4D-SMR6HsJL9V7Wb0N4tp8mZN11fKVG5nxG7V3Zsc37CgSzWW2czlc066WKnMW4P97pL3hnWzqW2vFyD9240GyFVXq8Nh8zrmXjW1Cj0-w1Jkv2jN70bqcydk4bzVRLjhS7086yjW5LY67q1XBqvcN4S8fBXp5-9TW6RNmMF3MZGkkW7mp7BM7bK4GYW933gHR3HpBnyW1wvT1v93G12tW1-p5cv6_HlFSW9lGLm27R8rqFW6198lq2ZSv5gW4ghzsp3Xc4DYW8CtYw13NGcqvVytCcG612Sq0Vsxqqy4-JNtkW8NLwj93W6q4mW1cwqlP1R2MKHM297JnmSl12W6HCs445g6KqLW4fF6GG58JKgdW107bF57QHGq2W5_-rCW1j0FR3W8xwwCG8Jg6m0W1bfCYB4vZRCYW95wv9696Ts1PW1B2tgR7m3Nq5W2ds6Bl7LyNpm3mfk1) *并停止使用雷达传感器。使用不同类型传感器的主要挑战是传感器融合。* [*正如埃隆马斯克所说:*](https://is.t.hubspotemail.net/e2t/tc/VWrRkc108hLtW4D-SMR6HsJL9V7Wb0N4tp8mZN11fKX73p_9rV1-WJV7CgFdGW2SvmfG6njMVCN4jBJcJk-pQJW7TBM_Q7-vqRmN7HFdMRdgnTRW98z2s28P98pXW6GtTpb7tj_yGVLqCT7660B1zW5J3rSV10YFfBW8Pm0md6Cl4B4W3WL5N48wscC9W3RJ0p87DlSVmW27fc4b8QrlgSW5wCQDZ2LdT9-W5nd2qR12CXNMW6n5jt11SfrwqW2Mylyl4ZZp6mN3yHmR8sHDRGW4MZp3w9dhRb5W1QBqBJ2G5W97N3Xn0x3gy3vDW89RHvR8LTqh-W2mK-zb7vr8LgW689Btc3dY5dpW3ZHxJ5909QvNW5KRftb2FDCpDW5NqTDp3Wtlh235vY1) *“当雷达和视觉不一致时，你相信哪一个？视觉具有更高的精度，因此在视觉上加倍努力比传感器融合更好。”*

如果你有传感器数据，从现实世界的传感器，你不只是丢弃一半，因为其他人更精确:考虑一个我工作了几年的场景:以 x 马赫的速度飞行的传感器平台(或以每秒 30-60 米，但这些的倍数)接收到一个无线电信号，x 马赫的一个在那之后存活了 15 秒，30-60 米的平台最多存活一分钟或两分钟。

使用“基于距离”的技术，如到达时间或到达时间差，可以获得非常精确的测量，除了当它们提供高分辨率时，它们给你至少两个交点(你需要 3 个传感器或两个平台来解决模糊)和到达角度测量，它们是如此“精确”，以至于由于衰落，你将幸运地获得正确的象限，还有到达频率差(但我知道你已经厌倦了)。

过去 80 年来研究它的领域是 ELINT，阅读 Steve Blanks [写下它是多么有趣](https://steveblank.com/2009/04/13/story-behind-the-secret-history-part-iv-undisclosed-location-library-hours/)。数据融合将导致飞机在导弹射程之外选择攻击或机动，如果做错了，你就完蛋了。不，你不能进行更多的测量——处理现有的测量，否则有暴露和被击落的风险。

为了评论上面的营销宣传:神经网络首先不利于数据融合，有许多专门针对该问题开发的算法——自举过滤器、粒子过滤器、进化算法或我的[自己的基于霍夫变换的东西](https://www.academia.edu/7525934/Multi_Cluster_Agent_Based_Emitter_Geolocation_using_Hough_Transform_Data_Fusion)。与摄像机相比，NN 不会学习雷达测量的物理和限制，但人类工程师设计的系统会学习，我们可以通过为不同类型的测量分配适当的权重，将人类知识纳入系统。

有一篇关于贝叶斯与非贝叶斯的哲学文章——数据融合的参数估计，但它需要大量的数学来支持讨论，并将发表在我在 www.sci-blog.com[的博客上](https://www.sci-blog.com/)