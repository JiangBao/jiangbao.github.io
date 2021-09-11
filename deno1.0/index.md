# Deno1.0

{{< figure src="https://deno.land/v1_wide.jpg" >}}
虽然之前了解 Node.js 原作者Ry针对 Node 存在的问题，一直在折腾 Deno，但只简单看过前期版本，也不知道到底什么进度。今天看到[ Deno 发布了 v1.0 正式版本](https://deno.land/v1)，来简单入门一下，等待后续跟进🦕

## 安装
* shell安装
  ```bash
  curl -fsSL https://deno.land/x/install/install.sh | sh
  ```
* Homebrew
  ```bash
  brew install deno
  ```
* [更多安装方式参考](https://github.com/denoland/deno_install)

## 开始
* 运行一个简单的远端程序
  ```bash
  $ deno run https://deno.land/std/examples/welcome.ts

  Welcome to Deno 🦕
  ```
* hello world 的 web 服务器: 直接添加远程 HTTP 服务器模块依赖，无需任何本地配置  
  编写 `hello-deno.js` 文件，内容如下：
  ```js
  import { serve } from "https://deno.land/std@0.50.0/http/server.ts";
  for await (const req of serve({ port: 8000 })) {
    req.respond({ body: "Hello Deno\n" });
  }
  ```
  执行 `deno run --allow-net hello-deno.js`，即可在本地 8000 端口预览效果

## 标准库
* [API文档](https://doc.deno.land/https/github.com/denoland/deno/releases/latest/download/lib.deno.d.ts)
* [标准模块](https://deno.land/std)
* [第三方模块](https://deno.land/x)

## 想法
* 对 ES 新特性，Promise 和模块有更好的支持；安全沙箱机制
* 看起来极具 Golang 风格，依赖去中心化，终于是丢开了沉重的 node_modules，可是依赖管理如何进一步优化？之前使用 Golang 做企业级开发总觉得包管理的方式实在粗糙
* 强硬地与 Node.js 区分开了，目前对 Node 包是不支持的。  
  我还需要更深入学习到底解决了哪些痛点问题，目前个人感觉替代原先的 Node.js 还是比较困难的，Node.js目前在大前端领域分量太重了，不过后面会持续关注，等待成熟。当初 Node.js 也是想让 JS 进入后端领域，结果导致了前端生态的繁荣，不知道 Deno 成熟起来又会在哪发光发热:confused:，结合作者的个人经历，难道前端玩转 AI 指日可待？
* 对比一下 Node.js 与 Deno 的官方描述
  > **Node**: Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine.  
  > 
  > **Deno**: A secure runtime for JavaScript and TypeScript.

  比较直观说明会对 TypeScript 有更友好的开箱即用支持，这应该属于历史必然原因，这两年 TS 如此火热，解决了不少脚本小子被诟病的痛点问题。  
  *但看[发布文章](https://deno.land/v1#tsc-bottleneck)的描述，目前 Deno 内部使用微软的 TypeScript 编译器检查类型、生成 JS 的速度非常慢，需要性能上的提升还得将 TSC 移植到 Rust。而这又将是个时间漫长的坑*
