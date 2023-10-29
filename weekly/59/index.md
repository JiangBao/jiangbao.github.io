# 兴趣周刊(第 59 期)


<!--more-->
{{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/jingshan1014.jpg" title="径山古道偶遇采蜜的蝴蝶">}}

## 代码那些事
* [真·技术分析瑞士轮抽签](https://m.hupu.com/bbs-share/62508080.html?share=share&euid=DFLitQFrO72B6OQctKiyIy4Fp7SOqdp6Bh8lvCbk/YM=&cid=139609826)  
英雄联盟 S13 进行中，今年新改了抽签规则，目前瑞士轮第一轮抽签结果已出，关于好签、坏签有不少的讨论，看到虎扑有 JR 写代码模拟了瑞士轮抽签，比较瑞士轮与小组赛对出线概率的影响，并证明第一轮对手完全不会影响出线概率，代码[在这](https://github.com/fudanchenjiahao/SwissGame)，可以帮忙分析正确与否。
{{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/293fddf0-18c7-4f68-8714-0ca5a6dbd99a.jpeg">}}

* [Node.js 21 来了](https://nodejs.org/en/blog/announcements/v21-release-announce)  
Node.js 团队在 10 月 17 日发布了 Node.js 21 版本，不得不说，这版本更新实在太快了，目前手头最新的项目还是用着 16.x，而目前 LTS 版本已经到了 18.x 😂。  
具体更新内容可见[官方文章](https://nodejs.org/en/blog/announcements/v21-release-announce)，主要的几处特性：  
  * JavaScript 引擎 V8 升级至 11.8
  * 测试运行器的多处更新
  * 稳定的 `WebStreams`，有助于在浏览器应用程序中缩减数据大小
  * `--experimental-default-type` 标志切换模块默认值

* [微信小程序遭批量破解的事件](https://developers.weixin.qq.com/community/develop/doc/00048c03f907002ccc7027a926b800)  
V 站看到的[帖子](https://www.v2ex.com/t/982914)，之前自己做小游戏时有过类似经历，产品有些数据量后，遭到别人疯狂上架数十款与你「相似」产品，最后搜索中自己的产品就被淹没了。  
对于这种严重破坏生态的违法行为，希望加强整治，别让劣币逐良币的事情做得太轻松。

* [微前端框架 MicroApp 1.0 正式发布](https://mp.weixin.qq.com/s/Tz4wIrpr10B10r7JWNqZPw)  
京东团队开源的微前端框架发布了 1.0 正式版，类 WebComponent 的设计思想，之前有项目使用了这个框架进行微应用拆分，还是很好用的。看了新的版本，接入方式更简单了，对 vite 的兼容也更好了。

## 其它的关注
* [RTX 4090 禁售风波](https://36kr.com/p/2481326502567814)  
各平台卖家：听说 RTX 4090 要被禁售？那我们集体囤货涨价一波吧。  
最近，美国商务部放出最严对华出口管制规定，受此影响，RTX 4090 全面下架，三方店铺遭到哄抢疯狂涨价，涨到了比买张机票去国外买还贵的程度。然后美国商务部紧急跳出来澄清：RTX 4090 不可在中国生产，但可销售。两点感受：
  * 理性看待任何「自由」贸易  
  * 囤积居奇让人反感，我没信心能抢过其他人，就像数月前的退烧药 🐶

* [微博测试访客记录功能，目前仅SVIP和VVIP可查看](https://www.donews.com/news/detail/8/3741075.html)  
微博上线访客记录功能，目前仅 SVIP 和 VVIP 可以看到，每日 8 点更新访客信息，博主能看到按粉丝、非粉丝、认证用户分类查看。这波操作一旦大规模上线，估计会流失不少用户（虽然我 6 年前就已不是微博用户）。
