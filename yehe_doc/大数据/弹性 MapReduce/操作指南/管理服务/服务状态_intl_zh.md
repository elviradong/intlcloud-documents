## 功能介绍
服务状态提供对集群上安装的主要服务的详细监控功能，包括 HDFS、YARN、HIVE、ZOOKEEPER、SPARK、HBase 和 PRESTO 等。本文为您介绍通过控制台查看集群服务状态操作。

## 操作步骤
1. 登录 [EMR 控制台](https://console.cloud.tencent.com/emr)，在【集群列表】中单击对应的集群【ID/名称】进入集群详情页。
2. 在集群详情页面中选择【集群服务】单击对应组件右上角【操作】>【服务状态】，以 HDFS 为例。
![](https://main.qcloudimg.com/raw/02eb6360afd2e6515724a4496ff6e708.png)
3. 服务状态页面提供了三部分服务维度的监控视图，分别为服务概览、服务摘要、角色列表。因各服务组件服务不同，展示部分维度不同。
4. 【服务概览】可直观查看对应时间段服务组件的各项指标及指标各项统计规则，系统默认最多展示6个指标项，可单击【设置指标】自定义展示指标。
![](https://main.qcloudimg.com/raw/12c9e198bd534098a84e6b89ce9f1df8.png)
![](https://main.qcloudimg.com/raw/8955aa65869661cd2422b0195290844f.png)
5. 【服务摘要】展示服务当前整体使用状态。
6. 【角色列表】展示当前服务组件，各角色服务部署情况。单击【节点IP】可查看当前节点监控指标及指标统计规则系统，默认展示6个指标，最多可展示12个指标。
![](https://main.qcloudimg.com/raw/5502b61780676857e2a9a2ce941aa80f.png)
7. 进入某一节点页面，可单击【设置指标】自定义展示指标。
>!
>- 服务监控默认展示 HDFS 服务组件，您可手动调整查看其它服务组件。
>- 因各服务组件服务性质不同，所以服务监控维度部分有所不同，如 HBASE 支持表级监控维度部分。
>
![](https://main.qcloudimg.com/raw/274f47e0f4d7f36d7b7f18a3c6940a9e.png)

## HBase 表负载
### 功能说明
Hbase 表负载提供了 HBASE 中各表的读写请求数以及表存储情况的监控。

### 查看 HBase 表负载
1. 登录 [EMR 控制台](https://console.cloud.tencent.com/emr)，在【集群列表】中单击对应的集群【ID/名称】进入集群详情页。
2. 在集群详情页中选择【集群服务】，选择 HBase 组件的【操作】>【服务状态】>【HBase 表负载】。
3. HBase 表负载展示了 HBase 表的实时读请求量、写请求量、memstore 大小、storeFile 大小四个维度的信息，单击表头可按相应维度进行排序。也可在左上角的搜索框输入表名，单击![](https://main.qcloudimg.com/raw/1f9d505f9e8e4688a73646781370b535.png)进行检索。
![](https://main.qcloudimg.com/raw/d3869744607345a3767186cdf11892da.png)

### 查看表详情
单击对应表名，即可弹出表详情。详情页可按整个表、节点维度展示所选择表的请求量（包括读和写）、store 大小（包括 memstore 和 storeFile）两个指标数据，选择右上角的【节点筛选器】可切换节点查看。
![](https://main.qcloudimg.com/raw/0d54bf6aa3fd5876272e6d91f9f084cf.png)
