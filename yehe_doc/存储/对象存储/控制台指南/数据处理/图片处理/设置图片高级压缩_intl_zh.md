## 简介

图片高级压缩是 COS 基于数据万象推出的图片压缩功能，可以更加高效地将图片格式转码为 TPG 或 HEIF 高压缩比格式，有效降低图片传输链路及加载耗时，降低带宽及流量成本。

>?
>
>- 图片高级压缩功能可将 JPG/ PNG/ GIF/ WEBP 等格式图片转码为 TPG/HEIF 格式。
>- TPG 是腾讯自研的图片格式，如需使用请确认**图片加载环境支持 TPG 解码**，腾讯云数据万象提供集成 TPG 解码器的iOS、Android、[Windows](https://main.qcloudimg.com/raw/851dd252378813d250eeca5ed55ffd36/TPG_win_SDK.zip) 终端 SDK，可帮助您快速接入和使用 TPG。
>- 目前 iOS 11以上及 Android P 系统已原生支持 HEIF 格式。
>- 图片高级压缩为付费服务，由数据万象收取，具体费用请参见数据万象的 [定价文档](https://intl.cloud.tencent.com/document/product/1045/33431)。

## 操作步骤

1. 登录 [对象存储控制台](https://console.cloud.tencent.com/cos5/bucket)，在【存储桶列表】页面选择需开启图片高级压缩功能的存储桶，进入存储桶管理页面。
2. 单击【数据处理】>【图片处理】页签，在界面中找到**图片高级压缩**配置项。
3. 单击【编辑】并将当前状态修改为“开启”，然后单击【保存】。
   ![](https://main.qcloudimg.com/raw/f336e71135338664375a4953fabb5a6e.png)
4. 开启功能后，对于当前存储桶中的图片资源，可使用相应的 [图片高级压缩](https://intl.cloud.tencent.com/document/product/436/40119) API 实现下载时进行 TPG/HEIF 压缩。
