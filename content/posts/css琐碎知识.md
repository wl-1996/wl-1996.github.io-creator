---
title: "Css琐碎知识"
date: 2019-11-28T17:09:36+08:00
draft: false
---

# css 小知识

## 琐碎布局经验

1. z-index 对没有定位的元素无用，只有定位的元素才可以用`z-index: 2;`
2. 元素不脱标的情况下`margin:0 auto;`才有用,即只有标准流才能用`marign: 0 atuto;`。此外`position: relative;`并不脱标，保留原坑，所以相对定位的元素可以用`margin: 0 auto;`居中
3. 插入一张通栏的大图：
   1. 方法一：直插法-直接插入图片:
   ![](/images/css.png)
   2. 方法二：用背景图片做
4. 绝对定位的盒子已经脱离标准流了，不能自动撑满父亲，因此一定要写宽度;
5. 同一层级后定位的元素会压住先定位的元素；
6. 邵说不要用 margin 踹它的父亲，这样做不好，应该用老爸的 padding 挤走儿子；
7. 绝对定位的元素无视父亲的 padding；
8. `display: none;`与`visibility: hidden`的区别：
   1. `display: none;`会放弃原来的位置，如同标签没有写；
   2. `visibility: hidden;`不会放弃原来的位置，只是把元素隐藏掉；

## 几种定位的区别

1. `position: static;`
   - 默认的定位方式
2. `position: relative;`相对定位
   - 适用轻微偏移的场景；比如“图标因为本身的设计问题导致无法居中”；
   - 未脱离标准流，保留原来的位置；
   - 参考点是原来的位置；
3. `position: absolute;`绝对定位
   - 适用出现元素重叠，放置任意位置的场景；
   * 脱离普通流，不保留原来的位置；
   * 参考点是向上层找非 **static** 的定位元素；
   * 不会产生**边距合并**现象；
   * 绝对定位的元素收缩，需要设置宽度；
   * 给**行内元素**设置绝对定位后就有了块级的特性，可以设置宽高，类似于浮动的元素；
4. `position: fixed;`固定定位
   - 使用需持续固定在浏览器某位置的场景，比如“企业站窗口底部的联系我，某些固定在页面顶部的导航条”；
   * 脱离普通流，不保留原来的位置；
   * 参考点是浏览器屏幕视口；
   * 一定要设置 **top/bottom**，否则可能无法展示；
5. `position: sticky;`
   - 可以理解为相对定位和固定定位的混合，元素在跨越特定阀值前为**相对定位**，之后为**固定定位**；
   - 一定要设置 **top/bottom**，否则可能无法展示；

## 伪类与伪元素

1. 伪类**一个**冒号，伪元素**两个**冒号；
2. 伪元素的样式一定要加上:
```css
    a::before{ 
        content:''; display:block; 
    }
```
3. 伪元素用途：常用来替代**图标**，**无实际意义的标签**；

## 样式表

### 行内样式

行内样式丧失了css的一些优点，工作很少用，简单了解即可。

行内样式的权重在**某些情况下**是最大的，可以认为是无穷大，任何的权重都比不上行内。
但是没有`!important`大(这句话说的不准确，根据下边的代码自己体会吧)。
```html
<div id="father">
    <div id="box" style="color:red">
        你猜文字是什么颜色？
    </div>
</div>
<style>
    #father #box{
        color:green;
    }
</style>
```
答案：**红色**，因为行内样式权重**某些情况下**最大。

```html
<div id="father">
    <div id="box" style="color:red">
        你猜文字是什么颜色？
    </div>
</div>
<style>
    #father #box{
        color:green!important;
    }
</style>
```
答案是：**绿色**，因为加了`！important`

### 导入式的样式表

这种样式表已经绝迹了，没有人再用了，因为四不像，又像内嵌的，又像外链的。

代码示例：
```html
<style type="text/css">
    @import url(css/style.css);
    body{
        background-color: pink;
    }
</style>
```
用`@important url(地址)`必须写在**第一行**，这种写法就是理论上的意义，无实战意义，
像是内嵌与外链样式表的混合，感觉多此一举。

## 样式表的组织

css是分层次的：![css层次](/images/css-tier.png)
也就是说一个网站可能要引入好几个样式表。

1. reset.css

   * 让所有的元素都有默认的样式，比如让ul没有小圆点，比如设置h1字号为22px...

   * 最最著名的reset.css就是雅虎公司的YUI的reset.css,网址是：[点击这里](https://yuilibrary.com/yui/docs/cssreset/)
   * `*{margin : 0 ; padding : 0 ;}`有超过1秒的渲染时间。所以可以用上边网址里的css.reset里的来代替；

2. base.css

   * 放公共类（原子类）的，这个网上没有说哪个非常好，**自己设置比较顺手**

   * ![公共类/原子类](/images/base-css.png)

3. common.css

   * 页面与页面之间共有的样式部分

4. page.css(我习惯用style.css,这只是一个名字)
   
   * 每个页面自己独有的样式

## 精灵的摆放

1. 摆放位置很有讲究，需要摸索加google学习，有的需要放在两边，具体原因我也没搞懂
![精灵图片](/images/icons.png)

2. 先导符号一般放在元素的padding里
![这是示例](/images/icons-explain.png)

3. 博雅二级页面精灵的放置：

## css钩子

```html
<i></i>
<b></b>
<s></s>
```

## 浮动

1. 只要父盒子内部有浮动的元素，那么该父元素就要用`overflow:hidden;`来清除浮动，否则会影响下边的元素。

2. 浮动的元素不写宽度，会自动收缩为儿子的宽度

## `display:inline-block;`

标准流把元素分为行内、块级，性质截然不同：

1. 行内：能并排，但是不能设置宽高
2. 块级：能设置宽高，但不能并排
3. 而`display:inline-block`让元素同时具有行内和块级的特性，即既能并排，又能设置宽高
4. 设置为行内块的元素在高级浏览器中与`float:left`非常像，但是有一个非常大的不同，就是会出现空白折叠现象：

    代码如下：

   ```html
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        .box1 {
            display: inline-block;
            width: 200px;
            height: 200px;
            background: red
        }

        .box2 {
            display: inline-block;
            width: 200px;
            height: 200px;
            background: green
        }

        .box3 {
            display: inline-block;
            width: 200px;
            height: 200px;
            background: blue
        }
    </style> 

    <body>
        <div class="box1"></div>
        <div class="box2"></div>
        <div class="box3"></div>
    </body> 
   ```

    折叠现象如下：![空白折叠](/images/kongbaizhedie.png)

5. `display:inline-block;`这么好用，为什么不用用浮动呢？有以下几个原因
   * 空白折叠现象
   * 只有左，没有右（float可以右浮动）
   * 盒子加上**inline-block**后，就和文本一样了，div变为文本，感觉别扭
   * 时代原因，在抢占技术实施标准那几年（2007 2008年），没有火起来

## iconfont

字体图标。图标不是图标了，也不是精灵了，而是字了。字怎么是图标，这个字在某一个字体下，就是图标。