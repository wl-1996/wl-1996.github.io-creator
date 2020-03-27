---
title: "写Vue组件的三种方式（单文本组件）"
date: 2020-03-27T11:12:08+08:00
draft: false
---

## JS 对象写法

```javaScript
<script>
    export default{
        name: 'Money',
        components: {
            Tags,Notes,Types,NumberPad
        },
        data(){
            return {
                type: '-'
            }
        },
        props:['x'],
        mixins: [xixi],
        methods:{
            add(){
            }
        },
        beforeCreate(){
        },
        created(){
        }
    }
</script>
```

## JS 类写法

```
<script lang=js>
    @Component
    export default class XXX extends Vue{
        xxx = 'hi';
    }
<script>
```

跟 TS 类写法基本一样，只是不用写编译时的类型而已。

## TS 类写法

```ts
<script lang=ts>
    import Vue from 'vue';
    // Component,Prop（外部属性）都是装饰器
    // vue-property-decorator是kaorun343写的一个库，他写的比尤雨溪写得好。
    // 尤雨溪写的叫vue-class-component,这是vue官方提供的ts支持库，官方的不好用，所以我们用kaorun343写的
    import {Component, Prop} from 'vue-property-decorator';

    @Component //告诉ts下边是vue组件，然后下边的东西会被自动处理为data,methods,props等选项
    export default class Types extends Vue{
        // 这个地方不用写类型，ts会自动检测
        type = '-';
        // 冒号左边是指定运行时类型(告诉vue xxx类型是number)，右边指定编译时类型（告诉ts xxx的类型）
        // @Prop告诉vue xxx是外部属性，不是data
        @Prop(Number) xxx: number | undefined;
        selectedType(type: string){
            this.type = type;
        };
        created(){
            console.log(this.xxx)
        }
    }
</script>
```

TypeScript 类写法的好处：

1. 类型提示：更智能的提示
2. 编译时报错：还没运行代码就知道自己写错了
3. 类型检查：无法点出错误的属性

用 TS 类写法时，首先 TS 代码会被 **webpack** 编译成 JS，然后 JS 运行在浏览器上：

![](/images/ts.png)

这样你的 TS 代码就会有两种报错，一种是编译时报错，如果编译失败的话会在终端报错。另一种是运行时报错，也就是在浏览器控制台报错。

TS 的本质就是在 JS 后边加上冒号，冒号后边写上类型，仅此而已：

![](/images/ts-2.png)

这个类型的功能就是来检查 JS 代码的，检查有两种结果：一种编译报错——那么你就根据报错改错误；另一种就是检查后没错误，这时就删掉你写的类型，得到可以在浏览器运行的 JS 代码。

注意有时即使编译报错也会编译成 JS，仍然能运行，这是一种妥协。比如有一百个错误，要是编译不了的话你就只能改完这一百个错误，才能得到在浏览器运行的 JS 代码，这样你就会很沮丧，所以做出这样一种妥协。

tsc（typeScript compiler） 来检查你的 TS 代码是否有错,同样也是 tsc 把 TS 代码编译为 JS 代码。当然除了 tsc，babel 也可以做把 TS 编译成 JS 这件事。
