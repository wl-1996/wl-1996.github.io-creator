---
title: "Vue记账琐碎知识点"
date: 2020-04-26T22:50:08+08:00
draft: false
---

`git reset --hard HEAD` 回到最开始的状态，会撤销所有更改，慎用。

`<router-view/>`标签将路由指向的文件内容渲染出来。

`<router-link to=""></router-link>`标签就是一个a标签，点击后会跳转到to属性指定的网址。

导航栏不要放到App.vue组件里，因为放到这里的话所有的页面都会有导航栏，导航栏成为了全局导航。应该把导航栏单独封装成一个组件，比如`Nav.vue`，然后每个页面单独引用这个`Nav.vue`组件。

方建议不要在手机端用`fixed`定位导航栏，坑多难排。但我觉得还好，导航栏不用fixed的话内容多的话会被挤到最下面。

---

导航栏制作过程中遇到的问题：

首先我将导航栏在App.vue组件里引入使用，但是我发现如果在App.vue组件引入使用的话所有页面都会有这个导航栏，比如说404页面，我并不希望404页面有导航栏。

于是我就将导航栏在不同的页面单独引入使用，但是我又发现这样每次单独引入很麻烦。

然后我就将导航栏在main.ts入口文件里全局引入，然后其他页面如果需要使用的话就只需使用即可，无需再重复引入。

---

下边代码里的`scoped`有什么用？
```css
<style lang="scss" scoped>
  .nav{
      border: 1px solid red;
  }
</style>
```
限制css样式只在本组件生效，不会影响其他组件的同名选择器的样式（比如其他组件也有nav类就不会受到影响）。

slot插槽用于复用组件。

用阴影的诀窍，不能让别人看出来你用了阴影，但我不这样认为。

下面代码中的`active-class="selected"`什么用？
```html
<router-link class="item" active-class="selected" to="/money">
  <Icon name="money"/>
  <span>记账</span>
</router-link>
```
作用就是该路由被激活时，给这个a标签添加一个`selected`的类，然后css样式就会生效。

`svgo-loader`加载器用来去掉svg文件的默认颜色（fill属性）,先下载，然后修改配置：vue.config.js。

---

使用：`@import '~@/assets/styles/test.scss'`这个语法引入scss文件时需要对webStorm进行webpack的配置：

![](/images/webpack-15.png)

---

一个文件如果超过150行，那么我们就把它拆分为多个文件，这就是模块化。

---

scss文件里可以使用变量，例如：
```scss
$font-hei: -apple-system, "Noto Sans",
 "Helvetica Neue", Helvetica, "Nimbus Sans L", 
 Arial, "Liberation Sans", "PingFang SC", 
 "Hiragino Sans GB", "Noto Sans CJK SC", 
 "Source Han Sans SC", "Source Han Sans CN", 
 "Microsoft YaHei", "Wenquanyi Micro Hei", 
 "WenQuanYi Zen Hei", "ST Heiti", SimHei, 
 "WenQuanYi Zen Hei Sharp", sans-serif;
```

helper.scss里面只能放变量，方法（函数），mixin，不要放别的东西。因为这些东西最后会消失，放别的不会消失，代码会臃肿。

---

button元素的字体不会继承父元素，除非你设置：
```css
font: inherit;
```

---

按钮元素点击时不想出现轮廓怎么办？——加样式即可：
```css
:focus{
  outline: none;
}
```

---
下面代码中的`.native`和`$router.back()`是干什么的？

```html
<header>
  <Icon @click.native="$router.back()" class="left" name="left"/>
  <span>编辑标签</span>
  <span></span>
</header>
```
路由回退，就是说当点击这个icon图标时浏览器会回退到上个页面。

---

如何将一个日期对象变为字符串？-用`toISOString()`

```js
new Date().toISOString()
```
注意：用这个API转化的时间是**零时区**的时间，不是东八区北京时间。

将`new Date()`出来的日期对象变为时间字符串就用`toISOString()`这个API；

将`toISOString()`后的日期字符串重新变为日期对象用两个API：

1. `Date.parse("日期字符串")`
2. `new Date(第一步的值)`

获取日期对象的小时用`getHours()`这个API

如图所示：![](/images/ISO-8001.png)

---

我们不用原生日期相关的API，我们用`moment.js`或者`day.js`;建议用`day.js`，因为体积更小。

---

template模版里好像无法访问ts里的this，也就是模版里用到ts里的属性的话，直接写这个属性，前边不要加this。

![](/images/ts-3.png)

---

vue-property-decorator是kaorun343写的一个ts支持库库，这个库比尤雨溪写的库要好，所以用这个库。尤雨溪写的库叫vue-class-component,这是vue官方提供的一个ts的支持库。

其中@component装饰器用的就是vue官方的，所以看用法时看官方@component的用法即可。这个装饰器的作用是告诉vue这是一个类组件。

用ts组件时必须用class类写法。

---

外部属性写法：

