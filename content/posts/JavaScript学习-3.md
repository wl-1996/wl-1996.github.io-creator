---
title: "JavaScript学习-3"
date: 2019-12-09T09:24:23+08:00
draft: false
---

# 循环语句后续，函数等知识

## for 循环里的 break语句

break语句的用途就是：我已经找到我要的答案了，我不需要进行更多的循环了的时候用。代码示例如下：

```javascript
for(var i = 1; i<=10; i++){
    console.log(i);
    if(i == 5){
        break; //找到5，然后终止for循环。
    }
}
```
break语句只能中断最内层循环，外层循环还在继续，代码示例：
```javascript
for(var i = 1; i<=10; i++){
    for(var j = 1; j<=10; j++){
        console.log(i,j)
        if(j == 5){
            break; //当 j == 5 的时候，终止内层for循环，外层继续。
        }
    }
}
```
如果你就想用 break 终止外层的 for 循环，那么要给外层for循环加标签，代码示例：
```javascript
waiceng : for(var i = 1; i<=10; i++){
    for(var j = 1; j<=10; j++){
        console.log(i,j)
        if(j == 5){
            break waiceng; //此时中断的就是外层的for循环
        }
    }
}
```
上边的语法虽然有，但是工作中很少用到，非常偏门，了解即可。

## for 循环里的 continue 语句

continue 语句的用途就是：这个答案不是我想要的，赶紧试试下一个数字吧(遇见continue语句，for会立即终止执行continue后面的语句，然后立刻进行下一次迭代)。代码示例如下：
```javascript
for(var i = 1; i<=100; i++){
    if(i%5 == 0){
        continue;// 如果 i 能被5整除，那么就忽略下边的`console.log(i)`语句，继续下一个i的值。
    }
    console.log(i);
}
```
同样的，continue只能中断当前最内层的for循环，外层for要加label。

break 和 continue 整体的目的：优化算法。

用 continue 语句来寻找2-100(1不是质数)之内的所有质数：
```javascript
// 原来的写法：
for(var i = 2; i<=100; i++){
    var count = 0;
    for(var j = 1; j<=i; j++ ){
        if(i%j == 0){
            count+=1;
        }
    }
    if(count == 2){
        console.log(i);
    }
}
```
```javascript
// 用continue后的写法：
waiceng : for(var i = 2; i<=100; i++){
    // 寻找2-100之间的所有质数，注意看与上边的不同，其中j从2开始，j<i（这样就排除了 1 和 i本身两个数字）;
    for(var j =2; j<i; j++){
        // 假如i为7，那么j的范围是2~6，也就是说在2~6之间，如果某个数j被i除尽了，说明这个数j就是i的约数，即i不是质数，此时用continue终止外层循环，进行下一次遍历
        if(i%j == 0){
            continue waiceng;
        }
    }
    console.log(i);// 上边的continue执行的话，就会忽略这个输出；上边的continue不执行的话才会有这个输出
}
```
上边的continue写法有一个地方还可以优化：
```javascript
waiceng : for(var i = 2; i<=100; i++){
    for(var j =2; j<=Math.sqrt(i); j++){// 可以看到j<=Math.sqrt(i),这个自己体会吧
        if(i%j == 0){
            continue waiceng;
        }
    }
    console.log(i);
}
```

## do...while...语句

## while 语句

踩地雷游戏：
```javascript
// 随机生成一个地雷数字，范围是0~100，左闭右开
var dilei = parseInt(Math.random()*100)
// 声明上限和下限：
var xiaxian = 0
var shangxian = 100
while(true){
    var num = parseInt(prompt("请输入一个数字,范围是 " + xiaxian + "到" + shangxian))
    if(num > xiaxian && num <shangxian){
        // 根据用户输入的数字与地雷数的关系来调整上下限：
        if(num<dilei){
            xiaxian = num
        }else if(num>dilei){
            shangxian = num
        }else{
            alert("恭喜你，踩到地雷了")
            break
        }
    }else{
        alert("您输入的数字不在范围内，请重新输入")
    }
}
```

## 随机数

`Math.random()`能够随机生成一个 0~1 的数字，包括0，不包括1。（但是shaoshanhuan说也不包括0），和`Math.pow();Math.sqrt()`一样都是**Math对象**的方法。

**公式：**如果想要在[a,b]闭区间取随机整数，能取到a，也能取到b的话用下边的公式：
```javascript
parseInt(Math.random()*(b - a + 1)) + a;
```

生成一个0-100的随机数：
```javascript
// Math.random()生成一个 0~1 的数字，左闭右开（即包括0，不包括1）
var randomNum = parseInt(Math.random()*100)
console.log(randomNum)
```

## 函数

### 函数定义和调用
语法：
```javascript
// 函数定义
function 函数名(){

}
// 调用函数
函数名();
```
能够感觉到，函数是一些语句的集合。函数里的语句要么不出动，要么全出动。调用函数的时候才会出动。

