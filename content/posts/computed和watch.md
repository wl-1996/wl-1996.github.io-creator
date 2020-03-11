---
title: "Computed和watch"
date: 2020-01-02T11:01:18+08:00
draft: false
---

## computed

被计算出来的属性就是计算属性

### 例 1-用户名展示：

```javascript
// 引入完整版的vue,方便讲解
import Vue from "vue/dist/vue.js";

Vue.config.productionTip = false;

new Vue({
  data: {
    user: {
      email: "741341649@qq.com",
      nickname: "wangxiaokuo",
      phone: "18790342745"
    }
  },
  computed: {
    displayName: {
      get() {
        const user = this.user;
        return user.nickname || user.email || user.phone;
      },
      set(value) {
        this.user.nickname = value;
      }
    }
  },
  template: `
    <div>
      {{displayName}} 
      <div>
        {{displayName}} 
        <button @click="displayName='lili'" >点我设置新的名字</button>  
      </div>
    </div>
  `
}).$mount("#app");
```

页面：![](/images/Vue-4.png)

---

### 例 2-列表展示：

```javascript
import Vue from "vue/dist/vue.js";

Vue.config.productionTip = false;

let id = 0;

const createUser = (name, gender) => {
  id += 1;
  return { id: id, name: name, gender: gender };
};

new Vue({
  data() {
    return {
      users: [
        createUser("方方", "男"),
        createUser("圆圆", "女"),
        createUser("小新", "男"),
        createUser("小葵", "女")
      ],
      gender: ""
    };
  },
  computed: {
    displayUsers() {
      const hash = {
        male: "男",
        female: "女"
      };
      const { users, gender } = this;
      if (gender === "") {
        return users;
      } else if (typeof gender === "string") {
        return users.filter(u => u.gender === hash[gender]);
      } else {
        throw new Error("gender 的值是不符合要求的");
      }
    }
  },
  template: `
    <div>
      <div>
        <button @click="gender = '' ">全部</button>
        <button @click="gender = 'male' ">男</button>
        <button @click='gender = "female" '>女</button>
      </div>
      <ul>
        <li v-for="(u,index) in displayUsers" :key="index">
          {{u.name}} - {{u.gender}}
        </li>
      </ul>
    </div>
  `
}).$mount("#app");
```

页面：![](/images/Vue-5.png)

### 缓存：

如果 computed 依赖的属性没有变化，就不会重新计算

getter/setter 默认不会做缓存，Vue 做了特殊处理

## watch

当数据变化时，执行一个函数

### 例 1-撤销：

```javascript
import Vue from "vue/dist/vue.js";

Vue.config.productionTip = false;

new Vue({
  data: {
    n: 0,
    history: [],
    inUndoMode: false
  },
  watch: {
    //监听n的变化
    n(newValue, oldValue) {
      // 当撤销模式关闭时，n的值每变化一次，就给history空数组添加一个对象
      if (this.inUndoMode === false) {
        this.history.push({ from: oldValue, to: newValue });
      } else {
        // 当撤销模式开启时，就什么都不做
        return;
      }
    }
  },
  template: `
    <div>
      {{n}}
      <hr />
      <button @click="add1">+1</button>
      <button @click="add2">+2</button>
      <button @click="minus1">-1</button>
      <button @click="minus2">-2</button>
      <hr>
      <button @click="undo">撤销</button>
      <hr>
      {{history}}
    </div>
  `,
  methods: {
    add1() {
      this.n += 1;
      console.log("执行+1");
    },
    add2() {
      this.n += 2;
    },
    minus1() {
      this.n -= 1;
    },
    minus2() {
      this.n -= 2;
    },
    undo() {
      const last = this.history.pop();
      console.log(last);
      this.inUndoMode = true;
      this.n = last.from; //watch是异步的
      this.$nextTick(() => {
        this.inUndoMode = false;
      }, 0); //这也是一个异步，晚于watch，就是说watch执行完后才会执行这句
    }
  }
}).$mount("#app");
```

页面：![](/images/Vue-6.png)

### 例 2-模拟 computed：

```javascript
import Vue from "vue/dist/vue.js";

Vue.config.productionTip = false;

new Vue({
  data: {
    user: {
      email: "741341649@qq.com",
      nickname: "wangxiaokuo",
      phone: "18790342745"
    },
    displayName: ""
  },
  watch: {
    "user.email": {
      handler() {
        //解构赋值：
        const {
          user: { email, nickname, phone }
        } = this;
        this.displayName = nickname || email || phone;
      },
      // immediat:true 的作用是一开始就执行上边的handler函数，即使此时user.email还没有变化
      immediate: true
    },
    "user.nickname": {
      handler() {
        //解构赋值：
        const {
          user: { email, nickname, phone }
        } = this;
        this.displayName = nickname || email || phone;
      },
      immediate: true
    },
    "user.phone": {
      handler() {
        //解构赋值：
        const {
          user: { email, nickname, phone }
        } = this;
        this.displayName = nickname || email || phone;
      },
      immediate: true
    }
  },
  template: `
    <div>
      {{displayName}}
      <button @click="user.nickname=undefined">remove nickname</button>
    </div>
  `,
  methods: {
    changed() {
      console.log(arguments);
      const user = this.user;
      this.displayName = user.nickname || user.email || user.phone;
    }
  }
}).$mount("#app");
```

