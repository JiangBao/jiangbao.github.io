# 将小程序分享到朋友圈


最近看到微信小程序支持分享到朋友圈的功能，恰好之前做小程序业务的时候遇到过这样的需求，就来尝试一下这个新功能。

<!--more-->

## 体验
参考[官方文档](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/share-timeline.html)，目前「分享到朋友圈」为Beta功能：
* 从基础库2.11.3开始支持
* 支持平台：`Android`

不支持「分享给好友」那样的页面内分享，只支持顶部菜单栏「分享到朋友圈」的入口。

另外，为防止滥用此功能，从朋友圈打开的时候并不是直接进入小程序，而是进入一个所谓“小程序单页模式”的页面，页面上交互功能受限，所以更适合内容展示型场景。

## 如何实现

1. 确保基础库版本在`2.11.3`以上，此时打开待分享页面顶部菜单栏，能看到「分享到朋友圈」的入口，由于没开启开关，所以此时看到应该是置灰的  

    {{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/share-time-line-1.png" width="371" height="328">}}

2. 打开「分享到朋友圈」菜单开关  
  在带分享页面`onLoad`或者`onShow`生命周期添加以下代码，这也就是官方文档所说的：设置允许“发送给朋友”, 允许“分享到朋友圈”
    ```js
    wx.showShareMenu({
      withShareTicket: true,
      menus: ['shareAppMessage', 'shareTimeline']
    });
    ```
    此时应该可以正常使用「分享到朋友圈」的功能(Android)  

    {{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/share-timeline-2.png" width="371" height="328">}}

    分享的默认标题为小程序名称，默认图片为小程序logo

    {{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/share-timeline-3.png" width="334" height="200">}}

3. 自定义分享参数  
  类似转发给好友的处理函数`onShareAppMessage`，提供了分享到朋友圈的自定义处理函数`onShareTimeline`。按照[文档](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onShareTimeline)，目前支持`title`、`query`、`imageUrl`三个自定义参数。添加代码：
    ```js
    onShareTimeline() {
      return {
        title: '这是一个自定义的标题！',
        imageUrl: 'https://image.url'
      }
    }
    ```
    可以看到自定义分享的效果：

    {{<figure src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/share-timeline-4.png" width="324" height="196">}}


## 遇到的问题
由于测试的时候使用的是uni-app构建的项目，导致自定义分享标题、图片的功能不生效，还是使用默认的分享字段，然后又使用微信小程序原生代码创建了项目进行测试，自定义功能是生效的，可能是uni-app版本对新功能的支持问题，果然升级版本之后，已经可以使用自定义功能了。所以使用第三方框架的项目想使用此功能需要注意进行版本升级。

