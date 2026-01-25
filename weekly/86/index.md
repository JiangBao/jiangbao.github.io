# 兴趣周刊(第 86 期)


<!--more-->
{{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/xihu2018.jpeg" title="西湖雪景（2018.01）">}}

> 本周杭州寒潮，迎来了一次降雪，想起了之前一次印象深刻的大雪，早上 5 点钟就跑去西湖边赏雪。翻了翻照片，竟然已经是 8 年前的事了

## 代码那些事
* [Vercel 开源 react-best-practices Skill](https://vercel.com/blog/introducing-react-best-practices)  
Vercel 于 2026 年 1 月 14 日正式发布并开源了 `react-best-practices` 仓库，将其 10 余年 React 和 Next.js 优化经验封装为AI Agent 专用技能包，包含了 45+ 条规则，覆盖 8 大性能类别，准备参考他们的实践，定义一个内部项目的 Skill

* [json-render: AI -> JSON -> UI 的可控式界面生成](https://json-render.dev/)  
还是 Vercel，2026 年 1 月 15 日开源了 [json-render](https://github.com/vercel-labs/json-render)，主要通过「约束 -> 生成 -> 渲染」的流程，为 AI 生成 UI 添加安全护栏，让 AI 通过自然语言生成 json，再按约定的 jsonSchema 渲染样式，解决传统 AI 直接生成代码的不可控、不安全、难维护问题。  
目前还是在本地跑了示例，有点像传统低代码的大模型增强版，更多的优越性和应用场景还得再探索探索
  ```
  ┌─────────────┐     ┌──────────────┐     ┌─────────────┐
  │ User Prompt │────▶│  AI + Catalog│────▶│  JSON Tree  │
  │ "dashboard" │     │ (guardrailed)│     │(predictable)│
  └─────────────┘     └──────────────┘     └─────────────┘
                                                  │
                      ┌──────────────┐            │
                      │  Your React  │◀───────────┘
                      │  Components  │ (streamed)
                      └──────────────┘
  ```

* [jQuery 4.0.0 发布](https://blog.jquery.com/2026/01/17/jquery-4-0-0/)  
2026.01.17 jQuery 发布了 4.0.0 版本，对，你没听错，是 jQuery，青春回来了，当然也看到不少人在回复：最近才维护完一个古老项目，用的 jQuery 🐶  
在博客中，晒出了 jQuery 20 年聚会的合照，真心为这么长时间做好一件事情感到开心
{{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/jquery4.0.jpg">}}

* [Vibe designing with precision: pencil.dev](https://www.pencil.dev/)  
本周发布的 AI 设计类产品 [Pencil](https://x.com/tomkrcha/status/2014028990810300498)，又是一阵爆火，只能说看着宣传视频太酷了，自然语言 -> 无限画布 -> UI 生成/调整 -> 代码生成。学习的速度完全跟不上新产品发布的速度，感觉每天一睁眼都能看到让人眼前一亮的新产品 👍🏻  
{{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/pencildev.jpg">}}

## 其它的关注
* [🌹 芝加哥退役罗斯公牛 1 号球衣](https://x.com/chicagobulls/status/2015284932768129141)  
无数青春回忆 🌹1️⃣  
太久没看 NBA 比赛了，为了这场球衣退役仪式，看了整场比赛（以为会在中场举行，没想到是赛后），赫尔特的三分绝杀给这个夜晚增加了更深的记忆，仿佛写好的剧本一般
{{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/derricroseretirement.jpg">}}

