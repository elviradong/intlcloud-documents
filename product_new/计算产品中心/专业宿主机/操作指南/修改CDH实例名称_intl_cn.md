为了方便用户快速辨识和管理专用宿主机，腾讯云支持通过控制台或者 API 的方式给专用宿主机设置名字，随时更改，立即生效。

## 使用控制台修改 CDH 实例名称

1. 登录 [专用宿主机控制台](https://console.cloud.tencent.com/cvm/cdh)。
2. 选择相应的地域，勾选需要修改名称的专用宿主机，单击列表顶部【更多操作】>【改名】。
   ![](https://main.qcloudimg.com/raw/5ab8292b6d76922c7fb35ed6484f6430.png)
3. 在改名操作弹窗中，输入新宿主机名称，单击【确定】完成操作。
   ![改名操作框](https://main.qcloudimg.com/raw/2f83d40760fefaff3212fd1b791e93bb.png)

## 使用 API 修改 CDH 实例名称
使用 ModifyHostsAttribute 接口可以修改 CDH 实例的名称，具体用法详见 [修改 CDH 实例的属性](https://intl.cloud.tencent.com/document/product/213/33278)。

