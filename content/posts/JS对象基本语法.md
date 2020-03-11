---
title: "JS对象基本语法"
date: 2019-10-02T21:47:38+08:00
draft: false
---

# 《JS 对象基本用法》

## 声明对象的两种语法

1. let obj = {'name': 'frank','age':18}
2. let obj = new Object({'name':'frank'})
3. console.log({'name':'frank','age':18})
4. 细节：
   - 键名是字符串，不是标识符，可以包含任意字符；
   - 引号可以省略，省略之后就只能写标识符；
   - 就算引号省略了，键名还是字符串（重要）；
5. 所有的属性名都会自动变成字符串，即使声明对象时不加引号，属性名也会自动变成字符串：
   ```
    let obj = {
        1:'a',
        3.2:'b',
        le2:true,
        le-2:true,
        .234:true,
        0xFF:true
    };
    Object.keys(obj)
    =>["1","100","255","3.2","0.01","0.234"]
   ```
6. 如何用变量做属性名，之前都是用常量做属性名：
   - let p1 = 'name'
   - let obj = {p1:'frank'}这样写，属性名为'p1'
   - let obj = {[p1]:'frank'}这样写，属性名为'name'
   - 不加[]的属性名会自动变成字符串，加了[]则会当作变量求值，值如果不是字符串，则会自动变成字符串
7. 隐藏属性：
   - JS 中每一个对象都有一个隐藏属性
   - 这个隐藏属性储存着其共有属性组成的对象的地址
   - 这个共有属性组成的对象叫做原型
   - 也就是说，隐藏属性储存着原型的地址
8. 隐藏属性代码示例：
   ```
    var obj = {}
   ```
   ```
    obj.toString()//居然不报错，因为obj的隐藏属性对应的对象上有toString()
   ```

## 如何删除对象的属性

1. delete obj.xxx 或 delete obj['xxx']
   - 即可删除 obj 的 xxx 属性
   - 请区分【属性值为 undefined】和【不含属性名】
2. 不含属性名：
   - 'xxx' in obj === false
3. 含有属性名，但是值为 undefined:
   - 'xxx' in obj && obj.xxx === undefined
4. 注意 obj.xxx === undefined
   - 不能断定'xxx'是否为 obj 的属性
5. 类比不含属性名和含有属性名但是值为 undefined
   - 你有没有卫生纸？
   - A：没有//不含属性名
   - B：有，但是没带//含有属性名，但是值为 undefined
6. 程序员就是这么严谨，没有就是没有，undefined 就是 undefined，绝不含糊

## 如何查看对象的属性

1. 查看自身所有属性：
   - Object.keys(obj)
2. 查看自身+共有属性
   - console.dir(obj)
   - 或者自己依次用 Object.keys 打印出
   ```
   obj.__proto__
   ```
3. 判断一个属性是自身的还是共有的：
   - obj.hasOwnProperty('toString')
4. 每个对象都有原型：
   - 原型里存着对象的共有属性
   - 比如 obj 的原型就是一个对象
   - obj.**proto**存着这个对象的地址：
   ```
   obj.__proto__
   ```
5. 对象的原型也是对象：
   - 所以对象的原型也有原型
   - obj = {}的原型即为所有对象的原型
   - 这个原型包含所有对象的共有属性，是对象的根
   - 这个原型也有原型，是 null
6. 查看属性：
   - 两种方法查看属性：
     1. 中间号语法：obj['key']
     2. 点语法：obj.key
     3. 坑新人语法：obj[key]//变量 key 值一般不为'key'
7. 请优先使用中括号语法：
   - 点语法会误导你，让你以为 key 不是字符串
   - 等你确定不会弄混两种语法，再改用点语法
8. obj.name 等价于 obj['name'];obj.name 不等价于 obj[name]
9. let name = 'frank' 。obj[name]等价于 obj['frank'],而不是 obj['name']
10. 考题：
    ```
    let list = ['name','age','gender']
    let person = {
        name:'frank',age:18,gender:'man'}
        for(let i = 0;i < list.length;i++){
            let name = list[i]
            console.log(person__???__)
        }
    }
    ```
    选项：
    1. cosole.log(person.name)
    2. console.log(person[name])
11. 区分 name 和'name'为什么这么重要？
    - 因为如果你现在不搞清楚，那么你在学 Vue 的时候，会更加迷惑

## 如何修改或增加对象的属性

1. 直接赋值：
   - let obj = {name:'frank'}//name 是字符串
   - obj.name = 'frank' //name 是字符串
   - obj['name'] = 'frank
   - obj[name] = 'frank' //错，因 name 是一个变量，name 的值不确定
   - obj['na'+'me'] = 'frank'
   - let key = 'name';obj[key]='frank'
   - let key = 'name';obj.key = 'frank' //错,因为 obj.key 等价于 obj['key']
2. 批量赋值：
   - Object.assign(obj,{age:18,gender:'man'})
3. 修改或增加共有属性：
   1. 无法通过自身修改或增加共有属性：
      - let obj = {},obj2 = {} //共有 toString
      - obj.toString = 'xxx'只会更改 obj 自身属性
      - obj2.toString 还是在原型上
   2. 我偏要修改或增加原型上的属性：
      - obj.\_\_proto\_\_.toString = 'xxx' //不推荐用\_\_proto\_\_
      - Object.prototype.toString = 'xxx'
      - 一般来说，不要修改原型，会引起很多问题
4. 修改隐藏属性：
   1. 不推荐使用\_\_proto\_\_
      - let obj = {name:'frank'}
      - let obj2 = {name:'jack'}
      - let common = {kind:'human'}
      - obj.\_\_proto\_\_ = common
      - obj2.\_\_proto\_\_ = common
   2. 推荐使用 Object.create
      - let obj = Object.create(common)
      - obj.name = 'frank'
      * let obj2 = Object.create(common)
      * obj2.name = 'jack'
      * 规范大概意思是，要改一开始就改，别后来再改

## 总结：

1. 删：
   - delete obj['name']
   - 'name' in obj //false
   - obj.hasOwnProperty('name') //false
2. 查：
   - Object.keys(obj)
   - console.dir(obj)
   - obj['name']
   - obj.name // 记住这里的 name 是字符串
   - obj[name] // 记住这里的 name 是变量
3. 改：
   - 改自身 obj['name'] = 'jack'
   - 批量改自身 Object.assign(obj,{age:18,...})
   - 改共有属性 obj.\_\_proto\_\_['toString'] = 'xxx'
   * 改共有属性 Object.prototype['toString'] = 'xxx'
   * 改原型 obj.\_\_proto\_\_=common
   * 改原型 let obj = Object.cerate(common)
   * 注：所有\_\_proto\_\_代码都是强烈不推荐写的
4. 增：
   - 基本同上：已有属性则改；没有属性则增

## 'name' in obj 和 obj.hasOwnProperty('name')的区别

'name' in obj 判断 'name' 是否是 obj 的属性时会先查找 obj 本身是否有，如果本身没有，就会去 obj 的原型上去找 'name' 属性；

而 obj.hasOwnProperty('name') 则只会判断 obj 本身是否有 'name' 属性，而不会去 obj 的原型上去查找 'name' 属性。
