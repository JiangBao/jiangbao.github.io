# 从Hexo转到Hugo

{{< figure src="https://d33wubrfki0l68.cloudfront.net/c38c7334cc3f23585738e40334284fddcaf03d5e/2e17c/images/hugo-logo-wide.svg" >}}
<!--more-->
> Hugo is one of the most popular open-source static site generators. With its amazing speed and flexibility, Hugo makes building websites fun again.  

[Hugo](https://gohugo.io/)是一个基于Go语言开发的静态网站生成器，主打简单、易用、高效、易扩展、快速部署，丰富的主题也使得Hugo也在个人博客站点搭建方面也使用广泛。之前自己的静态博客使用[Hexo](https://hexo.io/)搭建，Hexo是一个比较广泛使用的博客框架了，但总感觉速度有些慢，最近折腾着迁移到Hugo，安装、构建、部署整个流程都感觉到了对比Hexo速度的提升:rocket:，简单记录一下。

## 1 安装
本地macOS平台直接使用`Homebrew`安装
```Bash
brew install hugo
```

## 2 创建新站点
```Bash
hugo new site blog

cd blog

hugo new posts/first-post.md
```
这个时候就已经创建了新的博客站点`blog`，并且创建了第一篇文章`first-post.md`，新建的文章位于`/blog/content/posts`目录下。
{{< admonition type=notice title="新建文章注意" details=false >}}
默认情况下，所有文章新建都为草稿，草稿文章是不渲染的，需要修改头部`draft: true`为`draft: false`
{{< /admonition >}}

## 3 使用主题
Hugo提供了丰富的[主题](https://themes.gohugo.io/)，可以在这选择喜欢的主题，并添加到刚刚新加的博客站点，以我选择的[LoveIt](https://github.com/dillonzq/LoveIt)主题为例  
首先将主题添加到项目`blog/themes`目录，根目录下执行：
```Bash
git clone -b master https://github.com/dillonzq/LoveIt.git themes/LoveIt
```

然后在`/blog/config.toml`配置主题参数：
```toml
baseURL = "http://example.org/"
# [en, zh-cn, fr, ...] 设置默认的语言
defaultContentLanguage = "zh-cn"
# 网站语言, 仅在这里 CN 大写
languageCode = "zh-CN"
# 是否包括中日韩文字
hasCJKLanguage = true
# 网站标题
title = "我的 Hugo 博客站点"

# 更改使用 Hugo 构建网站时使用的默认主题
theme = "LoveIt"

[params]
  # LoveIt 主题版本
  version = "0.1.X"

[menu]
  [[menu.main]]
    identifier = "posts"
    # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
    pre = ""
    name = "文章"
    url = "/posts/"
    # 当你将鼠标悬停在此菜单链接上时, 将显示的标题
    title = ""
    weight = 1
  [[menu.main]]
    identifier = "tags"
    pre = ""
    name = "标签"
    url = "/tags/"
    title = ""
    weight = 2
  [[menu.main]]
    identifier = "categories"
    pre = ""
    name = "分类"
    url = "/categories/"
    title = ""
    weight = 3
```
这个主题功能很强大，更多详细配置及功能可以参考[项目Docs](https://hugoloveit.com/categories/documentation/)

## 4 本地展示
此时执行以下命令即可在本地[http://localhost:1313/](http://localhost:1313/)预览当前站点状态
```bash
hugo serve
```
![hugo-blog-example](https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/hugo-blog-example.png)

## 5 快速部署
准备好部署网站时，运行
```bash
hugo
```
可以快速构建网站，项目根目录下会生成`public`目录，其中包含博客站点所有内容和资源，直接部署在web服务器即可。

以部署到github pages为例，参考[Hugo官网](https://gohugo.io/hosting-and-deployment/hosting-on-github/)说明，创建`public`子模块，关联原先github page仓库`jiangbao.github.io`，将每次构建结果提交到远程仓库，可以通过自动部署脚本实现快速部署
```shell
#!/bin/sh

# If a command fails then the deploy stops
set -e

printf "\033[0;32mDeploying updates to GitHub...\033[0m\n"

# Build the project.
hugo # if using a theme, replace with `hugo -t <YOURTHEME>`

# Go To Public folder
cd public

# Add changes to git.
git add .

# Commit changes.
msg="rebuilding site $(date)"
if [ -n "$*" ]; then
	msg="$*"
fi
git commit -m "$msg"

# Push source and build repos.
git push origin master
```

到这一步，每次更新文章之后，需要本地手动执行`deploy.sh`来部署到github page、coding page等静态页面。  

貌似还是可以更简单...

如果我们只需要`git add . & git commit -m "" & git push `，其余的自动化处理，这样又可以偷点懒

要想快速实现自动部署，可以将执行放到`github actions`中，每次提交更新到master分支时，自动触发构建&部署，之后有时间再补上

还有更多的功能等待探索中...目前一天下来Hugo使用体验很不错，后面会将个人文章陆续迁移到这，慢慢完善
