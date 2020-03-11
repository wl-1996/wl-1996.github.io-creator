---
title: "Vue模版template语法"
date: 2020-01-03T19:48:07+08:00
draft: false
---

## 模版 template 的三种写法：

### Vue 完整版，写在 HTML 里

```html
<div id="xxx">
  {{n}}
  <button @click="add">+1</button>
</div>
```

```javascript
new Vue({
  el: "#xxx",
  data: { n: 0 }, //data可以改成函数
  methods: {
    add() {}
  }
});
```

### Vue 完整版，写在选项里

```html
<div id="app"></div>
```

```javascript
new Vue({
  template: `
      <div>
        {{n}}
        <button @click="add">+1</button>
      </div>
    `,
  //data可以改成函数
  data: {
    n: 0
  },
  methods: {
    add() {
      this.n += 1;
    }
  }
}).$mount("#app");
// 注意一个细节：div#app 会被替代
```

### Vue 非完整版，配合 xxx.vue 文件

```
<template>
  <div>
    {{n}}
    <button @click="add">+1</button>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        n: 0
      };
    },
    methods: {
      add() {
        this.n += 1;
      }
    }
  };
</script>

<style>
  /* 这里写css内容 */
</style>
```

```javascript
import Xxx from "./xxx.vue";
// Xxx是一个 options 对象
new Vue({
  render: h => h(Xxx)
}).$mount("#app");
```

## 模版 template 里有哪些语法？

### 展示内容：

1.表达式：

```xml
{{ object.a }} //选项里的data里的object对象的a属性
{{ n+1 }} //运算，加减乘除运算都可以，不支持if...else
{{ fn(n) }} //调用methods里的fn函数

//下边的写法基本没人用，大家都用花括号写法
<div v-text="表达式"></div> //与两个花括号的写法是一样的，可替代花括号的写法
```

2.HTML 内容：

假设 `data.x` 值为 `<strong>hi</strong>`, 那么`<div v-html="x"></div>`即可显示粗体的 hi

3.假如你就想展示 {{ n }},那么这么写:

```xml
//因为 v-pre 不会对模版进行编译
<div v-pre>{{ n }}</div>
```

### 绑定属性：

绑定 src：

```xml
<img v-bind:src="x" /> //x的值是一个url，data里的x
//也可简写为 <img :src="x" />
```

绑定对象：

```xml
//注意这里可以把 '100px' 写成 100
<div :style="{border:'1px solid red',height:100}"></div>
```

### 绑定事件：

v-on:事件名

```xml
// 点击之后，Vue会运行add()
<button v-on:click="add">+1</button>
// 点击之后，Vue会运行xxx(1)，不点击不运行
<button v-on:click="xxx(1)">xxx</button>
// 点击之后，Vue会运行n+=1
<button v-on:click="n+=1">n+=1</button>
```

缩写：`<button @click="add">+1</button>`

### 条件判断：

if...else

```xml
//如果x>0，就显示这个div
<div v-if="x>0">x大于0</div>
//如果x === 0 ,就显示这个div
<div v-else-if="x===0">x为0</div>
//如果x小于0，就显示这个div
<div v-else>x小于0</div>
```

### 循环：

for(value,key) in 对象或数组

```xml
//数组的例子：
<ul>
  <li v-for="(u,index) in users" :key="index">
    索引：{{index}} 值：{{u.name}}
  </li>
</ul>

//对象的例子
<ul>
  <li v-for="(value,name) in obj" :key="name">
    属性名：{{name}},属性值：{{value}}
  </li>
</ul>
```

### 显示、隐藏

v-show:

```xml
//如果表达式为真，这个div就显示，否则不显示div
<div v-show="n%2===0">n是偶数</div>
```

近似等于：

```xml
//注意，看的见得元素display不只有block，
//比如，table的display为table；
//li的display为list-item
<div :style="{display:n%2===0 ? 'block' : 'none'}">n是偶数</div>
```

## 总结：

Vue 模版主要特点有：

1. 使用 XML 语法（不是 HTML）
2. 使用`{{}}` 插入表达式
3. 使用 v-html v-on v-bind 等指令操作 DOM
4. 使用 v-if v-for 等指令实现条件判断和循环
