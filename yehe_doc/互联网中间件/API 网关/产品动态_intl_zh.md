## 2021年04月
<table><tr>
<th width="20%">动态名称</th>
<th width="45%">动态描述</th>
<th width="15%">发布时间</th>
<th width="20%">相关文档</th>
</tr><tr>
<td>优化服务日志检索</td>
<td>实时服务日志支持根据环境、路径检索。</td>
<td>2021-04-14</td>
<td>-</td>
</tr><tr>
<td>新增 API 文档功能</td>
<td>您可通过 API 文档功能把托管在 API 网关上的 API 生成一份精美的 API 文档，以提供给第三方调用您的 API。</td>
<td>2021-04-14</td>
<td><a href="https://cloud.tencent.com/document/product/628/54317">API 文档</a></td>
</tr><tr>
<td>新增插件功能</td>
<td>插件是 API 网关提供的高级功能配置，您可以通过插件创建 IP 访问控制等配置项，再将插件绑定到 API 上生效。</td>
<td>2021-04-13</td>
<td><a href="https://cloud.tencent.com/document/product/628/53380">插件</a></td>
</tr><tr>
<td>资源包正式对外开放售卖</td>
<td>API 网关资源包正式对外开放售卖，资源包可抵扣调用次数、出流量产生的费用，价格更优惠。</td>
<td>2021-04-08</td>
<td><a href="https://buy.cloud.tencent.com/apigateway">购买资源包</a></td>
</tr></table>

## 2021年01月
<table><tr>
<th width="20%">动态名称</th>
<th width="45%">动态描述</th>
<th width="15%">发布时间</th>
<th width="20%">相关文档</th>
</tr><tr>
<td>Base64 编码</td>
<td>后端对接云函数 SCF 时，支持开启 Base64 编码功能，API 网关会把请求内容进行 Base64 编码后转发给云函数，以解决二进制文件上传问题。</td>
<td>2021-01-21</td>
<td><a href="https://intl.cloud.tencent.com/document/product/628/39489">Base64 编码</a></td>
</tr><tr>
<td>优化监控</td>
<td>新增服务数据统计功能，支持查看账号下所有服务一天内的数据统计，帮助用户快速定位问题。</td>
<td>2021-01-21</td>
<td>-</td>
</tr></table>

## 2020年12月
<table><tr>
<th width="20%">动态名称</th>
<th width="45%">动态描述</th>
<th width="15%">发布时间</th>
<th width="20%">相关文档</th>
</tr><tr>
<td>自定义域名强制 HTTPS</td>
<td>在自定义域名配置页面，当协议为 HTTPS&HTTPS、HTTPS 时，支持开启强制 HTTPS 功能。开启后，API 网关会将使用该自定义域名的 HTTP 协议的请求重定向至 HTTPS 协议。</td>
<td>2020-12-25</td>
<td><a href="https://intl.cloud.tencent.com/document/product/628/11791">配置自定义域名</a></td>
</tr><tr>
<td>websocket 协议对接 SCF 支持传递 JSON</td>
<td>使用 websocket 协议对接云函数 SCF 时，新增对 JSON 类型数据的支持。</td>
<td>2020-12-18</td>
<td>-</td>
</tr><tr>
<td>触发器数据同步</td>
<td>优化了 API 网关后端对接 SCF 的 API 和云函数 SCF 的 API 网关触发器之间数据不同步的问题，优化后无论哪一侧进行修改，另一侧都会作出相应改变。</td>
<td>2020-12-02</td>
<td>-</td>
</tr></table>

## 2020年11月
<table><tr>
<th width="20%">动态名称</th>
<th width="45%">动态描述</th>
<th width="15%">发布时间</th>
<th width="20%">相关文档</th>
</tr><tr>
<td>支持响应压缩</td>
<td>支持基于 gzip 算法的响应压缩功能，可有效降低数据传输量、减少响应时间、节省服务端网络带宽、提升客户端性能。</td>
<td>2020-11-17</td>
<td><a href="https://intl.cloud.tencent.com/document/product/628/38851">响应压缩</a></td>
</tr></table>

## 2020年10月
<table><tr>
<th width="20%">动态名称</th>
<th width="45%">动态描述</th>
<th width="15%">发布时间</th>
<th width="20%">相关文档</th>
</tr><tr>
<td>调整计费</td>
<td><li>新增资源包计费（预付费），资源包支持通过免费额度和运营活动两种渠道获得。</li><li>免费额度采用资源包实现，新用户开通 API 网关服务后将自动获得 API 网关资源包。</li></td>
<td>2020-10-12</td>
<td><a href="https://intl.cloud.tencent.com/document/product/628/38407">资源包（预付费）</a></td>
</tr><tr>
<td>优化监控</td>
<td>新增 API 数据统计功能，支持查看服务下所有 API 一天内的数据统计，帮助用户快速定位问题。</td>
<td>2020-10-26</td>
<td><a href="https://intl.cloud.tencent.com/document/product/628/38833">查看 API 数据统计</a></td>
</tr></table>

## 2020年09月
<table><tr>
<th width="20%">动态名称</th>
<th width="45%">动态描述</th>
<th width="15%">发布时间</th>
<th width="20%">相关文档</th>
</tr><tr>
<td>新增签名工具</td>
<td>API 网关签名工具是腾讯云 API 网关为用户提供的 Web 工具，可用于生成密钥对认证 API 的请求签名。</td>
<td>2020-09-21</td>
<td><a href="https://intl.cloud.tencent.com/document/product/628/38376">签名工具</a></td>
</tr></table>

