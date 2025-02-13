
资源级权限是指能够指定用户对哪些资源具有执行操作的能力。游戏玩家匹配（Game Player Matchmaking，GPM）支持资源级权限，这意味着对于某些 GSE 的操作，您可以控制何时允许用户执行操作或是允许用户使用的特定资源。下表将向您介绍 GSE 可授权的资源类型。

>! 资源级权限指的是能够指定用户对哪些资源具有执行操作的能力。

在访问管理（Cloud Access Management，CAM）中可授权的资源类型如下：

| 资源类型                      | 授权策略中的资源描述方法              |
| :---------------------------- | ------------------------------------- |
| [匹配相关](#matchCorrelation) | ` qcs::gpm:$region:$account:match/* ` |
| [规则相关](#ruleCorrelation)  | `qcs::gpm:$region:$account:rule/*`    |

[匹配相关](#matchCorrelation) 和 [规则相关](#ruleCorrelation) 分别介绍了当前支持资源级权限的 GPM API 操作。设置资源路径时，您需要将 `$region`、`$account` 等变量参数修改为您实际的参数信息，同时您也可以在路径中使用 `\*` 通配符。相关操作示例介绍，详情可参见 [访问管理示例](https://intl.cloud.tencent.com/document/product/213/10312)。
>! 表中未列出的 GPM API 操作即表示该 GPM API 操作不支持资源级权限。针对不支持资源级权限的 GPM API 操作，您仍可以向用户授予使用该操作的权限，但是策略语句的资源元素必须指定为 `\*`。

<span id="matchCorrelation"></span>

### 匹配相关

| API 操作                                                     | 资源路径                                                     | 说明             |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :--------------- |
| [StartMatching](https://intl.cloud.tencent.com/document/product/1072/39904) | `qcs::gpm:$region:$account:match/*`<br>`qcs::gpm:$region:$account:match/$MatchCode` | 发起匹配         |
| [CancelMatching](https://intl.cloud.tencent.com/document/product/1072/39908) | `qcs::gpm:$region:$account:match/*`<br>`qcs::gpm:$region:$account:match/$MatchCode` | 取消匹配         |
| [DescribeMatchingProgress](https://intl.cloud.tencent.com/document/product/1072/39907) | `qcs::gpm:$region:$account:match/*`<br>`qcs::gpm:$region:$account:match/$MatchCode` | 查询匹配进度     |
| [ModifyMatch ](https://intl.cloud.tencent.com/document/product/1072/39929) | `qcs::gpm:$region:$account:match/*`<br>`qcs::gpm:$region:$account:match/$MatchCode` | 修改匹配         |
| [DeleteMatch  ](https://intl.cloud.tencent.com/document/product/1072/39937) | `qcs::gpm:$region:$account:match/*`<br>`qcs::gpm:$region:$account:match/$MatchCode` | 删除匹配         |
| [DescribeMatch](https://intl.cloud.tencent.com/document/product/1072/39934) | `qcs::gpm:$region:$account:match/*`<br>`qcs::gpm:$region:$account:match/$MatchCode` | 查询匹配详情     |
| [DescribeMatches  ](https://intl.cloud.tencent.com/document/product/1072/39932) | `qcs::gpm:$region:$account:match/*`<br>`qcs::gpm:$region:$account:match/$MatchCode` | 分页查询匹配列表 |
| [DescribeData ](https://intl.cloud.tencent.com/document/product/1072/39935) | `qcs::gpm:$region:$account:match/*`<br>`qcs::gpm:$region:$account:match/$MatchCode` | 查看匹配统计数据 |
| [DescribeMatchCodes](https://intl.cloud.tencent.com/document/product/1072/39933) | `qcs::gpm:$region:$account:match/*`<br>`qcs::gpm:$region:$account:match/$MatchCode` | 分页查询匹配 Code |


<span id="ruleCorrelation"></span>

### 规则相关

| API 操作      | 资源路径                                                     | 说明             |
| :------------ | :----------------------------------------------------------- | :--------------- |
| [ModifyRule](https://intl.cloud.tencent.com/document/product/1072/39928) | `qcs::gpm:$region:$account:rule/*`<br>`qcs::gpm:$region:$account:rule/$RuleCode` | 修改规则         |
| [DeleteRule](https://intl.cloud.tencent.com/document/product/1072/39936) | `qcs::gpm:$region:$account:rule/*`<br>`qcs::gpm:$region:$account:rule/$RuleCode` | 删除规则         |
| [DescribeRule](https://intl.cloud.tencent.com/document/product/1072/39931) | `qcs::gpm:$region:$account:rule/*`<br>`qcs::gpm:$region:$account:rule/$RuleCode` | 查询规则详情     |
| [DescribeRules](https://intl.cloud.tencent.com/document/product/1072/39930) | `qcs::gpm:$region:$account:rule/*`<br>`qcs::gpm:$region:$account:rule/$RuleCode` | 分页查询规则集列表 |





