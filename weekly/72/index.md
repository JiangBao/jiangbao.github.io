# 兴趣周刊(第 72 期)


<!--more-->
{{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/sunset20240914.jpg" title="夕阳时刻">}}

## 代码那些事
* [得物商家客服从 Electron 迁移到 Tauri 的技术实践](https://mp.weixin.qq.com/s/UxmJxU4-fv9GeRxl2fzOGw)  
数据提升很明显，能够看出 Tauri 相比 Electron 的明显优势，但同时生态问题、使用 Rust 的技术挑战等问题，依然是很多团队考虑的。  
Electron 目前在桌面端有足够多的应用，社区比较完善，大部分团队会选择在此基础上调优，而非如此激进地做技术选型改变，不过有这样成熟团队的迁移分享，应该能给其它项目带来很多参考。
  * 包体积 7M，Electron 80M 下降91.25%
  * 平均内存占用 249M Electron 497M 下降49.9%
  * 平均 CPU 占用百分比 20%，Electron 63.5% 下降 63.19%


* [ECMAScript 2024 新特性解读](https://mp.weixin.qq.com/s/sXQeojB36dAYuYsmS1clfA)  
[ECMAScript 2024](https://tc39.es/ecma262/2024/) 语言规范的最终版本于 6 月 26 日获得批准，一起看看这个版本新增了哪些提案吧。
{{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/ecma2024.png">}}

* [paint-board: 一款支持多终端的 web 画板应用](https://songlh.top/paint-board/)  
开源产品，简单好用的 Web 端创意画板，也支持移动端，[开源地址](https://github.com/LHRUN/paint-board)
{{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/paintboard.png">}}

* [Isocons: 一组 3D 图标库](https://www.isocons.app/)  
可以微调样式之后，导出成 svg 或者 png 格式，也提供了 Figma 插件，如果刚好想要此类风格的图标，或许可以到这找找。
{{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/isocons.png">}}

* [一名即将从 Q 音离职员工的总结](https://blog.ursb.me/posts/2024-summer)  
缘起于 V 站看到的一篇帖子：[《毕业 4 年在 Q 音干到 T11，准备跑路了，谈一谈想法》](https://www.v2ex.com/t/1069344)，非常优秀的大厂员工，文笔也很好，作者聊到了自己主动离职的很多思考：工作激情在消退、成长不明朗、想跳出舒适圈，寻找更多的可能性等等。  
优秀的年轻人真是让人自惭形秽，对比起来自己毕业那几年还是思考得太少了 😂，所以回顾起来时常有种「搞砸了」的感觉。文中类似 Google SRE 工程师 [Postmortem of my 9 year journey at Google](https://www.tinystruggles.com/posts/google_postmortem/) 这篇文章的职业生涯分析报告很有意思 👍🏻，每个人都可以偶尔去完善一下自己的分析报告。

## 其它的关注
* ['Slop' and 'Content'](https://pxlnv.com/blog/slop-and-content/?ref=letters.geekplux.com)  
看到一篇对于网络热词「Slop」的解读，其实在父母和孩子的日常 app 使用中，我也一度很焦虑，他们对网络不熟悉，属于很被动的网络内容消费者，经常收到算法推送的各类无效甚至是垃圾的信息，时常很焦虑他们被这些垃圾信息「毒害」。  
我自己作为对网络很熟悉的用户，也经常在搜索问题时找到一堆没啥营养的内容，目前 AI 盛行的时代，有更甚的趋势。其实诸如 B 站、抖音之类的平台还是有不少高质量内容的，但偏偏算法总想推有热度、有广告、自认为你喜欢的内容给你，所以各类没营养、蹭热度的视频总是占满你的首屏 😰，造成劣币逐良币的趋势。  
现在的网络平台，资源很丰富，却也很贫瘠，每条消息都需要好好甄别，逃脱圈养也是件辛苦的事情。
  > Not all promotional content is spam, and not all AI-generated content is slop. But if it’s mindlessly generated and thrust upon someone who didn’t ask for it, slop is the perfect term for it. 

* [风城玫瑰，35 岁的德里克·罗斯发长文向篮球生涯深情告别](https://news.sina.com.cn/o/2024-09-26/doc-incqnvur1790621.shtml?cre=tianyi&mod=pcspth&loc=1&r=0&rfunc=22&tj=cxvertical_pc_spth&tr=12)  
罗斯的职业生涯真是让人唏嘘，前几年太过绚烂：NBA 状元、NBA 历史最年轻的 MVP，感觉未来充满了无限可能；然而瞬间坠落，后面十几年职业生涯，一直在追逐顶峰的自己。  
罗斯星光璀璨的时候，正是我对 NBA 和篮球最狂热的高中时代，后来到大学，再到工作，慢慢不再关注 NBA，很少再打篮球了，上一次看到罗斯的消息好像还是他在森林狼砍下 50 分后掩面而泣的新闻，如今看到他退役的新闻，仿佛也是跟自己青春时代一段记忆告别了。  
想到《飞驰人生 2》中张弛的一句台词：我努力过无数次了，但我明白，机会只会存在于其中的一两次。每个人应该都幻想过人生中一段「如果」的故事，但其实越活越明白：花有重开日，人无再少年 🌹  
{{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/roseretire.jpg" width="500">}}

