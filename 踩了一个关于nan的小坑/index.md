# 踩了一个关于NaN的小坑

数值运算出错时，比如任何数除以0都会导致错误而终止程序运行，但是在JavaScript中不会影响程序执行，而是会返回一个特殊值--`NaN(Not a Number)`，表示本来要返回数值的操作数未返回数值的情况。最近在一个使用js开发的项目中，一不小心就踩到了一个关于`NaN`的坑。

<!--more-->

场景大致是一个计算某个道具的价值函数，伪代码如下：
```javascript
// 计算配置中某个id的道具价值
const getValue = (id, num=1) => {
  // 假设此处配置表中id道具价值存在
  const value = CONFIG[id].value;
  const totalValue = value * num;

  return totalValue;
};
```
道具id-数量kv值由上游提供，获取到道具价值后用于后续运算，后来在日志中发现了后续运算`NaN`的错误。上游传入的`num`参数有可能为`NaN`，最初认为默认赋值后即使上游传入的数量值不对也可有默认值，然后又继续愚蠢地加打印做测试：
```javascript
const getValue = (id, num=1) => {
  const value = CONFIG[id].value;
  const totalValue = value * num;

  // 在此处做了检测
  if (typeof totalValue != 'number') {
    console.error('something error!!!');
  }

  return totalValue;
};
```
果然在传入`num = NaN`时，默认参数赋值没生效，函数返回了`NaN`，并且也没有打印输出，然后试着用老办法看看：
```javascript
const getValue = (id, num) => {
  num = num || 1;
  const value = CONFIG[id].value;
  const totalValue = value * num;

  return totalValue;
};
```
这个时候，`num`参数在传入`NaN`时默认为1就可以生效，很有趣的坑，重新好好复习了一下js语法。总结一下犯的几个错误：
* `NaN`虽然是`Not a Number`的简写，但它依旧是`number`type的，所以`typeof NaN === 'number'`  
* ES6的默认参数赋值，看一下解释，指的是：允许在没有值或者`undefined`被传入时使用默认形参。而`NaN`确实js中一个真实值，所以之前使用默认参数显然没有达到屏蔽`NaN`参数的作用  
* 一切为了赶进度而放弃编写测试的习惯都是耍流氓，无论怎样，还是没做足够测试导致的问题

创业阶段，所有事情都在赶着做，自己代码缺乏足够测试，又没有专业QA功能验收，功能就被匆匆丢到线上，真是疲于应付各种线上问题了，希望自己能早点推动团队正常的开发、测试、发布流程。

