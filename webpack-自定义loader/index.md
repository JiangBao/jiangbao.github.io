# Webpack - 自定义 loader


<!--more-->
{{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/webpack.png" >}}

> loader 是导出为一个函数的 node 模块。该函数在 loader 转换资源的时候调用。给定的函数将调用[loader API](https://www.webpackjs.com/api/loaders/)，并通过 `this` 上下文访问。

## 背景
webpack 自身只能解析 JS 和 JSON 文件，无法理解其它类型的的文件模块，当我们需要处理 CSS、图片等非 JS 资源时，需要使用 `loader` 将之转换为 webpack 核心能够理解的形式。loader 本质是一个函数，接收待处理的资源模块，输出符合 webpack 处理规范的的内容。

## loader 使用
以简单的 `css` 模块处理为例，我们使用 `style-loader` 和 `css-loader` 这两个 loader，类似如下的 webpack 配置：
```js
module.exports = {
  //...
  module: {
    rules: [
      test: /\.css$/,
      // css-loader: 解析 CSS 文件，以字符串形式打包到 js 文件
      // style-loader: 把 js 里的样式代码插入到 html
      use: ['style-loader', 'css-loader'],
    ]
  }
  //...
};
```
loader 执行顺序是从后往前，先执行 `css-loader`，再将其执行结果交给 `style-loader` 处理。由此可以得到自定义 loader 时需要遵守的几个基本原则：
* 单一职责：每个 loader 只做一个任务，不仅易维护，也可以在更多场景链式调用。
* 链式传递：资源文件传入第一个 loader，后续 loader 接受上一个 loader 返回的处理结果。这意味着 loader 不一定要输出 js，只要下一个 loader 可以处理这个输出，这个 loader 就可以返回任意类型的模块。
* 逆向调用：体现在上面的例子就是 use 配置项从右往左执行，这与 js 的函数式编程而非管道流相关。

## 最简单的自定义 loader
编写一个 loader，实现功能：替换所有 `.txt` 文件中以 `[name]` 标记的关键词。[gitub 仓库](https://github.com/JiangBao/webpack-notes/tree/main/loader/replace)  
`replace-keyword-loader.js`:
```js
// 利用 loader-utils 工具包获取传入的参数
const { getOptions } = require('loader-utils');

module.exports = function(source) {
  const options = getOptions(this);
  source = source.replace(/\[name\]/g, options.keyword);

  return `export default ${JSON.stringify(source)}`;
}
```
`webpack.config.js`:
```js
const path = require('path');

module.exports = {
  //...
  module: {
    rules: [
      {
        test: /\.txt$/,
        use: [
          {
            loader: path.resolve('./src/replace-keyword-loader.js'),
            options: {
              keyword: 'JiangBao'
            }
          }
        ]
      }
    ]
  },
};
```

## raw 模式
之前的基础示例中，资源文件会默认转化为 UTF-8 字符串，然后传给 loader，但是在一些图片、音频、视频资源下，使用字符串就不合适了，这时候可以接收原文件的 `Buffer`，设置导出 `raw` 字段为 `true` 即可开启 buffer 类型的参数。  
一个简单的例子：实现最简化的 `url-loader` 功能，将图片转为 base64 编码格式。[github 仓库](https://github.com/JiangBao/webpack-notes/tree/main/loader/raw)  
`raw-loader.js`:
```js
const { getOptions } = require('loader-utils');

module.exports = function(source) {
  const options = getOptions(this);
  const mimetype = options.mimetype || '';

  return `export default ${JSON.stringify(`data:${mimetype};base64,${source.toString('base64')}`)}`;
}

// 开启 buffer 类型文件格式
module.exports.raw = true;
```
`webpack.config.js`:
```js
const path = require('path');

module.exports = {
  //...
  module: {
    rules: [
      {
        test: /\.png$/,
        use: [
          {
            loader: path.resolve('./src/raw-loader.js'),
            options: {
              mimetype: 'image/png'
            }
          }
        ]
      }
    ]
  },
};
```

## pitch
[官网对 pitching loader 的介绍](https://www.webpackjs.com/api/loaders/#%E8%B6%8A%E8%BF%87-loader-pitching-loader-)：loader总是从右到左地被调用，有些情况下，loader 只关心 request 后面的元数据(metadata)，并且忽略前一个 loader 的结果。在实际（从右到左）执行 loader 之前，会先从左到右调用 loader 上的 pitch 方法。如果某个 loader 在 pitch 方法中返回一个结果，那么这个过程就会跳过剩下的 loader。这个关系[引用两张图](https://zhuanlan.zhihu.com/p/104205895)可以看的很清晰
{{<figure src="https://pic4.zhimg.com/80/v2-2c9ccd95160f8d6ad46b46e8a4f98b23_1440w.jpg" >}}
{{<figure src="https://pic4.zhimg.com/80/v2-3c1e7d5a31b68883576e2b6240c75f6f_1440w.jpg" >}}
实例：实现一个简单的`style-loader`，[github 仓库](https://github.com/JiangBao/webpack-notes/tree/main/loader/style)  
`css-loader` 会将css文件打包成字符串到js模块，我们自定义 `style-loader` 则需要通过操作 dom，将这份css代码转成 `style` 标签插入到 `html` 的 `head` 中。避免循环引用问题，需要在 pitch 方法上执行。  
`my-style-loader.js`:
```js
const { stringifyRequest } = require('loader-utils');

function loader(source) {};

loader.pitch = function(remainingRequest) {
  return `
    let style = document.createElement('style');
    const content = require(${stringifyRequest(this, '!!' + remainingRequest)});
    style.innerHTML = content.default;
    document.head.appendChild(style);
  `;
}

module.exports = loader;
```
`webpack.config.js`:
```js
const path = require('path');

module.exports = {
  //...
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          path.resolve('./src/my-style-loader.js'),
          'css-loader'
        ]
      }
    ]
  },
};
```

## 参考
> [编写一个 loader](https://www.webpackjs.com/contribute/writing-a-loader/)  
> [揭秘 webpack loader](https://zhuanlan.zhihu.com/p/104205895)
