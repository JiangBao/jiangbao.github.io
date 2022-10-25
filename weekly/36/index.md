# 兴趣周刊(第 36 期)


<!--more-->

## 代码那些事
* 谷歌发布新的开源操作系统 [KataOS](https://opensource.googleblog.com/2022/10/announcing-kataos-and-sparrow.html)，用于进行机器学习的嵌入式设备的开源操作系统，几乎全由 Rust 实现，新坑来了。
* [一份 JavaScript 使用报告](https://almanac.httparchive.org/en/2022/javascript)，从加载数量、请求数，到常用库等等分析，前端从业者可以看看。最近在做一个历史项目的性能优化工作，对于文中提到的「大量被加载但是未使用的 JavaScript」深有感触。
* [React 渲染的未来](https://mp.weixin.qq.com/s/8j1-ZT_dfzHf9NA4KNuaNQ)，讨论了 React 当前的渲染模式、它们存在的问题，以及 React 18 引入的新模式如何是解决这些问题的。
* [微前端如何做样式隔离](https://mp.weixin.qq.com/s/AYI0FGFBOG1QAlbo5qVixQ)，目前比较常用的还是 css Module + 协商命名规则，本文也讨论了 Shadow DOM 实现完全隔离样式。
* [ViteConf 2022 回顾](https://viteconf.org/2022/replay)，可以看看尤雨溪《How Vite Came to Be》的演讲，了解 Vite 的诞生历程。

## 其它的关注
* s12 小组赛第二轮，[TES 输给越南战队 GAM，止步 16 强](https://www.163.com/dy/article/HJPMAQ0D0546TPMR.html)，坏消息是 LPL 二号种子被淘汰了，好消息是他们四点多就输了，国内观众不用早起看剩余比赛了。网友已热情为他们规划了游泳回国的路线。  
  出线的三支 LPL 战队，大部分都感染了新冠，希望快速恢复，保持健康，发挥出应有的水平。
* 随着 [iPad10](https://www.apple.com.cn/ipad-10.9/) 发布，苹果中国对 iPad 全线大涨价。望着手中的 iPad Pro 2018，真香。  
  iPad 10 已经用上了极其先进的 C 口，再搭配 lighting 接口一代笔库存，还能带动转接头的销量，实在高明 🐶🐶🐶  
  ![ipad10](https://pbs.twimg.com/media/FfZdBYQakAEVWaV?format=jpg&name=large)
* 最近被 AI 画画刷屏，这是一个 [基于 NovelAI 的画图机器人](https://github.com/koishijs/novelai-bot)，已实现功能：
  * 绘制图片
  * 更改模型、采样器、图片尺寸
  * 高级请求语法
  * 自定义违禁词表
  * 发送一段时间后自动撤回
  * 连接到私服 · NAIFU
  * img2img · 图片增强

