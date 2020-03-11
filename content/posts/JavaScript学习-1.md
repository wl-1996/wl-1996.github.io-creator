---
title: "JavaScript学习-1"
date: 2019-12-03T09:33:03+08:00
draft: false
---

## JavaScript简介

1. 前端三层
   * 结构层 HTML 从语义的角度描述页面的结构
   * 样式层 CSS  从审美角度装饰页面
   * 行为层 JavaScript 从交互的角度提升用户体验

2. 发明者
   * 1995年网景公司(Netscape)的工程师Brendan Eich创造了JavaScript

    ![JS之父](/images/Brendan Eich.png)

3. 历史版本
   * 1997年诞生ECMAScript第一版
   * 1999年更新诞生了ECMAScript第三版
   * 由于委员会内部分歧，第四版流产  
   * 2009年12月发布ECMAScript第五版
   * 2015年6月发布ECMAScript第六版

4. JavaScript从丑小鸭到白天鹅的发展历程
   * 2003年：牛皮鲜，页面上漂浮的广告、弹窗广告；所以当时的浏览器就推出一个功能，禁用广告，实际上本质就是禁用JavaScript。页面上的特效，都特别俗，比如鼠标后面跟随的星星，然后工程师对JS的感觉就是不用学习，生搬硬套，大量的“效果宝盒”软件，一套就有各种特效了。没有人琢磨语言特性。
   * 2004年：谷歌打开了Ajax这个潘多拉的盒子，从此JavaScript被人重视，很多人开始学习JS语言。当时问世了两本JS巨作**《犀牛书》**、**《高级程序设计》**
   * 2007年：三层分离，iPhone发布，人们开始**重视用户体验**。大家发现了，JavaScript是**web页面中制作交互效果唯一的语言**，所以把JS的重视程度，提到了相当高的一个地位。招聘信息里面开始出现独立的“JS工程师”职位了，之前都是后台工程师捎带脚写写JS。
   * 2008年：Chrome浏览器发布，**V8引擎**加快了JS的解析，之前的浏览器解析JS的时候卡顿卡顿的，动画效果是蹦蹦的。在Chrome里，它的引擎叫做V8，运行JS很流畅。
   * 2009年：**jQuery**变得流行，解决了浏览器兼容问题，制作页面效果变得简单，越来越多的初学者愿意学习JavaScript。
   * 2010年：**Canvas画布技术**得到众多浏览器支持，可以用Canvas替代Flash了，并且能制作小游戏，比如偷菜、停车小游戏。
   * 2011年：**Node.js**得到广泛应用，实际上就是把JavaScript运行在了服务器上，**单线程非阻塞**，能够让企业用最小的成本实现后台网站，比如之前4万的服务器都卡，现在用了node之后，4000的机器都很流畅。
   * 2012年：**HTML5+CSS3**的流行，也带火了JavaScript。
   * 2013年：**hybrid app模式**开始流行。就是做手机app的时候，老板们发现要雇佣三队人马，ios、安卓、windows phone。花三份工资，并且产品还不好迭代。所以人们发明了用网页技术开发手机App的技术，叫做web app。hybrid app就是混合app，同时结合web技术和原生开发技术。省钱，好迭代。
   * 2015年：**ECMA6**发布，叫做**ECMA2015**。重量级的改变，把语言的特性颠覆性的一个增强。

5. JavaScript总体比较好学
   
   * 好学的点：
      1. JavaScript是有界面效果：不像C语言，黑底白字，很枯燥的。
      2. JavaScript的语法来源于C和Java：有C和Java的经验同学好学很多。
      3. JavaScript是弱变量类型的语言，动态数据类型语言。
      4. JavaScript运行在宿主环境**（即浏览器）**中，不关心内存，垃圾回收。

   * 不好学的点：
      1. 兼容性问题： **IE8**是个怪胎，很多东西都不一样，所以就要写兼容写法，不怕造轮子，多写几遍
      2. 花式写法很多，抽象：从简单入手，细细品味代码
      3. 太多细节：认真写自己的笔记，自己做实验;

