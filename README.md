# Git Manual

## `git`基本概念

### 文件状态

`git`有三种状态：已修改（modified）、已暂存（staged）、已提交（committed）

- 已修改表示修改了文件，但还没保存到数据库中

- 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中

- 已提交表示数据已经安全的保存在本地数据库中

## 特定字符

- `HEAD`：总是指向当前的提交，无论是分支还是特定提交也好

- `~`：用于表示提交的父节点的相对引用。比如，`3157e~1`指向`3157e`前一个提交，`HEAD~3`是当前提交的回溯3个节点的提交

- `origin`：默认的远程仓库的名称。一种是`clone`远程仓库时系统指定的默认名称，一种是执行`git remote add [name] [url]`时指定的名称`[name]`


## 用户选项配置

```
// 配置提交时的用户名
git config --global user.name "XX"

// 配置提交时的用户邮箱
git config --global user.email "XX"

// 配置命令显示配色
git config --global color.ui auto

// 保存 push 操作时的用户名和密码
git config --global credential.helper store

// 修改 git 编辑时的默认编辑器
git config --system core.editor <editor>

// 配置提交时的 ssh 密钥
ssh-keygen -t rsa -C "email@example.com" // 复制生成的公开密钥 ~/.ssh/id_rsa.pub 到 git-setting 中

// 忽略文件权限变动
git config core.filemode false

// 忽略linux和windows换行符的差异
git config --global core.whitespace cr-at-eol

// 显示所有用户配置
git config --list
```

## 仓库基本操作

```
// 克隆仓库到本地
git clone --recursive -b [branch] [url]
// 通过 ssh 下载远程仓库
git clone --recursive -b [branch] username@host:[repo path]
// 通过 https 下载远程仓库
git clone --recursive -b [branch] https:[repo path]

// 更新 submodule
git submodule update --init --recursive

// 添加修改文件到暂存区
git add [new file]

// 添加暂存文件到提交区
git commit -m [commit]

// 推送提交文件到远程仓库
git push
```

## 基本指令介绍

### 1、`init`创建本地仓库

***`init`指令用来创建新的`git`仓库，用来将`已存在`但还没有版本控制的项目转换成一个`git`仓库，或者创建一个`空的新仓库`***

***该指令会在当前目录下生成新的`.git`文件夹，用来存储`git`过程生成的各类数据***

```
git init
```

### 2、 `clone`克隆远程仓库

***`clone`自动创建了一个名为`origin`的远程连接，指向原有仓库***

```
git clone --recursive -b [branch] [url]
```

### 3、`commit`基本操作

***主要是将暂存区里的改动给提交到本地的版本库。每次使用`git commit`命令我们都会在本地版本库生成一个40位的哈希值，这个哈希值也叫`commit-id`***

```
// -m 参数后的 commit 称作提交信息，是对这个提交的简要概述
git commit -m "commit"
```

- 调用编辑器来详细书写`commit`提交信息

```
git commit
```

- 修改上一次的提交信息

```
git commit --amend
```

- 撤回上一次`commit`的部分文件，将修改的文件从`committed`转移到`modified`区域

```
git reset HEAD [file]...
```

- 撤回上一次`commit`的全部文件，将修改的文件从`committed`转移到`modified`区域

```
git reset --soft HEAD^
```

- 跳过`staged`区域，将改动从`modified`转移到`committed`区域

```
git commit -a -m "commit"
```

### 4、`log`基本操作

***查看以往仓库中提交的日志，包括可以查看什么人在什么时候进行了提交或合并，以及操作前后有怎样的差别***

- 只显示`log`的第一行信息

```
git log --pretty=short
```

- 只显示指定文件或者目录的变动

```
git log [file]
```

- 显示文件改动前后差异

```
git log -p [file]
```

### 5、`diff`基本操作

***查看工作区、暂存区、最新提交之间的差别***

- 当对于工作区作出修改，但是没有执行`add`添加到暂存区时，`git diff`显示工作区和暂存区之间的差异

- 当对于工作区做出修改，执行过`add`添加到暂存区时，`git diff`显示无修改，此时需要使用`git diff --staged`比较暂存区与最新提交之间的差异

### 6、`checkout`基本操作

`checkout`命令有三种不同的作用：检出文件、检出提交、检出分支

- 将文件从`modified`区域回复到原始状态，***会丢失`modified`的全部修改，不建议使用***

```
git checkout -- [file]...
```

### 7、`branch`基本操作

- 在本地创建并切换到新的分支

```
git checkout -b [new branch]
```

等同于以下两条指令

```
git branch [new branch]
git checkout [new branch]
// 将本地创建的新分支，推送到远程新分支（远程不存在会自动创建）
git push origin [local new branch]:[remote new branch]
```

- 将新分支改动`merge`到主分支

```
// 保存新分支内容，切换到主分支
git checkout master
git merge --no-ff [new branch]
// push 前仔细查看 log 信息，可采用图形方式查看更清晰
git log --graph
```

- 本地和远程`branch`的删除操作

```
git push origin --delete [remote branch]
git branch -d [local branch]
```

### 8、`remote`操作

- 显示当前克隆的源地址

```
git remote -v
```

- 添加新的远程仓库地址

```
// 其中[shortname]为远程仓库的简称
// [url]为远程仓库的地址
git remote add [shortname] [url]
```

- 查看某个远程仓库的详细信息

```
git remote show [remote-name]

example: 
// 查看默认的远程仓库信息
git remote show origin
```

- 修改某个远程仓库在本地的简称

```
git remote rename [src-name] [dest-name]
```

- 删除对应的远程仓库

```
git remote rm [remote-name]
```

### 9、`reset`回溯操作

```
// log 查看需要回溯的版本哈希值
git log
// 回溯到指定的版本
git reset --hard [hash value]
// 回溯到指定版本后，可通过 reflog 查看所有版本的哈希值，方便撤销回溯
git reflog
```

### 10、`rebase`基本操作

***在合并特性分支之前，如果发现已提交的内容中有些许拼写错误等，可以提交一个修改，然后将这个修改包含到前一个提交之中，压缩成一个历史记录***

- 将之前`[n]`步`commit`但是还未`push`的记录合并成一条`commit`

```
// 合并的 commit 记录可以在日志前选择 pick
// 需要保留的 commit 可以在日志前选择 squash
// 需要舍弃的 commit 可以在日志前选择 fixup
git rebase -i HEAD~[n]
// 保存退出后会出现新的、合并后的 commit 编辑界面
```

- 合并指定区间的`commit`记录

```
// 合并 (start, end] 区间内的 commit 记录
git rebase -i [start] [end]
```

- 出现文件冲突情况下，按照提示合并冲突并继续`rebase`操作

```
git rebase --continue
```

- 合并`commit`记录错误需要修改信息

```
git rebase --edit-todo
```

- 放弃`rebase`操作

```
git rebase --abort
```


