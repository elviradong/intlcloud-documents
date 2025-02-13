## 操作场景
魅族推送通道是由**魅族官方提供**的系统级推送通道。在魅族手机上，推送消息能够通过魅族的系统通道抵达终端，并且无需打开应用，即可收到推送。

>?
>- 魅族推送通道通知标题不超过32字符，通知内容不超过100字符。
>- 魅族推送通道不支持透传消息。
>- 魅族通道支持抵达回调，支持点击回调，不支持透传。

## 操作步骤
### 获取密钥
1. 进入 [魅族推送官网](https://open.flyme.cn/open-web/views/push.html)，注册并登录开发者账号，获取 AppID、AppKey、AppSecret 三个密钥参数。更多详情请参见 [魅族开发文档](http://open.res.flyme.cn/fileserver/upload/file/201709/a271468fe23b47408fc2ec1e282f851f.pdf)。
2. 复制应用的 AppId、AppKey 和 AppSecret 参数填入 【[移动推送 TPNS 控制台](https://console.cloud.tencent.com/tpns)】>【配置管理】>【基础配置】>【魅族官方推送通道】栏目中。


### 集成方式
#### AndroidStudio 集成
```js
implementation 'com.tencent.tpns:meizu:[VERSION]-release'//meizu 推送 [VERSION] 为当前 SDK 版本号，版本号可在 Android SDK 发布动态查看
```
>? 魅族推送 [VERSION] 为当前 SDK 版本号，版本号可在 [Android SDK 发布动态](https://intl.cloud.tencent.com/document/product/1024/36191) 查看。

#### Eclipse 集成
1. 下载 [SDK 安装包](https://console.cloud.tencent.com/tpns/sdkdownload)。
2. 打开 Other-Push-jar 文件夹， 导入魅族推送相关 jar 包，将 mz4tpns1.1.2.1.jar 导入项目工程中。
3. 在 Android manifest 下配置以下配置：

```xml
<application>
<service
                 android:name="com.meizu.cloud.pushsdk.NotificationService"
                 android:exported="true"/>
<receiver android:name="com.meizu.cloud.pushsdk.SystemReceiver" >
 <intent-filter>
                <action android:name="com.meizu.cloud.pushservice.action.PUSH_SERVICE_START"/>
                <category android:name="android.intent.category.DEFAULT" />
 </intent-filter>
 </receiver>
</application>
 <!-- 注：魅族push 需要的权限 begin -->
 <!-- 兼容flyme5.0以下版本，魅族内部集成pushSDK必填，不然无法收到消息-->
<uses-permission android:name="com.meizu.flyme.push.permission.RECEIVE"></uses-permission>
<permission android:name="应用包名.push.permission.MESSAGE" 
                android:protectionLevel="signature"/>
<uses-permission android:name="应用包名.push.permission.MESSAGE"></uses-permission>
<!--  兼容flyme3.0配置权限-->
<uses-permission android:name="com.meizu.c2dm.permission.RECEIVE" />
<permission android:name="应用包名.permission.C2D_MESSAGE"
                android:protectionLevel="signature">
</permission>
<uses-permission android:name="应用包名.permission.C2D_MESSAGE"/>
<!-- 注：魅族push 需要的权限 end -->
```
4. 魅族消息 receiver：在 `AndroidManifest.xml` 增加 `Receiver` 配置如下：

```xml
<receiver android:name="com.tencent.android.mzpush.MZPushMessageReceiver">
    <intent-filter>
        <!-- 接收push消息 -->
        <action android:name="com.meizu.flyme.push.intent.MESSAGE" />
        <!-- 接收register消息-->
         <action android:name="com.meizu.flyme.push.intent.REGISTER.FEEDBACK"/>
        <!-- 接收unregister消息-->
         <action android:name="com.meizu.flyme.push.intent.UNREGISTER.FEEDBACK"/>
         <action android:name="com.meizu.c2dm.intent.REGISTRATION" />
         <action android:name="com.meizu.c2dm.intent.RECEIVE" />
         <category android:name="应用包名"></category>
     </intent-filter>
</receiver>
```

5.  Flyme 6.0 及以下版本的魅族手机，使用手动集成方式，需要在 drawable 不同分辨率的文件夹下对应放置一张名称必须为 stat_sys_third_app_notify 的图片，详情请参考 [TPNS Android SDK](https://console.cloud.tencent.com/tpns/sdkdownload) 中魅族厂商依赖目录的 flyme-notification-res 文件夹。

### 开启魅族推送
在启动移动推送 TPNS ，调用 `XGPushManager.registerPush` 之前，配置如下代码：

```java
//设置魅族APPID和APPKEY
XGPushConfig.enableOtherPush(context, true);
XGPushConfig.setMzPushAppId(this, APP_ID);
XGPushConfig.setMzPushAppKey(this, APP_KEY);
```

注册成功的日志如下：

```java
//成功的获取到移动推送 TPNS 的token和魅族的token，并且绑定成功说明注册成功
I/TPush: [OtherPushClient] handleUpdateToken other push token is : V5R5b7c02********47744c6b635e464b527e487802 other push type: meizu
I/TPush: [PushServiceBroadcastHandler] >> bind OtherPushToken success ack with [accId = 150000****  , rsp = 0]  token = 0398291156ce7d2f****66bd0952c87c372f otherPushType = meizu otherPushToken = V5R5b7c02********47744c6b635e464b527e487802
```

如需通过点击回调获取参数或者跳转自定义页面，您可使用 Intent 来实现。更多详情请参见 [Android 常见问题](https://intl.cloud.tencent.com/document/product/1024/32624)。

### 代码混淆
```xml
-dontwarn com.meizu.cloud.pushsdk.**
-keep class com.meizu.cloud.pushsdk.**{*;}
```
>?若出现 App Debug 版本注册魅族 token 正常，但在 Release 版本中无法获取，请检查是否添加以上代码混淆规则。
- 如果您正在使用 Android Studio 3.4 或以上版本出现上述问题，是因为 Android Studio 默认开启 R8 混淆所导致。
解决方法：请在 gradle.properties 中添加 android.enableR8 = false
- 混淆规则需要放在 App 项目级别的 proguard-rules.pro 文件中。


### 魅族通道抵达回执配置
魅族通道抵达回执需要开发者自行配置，您可参照 [魅族厂商通道回执配置指引](https://intl.cloud.tencent.com/document/product/1024/35246) 进行配置，完成后，可在推送记录中查看魅族推送通道的抵达数据。