6. 学习方法
   1. 要多去“品”程序，多去思考内在逻辑，读懂每一行代码！
   2. JS机械**重复性的劳动几乎为0**，基本都是创造性的劳动。HTML、CSS都是重复的劳动，margin、padding挤来挤去。
   3. 永远不要背程序，每一个程序都必须自己会写。

## Hello World

这事儿吧，挺有意思，就是学习任何的语言，我们都喜欢在屏幕上直接输出一点什么，当做最简单、最基本的案例。输出什么大家随意，但是很多人都习惯输出“hello world”，世界你好。感觉自己很有情怀的样子。

1. alert()语句：`alert("Hello World")`
2. 每条语句之后用**;**结尾。注意，所有的符号都是英文的符号，不要使用中文！
3. 程序是顺序执行的，任何程序都是这样。![](/images/9.png)
4. 控制台：![](/images/console.png)

    控制台是Chrome浏览器“检查”里面的功能，快捷键是F12。英文叫做console。程序的所有未捕获的错误，都会在控制台中输出。控制台是调试程序的一个利器。

5. 行文特性

    JavaScript语句和语句之间的换行、空格、缩进都不敏感。![](/images/hangwen.png)

    语句后面的分号，不是必须写的，如果语句是一行一行写的，那么分号是没有必要的。但是压缩页面的时候，语句结尾的分号，非常重要。

    我们把页面做好之后，通常都会进行压缩，用软件把所有的空格、换行都去掉。此时，语句末尾的分号显得非常重要，如果去掉分号，将不能执行。

6. 注释

    给人看的东西，对读程序是一个提示作用。![](/images/zhushi.png)

## 字面量

### 数字的字面量

数字的字面量，就是这个数字自己，并不需要任何的符号来界定这个数字。

JavaScript中，数字的字面量可以有三种进制：

1. 10进制
2. 8进制：如果以**0**开头、或者以**0o**开头、或者以**0O**开头的都是8进制，8进制只能用**0~7**来表示

```javascript
    <script type="text/javascript">
        //以0开头，所以就是八进制；显示的时候会以十进制显示
        //3*8+6=30
        console.log(036);  //30
        console.log(044);  //36
        console.log(010);  //8
        console.log(0o36); //30
        console.log(0O36); //30
    </script>
```
    注意，八进制只能出现0~7这8中字符，如果表示不合法，那么JS将自动的认为你输入错了，从而用十进制进行显示：
    ![](/images/bajinzhi.png)

    但是以0o开头、0O开头的数字，如果后面写错了，控制台报错！
    ![](/images/bajinzhi2.png)

3. 16进制：如果以**0x**开头的都是十六进制
```javascript
    console.log(0xff);  //255
    console.log(0x2b);  //43
    console.log(0x11);  //17
```

    如果后面有错误的写法，那么控制台报错：
    ![](/images/shiliujinzhi.png)

4. 总结：
   
    下面的输出结果都是15：
    ```javascript
    console.log(15);
    console.log(017);
    console.log(0o17);
    console.log(0O17);
    console.log(0xf);
    ```

    下面的输出结果都是负15：
    ```javascript
    console.log(-15);
    console.log(-017);
    console.log(-0o17);
    console.log(-0O17);
    console.log(-0xf);
    ```

5. 小数的字面量也很简单，就是数学上的点。计算机世界中，小数称为**“浮点数”**
   
    允许使用e来表示乘以10的几次幂：
    ```javascript
    console.log(-3.1415926);//-3.1415926
    console.log(.315);//0.315 如果整数位数是0，可以不写
    console.log(5e5);//500000
    console.log(5.6e5);//560000
    console.log(1e-4);//0.0001
    console.log(.1e-3);//0.0001
    ```

    **只有十进制有**小数的字面量，八进制、十六进制没有小数的字面量。

6. 最后学习两个特殊的字面量
   * Infinity 无穷大
   ```
    console.log(3e456498794465465)
   ```
   控制台会显示：![](/images/infinity.png)
   至于多大的数字能生成无穷大，不同浏览器不一样，不要管。

   * -Infinity 负无穷大
   ```
    console.log(-3e456498794465465)
   ```
   控制台会显示：![](/images/infinity2.png)

   * **NaN** 英语全名叫做not a number，不是一个数。比较哲学的是，这个“不是一个数”是一个数字字面量。
   ```
    console.log(0/0)
   ```
   控制台会显示：![](/images/NaN.png)
   总结一下，数字字面量有整数字面量（十进制、16进制、八进制），浮点数字面量（要记住e），Infinity，NaN

