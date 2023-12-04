# 使用 Github Actions 部署 Hugo 博客

前不久将博客[从 hexo 转到了 hugo](https://jiangbao.github.io/%E4%BB%8Ehexo%E8%BD%AC%E5%88%B0hugo/)，之前的文章简单介绍了 hugo 的基本使用，这一阶段我发布新文章的流程是这样的：  

1. 生成新文章，`hugo new posts/xxxx.md`
2. 编辑文章内容
3. 提交到github，`git add, git commit`
4. 编译及部署，执行快速部署脚本`./deploy.sh`，将内容部署到 github pages

虽然已经使用脚本简化了编译部署流程，但还是抱着偷懒的想法，将它接入 CI/CD 工具，只要专注于博客仓库本身的更新，编译及部署这些事交给工具去做:rocket:。所以理想的流程应该是这样：

1. 生成新文章
2. 编辑文章内容
3. 提交到代码仓库

然后等待工具做完剩余事情就行了，我们自己团队工作流中目前使用 jenkins 比较多，但这种个人的 github 项目，有 Github Actions 这么好用的工具自然是要利用起来。

## 要做什么
回顾一下之前的构建部署流程：

1. 项目下执行 `hugo` 编译网站，结果在 `./public` 路径
2. 将 `./public` 设置为子模块，每次编译更新，提交到 Github Pages 的仓库[`JiangBao/jiangbao.github.io`](https://github.com/JiangBao/jiangbao.github.io)

之前的脚本 `deploy.sh` 做的也是这些事，这次要处理的就是将这个流程整理到Github Actions。

## 工作流程
{{< mermaid >}}
graph LR;
    A(提交到仓库) --> B(获取分支代码)
    B --> C(安装 hugo 环境)
    C --> D(编译)
    D --> E(部署 Github Pages)
{{< /mermaid >}}

其中使用到的actions模块：

1. 获取分支代码：[`actions/checkout@v2`](https://github.com/marketplace/actions/checkout)
2. 安装Hugo环境：[`peaceiris/actions-hugo@v2`](https://github.com/marketplace/actions/hugo-setup)
3. 编译：执行 `hugo --minify`
4. 部署到Github Pages：[`peaceiris/actions-gh-pages@v3`](https://github.com/marketplace/actions/github-pages-action)

## 注意事项
因为[博客代码仓库](https://github.com/JiangBao/jiangbao-hugo-blog)与[Github Pages 仓库](https://github.com/JiangBao/jiangbao.github.io)属于两个不同的仓库，所以在获取和推送代码时会遇到权限问题，所以在这还需要做的事情是：

1. 产生一组 ssh 公私钥:key:，[参考](https://github.com/peaceiris/actions-gh-pages#%EF%B8%8F-create-ssh-deploy-key)
2. 将私钥配置在博客代码仓库的 `Setting->Secrets`
3. 将公钥配置在 Github Pages 所在仓库的 `Deploy Keys`，并勾选 `Allow write access`
4. 使用 `peaceiris/actions-gh-pages@v3` action模块的 `deploy_key` 方式，[参考](https://github.com/peaceiris/actions-gh-pages#%EF%B8%8F-deploy-to-external-repository)

## 完整的 workflow
```yaml
name: Build and Deploy

on:
  push:
    branches:
      - master

jobs:
  build_deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v2
        with:
          submodules: true

      - name: Setup hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.70.0'
          extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }}
          external_repository: JiangBao/jiangbao.github.io
          publish_dir: ./public
          publish_branch: master
```

## 参考
> https://jimytc.com/posts/2020/02/16/setup_with_github_action/


