---
title: "JQuery学习-3"
date: 2020-01-08T15:29:26+08:00
draft: false
---

## stop()方法

表示停止当前的 animate 动画，但是不清除队列，立即执行后面的 animate 动画：

```javascript
$("div").stop(); //等价于$(“div”).stop(false,false);
```

---

停止当前的 animate 动画，并且清除队列，盒子留在了此时的位置：

```javascript
$("div").stop(true); //等价于$(“div”).stop(true,false);
```

---

瞬间完成当前的 animate 动画，并且清除队列：

```javascript
$("div").stop(true, true);
```

---

瞬间完成当前的 animate 动画，但是不清楚队列，立即执行后面的动画：

```javascript
$("div").stop(false, true);
```

---

公式：

```javascript
stop(是否清除队列, 是否瞬间完成当前动画);
```

如果没有写 true 或者 false，默认是 false

## 节点

### 节点类型

任何 HTML 元素，都有 nodeType 属性，值有 1~11 个，但是我们只要记住 5 个常用的即可：

1：元素节点

3：文本节点

8：注释节点

9：document 节点

10：DTD

代码示例：

```html
<!-- 注意下边代码不能换行，否则有兼容性问题，下节节点关系再说 -->
<div id="box>文本<p></p><!-- 这是注释 --></div>
```

```javascript
  let box = document.getElementById("box") alert(box.nodeType) //1
  alert(box.childNodes[0].nodeType) //3
  alert(box.childNodes[1].nodeType) //1
  alert(box.childNodes[2].nodeType) //8
  alert(document.nodeType) //9
```

### 节点关系

childNodes:任何节点都有 childNodes 属性，里面是一个类数组对象，存放这自己所有的儿子。

下边说一下上一小节代码里提到的兼容问题，代码示例：

```html
<div id="box">
  <p></p>
  <p></p>
  <p></p>
  <p></p>
</div>
```

```javascript
let box = document.getElementById("box");
// 问题来了，高级浏览器对下面的代码会弹出 3
// 因为高级浏览器认为换行和空格也是文本节点
// 而IE6、7、8 则弹出 1
alert(box.childNodes[0].nodeType);

// 高级浏览器弹出 9 ，IE6、7、8 则弹出 4
alert(box.childNodes.length);
```

---

parentNode:某个元素的父节点

---

previousSibling:某个元素的上一个兄弟姐妹

nextSibling:某个元素的下一个兄弟姐妹

这个也有 bug，代码示例：

```html
<div id="box“>
  <p>AAA</p>
  <p>BBB</p>
  <p>CCC</p>
  <p>DDD</p>
</div>
```

```javascript
let ps = document.getElementsByTagName("p");
// 高级浏览器认为上一个兄弟空文本节点
// 而IE认为上一个兄弟是BBB那个p
console.log(ps[2].previousSibling);
```

---

firstChild:第一个孩子。也不好用，因为 IE6、7、8 认为
第一个孩子是节点，而 Chrome 认为第一个孩子是空文本。

lastChild:最后一个孩子
