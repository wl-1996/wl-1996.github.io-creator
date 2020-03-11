---
title: "WebStorm使用教程"
date: 2020-03-10T17:13:07+08:00
draft: false
---

## 配置终端

1. 打开设置，进入 Tools => Terminal
2. 将 Shell Path 改为你的 bash.exe 的绝对路径，比如我的路径是 C:\自己找路径\cmder\vendor\git-for-windows\bin\bash.exe，如下图（如果找不对路径，可以使用 everything 软件搜索 bash.exe，从搜索结果里找和我的路径近似的）

    ![](/images/WebStorm-setting-1.png)

3. 建议 cmder 用户将提示符从 λ 改为 $（Git bash 用户请跳过此步骤），用 VSCode 打开文件 C:\自己找路径\cmder\vendor\git-for-windows\etc\profile.d\git-prompt.sh，改其中第 36 行，将 λ 改为 $
   
    ![](/images/WebStorm-setting-2.png)

4. 此时就可以打开 WebStorm 的终端了

## 配置 git

1. 打开设置，进入 Version Control => Git
2. 将 Path to Git 改为 git.exe 的绝对路径，比如我的路径是 C:\请自己找到对应的目录\cmder\vendor\git-for-windows\bin\git.exe，如下图

    ![](/images/WebStorm-setting-3.png)
    
## 查看快捷键

1. 按两下 Shift，你会得到一个搜索框，这个搜索框可以搜索任何东西
2. 在搜索框里输入你想要的功能名称，比如 reformat （代码格式化），然后你就看到对应的操作（Action）
3. Action 后面就跟着对应的快捷键

    ![](/images/WebStorm-setting-4.png)
 
4. 不过这个方式的缺点是只能搜英文，所以可以看第二个方式：查看菜单栏，快捷键就写在菜单栏每一项 Action 的后面

    ![](/images/WebStorm-setting-5.png)
    
## 修改快捷键

1. 在 Settings 里的 keymap 里的搜索栏搜索即可，见下图
 
    ![](/images/WebStorm-setting-6.png)
    

