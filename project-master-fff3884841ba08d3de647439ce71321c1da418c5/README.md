
- 概述： 我们提供的项目`src` 目录下的代码虽然本身能正常运行，但是代码存在大量全局变量，js文件也很多，并且没有经过优化，在本项目我们希望大家使用 [
1. 使用 CommonJS 进行对 `src/js` 目录下的 js 代码进行模块化，所有模块都不产生全局变量，只通过 require 声明依赖，以及通过 module.exports 暴露模块接口。

2. 根目录增加 `webpack.config.js` 配置文件，使用 Webpack 对 js 进行打包, 入口文件为 `src/js/index.js`, 打包输出到 `dist/bundle.js`。

3. 使用 `css-loader` 和 `style-loader`, 将 `src/css/style.css` 也加入打包。

4. 使用 `html-webpack-plugin` 将 `src/index.html` 作为模板，删掉index.html 里面所有的 `script` 和 `link` 标签，最终在 `dist/` 目录自动生成引用打包后文件的 `index.html` 。

5. 使用 `copy-webpack-plugin` 将 `src/images` 下的所有图片复制到 `dist/images` 目录 (具体使用方式查看[文档](https://github.com/kevlened/copy-webpack-plugin))。

6. 使用 `webpack.optimize.UglifyJsPlugin` 对代码进行压缩

7. 经过上面的构建后，构建结果应该是在 `dist` 目录下有 `index.html`，`bundle.js` 和 `images` 目录，直接运行 `dist/index.html` 可以正常运行游戏。

8. 要求上面用到的 npm 包都装到本地，并且所有依赖都记录到 `package.json` 文件的 `devDependencies` 里面，其他人将代码 `clone` 下来之后运行 `npm install` 命令后会自动将所有依赖安装下来，运行 `webpack` 命令就可以成功构建。

## 提示

### Webpack 里的 CommonJS
Webpack 打包本身支持 CommonJS, AMD 甚至 ES6 Modules，而且不需要引用额外的库，只需要直接修改 js 文件，声明依赖和暴露接口就可以了，打包后的模块也会有自己单独的作用域，模块中声明的变量如 `var a = 1` 不会影响全局环境，除非通过 `window.a = 1` 声明，这样才会挂到全局变量。

所以我们修改源代码的 js 文件只需要根据注释在 ***头部声明依赖*** 以及 ***最后声明本模块暴露的接口或对象*** 即可。如 `src/dust.js`, 修改成下面代码即可：

```js
// 灰尘类

// 依赖 global
var global = require('./global');  // 头部声明依赖

// 中间代码不用修改
var Dust = function(){
}
Dust.prototype.init = function(){
}
Dust.prototype.drawDust = function(){
}

module.exports = Dust;   // 最后暴露 Dust 类
```
使用 `extract-text-webpack-plugin` 将 CSS 文件分离出来，构建后目录单独有一个 `style.css`

使用 `clean-webpack-plugin`， 每次构建之前删掉 `dist` 目录，避免上一次构建的影响

使用 `webpack-dev-server` 可以开启本地服务器，保存代码后页面自动刷新。

`webpack` 和 `webpack-dev-server` 只需装到项目本地，不需要全局安装，使用 [`npm scripts`](https://doc.webpack-china.org/guides/getting-started/#npm-npm-scripts-) 运行构建任务，比如`npm run build` 运行 `webpack` 命令, `npm run server` 可以开启本地服务器。

## 链接
* [项目源码地址](https://github.com/luckykun/tinyHeart)(经修改)
