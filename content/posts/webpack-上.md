---
title: "Webpack 上"
date: 2019-12-27T11:43:37+08:00
draft: false
---

# webpack-上

## 学习 webpack 需要的前置知识：

ES6 的使用：

1. let/const/箭头函数/.../new/class/Promise
2. 继承/this/原型

CSS 语法和布局

MVC 概念：

Model/View/Controller/EventHub

工具的使用：

1. VSCode 或 WebStorm
2. 终端命令行 npm/yarn/http-server/parcel/git

---

## webpack 用途和基本命令行

webpack 是干嘛的？

1. 转译代码（ES6 转为 ES5，SCSS 转为 CSS）
2. 构建 build
3. 代码压缩
4. 代码分析

---

查看 webpack 信息和版本：

命令行运行 npm info webpack

---

要安装最新版本或特定版本，请运行以下命令之一：

npm install --save-dev webpack

npm install --save-dev webpack@<version>

---

如果你使用 webpack 4+ 版本，你还需要安装 CLI:

npm install --save-dev webpack-cli

---

全局安装：

npm install --global webpack

---

webpack-dev-server 用于本地预览

http-server 也可以本地预览，但是功能没有 webpack 强

parcel 也可以本地预览，但是跟 webpack 不配套

---

## CRM 学习 webpack

### 目标一：用 webpack 转译 JS

首先我们创建一个目录，初始化 npm，然后 在本地安装 webpack，接着安装 webpack-cli（此工具用于在命令行中运行 webpack）：

1. mkdir webpack-demo-1 && cd webpack-demo-1
2. npm init -y //创建 package.json 文件
3. npm install webpack webpack-cli --save-dev //创建 node_modules 文件

---

创建 src 目录，在 src 里创建 index.js 文件

index.js 文件里写一句 js`console.log("hi")`

---

调用本地安装的 webpack：

命令行运行 ./node_modules/.bin/webpack --version

但是你发现这样需要打很多字，怎么办呢？

可以直接命令行运行 npx webpack ,但是这样不够稳定，容易有 bug，如果这个命令不能用了，那么就用上边的命令，手动找到 webpack 调用

---

运行 npx webpack 后报了一个错，先不管。这时你会发现 dist 里有了一个 main.js 文件。打开这个 main.js,在里面你会找到 index.js 文件里你写的的内容。说明 index.js 文件已经被转译了。

那么我想试一下 index.js 里写入更复杂的内容，能不能被 webpack 成功转译：

在 src 里新建一个 x.js 文件，里面写上：`export default "xxx"` 导出到 index.js 。index.js 文件导入 x.js:`import x from './x.js'`,再打印出`console.log(x)`

然后再运行 npx webpack ,你会发现 dist 里的 mian.js 文件成功转译了 index.js 里引入的 x.js 的内容

---

去除上边运行 npx webpack 命令行出现的警告：

警告内容是`WARNING in configuration The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.`

解决思路：读警告，提示配置警告，mode 没有设置。那么就去查官方文档，查配置，找 mode 配置粘贴进来。

解决办法：

1. 新建一个 webpack.config.js 文件
2. 文件里写入：`module.exports = { mode: "development" };`
3. 再运行 npx webpack 就会无警告了。
4. 注意：如果是开发中，就用`mode: development`，如果是发布，就用`mode:production`

---

webpack.config.js 配置文件中的 entry 和 output 如何设置？

entry 表示设置入口文件是哪一个，我们当前的入口文件是 src 文件夹里的 index.js 文件。

output 表示设置输出内容：path 表示输出到 dist 文件夹里，filename 表示输出的文件名字是什么。

所以应该这么写：

```javascript
module.exports = {
  mode: "development",
  entry: "./src/index.js",
  output: {
    filename: "index.js"
  }
};
```

### 目标二：理解文件名中的 hash 的用途

首先配置里的 filename 应该这么写：

```javascript
module.exports = {
  mode: "development",
  entry: "./src/index.js",
  output: {
    filename: "[name].[contenthash].js"
  }
};
```

按照这个文件名，你会发现 dist 输出目录里的文件名变为了很长很长的随机数字。

---

那么这个 filename 里的 [contenthash] 是干嘛的呢？首先需要插播另一个知识： Cache-Control:

Http 缓存的作用：大大节约访问速度，无需重复请求。

比如说你第一次访问百度，百度会给你返回几个文件，有 index.html、css、js 等等。那么当你第二次访问百度的时候，这几个文件还需要再加载一遍，但是这个时候可能只需要重新加载 index.html 文件（注意 index.html 不能缓存）即可，其他的文件会直接从缓存（存储在本地计算机的内存或者硬盘里）里拉取。注意缓存到本地是有有效期的，一般的都是一年有效期，就是说一年内再次访问会直接从本地缓存拉取文件。

注意：缓存是浏览器自动的，根据百度写的代码来进行缓存，百度写了缓存一年，那么浏览器你就要缓存一年，除非文件名被百度改了，浏览器才会重新缓存新的内容。

那么问题来了：如果这些文件缓存期间被百度更改了怎么办？怎么能识别是否发生了变化呢？

