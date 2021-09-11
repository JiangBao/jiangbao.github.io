# 使用 Promise 解决回调地狱问题


<!--more-->

JavaScript中有许多异步的操作，原生使用回调方法来实现异步。很多时候在维护一系列异步操作的执行顺序时会形成「回调地狱」，代码难以阅读。

之前接手的一个团队内部的老项目，使用Node.js开发，用的就是`error-first callback`风格的代码，我和同事接手后使用的都是ES6新特性——Promise来处理异步逻辑，所以最近我对一些公共方法进行了Promise化处理，虽然使用类似`bluebird`这样的库很容易实现，但这么简单的需求当然选择造一下轮子。

编写`promisify`公共方法，对旧代码目标异步函数做Promise化处理。

show me the code:

```js
function promisify(func) {
  return function (...args) {
    return new Promise((resolve, reject) => {
      function callback(err, res) {
        if (err) {
          reject(err);
        } else {
          resolve(res);
        }
      }

      args.push(callback);

      f.call(this, ...args);
    });
  };
}
```
