---
title: "JavaScript学习-2"
date: 2019-12-06T15:46:06+08:00
draft: false
---

# js

## 运算符

1. 种类

   - 数学运算符
     1. +
     2. -
     3. \*
     4. /
     5. %
   - 比较运算符
     1. >
     2. > =
     3. <
     4. <=
     5. == 等于
     6. != 不等于
     7. === 全等于
     8. !== 不全等于
   - 逻辑运算符:注意有短路逻辑
     1. 逻辑非 !
     2. 逻辑与 &&
     3. 逻辑或 ||
   - 赋值运算符
     1. =
     2. +=
     3. -=
     4. \*=
     5. /=
     6. %=
     7. ++
     8. --

2. 运算符优先顺序

   **++ -\- ！贴身的** →→→ **数学** →→→ **比较** →→→ **逻辑** →→→ **赋值**

3. 注意逻辑 且与或 的**短路逻辑**

4. 运算符的复习，快速判断输出结果
   ```javascript
   //数学运算符
   1 + ((2 * 6) % 3); //1
   false + true * null; //0,true是1 false是0 null是0
   6 + undefined; //NaN 注意undefined是不能在数学运算中转的
   0 / 0; //NaN
   6 / 0; //Infinity
   Infinity - Infinity; //NaN
   ```
   ```javascript
   //关系运算符
   5 == "5"; //true
   5 === "5"; //false
   5 != "5"; //false
   5 !== "5"; //true
   "66" < "8"; //true 两个字符串比较的是编码顺序
   66 < "8"; //false 因为有一个数字，所以字符串8也会隐式转换为数字8
   "66" < 8; //false 理由同上
   false == 0; //true
   true == 1; //true
   NaN == NaN; //false
   NaN != NaN; //true
   null == 0; //false ,因为null是一个对象，引用类型值，和基本类型值比，这个你着重记忆一下。
   3 > 2 > 1; //false ,因为是从做到右计算的
   ```
   ```javascript
   //注意逻辑运算符的短路逻辑
   false || (true && !!!false); //true
   false && 8; //false
   3 && 4; //4
   "" && 6; //""
   6 && undefined; //undefined
   null && undefined; //null
   3 || 4; //3
   "" || 18; //18
   ```

## 条件分支语句

### if...else...语句

条件分支的主力语句，这个主力英语能够书写**所有的**条件分支语句

标准写法：

```javascript
if(){

}else if(){

}else if(){

}else{

}
```

### switch case 语句

存在的意义：在某种情况下简化`if...else...`的写法

标准语法：

```javascript
switch(待检测值){
   case 值1 :
      值1 与 待检测值 相同时做的事情；
      break;
   case 值2 :
      值2 与 待检测值 相同时做的事情；
      break;
   case 值3 :
      值3 与 待检测值 相同时做的事情；
      break;
   default :
      默认要做的事情；
      break;
}
```

### 三元运算符（即问号冒号表达式）

? : 是一组运算符，这是 JS 中唯一一个需要三个元素参加的运算符。

语法：

```javascript
条件 ？ val1 : val2
```

表达式的值看**条件**是 true 还是 false 。如果**条件**是 true,那么表达式的值就是 val1 ,如果**条件**是 false,那么表达式的值就是 val2。

## 循环语句

JS 中**流程控制**的语句就两个：条**件分支、循环语句**。靠这两种语句就能完成所有的程序。

### for 循环

语法：

```javascript
for (; i < 一个数; ) {
  执行体;
}
alert("我是for循环后边的语句");
```

for 循环的**本质**：![](/images/for.png)

**注意：**

1. for 循环的顺序非常重要，不懂的话仔细看看上边的图片
2. for 循环和 if 可以嵌套

## 穷举法
