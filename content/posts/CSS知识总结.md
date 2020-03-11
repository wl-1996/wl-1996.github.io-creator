---
title: "CSS知识总结"
date: 2019-09-13T15:39:11+08:00
draft: false
---

# hello,好久不见

## 浏览器渲染原理：

### 步骤：

1. 根据 HTML 构建 HTML 树（DOM）;
2. 根据 CSS 构建 CSS 树（CSSOM）;
3. 将两棵树合并成一颗渲染树（render tree）;
4. Layout 布局（文档流、盒模型、计算大小和位置）;
5. Paint 绘制（把边框颜色、文字颜色、阴影等画出来）；
6. Compose 合成（根据层叠关系展示画面）;

### 如何更新样式？--一般我们用 JS 来更新样式

- 比如`div.style.background='red'`;
- 比如`div.style.display='none'`;
- 比如`div.classList.add('red')`;
- 比如`div.remove()`直接删掉节点;

### 三种更新方式：

1. JS/CSS>样式>布局>绘制>合成（全走）；
2. JS/CSS>样式>绘制>合成（跳过布局 layout）；
3. JS/CSS>样式>合成（跳过布局 layout 和绘制 paint）；

## CSS 动画的两种写法（transition 和 animation）：

### 第一种是使用 transform 和 transition 属性；

- 详情见[https://wangkuo.monster/Beating-heart/index.html];

### 第二种是使用 animation 属性设置；

- 详情见[https://wangkuo.monster/Beating-heart/index2.html];
- @keyframes 的写法有两种：

  1.  from to 写法：

  ```css
  @keyframes slidein {
    from {
      transform: translateX(0%);
    }

    to {
      transform: translateX(100%);
    }
  }
  ```

  2.  百分数写法：

  ```css
  @keyframes identifier {
    0% {
      top: 0;
      left: 0;
    }
    30% {
      top: 50px;
    }
    68%,
    72% {
      left: 50px;
    }
    100% {
      top: 100px;
      left: 100px;
    }
  }
  ```

* animation 语法-缩写语法：animation:时长|过渡方式|延迟|次数|方向|填充模式|是否暂停|动画名；

  1.  时长：1s 或者 1000ms；
  2.  过渡方式：跟 transition 取值一样，如 linear；
  3.  次数：3 或者 2.4 或者 infinite 无限次；
  4.  方向：reverse|alternate|alternate-reverse；
  5.  填充模式：none|forwards|backwards|both；
  6.  是否暂停：paused|running;
  7.  以上所有属性都有对应的单独属性；

## transform 的四个常用功能：

1. translate 位移；
2. scale 缩放；
3. rotate 旋转；
4. skew 倾斜；
