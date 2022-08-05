---
title: centos7安装SVN
urlname: wacbcd
date: '2021-04-14 10:25:38 +0800'
tags: []
categories: []
---

## 1.安装相关的包

```shell
[root@localhost ~]# yum install subversion
[root@localhost ~]# yum install mod_dav_svn
[root@localhost ~]# yum install httpd httpd-devel subversion mod_dav_svn mod_auth_mysql
```

## 2.确认已安装了 svn 模块

```shell
[root@localhost ~]# cd /etc/httpd/modules
[root@localhost ~]# ls | grep svn
mod_authz_svn.so
mod_dav_svn.so
```

## 3.新建一个目录用于存储 SVN 所有文件

```shell
[root@localhost ~]# mkdir /root/project
```

## 4.新建一个版本仓库(即项目)

```shell
[root@localhost ~]# svnadmin create /root/project/crm
```

## 5.配置工程用户，并设置用户权限

进入工程的配置目录：

```shell
[root@localhost ~] cd /source/svn/project/conf
[root@localhost ~] ls
authz passwd svnserve.conf
```

```shell
[root@localhost ~] vi svnserve.conf
[general]
anon-access = none
auth-access = write
password-db = passwd
authz-db = authz
```

```shell
[root@localhost ~] vi passwd
[users]
admin = admin
sheri = sheri
```

```shell
[root@localhost ~] vi authz
[groups]
@admin = admin,sheri
```

```shell
[crm:/](crm的根目录拥有读写权限)
@admin = rw
```

## 6.启动服务器

```shell
[root@localhost ~]# svnserve -d -r /root/project
```

## 7.测试服务器

```shell
[root@localhost ~] svn co svn://192.168.32.186/crm
```

## 8.关闭防火墙

```shell
systemctl stop firewalld.service
```

## 9.默认 SVN 服务的端口是 3690，查看 3690 服务是否已执行

```shell
netstat -nap | grep 3690
```

如果服务器没有 netstat 命令，安装方法

```shell
yum install net-tools
```
