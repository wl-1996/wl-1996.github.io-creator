---
title: "v-model的使用"
date: 2020-03-28T10:04:47+08:00
draft: false
---

v-model 指令是一个简写，跟.sync 修饰符类似是一个语法糖。在表单 `<input>、<textarea> 及 <select>` 元素上创建双向数据绑定。比如在 input 表单元素里用 v-model 写法：

```xml
  <input type='text' v-model='x'>
```

那么等价于：

```xml
  <input type='text' :value='x' @input = 'x = $event.target.value'>
```

v-model 在内部为不同的输入元素使用不同的属性并抛出不同的事件：

1. text 和 textarea 元素使用 value 属性和 input 事件；
2. checkbox 和 radio 使用 checked 属性和 change 事件；
3. select 字段将 value 作为 prop 并将 change 作为事件。
