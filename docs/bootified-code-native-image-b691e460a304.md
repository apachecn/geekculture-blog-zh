# 引导代码→本机映像

> 原文：<https://medium.com/geekculture/bootified-code-native-image-b691e460a304?source=collection_archive---------0----------------------->

## Spring Boot 本地通过云本地构建包

GraalVM 本机映像越来越受欢迎，因为它是无服务器式的零扩展、按需向外扩展/向内扩展架构，在这种架构中，容器引导时间和运行时内存占用在技术和商业意义上都起着重要作用。传统上，java 应用程序是由运行时编译的，它通常会进行大量的运行时优化。内存需求通常很高，因为基础设施需要适应这些运行时优化。GraalVM 支持提前编译，生成的二进制文件是机器优化的。这个机器优化的本机二进制文件拥有它需要的一切，因此生成的映像不需要打包 JRE 运行时。由于这些二进制文件被预编译成本机代码，它大大缩短了启动时间，因为这些应用程序可以在几秒钟内启动。此外，不需要考虑额外的基础设施来进行运行时优化，因为它是预编译的，并且部分应用程序可以在构建时初始化。这降低了运行本机映像的整体映像大小和内存需求。这使得它最适合基于容器的环境，因为我们只需构建一次，就可以在任何有容器运行时的地方运行它。

