---
title: docker安装Jenkins
urlname: qyrif7
date: '2022-06-14 15:47:18 +0800'
tags: []
categories: []
---

·一、安装 docker 1.下载 docker RPM 包
使用 wget 下载 docker-ce18.03.1 的安装包

```shell
wget https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-18.03.1.ce-1.el7.centos.x86_64.rpm
```

![image.png](https://cdn.nlark.com/yuque/0/2022/png/21382958/1655192957278-ddecf2d7-d375-4668-9399-0be4856c0610.png#clientId=u89d0f152-a003-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=194&id=u6834562f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=387&originWidth=2441&originalType=binary∶=1&rotation=0&showTitle=false&size=100578&status=done&style=none&taskId=u5fcb2411-f062-4bad-8cb7-bc06f587cfa&title=&width=1220.5)
2.yum 安装 rpm 包

```shell
yum install -y docker-ce-18.03.1.ce-1.el7.centos.x86_64.rpm
```

![image.png](https://cdn.nlark.com/yuque/0/2022/png/21382958/1655193010778-fdc707f3-273a-4b80-9962-94df1040a36f.png#clientId=u89d0f152-a003-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=281&id=ubf4e2d37&margin=%5Bobject%20Object%5D&name=image.png&originHeight=562&originWidth=1280&originalType=binary∶=1&rotation=0&showTitle=false&size=46270&status=done&style=none&taskId=ue86e2a1d-ff26-48be-8423-c7e7f51a132&title=&width=640) 3.启动 docker 并且开机启动

```shell
systemctl start docker.service
systemctl enable docker.service
```

4.查看 docker 版本

```shell
docker version
```

二、安装 Jenkins 1.下载 Jenkins

```shell
docker pull jenkins/jenkins
```

2.创建 Jenkins 目录并授予权限
在启动 Jenkins 时，需要先创建一个 Jenkins 的配置目录，并且挂载到 docker 里的 Jenkins 目录下

```shell
//创建目录
mkdir -p /var/jenkins_home
//授权权限
chmod 777 /var/jenkins_home
```

3.启动 Jenkins 容器
-d 后台运行镜像

-p 10240:8080 将镜像的 8080 端口映射到服务器的 10240 端口。

-p 10241:50000 将镜像的 50000 端口映射到服务器的 10241 端口

-v /var/jenkins_mount:/var/jenkins_home /var/jenkins_home 目录为容器 jenkins 工作目录，我们将硬盘上的一个目录挂载到这个位置，方便后续更新镜像后继续使用原来的工作目录。这里我们设置的就是上面我们创建的 /var/jenkins_home 目录

-v /etc/localtime:/etc/localtime 让容器使用和服务器同样的时间设置。

–name myjenkins 给容器起一个别名

```shell
docker run -d -p 10240:8080 -p 10241:50000 -v /var/jenkins_home:/var/jenkins_home -v /etc/localtime:/etc/localtime --name myjenkins jenkins/jenkins
```

![image.png](https://cdn.nlark.com/yuque/0/2022/png/21382958/1655193793322-b13cc4b6-5b32-4c77-bc44-05f8d91fbb77.png#clientId=u89d0f152-a003-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=46&id=u20e1ff51&margin=%5Bobject%20Object%5D&name=image.png&originHeight=92&originWidth=2192&originalType=binary∶=1&rotation=0&showTitle=false&size=72840&status=done&style=none&taskId=u46b3e050-5554-400f-92bd-a00244e53c4&title=&width=1096) 4.验证 Jenkins 容器是否启动

```shell
docker ps
```

![image.png](https://cdn.nlark.com/yuque/0/2022/png/21382958/1655193839749-0f1d1b63-45c3-435c-bb26-ad43feb1c0fc.png#clientId=u89d0f152-a003-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=72&id=u3a5079a5&margin=%5Bobject%20Object%5D&name=image.png&originHeight=144&originWidth=2202&originalType=binary∶=1&rotation=0&showTitle=false&size=101356&status=done&style=none&taskId=u02e0c2ac-99ff-4ddc-8f40-d54da2633f5&title=&width=1101)
到这一步，安装算是完成了，那么就可以开始使用浏览器远程访问了

三、浏览器访问 Jenkins 页面 1.输入 http://IP:10240
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21382958/1655263012802-3d96fcb2-aa7a-4fa8-b32e-1b47ecd7e668.png#clientId=u2ddb58ae-d5bf-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=722&id=ue9411152&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1444&originWidth=1986&originalType=binary∶=1&rotation=0&showTitle=false&size=224543&status=done&style=none&taskId=udf00892a-504b-4e8c-b104-0dfe4387543&title=&width=993) 2.获取管理员密码

```shell
vim /var/jenkins_home/secrets/initialAdminPassword
```

根据选择安装插件
