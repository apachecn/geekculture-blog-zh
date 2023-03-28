# 在 Google 联合实验室上使用 git 和 GitHub

> 原文：<https://medium.com/geekculture/using-git-github-on-google-colaboratory-7ef3b76fe61b?source=collection_archive---------29----------------------->

尝试过在 Google Colaboratory 上使用 git/GitHub 吗？想把一些大得吓人的目录从 Google Drive 转移到你的 GitHub 仓库吗？想把在 Colab GPUs 上训练的机器学习模型直接推送到 GitHub？

![](img/ebbe713f7f99c54aaee6a464f2ed89d5.png)

不要担心，我们今天将使用 Google 联合实验室上的 git & GitHub 来揭开这个话题的神秘面纱！您可以在自己的硬盘上创建这个 [Google 协作笔记本](https://colab.research.google.com/github/khanfarhan10/GitColab/blob/main/GitColab.ipynb#scrollTo=JyGIeO4rilcl)的副本。

本笔记本中描述的流程/方法适用于公共和私有存储库的 colab。除非你是 git/GitHub 专家，否则尽量不要修改/跳过任何步骤！用它们包含的实际名称替换所有的`{variables}`。

# 入门先决条件—需要的用户信息:

在这个过程中，在实际进行更改/修改并替换为您自己的值之前，您将需要一些`{variables}`。

*   `{your_username}` -你的 GitHub 用户名。
*   `{your_email_id}` -与您的 GitHub 账户关联的电子邮件地址。
*   `{your_password}` -您用来登录 GitHub 账户的密码。2021 年 9 月更新—出于安全原因，github 现在不推荐使用密码。请使用“个人访问令牌”,转到 github.com->设置- >开发者设置- >个人访问令牌，并为所需目的生成令牌。对于本教程中提到的所有任务，请使用此密码代替您的密码！
*   `{destination_repo_projectname}` -您希望使用的远程 GitHub 存储库(这也可以是其他远程托管存储库，如 GitLab/BitBucket)。
*   `{destination_repo_username}` -您希望处理的远程存储库的所有者(如果您希望处理自己的存储库，可以是您自己)。

# 克隆存储库:

克隆是下载远程存储库的本地副本的过程，其中预先启动了版本控制特权！您可以使用`git clone`命令克隆远程 git 存储库:

```
!git clone [https://{your_username}:{your_password}@github.com/{destination_repo_username}/{destination_repo_projectname}.git](https://{your_username}:{your_password}@github.com/{destination_repo_username}/{destination_repo_projectname}.git)
```

示例:

```
!git clone [https://khanfarhan10:thatsmypassword@github.com/khanfarhan10/LungSegmentationCustomUNET.git](https://khanfarhan10:thatsmypassword@github.com/khanfarhan10/LungSegmentationCustomUNET.git)
```

**注意**:请注意，我们使用 git 密码进行克隆。虽然对于公共存储库来说不是必需的，但是在远程推送存储库时会导致问题，所以最好使用您的密码和全部凭证进行克隆。

# 更改当前工作目录

对于 jupyter 笔记本，使用 line magic 命令`%cd`将目录更改为`{destination_repo_username}`。谷歌合作实验室的默认工作目录是`/content/`。因为我们克隆了存储库而没有改变原始的工作目录，所以它应该存在于`/content/`目录中。执行 thsi 命令，将目录更改到远程存储库的本地副本中。

```
%cd /content/{destination_repo_username}
```

示例:

```
%cd [/content/LungSegmentationCustomUNET](https://colab.research.google.com/drive/1EEoRltwnEgB-yVFEGP0-OWXujxNqY95u#)
```

# 验证您的步骤:

如果你确认你走的是对的，这可能是好的。我们可能不想搞乱我们的 GitHub 库。然而，通过练习，你可以跳过这些步骤。

## 拉

执行健全性检查，看看是否一切工作正常！

```
!git pull
```

如果在克隆后没有对远程 git 存储库进行任何更改，则应该显示以下输出:

```
Already up to date.
```

## 状态

同样，检查暂存/未暂存变更的状态。

```
!git status
```

它应该显示以下内容，并选择默认分支:

```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

# 检查旧日志

检查您之前对回购所做的承诺:

```
!git log -n 4
```

用日志输出 Git 提交 id:

```
commit 18ccf27c8b2d92b560e6eeab2629ba0c6ea422a5 (HEAD -> main, origin/main, origin/HEAD)
Author: Farhan Hai Khan <njrfarhandasilva10@gmail.com>
Date:   Mon May 31 00:12:14 2021 +0530

    Create README.md

commit bd6ee6d4347eca0e3676e88824c8e1118cfbff6b
Author: khanfarhan10 <njrfarhandasilva10@gmail.com>
Date:   Sun May 30 18:40:16 2021 +0000

    Add Zip COVID

commit 8a3a12863a866c9d388cbc041a26d49aedfa4245
Author: khanfarhan10 <njrfarhandasilva10@gmail.com>
Date:   Sun May 30 18:03:46 2021 +0000

    Add COVID Data

commit 6a16dc7584ba0d800eede70a217d534a24614cad
Author: khanfarhan10 <njrfarhandasilva10@gmail.com>
Date:   Sun May 30 16:04:20 2021 +0000

    Removed sample_data using colab (testing)
```

# 在本地回购中进行更改

从本地回购目录进行更改。

这些可能包括各种添加、删除或编辑。

专业提示:如果你愿意的话，你可以通过以下方式从驱动器复制粘贴内容到 git repo:

## 安装 Google Drive:

```
from google.colab import drive
drive.mount('[/content/gdrive](https://colab.research.google.com/drive/1EEoRltwnEgB-yVFEGP0-OWXujxNqY95u#)')
```

## 使用 Shell 实用程序/ Google Drive 复制内容:

```
import shutil,os

# For a folder:
shutil.copytree(src_folder,des_folder)

# For a file:
shutil.copy(src_file,des_file)

# Create a ZipFile
shutil.make_archive(archive_name, 'zip', directory_to_zip)

# Remove Folder
shutil.rmtree(folder_name)

# Remove File
os.remove(file_name)
```

# 设置 Git 凭据

告诉 git 你到底是谁？

```
!git config --global user.name "{your_username}"
!git config --global user.email "{your_email_id}"
!git config --global user.password "{your_password}"
```

示例:

```
!git config --global user.name "khanfarhan10"
!git config --global user.email "njrfarhandasilva10@gmail.com"
!git config --global user.password "thatsmypassword"
```

# 检查远程存储库 URL

这是确保没有错误出现的最后一步。要检查远程 url 的设置和配置是否正确，请运行:

```
!git remote -v
```

如果配置正确，它应该输出以下内容:

```
origin    https://{your_username}:{your_password}@github.com/{destination_repo_username}/{destination_repo_projectname}.git (fetch)
origin    https://{your_username}:{your_password}@github.com/{destination_repo_username}/{destination_repo_projectname}.git (push)
```

示例:

```
origin    https://khanfarhan10:thatsmypassword@github.com/khanfarhan10/LungSegmentationCustomUNET.git (fetch)
origin    https://khanfarhan10:thatsmypassword@github.com/khanfarhan10/LungSegmentationCustomUNET.git (push)
```

# 添加、提交、推送

只是常规的东西:`add`(所有)你希望更改到暂存区的文件。`commit`或者创建您想要更新的存储库版本。或者将您最近的更改上传到您的远程 git 存储库。你知道该怎么做。

```
!git add .
!git commit -m "{Message}"
!git push
```

使用 Google Colaboratory 享受 git 和 GitHub 的强大功能！如果你想问问题，请随时通过 LinkedIn[联系我](https://www.linkedin.com/in/fkpro/)。

# TL；灾难恢复完整流程简而言之:

```
!git clone https://{your_username}:{your_password}@github.com/{destination_repo_username}/{destination_repo_projectname}.git
%cd /content/{destination_repo_projectname}

!git config --global user.name "{your_username}"
!git config --global user.email "{your_email_id}"
!git config --global user.password "{your_password}"
```

进行更改，然后运行:

```
!git add .
!git commit -m "{Message}"
!git push
```