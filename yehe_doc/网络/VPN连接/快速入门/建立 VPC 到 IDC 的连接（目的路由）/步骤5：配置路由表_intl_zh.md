截止至步骤 4，我们已经将一条 VPN 通道配置成功，但仍需配置子网路由策略，将子网1中的流量路由至 VPN 网关上，同时配置 VPN 网关路由策略，将到达 VPN 网关上的流量导入至 VPN 通道中，这样子网1中的网段才能与 IDC 中的网段通信。

1. 登录 [私有网络控制台](https://console.cloud.tencent.com/vpc/vpc?rid=1)。
2. 在左侧目录中单击【子网】，选择对应的地域和私有网络，如示例中的**东京**和 `VPC1`，单击子网1所关联的路由表 ID，进入详情页。
![](https://main.qcloudimg.com/raw/2edc1c8c0d8596423383ccb4e3b86396.png)
3. 在“基本信息”页签，单击【新增路由策略】。
   ![](https://main.qcloudimg.com/raw/60984e42e4e2b0ae9b7c5d64c422fc54.png)
4. 在弹出框中，输入对端 IDC 子网网段（`10.0.1.0/24`），下一跳类型选择【VPN 网关】，下一跳选择刚创建的 VPN 网关 `VPN1`，单击【创建】即可完成子网1路由策略的配置。
   ![](https://main.qcloudimg.com/raw/0ccfdc5d0f1fb1b5a0d66d9657e7422e.png)
5. 在左侧目录中选择【VPN 连接】>【VPN网关】。
6. 单击 VPN 网关实例 ID 进入实例详情页。
7. 在“实例详情”页面，单击【路由表】页签，配置 VPN 网关的路由策略。
8. 单击【新增路由】，在弹出的对话框中填写如下参数：
   + 目的端：填写对端 IDC 需要与本端 VPC 通信的内网网段，本例填写10.0.1.0/24。
   + 下一跳类型：只能是 VPN 通道，无需设置。
   + 下一跳：选择[ 步骤3 ](https://intl.cloud.tencent.com/document/product/1037/39679)中创建的 VPN 通道。
   + 权重：当 VPC 与 IDC 之间有两条 VPN 通道时，可通过权重来设置主备链路，本例保持默认值0即可。
     ![](https://main.qcloudimg.com/raw/b6f5b65eadf82b090d60f79d4091bd65.png)
9. 单击【确定】完成 VPN 网关路由策略的配置。
   ![](https://main.qcloudimg.com/raw/7704e64f645f5dda5d0d43b4eeb5da28.png)
