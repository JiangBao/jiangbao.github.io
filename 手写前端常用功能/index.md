# 手写前端常用功能


<!--more-->

## 1. new
* 思路  
  js的`new`zw运算符用于创建对象的实例，使用`new`的时候主要有以下操作：
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

## 2. 节流throttle

## 3. 防抖debounce

## 4. bind

## 5. call

## 6. apply

## 7. 深拷贝

## 8. 手写Promise

