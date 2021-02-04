# 手写前端常用功能


<!--more-->

## 1. new
* 思路  
  js的`new`运算符用于创建对象的实例，使用`new`的时候主要有以下操作：
    1. 创建空对象`{}`
    2. 链接该对象(设置该对象的`constructor`)到另一个对象
    3. 将步骤1新创建的对象作为`this`的上下文
    4. 如果该函数没有返回对象，则返回`this`

* show me the code
  ```js
  function customizeNew(Fn, ...args) {
    // 创建空对象并链接到原型
    let obj = Object.create(Fn.prototype);
    // 绑定this
    let res = Fn.apply(obj, args);

    return res instanceof Object ? res : obj;
  }
  ```
## 2. 手写Promise
* 思路  
  `Promise`主要解决传统异步编程中“回调地狱”导致代码阅读性和维护性差的问题，使用链式风格替代传统的嵌套回调风格，是更优雅的异步编程解决方案。
  根据「Promise/A+规范」，`promise`有三个状态：`pending`(待定)、`fulfilled`(已兑现)、`rejected`(已拒绝)，需要实现的基础功能包括：
    1. `new Promise()`参数需要一个处理器函数`executor()`
    2. 处理器函数的参数为`resolve`和`reject`，成功调用`resolve`函数，失败调用`reject`函数
    3. 默认状态为`pending`，只能从`peding`到`fulfilled`或`rejected`，且状态确认后不可再改变
    4. 包含`then`方法，接收`onFulfilled`和`onRejected`参数，成功执行`onFulfilled`，失败执行`onRejected`

    {{<figure src="https://mdn.mozillademos.org/files/8633/promises.png">}}

* show me the code
  ```js
  const STATUS = {
    PENDING: 'PENDING',
    FULFILLED: 'FULFILLED',
    REJECTED: 'REJECTED',
  };

  class MyPromise {
    constructor(executor) {
      this.status = STATUS.PENDING;             //默认状态pending
      this.value = undefined;                   //fulfilled状态时的值
      this.error = undefined;                   //rejected状态时的错误原因
      this.onResolvedCallbacks = [];            //存放成功的回调
      this.onRejectedCallbacks = [];            //存放失败的回调
      this._resolve = this._resolve.bind(this);
      this._reject = this._reject.bind(this);

      try {
        executor(this._resolve, this._reject);
      } catch (e) {
        this._reject(e);
      }
    }

    _resolve(value) {
      setTimeout(() => {
        this.value = value;
        this.status = STATUS.FULFILLED;
        this.onResolvedCallbacks.forEach(fn => fn(value));
      })
    }

    _reject(error) {
      setTimeout(() =>  {
        this.error = error;
        this.status = STATUS.REJECTED;
        this.onRejectedCallbacks.forEach(fn => fn(error));
      })
    }

    then(onFulfilled, onRejected) {
      return new MyPromise((resolve, reject) => {
        this.onResolvedCallbacks.push((value) => {
          try {
            const res = onFulfilled(value);
            if (res instanceof MyPromise) {
              res.then(resolve, reject);
            } else {
              resolve(res);
            }
          } catch (e) {
            reject(e);
          }
        });

        this.onRejectedCallbacks.push((value) => {
          try {
            const res = onRejected(value);
            if (res instanceof MyPromise) {
              res.then(resolve, reject);
            } else {
              reject(res);
            }
          } catch (e) {
            reject(e);
          }
        })
      })
    }
  }
  ```

## 3. 节流throttle

## 4. 防抖debounce

## 5. bind

## 6. call

## 7. apply

## 8. 深拷贝

> [Promise|MDN](https://developer.mozilla.org/zh-cn/docs/Web/JavaScript/Reference/Global_Objects/Promise)  
  [juejin|iboying](https://juejin.cn/post/6873513007037546510)  
  [github|sisterAn](https://github.com/sisterAn/JavaScript-Algorithms)
