腾讯云提供了两种创建负载均衡的方式：官网购买页创建和 API 创建。本小节将详细介绍两种创建方式。

### 官网购买页创建
所有用户均可通过 [腾讯云官方网站](https://buy.cloud.tencent.com/lb) 购买负载均衡。其中，内网负载均衡免费，公网负载均衡仅收取实例费。公网负载均衡的计费模式是按量计费（以小时结算），公网网络请在 [云服务器](https://intl.cloud.tencent.com/document/product/213/495) 上购买，网络计费模式请参见 [公网计费模式](https://intl.cloud.tencent.com/document/product/213/10578)。

本文以标准账户类型为例，传统账户类型的购买方式请参考 [购买方式](https://intl.cloud.tencent.com/document/product/214/8849)。标准账户类型在官网购买负载均衡的具体操作如下：
1. 登录腾讯云 [负载均衡服务购买页](https://buy.cloud.tencent.com/lb)。
2. 在实例类型的选择上，推荐您选择“负载均衡”。
3. 根据实际情况和需求选择网络、所属项目等信息，各个属性的选择请参见 [产品属性选择](https://intl.cloud.tencent.com/document/product/214/13629)。
>?目前仅广州、上海、南京、济南、杭州、福州、北京、石家庄、武汉、长沙、成都、重庆地域支持静态单线 IP 线路类型，其他地域支持情况请以控制台页面为准。该功能处于内测阶段，如需体验，请提交内测申请。申请通过后，即可在购买页选择中国移动、中国联通或中国电信的运营商类型。
>
![](https://main.qcloudimg.com/raw/907a33302d20a9b399067bee190a1d78.png)
4. 确认购买后，您可以进行支付。
5. 支付完成后即开通负载均衡服务，您可进行负载均衡配置使用。

### API 创建
欲通过 API 购买负载均衡的用户，请参阅 [负载均衡API](https://intl.cloud.tencent.com/document/api/214/888) 创建实例。
