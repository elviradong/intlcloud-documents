## 简介

当您在推送消息时出现异常情况（例如推送消息收不到或推送失败）时，可通过排查工具进行自助排查以及通过账号或者 Token 查询设备详情、设备绑定的账号以及设备绑定的标签。

## 常见场景

1. 开发人员在控制台推送一条消息给测试设备，控制台推送状态显示成功，但在手机通知栏未看到这条消息，通过 Token 查询设备详情发现通知栏权限关闭，在手机上开启通知栏权限后，再次触发注册，此时往这台设备发送消息，手机通知栏成功展示。

2. 运营人员通过用户账号推送消息，推送状态显示“推送失败”，在排查工具中输入用户账号查询 Token，发现该应用下，此账号没有关联的设备 Token，可联系开发人员排查是否调用了 [账号绑定（Android）](https://intl.cloud.tencent.com/document/product/1024/30715)或 [账号绑定（iOS）](https://intl.cloud.tencent.com/document/product/1024/30727)接口。
	 ![](https://main.qcloudimg.com/raw/867d6c62be726c6fb8af6ca4922ae16f.png)
3. 开发人员完成了 TPNS 的接入工作后，对某一批设备 Token 推送消息，线上用户反馈没有收到该推送信息，获取该用户 Token 和 pushID 后，在推送排查得知，推送列表中并无该 Token。



## 操作步骤

### 通过用户账号查询

1. 登录 [移动推送 TPNS 控制台](https://console.cloud.tencent.com/tpns)。
2. 在左侧导航栏选择【工具箱】>【排查工具】。
3. 单击下拉框选择您需要查询产品及应用，选择【通过账号查询】。
   ![](https://main.qcloudimg.com/raw/ab3ee41ac3fcab0fe15b83b455a55d24.png)
4. 输入用户账号，单击【查询】。
5. 选择账号下关联的设备 Token 查看设备注册详情。
> ?
> - 账号：与 Token 绑定的用户唯一 ID，一般为 OpenID、UID 等。
> - Token 列表展示规则：Token 按与账号绑定时间的倒序展示，本页面最多展示10个 Token，推送目标设置为账号推送时，系统默认推送给最近一次绑定该账号的设备 ，例如需要更改为推送给所有绑定该账号的设备，请在 [控制台](https://console.cloud.tencent.com/tpns) 或 [API](https://intl.cloud.tencent.com/document/product/1024/33764) 推送时更改设置。

### 通过 Token 直接查询

1. 在排查工具页面中选择【通过 Token 直接查询】
2. 输入设备 Token，单击【查询】，页面右侧会显示当前设备注册详情。
> ? 
> - Token：TPNS 分配给每一个设备的唯一ID，长度36位。
> - 若您不知道如何获取设备 Token，可参考 SDK 集成文档中的 [获取 Token 交互建议（Android）](https://intl.cloud.tencent.com/document/product/1024/30713)或 [获取 Token 交互建议（iOS）](https://intl.cloud.tencent.com/document/product/1024/30726)。

### 推送查询

1. 在【排查工具】页面点击【推送查询】。
2. 输入 pushid（必选）、待查询的设备 Token（必选），单击【查询】，查看排查结果。
> ?pushid 获取方式：
>1. 可在左侧导航栏选择【推送管理】>【推送任务】，获取需要查询的 PushID。
>2. 在推送接口的应答参数中获取。
>3. 若查询结果与实际情况不符或仍无法解决问题，可查看 [推送常见问题](#.E6.8E.A8.E9.80.81.E5.B8.B8.E8.A7.81.E9.97.AE.E9.A2.98) 或 [提交工单](https://console.cloud.tencent.com/workorder/category) 反馈，并在工单中补充 pushID 和 Token 信息。

## 常见问题

### 设备查询常见问题

1. 设备 Token 什么情况下会失效？
   答：
   - Token 有效期为90天，若设备连续90天未连接移动推送服务器，则会被标记为无效设备。
   - 若设备卸载了 App，当前 Token 会被标记为无效。
2. 通知栏权限已经开启，为什么设备查询显示关闭状态？
   答：若手机通知栏设置从关闭到开启权限，需要在客户端重新触发一次注册请求，才可以把开启状态同步到移动推送服务器。

### 推送常见问题

1. 通过账号或者标签推送无法收到，但通过 Token 推送可以收到，是什么原因？
   答：在设备注册详情页中查看该 Token 是否关联了推送的目标账号或者标签，若未关联则无法通过账号或标签推送。
2. 推送状态显示已完成，设备状态正常，为什么手机没有收到推送？
   答：
 - iOS 平台推送环境是否与 Token 注册环境一致，不一致则无法收到推送，推送环境选择说明详情可参考文档 [推送环境选择](https://intl.cloud.tencent.com/document/product/1024/34214)。
 -  检查 App 包名是否和【[移动推送 TPNS 控制台](https://console.cloud.tencent.com/tpns)】>【配置管理】>【基础配置】中填写的包名一致，如果包名不一致检查是否开启 [多包名推送功能](https://intl.cloud.tencent.com/document/product/1024/35393)。
 - 安卓 P 及以上系统需要添加使用 Apache HTTP client 库，检查在 AndroidManifest 的 application 节点内是否有添加以下配置：
 ```
 <uses-library android:name="org.apache.http.legacy" android:required="false"/>
 ```
 - 小米手机确认是否被收纳到【不重要通知】。
 - 魅族手机确认是否被收纳到【消息盒子】。
 - OPPO 手机通知栏默认关闭，确认是否手动开启。
 - vivo 、OPPO 部分机型通知栏默认关闭，确认是否手动开启。
 - vivo 、华为部分机型，通知渠道的横幅、铃声等权限默认关闭，确认是否手动开启。
 - 应尽量避免使用“test”、“测试”等字眼，部分厂商通道会拦截这类型的通知。
 - vivo、OPPO 需要厂商审核通过后才可以测试推送，请注意确认。
