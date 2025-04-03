# Git学习笔记
## 创建并初始化仓库
```bash
# 创建一个新的目录
mkdir test

#进入目录
cd test

# 初始化一个新的Git仓库
git init
```

> 上述命令会在当前目录下创建一个`.git`目录，用于存储Git的版本控制信息。

## 新建文件添加到本地仓bash
```bash
#新建一个文件(win下直接新建)
touch test.c

#使用git add命令将文件添加到本地仓库的提交缓存
git add test.c

#使用git commit -m 命令将暂存区的内容提交到本地仓库,同时添加一条简短的提交说明
git commit -m "add new file test.c"
```
> git add<文件名>,将文件添加到暂存区
git add .会将当前目录下所有修改和新增文件都添加到暂存区
git add --all添加所有文件的同时,还包含一个删除操作

## 改写提交
### 改写提交并编辑
```bash
# 使用 --amend 来修改已commit到仓库的注释
git commit --amend
```

> 该命令会打开默认的文本编辑器
ctrl+o保存
ctrl+x退出

## 查看历史提交日志
```bash
#查看日志
git log
```
>commit c402164694ac48daf87b8198b06e9a47c2f3a4c1 (HEAD -> master)
Author: GG-GIRL <2903964879@qq.com>
Date:   Wed Apr 2 12:58:55 2025 +0800
     add file test.c

>第一行的commit 是哈希算法发算出的**id**
>因为分布式是没有主版本号的,都是使用id来做标志的
>同时使用master作为主仓库,其他分支如何迭代都不会影响到master
后面的head是指向的意思,表示提交到哪里:
head->***master*** 代表这次提交到master主仓库
head->***分支仓库*** 代表这次提交到分支仓库

>提交者,提交日期,注释

### 查看提交历史: git reflog
```bash
#git reflog可以查看当前版本库的提交历史，凡是对仓库版本进行迭代的都会出现在这个里面，包括你回滚版本都会出现在这个历史中
git reflog
```

## 回滚代码仓库:git reset --hard
### 回滚到指定历史版本
`1.先使用git log查看历史版本`
```bash
git log
```

`1.1 添加--pretty=oneline 简洁输出`

```bash
$ git log --pretty=oneline
755e1aa6cf131dbddc4130be12aea37df26ccc79 (HEAD -> master, origin/master, main)
又加了很多内容
1e01201d409fdbaa9b28134e1036f7ddcc53cbc3 修改文件
a8ff64c7eaf1d104d1609525b43e23e6b5489bf3 add new file Python习题.md
c402164694ac48daf87b8198b06e9a47c2f3a4c1 add file test.c
```

`2.再使用git reset --hard命令回滚`
```bash
git reset --hard 回滚id
#HEAD指向当前仓库,历史版本中可能有别的分支,我们只想迭代当前仓库的上一个版本,直接用HEAD来指向
git reset --hard HEAD^
git reset --hard HEAD~3
```

## 查看提交后文件是否改动:git status
```bash
git status
```
>查看当前仓库状态

>1.untracked files:当你新创建了文件，但还没有使用git add将其添加到暂存区时

>2.modified(red):文件已经被修改，但修改内容还未添加到暂存区

>3.modified(green):使用git add命令将文件的修改添加到暂存区后，这些文件会处于已暂存待提交状态
4.nothing to commit:当工作区和暂存区与最新提交状态一致，没有任何未跟踪、未暂存或待提交的更改时
5.当本地分支和远程分支有不同的提交历史时，会提示分支领先或落后
     5.1Your branch is ahead of:这表明本地 master 分支比远程 master 分支多一个提交，可以使用 git push 将本地提交推送到远程仓库。
     5.2Your branch is behind:此提示说明本地 master 分支比远程 master 分支少一个提交，可以使用 git pull 来更新本地分支。


## 删除文件: git rm
```bash
#rm删除文件,还需add和commit
rm 文件名
git add .
git commit -m "rm xxx.c"
#git rm删除文件,还需commit
git rm 文件名
git commit -m "rm xxx.c"
```
### git rm后恢复文件:git rm, git reset, git checkout
>此方法仅限git rm，因为git rm会先将文件放入缓存区,且没有使用commit提交的情况下

```bash
#首先使用git rm删除一个文件
git rm xxx.c
#在使用git reset重置所有缓存区操作
git reset
#重置完成之后在使用git checkout命令将文件取消操作
git checkout xxx.c
```

>已经提交的文件如何恢复
`这里给一个方法，就是把当前目录全部提交一次，这样做是为了防止我们等下回滚的时候导致一些修改的文件被替换掉了，然后我们回滚到有那个文件的版本，将那个文件copy到别的文件目录，这个文件目录要是你记得的，然后在回滚到最新版本代码，在将那个文件copy回来，在提交进去。`

## git创建分支: git branch, git checkout
