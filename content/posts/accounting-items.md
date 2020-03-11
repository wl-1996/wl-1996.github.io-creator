---
title: "Accounting Items"
date: 2020-03-10T13:59:51+08:00
draft: false
---


# 项目创建过程：

## 1. figma制作设计稿

该项目有四个页面，分别是记账页，标签页，编辑标签页，和统计页。

## 2. 前置条件：

1. 全局安装@vue/cli@4.1.2
2. 安装Node.js v10版本
3. VSCode最新版或者WebStorm最新版
4. 能FQ

## 3. 安装Node.js 10 和 @vue/cli@4.1.2

一、安装 Node.js 10

理论上来说只要你的版本是 10 以上（10、12）都可以，但保险起见，还是保持版本一致比较好。如果你是老手，可以使用 nvm 来安装 Node.js 10，与其他版本共存；新手请按照下面的步骤做

1. 运行 node --versioin 查看版本，如果不是 10，请先卸载当前版本：进入控制面板点击卸载即可（Mac 用户使用 brew uninstall node）
2. 去 Node.js 官网下载第 10 版的安装包
3. 一路点击下一步，注意安装目录可以改，一定不要在路径中出现中文和空格

注意重装 Node.js 后，你以前用 npm 或 yarn 全局安装的命令可能都会消失不见，如果你需要，可以需要重新全局安装这些命令。

```
npm install -g nrm --registry=https://registry.npm.taobao.org
nrm use taobao
```

二、安装 @vue/cli@4.1.2

如果你已经安装了其他版本的 @vue/cli ，请先卸载

命令行运行`vue --version` 如果这个命令打印出一个版本号，而版本号又不是 4.1.2 就说明你需要卸载：
```
yarn global remove @vue/cli
```
然后就可以安装了，命令如下：
```
yarn global add @vue/cli@4.1.2
```

```
vue --version  
```
版本号应该是 4.1.2,如果你是老手，想要使用更高版本的 @vue/cli，可以创建项目后，参考官方的升级教程（新手不用看）

建议安装指定版本，否则可能会报错

## 4. 创建项目：

命令行运行步骤：

1. vue create accounting-items ,意思是创建一个vue项目，名字是accounting-Items 注意运行该命令行后需要进行选项配置
    ![](/images/vue-create.png)

2. cd Accounting-Items ,表示进入该文件夹
3. yarn serve ,为了在线预览，可以用编辑器打开上边创建的文件夹，然后用编辑器终端运行该命令

## 5. 目录说明

![](/images/vue-muLuShuoMing.png)

# webStorm编辑器修改单文件组件模版：

![](/images/modify-template.png)

# import alias  '@' 设置

1. 在一个文件导入另一个js文件时，如何设置用 @ 表示 src 目录？从而避免思考路径问题

    直接这样引入即可`import x from '@/components/HelloWorld.vue'`,无需设置

2. css文件引入就需要设置了：
   
    首先引入语法是：`@import '~@/assets/styles/test.scss'`;

    其次设置方法是：![](/images/alias.png)
    