```ts
@Prop(Number) xxx: number | undefined;
```
左边的Number指定运行时类型（运行时浏览器报错），右边的指定编译时类型（编译时控制台报错）。

![](/images/ts-4.png)

---

TS本质：![](/images/ts-5.png)

如图所示，谁在做检查呢？——tsc（TypeScript Compiler）在检查；

谁在检查完后将TS变为JS呢？——也是tsc；当然除了tsc，babel也可以将TS变为JS。

检查TS有错误时，也可能妥协继续将TS编译为JS。为什么要妥协呢？举个例子，比如有一大片TS代码，有100个错误，如果不妥协的话只能在改完所有错误的情况下才能看到运行结果，这就让人很沮丧，所以要妥协。妥协后即使有很多错误也能看到运行结果，然后慢慢改，这样更容易让人接受。

当然可以配置，让ts不妥协，配置tsconfig.json文件，但是视频没有成功，这个不重要。

---

写Vue组件的三种方式（单文件组件）

已经写过了博客，这三种方式分别是：

1. 用JS对象；
2. 用TS类；
3. 用JS类；

---

强制指定类型：
```ts
inputContent(e: MouseEvent) {
    //强制指定类型
    //意思就是强制告诉你这是一个按钮
    //如果不指定，代码会提示button可能不存在，
    // 并且会提示EventTarget类型里没有textContent属性：
    const button = e.targetHTMLButtonElement;
    const value = button.textContent!;
    if (this.output.length === 16) {
        return;
    }
    if (this.output === '0') {
        // 如果点击的是0123456789等任一个
        if ('0123456789'.ind(value) > -1) {
            this.output = value;
        } else if (value === '.') {
            this.output = '0' + value;
        }
    } else {
        if (this.output.indexOf('.'-1 && value === '.') {
            return;
        } else {
            this.output += value;
        }
    }
}
```
不得已的情况下才需要强制指定类型，这里是因为Vue和TS的结合不够好。

---

v-model双向数据绑定：
```vue
<template>
    <div class="notes">
        <label>
            <span class="name">备注：</span>
            // 下边的表单元素用到了双向数据绑定：
            <input v-model="value" type="text" placeholder="请输入备注">
        </label>
    </div>
</template>
```

```ts
  import Vue from 'vue';
  import {Component} from'vue-property-decorator';

  @Component
  export default class Notes extends Vue {
      value = '你好吗';
  }
```

v-model的完成写法是：
```html
<input :value='value' @input="value = event.target.value" type="text">
```

---
@watch作用？监听一个属性的变化，当这个属性发生变化时执行一个函数。

@watch用法：

```ts
export default class Notes extends Vue {
    value = '';
    // 监听value属性的变化：
    @Watch('value')
    // value变化时执行一个函数：
    // 注意第一个参数是新的值，第二个参数是旧的值
    onValueChanged(newValue: string,oldValue: string){
        this.$emit('update:notes',newValue)
    }
}
```

---

如何在ts里声明一个类型？
```ts
// 声明一个类型recordItem，里面有四个属性，每个属性的类型声明好：
type recordItem = {
    tags: string[];
    notes: string;
    type: string;
    amount: number;
}
```

---

类型和类不同，类型有7种：4基2空1对象；其中对象又可以划分为类，比如Date类。
```ts
type recordItem = {
    tags: string[];
    notes: string;
    type: string;
    amount: number;
    time?: Date
}
```

---

数据库迁移策略：遇到再研究吧。在localStorage这一部分最后一节。

---

MVC：

1. M-数据层；
2. V-视图层；
3. C-业务逻辑层；

---
ts和js配合的话导入模块要用require，不能用默认导入。而且不要ts和js混用，既然用了ts就都用ts啊！！！

require导入默认导出的模块时需要加default，例如：

导出模块model.js：(使用的是默认导出)
```js
const model = {
    fetch() {
        return JSON.parse(window.localStorage.getItem('recordList') || '[]');
    },
    save(recordList) {
        window.localStorage.setItem('recordList', JSON.stringify(recordList));
    }
};
// 使用的默认导出：
export default model;
```

导入模块Money.vue：（需要加default，否则无法成功使用导入的内容）
```ts
// 需要加default：
const model = require('@/model.js').default;
```

---

本地存储只支持字符串，所以要通过JSON序列化：
```ts
window.localStorage.setItem('recordList', JSON.stringify(this.recordList));
```

---

把所有的类型都放到src目录里的custom.d.ts文件里，这样全局都可以访问到这里的类型（src目录里无需引入就可以使用这些类型）。只要TypeScript发现是以d.ts结尾的，就知道这个文件是类型声明。

当然了，后缀只要以d.ts结尾就可以了，前面的名字可以任意。也可以叫haha.d.ts。

![](/images/ts-6.png)

---

Vue单文件组件必须首字母大写。

---

custom.d.ts里的类型声明都用大写字母开头比较好，大神方的做法就是如此。

---

filter方法返回一个符合条件的新数组：
```ts
// 最后边加[0]表示返回数组的第一项
const tags = tagList.filter(i=>i.id === id)[0]
```

