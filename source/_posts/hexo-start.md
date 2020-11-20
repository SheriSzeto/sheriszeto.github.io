---
title: hexo+github搭建博客
date: 2019-02-30 11:47:57
tags:
    - hexo
categories: Hexo
toc: true 
top_img: false 
cover: https://cdn.jsdelivr.net/gh//SheriSzeto/image@main/blog/hexo-start.jpg
---

### 1. 搭建GitHub博客
#### 1.1 创建仓库
新建一个名为`你的用户名.github.io`的仓库，比如说，如果你的github用户名是test，那么你就新建`test.github.io`的仓库，博客访问地址为http://test.github.io。

#### 1.2 绑定域名
如果没有自己的域名也可不绑定，想要个性化域名先去注册一个。在域名控制台配置你的GitHub地址；

域名配置最常见有2种方式，CNAME和A记录，CNAME填写域名，A记录填写IP，由于不带www方式只能采用A记录，所以必须先ping一下`你的用户名.github.io`的IP，然后到你的域名DNS设置页，将A记录指向你ping出来的IP，将CNAME指向`你的用户名.github.io`，这样可以保证无论是否添加www都可以访问，如下：
![](https://blog-1257457106.cos.ap-chengdu.myqcloud.com/hexo-start-1.png)

### 2. 配置SSH key
直接使用用户名和密码可以上传代码，但是不安全，下面示范用ssh的方式提交，先配置本地和服务器连接：
```bash
$ cd ~/. ssh #检查本机已存在的ssh密钥
```
如果提示：No such file or directory 说明你是第一次使用git。
```bash
ssh-keygen -t rsa -C "邮件地址"
```
然后连续3次回车，最终会生成一个文件在用户目录下,
```bash
cd ~/.ssh
ls
cat id_rsa.pub
```
复制里面的内容，打开你的github主页，进入个人设置 -> SSH and GPG keys -> New SSH key：
![](https://blog-1257457106.cos.ap-chengdu.myqcloud.com/hexo-start-2.png)

#### 2.1 测试是否成功
```bash
$ ssh -T git@github.com # 注意邮箱地址不用改
```
如果提示Are you sure you want to continue connecting (yes/no)?，输入yes，然后会看到：
```bash
Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
```
<!-- more -->
### 3. 使用hexo写博客
#### 3.1 安装
```bash
$ npm install -g hexo
```
#### 3.2 初始化
在电脑的某个地方新建一个名为hexo的文件夹（名字可以随便取），比如我的是F:\Workspaces\hexo，由于这个文件夹将来就作为你存放代码的地方，所以最好不要随便放。
```bash
$ cd /f/Workspaces/hexo/
$ hexo init
```
```bash
$ hexo g # 生成
$ hexo s # 启动服务
```
执行以上命令之后，hexo就会在public文件夹生成相关html文件，提交到GitHub。

`hexo s`是开启本地预览服务，打开浏览器访问 http://localhost:4000 即可看到内容。

#### 3.2 修改主题
比如next主题，先安装下载下来：
```bash
$ cd /f/Workspaces/hexo/
$ git clone https://github.com/litten/hexo-theme-next.git themes/next
```
修改`_config.yml`中的`theme: landscape`改为`theme: next`，然后重新执行`hexo g`来重新生成。

#### 3.3 上传之前
在上传代码到github之前，一定要记得先把你以前所有代码下载下来（虽然github有版本管理，但备份一下总是好的），因为从hexo提交代码时会把你以前的所有代码都删掉，最好新建一个分支存储代码，发布到生产的是master分支。

#### 3.4 上传到github
配置好要上传的地址，在`_config.yml`中添加有关deploy的部分：
```vue
deploy:
  type: git
  repository: git@github.com:SheriSzeto/sheriszeto.github.io.git
  branch: master
```
因为GitHub的网络速度限制原因，毕竟是国外的网址，可以考虑也上传到coding，配置的部分修改为：
```vue
deploy:
  type: git
  repo:
    github: https://github.com/SheriSzeto/sheriszeto.github.io,master
    coding: git@e.coding.net:sheri/sheri/sheri.git,master
```
配置好之后，安装插件
```bash
npm install hexo-deployer-git --save
```
然后输入命令部署并提交：
```bash
hexo clean
hexo g
hexo d
```

#### 3.5 常用hexo命令
```bash
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #部署到GitHub
hexo help  # 查看帮助
hexo version  #查看Hexo的版本
```

参考：https://www.cnblogs.com/liuxianan/p/build-blog-website-by-hexo-github.html