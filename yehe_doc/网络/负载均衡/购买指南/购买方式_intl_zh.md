腾讯云提供了两种购买负载均衡的方式：官网购买和 API 购买。本小节将详细介绍两种购买方式。

## 官网购买
所有用户均可通过 [腾讯云官方网站](https://buy.cloud.tencent.com/lb) 购买负载均衡。腾讯云账户分为标准账户类型和传统账户类型，2020年6月17日零点后注册的账户均为标准账户类型，该时间点前注册的账户请在控制台查看您的账户类型，具体操作请参见 [判断账户类型](https://intl.cloud.tencent.com/document/product/684/15246)。
### 传统账户类型
内网负载均衡免费，公网负载均衡仅收取实例费。公网负载均衡的计费模式是按量计费（以小时结算），公网网络请在 [云服务器](https://intl.cloud.tencent.com/document/product/213/495) 上购买，网络计费模式请参见 [公网计费模式](https://intl.cloud.tencent.com/document/product/213/10578)。
在官网购买负载均衡的具体操作如下：
1. 登录腾讯云 [负载均衡服务购买页](https://buy.cloud.tencent.com/lb)。
2. 在实例类型的选择上，推荐您选择“负载均衡”。
3. 根据实际需求选择网络、所属项目等信息，各属性的选择请参考 [产品属性选择](https://intl.cloud.tencent.com/document/product/214/13629)。
![](https://main.qcloudimg.com/raw/33dfce7ca7a0bbde210346a313a42d8e.png)
4. 确认购买后，您可以进行支付。
5. 支付完成后即开通负载均衡服务，您可进行负载均衡配置使用。

### 标准账户类型
内网负载均衡免费，公网负载均衡收取实例费和公网网络费。公网负载均衡支持包年包月和按量计费两种计费模式，包年包月的公网网络费仅支持按带宽计费，按量计费的公网网络费支持按带宽计费、按流量计费、共享带宽包三种计费模式。
在官网购买负载均衡的具体操作如下：
1. 登录腾讯云 [负载均衡服务购买页](https://buy.cloud.tencent.com/lb)。
2. 在实例类型的选择上，推荐您选择“负载均衡”。
3. 根据实际情况和需求选择网络、网络计费模式、所属项目等信息，各个属性的选择请参考 [产品属性选择](https://intl.cloud.tencent.com/document/product/214/13629)。
>?目前仅广州、上海、南京、济南、杭州、福州、北京、石家庄、武汉、长沙、成都、重庆地域支持静态单线 IP 线路类型，其他地域支持情况请以控制台页面为准。该功能处于内测阶段，如需体验，请提交内测申请。申请通过后，即可在购买页选择中国移动、中国联通或中国电信的运营商类型。
>
![](https://main.qcloudimg.com/raw/f09271383a733a0c0281ebe324b6ff56.png)
4. 确认购买后，您可以进行支付。
5. 支付完成后即开通负载均衡服务，您可进行负载均衡配置使用。

## API 购买
欲通过 API 购买负载均衡的用户，请参见 [负载均衡 API - 购买负载均衡实例](https://intl.cloud.tencent.com/document/product/214/33841)。

## 后续步骤
- 若需要为负载均衡添加 TCP 监听器，请参见 [配置 TCP 监听器](https://intl.cloud.tencent.com/document/product/214/32517)。
- 若需要为负载均衡添加 UDP 监听器，请参见 [配置 UDP 监听器](https://intl.cloud.tencent.com/document/product/214/32518)。
- 若需要为负载均衡添加 TCP SSL 监听器，请参见 [配置 TCP SSL 监听器](https://intl.cloud.tencent.com/document/product/214/32519)。
- 若需要为负载均衡添加 HTTP 监听器，请参见 [配置 HTTP 监听器](https://intl.cloud.tencent.com/document/product/214/32515)。
- 若需要为负载均衡添加 HTTPS 监听器，请参见 [配置 HTTPS 监听器](https://intl.cloud.tencent.com/document/product/214/32516)。
