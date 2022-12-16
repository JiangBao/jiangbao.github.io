# Turbopack Is Coming

{{<figure src="https://vercel.com/_next/image?url=https%3A%2F%2Fimages.ctfassets.net%2Fe5382hct74si%2F5xISJZLpC7OKEDNRII3r8T%2F491842a26bbc8ed1a976393dc42e2755%2FFrame_427319031.png&w=3840&q=75">}}
<!--more-->

最近 Vercel 开源了新的打包工具 [Turbopack](https://turbo.build/)，该项目由 `Webpack` 创建者 Tobias Koppers 牵头，基于 [Rust](https://www.rust-lang.org/) 实现。

## 背景
在此之前，Vercel 已经做过很多类似工作：使用 [SWC](https://swc.rs/) 替换 [Babel](https://babeljs.io/)，使转译速度提升了 17 倍；替换 [Terser](https://terser.org/)，使压缩速度提高了 6 倍。而这一次，终于轮到 `Webpack` 了，`Webpack` 目前已成为前端构建的默认标准，但是业界普遍对大而臃肿的配置和构建速度声讨声很大。 

Vercel 在[文章](https://vercel.com/blog/turbopack)中称「在大型应用程序上，Turbopack 更新速度比 Vite 快 10 倍，比 Webpack 快 700 倍，在更大的应用程序上，差异更大」。   
> Using the Turbopack alpha with Next.js 13 results in:  
> * **700x** faster updates than Webpack  
> * **10x** faster updates than Vite  
> * **4x** faster cold starts than Webpack

700 倍...确实是非常夸张的数据，可以感受到天下苦 `Webpack` 久矣...😂😂😂

## 核心概念
核心：基于 Rust 的增量计算引擎 [turbo](https://turbo.build/pack/docs/core-concepts#the-turbo-engine)，精准到函数级别的缓存机制。

当进行构建时，命中缓存的函数不会重复运行，这种细粒度的结构可以使程序在函数级别避免很多重复工作。

对应 `package.json --> scripts` 中 `build` 脚本，可以在 [turbo.json](https://turbo.build/repo/docs/reference/configuration) 中配置对应的 `tasks`:
```json
{
  "pipeline": {
    "build": {
      "outputs": [".next/**"]
    }
  }
}
```
执行 `turbo run build`，没有命中缓存，生成新的输出文件，并缓存至 `./node_modules/.cache/turbo/**`
{{<figure src="https://turbo.build/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fcache-miss.21d45e92.png&w=3840&q=75" title="cache miss, from turbo.build">}}
不修改源码，再次执行编译，此时可以命中缓存，直接使用缓存数据，速度起飞 🚀
{{<figure src="https://turbo.build/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fcache-hit.3bac1eb9.png&w=3840&q=75" title="cache hit, from turbo.build">}}

## 简单使用
### 从现有脚手架创建项目
通过 `create-next-app` 脚手架新建一个 `Next.js` 项目，安装 `turbo` 依赖后，项目根目录添加 `turbo.json` 配置文件即可开始 `turbopack` 打包：
```json
{
  "pipeline": {
    "build": {
      "outputs": [".next/**"]
    },
    "lint": {
      "outputs": []
    }
  }
}
```
不改变源文件场景时，重复打包两次(`npx turbo build`)，可以感受到缓存带来的快速打包体验：
```shell
 Tasks:    2 successful, 2 total
Cached:    2 cached, 2 total
  Time:    197ms >>> FULL TURBO
```
{{< admonition type=tip title="cache miss 问题" details=false >}}
按照[官方文档说明](https://turbo.build/repo/docs/getting-started/add-to-project)操作，出现了 `cache miss` 问题，重复打包一直没使用缓存，后来发现是新建项目后习惯性使用了 `yarn` 安装依赖，但测试时依然使用 `npx turbo build lint` 命令导致
{{< /admonition >}}

### 新建 Monorepo
通过命令行 `npx create-turbo@latest` 新建 monorepo 项目，`/apps/docs` 和 `/apps/web` 是两个基于 `Next.js` 的应用，`/packages/ui` 是本地组件。  
通过根目录下 `turbo.json` 配置打包，默认配置了 3 个 task，每个 task 对应根目录下 `package.json` 中的 `scripts`。执行 `pnpm run dev` 会分别执行 `/apps/docs` 和 `/apps/web` 对应的 `dev` 脚本，`build` 同理。
```json
{
  "$schema": "https://turborepo.org/schema.json",
  "pipeline": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": ["dist/**", ".next/**"]
    },
    "lint": {
      "outputs": []
    },
    "dev": {
      "cache": false
    }
  }
}
```

{{< admonition type=tip title="Module parse failed" details=false >}}
创建新的 monorepo，执行 `pnpm run dev` 时，出现本地模块解析问题：
```shell
docs:dev: error - ../../packages/ui/Button.tsx
docs:dev: Module parse failed: Unexpected token (3:9)
docs:dev: You may need an appropriate loader to handle this file type, currently no loaders are configured to process this file. See https://webpack.js.org/concepts#loaders
docs:dev: | import * as React from "react";
docs:dev: | export const Button = () => {
docs:dev: >   return <button>Boop</button>;
docs:dev: | };
docs:dev: |
```
通过 [next-transpile-modules](https://www.npmjs.com/package/next-transpile-modules) 解决，`/apps/web/next.config.js` 和 `/apps/docs/next.config.js` 加上 `next-transpile-modules` 配置。如果有遇到相同问题，可以参考此解决方式。
{{< /admonition >}}

## 我的想法
`Vercel` 团队，加上 `SWC`、`Webpack` 等核心成员的加入，我觉得 `Turbopack` 还是值得期待的，它是继 `Next.js 11.x` 使用 `SWC` 之后，向 `Rust-based` 工具链生态的更进一步探索。

目前 `Next.js 13` 版本已经集成了 `Turbopack`，甚至基于 `Vercel` 生态的远程缓存能力，团队内可以共享 turbo 缓存。Next.js 生态无疑是很丰富的，但是想要突破 `Webpack`，`Turbopack` 还需辐射到 `Next.js` 之外的生态，所以期待插件机制早日完善。

前端工具链正在经历一波 `go`、`Rust` 生态的重构，你已经在学习了吗...

