# Git Rebase

## 原分支示意图

```
=================================================
                       feature <- HEAD
                         |
                        (C3)
                         /
                  .-----'
                 /
(C0) <- (C1) <- (C2) <- (C4) <- (C3')
                         | 
                       master
=================================================
                               feature
                                 |
(C0) <- (C1) <- (C2) <- (C4) <- (C3')
                                 | 
                               master <- HEAD
==================================================
```

## `rebase`原理

- 1、回到两个分支最近的共同祖先（如示意图中`C3`和`C4`最近的共同祖先为`C2`）

- 2、从共同祖先（`C2`）开始，根据后续历次提交生成一系列文件补丁（如示意图中从共同祖先`C2`算起，`feature`分支历次提交只有`C3`一项）

- 3、然后以基底分支（`master`分支）最后一个提交对象为新的出发点，逐个应用之前准备好的补丁文件

- 4、最后生一个新的合并提交对象（`C3'`），从而改写`feature`分支的提交历史，使它成为`master`分支的直接下游

## 操作示例

- 切换到`feature`分支

```
git checkout feature
```

- `feature`分支开发产生`C3`提交对象

- 基于`master`分支，在`feature`分支上进行`rebase`操作

```
git rebase master
```