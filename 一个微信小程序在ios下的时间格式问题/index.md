# 一个微信小程序在IOS下的时间格式问题


<!--more-->

**背景：** 最近使用uni-app开发了一个微信小程序，其中封装了`picker`组件做了个日期-时间选择器，在开发者工具和安卓设备下工作都正常，但是在iPhone真机下调试发现工作不生效。

**机制：** 自定义组件封装了`picker`子组件，主要监听`picker`组件的`change`事件，向顶层传递变化后的时间，其中格式化时间戳的时候，使用了`Date.parse('yyyy-MM-dd hh:mm:ss')`的方法。调试发现，各平台下，组件间数据传递都没有问题，但是在iPhone真机下，格式化时间会出现`Invalid Date`的错误。

**问题：** safari浏览器使用`new Date()`解析日期时，不支持`yyyy-MM-dd 或 yyyy.MM.dd`这样的格式，参考[Stack Overflow上这个问题](https://stackoverflow.com/questions/4310953/invalid-date-in-safari)。但是`yyyy/MM/dd`这样的格式是chrome和safari都支持的，所以将原先时间字符串格式化为`yyyy/MM/dd`的格式即可解决问题。

**解决：** `Date.parse('yyyy-MM-dd hh:mm:ss'.replace(/-/g, '/'))`