### 字符串的字面量

字符串是一个术语，就是人类说的语句、词。

字符串的字面量，必须用**双引号、单引号**包裹起来。字符串被限定在同种引号之间；也即，必须是**成对**单引号或**成对**双引号。

1. 如果一个数字，用引号引起来，那么就是字符串了：`console.log("3")`
2. 转义字符：
   * `\n` 回车换行
   * `\t` tab缩进
   * 
   ```javascript
   alert("你好\n啊\n我很爱你\n啊");
   ```
   输出效果：![](/images/zhuanyizifu.png)
   
   * 可以用`\"`来表达引号: `console.log("老师说你像\"考拉\"一样漂亮")`
   * 也可以用`\\`来表达\号：`console.log("c:\\a\\b.jpg")`

## 变量

### 整体感知：

变量（Variables）,和高中代数学习的x、y、z很像，它们不是字母，而是蕴含值的符号。

```javascript
<script>
    var a;  //定义一个变量
    a = 100;  //赋值
    console.log(a);  //输出变量
</script>
```

我们使用**var**关键字来定义变量，所谓的关键字就是一些**有特殊功能的小词语**，关键字后面要有空格。

### 变量必须先声明（定义）才能使用：

变量的名称就是标识符（identifiers），任何标识符的命名都要遵守一定的**规则**：

   * js语言中，一个标识符可以由字母、下划线(_)、美元符($)、数字(0-9)组成，但是**不能以数字开头**
   * js是区分大小写的，即A和a不是同一个变量
   * 标识符不能是js的**保留字**和**关键字**
   * **保留字**是系统里面的有用途的字，有这些：
      * **abstract、boolean、byte、char、class、const、debugger、double、enum、export、extends、fimal、float
goto、implements、import、int、interface、long、mative、package、private、protected、public、short、static、super、synchronized、throws、transient、volatile**

非法标识符举例：

```javascript
var 123a;  //不能数字开头
var 12_a;  //不能数字开头
var abc@163;  //不能有特殊符号，符号只能有_和$
var abc￥;  //不能有特殊符号，符号只能有_和$
var var;  //不能是关键字
var class;  //不能是保留字
```

### 变量的赋值：

变量的赋值用等号，等号就是赋值的符号，在JS中等号**没有其他的含义，等号就是赋值**。等号右边的值给左边，**等号右边的值不变**。

如果一个变量，仅仅被var了，但是没有被赋初值呢，此时这个变量的值就是**undefined**：

```javascript
var m;
console.log(m)  //输出undefined
```
实际上我们已经var了这个m，已经定义了这个m，只不过这就是浏览器的一个规矩，如果这个变量没有被赋初值，那么这个变量就视为没有“定义完成”。值就是undefined

### 变量声明的提升:

这是js特有的一个特点，其他语言都没有这个特点。有些程序员挺反感这个特点的。

我们现在先去改变变量的值，然后定义变量，由于JS有一个机制，叫做变量声明的提升，所以现在程序在执行前会已经看见这个程序中有一行定义变量，所以就会提升到程序开头去运行：

```javascript
<script type="text/javascript">
    a = 100;
    var a;  //这行定义变量会自动提升到所有语句之前
    console.log(a);
</script>
```

记住，js只能提升变量的声明，而不能提升变量的赋初值：

```javascript
console.log(a);
var a = 100;  //输出undefined
```
等价于：

```javascript
var a;  //变量声明自动提升到第一行
console.log(a);
a = 100;  //但是赋初值还留在原地
```  

### 不写var的情况：

```javascript
abc = 123;
console.log(abc);  //输出123
```
定义abc的时候没有写var，程序没有报错，说明这个abc变量真的已经被定义成功了。现在你看不出来var和不var的区别，感觉都是成功的，但是日后你就会知道**不写var定义了一个全局变量，作用域是不能控制的**。

