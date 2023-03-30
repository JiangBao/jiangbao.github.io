# 兴趣周刊(第 47 期)

{{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/20230326-DSC00837.jpg" title="无聊就去看看小动物吧，它们也很无聊 🐶">}}
<!--more-->

## 代码那些事
* [React 官方的新文档终于正式上线](https://react.dev/blog/2023/03/16/introducing-react-dev)  
在 Hooks 发布五年之后，React 官方终于正式上线了[新的官方文档 react.dev](https://react.dev/)，旧文档被移至[这个地址](https://legacy.reactjs.org/)。  
之前在 beta 阶段已经读过了大部分页面，使用 hooks 完善了所有的交互示例，内容比旧文档丰富很多，推荐仔细阅读。  
PS: 新的文档[快速启动项目章节](https://react.dev/learn/start-a-new-react-project) 推荐方式，CRA 消失了，出现了 Next.js 和 Remix，有点意思 🤔

* [字节跳动开源前端模块打包工具 Rspack: 基于 Rust，主打高性能](https://mp.weixin.qq.com/s/R-tjPrj2N2DKMO8_cPsp9Q)  
  > 基于 Rust 语言开发的 Web 构建工具，拥有高性能、兼容 Webpack 生态、定制性强等多种优点，解决了我们在业务场景中遇到的非常多的问题，让很多开发者的体验有了质的提升。  

  从 Turbopack 获取灵感的，兼容 Webpack 生态的，对 Webpack 深度优化的 Rust 版本，不知道这样理解是否正确。官方团队也提到：他们已经和 Webpack 团队确立了合作关系，Rspack 作为 Webpack 通过 Rust 进行性能优化的一个尝试，未来会和 Webpack 团队一起探索优化 Webpack 的更多可能性。

  Webpack、Vite、Turbopack、Rspack... 2023 年了，前端打包工具这个领域还这么卷吗？

* [OpenAI 官宣发布 GPT-4](https://twitter.com/OpenAI/status/1635688570710298625?s=20)  
  > GPT-4 has enhanced capabilities in:
  > - Advanced reasoning
  > - Complex instructions
  > - More creativity
  
  关于最近 ChatGPT 引发的产品爆炸，可以看看 [这个周刊](https://decohack.zhubai.love/posts/2244447748458225664) 整理的一些产品汇总。  
  我这个「周刊」还未发布，微软又接二连三发布了 [Microsoft 365 Copilot](https://blogs.microsoft.com/blog/2023/03/16/introducing-microsoft-365-copilot-your-copilot-for-work/)、[GitHub Copilot X](https://github.com/features/preview/copilot-x
)  
  势头这么猛，我还不想失业啊 🐶

* 你 ssh 登录的 GitHub 仓库还正常吗？如果出现了类似以下的警告，那就对啦，GitHub 官方[更新了 RSA SSH host key](https://github.blog/2023-03-23-we-updated-our-rsa-ssh-host-key/)。参考文章更新一下 `~/.ssh/known_hosts` 文件就可恢复正常啦。  
  ```shell
  DNS SPOOFING is happening or the IP address for the host
  and its host key have changed at the same time.
  @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
  @    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
  @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
  IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
  ```

* [微软开源项目：Visual ChatGPT，可以使用图像交互的方式，跟 ChatGPT 互动](https://github.com/microsoft/visual-chatgpt)  
最近 ChatGPT 相关话题热度实在太高，微软最近开源 Visual ChatGPT，直接霸榜 GitHub Trending。  
Visual ChatGPT 主要是将 ChatGPT 单纯的文字交互拓展为文字 + 图片的形式。感兴趣的可以前往 [官方地址](https://github.com/microsoft/visual-chatgpt) 体验一下。  
{{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/visual-chatgpt.jpg">}}

## 其它的关注
* [中国、沙特阿拉伯和伊朗三国在北京发表三方联合声明，同意恢复沙、伊双方外交关系](https://www.guancha.cn/internation/2023_03_11_683678.shtml)  
中东地区的大事件，可以说是中国外交的重大成果了。这才是大国应有的风范，没有制裁、没有飞机导弹，中国人骨子里的以和为贵，才应该是世界外交的主旋律，愿世界和平。

* [硅谷银行倒闭：恐慌挤兑一夜压垮](https://36kr.com/p/2169164640366850)  
上周四，硅谷银行母公司 SVB Financial 突然宣布低价出售可变现资产(亏损 18 亿美元)，释放信号：硅谷银行已经出现了严重的现金危机。该股股价当天就暴跌了60%。  
仅仅一天时间，挤兑浪潮就彻底压垮了硅谷银行。周五中午，FDIC 就紧急接管了硅谷银行，创建一个新实体圣克拉拉存款保险国家银行(DINSC)，将硅谷银行所有的存款都注入这个新实体。  
作为金融界门外汉，第一次真正认识硅谷银行，就是在它破产的时候 🐶

