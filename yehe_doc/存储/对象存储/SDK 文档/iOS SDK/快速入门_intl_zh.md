## 相关资源

- SDK 源码地址请参考： [XML iOS SDK](https://github.com/tencentyun/qcloud-sdk-ios.git)。
- 示例 Demo 可参考：[XML iOS SDK Demo](https://github.com/tencentyun/qcloud-sdk-ios-samples.git)。
- SDK 接口与参数文档请参见 [SDK API 参考](https://cos-ios-sdk-doc-1253960454.file.myqcloud.com)。
- SDK 文档中的所有示例代码请参见 [SDK 代码示例](https://github.com/tencentyun/cos-snippets/tree/master/iOS)。
- SDK 更新日志请参考：[ChangeLog](https://github.com/tencentyun/qcloud-sdk-ios/blob/master/CHANGELOG.md)。

## 准备工作

1. 您需要一个 iOS 应用，这个应用可以是您现有的工程，也可以是您新建的一个空的工程。
2. 请确保应用基于 iOS 8.0 及以上版本的 SDK 构建。
3. 您需要一个可以获取腾讯云临时密钥的远程地址，关于临时密钥的有关说明请参考 [移动应用直传实践](https://intl.cloud.tencent.com/document/product/436/30618)。

## 第一步：安装 SDK

### 方式一：使用 Cocoapods 集成（推荐）

#### 标准版 SDK

在您工程的 `Podfile` 文件中使用：

```shell
pod 'QCloudCOSXML'
```

#### 关闭灯塔上报功能（适用于5.8.3及以上版本）

为了持续跟踪和优化 SDK 的质量，给您带来更好的使用体验，我们在 SDK 中引入了灯塔上报 SDK。

若是想关闭该功能，在您工程的 `Podfile` 文件中使用：

```shell
pod 'QCloudCOSXML/Slim'
```

#### 精简版 SDK

如果您仅仅使用到上传和下载功能，并且对 SDK 体积要求较高，可以使用我们的精简版 SDK。精简版不带有 mta 功能。

精简版 SDK 是通过 Cocoapods 的 Subspec 功能实现的，因此目前只支持通过自动集成的方式。在您工程的 `Podfile` 文件中使用：

```shell
pod 'QCloudCOSXML/Transfer'
```

### 方式二：手动集成

您可以通过 [快速下载地址](https://cos-sdk-archive-1253960454.file.myqcloud.com/qcloud-sdk-ios/latest/qcloud-sdk-ios.zip) 下载最新的正式包，也可以在 [SDK Releases](https://github.com/tencentyun/qcloud-sdk-ios/releases) 里面找到我们所有历史版本的正式包。

#### 1. 导入二进制库

将 **QCloudCOSXML.framework、QCloudCore.framework 和 BeaconAPI_Base.framework 以及 BeaconId.framework** 拖入到工程中。

![](https://main.qcloudimg.com/raw/5bdcb1f0bb6ac5dc80e82b1cd1960a19.png)  

并添加以下依赖库：
 - CoreTelephony
 - Foundation
 - SystemConfiguration
 - libc++.tbd

#### 2. 工程配置

在 Build Settings 中设置 Other Linker Flags，加入以下参数：

```shell
-ObjC
-all_load
```

![](https://main.qcloudimg.com/raw/125218fad3f4781cae8f992d9a152057.png)

## 第二步：开始使用

### 1. 导入头文件

**Objective-c**

```objective-c
#import <QCloudCOSXML/QCloudCOSXML.h>
```

**swift**

```swift
import QCloudCOSXML
```

对于精简版 SDK，请导入：

**Objective-c**

```objective-c
#import <QCloudCOSXML/QCloudCOSXMLTransfer.h>
```

**swift**

```swift
import QCloudCOSXMLTransfer
```

### 2. 创建 COS 服务实例

#### 方式一：获取临时密钥对请求授权（推荐）

建议把初始化过程放在 `AppDelegate` 中。您需要实现以下两个协议：

- QCloudSignatureProvider
- QCloudCredentailFenceQueueDelegate

我们提供了一个 `QCloudCredentailFenceQueue` 的脚手架，实现对临时密钥的缓存与复用。

COS 服务实例 `QCloudCOSXMLService` 跟 `QCloudCOSTransferMangerService` 建议您设计为 **程序单例**。

请参考下面的完整示例代码。

**Objective-c**

```objective-c
//AppDelegate.m
//AppDelegate 需遵循 QCloudSignatureProvider 与 
//QCloudCredentailFenceQueueDelegate 协议

@interface AppDelegate()<QCloudSignatureProvider, QCloudCredentailFenceQueueDelegate>

// 一个脚手架实例
@property (nonatomic) QCloudCredentailFenceQueue* credentialFenceQueue;

@end

@implementation AppDelegate

- (BOOL)application:(UIApplication * )application 
        didFinishLaunchingWithOptions:(NSDictionary * )launchOptions {
    QCloudServiceConfiguration* configuration = [QCloudServiceConfiguration new];
    QCloudCOSXMLEndPoint* endpoint = [[QCloudCOSXMLEndPoint alloc] init];
    // 服务地域简称，例如广州地区是 ap-guangzhou
    endpoint.regionName = @"COS_REGION";
    // 使用 HTTPS
    endpoint.useHTTPS = true;
    configuration.endpoint = endpoint;
    // 密钥提供者为自己
    configuration.signatureProvider = self;
    // 初始化 COS 服务示例
    [QCloudCOSXMLService registerDefaultCOSXMLWithConfiguration:configuration];
    [QCloudCOSTransferMangerService registerDefaultCOSTransferMangerWithConfiguration:
        configuration];

    // 初始化临时密钥脚手架
    self.credentialFenceQueue = [QCloudCredentailFenceQueue new];
    self.credentialFenceQueue.delegate = self;

    return YES;
}

- (void) fenceQueue:(QCloudCredentailFenceQueue * )queue requestCreatorWithContinue:(QCloudCredentailFenceQueueContinue)continueBlock
{
    //这里同步从后台服务器获取临时密钥
    //...

    QCloudCredential* credential = [QCloudCredential new];
    // 临时密钥 SecretId
    credential.secretID = @"COS_SECRETID";
    // 临时密钥 SecretKey
    credential.secretKey = @"COS_SECRETKEY";
    // 临时密钥 Token
    credential.token = @"COS_TOKEN";
    // 强烈建议返回服务器时间作为签名的开始时间
    // 用来避免由于用户手机本地时间偏差过大导致的签名不正确
    credential.startDate = [[[NSDateFormatter alloc] init] 
        dateFromString:@"startTime"]; // 单位是秒
    credential.experationDate = [[[NSDateFormatter alloc] init] 
        dateFromString:@"expiredTime"];

    QCloudAuthentationV5Creator* creator = [[QCloudAuthentationV5Creator alloc]
        initWithCredential:credential];
    continueBlock(creator, nil);
}

// 获取签名的方法入口，这里演示了获取临时密钥并计算签名的过程
// 您也可以自定义计算签名的过程
- (void) signatureWithFields:(QCloudSignatureFields*)fileds
                     request:(QCloudBizHTTPRequest*)request
                  urlRequest:(NSMutableURLRequest*)urlRequst
                   compelete:(QCloudHTTPAuthentationContinueBlock)continueBlock
{
    [self.credentialFenceQueue performAction:^(QCloudAuthentationCreator *creator, 
        NSError *error) {
        if (error) {
            continueBlock(nil, error);
        } else {
            QCloudSignature* signature =  [creator signatureForData:urlRequst];
            continueBlock(signature, nil);
        }
    }];
}


@end
```

**Swift**

```swift
//AppDelegate.swift
//AppDelegate 需遵循 QCloudSignatureProvider 与 
//QCloudCredentailFenceQueueDelegate 协议

class AppDelegate: UIResponder, UIApplicationDelegate,
    QCloudSignatureProvider, QCloudCredentailFenceQueueDelegate {

    var credentialFenceQueue:QCloudCredentailFenceQueue?;

    func application(_ application: UIApplication, 
        didFinishLaunchingWithOptions launchOptions: 
        [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        let config = QCloudServiceConfiguration.init();

        let endpoint = QCloudCOSXMLEndPoint.init();
        //服务地域简称，例如广州地区是 ap-guangzhou
        endpoint.regionName = "COS_REGION";
        // 使用 HTTPS
        endpoint.useHTTPS = true;
        config.endpoint = endpoint;
        // 密钥提供者为自己
        config.signatureProvider = self;

        // 初始化 COS 服务示例
        QCloudCOSXMLService.registerDefaultCOSXML(with: config);
        QCloudCOSTransferMangerService.registerDefaultCOSTransferManger(
            with: config);

        // 初始化临时密钥脚手架
        self.credentialFenceQueue = QCloudCredentailFenceQueue.init();
        self.credentialFenceQueue?.delegate = self;

        return true
    }

    func fenceQueue(_ queue: QCloudCredentailFenceQueue!, 
        requestCreatorWithContinue continueBlock: 
        QCloudCredentailFenceQueueContinue!) {
        //这里同步从后台服务器获取临时密钥
        //...

        let credential = QCloudCredential.init();
        // 临时密钥 SecretId
        credential.secretID = "COS_SECRETID";
        // 临时密钥 SecretKey
        credential.secretKey = "COS_SECRETKEY";
        // 临时密钥 Token
        credential.token = "COS_TOKEN";
        // 强烈建议返回服务器时间作为签名的开始时间
        // 用来避免由于用户手机本地时间偏差过大导致的签名不正确
        credential.startDate = DateFormatter().date(from: "startTime");
        // 这里返回的时间单位是秒
        credential.experationDate = DateFormatter().date(from: "expiredTime");

        let auth = QCloudAuthentationV5Creator.init(credential: credential);
        continueBlock(auth,nil);
    }

    // 获取签名的方法入口，这里演示了获取临时密钥并计算签名的过程
    // 您也可以自定义计算签名的过程
    func signature(with fileds: QCloudSignatureFields!, 
        request: QCloudBizHTTPRequest!, 
        urlRequest urlRequst: NSMutableURLRequest!, 
        compelete continueBlock: QCloudHTTPAuthentationContinueBlock!) {
        self.credentialFenceQueue?.performAction({ (creator, error) in
            if error != nil {
                continueBlock(nil,error!);
            }else{
                let signature = creator?.signature(forData: urlRequst);
                continueBlock(signature,nil);
            }
        })
    }
}
```

>!
>- 关于存储桶不同地域的简称请参考 [地域和访问域名](https://intl.cloud.tencent.com/document/product/436/6224)。
>- 建议您使用 HTTPS 来请求数据，但如果您希望使用 HTTP 协议，为了确保在 iOS 9.0 以上的系统上可以运行，您需要为应用开启允许通过 HTTP 传输。详细的指引请参考 Apple 官方的说明文档 [Preventing Insecure Network Connections](https://developer.apple.com/documentation/security/preventing_insecure_network_connections)。

如果您的 `QCloudServiceConfiguration` 发生改变，可以通过以下方法注册一个新的实例：

```objective-c
+ (QCloudCOSTransferMangerService*) registerCOSTransferMangerWithConfiguration:(QCloudServiceConfiguration*)configuration withKey:(NSString*)key;
```

#### 方式二：使用永久密钥进行本地调试

您可以使用腾讯云的永久密钥来进行开发阶段的本地调试。**由于该方式存在泄漏密钥的风险，请务必在上线前替换为临时密钥的方式。**

在使用永久密钥时，您可以不实现 `QCloudCredentailFenceQueueDelegate` 协议。

**Objective-C**

```objective-c
- (void) signatureWithFields:(QCloudSignatureFields*)fileds
                     request:(QCloudBizHTTPRequest*)request
                  urlRequest:(NSMutableURLRequest*)urlRequst
                   compelete:(QCloudHTTPAuthentationContinueBlock)continueBlock
{
    
    QCloudCredential* credential = [QCloudCredential new];
    credential.secretID = @"COS_SECRETID"; // 永久密钥 SecretId
    credential.secretKey = @"COS_SECRETKEY"; // 永久密钥 SecretKey

    // 使用永久密钥计算签名
    QCloudAuthentationV5Creator* creator = [[QCloudAuthentationV5Creator alloc] 
        initWithCredential:credential];
    QCloudSignature* signature = [creator signatureForData:urlRequst];
    continueBlock(signature, nil);
}
```

**Swift**

```swift
func signature(with fileds: QCloudSignatureFields!, 
                request: QCloudBizHTTPRequest!, 
                urlRequest urlRequst: NSMutableURLRequest!, 
                compelete continueBlock: QCloudHTTPAuthentationContinueBlock!) {
    let credential = QCloudCredential.init();
    credential.secretID = "COS_SECRETID"; // 永久密钥 SecretId
    credential.secretKey = "COS_SECRETKEY"; // 永久密钥 SecretKey

    // 使用永久密钥计算签名
    let auth = QCloudAuthentationV5Creator.init(credential: credential);
    let signature = auth?.signature(forData: urlRequst)
    continueBlock(signature,nil);
}
```

#### 方式三：使用后台计算的签名对请求授权

在签名过程放在后台时，您可以不实现 `QCloudCredentailFenceQueueDelegate` 协议。

**Objective-C**

```objective-c
- (void) signatureWithFields:(QCloudSignatureFields*)fileds
                     request:(QCloudBizHTTPRequest*)request
                  urlRequest:(NSMutableURLRequest*)urlRequst
                   compelete:(QCloudHTTPAuthentationContinueBlock)continueBlock
{
    // 签名过期时间
    NSDate *expiration = [[[NSDateFormatter alloc] init] 
                            dateFromString:@"expiredTime"];
    QCloudSignature *sign = [[QCloudSignature alloc] initWithSignature:
        @"后台计算好的签名" expiration:expiration];
    continueBlock(signature, nil);
}
```

**Swift**

```swift
func signature(with fileds: QCloudSignatureFields!, 
                request: QCloudBizHTTPRequest!, 
                urlRequest urlRequst: NSMutableURLRequest!, 
                compelete continueBlock: QCloudHTTPAuthentationContinueBlock!) {
    // 签名过期时间
    let expiration = DateFormatter().date(from: "expiredTime");
    let sign = QCloudSignature.init(signature: "后台计算好的签名", 
                expiration: expiration);
    continueBlock(signature,nil);
}
```

## 第三步：访问 COS 服务

### 上传对象

SDK 支持上传本地文件与二进制数据 NSData。下面以上传本地文件为例。

**Objective-C**

```objective-c
QCloudCOSXMLUploadObjectRequest* put = [QCloudCOSXMLUploadObjectRequest new];
// 本地文件路径
NSURL* url = [NSURL fileURLWithPath:@"文件的URL"];
// 存储桶名称，格式为 BucketName-APPID
put.bucket = @"examplebucket-1250000000";
// 对象键，是对象在 COS 上的完整路径，如果带目录的话，格式为 "dir1/object1"
put.object = @"exampleobject";
//需要上传的对象内容。可以传入NSData*或者NSURL*类型的变量
put.body =  url;
//监听上传进度
[put setSendProcessBlock:^(int64_t bytesSent,
                            int64_t totalBytesSent,
                            int64_t totalBytesExpectedToSend) {
    //      bytesSent                 本次要发送的字节数（一个大文件可能要分多次发送）
    //      totalBytesSent            已发送的字节数
    //      totalBytesExpectedToSend  本次上传要发送的总字节数（即一个文件大小）
}];

//监听上传结果
[put setFinishBlock:^(id outputObject, NSError *error) {
    //可以从 outputObject 中获取 response 中 etag 或者自定义头部等信息
    NSDictionary * result = (NSDictionary *)outputObject;
}];

[[QCloudCOSTransferMangerService defaultCOSTransferManager] UploadObject:put];
```

>?
>- 更多完整示例，请前往 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/TransferUploadObject.m) 查看。
>- 上传之后，您可以用同样的 Key 生成文件下载链接，具体使用方法见 **生成预签名链接** 文档。但注意如果您的文件是私有读权限，那么下载链接只有一定的有效期。

**Swift**

```swift
let put:QCloudCOSXMLUploadObjectRequest = QCloudCOSXMLUploadObjectRequest<AnyObject>();
// 存储桶名称，格式为 BucketName-APPID
put.bucket = "examplebucket-1250000000";
// 对象键，是对象在 COS 上的完整路径，如果带目录的话，格式为 "dir1/object1"
put.object = "exampleobject";
//需要上传的对象内容。可以传入NSData*或者NSURL*类型的变量
put.body = NSURL.fileURL(withPath: "Local File Path") as AnyObject;

//监听上传结果
put.setFinish { (result, error) in
    // 获取上传结果
    if error != nil{
        print(error!);
    }else{
        print(result!);
    }
}

//监听上传进度
put.sendProcessBlock = { (bytesSent, totalBytesSent,
    totalBytesExpectedToSend) in
    //      bytesSent                 本次要发送的字节数（一个大文件可能要分多次发送）
    //      totalBytesSent            已发送的字节数
    //      totalBytesExpectedToSend  本次上传要发送的总字节数（即一个文件大小）
};
//设置上传参数
put.initMultipleUploadFinishBlock = {(multipleUploadInitResult, resumeData) in
    //在初始化分块上传完成以后会回调该 block，在这里可以获取 resumeData
    //并且可以通过 resumeData 生成一个分块上传的请求
    let resumeUploadRequest = QCloudCOSXMLUploadObjectRequest<AnyObject>
        .init(request: resumeData as Data?);
}

QCloudCOSTransferMangerService.defaultCOSTransferManager().uploadObject(put);
```

>?
>- 更多完整示例，请前往 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/TransferUploadObject.swift) 查看。
>- 上传之后，您可以用同样的 Key 生成文件下载链接，具体使用方法见 **生成预签名链接** 文档。但注意如果您的文件是私有读权限，那么下载链接只有一定的有效期。

### 下载对象

**Objective-C**

```objective-c
QCloudCOSXMLDownloadObjectRequest * request = [QCloudCOSXMLDownloadObjectRequest new];
    
// 存储桶名称，格式为 BucketName-APPID
request.bucket = @"examplebucket-1250000000";
// 对象键，是对象在 COS 上的完整路径，如果带目录的话，格式为 "dir1/object1"
request.object = @"exampleobject";

//设置下载的路径 URL，如果设置了，文件将会被下载到指定路径中
request.downloadingURL = [NSURL fileURLWithPath:@"Local File Path"];

//监听下载结果
[request setFinishBlock:^(id outputObject, NSError *error) {
    //outputObject 包含所有的响应 http 头部
    NSDictionary* info = (NSDictionary *) outputObject;
}];

//监听下载进度
[request setDownProcessBlock:^(int64_t bytesDownload,
                                int64_t totalBytesDownload,
                                int64_t totalBytesExpectedToDownload) {
    //      bytesDownload                   本次要下载的字节数（一个大文件可能要分多次发送）
    //      totalBytesDownload              已下载的字节数
    //      totalBytesExpectedToDownload    本次要下载的总字节数（即一个文件大小）
}];

[[QCloudCOSTransferMangerService defaultCOSTransferManager] DownloadObject:request];
```

>?更多完整示例，请前往 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/TransferDownloadObject.m) 查看。

**Swift**

```swift
let request : QCloudCOSXMLDownloadObjectRequest = QCloudCOSXMLDownloadObjectRequest();
        
// 文件所在桶
request.bucket = "examplebucket-1250000000";
// 对象键
request.object = "exampleobject";

//设置下载的路径 URL，如果设置了，文件将会被下载到指定路径中
request.downloadingURL = NSURL.fileURL(withPath: "Local File Path") as URL?;

//监听下载进度
request.sendProcessBlock = { (bytesDownload, totalBytesDownload,
    totalBytesExpectedToDownload) in
    //      bytesDownload                   本次要下载的字节数（一个大文件可能要分多次发送）
    //      totalBytesDownload              已下载的字节数
    //      totalBytesExpectedToDownload    本次要下载的总字节数（即一个文件大小）
}

//监听下载结果
request.finishBlock = { (copyResult, error) in
    if error != nil{
        print(error!);
    }else{
        print(copyResult!);
    }
}

QCloudCOSTransferMangerService.defaultCOSTransferManager().downloadObject(request);
```

>?更多完整示例，请前往 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/TransferDownloadObject.swift) 查看。

