# Java 应用程序中的版本控制和文件管理

> 原文：<https://medium.com/geekculture/versioning-and-file-management-in-java-application-56eeb4520e76?source=collection_archive---------19----------------------->

![](img/9fa30337dbae495d4b2ddfc922876ef0.png)

管理一个文件的不同文件版本可能会很麻烦。它会成为存储猪，还会使文件管理变得困难。为了解决这样的问题，使用了版本控制系统。这是一种跟踪文件中的变化并从中制作版本的方法，允许我们根据需要恢复到特定的版本。git 就是这样一个版本工具，它可以用来管理文件夹中的文件版本。在本文中，您将了解如何将 git 集成到您的软件中，以便从 java 应用程序内部管理版本。

在深入研究技术细节之前，您需要了解一些关于 git 的基本知识。在 git 中，存储库是管理文件版本的文件夹。每个版本都通过提交保存在 git 中。此外，可以创建一个完全不同的版本行，我们称之为分支。可以从任何提交或任何其他分支创建分支。要了解更多关于 git 如何工作的信息，你可以点击这里查看。

在本教程中，我们将使用 git 的 Java 接口 JGit，这是一个 eclipse 支持的社区。

JGit 有两个基本级别的 API: *管道*和*瓷器*。这些术语来自 Git 本身。JGit 分为相同的区域:

*   *瓷瓶*API——常见用户级操作的前端(类似于 Git 命令行工具)
*   *管道*API—直接访问低级存储库对象

下面的代码可以用来使用 JGit 创建一个存储库

```
public Repository createNewRepository(String dirPath) {
    // prepare a new folder
  Path path = Paths.*get*("path/to/folder");
    try {
        if (!Files.*exists*(root)) {
            Files.*createDirectory*(root);
        }
        if (!Files.*exists*(path)) {
            Files.*createDirectory*(path);
        }
        // create the directory
        Repository repository = FileRepositoryBuilder.*create* (new File(path.toFile(), ".git"));
        try {
            repository.create();
        } catch (IllegalStateException repositoryExists) {
            repositoryExists.printStackTrace();
        }
        System.*out*.println(repository.getDirectory());
        return repository;

    } catch (IOException e) {
        throw new RuntimeException("Could not initialize folder");
    }

}
```

要获取存储库，可以使用以下代码:

```
public Repository getRepository(String dirPath) throws IOException {
    Path path = Paths.*get*(dirPath);
    Repository repository = Git.*open*(new File(path.toFile(),".git"))
                               .getRepository();
    return repository;
}
```

以下代码可用于在暂存中添加文件

```
public Message addOutputFiles(String fileName) {
    String dir = "path/to/directory"; 

    String branchName = "branchName";

    try {
        Repository repository = getRepository
                           (root.toString() + File.*separator* + dir);
        try (Git git = new Git(repository)) {

            // checkout of branch if needed
       if(!git.getRepository().getBranch().equalsIgnoreCase(branchName)) {
                git.checkout().setName(branchName).call();
            }
           // add file in staging
            git.add()
                    .addFilepattern(approval)
                    .call();
       } catch (Exception e) {
            e.printStackTrace();
        }

    } catch (Exception e) {
   throw new RuntimeException("Could not store the file. Error: " );
    }

}
```

***注意*** *JGit 到目前为止还不支持 regex 模式或“*”将文件添加到 staging，就像 git CLI 提供的选项一样。*

为了提交暂存文件，可以利用以下代码:

```
public Message commitModelRepository( String fileName) 
throws IOException {
        String dir = "path/to/directory";
        try {
            Repository repository = getRepository("path/to/repo");
            try (Git git = new Git(repository)) {

                // checkout branch if needed
                 if(!git.getRepository().getBranch().equalsIgnoreCase("master"))
 {
                    git.checkout().setName("master").call();
                }

                // and then commit the changes
                String commit = git.commit()
                        .setMessage("Added " + fileName)
                        .call().getName();
            } catch (Exception e) {
                e.printStackTrace();
            }

        } catch (Exception e) {
    throw new RuntimeException("Could not store the file. Error: ");
        }

    }
```

本教程只是冰山一角。在很多用例中，我们可以在任何其他类型的应用程序中利用 JGit 进行文件版本控制。它允许像 git 一样管理版本，git 允许开发人员利用 Jgit 轻松开发基于版本的特性。如果您想进一步了解，可以在下面的[链接](https://www.baeldung.com/jgit)和[链接](https://git-scm.com/book/pl/v2/Appendix-B%3A-Embedding-Git-in-your-Applications-JGit)中找到关于 JGit 的详细信息。要在 Jgit examples 上找到一个例子，你可以查看这个[链接](https://github.com/centic9/jgit-cookbook)