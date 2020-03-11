---
title: "HTML入门笔记"
date: 2019-08-26T21:20:53+08:00
draft: false
---

# HTML 入门笔记

## [HTML 全解]HTML 概览

### 一.WWW 的历史

1. html 是李爵士 1990 年左右发明的，他的英文名字是 Tim Berners-Lee;
2. 李爵士写了第一个浏览器、第一个服务器、发明了 HTML、HTTP、URL，用自己写的浏览器访问了自己写的服务器;
3. WWW 指的是万维网，英文是 world wide web;
4. WWW=URL+HTTP+HTML;
5. html:hypertext markup language;http:一种传输协议；url：uniform resource locator;
6. 万维网与互联网的关系：万维网是基于互联网实现的，输入地址就能看到网页的 1 个网络；

### 二.HTML 语法

1. 看维基百科，不要看百度知道；
2. HTML 是结构化标记语言；
3. Markdown 也是标记语言，在 markdown 里加 url 应该这么加：[百度](http://www.baidu.com)；
4. html 最初只有 18 个标签，但是现在有 110 个标签；
5. 不要看 w3school，该网站落后并且有很多错误不及时更新。看 mdn，因为有错误会及时得到纠正；
6. h5 是什么？就是手机上可以看得页面，跟 html5 没关系，不用 html5 一样可以做出在手机上可以看的页面；
7. 查资料有两种方式：mdn 和 html 文档，html 文档晦涩难懂，不推荐；
8. W3C 是 html 的标准制定者；
9. 记得 CRM 学习法：copy+run+modify；
10. 如果面试官问你英文是什么意思，你就把它翻译成中文即可；
11. `<!DOCTYPE html>`是什么意思：我是一个 html，请按照 html 解析我吧；
12. webStorm 的使用条件：4G 内存以上，容忍开一个项目要 10 秒以上，SSD 固态硬盘；
13. HTML 排错方法：

- VS code 自带的颜色提示，但是很弱，很可能查不出来；
- webStorm 的颜色提示；
- HTML5 验证器（在线验证或者通过下载 npm 工具-我下载了工具但是验证输出乱码，老师说没办法，开发者的问题）。
- 用 npm 工具验证时输入命令行：node=w3c-validator-i index.html;

14. 遇到不会的，直接搜 “什么什么 mdn”。例如 ruby 这个标签不会用，搜 ruby mdn；

## [HTML 全解]HTML 标签

1. 不要用 vs code 打开一个大目录，会很占内存，只能打开一个小目录；
2. 如何提问：

   - 代码链接
   - 期望效果
   - 实际效果

3. 如何获取代码链接？

   - JS.jiengu.com 里获取；
   - 代码沙盒-Codesandbox.io-我在 html 里用这个操作时遇到了问题，输入！号，页面不会不全代码；

4. html 起手要写这些东西：

```javascript
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta c="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    </head>
```

5. 常用的表示章节的标签

   - h1,h2,h3,h4,h5,h6
   - section
   - header
   - footer
   - 版权声明："&copy；"
   - main
   - aside
   - div

6. 全局属性有

   - class
   - contenteditable
   - hidden
   - id
   - style
   - tabindex
   - title

7. 不到万不得已，千万不要用 id 属性；
8. 优先级：JS>style>css；
9. babindex 控制 tab 的顺序，`tabindex=0` 表示最后一个，`tabindex=-1` 表示不要切换到我这里来；
10. html 有默认样式，需要干掉默认样式。常用的有这些：

```javascript
    *{
        margin:0;
        padding:0;
    }
    *::after;
    *::before{
        box-sizing:border-box;
    }
    h1,h2,h3,h4,h5,h6{
        font-weight:normal;
    }
    a{
        color:inherit;
        text-decoration:none;
    }
    ul,ol{
        list-style:none;
    }
```

11. table 标签记得一定要加如下样式

```javascript
    table{
        border-collapse:collapse;
        border-spacing:0;
    }
```

12. 经常用到的一些内容标签

    - ol+li 有序列表；
    - ul+li 无序列表;
    - dl+dt+dd 描述列表;
    - pre 处理空格缩进现象;
    - hr 加分割线;
    - br 换行;
    - a 加超链接;
    - em 强调;
    - strong 重要;
    - code 使代码等宽;
    - quote 内联引用;
    - blockquote 块级引用;
