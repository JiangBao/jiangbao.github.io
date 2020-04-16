# Node.js核心模块—Events

`events`是非常重要的Node.js核心模块，大多数Node.js核心API构建于惯用的异步事件驱动架构，发布/订阅模式，触发器通过触发命名事件来调用监听器，比如常见的net服务`net.Server`会在新连接建立时触发`connection`事件。

## 简单使用
>所有能触发事件的对象都是`EventEmitter`类的实例。 这些对象有一个`eventEmitter.on()`函数，用于将一个或多个函数绑定到命名事件上。事件的命名通常是驼峰式的字符串，但也可以使用任何有效的`JavaScript`属性键。  
>当`EventEmitter`对象触发一个事件时，所有绑定在该事件上的函数都会被同步地调用。被调用的监听器返回的任何值都将会被忽略并丢弃。
```js
const EventEmitter = require('events');

class MyEmitter extends EventEmitter { }

const myEmitter = new MyEmitter();

myEmitter.on('someEvent', () => {
  console.log('get someEvent!');
});

myEmitter.emit('someEvent');
```

## 错误处理
当`EventEmitter`实例出错时，应该触发`error`事件，需要为`error`时间注册监听器，防止错误事件导致进程崩溃
```js
myEmitter.on('error', (err) => {
  console.error(`something error: ${err.stack}`);
});

myEmitter.emit('error', new Error('this is a error'));
```

## 数量限制
默认每个事件最多注册10个监听器，事件对象有`EventEmitter.defaultMaxListeners`属性，但修改这个值会影响所有`EventEmitter`实例，为了改变单个实例的限制，可以使用`emitter.setMaxListeners(n)`方法

## 异步 vs 同步
`EventEmitter`以注册顺序同步调用所有监听器，确保事件的正确排序。可以使用`setImmediate()`和`process.nextTick()`切换到异步模式操作
```js
const myEmitter = new MyEmitter();
// 默认同步
myEmitter.on('event', function firstListener() {
  console.log('第一个监听器');
});
myEmitter.on('event', function secondListener(arg1, arg2) {
  console.log(`第二个监听器中的事件有参数 ${arg1}、${arg2}`);
});
myEmitter.on('event', function thirdListener(...args) {
  const parameters = args.join(', ');
  console.log(`第三个监听器中的事件有参数 ${parameters}`);
});
myEmitter.emit('event', 1, 2, 3, 4, 5);

// 异步操作
myEmitter.on('event', (a, b) => {
  setImmediate(() => {
    console.log('异步操作', a, b);
  });
});
myEmitter.emit('event', 'a', 'b');
```

## 基于EventEmitter实现自定义任务模块
之前对于游戏服务器的任务模块，使用了基于事件的发布/订阅模式进行了重构，减少了一些逻辑耦合和回调地狱问题。
```js
const util = require('util');
const EventEmitter = require('events');

function Mission() {
  EventEmitter.call(this);
}

util.inherits(Mission, EventEmitter);


const mission = new Mission();

function handleMission(missionId, progress) {
  console.log(`id:${missionId}, progress is ${progress}`);
}

mission.on('mission1', function(progress) {
  handleMission('mission1', progress)
})

mission.emit('mission1', 123);
```
