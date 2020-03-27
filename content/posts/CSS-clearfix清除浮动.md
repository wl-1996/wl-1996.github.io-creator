---
title: "CSS Clearfix类清除浮动"
date: 2020-03-26T18:06:11+08:00
draft: false
---

## 为什么要使用 clearfix 类来清除浮动？

因为在写 html+css 时，如果一个父级元素内部的子元素都是浮动的（float），那么父元素就不能被子元素撑开，如图所示：

![](/images/clearfix.png)

## 在用 clearfix 类解决这个问题之前，先来看一个简单的使用 clear 清除浮动 的例子：

![](/images/clearfix-2.png)

我们可以看到 footer 的布局方式并不是我们想让它做的，为了让 footer 置于底部，可以给 footer 加上 clear:both; 来清除 footer 两侧的浮动:

```css
.footer {
  clear: both;
}
```

![](/images/clearfix-3.png)

## 如何用 clearfix 类来清除浮动带来的影响？

首先代码示例：

```html
<body>
<div class="container">
    <div class="left">left</div>
    <div class="right">right</div>
</div>

<div class="footer">footer</div>
</body>
</html>
```

```css
.container {
  width: 500px;
  background-color: black;
}

.left {
  width: 200px;
  height: 300px;
  background-color: blue;
  float: left;
}

.right {
  width: 200px;
  height: 200px;
  background-color: yellow;
  float: right;
}

.footer {
  width: 400px;
  height: 50px;
  background-color: green;
  clear: both;
}
```

此时的代码效果如图：

![](/images/clearfix-4.png)

我们可以看到，虽然 footer 在 container 外部，却没位于底端，因为 container 内部子元素为 float，导致 container 并没有被撑开（图中根本没有黑色元素显示出来）。

如果我们给 footer 添加 clear:both;，布局问题可以被解决，但是 container 依旧没有被撑开，有一种强行解决问题的感觉。

要解决此问题，我们可以给 container 添加一个类，叫做 clearfix，下面是 clearfix 的实现形式（之一）：

```css
.clearfix:after {
  display: block;
  content: "";
  clear: both;
}
```

上述代码通过伪元素 :after 在 container 后添加内容（content），来实现清除浮动。

![](/images/clearfix-5.png)

**本博客参考以下来源：**

作者：Wenliang
链接：https://www.jianshu.com/p/9d6a6fc3e398
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
