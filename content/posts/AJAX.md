---
title: "AJAX"
date: 2019-11-07T09:05:11+08:00
draft: false
---

# AJAX-Asynchronous JavaScript And XML

## 简介：
1. 用 JS 发请求和收响应，这就是 AJAX 的全部内容
2. AJAX 是浏览器上的功能：
   - 浏览器可以发请求，收响应
   * 浏览器在 window 上加了一个 XMLHttpRequest 函数
   * 用这个构造函数（类）可以构造出一个对象
   * JS 通过它实现发请求，收响应
  
## 使用方法：
1. 准备一个服务器：
   - 使用 server.js 作为我们的服务器
   - 下载或复制代码即可用 node server.js 8888 启动
   - 添加 index.html/man.js 两个路由
2. 加载 html/css/js/xml 的四个标准步骤：
   1. 创建 HttpRequest 对象（全称是 XMLHttpRequest）
   2. 调用对象的 open 方法
   3. 监听对象的 onreadystatechange 事件——在事件处理函数里操作 HTML 文件内容/CSS 文件内容/JS 文件内容/responseXML
   4. 调用对象的 send 方法（发送请求）——具体代码请打开 MDN 用 CRM 大法，或者看我的个人博客：https://github.com/wl-1996/ajax-1
   5. 代码如下：
   
   ```javaScript
        getHTML.onclick = () => {
            const request = new XMLHttpRequest();//创建对象
            request.open("GET", "/3.html");//调用对象的open方法（规定请求类型，请求资源的位置，是否异步-默认不写就是异步）
            request.onreadystatechange = () => {//监听对象的onreadystatechange事件
                console.log(request.readyState);
                if (request.readyState === 4) {
                    if (request.status >= 200 && request.status < 300) {
                        const div = document.createElement("div");
                        div.innerHTML = request.response;
                        document.body.appendChild(div);
                    }
                }
            };
            request.send();//调用对象的send方法（发送请求）
        };
   ```
3. Http 是个框，什么都能往里装：
   - 可以装 HTML、CSS、JS、XML......
   - 记得设置正确的 Content-Type(server.js 文件里)
   - 只要你知道怎么解析这些内容，就可以使用这些内容
   - 代码如下：
   
   ```javaScript
        else if (path === "/main.js") {
            response.statusCode = 200;
            response.setHeader("Content-Type", "text/javascript;charset=utf-8");//记得这里设置正确的Content-Type,请求的类型是什么，这里就写什么
            response.write(fs.readFileSync("public/main.js"));
            response.end();
        }
   ```
4. 解析方法：
   - 得到 CSS 之后生成 style 标签

   ```javaScript
        getCSS.onclick = () => {
            const request = new XMLHttpRequest();
            request.open("GET", "/style.css");
            request.onreadystatechange = () => {
                console.log(request.readyState);
                if (request.readyState === 4) {
                    if (request.status >= 200 && request.status < 300) {
                        const style = document.createElement("style");
                        style.innerHTML = request.response;
                        document.head.appendChild(style);
                    } else {
                    alert("加载CSS失败");
                    }
                }
            };
            request.send();
        };
   ```
   
   - 得到 JS 之后生成 script 标签
   
   ```javaScript
        getJS.onclick = () => {
            const request = new XMLHttpRequest();
            request.open("GET", "/2.js");
            request.onreadystatechange = () => {
                console.log(request.readyState);
                if (request.readyState === 4) {
                    if (request.status >= 200 && request.status < 300) {
                        const script = document.createElement("script");
                        script.innerHTML = request.response;
                        document.body.appendChild(script);
                     } else {
                        alert("加载JS失败");
                    }
                }
            };
            request.send();
        };
   ```
   - 得到 HTML 之后使用 innerHTML 和 DOM API
   
   ```javaScript
        getHTML.onclick = () => {
            const request = new XMLHttpRequest();
            request.open("GET", "/3.html");
            request.onreadystatechange = () => {
                console.log(request.readyState);
                if (request.readyState === 4) {
                    if (request.status >= 200 && request.status < 300) {
                        const div = document.createElement("div");
                        div.innerHTML = request.response;
                        document.body.appendChild(div);
                    }
                }
            };
            request.send();
        };
   ```
   
   - 得到 XML 之后使用 responseXML 和 DOM API
   
   ```javaScript
        getXML.onclick = () => {
            const request = new XMLHttpRequest();
            request.open("GET", "/4.xml");
            request.onreadystatechange = () => {
            // console.log(request.readyState);
                if (request.readyState === 4) {
                    if (request.status >= 200 && request.status < 300) {
                        const dom = request.responseXML;
                        // 找到标签名为warning的第一个元素，获取它的文本内容
                        const text = dom.getElementsByTagName("warning")[0].textContent;
                        console.log(text.trim()); //.trim()方法删除字符串两端的空格
                    }
                }
            };
            request.send();
        };
   ```
   - 不同类型的数据有不同类型的解析方法
  
## JSON
1. 什么是 JSON?
   - JavaScript Object Notation
   * JSON 是一门语言，跟 HTML、CSS、XML、JS 一样，是一门独立的语言
   * JSON 不是编程语言，是标记语言，跟 HTML、XML、Markdown 一样，用来展示数据
2. JSON 支持的数据类型
   - string 只支持双引号，不支持单引号和无引号
   - number 支持科学计数法
   - bool true 和 false
   - null 没有 undefined
   - object
   - array
   - 就这六种，注意跟 JS 的七种数据类型区别开来
   - 不支持函数，不支持变量（所以也不支持引用）
3. JSON 借鉴（抄袭）JS
4.  加载 JSON 的四个步骤：
   - 创建 HttpRequest 对象（全称是 XMLHttpRequest）
   - 调用对象的 open 方法
   - 监听对象的 onreadystatechange 事件——在事件处理函数里使用 JSON.parse
   - 调用对象的 send 方法（发送请求）
   - 具体代码请打开 MDN 用 CRM 大法，或者看我的个人博客：https://github.com/wl-1996/ajax-1
   - 代码如下：   

   ```javaScript
        getJSON.onclick = () => {
            const request = new XMLHttpRequest();
            request.open("GET", "/5.json");
            request.onreadystatechange = () => {
                if (request.readyState === 4 && request.status === 200) {
                    const bool = JSON.parse(request.response);
                }
            };
            request.send();
            console.log(request.response);
            setTimeout(() => {
                console.log(request.response);
            }, 2000);
        }   
   ```
5. JSON.parse:
   - 将符合 JSON 语法的字符串转换成 JS 对应类型的数据
   - JSON 字符串=》JS 数据
   - 由于 JSON 只有六种类型，所以转成的数据也只有 6 种类型
   - 如果不符合 JSON 语法，则直接抛出一个 Error 对象
   - 一半用 try catch 捕获错误
6. JSON.stringify：
   - 是 JSON.parse 的逆运算
   - JS 数据=》JSON 字符串
   - 由于 JS 的数据类型比 JSON 多，所以不一定能成功
   - 如果失败，就抛出一个 Error 对象
