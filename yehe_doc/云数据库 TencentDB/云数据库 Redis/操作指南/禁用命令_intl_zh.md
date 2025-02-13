## 操作场景
Redis 部分命令的使用可能会导致服务不稳定、或者数据误删除，因此云数据库 Redis 提供了禁用部分命令的功能。

云数据库 Redis 支持配置`disable-command-list`参数来禁用部分命令，如果您的控制台不可见`disable-command-list`参数，可 [提交工单](https://console.cloud.tencent.com/workorder/category) 升级后台版本，版本升级会有连接闪断，重新连接即可。

## 操作步骤
### 禁用命令
1. 登录 [Redis 控制台](https://console.cloud.tencent.com/redis)，在实例列表，单击实例名，进入实例管理页面。
2. 在实例管理页面，选择【参数配置】>【可修改参数】页，在`disable-command-list`参数行，可配置禁用命令名单。
>?
>- 支持禁用的命令包括 flushall、flushdb、keys、hgetall、eval、evalsha、script，云数据库 Redis 从2019年01月01日起新购的实例默认不禁用以上命令。
>- 配置禁用参数后会在2分钟内生效，禁用命令参数不会重启 Redis 服务，对存量链接生效。
>
![](https://main.qcloudimg.com/raw/3641b6811d698955f9564a34dfe73e36.png)

### 取消禁用命令
在【可修改参数】页的“当前运行参数值”禁用列表，删除相应的命令即可取消命令禁用。

### 参数修改历史
在【修改历史】页，可查看参数修改历史记录。

