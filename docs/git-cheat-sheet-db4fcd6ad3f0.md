# Git 备忘单

> 原文：<https://medium.com/geekculture/git-cheat-sheet-db4fcd6ad3f0?source=collection_archive---------27----------------------->

![](img/348ebcc3ff0f35870128b8c62dc43e41.png)

Photo by [Roman Synkevych](https://unsplash.com/@synkevych?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这将是你日常使用的 Git 最常用命令的快速指南。

> **添加 Git，推送一个项目到 Github**

```
**cd project/****git init    **                
# initialises a repository**git add .**                   
# add all the files from working directory to the staging area**git commit -m "COMMIT_MESSAGE"  **             
# commit all changes**git remote add origin** [**https://github.com/GIT_USER_NAME/REPO_NAME.git**](https://github.com/aroranubhav/Dummy.git)#To add a new remote **git push -u origin <new-branch>** 
# Sync local branch with remote
```

> **克隆仓库**

```
git clone <url>
# Clone a repository locally
```

> **分支和合并**

```
**git branch  **                        
# show list of all branches (* active) (for now we are assuming the current branch is master)**git checkout -b git_demo**        
# create a new branch with the name git_demo<make changes>**git commit -a****git checkout master **                
# go back to master branch**git merge git_demo  **              
# merge changesets from git_demo (Git >= 1.5)**git pull . git_demo **              
# merge changesets from git_demo (all Git versions)**git branch -m <oldname> <newname>**   
# rename branch with oldname to new name**git branch -m <newname> **            
# rename current branch
```

> **撤消未发布的合并**

```
**git reset --hard commit_sha** # with **git reflog**check which commit is one prior the merge (git reflog will be a better option than git log)another way, **git reset --hard HEAD~1** # it will get you back 1 commit.
```

> **删除存储库**

```
**git branch -d <branch_name>  **    
# deletes local branch**git push origin :<branchname>** 
# deletes remote branch**git remote prune <branchname>  ** 
# update local/remote sync
```

> **日志**

```
**git log** # view information about previous commits, commit hash, author, message, and date
```