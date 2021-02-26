# Webpack 自定义plugin


<!--more-->
{{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/webpack.png" >}}

> 插件向第三方开发者提供了webpack引擎中完整的能力。使用阶段式的构建回调，开发者可以引入他们自己的行为到webpack构建流程中。

webpack在编译过程中，会触发一系列Tapable钩子事件，注册相应的钩子，在webpack构建过程中触发钩子执行自定义的任务，就形成了我们自己的plugin。

## 背景
1. 插件组成
    * 一个JavaScript命名函数
    * 在插件函数原型上定义一个`apply`方法
    * 注册一个webpack自身的事件钩子
    * 处理webpack内部实例的特定数据
    * 功能完成后调用webpack提供的回调

2. 执行流程
    * 合并命令行参数与配置文件，解析得到参数对象
    * 参数对象传给webpack得到`Compiler`对象
    * 执行`Compiler`的`run`方法，生成`Compilation`对象
    * 触发 `Compiler` 的 `make`方法分析入口文件，调用 `compilation` 的 `buildModule` 方法创建主模块对象
    * 生成入口文件AST，加载依赖模块
    * 模块分析完成后，执行`Compilation`的`seal`方法
    * 最后执行`Compilation.emitAssets()`输出文件
      > [参考这个图示](https://zhuanlan.zhihu.com/p/102917655)
      > ![webpack](https://pic4.zhimg.com/80/v2-8eb6ec78f33bb6a7c217df16dc2a06bf_1440w.jpg)

3. Compiler与Compilation
    * Compiler：完整的webpack配置环境。这个对象在启动webpack时被一次性创建，并配置好所有可操作的设置，包括options、loader、plugin。当在webpack环境中应用一个插件时，插件可通过compiler对象引用来访问webpack主环境。
    * Compilation：一次资源版本的构建。开发环境每当检测到问价变化，就会创建新的compilation。

## 示例
[参考官网](https://www.webpackjs.com/contribute/writing-a-plugin/#%E7%A4%BA%E4%BE%8B)，实现一个简单的插件：生成`.md`文件展示构建生成的文件列表。[github仓库](https://github.com/JiangBao/webpack-notes/tree/main/plugin/filelist)  
`file-list-plugin.js`:
```js
function FileListPlugin(options={}) {
  this.filename = options.filename ? options.filename : 'filelist.md';
}

FileListPlugin.prototype.apply = function(compiler) {
  // 注册输出文件之前的钩子 emit 
  compiler.hooks.emit.tapAsync('FileListPlugin', (compilation, cb) => {
    // 从compilation.assets获取文件信息
    const len = Object.keys(compilation.assets).length;
    let filelist = `${len} file${len > 1 ? 's': ''} in this build:\n\n`;
    for (let filename in compilation.assets) {
      filelist += `* ${filename}\n`;
    }

    // 将列表作为新的文件资源，插入到webpack构建
    compilation.assets[this.filename] = {
      source: function() {
        return filelist;
      },
      size: function() {
        return filelist.length;
      }
    }

    cb();
  });
}

module.exports = FileListPlugin;
```
`webpack.config.js`:
```js
module.exports = {
  //...
  plugins: [
    new FileListPlugin({filename: '_filelist.md'}),
  ],
};
```

## 参考
> [编写一个插件](https://www.webpackjs.com/contribute/writing-a-plugin/)  
> [揭秘webpack plugin](https://zhuanlan.zhihu.com/p/102917655)