### 用逗号来隔开多个变量的定义：

    var a = 7,b = 9,c = 10;
逗号这个表示法，只能用于变量的连续定义，不要瞎用。

### 变量的类型：

基本类型 5 种：

1. number  数字类型
2. string  字符串类型
3. boolean  布尔类型，仅有 true 和 false 
4. null  这个值自己是一种类型
5. undefined  一个变量只var过，没有赋初值，它的默认值是undefined;这个值自己是一种类型

引用类型：以后再说

`typeof`关键字：用来检测一个变量的类型：

    var a = 100;
    console.log(typeof a);  //输出 number

**所有的数字都是 number 类型**：

```
<script type="text/javascript">
    //下面定义的变量都是number类型
    var a = 100;
    var b = 234243245345;
    var c = -345345435435;
    var d = 345.3245234;
    var e = .5e6;
    var f = 0xff;
    var g = 017;
    var h = Infinity;
    var i = NaN;

    console.log(typeof a);
    console.log(typeof b);
    console.log(typeof c);
    console.log(typeof d);
    console.log(typeof e);
    console.log(typeof f);
    console.log(typeof g);
    console.log(typeof h);
    console.log(typeof i);
</script>
```
JS中所有的数字都是number类型的，不在细分为整形int、浮点型float这些乱七八糟的东西。

number类型的东西：所有数字（不分正负、不分整浮、不分大小、不分进制）、Infinity、NaN。

**string类型：**

```javascript
//下面定义的变量都是string类型
var m1 = "哈哈";
var m2 = "123";	
var m3 = "";  //空字符串，也是字符串
console.log(typeof m1);
console.log(typeof m2);
console.log(typeof m3);
```

### 变量类型的转换：

先来学习一个语句，这个语句和 alert 差不多，也是弹窗，弹的是输入框：
```javascript
prompt("请输入您的电话号码","139");
```
弹框如图所示：![](/images/prompt.png)

用prompt接收的任何东西都是字符串，哪怕用户输入了一个数字，也是字符串的数字。

这些小功能，**就叫做程序给我们提供的API**，每个API都有自己不同的语法。

1. parseInt() 
   * parseInt就是**将一个string转为一个整数**，不四舍五入，直接截取整数部分。如果这个string有乱七八糟的东西，那么就截取前面数字部分。
   * parseInt()不仅仅能够将一个 string 转为整数，更能进行一个进制的转换，把任何进制的数字，都换为10进制。进制和要转换的字符串，用逗号隔开。
  
   ```javascript
    //下面的运算结果都是 15
    parseInt(15,10)
    parseInt(17,8)
    parseInt(1111,2)
    parseInt("0xf",16)
    parseInt("f",16)
    parseInt(16,9)
    parseInt("15e6",10)
    parseInt("15*6",10)
   ```
   * parseInt如果不能转，那么就返回NaN：
   ```javascript
    parseInt("Hello",8);
    parseInt("546",2);
    parseInt("三百六十五")
   ```

2. parseFloat()
   * parseFloat就是将字符串转为浮点数
   * 尽可能的将一个字符串转为浮点数，浮点数之后如果有乱七八糟的内容，直接舍弃
   ```javascript
    <script type="text/javascript">
    var a = "123.67.88";
    var b = parseFloat(a);
    console.log(b);  //控制台输出 123.67
    </script>
   ```

3. number → string:

   * 将一个数字，与一个空字符串进行连字符运算，那么就是自动转为字符串了。

   ```javascript
   var a = 123;
   var b = a + "";
   console.log(b);  //输出字符串 123
   console.log(typeof b);  //控制台输出 string
   ```


## 数学运算符

1. + 加法
2. - 减法
3. * 乘法
4. / 除法
5. % 取余数
6. () 括号

乘方API: `Math.pow(3,4)` **表示3的4次方**

根号API: `Math.sqrt(81)` **表示81开根号**

## 总结今天学习的所有 API：

1. alert("哈哈");
2. prompt("请输入数字","默认值");
3. console.log("哈哈");
4. parseInt("123",8);
5. parseFloat("123");
6. Math.pow(3,4);
7. Math.sqrt(81);
