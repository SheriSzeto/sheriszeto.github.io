---
title: git常用命令
urlname: oqa0ai
date: '2021-04-14 16:42:35 +0800'
tags: []
categories: []
---

# **1、克隆库**

```shell
git clone https://github.com/SheriSzeto/learnt.git
```

# **2、创建并切换分支**

```shell
git checkout -b dev
```

# **3、创建分支**

```shell
git branch dev
```

# **4、切换分支**

```shell
git checkout dev
```

# **5、合并分支（合并远程库 master 分支）**

```shell
git merge origin/master
```

# **6、推送分支（推送远程库 master 分支）**

```shell
git push origin master
```

# **7、添加所有文件到暂存区**

```shell
git add .
```

# **8、添加和提交文件到本地**

```shell
git add test.txt
git commit -m 'add some'
```

# **9、删除分支**

```shell
git branch -d feature
```

# **10、创建远程分支到本地分支（可追踪）**

```shell
git checkout -b dev origin/dev
```

# **11、取消暂存文件（已经执行 git add）**

```shell
git reset HEAD README.md
```

# **12、从远程库中拉取数据（只拉不合并到本地分支，需手动 merge）**

```shell
git fetch origin
```

# **13、查看每个文件中修改的行**

```shell
git diff --stat
```

# **14、将暂存区文件临时存起来**

```shell
git stash
```

# **15、将临时存起来的文件恢复**

```shell
git stash pop
```

# 16、将 A 项目的某个分支复制到 B 项目

```shell
// 进入A项目
git remote
git remote add origin2 master (origin2随便起)
git remote set-url origin2 git@github.com:B.git (B仓库地址关键)
// 进入A项目的branch1
git pull
git checkout -b branch2 (基于branch1建一个新分支，主要是怕B项目也有一个branch1分支，造成冲突)
git push origin2
```
