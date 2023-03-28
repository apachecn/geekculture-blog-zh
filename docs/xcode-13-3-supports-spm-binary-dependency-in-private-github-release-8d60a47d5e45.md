# Xcode 13.3 支持私有 GitHub 版本中的 SPM 二进制依赖

> 原文：<https://medium.com/geekculture/xcode-13-3-supports-spm-binary-dependency-in-private-github-release-8d60a47d5e45?source=collection_archive---------2----------------------->

在这篇博文中，我将分享一个在 [Xcode 13.3 发行说明](https://developer.apple.com/documentation/Xcode-Release-Notes/xcode-13_3-release-notes)中没有提到的新功能的细节。当开发专有的、封闭源代码的库时，需要将代码作为二进制文件提供以保护其知识产权的开发人员会对这个特性感兴趣。

要以二进制形式作为 Swift 包分发代码，请创建一个包含二进制文件的 XCFramework 包。然后，使这个包在本地或服务器上可用。下面是一个分发存储在服务器上的二进制框架的`Package.swift`清单示例:

```
// swift-tools-version:5.3
import PackageDescriptionlet package = Package(
    name: "MyLibrary",
    platforms: [
        .macOS(.v10_14), .iOS(.v13), .tvOS(.v13)
    ],
    products: [
        .library(
            name: "MyLibrary",
            targets: ["SomeRemoteBinaryPackage"])
    ],
    dependencies: [],
    targets: [
        .binaryTarget(
            name: "SomeRemoteBinaryPackage",
            url: "[https://url/to/some/remote/xcframework.zip](https://url/to/some/remote/xcframework.zip)",
            checksum: "The checksum of the ZIP archive that contains the XCFramework."
        )
    ]
)
```

xcframework.zip 文件需要在服务器上公开访问。但是，如果您需要将公共访问限制为经过身份验证的用户，该怎么办呢？

`.netrc`文件格式通常用于 HTTP 请求的自动认证。

```
machine <example.com> login <my-user-name> password <my-password>
```

它通常驻留在用户的主目录中(`~/.netrc`)。通过阅读 [gnu 文档](https://www.gnu.org/software/inetutils/manual/html_node/The-_002enetrc-file.html)了解更多关于 netrc 的信息。

我以前的同事 [Stan Stadelman](https://twitter.com/StadelmanStan) 将`netrc`引入 SPM，以支持面向非 git 二进制依赖主机的基本身份验证。

[](https://github.com/apple/swift-package-manager/pull/2833) [## 通过 sstadelman Pull 请求#2833 添加对 Netrc 下载程序的支持…

### 实现了对 netrc 文件的处理。匹配二进制工件主机的连接设置，并返回基本…

github.com](https://github.com/apple/swift-package-manager/pull/2833) 

用户无需选择使用 netrc，因为在 Swift Package Manager 中默认情况下[选项是打开的。](https://github.com/apple/swift-package-manager/blob/5a94f3c4625158c4dd8da6c92b6800ce2cc0e1e7/Sources/Commands/Options.swift#L144)

```
 /// Whether to load .netrc files for authenticating with remote servers
    /// when downloading binary artifacts or communicating with a registry.
    [@Flag](http://twitter.com/Flag)(inversion: .prefixedEnableDisable,
          exclusivity: .exclusive,
          help: "Load credentials from a .netrc file")
    var netrc: Bool = true
```

SPM 将自动检测`.netrc`文件并应用所需的`Authentication`标题。此外，通过选项`netrc-file <netrc-file>`，您可以指定不同的位置。

Xcode 12.5 提供了这项功能。然而，社区指出这并不[对一个私有的 GitHub 版本](https://forums.swift.org/t/spm-binary-dependency-in-private-github-release/52514/3)有效。

首先，您不能使用浏览器下载 URL，因为 HTTP 请求会导致 302 重定向。您必须[查找资产名称](https://forums.swift.org/t/spm-support-basic-auth-for-non-git-binary-dependency-hosts/37878/35)，然后使用资产 url。

*   不正确的网址:`[https://github.com/:owner/:repo/releases/download/:tag/some.xcframework.zip](https://github.com/:owner/:repo/releases/download/:tag/some.xcframework.zip)`
*   正确的网址:`[https://api.github.com/repos/:owner/:repo/releases/assets/\(assetId).zip](https://api.github.com/repos/:owner/:repo/releases/assets/\(assetId).zip)`

但更重要的是，还有一个 SPM 没有意识到的问题。

```
// netrc support is NOT enough to make this work for a private repo asset
.binaryTarget(
    name: privateName,
    url: "[https://api.github.com/repos/:owner/:repo/releases/assets/\(assetId).zip](https://api.github.com/repos/:owner/:repo/releases/assets/\(assetId).zip)",
    checksum: checksum
)
```

[Github API](https://docs.github.com/en/rest/reference/releases#get-a-release-asset) 声明下载资产的二进制内容需要对`application/octet-stream`的请求的`Accept`头。否则，将返回 API JSON 响应。

因此， [Jimmy Arts](/@jimmyarts) 在 SPM 中引入了以下更改，以自动设置割台:

[](https://github.com/apple/swift-package-manager/pull/3795) [## 将接受标题添加到 jimmya Pull Request # 3795 apple/swift-package-manager 的下载请求中

### 向二进制下载请求添加 Accept:application/octet-stream 头，以允许使用 Github API。动机:我想要…

github.com](https://github.com/apple/swift-package-manager/pull/3795) 

Xcode 13.3 包含了这一变化。因此，使用 Xcode 13.3 和一个带有 GitHub 个人访问令牌的`.netrc`文件将最终允许您从私有 GitHub 存储库中下载二进制资产。😊🎉

例如:在我的 GitHub 用户`MarcoEidinger`下有一个存储库`MyPrivateRepo`，我用资产`MyBinaryModuleName.xcframework.zip`创建了发布版`1.0.0`。因此`Package.swift`应该是这样的

```
.binaryTarget(
    name: "MyBinaryModuleName",
    url: "[https://api.github.com/repos/MarcoEidinger/MyPrivateRepo/releases/assets/58858051.zip](https://api.github.com/repos/MarcoEidinger/MyPrivateRepo/releases/assets/58858051.zip)",
    checksum: "c533f08210ac21def782d48c2ff1e1a538b05b051e128aa88ffcd44051ddc2b3"
),
```

我的`~/.netrc`文件应该是这样的

```
machine api.github.com login MarcoEidinger password ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Netrc 是一个很棒的解决方案，可以在 Mac 上运行，也可以在 Linux 上运行。如果你只在 Mac 上工作，那么你可以利用苹果的钥匙串作为替代。您可以创建一个包含以下内容的互联网密码，而不是在`~/.netrc`文件中创建一个条目

*   `Keychain Item Name`:api.github.com
*   `Account Name` : GitHub 用户名
*   `Password`:个人访问令牌

![](img/f46f256d810666fe3db83079e5ef15a1.png)![](img/3da123fe1bb39fd341033b9973627427.png)

一旦您授予权限，Xcode (SPM)将从钥匙串中读取。这样的对话可能在包解析期间出现。

![](img/36815254a78aa524f939359c9e72642b.png)

附注:整个历史可以在 Swift 论坛的以下线索中找到:

[](https://forums.swift.org/t/spm-support-basic-auth-for-non-git-binary-dependency-hosts/37878) [## SPM 支持非 git 二进制依赖主机的基本身份验证

### 我的公司在 Nexus 存储库中提供对 iOS SDK 二进制依赖项的有限公共访问。开发者是…

forums.swift.org](https://forums.swift.org/t/spm-support-basic-auth-for-non-git-binary-dependency-hosts/37878) 

*原载于*[*https://blog . eidinger . info*](https://blog.eidinger.info/xcode-133-supports-spm-binary-dependency-in-private-github-release)*。*