页面：![](/images/Vue-7.png)

### 什么是变化？

```javascript
import Vue from "vue/dist/vue.js";

Vue.config.productionTip = false;

new Vue({
  data: {
    n: 0,
    obj: {
      a: "a"
    }
  },
  template: `
  <div>
    <button @click="n+=1">n+=1</button>
    <button @click="obj.a+='hi'">obj.a+="hi"</button>
    <button @click="obj = {a:'a'}">obj = {a:'a'}</button>
  </div>
  `,
  watch: {
    n() {
      console.log("n变了");
    },
    "obj.a"() {
      console.log("obj.a变了");
    },
    obj() {
      console.log("obj变了");
    }
  }
}).$mount("#app");
```

页面：![](/images/Vue-8.png)

obj 原本是`{a:'a'}` ,第三个按钮使得 `obj={a:'a'}`：

obj 变了没有？ 变了，因为 obj 的地址变了

obj.a 变了没有？ 没有变，因为 obj.a 的值没变，还是 `'a'`

简单类型看值，复杂类型看地址

### `deep:true` 属性

`deep:true` 意思就是如果监听的是一个对象，那么就深入 obj 对象里面去监听，obj 对象里面任何变化都会被监测，并且视为 obj 发生了变化

```javascript
import Vue from "vue/dist/vue.js";

Vue.config.productionTip = false;

new Vue({
  data: {
    n: 0,
    obj: {
      a: "a"
    }
  },
  template: `
  <div>
    <button @click="n+=1">n+=1</button>
    <button @click="obj.a+='hi'">obj.a+="hi"</button>
    <button @click="obj = {a:'a'}">obj = {a:'a'}</button>
  </div>
  `,
  watch: {
    n() {
      console.log("n变了");
    },
    obj: {
      handler() {
        console.log("obj变了");
      },
      //deep:true 意思就是深入obj里面去监听，obj对象里面任何变化都会被监测,并且视为obj的变化，执行 handler
      deep: true
    }
  }
}).$mount("#app");
```

页面：![](/images/Vue-9.png)

上边的代码，`obj.a` 变了，那么 obj 也变了。deep 的意思是，监听 obj 的时候是否往深了看。

### watch 的语法

语法一（推荐看文档）：

```javascript
watch:{
  //不要用这种箭头函数，因为这里的 this 是全局对象
  ol:()=>{},
  // 这两个参数是Vue传给你的
  o2:function(value,oldValue){},
  o3(){},//es6新语法：缩写
  o4:[f1,f2],//o4变化时f1，f2依次执行
  //o5变化时会调用 methods 里面的 methodName 函数
  o5:'methodName',
  // deep:true 表示该回调会在对象o6的属性被改变时调用，不论其被嵌套多深
  // immediate:true 表示该回调会在监听开始之后立即被调用
  o6:{handler:fn,deep:true,immediate:true},
  'object.a':function(){}
}
```

注意：watch 里面不要用箭头函数，因为无法指定 this

---

语法二：

```javascript
vm.$watch('xxx',fn,{deep:...,immediate:...})
```

代码示例：

```javascript
import Vue from "vue/dist/vue.js";

Vue.config.productionTip = false;

let vm = new Vue({
  data: {
    n: 0,
    obj: {
      a: "a"
    }
  },
  template: `
  <div>
    <button @click="n+=1">n+=1</button>
    <button @click="obj.a+='hi'">obj.a+="hi"</button>
    <button @click="obj = {a:'a'}">obj = {a:'a'}</button>
  </div>
  `,
  watch: {
    obj: {
      handler() {
        console.log("obj变了");
      },
      //deep:true 意思就是深入obj里面去监听，obj对象里面任何变化都会被监测
      deep: true
    }
  }
}).$mount("#app");

vm.$watch(
  "n",
  function() {
    console.log("n变了");
  },
  { immediate: true }
);
```

## computed 和 watch 的区别

computed 就是计算属性的意思，watch 就是监听的意思

---

computed 是用来计算出一个值，这个值：

1.调用的时候不需要加括号；

2.根据依赖是否变化来缓存；

---

watch 是用来监听的，如果某个属性变化了，就去执行一个函数。watch 有两个选项：

1.immediate 表示是否在第一次渲染的时候就执行监听里的函数;

2.deep 表示如果我们监听一个对象，那我们是否要看该对象里面的属性的变化；`deep:true`表示看里面的属性的变化，`deep:false`表示不看里面的 属性的变化。
