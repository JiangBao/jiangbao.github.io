# 一次JS连续时间判断问题的排查

最近接手的一个业务，历史遗留下关于连续登录天数功能的bug，历史代码有一项判断是否连续登录天数的逻辑，正好复习一下JS关于时间的操作。

<!-- more -->

在之前自己使用过`moment`库实现过类似功能：
```javascript
function isContinuous(localTime) {
  let yesterday = moment().subtract(1, 'days').startOf('day');
  return moment(localTime).isSame(yesterday, 'd')
}
```
这次接手的代码使用原生JS实现，比较记录的两个时间的连续性，类似：
```javascript
function isContinuous(prevLocalTime, nextLocalTime) {
  let prevDate = new Date(prevLocalTime);
  let nextDate = new Date(nextLocalTime);
  nextDate.setDate(nextDate.getDate() - 1);
    
  return prevDate.getTime() === nextDate.getTime();
}
```
测试发现，入参处：前一次登录时间prevLocalTime保存为`YYYY-MM-DD`的格式，但是作为比较的下次登录时间使用`new Date().toLocaleDateString()`获取时间日期字符串，所以又是JS比较坑人的时间字符串环节。

首先看看`toLocaleDateString`方法获取的入参问题：
> `toLocaleDateString()`方法返回该日期对象日期部分的字符串，该字符串格式因不同语言而不同。新增的参数`locales`和`options`使程序能够指定使用哪种语言格式化规则，允许定制该方法的表现`（behavior）`。在旧版本浏览器中，`locales`和`options`参数被忽略，使用的语言环境和返回的字符串格式是各自独立实现的。
> from: [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleDateString)

所以这个方法获取时间日期字符串最大的问题是不同环境下得到的结果是不同的，当获取到`YYYY/MM/DD`这样的格式，不用于记录中保存的`YYYY-MM-DD`格式，使用`Date`对象时，在ES5标准下，没有提供时区的ISO 8601标准字符串都会被默认为标准时区，简而言之，`YYYY-MM-DD`和`YYYY/MM/DD`使用Date对象生成的时间字符串是不同的。

结论：由于`toLocaleDateString`方法的不可靠性，需要注意获取参数与记录参数保持一致性，此外，仅针对这段判断代码，因为比较前只做了日期的同步，根据`Date`对象导致的问题，可以额外针对 时、分、秒、毫秒单位进行同步，也可以解决
```javascript
function isContinuous(prevLocalTime, nextLocalTime) {
  let prevDate = new Date(prevLocalTime);
  let nextDate = new Date(nextLocalTime);
  nextDate.setDate(nextDate.getDate() - 1);

  prevDate.setHours(0);
  prevDate.setMinutes(0);
  prevDate.setSeconds(0);
  prevDate.setMilliseconds(0);
  nextDate.setHours(0);
  nextDate.setMinutes(0);
  nextDate.setSeconds(0);
  nextDate.setMilliseconds(0);
    
  return prevDate.getTime() === nextDate.getTime();
}
```

