---
title: "JS基本语法"
date: 2019-09-26T11:47:31+08:00
draft: false
---

# JS 基本语法

## 什么是表达式和语句

1. 表达式：
   - 1+2 表达式的值为 3；
   - add(1,2)表达式的值为函数的返回值；
   - console.log 表达式的值为函数本身；
   - console.log(3)表达式的值为多少?
2. 语句：
   - var a=1 是一个语句；
3. 表达式和语句的区别？
   - 表达式一般都有值，语句可能有也可能没有值；
   - 语句一般会改变环境(声明、赋值)；
   - 上面两句话并不是绝对的；
4. 大小写敏感：
   - var a 和 var A 是不同的；
   - object 与 Object 是不同的；
   - function 和 Function 是不同的；
5. 空格：
   - 大部分空格没有实际意义；
   - var a=1 和 var a = 1 没有区别；加回车大部分时候也不影响；
   * 只有一个地方不能加回车，那就是 return 后面；

## 标识符的规则

1. 规则：
   - 第一个字符，可以是 Unicode 字母或者\$或者\_或者中文；
   - 后面的字符，除了上面所说，还可以有数字；
2. 变量名是标识符：
   - var \_=1;
   - var \$=2;
   - var**\_**=6;
   - var 你好="hi";
3. 注释的分类：
   1. 不好的注释：
      - 把代码翻译成中文；
      - 过时的注释；
      - 发泄不满的注释；
   2. 好的注释：
      - 踩坑注释；
      - 为什么代码会写的这么奇怪，遇到什么 bug；
4. 区块 block
   - 把代码包在一起，例如：
   ```
   {
    let a = 1
    let b = 2
   }
   ```
   - 常常与 if/for/while 合用

## if else 语句

1. 语法：
   - if(表达式){语句 1}else{语句 2}
   - {}在语句只有一句的时候可以省略，但不建议这样做；
2. 变态情况：
   - 表达式可以非常变态，如 a=1；
   - 语句 1 里可以非常变态，如嵌套的 if else;
   - 语句 2 里可以非常变态，如嵌套的 if else;
   - 缩进也可以很变态，如面试题常常下套：
     ```
     a=1
     if(a===2)
       console.log("a")
       console.log("a等于2")
     ```
3. 使用最没有歧义的写法——程序员戒律；
4. 最推荐使用的写法：
   ```
   if (表达式) {
       语句
   } else if (表达式) {
       语句
   } else {
       语句
   }
   ```
5. 次推荐使用的写法:
   ```
    function fn(){
        if (表达式) {
            return 表达式
        }
        if(表达式){
            return 表达式
        }
        return 表达式
    }
   ```

## while for 语句

1. while 语法：
   - while(表达式){语句}
   - 判断表达式的真假
   - 当表达式为真，执行语句，执行完再判断表达式的真假；
   - 当表达式为假，执行后面的语句；
2. do while 用的不多，自行了解；
3. ## for 循环语法：
   ```
   for(语句1;表达式2;语句3){
       循环体
   }
   ```
   - 先执行语句 1；
   - 然后判断表达式 2；
   - 如果为真，执行循环体，然后执行语句 3；
   - 如果为假，直接退出循环，执行后面的语句；

## break continue

1. 退出当前循环 VS 退出当前一次循环；

## label

1. 用的很少，面试会考（概率 5%）;
2. label 语法：
   ```
    foo: {
        console.log(1);
        break foo;
        console.log("本行不会输出");
    }
    console.log(2);
   ```

## 推荐书籍：

1. 阮一峰的免费教程；
   - http://wangdoc.com/javascript/basic/grammar.html
2. 你不知道的 JavaScript
   - 先买上卷，适合进阶；
