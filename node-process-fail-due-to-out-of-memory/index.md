# Node Process Fail Due to Out of Memory


<!--more-->

最近换了新的`MacBook M1`开发，一个旧项目运行出现了`out of memory`的错误：
![node out of memory](https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/node-out-of-memory.png)

> 环境
> * Node.js: v14.16.1
> * OS: macOS Big Sur with an M1 chip

找了一下，貌似还是`M1`芯片下比较常见的问题，参考[此处](https://github.com/TypeStrong/typedoc/issues/1491)，升级至`Node.js v15`解决了此问题。
