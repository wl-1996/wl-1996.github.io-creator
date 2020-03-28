---
title: "ClassPrefix的使用"
date: 2020-03-27T16:52:14+08:00
draft: false
---

classPrefix 用于给组件里的元素批量添加类

下边通过两个组件Money.vue和Layout.vue来说明如何使用classPrefix。

这是Money.vue组件的代码：
```vue
<template>
  //这里用到了Layout组件，Layout组件接受一个外部属性
  //这里通过传入外部属性class-prefix从而给Layout组件内部的元素批量添加类
  //注意，本来Layout接受的外部属性是： classPrefix,
  //这里怎么是class-prefix? 
  //不要纠结，好像是vue组件等号左边不能有大写字母，所以只能这样写。
  <Layout class-prefix="layout"></Layout>
</template>

<script lang="ts">
  import Vue from "vue";
  import { Component } from "vue-property-decorator";

  @Component
  export default class Money extends Vue {}
</script>

<style lang="scss">
  //上边class-prefix已经给Layout组件里的元素批量添加了类，layout-content就是其中一个
  //这里通过layout-content类设置样式:
  .layout-content {
    display: flex;
    border: 3px solid red;
  }
</style>
```

这是Layout.vue组件的代码：
```vue
<template>
  //意思是如果外部传了属性classPrefix,那么这个div元素的类就为：classPrefix-wrapper
  <div :class = ' classPrefix && `${classPrefix}-wrapper`'>
    //如果外部传了属性classPrefix,那么这个div元素就有：classPrefix-content 这个类
    <div :class = 'classPrefix && `${classPrefix}-content`'>
      <slot/>
    </div>
  </div>
</template>

<script lang='ts'>
  import Vue from 'vue';
  import {Component,Prop} from 'vue-property-decorator';

  @Component
  export default class Layout extends Vue{
      //该组件接受一个外部属性classPrefix：
      @Prop(String) classPrefix: string | undefined;
  }
</script>
```

也就是说比如有两个组件Money.vue和Layout.vue,需求是通过Money.vue组件给Layout.vue组件里的元素批量添加类，怎么做呢？

1. Layout.vue组件接受一个外部属性：classPrefix;
   ```
   @Prop(String) classPrefix: string | undefined;
   ```
2. Layout.vue组件里的元素通过v-bind动态绑定类；
   ![](/images/classPrefix.png)
3. Money.vue里引用了Layout.vue组件；
   ![](/images/classPrefix-2.png)
4. Money.vue给Layout.vue组件传入一个属性：class-prefix;
   ![](/images/classPrefix-3.png)
5. 这样Layout.vue组件里的元素就都有了一个以classPrefix为前缀的类；
   ![](/images/classPrefix-4.png)
