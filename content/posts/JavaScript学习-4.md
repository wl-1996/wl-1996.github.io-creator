---
title: "JavaScript学习-4"
date: 2019-12-10T12:01:33+08:00
draft: false
---

## 作用域

### 函数能封闭住定义域

一个变量如果定义在了一个 function 里面，那么这个变量就是一个局部变量，只有这个 function 里面有定义。出了这个 function ，就如同没有定义过一样。代码示例：
```javascript
function fn(){
    var a = 1;// 定义在一个函数里面的变量，局部变量，只有在函数里面有定义。
    console.log("我是函数里面的语句，我认识 a 值为 " + a);
}

fn();
console.log("我是函数里面的语句，我不认识 a " + a);// 这行会报错，因为 a 作用于函数内部，这里提取不到 a 的值。
```

a 被var在了 function 里面，所以现在这个 a 变量只有在红框范围内有定义：![](/images/zuoyongyu.png)

---

JavaScript 变量作用域非常的简单，没有块级作用域，管理作用域的只有一个东西：函数。

---

如果一个变量，没有定义在任何的 function 中，那么他将在全部程序范围内都有定义：
```javascript
var a = 1;// 定义在全局范围内的一个变量，全局变量，在程序任何一个角落都有定义：

function fn(){
    console.log("我是函数里面的语句，我认识全局变量 a 值为 " + a);
}
fn();
console.log("函数外面的语句也认识 a 值为 " + a);
```
---

总结一下：

1. 定义在 **function 里面**的变量叫做局部变量；
2. 定义在**全局范围内的**，**没有写在任何 function 里面的**，叫做全局变量。
   
### 作用域链

当遇见一个变量时，JS引擎会从其所在的作用域依次向外层查找，查找会在找到第一个匹配的标识符的时候停止。代码示例：
```javascript
function outer(){
    var a = 1;// a的作用域就是outer
    inner();// 调用inner函数
    function inner(){
        var b = 2;// b的作用域就是inner
        console.log(a);// 输出1，a在本层没有定义，会去上层找a
        console.log(b);// 输出2，b在本层定义了
    }
}

outer();
console.log(a);// 报错，因为a的作用域在outer函数内部
```
---

多层嵌套，如果有同名的变量，那么就会发生“遮蔽效应”：
```javascript
var a = 1;// 这里声明了一个全局变量
function fn(){
    var a = 5;// 这里声明了一个局部变量，把外层的a给遮蔽了，在函数内部看不见外层的a了
    console.log(a);// 输出5，因为变量a在当前作用域已经找到
}
fn();
console.log(a);// 输出1，变量a在当前作用域找（不进到函数fn内部）
```

---

作用域链：一个变量在使用的时候得几呢？就会在当前层去寻找它的定义，如果找不到，就找上一层，直到找到全局变量，如果全局变量也没有，就报错。
```javascript
var a = 1;// 全局变量
var b = 2;// 全局变量
function outer(){
    var a = 3;// 遮蔽了外层的a，这个a是局部变量
    function inner(){
        var b = 4;// 遮蔽了外层的b，这个b是局部变量
        console.log(a);// 输出 3，在上一层找到的
        console.log(b);// 输出4，在本层找到的
    }
    inner();// 调用函数
    console.log(a);// 输出3，在本层找到的
    console.log(b);// 输出2，在上一层找到的
}
outer();// 调用函数
console.log(a);// 输出全局变量 1
console.log(b);// 输出全局变量 2
```

### 不写 var 就自动变成全局变量了

代码示例：
```javascript
function fn(){
    a = 1;// a在第一次赋值的时候，由于没有写 var ，变为了全局变量，相当于在最开始程序自己加了：var a
}

fn();
console.log(a);
```
这是JS的一个机理，如果遇见了一个标识符，从来没有 var 过，并且还赋值了（`abs = 123`）;那么就会自动帮你在全局范围内定义：`var abs`;由此可见，变量要老老实实写 var 。

### 函数的参数，会默认定义为这个函数的局部变量

```javascript
function fn(a,b,c,d){

}
```
a,b,c,d 就是一个 fn 内部的局部变量，出了 fn 就没有定义。

### 全局变量的作用

全局变量有两个功能：

功能1：通信，共同操作同一个变量