在出现大量程序相同的时候，可以封装成一个function，这样只需要调用一次函数，就能执行函数里的很多语句。

我们在调用一个函数的时候，不用关心函数内部的实现细节，甚至这个函数是你上网抄的，可以运用。所以这个东西给我们团队开发带来了好处。

### 函数的参数

```javascript
function sum(a,b){
    return a + b;
}
sum(3,8);
```

### 函数的返回值
 
函数可以通过**参数**来接收东西，更可以通过 **return** 的语句来吐出东西。

```javascript
function sum(a,b){
    return a+b;// 现在这个函数的返回值就是 a + b 的和
}
sum(3,8);// 此时调用了sum函数，但是不会在控制台输出 8 ，因为并没有一个输出语句
console.log(sum(3,8));// 这个语句才会在控制台输出 8。
// 这样理解：这个时候sum(3,8)是一个表达式，需要计算。
// 表达式的值是11，也就是说`console.log(11)`
```
函数有一个 return 的值，那么现在这个函数实际上就是一个**表达式**，换句话说这个函数就是一个值。**所以这个函数可以当作其他函数的参数**。代码示例：
```javascript
function sum(a,b){
    return a+b;// 现在这个函数的返回值就是 a + b 的和
}
console.log(sum(3,sum(4,5)));// sum(4,5)是一个参数，这个参数的值为9，也就是说sum(4,5)这个函数调用当作了另一次sum调用函数的参数。
```


函数只能有**唯一的** return，有if语句除外，程序**遇见了 return ，将立即返回结果，而不执行函数内剩余的程序**。代码示例：
```javascript
function fn(){
    console.log(1);
    console.log(2);
    return;// 函数返回一个空值，实际就是返回 undefined
    console.log(3);// 这行语句不执行，因为函数已经 return 了
}
fn(); // 此时只在控制台输出 1，2，不输出3。
```

### 利用函数简化编程

声明一个函数，传入一个数字，返回这个数字的约数个数：
```javascript
function yueshugeshu(n){
    var count = 0;
    for(var i = 1; i<=n ; i++){
        if(n%i == 0){
            count++;
        }
    }
    return count;// 返回这个数字的约数的个数
}
```

再声明一个函数，传入一个数字，判断这个数字是否是质数（用到了上边的yueshugeshu函数）：
```javascript
function isZhishu(m){
    if(yueshugeshu(m) == 2){// 如果 m 的约数个数是2，则返回 true
        return true;
    }else{
        return false;
    }
}
```

for 循环遍历1~100，找出所有的质数(用到了上边的两个函数)：
```
for(var i = 1; i<=100; i++){
    if(isZhishu(i)){
        console.log(i);
    }
}
```

哥德巴赫猜想：任何一个偶数，都可以拆分为两个质数的和(用到了上边的两个函数)
```javascript
var oushu = parseInt(prompt("请输入一个偶数"));
for(var i = 1; i < oushu; i++){
    var j = oushu - i;
    if(isZhishu(i) && isZhishu(j)){
      console.log(oushu + " 可以拆分为 " + i + " 与 " + j + "  的和")
    }
}
```

遍历4~1000000之间的偶数，如果这个数能表示为两个质数的和，就输出这两个质数：
```javascript
function yueshugeshu(n){
    var count = 0;
    for(var i = 1; i<=n ; i++){
        if(n%i == 0){
            count++;
        }
    }
    return count;// 返回这个数字的约数的个数
}

function isZhishu(m){
    if(yueshugeshu(m) == 2){// 如果 m 的约数个数是2，则返回 true
        return true;
    }else{
        return false;
    }
}

waiceng : for(var i = 4;i<=1000000; i+=2){
    for(var j = 2; j < i; j++){
        var k = i - j;
        if(isZhishu(j) && isZhishu(k)){
            console.log(i + " 可以拆分为 " + j + "与" + k + " 的和");
            continue waiceng;// 如果拆成功了（也就是if里的语句执行了），那么continue就会忽略下边的continue语句，继续下一个i拆分；
        }
    }
    alert("哥德巴赫猜想失败");// 如果拆失败了，也即是if为false，这个时候if里的continue不会执行，那么这个alert语句就可以执行。
}
```
通过上边的哥德巴赫猜想可以看出：高层的业务，能使用底层的函数提供的API：

**约数个数函数 → 判断质数函数 → 高层业务**

小例子：计算1! + 2! + 3! + ... +10! 的值
```javascript
// 我自己写的这两个函数和调用，比老师讲的高级（老师第二个没封装函数，直接for循环了）

// 先声明阶乘函数，接受一个参数，返回这个数的阶乘： 
function jiecheng(n){
    var chengji = 1;
    for(var i = 1; i<=n; i++){
        chengji *= i;
    }
    return chengji;
}


function qiuhe(n){
    var sum = 0;
    for(var i = 1; i<=n ; i++){
        sum = sum + jiecheng(i);
    }
    return sum;
}

qiuhe(10);
```

