
## 功能说明
腾讯云 CDN 的 POST 请求大小上限，即请求 body 大小的上限，默认为 32MB。您可根据业务实际情况调整此处的上限。

>! 此功能暂不支持 HTTPS 回源，请确保域名的回源协议为 HTTP。

## 配置指南

### 查看配置

登录 [CDN 控制台](https://console.cloud.tencent.com/cdn)，在左侧菜单栏选择【域名管理】，单击域名操作列的【管理】，进入域名配置页面，切换 Tab 至【高级配置】，即可找到【POST 请求大小配置】，最大可调整为 **200MB**。
![](https://main.qcloudimg.com/raw/e3a25b8ba81fe251e76fbf0deb22e966.png)

>!部分平台无 POST 请求大小的限制，域名暂不支持此功能。