下边两个函数同时操作同一个变量，一个增加，一个减少，函数和函数通信。
```javascript
var num = 0;// 全局变量 num

function add(){
    num++;
}
function remove(){
    num--
}
```

---

功能2：累加，重复调用函数的时候，不会重置
```javascript
var num = 0;
function baoshu(){
    num++;
    console.log(num);
}

baoshu();// 输出1
baoshu();// 输出2 
baoshu();// 输出3
```
上边的 num 变量如果定义在函数baoshu里面的话，每次执行该函数就会把 num 重置为 0,也即是输出三次1：
```javascript
function baoshu(){
    var num = 0;
    num++;
    console.log(num);
}

baoshu();// 输出1
baoshu();// 输出1
baoshu();// 输出1
```

### 函数的定义也有作用域

```javascript
// 这个函数返回 a 的平方加 b 的平方
function pingfanghe(a,b){
    return pingfang(a) + pingfang(b);
    // 这个函数返回 m 的平方
    function pingfang(m){
        return Math.pow(m,2);
    }
}
// 现在求 4 的平方，想输出16，但是会报错
pingfang(4);// 会报错，因为全局作用域下，没有一个函数叫做 pingfang
```

公式：
```javascript
function 大{
    function 小{

    }
    小();// 可以运行
}

小();// 不能运行，因为小函数定义在大函数里面，离开大函数没有作用域
```

## 闭包

### 什么是闭包

任何的培训机构，任何的书，讲闭包，一定是下面的案例：
```javascript
function outer(){
    var a = 333;
    function inner(){
        console.log(a);
    }
    return inner;// 调用outer函数时会返回inner函数
}

var inn = outer();// 把inner函数赋值给变量 inn
inn();// 调用 inn 函数，此时就会控制台打印出a的值，为 333
```

---

推导过程：我们之前已经学习过，inner()这个函数不能在outer函数外部调用，因为outer外面没有inner的定义：
```javascript
function outer(){
    var a = 888;
    function inner(){
        console.log(a);
    }
}

inner();// 会报错，因为函数inner的作用域在outer内部
```
但是我们现在就想在全局作用域下，运行outer 内部的inner函数，那么我们就必须想一些别的奇奇怪怪的方法。有一个简单可行的方法，就是让outer自己return inner函数：
```javascript
function outer(){
    var a = 333;
    function inner(){
        console.log(a);
    }
    return inner;// outer 返回了 inner的引用
}

var inn = outer();// 调用outer时会返回inner的引用，即把inner函数赋值给变量 inn
inn();// 执行inn(),即调用了inner函数。此时会打印出a，可以看出全局作用域下并没有声明变量 a。但是没关系，因为函数闭包，能够把定义函数时的作用域一起记住，也就是能够记住 var a = 333;从而能够顺利输出 333.
```
这个例子就说明了，inner 函数能够持久保存自己定义时的所处环境，并且即使自己在其它的环境被调用的时候，依然可以访问自己定义时所处环境的值。

---

一个函数可以把它自己内部的语句，和自己声明时所处的作用域一起封装成一个密闭的环境，我们称为“闭包”（Closures）:![](/images/closures.png)

---

总结：

**每个函数都是闭包，每个函数天生都能够记忆自己定义时所处的作用域环境**。但是，我们必须将这个函数，挪到别的作用域，才能更好的观察闭包。这样才能实验它有没有把作用域给“记住”。 

**我们发现，把一个函数从它定义的那个作用域，挪走，运行。嘿，这个函数居然能够记忆住定义时的那个作用域。不管函数走到哪里，定义时的作用域就带到了哪里。这就是闭包。**

闭包在工作中是一个用来防止产生隐患的事情，而不是加以利用的性质。

因为我们总喜欢在函数定义的环境中运行函数。从来不会把函数往外挪。那为啥学习闭包，防止一些隐患，面试绝对考。

### 闭包的性质

每次重新引用函数的时候，闭包是全新的。
```javascript
function outer(){
    var count = 0;
    function inner(){
        count++;
        console.log(count);
    }
    return inner;
}

var inn1 = outer();
var inn2 = outer();

inn1();// 输出 1
inn1();// 输出 2
inn1();// 输出 3
inn1();// 输出 4
inn2();// 输出 1
inn2();// 输出 2
inn1();// 输出 5
```
无论它在何处被调用，它总是能访问它定义时所处作用域中的全部变量。

## 数组

### 数组概念

