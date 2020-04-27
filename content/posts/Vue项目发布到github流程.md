---
title: "Vue项目发布到github流程"
date: 2020-04-25T16:34:04+08:00
draft: false
---

## 项目做好了如何发布到github？

1. 运行`rm -rf dist` 目的是清空dist目录
2. 运行`yarn build` 进行打包构建
3. 运行` yarn global add serve` 全局安装预览工具，下次就不用再安装了
4. 运行`serve -s dist` 预览dist目录，看打包后的dist目录是否正常显示,测试后就可以关了。
   

5. github上新建第一个仓库accounting-items-2用来存放源码（该仓库忽略dist），并提交本地代码
6. github上新建第二个仓库accounting-items-2-website（用来存放dist里的打包后的项目）
7. 配置vue.config.js文件：
   ![](/images/publicPath.png)
8. 根目录下新建deploy.sh文件，其内容为：
   
```sh
#!/usr/bin/env sh

# 当发生错误时中止脚本
set -e

# 构建
yarn build

# cd 到构建输出的目录下 
cd dist

# 部署到自定义域域名
# echo 'www.example.com' > CNAME

git init
git add -A
git commit -m 'deploy'

# 部署到 https://<USERNAME>.github.io/<REPO>
# 下边的需要根据上边新建的仓库进行修改：
# git push -f git@github.com:<USERNAME>/<REPO>.git master:gh-pages

cd -
```

10. 命令行运行 `sh deploy.sh`
