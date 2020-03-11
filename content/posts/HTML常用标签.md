---
title: "HTML常用标签"
date: 2019-09-06T18:13:05+08:00
draft: false
---

# hello,好久不见

今天学习了《HTML全解》HTML重难点这部分视频，最近私事较多，已经落后大家很多了，刚回来学习，要马上跟上进度。

## a标签的用法
1. 属性有href/target/download/rel=noopener；
2. a标签的作用是：
跳转外部页面/跳转内部锚点/跳转到邮箱或者电话；
3. a的href属性取值有四种：
   * 网址：https://google.com，http://google.com，//google.com，第三种写法最简单；
   * 路径：/a/b/c以及a/b/c，index.html以及./index.html;
   * 伪协议：javascript：代码。mailto：邮箱。tel：手机号。
   * id：href=#xxx
1. a的target的取值：
   * 内置名字：_blank，_top,_parent,_self;
   * 程序员命名：window的name,iframe的name;
5. a的download：
   * 作用：不是打开页面，而是下载页面；
   * 问题：不是所有的浏览器都支持，尤其是手机浏览器可能不支持；

## img标签的用法
1. 作用：发出get请求，展示一张图片；
2. 属性有alt/height/width/src;
3. 时间有onload/onerror；
4. 图片过大时候如何在手机端实现自适应？——在css样式里加上:
```
img{
    max-width:100%;
}
```

## table标签的用法
1. 相关的标签有table/thead/tbody/tfoot/tr(行)/td(数据)/th(标题);
2. 相关的样式有table-layout（调整列宽）/border-collapse（设置单元格之间有无缝隙）/border-spacing（设置单元格之间缝隙大小）;

## input标签的用法
1. 有以下属性：text-文本框/color-颜色/password-密码框/radio-单选框/checkbox-复选框/file-上传文件/hidden-表示看不见的输入；
2. 其中单选框属性(radio)需要设置相同的名字才是单选，否则就是可以都选中：
```
<input name="gender" type="radio" />男
<input name="gender" type="radio" />女
```
3. 上传文件属性（file），如果向上传多个文件需要加上multiple：
```
<input type="file" multiple />
```