数组（array）是一个有序的数据集合，说白了数组就是一组数。
```javascript
var arr = [16,33,23,12,53]
```
变量 arr 就是一个数组变量，里面存储的不是一个数字，而是一组数。可以使用下标或称为索引值 index 来精确访问数组中的某一个项，下标从0开始。代码示例：
```javascript
console.log(arr[0]);// 输出16
console.log(arr[1]);// 输出33
console.log(arr[4]);// 输出53
```

---

数组中并不规定保存相同类型的项，**但实际应用中，我们一般还是将相同类型的项保存在其中。**  例如下面的数组中，存储的内容类型都是不一样的，但是是合法的：
```javascript
var arr = [1,"哈哈",true,undefined,"么么哒"];
```

---

数组有一个属性，就是 length ，英语是长度的意思，表示这个数组的项的个数。

先说说什么是“属性”，数组是对象，对象有属性，属性就是描述这个对象的特点、特性、特征。用点操作符来表示一个对象的属性：
```javascript
var arr = [34,563,4576,334,46,433];
alert(arr.length);// 输出6，因为数组里一共有 6 项
```

---

数组的最后一项的下标是：arr.length - 1 
```javascript
var arr = [1,5,6,7,81,9];
console.log(arr[arr.length - 1]);// 输出 9
```

---

数组的最大下标是 arr.length - 1 ，尝试输出一个大于这个数字的下标会输出什么呢？

答案是：会输出 undefined

---

可以跳跃指定数组：`arr[66] = 88;` 那么此时其他没有指定的项就是 undefined。这个时候 `arr.length;` 的值为67，因为我们刚才设置了下标为 66 的项，强制“拉长”了数组。 

---

写一个小于数组元素数量的长度会缩短数组，写 0 会彻底清空数组。例如我们写 `arr.length = 2;` 的话，这个数组就会只剩下两项： `a[0]` 和 `a[1]` ,其他的项就丢失了。

### 数组的遍历

用 for 循环语句来遍历一个数组，应该这么写：
```javascript
for(var i = 0; i<=arr.length - 1; i++){

}
```

### 数组是一个引用类型

```javascript
var arr = [1,2,3,4];
console.log(typeof arr);// 输出 object
```
用 typeof 来检测数组类型，会输出 object。因为数组是对象

---

保存数组的变量，实际上保存的是数组的内存地址：
```javascript
var a = [1,2,3,4];
var b = a;
b[0] = 88;// 修改的是数组 b 下标为 0 的项
console.log(a);// 打印出a数组后你会发现a数组下标为 0 的项也变为了 88
```
下边的示意图可以很好的解释这个现象：![](/images/array.png)

---

下边两个数组看起来完全一样，但是并不 ==：
```javascript
var a = [1,2,3];
var b = [1,2,3];
console.log(a == b);// 输出 false
```
这两个数组的内容、长度、项的位置完全一样，但是为什么不相等呢？ 这是因为**引用类型**比较的是地址，变量a和变量b指向的位置不一样，不能判相等。

---

如果a里面存储的是基本类型，那么语句b=a就是把a的值赋值一份给b。如果a里面存储的是引用类型，那么b将指向a现在指向的位置，a的值不会赋值一份，a、b指向同一个地方。

### 数组的常见方法

数组是对象，现在你要知道对象都有属性和方法。

属性已经介绍了，数组有length属性。属性就是描述对象的特点的，比如“性别”、“姓名”、“身高”

方法就是对象能够执行的事情。比如“吃饭”、“睡觉”、“打dota”
我们现在就要来学数组能执行什么方法。


数组的常见方法有这几种，如图所示：![](/images/array2.png)

---

**数组的头尾操作 pop()、push()、shift()、unshift()**

push() 方法，在数组的末位添加项目，可以添加 1 个，也可以添加多个。
```javascript
var arr = ["东","西","南","北"];
arr.push("中","发","白");
console.log(arr);
```
输出：![](/images/array-push.png)

---

pop()方法，删除数组的最后一项，只能删除最后一项，无法删除多项。能够返回被删除的元素。

---

1. push 尾插
2. pop 尾删
3. unshift 头插
4. shift 头插
   
---
---

**数组的合并与拆分 concat()、slice()**

