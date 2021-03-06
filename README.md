# 集思会Lite 小程序

## 访问

* 微信搜索集思会Lite
* 扫码访问

![二维码](/images/2.jpg)

## 预览

![](/images/1.jpg)

## 该小程序的相关数据统计

![](/images/a.jpg)

最近接触小程序，记录一下从零开发到提交审核正式发布的完整过程，和其中踩过的坑，与大家一起学习。

## 一.选型

项目的前期调研：

从小程序随着微信对小程序的推广越来越大，小程序的相关生态也日渐完善，于是，在进行小程序的相关开发工作之前，进行了开发的调研工作。

首先从项目框架的选型上，主要有3种选择，微信小程序的原生开发，wepy 框架开发，labrador框架开发，原生开发是使用小程序的自己的一套开发标准，参考web 的原生开发，wepy 是 vue 风格的小程序开发框架，labrador 则比较亲和 React，对于详细的区别和考量，我们来看看一些别人的讨论：

《微信小程序开发最佳实践》——skylor：

![](https://user-gold-cdn.xitu.io/2018/2/6/16169217b4e62eb7?w=1742&h=458&f=png&s=143272)

主要意思就是说使用原生开发少去了第三方框架的学习成本。

腾讯官方的wepy框架下相关人员提出的关于选型问题的讨论：

![](https://user-gold-cdn.xitu.io/2018/2/6/16169220cd888884?w=1920&h=2148&f=jpeg&s=390364)
总结一下就是，wepy 项目是官方维护的，它的可靠性是有保障的，它的下面这句话很重要：

从Web转向WePY时，开发者会拿Vue, React之类的来对比，因此会更多的看到的是WePY的不足，而原生切WePY的开发者更多的看到的是WePY的优势。

之前使用过 vue ，于是优先考虑了 wepy 的选择，但实际上你会发现比较尴尬的一点，这个 因为小程序本身的一些限制，在使用wepy的时候有时候像 vue，有时候又不像，你还得在其中去区分在 vue 里可以使用的方式在 wepy 里可不可以使用，反而会弄混淆，这样的学习成本倒不如一开始就使用原生来的简单清晰。

labrador直接放弃，原因见第一个截图。

至此，确定了目前选择原生开发。

## 二.注册

如下图开始，填写相应的注册信息：

![image.png](https://user-gold-cdn.xitu.io/2018/2/26/161d01cc64407960?w=1232&h=741&f=png&s=89385)

注册成功以后，会在这里得到一个APPID：

![image.png](http://upload-images.jianshu.io/upload_images/165092-261bd603b7c40616.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

详细的请看这里[官方注册流程地址](https://mp.weixin.qq.com/debug/wxadoc/introduction/index.html?t=201828)，


## 三.微信web开发者工具

确定选型后下载官方微信web开发者工具进行开发

[官方下载地址](https://mp.weixin.qq.com/debug/wxadoc/dev/devtools/uplog.html)

扫码登录，填入项目地址，APPID和项目名称：

![image.png](http://upload-images.jianshu.io/upload_images/165092-033dddd199a791af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 3.开发

* 目录结构：

```
|	|___assets 				// 存储图片
|	|___pages  				// 页面
|	|	|___index // 首页
|   |		|___index.wxml  // 页面结构文件
|	|		|___index.wxss  // 样式表文件
|	|		|___index.js    // js文件
|	|___utils 				// 全局公共函数
|	|___app.js   			// 系统的方法处理文件
|	|___app.json 			// 系统全局配置文件
|	|___app.wxss 			// 全局的样式表
|	|___config.json 		// 配置文件

```

* 小程序配置文件app.json内容

具体的这个文件的配置起什么作用，可以看这里[app.json配置官方文档](https://mp.weixin.qq.com/debug/wxadoc/dev/quickstart/basic/file.html#JSON-%E9%85%8D%E7%BD%AE)

![image.png](http://upload-images.jianshu.io/upload_images/165092-0fbe2c509179085c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

而写在 pages 字段的第一个页面就是这个小程序的首页(打开小程序看到的第一个页面)。

* 接口地址记得要登录小程序后台添加，如：

在[小程序管理后台-开发设置-服务器域名](https://link.juejin.im/?target=https%3A%2F%2Fmp.weixin.qq.com%2Fwxopen%2Fdevprofile%3Faction%3Dget_profile%26token%3D1316597147%26lang%3Dzh_CN)中将`request合法域名`配置为`https://xxx.com`

## 一些踩过的，少走弯路的坑：

1. wxss里面的css样式引用图片地址，不能引用本地的，比如：

解决方案，引用在线地址和使用image标签，或者图片可以使用base64。

2. 下拉刷新。使用onPullDownRefresh下拉处理下后，要在后面使用wx.stopPullDownRefresh()停止下来刷新，否则界面会停留在下拉以后的界面，也就是上方会留下下拉空白。（原来以为下拉后会自动收回上去的）
![image.png](https://user-gold-cdn.xitu.io/2018/2/26/161d01cc6611291c?w=271&h=145&f=png&s=2136)

3.wx.switchTab: url 不支持 queryString，也就是说，跳转到tab页面不支持带参数，同时也要注意跳转到底部tab页面和非tab页面调用不同方法。

4.有地方需要用到嵌入网页的功能，使用了发现不起效果，原来是有限制，如下：

![image.png](http://upload-images.jianshu.io/upload_images/165092-5c98dcfe68682cf5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我是个人版，所以无法使用该组件。支付方面个人版也会有限制，有条件的推荐申请企业版。

5.绑定事件获取的target与currentTarget是有区别的

通常我们点击某个元素的时候需要获取到当前元素的属性，点击以后拿到的参数里有这两个属性需要区分一下，target 和 currentTarget。两者的区别在于：

target：触发事件的源组件;
currentTarget： 事件绑定的当前组件;

6.请求的接口地址需要是HTTPS的，所以需要升级下HTTPS，不用全站HTTPS，只要接口是HTTPS就可以了。此外，第一项里提到的在线图片地址可以不用是HTTPS。

7.任何情况下的视图更新只能通过setData()。

如果你以为数据变了，对应的视图也会相应改变，那就错了，这个时候你还是需要通过setData()方法改变相应变量，视图才会更新。

## 四.提交体验版进行多用户实机测试

当我们本地开发好以后，点击预览，会得到一个二维码，扫描二维码即可在自己的手机上预览实机效果（不过每次修改后都得点击重新编译预览才行，并不方便），但是我想要别人也来测试实机效果的时候，把这个二维码发过去却不行，界面会提示：

```
Unauthorized access
```
看来这个预览还的是开发者的自己的微信才行，那么如何让别的用户测试呢？点击这里：

![image.png](http://upload-images.jianshu.io/upload_images/165092-9b2cd09931035849.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上传成功以后，登录小程序后台，选择开发管理：

![image.png](http://upload-images.jianshu.io/upload_images/165092-330fb0d4ab1ca8ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击箭头，选择将刚才提交的版本作为体验版，然后点击版本号下面的文字就能得到开发版的分享二维码了。

但是这个二维码别人还不能用，原来还要进入左边的用户身份栏，然后点击箭头：

![image.png](http://upload-images.jianshu.io/upload_images/165092-9c13e9995bfcd581.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

添加用户并勾选体验者权限，该用户才能体验。否则得到的二维码也会提示没有权限。每添加一个用户都要扫一下二维码，而且只能是管理员添加，而且提示体验名额有限制。。。

## 五.提交审核

好了，大家终于都测试没问题了，准备提交审核，这个时候提示完善小程序信息，填写相应信息提交即可。

![image.png](http://upload-images.jianshu.io/upload_images/165092-4857836b4456989d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

提交上去，一个晚上就通过了审核，管理员手机会收到已经通过审核的微信消息。以为就这样结束了吗？在小程序搜索通过审核的小程序，没有结果，原来，通过审核后还得登录后台，将刚才通过审核的版本发布出去，然后过几分钟，就可以搜索到了。

![image.png](http://upload-images.jianshu.io/upload_images/165092-58c0b3104350413f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

关于什么是线上版本，什么是审核版本，官方文档里有解释。

## 六.建议

推荐多看看微信小程序官方文档，很多情况已经在文档里说明了。（吐槽下文档的搜索功能不怎么好用）

## 七.对小程序的感受和看法

> 它无法取代 APP。

小程序的优缺点很明显，这些也是为什么选择小程序和为什么不放弃 APP 的原因。

优点：

* 微信入口，无需下载，更容易带来流量。要知道，很多时候，用户可能对你的服务有兴趣但是一看到要下载 APP 就走开了，一是麻烦，二是担心手机存储，而小程序即扫即用的优势显而易见。用户可以使用通过扫码使用你的服务，对于更高级的功能再引导用户下载 APP 体验，这种推广方式更加友好，比直接一上来就要求用户下载 APP 有效的多。

* 用户无需更新客户端。APP 的模式使得不仅商家需要上架新的 APP进行审核，用户也需要下载新的 APP 才能体验最新功能。而微信小程序免去了用户更新的痛苦。特别是还有一些用户有基本不升级 APP 的习惯。

* 不用区分系统。我们知道一个 APP 要有安卓版和 iOS 版本，不同的语言编写，开发成本高。但是小程序是基于微信的，不论是安卓还是 iOS 手机，只要这个手机上有微信，那么你的这一套程序就能在两个系统上运行，不存在区分安卓版本和 iOS 版本的情况。最多会有一些显示界面的微小差别的兼容问题而已，问题不大。

* 开发成本比 APP 小。这里的成本包括且不限于，人力成本，时间周期成本等。

* 作为一个在微信基础上建立的产品，天生自带微信的社交属性，便于传播和分享。

以上几点，可以感觉到，小程序对于初创型的公司比较有利，开发速度快，成本低，还有流量入口，便于快速迭代产品，抢占市场，占得先机。这其中的一个典型案例，就是“头脑王者”，有了微信这个平台，加上本身的营运，上千万用户成就迅速达成。

缺点：

* 比较重的功能还是得放到 APP 上进行。小程序上只适合轻量级操作，功能没有 APP 强大。

* 被微信说封就封了，不过 App Store 也有下架的例子。

* 程序入口不如 APP直接，还得先打开微信。

所以小程序的定位一直是轻量的，也如微信自己所说，它无法取代 APP。

微信小程序的发展。刚开始推出的时候是轰轰烈烈的，包括各种媒体也好啊，大家都在说，你看多好啊，即用即走，用户也不用下载那么多APP了，以后大家手机只需要装一个APP，那就是微信，再加上当时微信因为打赏功能和Apple闹得不愉快，人们纷纷议论微信这种有点自建生态系统的行为，会不会导致被APP store下架。毕竟终于有人站出来和Apple扳手腕了，大家自然很嗨，搬着板凳嗑着瓜子叫嚣着。不过确实，找遍国内，也只有微信有这个能量去做自己的生态。

但渐渐地，人们发现，此时的小程序还很鸡肋，所支持的功能还很少（比如还不支持嵌入网页），限制还很多（比如打包后的大小），在这些功能和限制下，打造出来的小程序，再加上入口比较深，并没有取得之前预料的效果。热度开始慢慢减退了。

但是，在后来的一段时间里，我感觉几乎每周，都能收到小程序功能升级的推送，在冷清的市场中，微信依然有周期的迭代新的功能，而且更新的频率也刚好，让人感觉，哎，现在功能月来越多，是不是可以尝试一下了。

而小程序再一次高调吸引眼球的，是2017年底更新的小游戏功能，微信客户端更新后再次进去首屏，就是当时刷屏的“跳一跳”小程序了。可以嵌入网页，可以打开APP等等越来越多的功能升级，小程序的热度逐渐从高到低再到回归正常，它在不断摸索中寻找自己在市场中的定位，又，何尝不像我们。
