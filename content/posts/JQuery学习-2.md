---
title: "JQuery学习-2"
date: 2020-01-05T15:32:55+08:00
draft: false
---

## CSS 函数

CSS 函数可以读样式，也可以设样式。

读样式，可以读取计算后样式，写一个参数，是不是驼峰都可以，但是必须加引号：

```javascript
$("p:first").css("background-color");
$("p:first").css("backgroundColor");
```

---

设置样式，有两种语法，如果你只想设置一个样式，逗号隔开 k 和 v：

```javascript
$("p:odd").css("backgroundColor", "blue");
```

如果想设置很多样式，就写 JSON：`javascript $("p:odd").css(JSON);`

```javascript
$("p:lt(4)").css({
  width: 20,
  height: 20,
  backgroundColor: "red"
});
```

---

特别的，还支持+=写法：`$("p:eq(5)").css("width","+=20px")`

## animate 函数

animate 函数负责处理动画，语法是：`$("选择器").animate(终点JSON,动画时间,回调函数);`

```javascript
$("p").animate({ left: 1000 }, 2000, function() {
  $(this).css("background-color", "red");
});
```

---

jQuery 默认有一个处理机制，叫做动画排队。当一个元素接收到了两个 animate 命令之后，后面的 animate 函数会排队等待执行：

```javascript
$("p").animate({ left: 1000 }, 2000);
$("p").animate({ top: 400 }, 2000);
```

上边代码的顺序是：先 2000 毫秒横着跑，然后 2000 毫秒竖着跑。动画总时长 4000。

如果想让元素斜着跑，就是同时变化 left 和 top ，就写在同一个 JSON 里面：`$("p").animate({"left":1000,"top":400},2000)`

## 事件监听

语法：

```javascript
$(".box").click(function() {
  // 点击 box1 之后做的事情
});
```

JS 原生语法是：`oDiv.onclick=function(){}`

事件名一律不写 on。特别的，鼠标进入改成了 mouseenter，鼠标离开改为了 mouseleave。