---

如何通过路由跳转到某个页面：
```ts
// 意思就是跳转到路径为404的页面
this.$router.replace('/404')
```

---

.native用法：

子组件：Button.vue
```vue
<template>
    <button class="button">
        <slot/>
    </button>
</template>
```

父组件：Labels.vue
```vue
<template>
    <Layout>
        <div class="tagList">
            <router-link :to="`/labels/edit/${tag.id}`"
                         class="tag"
                         v-for="tag in tagList" :key="tag.index">
                <span>{{tag.name}}</span>
                <Icon name="right"/>
            </router-link>
        </div>
        // 父组件用到了子组件：Button.vue
        // 但是监听点击事件以后不能触发createTag函数，怎么办，加.native就可以啦：
        <Button @click.native="createTag">
            新建标签
        </Button>
    </Layout>
</template>

<script lang="ts">
    import Vue from 'vue';
    import {Component} from 'vue-property-decorator';
    import Layout from '@/components/Layout.vue';
    import Icon from '@/components/Icon.vue';
    import tagListModel from '@/models/tagListModel';
    import Button from '@/components/Button.vue';

    @Component({
        components: {Button, Icon, Layout}
    })
    export default class Labels extends Vue {
        tagList = tagListModel.tagList;

        beforeCreate() {
            tagListModel.fetch();
        }

        //上边用到了.native，这里才可以触发该函数：
        createTag() {
            tagListModel.create();
        }

    }
</script>
```

上面暴漏的问题是：你点击时点击的是子组件里的小button元素，但是点击事件是给了父组件大Button，所以无法触发大Button的点击事件。

解决方法就是通过.native，或者子组件的小button元素里点击时发布一个点击事件，然后父组件大Button监听这个点击事件。

---

ID的原则：
1. 一旦给了id，就不要修改；
2. id不能重复；

---

Vue项目中如何给Window加一个全局属性？

custom.d.ts文件：
```TS
type Tag = {
    id: number;
    name: string;
}

// 这就给window加了一个tagList属性：
interface Window{
    tagList: Tag[]
}
```

然后就可以在main.ts里使用：
```ts
window.tagList = tagListModel.fetch();
```

再然后其他页面就可以使用window.tagList了：(下面是Money.vue组件)
```Vue
export default class Money extends Vue {
    tagList = window.tagList;
}
```

---

如何在webStorm里搜索时排除某个目录：

![](/images/WebStorm-setting-7.png)

---

ts里赋值时，**简单类型保存的是值，引用类型保存的是地址**：

Money.vue组件：
```Vue
<template>
    <Layout class-prefix="layout">
        {{count}}
        // 点击按按钮，逻辑上是count会加1，但是并不会，因为count保存的是简单类型的值，而不是复杂类型的地址： 
        <button @click="add">
            点击我count+1
        </button>
</template>

<script lang="ts">
    import Vue from 'vue';
    import {Component} from 'vue-property-decorator';
    import tagListModel from '@/models/tagListModel';

    @Component({
        components: {FormItem, Layout, NumberPad, Types, Tags}
    })


    export default class Money extends Vue {
        add() {
            tagListModel.add();
        }
        // tagListModel里的count是一个简单类型number（值为0），
        // 所以这个地方count属性保存的是这个值，而不是引    用地址。
        // 也就是说视图里点击button按钮时不会导致count加1，因为count保存的是tagListModel里的0，不是地   址。两者互不影响：
        count = tagListModel.count;
    }
</script>
```

tagListModel.ts文件：
```ts
const tagListModel: TagListModel = {
    count: 0,
    <!-- 这也是一个闭包 -->
    add() {
        this.count++;
    }
}
```

那怎么办？用computed计算属性：

Money.vue组件：
```Vue
<template>
    <Layout class-prefix="layout">
        {{count}}
        <button @click="add">
            点击我count+1
        </button>
</template>

<script lang="ts">
    import Vue from 'vue';
    import {Component} from 'vue-property-decorator';
    import tagListModel from '@/models/tagListModel';

    @Component({
        components: {FormItem, Layout, NumberPad, Types, Tags},
        // 第一个改的地方，把count属性改为计算属性：
        computed: {
            count() {
                return tagListModel.count;
            }
        }

    })


    export default class Money extends Vue {
        add() {
            tagListModel.add();
        }
        // 第二个改的地方，监听tagListModel的变化：
        // 为什么要这样？因为如果不这样tagListModel变化时Vue就监听不到；目的就是告诉我Vue监听tagListModel的变化：
        // 当然也可以把这句代码放到最外面：App.vue组件里：
        tagListModel = tagListModel
    }
</script>
```

把上边的那句代码写到App.vue里：
```vue
// 我试了ts的写法，但是不行，不知道为啥
<script>
    import tagListModel from '@/models/tagListModel';

    export default {
        data() {
            return{
                tagListModel : tagListModel
            }
        }
    };
</script>

```