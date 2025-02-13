### MongoDB 的最新版本号是多少？
腾讯云数据库 MongoDB 目前提供的版本是3.2、3.6、4.0。

### MongoDB 云数据库怎么删除，到期后不续费会自动删除吗？
云数据库 MongoDB 实例到期后不续费会自动销毁，请您确认并备份数据。您可在 [控制台](https://console.cloud.tencent.com/mongodb) 实例列表的“操作”列，选择【更多】>【退货退费】/【销毁】执行自助销毁操作。

### MongoDB 如何申请安全凭证？
第一次使用云 API 之前，您需要在腾讯云 [CVM 控制台](https://console.cloud.tencent.com/cvm) 上申请安全凭证。
安全凭证包括 SecretId 和 SecretKey：
 - SecretId：用于标识 API 调用者身份。
 - SecretKey：用于加密签名字符串和服务器端验证签名字符串的密钥。

>!API 密钥是构建腾讯云 API 请求的重要凭证，使用腾讯云 API 可以操作您名下的所有腾讯云资源，为了您的财产和服务安全，请妥善保存和定期更换密钥，当您更换密钥后，请及时删除旧密钥，详情请参见 [签名方法](https://intl.cloud.tencent.com/document/product/240/32137)。

### MongoDB 用户名使用限制是什么？
云数据库 MongoDB 內建了默认用户 mongouser，mongouser 采用 SCRAM-SHA-1 认证方式，角色为 [readWriteAnyDatabase+dbAdmin](https://docs.mongodb.org/v3.0/reference/built-in-roles/)，也就是说您可以用此默认用户读写任意数据库，但是不具备高危操作的权限。

3.2版本的实例支持另外一个内建用户 rwuser，采用 MONGODB-CR 认证方式，该认证方式已被官方废弃，建议您优先使用 mongouser 连接数据库。

您也可以使用 [MongoDB 控制台](https://console.cloud.tencent.com/mongodb) 进行账号和权限管理以满足您的业务需要。
