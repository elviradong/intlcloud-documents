
### 如何修改云数据库 MySQL 的配置参数？
- **云数据库 MySQL 控制台方式**
在 [云数据库 MySQL 控制台](https://console.cloud.tencent.com/cdb)，单击实例名称进入管理页，选择【数据库管理】>【参数设置】，其中常见的 var\_name 包括如下变量：
<table>
<tbody><tr>
<th>变量</th><th>说明</th></tr>
<tr>
<td>character_set_server</td><td>服务器默认字符集</td></tr>
<tr>
<td>connect_timeout</td><td>连接超时</td></tr>
<tr>
<td>long_query_time</td><td>超过该时间的查询为慢查询</td></tr>
<tr>
<td> max_allowed_packet</td><td>最大包长度</td></tr>
<tr>
<td>max_connections</td><td>最大连接数</td></tr>
<tr>
<td>sql_mode</td><td>当前的服务器 SQL 模式</td></tr>
<tr>
<td>table_open_cache</td><td>全部线程打开表的个数，增大该值可以增加 mysqld 被请求打开的文件描述符个数</td></tr>
<tr>
<td>wait_timeout</td><td>非交互连接超时时间</td></tr>
</tbody></table>

- **phpMyAdmin 控制台方式**
通过 phpMyAdmin 登录云数据库 MySQL 后，单击上方菜单中的【变量】，在下面的变量列表中，单击需要修改的变量对应的【编辑】，对其进行修改后单击【保存】。
![](https://main.qcloudimg.com/raw/214fec618ff4e166c6e4d747be3fea0b.png)

更多配置参数可在控制台的【数据库管理】>【参数设置】页查看。

### 云数据库 MySQL 怎么设置中文查询？
云数据库 MySQL 目前不支持中文字符。

### 如何设置开启云数据库 MySQL 的定时器功能？
在 [云数据库 MySQL 控制台](https://console.cloud.tencent.com/cdb)，单击实例名称进入管理页，选择【数据库管理】>【参数设置】页，在参数设置中将 event_scheduler 参数设置为 ON。

### 云数据库 MySQL 超时连接设置太短，如何增加时间？
在 [云数据库 MySQL 控制台](https://console.cloud.tencent.com/cdb)，单击实例名称进入管理页，选择【数据库管理】>【参数设置】页，在参数设置中修改 wait_timeout 参数。

### 云数据库 MySQL 如何调整 group_concat_max_len 参数？
在 [云数据库 MySQL 控制台](https://console.cloud.tencent.com/cdb)，单击实例名称进入管理页，选择【数据库管理】>【参数设置】页，在参数设置中修改 group_concat_max_len 参数。

### 云数据库 MySQL 全表扫描的 SQL 语句有什么方法可以找到吗？
默认是不记录全表扫描的语句，可在云数据库 MySQL 控制台【参数设置】中将 log_queries_not_using_indexes 参数设置为 ON，注意不要开太久。

### 云数据库的默认字符集编码如何修改？
云数据库 MySQL  默认字符集编码格式是 UTF8，目前支持 LATIN1 、GBK、UTF8 、UTF8MB4 四种字符集设置。

虽然云数据库支持默认字符集编码的设置，但我们还是建议您在创建表时，显式的指定表的编码，并在连接建立时指定连接的编码，这样，您的应用将会有更好的移植性，MySQL 默认字符集说明以及修改方法请参见 <a href="https://intl.cloud.tencent.com/document/product/236/7259" target="_blank">使用限制</a>，也可通过 [控制台](https://console.cloud.tencent.com/cdb) 修改字符集。
