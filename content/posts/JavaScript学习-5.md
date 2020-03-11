---
title: "JavaScript学习-5"
date: 2019-12-17T09:29:21+08:00
draft: false
---

## IIFE

IIFE 就是 immediately invoked function expression,即 立即调用函数表达式/即时调用函数表达式。

MDN定义：IIFE（ 立即调用函数表达式）是一个在定义时就会立即执行的  JavaScript 函数。

shaoshanhuan定义：如果一个函数，在定义的时候，我们就想直接调用它，就是一个IIFE。代码示例：
```javascript
(function(){
    alert("哈哈")
} )()
```
IIIFE里面的函数，都是匿名函数。因为用这种方法定义的函数，名字是无效的，不能在其他的地方通过函数名调用这个函数。

## 结合数组观察闭包

数组中，什么都能放。能放 string、能放数字、更能放函数。

下面的例子就是经典的数组与闭包的结合：
```javascript
var arr = []; //空数组

// 用循环语句去填充数组里面的每个项
for(var i = 0; i<=10; i++){
  arr[i] = function(){
    alert(i); //我们在这里，试图每个函数弹出自己的序号
  }
}

// 循环语句执行完毕，arr 里面就有了11个函数了，每个函数都是弹出i的值
arr[6](); //弹出11
arr[9](); //弹出11
arr[10](); //弹出11
```
上边的代码弹出的都是11，而不是预想中的6、9、10。原因就是每个函数定义的时候，都产生闭包，函数就认识 i 了，而不是说把 i 这个值复制一份记忆住，而是动态的、有呼吸、有生命的认识这个 i ，i在调用的时候得几了，这个函数认为 i 就是几。

所以在调用的时候，i 已经变为了11，所以每个函数都弹出11了。

---

解决办法：
```javascript
for(var i = 0; i<=10; i++){
    (function(m){
        arr[m] = function(){
            alert(m)
        }
    })(i)
}
```
每个arr在赋值的时候，都是在一个IIFE里面，IIFE里面是作用域隔离的，所以即使变量名都是 m ，但是视为“不同国家的m”，每个arr里面的小函数，都只能看见自己的m值。m值纵然改变，那也察觉不到

## 冒泡排序法

冒泡排序原理图：![](/images/maopaoSort.png)

现在有n个数字进行排序，那么就需要进行 n-1 趟比较，其中第一趟要进行 n-1 次比较

所以总比较次数就是：（n-1） + （n-2） + （n-3） + ... + 1 次。

比如，6个数字要排序，就要比较 5+4+3+2+1 共15次。

代码如下：
```javascript
let arr = [45,33,12,8,56,34];
//比如有6个数，就要比较 5 趟，下标从0~4就是5趟
for(let i = 0; i<arr.length - 1; i++){
  //每趟内从后向前进行两个数字的比较
  for(let j = arr.length - 1; j > i; j--){
    //如果后面的数字大于前面的数字（56>34），交换数字位置
    if(arr[j] < arr[j - 1]){
      let temp = arr[j - 1]
      arr[j - 1] = arr[j]    
      arr[j] = temp
    }  
  }
}
console.log(arr)
```

## 快速排序

## DOM

### 整体感知

DOM（Document Object Model，文档对象模型）描绘了一个层次化的节点树，允许开发人员添加、移除和修改页面的某一部分。这使得JavaScript操作HTML，不是在操作字符串，而是在操作节点，极大地降低了编程难度。编写例子整体感知一下这个事儿。

### 得到元素 getElementById

我们HTML负责页面的布局、结构。JS要操作HTML标签，第一件事儿就是得到这个标签。

说白了，就是把HTML标签拿到JS里面来。

JS中，最最基本的得到元素的方法有两个：
```javascript
document.getElementById("")
document.getElementsByTagName("")
```

### 更改 HTML 属性

HTML标签有很多属性，比如src、href、title等等。

JS可以更改HTML的任何属性，方法是两种：点语法和`setAttribute()、getAttribute()`

---

得到一个元素之后，直接打点调用它的属性名，就能对HTML相应的属性进行更改：
```javascript
oTutu.src = "images/2.jpg"
oLianjie.href = "https://www.baidu.com"
oKK.value = "嘻嘻嘻嘻嘻"
```
注意id属性不能更改，id是只读的。

注意class这个属性，要换成className，因为class是JS的保留字：
```javascript
oDiv.className = "da"
```

不仅是 class 属性需要用 className 避讳一下，还有：
1. for 要写成 .htmlFor(label用的)
2. rowspan 要写成 rowSpan
3. colspan 要写成 colSpan

---

还有一种方法，可以设置HTML元素的属性，就是：
```javascript
getAttribute(); //得到属性
setAttribute(); //设置属性
```
```javascript
oImg.setAttribute("src","images/2.jpg")
// 等价于：
oImg.src = "images/2.jpg"
```

setAttribute和点语法有一丢丢不一样：

1. 第一，所有自定义的属性，都不能通过点语法得到
   ```javascript
    <div shaoshanhuan = "38" id= "box"></div>
    let oBox = document.getElementById("box")
    alert(oBox.shaoshanhuan) //undefined,自定义的属性不是w3c的属性，不能通过点语法得到
    alert(oBox.getAttributte("shaoshanhuan")) //38
   ```
2. 第二，所有的行内样式，点语法.style得到的是一个样式对象。我们可以通过.style.border继续得到小样式。但是getAttribute()得到的是字符串
   ```javascript
    var oDiv = document.getElementById("box")
    console.log(typeof oDiv.style) //object
    console.log(typeof oDiv.getAttribute("style")) //string
   ```
3. 第三，getAttribute()不需要避讳，直接`oDiv.getAttribute("class")`就行了。而使用点语法需要这么写`oDiv.className("class")`

点语法的效率远高于getAttribute()、setAttribute()。
所以，如果我们是要读取自定义的属性，必须shaoshanhuan属性，偶尔用一次getAttribute，除此之外，都用点语法。

### 操作元素样式

通过点语法.style能够得到所有样式的封装  注意，**只能得到行内样式**，所有写在css内嵌的、外联的，一律不能得到。需要我们后面学习的知识，得到计算后样式。

### 事件监听

JavaScript制作交互效果，离不开事件。**所谓的事件，就是用户的某个行为，能够触发一个函数的执行**

下边介绍一些常用事件：

1. onclick 		    单击
2. onmouseover	    鼠标进入
3. onmouseout		鼠标离开
4. ondblclick		双击
5. onfocus			得到焦点
6. onblur			失去焦点
7. onmousedown		鼠标按下
8. onmouseup		鼠标按键抬起

onload 当页面完全加载成功

window.onload 表示页面中的所有的代码都已经加载完毕了