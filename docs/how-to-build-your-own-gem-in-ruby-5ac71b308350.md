# 如何在 Ruby 中构建自己的 Gem

> 原文：<https://medium.com/geekculture/how-to-build-your-own-gem-in-ruby-5ac71b308350?source=collection_archive---------24----------------------->

![](img/9a9f7b707fc146d4629d91ccfefb1907.png)

顾名思义，Ruby 编程语言的繁荣得益于 [gems](https://blog.engineyard.com/7-ruby-gems-to-keep-in-your-toolbox) 的出现。和之前的插件一样，gems 是用来执行特定任务的迷你代码，不会干扰或直接成为我们代码的一部分。gem 所需的代码仍然是我们项目生态系统的一部分，但是我们最终会通过使用 gem 而不是自己编写代码来消除一些开销——并避免不必要的重复。

那么为什么要创造一个宝石呢？当你有一段独立的代码，或者当开发人员意识到这段代码可能对其他人有帮助时，Gems 就是一个有用的副产品。如果我们想在其他地方使用这些代码，或者想分享它们，那么就到了打造宝石的时候了。

gem 很容易构建，而且创建它们所需的工具很可能已经安装在您的环境中。我们的第一步是安装[捆扎机](http://bundler.io/)。Rails 是 Bundler 最广为人知的应用项目，但是我们也可以使用 Bundler——不仅用于 gem 依赖管理——还用于编写我们自己的 gem。

一旦捆扎机安装完毕，我们就可以开始制作宝石了。但是在实际编写任何东西或者创建一个沙盒来使用之前，我们需要做所有编程中最困难的事情之一:命名事物。我们的 gem 应该有一个描述它做什么的名字，容易记住，希望以前没有用过。要仔细检查潜在宝石的名称，请查看 Rubygems.org 的[搜索功能。此外，这种搜索还可能将我们带到一个具有我们最初想要创建的相同功能的 gem，从而从长远来看节省了我们的额外工作。](https://rubygems.org/)

对于这篇博文，我们要写一个名为 heynow 的宝石。这个宝石的功能将面向下班后使用我们的应用程序的人，向他们指出现在是什么时间。不是很有用，但是对于我们的例子来说非常好，因为创建起来非常简单明了。我们开始吧！

```
$ bundle gem heynow
```

系统将提示我们选择一个测试框架(、或无)。请选择一个首选的测试框架，如果它没有出现在这里，请选择无。出于我们的目的，我们将选择 rpec。

接下来，我们将有机会选择我们是否希望这个宝石在麻省理工学院的许可下发布，以及是否将行为准则归于您的宝石。我们推荐这两种方式，但是当然这些决定取决于你的团队。

一旦完成，就会创建一个名为“heynow”的目录供我们使用。该目录将如下所示:

```
projects pjhagerty$ cd heynow/
heynow pjhagerty$ ls
CODE_OF_CONDUCT.md LICENSE.txt Rakefile heynow.gemspec spec
Gemfile README.md bin lib
```

我们首先需要查看的是 gemspec 文件——hey now . gem spec。该文件保存了有关宝石的所有元数据。这包括从你的作者姓名到你想使用的许可证的一切。在这种情况下，我们将添加描述并保留自动生成的标准 MIT 许可证。我们的 heynow.gemspec 文件应该如下所示:

```
require_relative ‘lib/heynow/version’Gem::Specification.new do |spec|
spec.name = “heynow”
spec.version = Heynow::VERSION
spec.authors = [“PJ Hagerty”]
spec.email = [“pjhagerty@gmail.com”]spec.summary = %q{The function of this gem will be oriented to people who are using our app after hours, pointing out to them what time it is right now.}
spec.description = %q{The function of this gem will be oriented to people who are using our app after hours, pointing out to them what time it is right now.}
spec.homepage = “https://github.com/aspleenic/heynow”
spec.license = “MIT”
spec.required_ruby_version = Gem::Requirement.new(“>= 2.3.0”)# spec.metadata[“allowed_push_host”] = “https://github.com/aspleenic/heynow"spec.metadata[“homepage_uri”] = spec.homepage
spec.metadata[“source_code_uri”] = spec.homepage
spec.metadata[“changelog_uri”] = spec.homepage# Specify which files should be added to the gem when it is released.
# The `git ls-files -z` loads the files in the RubyGem that have been added into git.
spec.files = Dir.chdir(File.expand_path(‘..’, __FILE__)) do
 `git ls-files -z`.split(“\x0”).reject { |f| f.match(%r{^(test|spec|features)/}) }
end
spec.bindir = “exe”
spec.executables = spec.files.grep(%r{^exe/}) { |f| File.basename(f) }
spec.require_paths = [“lib”]
end
```

如果这里有几行代码你不能马上理解，不要担心。gemspec 文件的大部分是自动构建的，因此我们可以专注于制作有用的 gem。我们负责作者部分、电子邮件、描述、摘要和主页(只有当我们的 gem 变得如此受欢迎以至于需要时)，然后让生成的代码做剩下的事情。

一旦我们完成了排序，并且我们将代码提交给了我们选择的回购协议，我们就可以看看如何对我们的 gem 进行版本控制了。重要的是，我们要遵循我们在 Ruby 代码的其余部分所学到的语义版本控制技术。关于语义版本化的更多信息，[请看这篇文章](http://guides.rubygems.org/patterns/#semantic-versioning)。

gem 的版本控制保存在 lib 目录下的 gem 名称文件中，该文件位于一个名为 version.rb 的脚本中(在我们的例子中:lib/heynow/version.rb)。这是一个简单的模块，我们不需要更改它，因为我们正在开发 gem 的第一个版本:

```
module Heynow VERSION = “0.1.1”end
```

(一旦我们有了工作中的 gem 的未来迭代，我们就可以担心升级版本了。目前，我们可以使用 0.0.1 版本。)

现在是时候写代码来做我们希望我们的 gem 做的事情了。实际的可执行代码将写在 lib/heynow.rb 中，最初看起来是这样的:

```
require “heynow/version”module Heynow
 class Error < StandardError; end
 # Your code goes here…
end
```

有人好心地创造了一个空间，让我们知道把你的代码放在哪里，这是假设你知道如何编码。这篇文章不会涉及代码本身的编写或测试的重要性，但我们将继续下一步，创建一个 gem，这意味着向全世界发布它。要看完整的 heynow 宝石，看一看 [heynow 回购](https://github.com/aspleenic/sleepr)。

我们已经为创业板建立了回购机制，因此它已经[公开上市](https://github.com/aspleenic/heynow)。但是我们需要把它放在别的地方，让人们知道我们正在把它献给世界:RubyGems.org。我们的第一步将是用他们方便的表格建立一个账户。一旦您有了帐户，我们就可以设置自己的凭据:

```
heynow pjhagerty$ gem build heynow.gemspec
 Successfully built RubyGem
 Name: heynow
 Version: 0.1.1
 File: heynow-0.1.1.gem
```

然后，我们可以通过发出以下命令将我们的 gem 移动到 RubyGems:

```
heynow $ gem push heynow-0.1.2.gem
Enter your RubyGems.org credentials.
Don’t have an account yet? Create one at
https://rubygems.org/sign_up
 Email: pj@gmail.com
Password:Signed in.
Pushing gem to [https://rubygems.org...](https://rubygems.org...)
Successfully registered gem: heynow (0.1.2)
```

有了这个命令，将会发生两件事:我们的 git repo 将被标记上版本号，我们的 gem 将可以通过它的 [RubyGems 页面](https://rubygems.org/gems/heynow)访问。幸运的话，由于你的勤奋工作，人们很快就会将 heynow 放入他们的 Gemfile，并为他们的最终用户捆绑更好的体验。

如果你对 Engine Yard 发布的或者过去参与的一些宝石感兴趣，看看我们的开源贡献或者深入我们的[公共 github repos](https://github.com/search?utf8=%E2%9C%93&q=Engine+Yard) 。

【https://blog.engineyard.com】最初发表于[](https://blog.engineyard.com/how-to-build-your-own-gem-in-ruby)**。**