尽管它仍处于早期，但它在 Spring 扮演重要角色的云原生空间中开辟了许多可能性。对原生映像的最初支持是从 Spring 5.1 开始提供的。从那以后，Spring 和 GraalVM 团队一直在努力改进整体支持。Spring Graal 支持仍然是试验性的，[spring-graalvm-native](https://github.com/spring-projects-experimental/spring-graalvm-native)repo 提供了从源代码构建本机映像的示例和说明。

开始使用 Spring 原生映像的最佳方式是通过**云原生构建包(CNB)** 。从 Spring boot 2.3 开始，增加了对使用云原生构建包(CNB)从源代码构建符合 OCI 的映像的支持。**pake to-build packs**——CNB 的一个参考实现支持为基于 spring boot 的应用构建本地映像。[pake to-build packs/Java-native-image](https://github.com/paketo-buildpacks/java-native-image)是元构建包，它提供创建本机映像所需的构建包位。

![](img/89735e07af5e30c26d294654d32dd4bb.png)

Spring boot native

让我们有一个简单的 spring boot 应用程序，并让 paketo-buildpacks 原生映像构建器来构建原生映像。如果你想了解更多关于 CNB 和 paketo-buildpacks 的信息，我已经写了一篇单独的[文章](/@srinivasan.surprise/unpack-cloud-native-buildpacks-9959b601424b)。

本文中使用的源代码可以从[这里](https://github.com/srinivasa-vasu/spring-boot-k8s.git)获得。探索 CNB 最快的方法是通过`[pack-cli](https://github.com/buildpacks/pack/releases/tag/v0.15.1)`

```
╰─ **pack suggest-builders**
Suggested builders:
 Google:                gcr.io/buildpacks/builder:v1      Ubuntu 18 base image with buildpacks for .NET, Go, Java, Node.js, and Python
 Heroku:                heroku/buildpacks:18              heroku-18 base image with buildpacks for Ruby, Java, Node.js, Python, Golang, & PHP
 Paketo Buildpacks:     paketobuildpacks/builder:base     Ubuntu bionic base image with buildpacks for Java, NodeJS and Golang
 Paketo Buildpacks:     paketobuildpacks/builder:full     Ubuntu bionic base image with buildpacks for Java, .NET, NodeJS, Golang, PHP, HTTPD and NGINX
 Paketo Buildpacks:     paketobuildpacks/builder:tiny     Tiny base image (bionic build image, distroless run image) with buildpacks for Golang> This will list all the CNB based trusted reference implementation builders.
```

paketo-buildpacks 有三个特别的构建器——完全、基本和极小，每个构建器的不同之处在于捆绑在基本映像中的库和包。在这种情况下，让我们使用微型构建器，因为该构建器提供的运行时映像是分布式的，只够运行应用程序。让我们检查一下小构建器，找出捆绑的构建包。

```
╰─ **pack inspect-builder paketobuildpacks/builder:tiny**
Inspecting builder: paketobuildpacks/builder:tinyDetection Order:
 ├ Group #1:
 │  └ paketo-buildpacks/java-native-image@4.5.0
 │     └ Group #1:
 │        ├ paketo-buildpacks/graalvm@3.4.0
 │        ├ paketo-buildpacks/gradle@3.4.0                      (optional)
 │        ├ paketo-buildpacks/leiningen@1.2.1                   (optional)
 │        ├ paketo-buildpacks/maven@3.2.1                       (optional)
```

它在检测顺序中有一个 java-native-image 构建包。基于构建时输入，构建器中的`lifecycle`编排这些构建包以生成符合 OCI 的可运行映像。

pack-cli 的另一种替代方法是使用 Spring 的原生构建工具集成支持。Spring boot 添加了一个构建任务，以利用 maven/gradle 的云原生构建包特性。如果启用了 CNB 构建任务，默认情况下它使用 paketo-buildpacks 构建器。但是，这可以在构建时定制。更多关于 gradle build 定制的信息请点击[这里](https://docs.spring.io/spring-boot/docs/current/gradle-plugin/reference/htmlsingle/#build-image)。

让我们看看 gradle 构建文件中的更改，以启用 CNB 构建步骤。

```
**bootBuildImage** **{
**   pullPolicy = "IF_NOT_PRESENT"
   imageName = "harbor.sys.humourmind.com/kna/spring-kloud-k8s:1.0"
   environment = ["BP_BOOT_NATIVE_IMAGE" : "1", "BP_JVM_VERSION" : "11"]
   builder    = "paketobuildpacks/builder:tiny"
**}**---
**bootBuildImage**
> is the CNB build task
**environment**
> passes build time inputs to the builder
> BP_BOOT_NATIVE_IMAGE would activate the spring-graalvm-native image [buildpack](https://github.com/paketo-buildpacks/spring-boot-native-image)
**builder** > to reference paketobuildpacks/builder:tiny builder during the build phase
```

Graal VM 原生映像工具使用字节码工具，因此它需要知道在映像生成过程中可以到达的所有字节码。这是一项耗时且占用大量内存的操作。因此，最好在本地工作站上为 docker 守护进程分配更高的内存(可能至少 6GB ),这样图像生成就不会出现任何 OOM 问题。

有两种方法可以构建映像——通过 gradle 或 pack-cli。来建造 via gradle，

```
gradle bootBuildImage
```

此任务构建计算机优化的本机容器映像。要通过 pack-cli 构建，

```
pack build harbor.sys.humourmind.com/kna/spring-kloud-k8s:2.0 -B paketobuildpacks/builder:tiny -e BP_BOOT_NATIVE_IMAGE=1 -e BP_JVM_VERSION=11
```

在图像创建过程中，我们可能会遇到一些类初始化的问题。Graal 的初始设计倾向于在构建时进行类初始化，而在 Graal 19 . x 及更高版本中，这一点已被更改为运行时。如果我们碰巧遇到了问题，那么我们可以通过构建时输入`--initialize-at-build-time`和`--initialize-at-run-time`来修复那些与类初始化相关的问题。多个类或包可以作为逗号分隔的值传递。另一个有助于理解类初始化层次结构的构建输入是`+H:TraceClassInitialization=true`示例输入，

```
environment = ["BP_BOOT_NATIVE_IMAGE" : "1", **"BP_BOOT_NATIVE_IMAGE_BUILD_ARGUMENTS": " -H:TraceClassInitialization=true --initialize-at-build-time=org.springframework.boot.logging"**]
```

我们在不到几秒的时间内就启动并运行了 spring 本机映像！

```
2020-12-02 09:06:32.921  INFO 1 --- [           main] o.s.b.web.embedded.netty.NettyWebServer  : Netty started on port(s): 8080
2020-12-02 09:06:32.923  INFO 1 --- [           main] i.h.kloudnative.KloudNativeApplication   : **Started KloudNativeApplication in 0.107 seconds (JVM running for 0.112)**
```

这个图像是基于 Ubuntu bionic 构建的，这是由 paketobuildpacks/builder 提供的基本运行图像:tiny。使用 [dive](https://github.com/wagoodman/dive) ，我们可以找到各种连接的图层和图像结构，这是一个检查图像的优秀工具。如果我们检查构建的图像，基本层看起来会有些相似，

```
"Layers": [
**"sha256:906014996d9387b59468387ffdc46993c459ddf00f90e6324312f25f04ca4380",
"sha256:0acf37ff77b2c3ece119a8c192a4dfb92f07d486fd70e69dac1f069ee430df07",** "sha256:583a10d296e30d621fc8ff791c7b3ebc0da7357b10dd03d92068e92c4c31ac7b",
"sha256:52da9578e67922c62d17a1e3d4a908e0b3e9e46f92da45d37aa5d615154073a2",
"sha256:e81d17f1c46a1828c5e9bf55c81ea8af5965e7ab47e917fd5bd472a3dd8463b6"
]
```

假设我们想用一个不同的运行映像交换基本 OS 文件系统(用户空间)层。在这种情况下，传统上，我们必须使用更新/修补的基础层来重建应用程序和其他中间映像。当我们必须大规模实施时，这通常会非常耗时且操作密集。有了云原生构建包，更新/修补基础层就像代码还原一样简单，应用的清单元数据需要用基础层的信息进行更新。`pack rebase`几秒钟后，工作完成，图层重置开始。

```
pack rebase harbor.sys.humourmind.com/kna/spring-kloud-k8s:2.0 --run-image humourmind/cnb-bionic-run@sha256:eb7edb490c2661ce80087b404b29cb593a22a6d877aad188827b482949c23280
```

如果我们再次检查图像，基本层将被更新为新的仿生运行图像参考，

```
"Layers": [
**"sha256:c8d644964be895653b0d5334e287115920beba57953249711b701df70e23d81d",
"sha256:7f1c64917d142dd155ba2b0111689c11fc8e59f644bca95ff660b263246080c7",
"sha256:c009f0e83ff5223756125dc4a2ab0428723e146601f7cc593fcd0878019fb1e7",** "sha256:583a10d296e30d621fc8ff791c7b3ebc0da7357b10dd03d92068e92c4c31ac7b",
"sha256:52da9578e67922c62d17a1e3d4a908e0b3e9e46f92da45d37aa5d615154073a2",
"sha256:e81d17f1c46a1828c5e9bf55c81ea8af5965e7ab47e917fd5bd472a3dd8463b6"
]
```

如果我们重新运行应用程序，一切都完好无损，ABI 契约确保了应用程序和用户空间层之间的二进制兼容性。

```
2020-12-02 09:24:56.265  INFO 1 --- [           main] o.s.b.web.embedded.netty.NettyWebServer  : Netty started on port(s): 8080
2020-12-02 09:24:56.267  INFO 1 --- [           main] i.h.kloudnative.KloudNativeApplication   : **Started KloudNativeApplication in 0.15 seconds (JVM running for 0.153)**
```

虽然这不是一个真正成熟的应用程序，但其目的是让您了解集成是如何进行的，以及最快的方法是什么。弹簧支撑还在α。这意味着让一系列更全面的 Spring 项目实现原生功能需要一些时间。早期的迹象很有希望，未来的道路看起来相当有趣。

> ***参考文献:*****CNB**
> [https://buildpacks.io/](https://buildpacks.io/)
> **paketo-build packs**
> [https://paketo.io/](https://paketo.io/)
> 潜
> [https://github.com/wagoodman/dive](https://github.com/wagoodman/dive)
> **pack-CLI**
> [https://github.com/buildpacks/pack](https://github.com/buildpacks/pack)
> **来源**