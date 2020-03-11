---
title: "Vue"
date: 2019-12-28T21:24:44+08:00
draft: false
---

---

英文文档：https://vuejs.org/index.html

中文文档：https://cn.vuejs.org/index.html

Vue 的中英文文档都是尤雨溪写的

---

Vue 有完整版和只包含运行时版（非完整版），这两种版本的区别概括如下图所示：

![](/images/vue-1.png)

---

术语：

完整版：同时包含编译器和运行时的版本。

编译器：用来将模板字符串编译成为 JavaScript 渲染函数的代码。

运行时：用来创建 Vue 实例、渲染并处理虚拟 DOM 等的代码。基本上就是除去编译器的其它一切。

---

运行时+编译器 vs.只包含运行时：

如果你需要在客户端编译模板 (比如传入一个字符串给 template 选项，或挂载到一个元素上并以其 DOM 内部的 HTML 作为模板)，就将需要加上编译器，即完整版：

```javascript
// 需要编译器
new Vue({
  template: "<div>{{ hi }}</div>"
});

// 不需要编译器
new Vue({
  render(h) {
    return h("div", this.hi);
  }
});
```

当使用 vue-loader 或 vueify 的时候，\*.vue 文件内部的模板会在构建时预编译成 JavaScript。你在最终打好的包里实际上是不需要编译器的，所以只用运行时版本即可。

因为运行时版本相比完整版体积要小大约 30%，所以应该尽可能使用这个版本。如果你仍然希望使用完整版，则需要在打包工具里配置一个别名：

webpack:

```javascript
module.exports = {
  // ...
  resolve: {
    alias: {
      vue$: "vue/dist/vue.esm.js" // 用 webpack 1 时需用 'vue/dist/vue.common.js'
    }
  }
};
```
