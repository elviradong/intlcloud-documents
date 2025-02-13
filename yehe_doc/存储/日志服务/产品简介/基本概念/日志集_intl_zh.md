日志集（Logset）是日志服务的项目管理单元，用于区分不同项目的日志。一个日志集对应一个项目或应用，建议将不同项目和不同产品的日志，使用不同的日志集进行管理。例如，某公司有两种业务：电商业务、支付业务，每种业务可以创建一个日志集。

一个日志集可以包含多个 [日志主题](https://intl.cloud.tencent.com/document/product/614/32849)，每个日志集有以下基本属性信息：
- 日志集名称：日志集命名。
- 日志集 ID：标识一个日志集的 ID 编号，全局唯一。
- 地域：日志集所属 [地域](https://intl.cloud.tencent.com/document/product/614/18940)。
- 存储周期：当前日志集里数据的保存时间周期，可保存时间周期为90天。若需要更多存储空间，可 [提交工单](https://console.cloud.tencent.com/workorder/category)。
