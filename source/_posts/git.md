---
title: Git
date: 2023-03-01 10:00:00
description: Git常用操作
---
# Git
Git是一个分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。主要的目的是为了更好地管理项目，记录文件的修改历史，以便在将来某个时刻可以查阅特定版本的文件内容，并且实现多人协作开发。
1. Keeps track of changes to code
2. Synchronizes code between different people
3. Test changes to code without losing the original
4. Revert back to previous versions of code

## 命令
### git clone
git clone命令用于将远程仓库克隆到本地。
1. make a copy of a repository
2. stores it on your computer
3. a "fork" creates your own copy of someone else's repository
```bash
git clone <url>
```
### git add
git add命令用于将文件添加到暂存区。
1. adds a file to "staging area"
2. tells git to include the file in the next revision to the repository
3. git add * adds all changed files
```bash
git add <file>
```
### git commit
git commit命令用于将文件提交到本地仓库。
1. saves the changes to repository as a new revision
2. records a message
3. git commit -am "message" adds and commits in same step
```bash
git commit -m "message"
```
### git status
git status命令用于查看当前文件状态。
1. show current status of repository
```bash
git status
```
### git push
git push命令用于将本地仓库推送到远程仓库。
1. sends committed changes to remote repository
2. more explicitly, could write git push origin master
```bash
git push <remote> <branch>
```
### git pull
git pull命令用于将远程仓库拉取到本地。
1. retrieves changes from remote repository
```bash
git pull <remote> <branch>
```
## Merge Conflicts
git push之前先git pull，确保自己是在最新的代码上进行改动。此时可能会出现Merge Conflicts，如果有冲突，需要手动解决冲突。解决完冲突之后，再次git add，git commit，git push。

### git log
git log命令用于查看提交历史。
1. shows a history of commits and messages
```bash
git log
```
### git reset
git reset命令用于回退版本。
1. git reset --hard <commit> reverts code back to a previous commit
2. git reset --hard origin/master reverts code back to remote repository version
```bash
git reset --hard <commit>
```
## branching
branching是git的一个重要特性，可以在不影响主分支的情况下，创建一个新的分支，进行开发。开发完成之后，再将新分支合并到主分支上。
### git branch
git branch命令用于查看分支，或者创建分支。创建的分支是基于当前分支的，拥有当前分支的所有内容。
1. shows all branches of code
2. create a branch with git branch <branch_name>
3. switch to ("checkout") a new branch with git checkout <branch_name>
```bash
git branch
```
### git checkout
git checkout命令用于切换分支。
1. git checkout <branch_name> switches to branch_name
```bash
git checkout <branch_name>
```
### git merge
git merge命令用于合并分支。
1. git merge <branch_name> merges branch_name into current branch
```bash
git merge <branch_name>
```

