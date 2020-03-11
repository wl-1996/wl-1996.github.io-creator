---
title: "Vue中的.sync修饰符"
date: 2020-01-03T19:40:27+08:00
draft: false
---

## Vue 中的.sync 修饰符有什么用？

### 概述：

.sync 就是一个语法糖，它会被扩展为一个自动更新父组件属性的 v-on
监听器。

示例代码：

```
<Child :money.sync="total" />
```

会被扩展为：

```
<!-- :money="total" 的意思是把变量total的值赋给money -->
<!-- 后半句的意思是 当儿子组件中更新了 money 时，这个变化会同步给父组件的 total ，父组件通过 $event 获取这个儿子组件中的变化-->
<Child :money="total" v-on:update:money="total = $event"/>
```

当子组件需要更新 money 的值时，它需要显示地触发一个更新事件：

```
<!-- 当按钮被点击时会更新money，这些变化会被父亲组件获取到 -->
<button @click="$emit('update:money', money - 100)">
```

### 例子：

猛一看不明白，下边我么通过一个实例（儿子打电话（触发事件）向爸爸要钱）来说明这个代码到底是怎么运用的。

**main.js 文件：**

```
import Vue from "vue";
import App from "./App.vue";

Vue.config.productionTip = false;

new Vue({
  render: h => h(App)
}).$mount("#app");

```

**App.vue 文件：**

```
<template>
  <div class="app">
    App.vue 我现在有 {{total}}
    <hr />
    <Child :money.sync="total" />
    <!-- 上边代码等价于： -->
    <!-- <Child :money="total" v-on:update:money="total = $event"/> -->
  </div>
</template>

<script>
  import Child from "./Child.vue";
  export default {
    data() {
      return { total: 10000 };
    },
    components: { Child: Child }
  };
</script>

<style>
  .app {
    border: 3px solid red;
    padding: 10px;
  }
</style>
```

**Child.vue 文件：**

```
<template>
  <div class="child">
    {{money}}
    <button @click="$emit('update:money', money - 100)">
      <span>花钱</span>
    </button>
  </div>
</template>

<script>
export default {
  props: ["money"]
};
</script>

<style>
.child {
  border: 3px solid green;
}
</style>
```

**展示页面：**

![](/images/Vue-10.png)

![](/images/Vue-11.png)

vue 修饰符 sync 的功能是：当一个子组件改变了一个 prop(外部属性) 的值时，这个变化也会同步到父组件中所绑定。

### 总结-Vue 规则：

1. 组件不能修改 props 外部数据；
2. `this.$emit` 可以触发事件，并传参；
3. `$event` 可以获取 `$emit` 的参数
4. `:money.sync="total"` 等价于 `:money="total" v-on:update:money="total=$event"`

## 参考博客：

1. https://www.jianshu.com/p/6b062af8cf01
2. https://segmentfault.com/a/1190000010700521
