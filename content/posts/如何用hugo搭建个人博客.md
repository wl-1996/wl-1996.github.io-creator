---
title: "如何用hugo搭建个人博客"
date: 2019-08-24T17:20:18+08:00
draft: false
---

## 今天学习了以下几个内容：

1. 用 hugo 搭建个人博客并上传到 github；
2. 域名购买，视频里讲了两种购买方式，一种是 namesilo，另一种是阿里云。原本想在阿里云买，结果阿里云太贵，没办法，等有钱了再用阿里云的域名吧；
3. 域名的配置，配置出 namesilo 的四个 A 就可以了，然后用命令行：nslookup wangkuo.monster 检测是否配置成功；
4. hugo 里插入图片；

## 今天的学习不太顺利，其中四个内容都在操作过程中遇到了问题：

### 一.搭建个人博客时遇到了如下问题

- 运行 git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke 时老是不成功，试了多次最后终于成功了，而且非常慢，估计是 git 命令行没有翻墙的原因，回头我需要配置一下命令行的翻墙；

### 二.域名购买时遇到了如下问题

- 购买域名需要设置隐私设置，不小心设置成了 whois privacy，应该设置成 no privacy。这导致后来在 github 仓库里自定义域名（Custom domain）时一直提示错误：

![](/images/2.png)

### 三.域名配置时遇到了以下问题

- 配置完 4 个 A 后在命令行里输入 nslookup wangkuo.monster 检测时一直出不来配置的四个地址，试了好多次，最后我把 cmder 重启后再运行该命令行就成功返回了配置的四个地址；

### 四.hugo 里插入图片时遇到了如下问题：

- 我是在 content-post-如何用hugo搭建个人博客.md 里插入图片，图片是 static-images-2.png，我第一次插入时的地址多写了一个/号，发现预览不出来（markdown preview）。然后我去掉最前边的/号就可以了。

## 下面写一下第一次如何用 hugo 搭建个人博客（第二次就比较简单了）：

### 1. 前提准备：

- 下载 hugo 到 D 盘的 Software 目录；
- 配置环境变量，把hugo添加进去；
- 在~/Desktop 里创建一个 Jirengu 目录；

### 2. 具体操作：

- 在命令行里进入新创建的 Jirengu 目录；
- 运行命令行：hugo new site wl-1996.github.io-creator;
- 进入新建的站点目录里；
- 运行：git init;
- 运行：git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke； 这一步如果不行，也可以下载安装包，然后复制到themes文件夹
- 运行：echo 'theme = "ananke"' >> config.toml；
- 运行：hugo new posts/my-first-post.md；目的是在content目录里创建posts文件夹和在posts文件夹里创建新文章；
- 在创建的.md 文件里改 title、draft（改为 false,否则不会发布）；
- 正式在.md 文件里写内容；
- 写完文章后命令行里运行：**hugo server -D**在线预览;这一步注意运行完后会出现一个地址在倒数第二行，不要中断命令行，否则该地址将不能访问
- 定义主题，用 VS code 进入 config.toml 文件，在里边可以改语言（改为 zh-Hans）、标题、主题；
- **新开一个终端，运行：hugo；目的是创建 public 文件夹**
- 点击最外边的文件后再新建一个文件，命名为：**.ignore**，在.ignore 里写上/public/；目的是当前仓库忽略掉 public，不存储 public，public 自成一个仓库；
- 进入 public；
- 在 public 里运行：git init；
- 运行：git add .;
- 运行 git commit -v,然后在打开的 VScode 文件里写上本次操作的描述，保存，关闭；
- 在 github 里新建一个仓库 wl-1996.github.io，保持默认设置，第一次创建后以后就不用创建了；
- 运行：git remote add origin git@github.com:wl-1996/wl1996.github.io.git;这一行创建完远程仓库后会看到的，直接复制到命令行里即可
- 运行 git push -u origin master；
- 刷新 github，点击设置，在 Github Pages 里可以看到网址；如果没看到，就修改Source，改为master后就可以看到；
- 可以修改 Github Pages 里的 Custom domain（前提是你买了域名）；
