---
title: "如何在项目里引入SVG"
date: 2020-03-12T11:53:11+08:00
draft: false
---

1. 去 https://www.iconfont.cn 下载合适的图标,选择 SVG 下载
2. 在 vue 项目 src-assets 目录里新建 icons 目录，把下载的 svg 图片放到这里面(svg 实际就是 xml 文件)
3. 此时在 vue 组件里导入 svg 会报错：`import x from '@/assets/icons/label.svg'` ,怎么办？接着做：
4. 在 src 目录里的 shims-vue.d.ts（另一个 ts 文件可能也可以）里加上如下代码：

```typeScript
declare module "*.svg" {
   const content: string;
   export default content;
}
```

5. 此时报错会消失，但还不能用，接着命令行运行 `yarn add svg-sprite-loader -D` 安装一个 loader
6. 在 vue.config.js 文件里添加代码(这一步配置的是 webpack)，最终代码为：

```js
const path = require("path");

module.exports = {
  lintOnSave: false,
  chainWebpack: config => {
    const dir = path.resolve(__dirname, "src/assets/icons");

    config.module
      .rule("svg-sprite")
      .test(/\.svg$/)
      .include.add(dir)
      .end() // 包含 icons 目录
      .use("svg-sprite-loader")
      .loader("svg-sprite-loader")
      .options({ extract: false })
      .end();
    config
      .plugin("svg-sprite")
      .use(require("svg-sprite-loader/plugin"), [{ plainSprite: true }]);
    config.module.rule("svg").exclude.add(dir); // 其他 svg loader 排除 icons 目录

    // config.module
    //   .rule('svg-sprite')
    //   .test(/\.(svg)(\?.*)?$/)
    //   .include.add(dir).end()
    //   .use('svg-sprite-loader-mod').loader('svg-sprite-loader-mod').options({extract: false}).end()
    //   .use('svgo-loader').loader('svgo-loader')
    //   .tap(options => ({...options, plugins: [{removeAttrs: {attrs: 'fill'}}]}))
    //   .end()
    // config.plugin('svg-sprite').use(require('svg-sprite-loader-mod/plugin'), [{plainSprite: true}])
    // config.module.rule('svg').exclude.add(dir)
  }
};
```

7. 重新运行 `yarn serve` ,此时就可以在 vue 组件的 template 里使用了：

```xml
  <svg>
     <use xlink:href="#label" />
  </svg>
```

    ![](/images/svg.png)

8. 但是这样每次都要引入某个 svg 文件太麻烦，能一步引入所有 svg 文件么？可以，在引用 svg 的文件里加上以下代码：

```typescript
let importAll = (requireContext: __WebpackModuleApi.RequireContext) =>
  requireContext.keys().forEach(requireContext);

try {
  importAll(require.context("../assets/icons", true, /\.svg$/));
} catch (error) {
  console.log(error);
}
```

    ![](/images/svg-2.png)

9.  下载的 svg 可能默认有填充颜色，这样你就无法再变更 svg 的颜色，怎么办？

    简单方法是找到 svg 文件，在文件里搜索 fill ，去掉 fill 即可，例如：![](/images/svg-3.png)

    但是这种方法不好，当你有 100 个 svg 文件时，难道你要修改 100 次么？

    因此正确的方法是使用一个 loader，即修改 vue.config.js 文件（上边第 6 步已经修改过了，这次是增加两行代码）：

```javaScript
.use('svgo-loader').loader('svgo-loader')
  .tap(options => ({...options, plugins: [{removeAttrs: {attrs: 'fill'}}]})).end()
```

    最终代码为：![](/images/svg-4.png)

```javascript
const path = require("path");
module.exports = {
  lintOnSave: false,
  chainWebpack: config => {
    const dir = path.resolve(__dirname, "src/assets/icons");
    config.module
      .rule("svg-sprite")
      .test(/\.svg$/)
      .include.add(dir)
      .end() // 包含 icons 目录
      .use("svg-sprite-loader")
      .loader("svg-sprite-loader")
      .options({ extract: false })
      .end()
      .use("svgo-loader")
      .loader("svgo-loader")
      .tap(options => ({
        ...options,
        plugins: [{ removeAttrs: { attrs: "fill" } }]
      }))
      .end();
    config
      .plugin("svg-sprite")
      .use(require("svg-sprite-loader/plugin"), [{ plainSprite: true }]);
    config.module.rule("svg").exclude.add(dir); // 其他 svg loader 排除 icons 目录

    // config.module
    //   .rule('svg-sprite')
    //   .test(/\.(svg)(\?.*)?$/)
    //   .include.add(dir).end()
    //   .use('svg-sprite-loader-mod').loader('svg-sprite-loader-mod').options({extract: false}).end()
    //   .use('svgo-loader').loader('svgo-loader')
    //   .tap(options => ({...options, plugins: [{removeAttrs: {attrs: 'fill'}}]}))
    //   .end()
    // config.plugin('svg-sprite').use(require('svg-sprite-loader-mod/plugin'), [{plainSprite: true}])
    // config.module.rule('svg').exclude.add(dir)
  }
};
```

    然后安装这个 loader： 命令行运行 `yarn add --dev svgo-loader`

    重新运行 `yarn serve` 预览效果

10. 还有一个 bug，但是视频里又说已经解决了，先不说这个问题了，遇到再加这部分内容吧。

    原话：
    2020 年，svg-sprite-loader 把我下个视频里说的 bug 给修复了。所以你在 WebStorm 里应该看不到我说的 bug 了。
