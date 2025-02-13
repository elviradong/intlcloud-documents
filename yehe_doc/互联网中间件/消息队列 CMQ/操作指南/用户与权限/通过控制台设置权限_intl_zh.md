## 操作场景

本文以指定某用户拥有 CMQ 队列模型的 **消费消息及批量消费消息写权限** 为例，演示如何设置 CMQ 权限。

## 权限说明

接入 CAM 后，子账号除了可以查看 list ，默认是没有其他操作权限的（登录控制台用的是子账号的密钥），需要主账号通过 CAM 授权，才有访问权限。

**子账号通过控制台查看监控数据需要对云监控接口进行 CAM 授权**。


## 操作步骤

### 创建子用户

1. 登录【[访问管理控制台](https://console.cloud.tencent.com/cam)】>【用户】>【用户列表】，单击左上角的【新建用户】。
2. 在新建用户页面，您可以通过快速创建、自定义创建、微信/企业微信导入三种方式创建子用户，具体操作方法请参考访问管理的 [新建子用户](https://intl.cloud.tencent.com/document/product/598/13674) 文档。
3. 在【用户】>【用户列表】中，即可查看已添加的子用户。


### 新建自定义策略

您可以创建某自定义策略指定开启具体 API 接口的权限，下面以指定 CMQ Queue 的写权限（消费消息、批量消费消息）为例：

1. 登录【访问管理】>【[策略](https://console.cloud.tencent.com/cam/policy)】，单击左上角的【新建自定义策略】。
2. 在选择创建策略方式中，选择**按策略生成器创建**，进入配置服务类型页面。
3. 在选择服务和操作页面，补充以下信息。[](id:step3)
  - 服务（必选）：选择队列模型 (cmqqueue)（若未找到请确认是否已经开通 CMQ 服务）。
  - 操作（必选）：选择您要授权的操作。
  - 资源（必填）：填入您要授权的资源的资源六段式。CMQ 的资源六段式描述，例如`qcs::cmqqueue:bj:uin/1238423:queueName/uin/3232/myqueue`。详情参考 [接入 CAM 的 API 授权详情](#接入CAM的API授权详情)
    - 第一段为固定格式 qcs；
    - 第二段为空；
    - 第三段表示消息队列的类型，队列模型为 cmqqueue，主题模型为 cmqtopic；
    - 第四段为地域信息，例如 gz、bj、sh 若为全地域，则设置为空；
    - 第五段为主账号 `uin/{主账号uin}`；
    - 第六段为资源的描述，当为队列模式时，则 `queueName/uin/{创建者Uin}/{队列名字}` ，当为主题模式时该值取 `topicName/uin/{创建者Uin}/{主题名字}`。创建者的 Uin 可以通过控制台详情页获取，或者通过云api 接口 GetQueueAttributes 或者GetTopicAttributes 的返回值 createUin 获取。
  - 条件（选填）：设置子账号上述授权的生效条件。详细可参阅 [生效条件](https://intl.cloud.tencent.com/document/product/598/10608)。

 ![](https://main.qcloudimg.com/raw/0d76bb4ade99b83b0ca2093561e11b5f.png)

4. 单击【添加声明】>【下一步】，进入编辑策略页面。
5. 在策略编辑页面，补充策略名称、描述信息，确认策略内容，其中策略名称和策略内容由控制台自动生成。
  - 策略名称：默认为 "policygen" ，后缀数字根据创建日期生成。您可进行自定义。
  - 策略内容：与 [第 3 步](#step3) 的服务和操作对应，您可根据实际需求进行修改。

 ![](https://main.qcloudimg.com/raw/f7618d543db73c39486aac93a6bfe9f0.png)

6. 单击【完成】，完成按策略生成器创建自定义策略的操作。

7. 在策略列表，选择目标策略，单击操作列的【关联用户/组】，对已开启的权限配置策略的关联对象，选择关联对象后，单击【确认】，完成配置。
 ![](https://main.qcloudimg.com/raw/a56517e94b4e06badb6e76b5eedc63ad.png)

关于 CAM 策略的其他详细说明可参考 [策略](https://intl.cloud.tencent.com/document/product/598/10601) 文档。

>?CMQ 的 list 接口权限默认全部开放（即登录 CMQ 的控制台，可以在控制台看到具体的资源列表），具体查看的资源内容可以通过权限控制。

<span id="接入CAM的API授权详情"></span>
## 接入 CAM 的 API 授权详情

#### 支持资源级授权的 API 列表

| API名                       | API描述              | 资源类型     | 资源六段式示例                                               |
| --------------------------- | -------------------- | ------------ | ------------------------------------------------------------ |
| ClearSubscriptionFilterTags | 清空订阅者消息标签   | 订阅接口相关 | qcs::cmqqueue:$region:uin/{主账号uin}:topicName/uin/{创建者Uin}/{主题名字} |
| CreateSubscribe             | 创建订阅接口         | 订阅接口相关 | qcs::cmqqueue:$region:uin/{主账号uin}:topicName/uin/{创建者Uin}/{主题名字} |
| DeleteSubscribe             | 删除订阅             | 订阅接口相关 | qcs::cmqqueue:$region:uin/{主账号uin}:topicName/uin/{创建者Uin}/{主题名字} |
| ModifySubscriptionAttribute | 修改订阅属性         | 订阅接口相关 | qcs::cmqqueue:$region:uin/{主账号uin}:topicName/uin/{创建者Uin}/{主题名字} |
| CreateTopic                 | 创建主题             | 主题接口相关 | qcs::cmqqueue:$region:uin/{主账号uin}:topicName/uin/{创建者Uin}/{主题名字} |
| DeleteTopic                 | 删除主题             | 主题接口相关 | qcs::cmqqueue:$region:uin/{主账号uin}:topicName/uin/{创建者Uin}/{主题名字} |
| ModifyTopicAttribute        | 修改主题属性         | 主题接口相关 | qcs::cmqqueue:$region:uin/{主账号uin}:topicName/uin/{创建者Uin}/{主题名字} |
| ClearQueue                  | 清空消息队列中的消息 | 队列接口相关 | qcs::cmqqueue:$region:uin/{主账号uin}:queueName/uin/{创建者Uin}/{队列名字} |
| CreateQueue                 | 创建队列接口         | 队列接口相关 | qcs::cmqqueue:$region:uin/{主账号uin}:queueName/uin/{创建者Uin}/{队列名字} |
| DeleteQueue                 | 删除队列             | 队列接口相关 | qcs::cmqqueue:$region:uin/{主账号uin}:queueName/uin/{创建者Uin}/{队列名字} |
| ModifyQueueAttribute        | 修改队列属性         | 队列接口相关 | qcs::cmqqueue:$region:uin/{主账号uin}:queueName/uin/{创建者Uin}/{队列名字} |

#### 不支持资源级授权的 API 列表

| API名                          | API 描述            | 资源类型     | 资源六段式示例 |
| ------------------------------ | ------------------ | ------------ | -------------- |
| DescribeSubscriptionDetail     | 查询订阅详情       | 订阅接口相关 | *              |
| DescribeTopicDetail            | 查询主题详情       | 主题接口相关 | *              |
| DescribeDeadLetterSourceQueues | 枚举死信队列源队列 | 队列接口相关 | *              |
| DescribeQueueDetail            | 枚举队列           | 队列接口相关 | *              |
| RewindQueue                    | 回溯队列           | 队列接口相关 | *              |
| UnbindDeadLetter               | 解绑死信队列       | 队列接口相关 | *              |
