---
title: Hexo部署时提示Fatal： Could not read from remote repository的处理
date: 2019-03-24 14:49:02
tag:
    - hexo
    - question
categories: Hexo    
---
第一次在Mac中配置好了hexo,执行hexo d发布的时候一直提示：
```bash
Error: Permission denied (publickey). fatal: Could not read from remote repository.
```
这个大部分是因为公钥配置的问题引起的。

在GitHub配置了公钥信息，但是执行sudo hexo deploy就报上面的问题，可以猜测到基本和sudo权限有关。

在执行hexo命令时，如果不加sudo，会报以下错误：
```bash
Unhandled rejection Error: EACCES: permission denied, open '/Users/sheri/Developer/blog/db.json' at Error (native)
```

### 生成公钥
前面生成公钥的命令是：
```bash
ssh-keygen -t rsa -C “xxx@github.com”
```
没有加sudo，生成的公钥是当前用户的，路径是/Users/sheri/.ssh。但是sudo hexo d命令执行的时候会去读取root用户的公钥，这时候root用户下的公钥肯定还没生成。

### root用户生成公钥
```bash
sudo ssh-keygen -t rsa -C “xxx@github.com”
```
对应路径为/var/root/.ssh

重新把公钥信息配置到GitHub中，
```bash
sudo cat /var/root/.ssh/id_rsa.pub
```
执行
```bash
sudo hexo d
```
推送成功！


