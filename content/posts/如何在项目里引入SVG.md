---
title: "如何在项目里引入SVG"
date: 2020-03-12T11:53:11+08:00
draft: false
---

1. 去 https://www.iconfont.cn 下载合适的图标,选择SVG下载
2. 在vue项目src-assets目录里新建icons目录，把下载的svg图片放到这里面(svg实际就是xml文件)
3. 此时在vue组件里导入svg会报错：`import x from '@/assets/icons/label.svg'` ,怎么办？接着做：
4. 在src目录里的shims-vue.d.ts（另一个ts文件可能也可以）里加上如下代码：

  ```typeScript
  declare module "*.svg" {
     const content: string;
     export default content;
  }
  ```

5. 此时报错会消失，但还不能用，接着命令行运行 yarn add svg-sprite-loader -D 安装一个loader
6. 在vue.config.js文件里添加代码(这一步配置的是webpack)，最终代码为：
   
```js
const path = require('path')

module.exports = {
  lintOnSave: false,
  chainWebpack: config =>{
    const dir = path.resolve(__dirname, 'src/assets/icons')

    config.module
      .rule('svg-sprite')
      .test(/\.svg$/)
      .include.add(dir).end() // 包含 icons 目录
      .use('svg-sprite-loader').loader('svg-sprite-loader').options({extract:false}).end()
    config.plugin('svg-sprite').use(require('svg-sprite-loader/plugin'), [{plainSprite: true}])
    config.module.rule('svg').exclude.add(dir) // 其他 svg loader 排除 icons 目录


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
}
```

7. 此时就可以在vue组件的template里使用了：

```xml
  <svg>
     <use xlink:href="#label" />
  </svg>
```

    ![](/images/svg.png)

8. 但是这样每次都要引入某个svg文件太麻烦，能一步引入所有svg文件么？可以，在引用svg的文件里加上以下代码：

```typescript
let importAll = (requireContext: __WebpackModuleApi.RequireContext) => requireContext.keys().forEach(requireContext);

try {importAll(require.context('../assets/icons', true, /\.svg$/));} catch (error) {console.log(error);}
```

    ![](/images/svg-2.png)

9. 下载的svg可能默认有填充颜色，这样你就无法再变更svg的颜色，怎么办？

    简单方法是找到svg文件，在文件里搜索 fill ，去掉 fill 即可，例如：![](/images/svg-3.png)

    但是这种方法不好，当你有100个svg文件时，难道你要修改100次么？

    因此正确的方法是使用一个loader，即修改 vue.config.js 文件：

    ```javaScript
    .use('svgo-loader').loader('svgo-loader')
      .tap(options => ({...options, plugins: [{removeAttrs: {attrs: 'fill'}}]})).end()
    ```

    然后安装这个loader： 命令行运行 yarn add --dev svgo-loader

10. 还有一个bug，但是视频里又说已经解决了，先不说这个问题了，遇到再加这部分内容吧。