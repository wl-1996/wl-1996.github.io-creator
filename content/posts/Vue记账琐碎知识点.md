---
title: "Vue记账琐碎知识点"
date: 2020-04-26T22:50:08+08:00
draft: false
---

`git reset --hard HEAD` 回到最开始的状态，会撤销所有更改，慎用。

`<router-view/>`标签将路由指向的文件内容渲染出来。

`<router-link to=""></router-link>`标签就是一个a标签，点击后会跳转到to属性指定的网址。

导航栏不要放到App.vue组件里，因为放到这里的话所有的页面都会有导航栏，导航栏成为了全局导航。应该把导航栏单独封装成一个组件，比如`Nav.vue`，然后每个页面单独引用这个`Nav.vue`组件。

方建议不要在手机端用`fixed`定位导航栏，坑多难排。但我觉得还好，导航栏不用fixed的话内容多的话会被挤到最下面。

---

导航栏制作过程中遇到的问题：

首先我将导航栏在App.vue组件里引入使用，但是我发现如果在App.vue组件引入使用的话所有页面都会有这个导航栏，比如说404页面，我并不希望404页面有导航栏。

于是我就将导航栏在不同的页面单独引入使用，但是我又发现这样每次单独引入很麻烦。

然后我就将导航栏在main.ts入口文件里全局引入，然后其他页面如果需要使用的话就只需使用即可，无需再重复引入。

---

下边代码里的`scoped`有什么用？
```css
<style lang="scss" scoped>
  .nav{
      border: 1px solid red;
  }
</style>
```
限制css样式只在本组件生效，不会影响其他组件的同名选择器的样式（比如其他组件也有nav类就不会受到影响）。

slot插槽用于复用组件。

用阴影的诀窍，不能让别人看出来你用了阴影，但我不这样认为。

下面代码中的`active-class="selected"`什么用？
```html
<router-link class="item" active-class="selected" to="/money">
  <Icon name="money"/>
  <span>记账</span>
</router-link>
```
作用就是该路由被激活时，给这个a标签添加一个`selected`的类，然后css样式就会生效。

`svgo-loader`加载器用来去掉svg文件的默认颜色（fill属性）,先下载，然后修改配置：vue.config.js。

---

使用：`@import '~@/assets/styles/test.scss'`这个语法引入scss文件时需要对webStorm进行webpack的配置：

![](/images/webpack-15.png)

---

一个文件如果超过150行，那么我们就把它拆分为多个文件，这就是模块化。

---

scss文件里可以使用变量，例如：
```scss
$font-hei: -apple-system, "Noto Sans",
 "Helvetica Neue", Helvetica, "Nimbus Sans L", 
 Arial, "Liberation Sans", "PingFang SC", 
 "Hiragino Sans GB", "Noto Sans CJK SC", 
 "Source Han Sans SC", "Source Han Sans CN", 
 "Microsoft YaHei", "Wenquanyi Micro Hei", 
 "WenQuanYi Zen Hei", "ST Heiti", SimHei, 
 "WenQuanYi Zen Hei Sharp", sans-serif;
```

---

helper.scss里面只能放变量，方法（函数），mixin，不要放别的东西。因为这些东西最后会消失，放别的不会消失，代码会臃肿。