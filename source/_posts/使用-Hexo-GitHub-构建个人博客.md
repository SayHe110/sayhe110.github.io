---
title: 使用 Hexo + GitHub 搭建个人博客
date: 2019-06-08 22:34:13
tags:
---
### 前言
之前一直用的是 WordPress + 阿里云 来搭建个人博客，因为自己的服务器配置处于乞丐版，当然用于搭建个人博客是完全够了，因为也没有推广自己的博客，所以流量几乎没有，大部分都是爬虫访问😅，还有个重要的原因是阿里云服务器是学生优惠买的，若到期了就没办法继续使用了，所以一直有将个人博客搭建在 Github 上的想法。

### 什么是 Hexo ？
Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

### 安装及部署
#### 个人网站域名（不是必须）
个人网站域名并不是必须的，若没有的话也可以，当然访问的域名就没有那么直白，可以这样访问：username.github.io，这样访问个人博客，若有域名的话可以更容易记一点，比如我的域名 blog.sayhe110.cn ，这样让别人更加容易记住。<br>

申请域名的地方有很多，个人比较推荐 [阿里云](https://www.aliyun.com/) 与 [腾讯云](https://cloud.tencent.com/)。

#### Github
因为是基于 Hexo + Github 搭建个人博客，所以需要 Github 账号，以及创建 Github 仓库，在这就不介绍如何使用 Github 了，可以看这篇 [文章](https://www.runoob.com/w3cnote/android-tutorial-git-repo-create.html) 学习如何使用及创建仓库。例如我的仓库：

<img src="http://ww2.sinaimg.cn/large/006tNc79gy1g3uu87oeikj30js04c74g.jpg" width="300" height="80"/>

#### 安装 Git
什么是 Git ？Git 为分布式版本控制系统，我们将文章在本地写好后，需要同步至 GitHub，所以需要 Git 。Git 的下载与使用直接看文档即可，[文档地址](https://git-scm.com/book/zh/v2)。友情提示：不要忘记设置 Git 本地的全局配置与 Key，若不明白可以看下 [廖雪峰教程](https://www.liaoxuefeng.com/wiki/896043488029600)。

#### 安装 Node.js
因为 Hexo 是基于 Node.js ，所以还需要下载 Node.js。也是看 [文档](http://nodejs.cn/) 安装就好。
安装完成后使用 `node -v` 和 `npm -v` 命令查看是否安装成功。

<img src="http://ww4.sinaimg.cn/large/006tNc79gy1g3uv5mjto2j314m0800ui.jpg" width=70%>

#### 安装 Hexo
之前介绍过 Hexo，所以就直接通过 `npm install -g hexo-cli` 命令安装即可，若 npm 过慢，可以使用 cnpm，具体方法百度、谷歌。<br>
在合适的文件夹位置命令行中，通过 `hexo init blog` 命令来初始化博客， `blog` 为博客项目名称。<br>
可以通过以下命令来检查是否安装成功。
```shell
hexo new my_blog

hexo g

hexo s
```
然后在浏览器中输入 `localhost:4000` 即可访问博客。

#### 关联 GitHub
打开站点的 `_config.yml` 文件，修改以下配置：
```yml
deploy: 
    type: git
    repo: 这里填入你之前在GitHub上创建仓库的完整路径，记得加上 .git
    branch: master
```
我的配置供你参考：
```yml
deploy: 
    type: git
    repo: https://github.com/SayHe110/sayhe110.github.io.git
    branch: master
```
最后安装 Git 部署插件：
```shell
npm install hexo-deployer-git --save
```
通过以下命令将网站部署（命令在最后解释，以及介绍常用命令）：
```shell
hexo clean 

hexo g 

hexo d
```

试着访问博客，在浏览器输入 username.github.io (username 为自己的github 用户名)，例如我的地址为：sayhe110.github.io

#### 绑定域名（不是必须）
若我们有域名的话，我们可以绑定自己的域名，下次就可以通过这个域名进行访问，在域名服务厂商中解析域名，比如：
![](http://ww3.sinaimg.cn/large/006tNc79gy1g3uvqjx4apj31vc084gn4.jpg)
![](http://ww4.sinaimg.cn/large/006tNc79gy1g3uvs1rffyj31v2052gm7.jpg)

其中记录类型为 CNAME 记录值为 github 仓库域名如：sayhe110.github.io

在博客项目中 `source` 目录下创建 `CNAME` 文件，然后输入域名如：
<img src="http://ww3.sinaimg.cn/large/006tNc79gy1g3uvxk94j0j30y00aqq3t.jpg" width=70%/>

最后设置 Github 仓库个性化域名，博客项目中进入 `settings -> custom domain` ，设置自己域名，然后 `save` 即可。

然后在本地博客项目文件夹目录下，输入以下命令：
```shell
hexo clean

hexo g

hexo d
```

现在在浏览器地址栏输入个性化域名即可访问博客。

### 个性化主题
Hexo 有许多有趣好看的主题供大家选择，可以直接看[这里](https://hexo.io/themes/)，选取心仪的主题，大部分主题都有安装步骤，跟着安装步骤安装即可。

### 常用命令介绍
博客大体已经完成，最常用的就几条命令：
```shell
hexo n "我的博客" == hexo new "我的博客" #新建文章

hexo clean # 清除缓存，若是网页正常情况下可以忽略这条命令

hexo g # hexo generate 简写

hexo d # hexo deploy 简写
```
其他常用命令介绍：
```shell 
npm update hexo -g #升级 

hexo init #初始化博客

hexo server #Hexo会监视文件变动并自动更新，无须重启服务器

hexo server -s #静态模式

hexo server -p 5000 #更改端口

hexo server -i 192.168.1.1 #自定义 IP
```

### 其他
大家可以尝试更多好玩的，以及尝试更多好看的主题，😄<br>
关于图片大家可以选择图床进行上传文件

### 参考资料
- [GitHub+Hexo 搭建个人网站详细教程](https://zhuanlan.zhihu.com/p/26625249)
- [Hexo 文档](https://hexo.io/zh-cn/docs/index.html)
