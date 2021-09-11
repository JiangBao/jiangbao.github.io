# 使用 Github Actions 做持续集成

之前团队代码托管在 [Gitee](https://gitee.com/)，自动化流程是 git 仓库 webhook + jenkins 那一套。最近部分业务迁移到 github，折腾一下，使用 [Github Actions](https://github.com/features/actions) 做自动化流程。因为自己以前在 Github 代码使用的是 Travis CI，所以这次简单记录一下 Github Actions 的使用。

## 了解
Github 将持续集成过程中：获取分支代码、测试、构建、发布到远程服务器...等等操作定义为 `Actions`。 

在项目根目录 `.github/workflows/` 文件夹下编写 `*.yml` 后缀的 `YAML` 类型文件，可以使用  [actions 库](https://github.com/marketplace?type=actions)中官方或第三方的库来快速实现获取分支代码、发布到远程服务器等操作，以此组合各类actions。  

Github仓库发现 `.github/workflows/` 目录下有 `*.yml` 就会自动执行，由此实现自动化流程。

## 概念
主要理解以下概念，更多的配置参考[官方文档](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions)

* on: 定义 workflow 何时被触发执行
  * push 到 master 分支时
    ```yaml
    on:
      push:
        branches:
        - master
    ```
  * PR 到 master 分支时
    ```yaml
    on:
      pull_request:
        branches:
        - master
    ```
  * 每小时触发执行
    ```yaml
    on:
      schedule:
      - cron: "0 * * * *"
    ```
* jobs: 要执行的多项任务
  ```yaml
  jobs:
    first_job:
      # 指定虚拟机环境(必须)
      runs-on: ubuntu-latest

      # 步骤
      steps:
        # 使用官方 actions 库获取分支代码
        - name: Checkout
          uses: actions/checkout@v2

        # 执行命令行指令，安装依赖
        - name: Install dependencies
          run: npm install
    second_job:
      # 另一个任务
  ```

## 例子
**需求**：将一个 create-react-app 生成的典型 react 工程接入 Github Actions，实现自动化构建、部署
### 准备
整个过程可以分为: 获取分支代码->设置 npm 版本->安装依赖->构建->部署到远程服务器

**部署文件到远程服务器** 这一步涉及服务器权限认证，我们采用 ssh 登录，使用 [`easingthemes/ssh-deploy`](https://github.com/marketplace/actions/ssh-deploy) 第三方库。会使用到 ssh 私钥、服务器登录用户名、服务器地址三个参数，将这三个参数配置到代码仓库-> Settings -> Secrets，使之私密不可见

![secrets](https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/github-actions-secrets.jpg)

{{< admonition type=tip title="对于没有SSH秘钥生成经验的可以参考" details=false >}}
1. 执行 `ssh-keygen -t rsa -f <YOUR_NAME>`，然后一路回车，可以在`~/.ssh`目录下看到生成了两个文件：`YOUR_NAME`(私钥)和`YOUR_NAME.pub`(公钥)。私钥是个人登录凭证，需保密；公钥要配置到部署的目标服务器

2. 将公钥 `YOUR_NAME.pub` 内容复制到目标服务器的 `~/.ssh/authorized_keys` 中，使目标服务器允许这个秘钥对登录

3. 将私钥文件 `YOUR_NAME` 配置到代码仓库 `Secrets` 中
{{< /admonition >}}

### 编写
项目根目录下新建 `./github/workflows/deploy.yml` 文件
```yaml
name: CI

on:
  push:
    # 推送到 master 分支时触发
    branches:
      - master

jobs:
  build_deploy:
    runs-on: ubuntu-latest

    steps:
      # 获取分支代码
      - name: Checkout
        uses: actions/checkout@master
      
      # 设置 node 版本
      - name: Setup node
        uses: actions/setup-node@v1
        with:
          node-version: "12.16.1"
      
      # 安装依赖
      - name: Install Dependencies
        run: npm install

      # 构建
      - name: Build
        run: npm run build

      # 部署到远程服务器
      - name: Deploy to server
        uses: easingthemes/ssh-deploy@v2.0.7
        env:
          SSH_PRIVATE_KEY: ${{ secrets.DEPLOY_TOKEN }}  #部署秘钥
          ARGS: "-rltgoDzvO --delete"                   #执行参数，参考库文档
          SOURCE: "./build/*"                           #要推送的文件
          REMOTE_HOST: ${{ secrets.SSH_HOST }}          #服务器地址
          REMOTE_USER: ${{ secrets.SSH_USER }}          #登录用户名
          TARGET: "/your/target/dir"                    #推送到目标服务器的路径
```
将更新提交到仓库 `master` 分支，点开 `actions` 可以看到类似下面的构建过程，至此一个简单的自动化流程就完成了，关于 `Github Actions` 还有更多可玩性值得去探索。

![构建过程](https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/github-actions-demo.jpg)

## 参考
> http://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html
> https://frostming.com/2020/04-26/github-actions-deploy