concat() 方法就是合并两个数组:
```javascript
var arr1 = ["东","西","南","北"];
var arr2 = ["一条","二条"];

arr1.concat(arr2);// 这里有一个超级大坑，concat是把 arr1 和 arr2 合并为一个新数组返回
console.log(arr1);// arr1 不变
```
因为concat是把 arr1 和 arr2 合并为一个新数组返回，所以必须赋值：
```javascript
arr1 = arr1.concat(arr2);
```
concat() 的参数非常灵活，可以是数组变量、数组字面量、散的值也行：
```javascript
var arr1 = ["东","西","南","北"];
var arr2 = ["一条","二条"];

arr = arr.concat(arr2,["一筒","八条"],"幺鸡");
console.log(arr1);
```
输出：![](/images/concat.png)

---

slice()方法可从已有的数组中返回选定的元素。
```javascript
var arr = ["东","西","南","北","中","发","白"];
var arr2 = arr.slice(1,4); //截取下标为1、2、3的为一个新数组返回
console.log(arr2);   //["西", "南", "北"]
```
arr.slice(start,end) 返回一个新的数组，包含从 start 到 end （不包括 end）的元素。

只有开头：
```javascript
var arr = ["东","西","南","北","中","发","白"];
var arr2 = arr.slice(3); //  从下标为3的项目开始截取后面全部了
console.log(arr2); // ["北", "中", "发", "白"]
```

slice(a,b)取出了b-a项

---
---

**多功能splice()插入、删除、替换**

我们先确认一个事情，`arr.splice(3,2,"斑马","骆驼");`
一旦应用，arr立即改变。并不需要重新复制，换句话说，这个函数不返回新的数组。


```javascript
var arr = ["A","B","C","D","E","F","G"];
arr.splice(3,2,"X","Y","Z","思密达"); //从数组下标为3开始这项，连数2项，改为……
console.log(arr);
```
输出：![](/images/splice.png)

```javascript
// ***************插入一些项 ***************
var arr = ["A","B","C","D","E","F","G"];
arr.splice(2,0,"嘻嘻","哈哈");// 插入到下标为2的项前，不删除项目
console.log(arr);
```
输出：![](/images/splice2.png)

---

splice依据参数的多少，和参数是什么，有多功能。现在你要能反应过来。

删除数组的最后8项:
```javascript
arr.pop();
arr.pop();
arr.pop();
arr.pop();
arr.pop();
arr.pop();
arr.pop();
arr.pop();
```
简化为：
```javascript
for(var i = 1 ; i <= 8 ; i++){
arr.pop();
}
```
也可以：
```javascript
arr.splice(-8)
```

---
---

**逆序reverse();**

reverse()方法就是立即让数组倒置：
```javascript
var arr = ["A","B","C","D","E","F","G"];
arr.reverse(); //不需要赋值
console.log(arr); //["G", "F", "E", "D", "C", "B", "A"]
```

---
---

**排序 sort()**

sort() 方法排序：
```javascript
var arr = ["G","A","C","B","I","H","G","I","B"];
arr.sort();
console.log(arr);
```
输出：![](/images/sort.png)

---

```javascript
//sort函数默认是按照字符顺序排的，隐式将数字转为string
//比字符编码顺序
var arr = [23,435,456,23,2,345,2,32,11,324,32,43,65,667,78,43];
arr.sort();
console.log(arr);
```
输出：![](/images/sort2.png)

可以看出上边的排序输出的结果不是我们想要的，那么如果解决该问题：

sort()里面有一个参数，这个参数是一个函数。
```javascript
arr.sort(function(a,b){
    //如果a要放在b前面，那么返回负数
    //如果a要放在b后面，那么返回正数
    //如果a和b不区分大小，那么返回0
    if(a < b){
	return -1;
    }else if(a > b){
	return 1;
    }else if(a == b){
	return 0;
    }
});
```

---
---

**转为字符串**
```javascript
var arr = [1,2,3,4,5,6,7];
var str = arr.join("★");
console.log(str);
```
输出：![](/images/arr-join.png)

语法：
```javascript
var str = arr.join(分隔符);
```

如果不写分隔符，那么等价于用逗号分开:
```javascript
var arr = [1,2,3,4,5,6,7];
var str = arr.join();
console.log(str);	
```
输出：![](/images/join.png)

## 字符串的常见属性和方法

String 对象属性：![](/images/string.png)

String 对象方法：

![](/images/string2.png) 

![](/images/string3.png)