考拉数计算：一个三位数，如果这个三位数的各位，十位，百位的阶乘加起来等于这个数本身，那么这个数就是考拉数。请找到100~999之间的一个考拉数：
```javascript
function jiecheng(n){
    var chengji = 1;
    for(var i = 1; i<=n; i++){
        chengji *= i;
    }
    return chengji;
}

for(var i = 100; i <= 999; i++){
    var gewei = i % 10;
    var shiwei = parseInt(i/10)%10;
    var baiwei = parseInt(i/100);
    if(jiecheng(gewei) + jiecheng(shiwei) + jiecheng(baiwei) == i){
        console.log(i);
    }
}

```

### 递归（函数自己调用自己就叫做递归）

递归代码示例：
```javascript
function haha(){
    console.log("哈哈");
    haha();// 函数自己调用了自己
}

haha();
``` 

斐波那契数列就是经典的递归算法：

1、1、2、3、5、8、13、21、34、55、89、144、233...

下边用编程实现输出斐波纳挈数列：
```javascript
// 下边的代码会让浏览器崩溃，因为fib()函数会不断的递归调用自己，因为下边的for循环计算量太大了：
function fib(n){
    if(n == 1 || n == 2){
        return 1;
    }else{
        return fib(n -1) + fib(n - 2);
    }
}

for(var i = 1; i <= 50; i++){
    console.log(fib(i));
}
```

### 函数表达式

定义函数除了使用 function 之外，还有一种方法就是函数表达式。就是函数没有名字，成为“**匿名函数**”，为了今后能调用它，我们把这个**匿名函数**，直接赋值给一个变量。

代码示例：
```javascript
// 把匿名函数赋值给一个变量
var haha = function(a,b){
    return a + b;
}

// 调用的时候，可以直接使用 haha 变量来调用
console.log(haha(1,3));
```

### 函数声明的提升

JS在执行前，会有一个预解析的过程，把所有的函数声明，都提升到了最最开头，然后再执行第一行语句。代码示例：

```javascript
// 在声明之前先调用函数
fn();

// 然后再定义函数：
function fn(){
    alert("我是函数，我执行了！");
}
```
上边的代码是先使用，后声明的，但是不会报错，因为函数声明提升到了最开头。

---

函数声明能提升，但是**函数表达式不能提升**：
```javascript
// 先调用
haha();

// 再声明
var haha = function(){
    alert("我是函数，我执行了！");
}
```
**上边的代码会报错，因为函数表达式不能提升。**

---

上边的例子告诉我们，没有极特殊的理由，都要使用`function fn(){}`来定义函数，而不要使用函数表达式：`var haha = function(){}`

---

函数优先：如果同一个标识符，在程序中又是变量的名字，又是函数的名字，解析器会把标识符**给函数**！

```javascript
aaa();// 这个 aaa 是函数

var aaa = 5;// 变量声明会提升（注意提升的是变量声明，变量的值不能提升），但是函数提升优先
function aaa(){
    alert("我是aaa函数，我执行了！");// 函数提升比变量声明提升优先
}
```

函数优先的另一个例子-这两个例子对照起来让我觉得不好理解（这是shaoshanhuan第五天讲的，我补到这里了）
```javascript
var a = 1;
function a(){
    alert("我是函数");
}

a();// 此时会报错，系统认为 a 是一个变量
```
在执行 `var a = 1` 之前，函数已经把 `function a()`预解析了，程序就知道页面上有一个函数叫做 a 。但是开始执行程序之后，定义了一个变量 a ,所以标识符 a 就又变成变量了。接下来遇见 function 定义，程序会无视，因为已经预解析了。直到 a() 运行的时候，a 就是变量，无法运行，报错。

### 函数是一个引用类型

我们之前说的，基本类型：number string boolean undefined null。引用类型也有很多种：object function array RegExp Math Date。

```javascript
function fn(){

}
console.log(typeof fn);// 输出 function
```
函数也是一种类型，这个类型就叫做 function，是引用类型的一种。

---

**基本类型保存值，引用类型保存地址。**

```javascript
var a = 1;
var b = a;// b得到的值，是a的副本。a把自己的值赋值了一份，给了b
b = 3; // 更改了b的值，a不受影响
console.log(a);
```
我们现在变量 a = 6; 那么这个 a 变量里面存储的是 6 这个数值。而 a = function(){} 那么这个 a 标签里面存储的是 function 的内存地址。代码示例：
```javascript
// 定义了一个变量 a ，引用了一个匿名函数。这个 a 变量里存储的是这个匿名函数的内存地址：
var a = function(){
    alert("我是一个函数，我执行了");
}
var b = a;// 把匿名函数的地址也给了 b

b.haha = 1;// 给 b 添加一个属性
console.log(a.haha);// 输出1，你会发现和 b 的哈哈属性值相同
b.haha++;
b.haha++;
b.haha++;
console.log(a.haha); //输出4，b一变，a也跟着变；因为 a 和 b 都是指向的同一个对象
```

---