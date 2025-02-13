## 操作场景

本文使用了云函数 SCF，并在函数中通过 [MTA 腾讯移动分析](https://mta.qq.com/) 和企业微信机器人实现定时对报表内容进行采集，数据展示等任务。

## 操作步骤

### 创建云函数

1. 登录云函数控制台，选择左侧导航栏中的【[函数服务](https://console.cloud.tencent.com/scf/list)】。
2. 在“函数服务”页面上方选择**北京**地域，并单击【新建】进入新建函数页面，根据页面相关信息提示进行配置。如下图所示：
	![](https://main.qcloudimg.com/raw/64d306ffcafa6376da8eab452d153141.png)
	
	- **创建方式**：选择【模板创建】。
	- **模糊搜索**：输入“定时报表”，并进行搜索。
		单击模板中的【查看详情】，即可在弹出的“模板详情”窗口中查看相关信息，支持下载操作。 
3. 单击【下一步】，函数名称默认填充，可根据需要自行修改。
4. 在【触发器配置】中，选择【自动创建】，则默认创建一个每1小时0分执行一次的定时触发器。如下图所示：
![](https://main.qcloudimg.com/raw/ca3cd6ac890d4f0a7325d6487c2d66cc.png)
   >?
   >- 如需根据需求自行调整触发器配置，请选择【自定义创建】。
   >- 如需在测试成功后再创建定时触发器，请选择【暂不创建】。
5. 单击【完成】，完成函数的创建。


### 测试云函数

1. 在函数代码界面的下方，单击【测试】，查看函数的执行日志，并前往企业微信群确认。
2. 测试成功后，可以根据实际情况，在【触发方式】页签中配置定时触发器。如需对配置进行修改，可参见 [修改函数模板](#puppeteer)。


## 相关操作 

<span id="puppeteer"></span>

### 修改函数模板

当前模板函数仅支持 MTA 站点报表拉取，详情请参见 [MTA 数据接口说明](https://mta.qq.com/docs/h5_api.html)。
添加以下代码，对 MTA 参数进行修改：

```
$app_id ="xxxxxx"; // MTA APPID 
$secret_key = 'xxxxx'; //MTA  SECRET KEY
```

执行以下命令接入微信机器人，使用详情可参见 [微信机器人说明](https://work.weixin.qq.com/help?person_id=1&doc_id=13376)。

```
  CURLOPT_URL => "https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=xxxxx", // 企业微信机器人接口
```