答：这个缓存是跟着文件名走的，如果百度更改了文件名，那么自然就会重新请求数据。那么如何更改文件名呢？那就是用上边的 filename 里的 [contenthash]自动生成新的文件名，当你这么设置 filename 的时候，webpack 会每次文件发生变动打包时，都自动帮你生成新的文件名。

---

每次打包都生成新的 dist 文件，并不会覆盖原来的，这样 dist 文件夹里的文件越来越多，怎么办？

应该每次打包之前删除旧的 dist 文件：命令行运行 rm -rf dist 后再运行 npx webpack

---

但是这样每次都要操作两步，不但麻烦，还容易遗忘，如何解决？

在 package.json 文件里加上一句代码：![](/images/webpack-1.png)

以后打包时直接运行 yarn build 就行了

### 目标三：用 webpack 生成 HTML

注意虽然是学习，也记得 git 提交，便于后续出错撤销修改。（我不知道怎么更改，难道是之前讲的版本控制，忘了，回头再温习吧）。注意添加.gitignore 文件忽略 dist 和 node_modules 文件夹。

---

生成 HTML 内容只需要在 webpack 配置文件里加上两句代码：![](/images/webpack-2.png) ，然后再 yarn build 打包就行了

打包后就会在 dist 文件夹里自动生成一个 html 文件，并且这个 html 文件自动引入打包后的 入口 js 文件：![](/images/webpack-3.png)

---

注意这个配置里的输出文件名是可以改的：![](/images/webpack-4.png) ,改完后重新打包后输出目录 dist 里的 js 文件就会变为你改的名：![](/images/webpack-5.png)

---

但是这个生成的 html 什么也没有，加不了东西啊，怎么办？ 在 src 里创建一个 assets 文件夹，再创建一个 index.html 文件。然后修改配置：![](/images/webpack-6.png)

注意咱们自己写的 html 文件里 title 名的修改：![](/images/webpack-7.png)

---

修改 viewport,抄淘宝的

### 目标四：用 webpack 引入 CSS

先下载 style-loader 和 css-loader:

1. npm install --save-dev css-loader
2. npm install --save-dev style-loader

---

再修改配置：![](/images/webpack-8.png)

---

然后新建 css 文件，在 js 文件里引入 css 文件，然后再重新打包就行了。

打包后怎么验证是否成功引入了 css 呢？进入 dist 目录，用 http-server . -c-1 预览

---

这样每次都要先进到根目录运行打包命令，然后再进入到 dist 目录运行 http-server . -c-1 预览，太麻烦怎么办呢？

1. 首先把配置改为开发环境（development）
2. 安装：npm install --save-dev webpack-dev-server
3. 修改配置：![](/images/webpack-9.png)
4. 修改 package.json 文件：![](/images/webpack-10.png)
5. 命令行运行 yarn start 即可自动打开浏览器进行预览

---

但是这样引入的 css 是写到 sytle 标签里的 ![](/images/webpack-11.png)，怎么引入一个文件 css 呢？

1. 下载插件 npm install --save-dev mini-css-extract-plugin
2. 修改配置：

```javascript
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  plugins: [new MiniCssExtractPlugin()],
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: [MiniCssExtractPlugin.loader, "css-loader"]
      }
    ]
  }
};
```

3. yarn build 打包

---

如何修改输出的 css 文件名，便于浏览器缓存呢？

在配置里修改一行代码：![](/images/webpack-12.png)

---

webpack 开发有两种模式，一种是 development 模式，一种是 production 模式，能不能设置自动切换呢？

npx webpack--help 帮助文档命令

1. 新建 webpack.config.prod.js ，记得修改两个地方：![](/images/webpack-13.png)
2. 设置 package.json 里的配置即可：

```javascript
  "scripts": {
    "build": "rm -rf dist && npx webpack --config webpack.config.prod.js",
  },

```

3. 那么以后开发用 yarn start 命令，上线用 yarn build 命令

---

有人说了，这两个配置文件重复太多啊感觉，怎么办呢？抽离出共有的放到一个 base 文件里，然后利用继承的思想继承共有的，再修改单独的。妈的 不写了，太费事。

---

loader 和 pulgin 的区别是什么？

1. 翻译：loader 是加载器，plugin 是插件；
2. loader 加载器，用来加载一个文件。比如说 babel-loader 用来加载高级 js，把它变成 IE 支持的 js；css-loader 和 style-loader 用来加载 css，并把它变成页面中的 style 标签。
3. plugin 插件用来加强功能。比如 HtmlWebpackPlugin 插件用来生成一个 html 文件；MiniCssExtractPlugin 用来抽取 css 代码变成单独的的文件，插入到 html 里。

### 目标五：用 webpack 引入 SCSS（SASS）

配置文件：

```javascript
module: {
  rules: [
    {
      test: /\.s[ac]ss$/i,
      use: [
        // Creates `style` nodes from JS strings
        "style-loader",
        // Translates CSS into CommonJS
        "css-loader",
        {
          // Compiles Sass to CSS
          loader: "sass-loader",
          options: {
            implementation: require("dart-sass")
          }
        }
      ]
    }
  ];
}
```

### 目标六：用 webpack 引入 LESS 和 Stylus

### 目标七：用 webpack 引入图片

### 目标八：使用懒加载

### 目标九：部署到 Github Pages

### 目标十：自己写一个 loader

### 目标十一：自己写一个 plugin

## 总结