## 2020年08月
<table><tr>
<th width="20%">动态名称</th>
<th width="45%">动态描述</th>
<th width="15%">发布时间</th>
<th width="20%">相关文档</th>
</tr><tr>
<td>体验优化</td>
<td><li>支持在线查看 API 文档，并提供了富文本、Markdown 两种格式。</li><li>从 API 详情页返回 API 列表页支持保留搜索参数、分页参数，方便管理 API。</li></td>
<td>2020-08-20</td>
<td>-</td>
</tr><tr>
<td>接入标签</td>
<td>API 网关服务粒度接入标签，支持通过标签管理云资源。</td>
<td>2020-08-05</td>
<td><a href="https://intl.cloud.tencent.com/document/product/651">标签</a></td>
</tr><tr>
<td>体验优化</td>
<td>API 网关新控制台概览页上线，新增快速入口、异常告警、配额限制、最新公告等多个特色模块。</td>
<td>2020-08-05</td>
<td>-</td>
</tr></table>

## 2020年07月
<table><tr>
<th width="20%">动态名称</th>
<th width="45%">动态描述</th>
<th width="15%">发布时间</th>
<th width="20%">相关文档</th>
</tr><tr>
<td>体验优化</td>
<td>API 网关控制台支持一键删除服务、批量删除服务。</td>
<td>2020-07-20</td>
<td><a href="https://intl.cloud.tencent.com/document/product/628/11790">服务删除</a></td>
</tr></table>

## 2020年06月
<table><tr>
<th width="20%">动态名称</th>
<th width="45%">动态描述</th>
<th width="15%">发布时间</th>
<th width="20%">相关文档</th>
</tr><tr>
<td>支持别名配置</td>
<td>API 后端对接云函数 SCF 时，支持配置别名。</td>
<td>2020-06-20</td>
<td>-</td>
</tr></table>

## 2020年05月
<table><tr>
<th width="20%">动态名称</th>
<th width="45%">动态描述</th>
<th width="15%">发布时间</th>
<th width="20%">相关文档</th>
</tr><tr>
<td>实名制要求</td>
<td>增加了实名制的要求，未实名的用户将无法接入 API 网关。</td>
<td>2020-05-31</td>
<td>-</td>
</tr><tr>
<td>日志检索升级</td>
<td>实施服务日志支持运算符检索，可更高效的从海量日志数据中定位问题。</td>
<td>2020-05-14</td>
<td><a href="https://intl.cloud.tencent.com/document/product/628/34636">查看服务日志</a></td>
</tr></table>

## 2020年04月
<table><tr>
<th width="20%">动态名称</th>
<th width="45%">动态描述</th>
<th width="15%">发布时间</th>
<th width="20%">相关文档</th>
</tr><tr>
<td>体验优化</td>
<td>支持查看环境粒度访问监控。</td>
<td>2020-04-15</td>
<td>-</td>
</tr><tr>
<td>体验优化</td>
<td>控制台概览页支持查看月度汇总数据。</td>
<td>2020-04-15</td>
<td>-</td>
</tr><tr>
<td>支持导入 API</td>
<td>支持将 OpenAPI 定义导入 API 网关来创建 API，提升开发体验。</td>
<td>2020-04-10</td>
<td><a href="https://intl.cloud.tencent.com/document/product/628/37875">导入 API</a></td>
</tr><tr>
<td>接入云审计</td>
<td>正式接入云审计，支持用户自主查看操作日志。</td>
<td>2020-04-03</td>
<td><a href="https://intl.cloud.tencent.com/document/product/628/35597">查看操作日志</a></td>
</tr></table>

## 2020年03月
<table><tr>
<th width="20%">动态名称</th>
<th width="45%">动态描述</th>
<th width="15%">发布时间</th>
<th width="20%">相关文档</th>
</tr><tr>
<td>支持云 API 3.0</td>
<td>全量支持云 API 3.0，提升开发体验。</td>
<td>2020-03-26</td>
<td><a href="https://intl.cloud.tencent.com/document/product/628/36595">API 文档</a></td>
</tr><tr>
<td>体验优化</td>
<td>欠费停服时间从2小时延长至24小时。</td>
<td>2020-03-03</td>
<td><a href="https://intl.cloud.tencent.com/document/product/628/11934">欠费说明</a></td>
</tr></table>

## 2020年02月
<table><tr>
<th width="20%">动态名称</th>
<th width="45%">动态描述</th>
<th width="15%">发布时间</th>
<th width="20%">相关文档</th>
</tr><tr>
<td>支持导出服务日志</td>
<td>实时服务日志功能支持日志导出，方便用户进行多维度的数据分析和问题定位。</td>
<td>2020-02-19</td>
<td><a href="https://intl.cloud.tencent.com/document/product/628/37876">导出服务日志</a></td>
</tr><tr>
<td>正式计费</td>
<td>API 网关产品正式商业化售卖，包括调用次数费用和流量费用两个计费项。</td>
<td>2020-02-14</td>
<td><a href="https://intl.cloud.tencent.com/document/product/628/11771">计费概述</a></td>
</tr></table>
