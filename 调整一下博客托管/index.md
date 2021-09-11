# 调整一下博客托管

2020 年的第一天，找时间调整自己个人博客的托管服务，更新一下博客图片的图床。

<!--more-->

原先这个博客用 Hexo 搭建，部署到 Github Pages，站点部署在 [jiangbao.github.io](https://jiangbao.github.io) (Hexo + NexT 主题的博客已经见了很多，看来挺受欢迎，前端的同学应该很熟悉)，博客的图片使用 [PicGo](https://molunerfinn.com/PicGo/) + Github 做图床。

最近受 Github 影响，博客页龟速加载实在让人难受，所以还是决定把页面托管一份到国内平台，正好之前个人阿里云的服务到期了，在阿里云续费和腾讯云首次优惠之间，贫穷使我选择了将个人业务迁移到腾讯云(实在折腾🙄)，正好来一整套腾讯云产品，所以最后选择了 [CODING](https://coding.net/)。

最后选择 coding pages 部署站点，PicGo + 腾讯云 COS 做图床，为了方便还将之前[个人主页 www.u9c8d.com](http://www.u9c8d.com) 映射到了 coding 页面地址。忙了一下午，[新地址](http://www.u9c8d.com)打开速度让整个人都轻松了。翻了一下博客仓库的首次提交还是 2017 年 5 月份，快三年了，折腾了几次修改、调整，还是没有好好记录下东西，实在惭愧。

----

**=============2020-10-29更新=============**  
由于 coding 个人、企业版入口混乱，之前改版还导致丢了一些代码，pages 服务不稳定的问题，于是又转向另一个国内代码平台 [gitee](http://gitee.com)，将静态页面放到 [gitee pages](https://gitee.com/help/articles/4136#article-header0)。因为之前使用了 github actions 来做项目的自动化服务，所以只需要在原项目 `workflows/deploy.yaml` 添加自动化流程即可，整体思路比较简单：

1. gitee 下新建同名空项目，例如我个人用户名 `jiangbao1123`，新建同名项目，开启 gitee pages 后即可在`http://jiangbao1123.gitee.io`访问到页面，[参考](https://gitee.com/help/articles/4136#article-header0)

2. 代码提交到github仓库时同步一份到gitee仓库，可以使用 [wearerequired/git-mirror-action](https://github.com/wearerequired/git-mirror-action)

3. 如果使用的是gitee pages pro(付费)，则在代码同步后会自动使用 `hugo` 生成静态文件，刷新页面。如果不想使用付费服务，可以参考 [Gitee Pages Action](https://github.com/marketplace/actions/gitee-pages-action) 实现自动部署。

> 关于个人的域名：由于在各平台，自己首选的名称都是『酱鲍』、『jiangbao』之类，但可惜此域名已被使用，所以自己选了『鲍』的Unicode编码 `\u9c8d`，虽然识别度不够，但胜在够便宜🤔，够自己折腾了。

