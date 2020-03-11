---
title: "Vue 数据响应式"
date: 2020-01-01T10:32:45+08:00
draft: false
---

什么是响应式？

我打你一拳，你会喊疼，那你就是响应式的。也就是说如果一个物体能对外界的刺激做出反应，它就是响应式的。

---

Vue 的 data 是响应式的：

在 vm 示例：`const vm = new Vue({data:{n:0}})`中，

我如果修改了 vm.n,那么 UI 中的 n 就会响应我，

Vue 2 通过 Object.defineProperty 来实现数据响应式。

---

数据响应式原理：

当把 options.data 传给 Vue 之后：

1. data 会被 Vue 监听
2. data 会被 Vue 实例代理
3. 每次对 data 的读写都会被 Vue 监控
4. Vue 会在 data 变化时更新 UI

---

Vue 有 bug，代码示例：

```javascript
import Vue from "vue/dist/vue.js";
// import App from "./App.vue";

Vue.config.productionTip = false;

new Vue({
  data: {
    obj: {
      a: 0
    }
  },
  template: `
    <div>
      {{obj.b}}
      <button @click="setB">set b</button>
    </div>
  `,
  methods: {
    setB() {
      this.obj.b = 1;
    }
  }
}).$mount("#app");
```

此时页面显示：![](/images/Vue-2.png)

bug: 点击按钮后页面仍无法出现 1 。原因是：Vue 没法监听一开始不存在的 obj.n。

解决办法有两个：

1.把 key 都声明好，后面不再加属性不就行了；

```javascript
import Vue from "vue/dist/vue.js";
// import App from "./App.vue";

Vue.config.productionTip = false;

new Vue({
  data: {
    obj: {
      a: 0,
      b: undefined //提前把b属性写好不就行了
    }
  },
  template: `
    <div>
      {{obj.b}}
      <button @click="setB">set b</button>
    </div>
  `,
  methods: {
    setB() {
      this.obj.b = 1;
    }
  }
}).$mount("#app");
```

2.使用 `Vue.set` 或者 `this.$set` ：

```javascript
import Vue from "vue/dist/vue.js";
// import App from "./App.vue";

Vue.config.productionTip = false;

new Vue({
  data: {
    obj: {
      a: 0
    }
  },
  template: `
    <div>
      {{obj.b}}
      <button @click="setB">set b</button>
    </div>
  `,
  methods: {
    setB() {
      Vue.set(this.obj, "b", 1); //给data.obj 加一个 b 属性，并赋值为 1
      //   或者可以这么写：
      //   this.$set(this.obj,'b',1)
    }
  }
}).$mount("#app");
```

---

Vue.set 和 this.\$set 做了什么？

1. 新增 key
2. 自动创建代理和监听（如果没有创建过）
3. 触发 UI 更新（但并不会立刻更新）

代码写法：`this.$set(this.obj,'n',100` 意思就是给 this.obj 对象设置一个新属性 n ，并赋值为 100 。

---

data 中有数组怎么办？你没法提前声明所有的 key ，那么难道每次改数组都要用 Vue.set 或者 this.\$set 么？

尤雨溪的做法：

篡改数组的 API，有 7 个 API 被 Vue 篡改了，调用后会更新 UI

代码示例：

```javascript
import Vue from "vue/dist/vue.js";
// import App from "./App.vue";

Vue.config.productionTip = false;

new Vue({
  data: {
    array: ["a", "b", "c"] //此时 data 中是数组
  },
  template: `
    <div>
      {{array}}
      <button @click="setB">set b</button>
    </div>
  `,
  methods: {
    setB() {
      this.array.push("d"); //这个push方法就是被篡改的7个API之一
      console.log(this.array);
    }
  }
}).$mount("#app");
```

此时点击 button 按钮就会给显示新加的数组的项：![](/images/Vue-3.png)
