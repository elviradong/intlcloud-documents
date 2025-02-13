## 简介

本文档提供关于对象的高级接口，简单操作、分块操作相关的 API 概览以及 SDK 示例代码。

**简单操作**

| API                                                          | 操作名         | 操作描述                                  |
| ------------------------------------------------------------ | -------------- | ----------------------------------------- |
| [GET Bucket（List Objects）](https://intl.cloud.tencent.com/document/product/436/30614) | 查询对象列表   | 查询存储桶下的部分或者全部对象            |
| [PUT Object](https://intl.cloud.tencent.com/document/product/436/7749) | 简单上传对象   | 上传一个对象至存储桶                      |
| [HEAD Object](https://intl.cloud.tencent.com/document/product/436/7745) | 查询对象元数据 | 查询对象的元数据信息                  |
| [GET Object](https://intl.cloud.tencent.com/document/product/436/7753) | 下载对象       | 下载一个对象至本地        |
| [PUT Object - Copy](https://intl.cloud.tencent.com/document/product/436/10881) | 设置对象复制   | 复制文件到目标路径                        |
| [DELETE Object](https://intl.cloud.tencent.com/document/product/436/7743) | 删除单个对象   | 在存储桶中删除指定对象 |
| [DELETE Multiple Objects](https://intl.cloud.tencent.com/document/product/436/8289) | 删除多个对象   | 在存储桶中批量删除指定对象                |
| [POST Object restore](https://intl.cloud.tencent.com/document/product/436/12633) | 恢复归档对象 | 将归档类型的对象取回访问           |

**分块操作**

| API                                                          | 操作名         | 操作描述                             |
| ------------------------------------------------------------ | -------------- | ------------------------------------ |
| [List Multipart Uploads](https://intl.cloud.tencent.com/document/product/436/7736) | 查询分块上传   | 查询正在进行中的分块上传信息         |
| [Initiate Multipart Upload](https://intl.cloud.tencent.com/document/product/436/7746) | 初始化分块上传 | 初始化 Multipart Upload 上传操作     |
| [Upload Part](https://intl.cloud.tencent.com/document/product/436/7750) | 上传分块       | 分块上传文件                         |
| [Upload Part - Copy](https://intl.cloud.tencent.com/document/product/436/8287) | 复制分块       | 将其他对象复制为一个分块             |
| [List Parts](https://intl.cloud.tencent.com/document/product/436/7747) | 查询已上传块   | 查询特定分块上传操作中的已上传的块   |
| [Complete Multipart Upload](https://intl.cloud.tencent.com/document/product/436/7742) | 完成分块上传   | 完成整个文件的分块上传               |
| [Abort Multipart Upload](https://intl.cloud.tencent.com/document/product/436/7740) | 终止分块上传   | 终止一个分块上传操作并删除已上传的块 |

## 简单操作

### 查询对象列表

#### 功能说明

查询存储桶下的部分或者全部对象。

#### 方法原型

```java
public ObjectListing listObjects(ListObjectsRequest listObjectsRequest) throws CosClientException, CosServiceException;
```

#### 请求示例

[//]: # (.cssg-snippet-get-bucket)
```java
// Bucket的命名格式为 BucketName-APPID ，此处填写的存储桶名称必须为此格式
String bucketName = "examplebucket-1250000000";
ListObjectsRequest listObjectsRequest = new ListObjectsRequest();
// 设置bucket名称
listObjectsRequest.setBucketName(bucketName);
// prefix表示列出的object的key以prefix开始
listObjectsRequest.setPrefix("images/");
// deliter表示分隔符, 设置为/表示列出当前目录下的object, 设置为空表示列出所有的object
listObjectsRequest.setDelimiter("/");
// 设置最大遍历出多少个对象, 一次listobject最大支持1000
listObjectsRequest.setMaxKeys(1000);
ObjectListing objectListing = null;
do {
    try {
        objectListing = cosClient.listObjects(listObjectsRequest);
    } catch (CosServiceException e) {
        e.printStackTrace();
        return;
    } catch (CosClientException e) {
        e.printStackTrace();
        return;
    }
    // common prefix表示表示被delimiter截断的路径, 如delimter设置为/, common prefix则表示所有子目录的路径
    List<String> commonPrefixs = objectListing.getCommonPrefixes();

    // object summary表示所有列出的object列表
    List<COSObjectSummary> cosObjectSummaries = objectListing.getObjectSummaries();
    for (COSObjectSummary cosObjectSummary : cosObjectSummaries) {
        // 文件的路径key
        String key = cosObjectSummary.getKey();
        // 文件的etag
        String etag = cosObjectSummary.getETag();
        // 文件的长度
        long fileSize = cosObjectSummary.getSize();
        // 文件的存储类型
        String storageClasses = cosObjectSummary.getStorageClass();
    }

    String nextMarker = objectListing.getNextMarker();
    listObjectsRequest.setMarker(nextMarker);
} while (objectListing.isTruncated());
```


#### 参数说明

| 参数名称           | 描述             | 类型               |
| ------------------ | ---------------- | ------------------ |
| listObjectsRequest | 获取文件列表请求 | ListObjectsRequest |

Request 成员说明 ：

| Request 成员 | 设置方法&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;            | 描述                                                         | 类型    |
| ------------ | ------------------- | ------------------------------------------------------------ | ------- |
| bucketName   | 构造函数或 set 方法 | Bucket 的命名格式为 BucketName-APPID ，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String  |
| prefix       | 构造函数或 set 方法 | 限制返回的结果对象，以 prefix 为前缀。默认不进行限制，即 Bucket 下所有的成员。<br>默认值为空： "" | String  |
| marker       | 构造函数或 set 方法 | 标记 list 的起点位置，第一次可设置为空，后续请求需设置为上一次 listObjects 返回值中的 nextMarker | String  |
| delimiter    | 构造函数或 set 方法 | 分隔符，限制返回的是以 prefix 开头，并以 delimiter 第一次出现的结束的路径 | String  |
| maxKeys      | 构造函数或 set 方法 | 最大返回的成员个数（不得超过1000)。<br>默认值： 1000          | Integer |

#### 返回结果说明

- 成功：返回 ObjectListing 类型， 包含所有的成员， 以及 nextMarker。  
- 失败：抛出异常 CosClientException 或者 CosServiceException。详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。


### 简单上传对象

#### 功能说明

上传对象到指定的存储桶中（PUT Object）。将本地文件或者已知长度的输入流内容上传到 COS。适用于图片类小文件上传（20MB以下），最大支持5GB（含），5GB以上请使用 [分块上传](#.E5.88.86.E5.9D.97.E6.93.8D.E4.BD.9C) 或 [高级 API](#.E9.AB.98.E7.BA.A7.E6.8E.A5.E5.8F.A3.EF.BC.88.E6.8E.A8.E8.8D.90.EF.BC.89) 上传。

- 上传过程中默认会对文件长度与 MD5 进行校验（关闭 MD5 校验参见示例代码）。
- 若 COS 上已存在同样 Key 的对象，上传时则会进行覆盖。
- 上传之后，您可以用同样的 key，调用 GetObject 接口将文件下载到本地，也可以生成 [预签名链接](https://intl.cloud.tencent.com/document/product/436/31536)（下载请指定 method 为 GET，具体接口说明见下文），发送到其他端来进行下载。

#### 方法原型

```java
// 方法1  将本地文件上传到 COS
public PutObjectResult putObject(String bucketName, String key, File file)
            throws CosClientException, CosServiceException;
// 方法2  输入流上传到 COS
public PutObjectResult putObject(String bucketName, String key, InputStream input, ObjectMetadata metadata)
            throws CosClientException, CosServiceException;
// 方法3  对以上两个方法的包装, 支持更细粒度的参数控制, 如 content-type,  content-disposition 等
public PutObjectResult putObject(PutObjectRequest putObjectRequest)
            throws CosClientException, CosServiceException;
```

#### 请求示例

[//]: # (.cssg-snippet-put-object-flex)
```java
// Bucket的命名格式为 BucketName-APPID ，此处填写的存储桶名称必须为此格式
String bucketName = "examplebucket-1250000000";
// 方法1 本地文件上传
File localFile = new File(localFilePath);
String key = "exampleobject";
PutObjectResult putObjectResult = cosClient.putObject(bucketName, key, localFile);
String etag = putObjectResult.getETag();  // 获取文件的 etag

// 方法2 从输入流上传(需提前告知输入流的长度, 否则可能导致 oom)
FileInputStream fileInputStream = new FileInputStream(localFile);
ObjectMetadata objectMetadata = new ObjectMetadata();
// 设置输入流长度为500
objectMetadata.setContentLength(500);
// 设置 Content type, 默认是 application/octet-stream
objectMetadata.setContentType("application/pdf");
putObjectResult = cosClient.putObject(bucketName, key, fileInputStream, objectMetadata);
etag = putObjectResult.getETag();
// 关闭输入流...

// 方法3 提供更多细粒度的控制, 常用的设置如下
// 1 storage-class 存储类型, 枚举值：Standard，Standard_IA，Archive。默认值：Standard。更多存储类型请参见 https://intl.cloud.tencent.com/zh/document/product/436/30925
// 2 content-type, 对于本地文件上传，默认根据本地文件的后缀进行映射，例如 jpg 文件映射 为image/jpeg
//   对于流式上传 默认是 application/octet-stream
// 3 上传的同时指定权限(也可通过调用 API set object acl 来设置)
// 4 若要全局关闭上传MD5校验, 则设置系统环境变量，此设置会对所有的会影响所有的上传校验。 默认是进行校验的。
// 关闭MD5校验：  System.setProperty(SkipMd5CheckStrategy.DISABLE_PUT_OBJECT_MD5_VALIDATION_PROPERTY, "true");
// 打开MD5校验  System.setProperty(SkipMd5CheckStrategy.DISABLE_PUT_OBJECT_MD5_VALIDATION_PROPERTY, null);
localFile = new File(localFilePath);
key = "picture.jpg";
PutObjectRequest putObjectRequest = new PutObjectRequest(bucketName, key, localFile);
// 设置存储类型为低频
putObjectRequest.setStorageClass(StorageClass.Standard_IA);
// 设置自定义属性(如 content-type, content-disposition 等)
objectMetadata = new ObjectMetadata();
// 限流使用的单位是 bit/s, 这里设置上传带宽限制为 10MB/s
putObjectRequest.setTrafficLimit(80*1024*1024);
// 设置 Content type, 默认是 application/octet-stream
objectMetadata.setContentType("image/jpeg");
putObjectRequest.setMetadata(objectMetadata);
putObjectResult = cosClient.putObject(putObjectRequest);
// 获取对象的 Etag
etag = putObjectResult.getETag();
// 获取对象的 CRC64
String crc64Ecma = putObjectResult.getCrc64Ecma();
```


#### 参数说明

| 参数名称         | 描述         | 类型             |
| ---------------- | ------------ | ---------------- |
| putObjectRequest | 上传文件请求 | PutObjectRequest |

Request 成员说明：

| Request 成员 | 设置方法&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   | 描述                                                         | 类型   | 必填 |
| ------------ | ------------------- | ------------------------------------------------------------ | -------------- |---|
| bucketName   | 构造函数或 set 方法 | Bucket 的命名格式为 BucketName-APPID，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String         | 是|
| key          | 构造函数或 set 方法 | 对象键（Key）是对象在存储桶中的唯一标识。<br>例如，在对象的访问域名`examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/doc/picture.jpg`中，对象键为 doc/picture.jpg，详情请参见 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String         | 是|
| file         | 构造函数或 set 方法 | 本地文件                                                     | File           |否|
| input        | 构造函数或 set 方法 | 输入流                                                       | InputStream    | 否|
| metadata     | 构造函数或 set 方法 | 对象的元数据                                                 | ObjectMetadata | 否|
|trafficLimit | set 方法| 用于对上传对象进行流量控制，单位：bit/s，默认不进行流量控制 | Int|否|

ObjectMetadata 类用于记录对象的元信息，其主要成员说明如下：

| 成员名称        | 描述                                                | 类型                |
| --------------- | --------------------------------------------------- | ------------------- |
| httpExpiresDate | 缓存的超时时间，为 HTTP 响应头部中 Expires 字段的值 | Date                |
| ongoingRestore  | 正在从归档存储类型恢复该对象                        | Boolean             |
| userMetadata    | 前缀为 x-cos-meta- 的用户自定义元信息               | Map<String, String> |
| metadata        | 除用户自定义元信息以外的其他头部                    | Map<String, String> |
| restoreExpirationTime  | 归档对象恢复副本的过期时间  | Date |


#### 返回结果说明

- 成功：PutObjectResult，包含文件的 eTag 等信息。
- 失败：发生错误（如身份认证失败），抛出异常 CosClientException 或者 CosServiceException。详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。

#### 返回参数说明

PutObjectResult 类用于返回结果信息，其主要成员说明如下：

| 成员名称        | 描述                                                | 类型                |
| --------------- | --------------------------------------------------- | ------------------- |
| requestId | 请求 ID | String                |
| dateStr  | 当前服务端时间                        | String             |
| versionId    | 开启版本控制的存储桶，返回对象的版本号 ID              | String |
| eTag        | 简单上传接口返回对象的 MD5 值                    | String |
|crc64Ecma| 服务端根据对象内容计算出来的 CRC64| String |


### 查询对象元数据

#### 功能说明

查询存储桶中是否存在指定的对象。

#### 方法原型

```java
public ObjectMetadata getObjectMetadata(String bucketName, String key)
            throws CosClientException, CosServiceException;
```

#### 请求示例

[//]: # (.cssg-snippet-head-object)
```java
// Bucket的命名格式为 BucketName-APPID ，此处填写的存储桶名称必须为此格式
String bucketName = "examplebucket-1250000000";
String key = "exampleobject";
ObjectMetadata objectMetadata = cosClient.getObjectMetadata(bucketName, key);
```


#### 参数说明

| 参数名称   | 描述                                                         | 类型   |
| ---------- | ------------------------------------------------------------ | ------ |
| bucketName | Bucket 的命名格式为 BucketName-APPID ，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String |
| key        | 对象键（Key）是对象在存储桶中的唯一标识。例如，在对象的访问域名 `examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/doc/picture.jpg` 中，对象键为 doc/picture.jpg，详情请参见 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String |

#### 返回结果说明

- 成功：返回 ObjectMetadata 类型， 包含用户自定义头部、Etag等对象元信息。
- 失败：发生错误（如身份认证失败），抛出异常 CosClientException 或者 CosServiceException。详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。

#### 返回参数说明

ObjectMetadata 类用于记录对象的元信息，其主要成员说明如下：

| 成员名称        | 描述                                                | 类型                |
| --------------- | --------------------------------------------------- | ------------------- |
| httpExpiresDate | 缓存的超时时间，为 HTTP 响应头部中 Expires 字段的值 | Date                |
| ongoingRestore  | 正在从归档存储类型恢复该对象                        | Boolean             |
| userMetadata    | 前缀为 x-cos-meta- 的用户自定义元信息               | Map<String, String> |
| metadata        | 除用户自定义元信息以外的其他头部                    | Map<String, String> |
| restoreExpirationTime  | 归档对象恢复副本的过期时间  | Date |


### 下载对象

#### 功能说明

下载对象到本地（GET Object）。

#### 方法原型

```java
// 方法1 下载文件，并获取输入流
public COSObject getObject(GetObjectRequest getObjectRequest)
            throws CosClientException, CosServiceException;
// 方法2 下载文件到本地.
public ObjectMetadata getObject(GetObjectRequest getObjectRequest, File destinationFile)
            throws CosClientException, CosServiceException;
```

#### 请求示例

[//]: # (.cssg-snippet-get-object)
```java
// Bucket的命名格式为 BucketName-APPID ，此处填写的存储桶名称必须为此格式
String bucketName = "examplebucket-1250000000";
String key = "exampleobject";
// 方法1 获取下载输入流
GetObjectRequest getObjectRequest = new GetObjectRequest(bucketName, key);
// 限流使用的单位是 bit/s, 这里设置下载带宽限制为10MB/s
getObjectRequest.setTrafficLimit(80*1024*1024);
COSObject cosObject = cosClient.getObject(getObjectRequest);
COSObjectInputStream cosObjectInput = cosObject.getObjectContent();
// 下载对象的 CRC64
String crc64Ecma = cosObject.getObjectMetadata().getCrc64Ecma();
// 关闭输入流
cosObjectInput.close();

// 方法2 下载文件到本地
String outputFilePath = "exampleobject";
File downFile = new File(outputFilePath);
getObjectRequest = new GetObjectRequest(bucketName, key);
ObjectMetadata downObjectMeta = cosClient.getObject(getObjectRequest, downFile);
```


#### 参数说明

| 参数名称         | 描述           | 类型             |
| ---------------- | -------------- | ---------------- |
| getObjectRequest | 下载文件请求   | GetObjectRequest |
| destinationFile  | 本地的保存文件 | File             |

Request 成员说明：

| Request 成员 | 设置方法&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;            | 描述                                                         | 类型   |
| ------------ | ------------------- | ------------------------------------------------------------ | ------ |
| bucketName   | 构造函数或 set 方法 | Bucket 的命名格式为 BucketName-APPID ，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String |
| key          | 构造函数或 set 方法 | 对象键（Key）是对象在存储桶中的唯一标识。例如，在对象的访问域名 `examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/doc/picture.jpg` 中，对象键为 doc/picture.jpg，详情请参见 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String |
| range  | set 方法             | 下载的 range 范围                                            | Long[] |
| trafficLimit | set 方法    | 用于对下载对象进行流量控制，单位：bit/s，默认不进行流量控制  | Int |


#### 返回结果说明

- **方法1 （获取下载输入流）**
  - 成功：返回 COSObject 类，包含输入流以及对象属性。
  - 失败：发生错误（如身份认证失败），抛出异常 CosClientException 或者 CosServiceException。详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。
- **方法2 （下载文件到本地）**
  - 成功：返回文件的属性 ObjectMetadata，包含文件的自定义头和 content-type 等属性。
  - 失败：发生错误（如身份认证失败），抛出异常 CosClientException 或者 CosServiceException。详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。

#### 返回参数说明

COSObject 类用于返回结果信息，其主要成员说明如下：

| 成员名称        | 描述                                                | 类型                |
| --------------- | --------------------------------------------------- | ------------------- |
| bucketName   |  Bucket 的命名格式为 BucketName-APPID ，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String |
| key          | 对象键（Key）是对象在存储桶中的唯一标识。例如，在对象的访问域名`examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/doc/picture.jpg` 中，对象键为 doc/picture.jpg，详情请参见 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String |
| metadata | 对象的元数据  | ObjectMetadata |
| objectContent | 包含 COS 对象内容的数据流  | COSObjectInputStream |


### 设置对象复制

#### 功能说明 

将一个对象复制到另一个对象（Put Object Copy）。支持跨地域跨账号跨 Bucket 复制，需要拥有对源文件的读取权限以及目的文件的写入权限。最大支持5G文件复制，5G以上文件请使用 [高级 API](#.E5.A4.8D.E5.88.B6.E5.AF.B9.E8.B1.A1) 复制。

#### 方法原型

```java
public CopyObjectResult copyObject(CopyObjectRequest copyObjectRequest)
            throws CosClientException, CosServiceException
```

#### 请求示例

[//]: # (.cssg-snippet-copy-object)
```java
// 同地域同账号拷贝
// 源 Bucket, Bucket的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式
String srcBucketName = "sourcebucket-1250000000";
// 要拷贝的源文件
String srcKey = "sourceObject";
// 目标存储桶, Bucket的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式
String destBucketName = "examplebucket-1250000000";
// 要拷贝的目的文件
String destKey = "exampleobject";
CopyObjectRequest copyObjectRequest = new CopyObjectRequest(srcBucketName, srcKey, destBucketName, destKey);
CopyObjectResult copyObjectResult = cosClient.copyObject(copyObjectRequest);

// 跨账号跨地域拷贝（需要拥有对源文件的读取权限以及目的文件的写入权限）
String srcBucketNameOfDiffAppid = "bucket-own-by-others-1251668577";
Region srcBucketRegion = new Region("ap-shanghai");
copyObjectRequest = new CopyObjectRequest(srcBucketRegion, srcBucketNameOfDiffAppid, srcKey, destBucketName, destKey);
copyObjectResult = cosClient.copyObject(copyObjectRequest);
// 获取对象的CRC64
String crc64Ecma = copyObjectResult.getCrc64Ecma();
```


#### 参数说明

| 参数名称          | 描述         | 类型              |
| ----------------- | ------------ | ----------------- |
| copyObjectRequest | 拷贝文件请求 | CopyObjectRequest |

Request 成员说明：

| 参数名称              | 描述                                                         | 类型   |
| --------------------- | ------------------------------------------------------------ | ------ |
| sourceBucketRegion    | 源 Bucket region。默认值：与当前 clientConfig 的 region 一致，表示同地域拷贝 | String |
| sourceBucketName      | 源存储桶名称，命名格式为 BucketName-APPID，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String |
| sourceKey             | 源对象键，对象键（Key）是对象在存储桶中的唯一标识。例如，在对象的访问域名`examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/doc/picture.jpg`中，对象键为 doc/picture.jpg，详情请参见 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String |
| sourceVersionId       | 源文件 version id（适用于开启了版本控制的源 Bucket）。默认值：源文件当前最新版本 | String |
| destinationBucketName | 目标存储桶名称，Bucket 的命名格式为 BucketName-APPID ，name 由字母数字和中划线构成 | String |
| destinationKey        | 目的对象键，对象键（Key）是对象在存储桶中的唯一标识。例如，在对象的访问域名`examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/doc/picture.jpg`中，对象键为 doc/picture.jpg，详情请参见 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String |
| storageClass          | 拷贝的目的文件的存储类型。枚举值：Standard，Standard_IA。默认值：Standard。更多存储类型请参见 [存储类型概述](https://intl.cloud.tencent.com/document/product/436/30925) | String |

#### 返回结果说明

- 成功：返回 CopyObjectResult，包含新文件的 Etag 等信息。
- 失败：发生错误（如身份认证失败），抛出异常 CosClientException 或者 CosServiceException。详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。


### 删除单个对象

#### 功能说明

在存储桶中删除指定 Object （文件/对象）。

#### 方法原型

```java
public void deleteObject(String bucketName, String key)
            throws CosClientException, CosServiceException;
```

#### 请求示例

[//]: # (.cssg-snippet-delete-object)
```java
// Bucket的命名格式为 BucketName-APPID ，此处填写的存储桶名称必须为此格式
String bucketName = "examplebucket-1250000000";
String key = "exampleobject";
cosClient.deleteObject(bucketName, key);
```


#### 参数说明

| 参数名称   | 描述                                                         | 类型   |
| ---------- | ------------------------------------------------------------ | ------ |
| bucketName | Bucket 的命名格式为 BucketName-APPID ，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String |
| key        | 对象键（Key）是对象在存储桶中的唯一标识。例如，在对象的访问域名 `examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/doc/picture.jpg` 中，对象键为 doc/picture.jpg，详情请参见 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String |

#### 返回结果说明

- 成功：无返回值。
- 失败：发生错误（如身份认证失败），抛出异常 CosClientException 或者 CosServiceException。详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。


### 删除多个对象

#### 功能说明

删除多个指定的对象（DELETE Multiple Objects）。

#### 方法原型

```java
public DeleteObjectsResult deleteObjects(DeleteObjectsRequest deleteObjectsRequest)
    throws MultiObjectDeleteException, CosClientException, CosServiceException;
```

#### 请求示例

[//]: # (.cssg-snippet-delete-multi-object)
```java
// Bucket的命名格式为 BucketName-APPID ，此处填写的存储桶名称必须为此格式
String bucketName = "examplebucket-1250000000";

DeleteObjectsRequest deleteObjectsRequest = new DeleteObjectsRequest(bucketName);
// 设置要删除的key列表, 最多一次删除1000个
ArrayList<DeleteObjectsRequest.KeyVersion> keyList = new ArrayList<DeleteObjectsRequest.KeyVersion>();
// 传入要删除的文件名
keyList.add(new DeleteObjectsRequest.KeyVersion("project/folder1/picture.jpg"));
keyList.add(new DeleteObjectsRequest.KeyVersion("project/folder2/text.txt"));
keyList.add(new DeleteObjectsRequest.KeyVersion("project/folder2/music.mp3"));
deleteObjectsRequest.setKeys(keyList);

// 批量删除文件
try {
    DeleteObjectsResult deleteObjectsResult = cosClient.deleteObjects(deleteObjectsRequest);
    List<DeleteObjectsResult.DeletedObject> deleteObjectResultArray = deleteObjectsResult.getDeletedObjects();
} catch (MultiObjectDeleteException mde) { // 如果部分删除成功部分失败, 返回MultiObjectDeleteException
    List<DeleteObjectsResult.DeletedObject> deleteObjects = mde.getDeletedObjects();
    List<MultiObjectDeleteException.DeleteError> deleteErrors = mde.getErrors();
} catch (CosServiceException e) { // 如果是其他错误，例如参数错误， 身份验证不过等会抛出 CosServiceException
    e.printStackTrace();
    throw e;
} catch (CosClientException e) { // 如果是客户端错误，例如连接不上COS
    e.printStackTrace();
    throw e;
}
```


#### 参数说明

| 参数名称             | 描述 | 类型                 |
| -------------------- | ---- | -------------------- |
| deleteObjectsRequest | 请求 | DeleteObjectsRequest |

Request 成员说明：

| 参数名称   | 描述                                                         | 类型               |
| ---------- | ------------------------------------------------------------ | ------------------ |
| bucketName | Bucket 的命名格式为 BucketName-APPID ，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String             |
| quiet      | 指明删除的返回结果方式，可选值为 true，false，默认为 false。设置为 true 只返回失败的错误信息，设置为 false 时返回成功和失败的所有信息 | boolean            |
| keys       | 对象路径列表，对象的版本号为可选                             | `List<DeleteObjectsRequest.KeyVersion>` |

DeleteObjectsRequest.KeyVersion 成员说明：

| 参数名称 | 描述                                                         | 类型   |
| -------- | ------------------------------------------------------------ | ------ |
| key      | 对象键（Key）是对象在存储桶中的唯一标识。例如，在对象的访问域名 `examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/doc/picture.jpg` 中，对象键为 doc/picture.jpg，详情请参见 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String |
| version  | 在开启存储桶版本控制时，指定被删除对象的版本号，可选           | String |

#### 返回结果说明

- 成功：无返回值。
- 失败：发生错误（如身份认证失败），抛出异常 CosClientException 或者 CosServiceException，详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。


### 恢复归档对象 

#### 功能说明

将归档类型的对象取回访问（POST Object restore）。

#### 方法原型

```java
public void restoreObject(RestoreObjectRequest restoreObjectRequest)
    throws CosClientException, CosServiceException;
```

#### 请求示例

[//]: # (.cssg-snippet-restore-object)
```java
// 存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式
String bucketName = "examplebucket-1250000000";
String key = "exampleobject";

// 设置 restore 得到的临时副本过期天数为1天
RestoreObjectRequest restoreObjectRequest = new RestoreObjectRequest(bucketName, key, 1);
// 设置恢复模式为 Standard，其他的可选模式包括 Expedited 和 Bulk，三种恢复模式在费用和速度上不一样
CASJobParameters casJobParameters = new CASJobParameters();
casJobParameters.setTier(Tier.Standard);
restoreObjectRequest.setCASJobParameters(casJobParameters);
cosClient.restoreObject(restoreObjectRequest);
```

#### 参数说明

| 参数名称             | 描述   | 类型                 |
| -------------------- | ------ | -------------------- |
| restoreObjectRequest | 请求类 | RestoreObjectRequest |

Request 成员说明：

| 参数名称         | 描述                                                         | 类型             |
| ---------------- | ------------------------------------------------------------ | ---------------- |
| bucketName       | 存储桶的命名格式为 BucketName-APPID，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String           |
| key              | 对象键（Key）是对象在存储桶中的唯一标识。例如，在对象的访问域名`examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/doc/picture.jpg`中，对象键为 doc/picture.jpg，详情请参见 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String           |
| expirationInDays | 恢复出的临时文件的过期天数                      | int              |
| casJobParameters | 描述恢复类型的配置信息，可调用 setTier 函数设置为 Tier.Standard、Tier.Expedited、Tier.Bulk 三种恢复类型之一，若恢复深度归档存储类型，则仅支持 Tier.Standard 和 Tier.Bulk | CASJobParameters |

#### 返回结果说明

- 成功：无返回值。
- 失败：发生错误（如身份认证失败）， 抛出异常 CosClientException 或者 CosServiceException。详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。



## 分块操作

分块上传对象可包括的操作：

- 分块上传对象：初始化分块上传，上传分块，完成分块上传。
- 分块续传：查询已上传块，上传分块，完成分块上传。
- 删除已上传分块。

### 查询分块上传

#### 功能说明

查询指定存储桶中正在进行的分块上传（List Multipart Uploads）。

#### 方法原型

```java
public MultipartUploadListing listMultipartUploads(
            ListMultipartUploadsRequest listMultipartUploadsRequest)
            throws CosClientException, CosServiceException
```

#### 请求示例

[//]: # (.cssg-snippet-list-multi-upload)
```java
// Bucket的命名格式为 BucketName-APPID ，此处填写的存储桶名称必须为此格式
String bucketName = "examplebucket-1250000000";
ListMultipartUploadsRequest listMultipartUploadsRequest = new ListMultipartUploadsRequest(bucketName);
listMultipartUploadsRequest.setDelimiter("/");
listMultipartUploadsRequest.setMaxUploads(100);
listMultipartUploadsRequest.setPrefix("");
listMultipartUploadsRequest.setEncodingType("url");
MultipartUploadListing multipartUploadListing = cosClient.listMultipartUploads(listMultipartUploadsRequest);
```


#### 参数说明

| 参数名称                    | 描述 | 类型                        |
| --------------------------- | ---- | --------------------------- |
| listMultipartUploadsRequest | 请求 | ListMultipartUploadsRequest |

Request 成员说明：

| 参数名称       | 描述                                                         | 类型   |
| -------------- | ------------------------------------------------------------ | ------ |
| bucketName     | Bucket 的命名格式为 BucketName-APPID ，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String |
| keyMarker      | 列出条目从该 Key 值开始                                      | String |
| delimiter      | 定界符为一个符号，如果有 Prefix，则将 Prefix 到 delimiter 之间的相同路径归为一类，定义为 Common Prefix，然后列出所有 Common Prefix。如果没有 Prefix，则从路径起点开始 | String |
| prefix         | 限定返回的 Object key 必须以 Prefix 作为前缀。注意使用 prefix 查询时，返回的 key 中仍会包含 Prefix | String |
| uploadIdMarker | 列出条目从该 UploadId 值开始                                 | String |
| maxUploads     | 设置最大返回的 multipart 数量，合法值1到1000                 | String |
| encodingType   | 规定返回值的编码方式，可选值：url                            | String |

#### 返回结果说明

- 成功：返回 MultipartUploadListing，包含正在进行分块上传的信息。
- 失败：发生错误（如身份认证失败），抛出异常 CosClientException 或者 CosServiceException。详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。



### 初始化分块上传

#### 功能说明

初始化分块上传任务（Initiate Multipart Upload）。

#### 方法原型

```java
public InitiateMultipartUploadResult initiateMultipartUpload(
    InitiateMultipartUploadRequest request) throws CosClientException, CosServiceException;
```

#### 请求示例

[//]: # (.cssg-snippet-init-multi-upload)
```java
// Bucket的命名格式为 BucketName-APPID
String bucketName = "examplebucket-1250000000";
String key = "exampleobject";
InitiateMultipartUploadRequest initRequest = new InitiateMultipartUploadRequest(bucketName, key);
InitiateMultipartUploadResult initResponse = cosClient.initiateMultipartUpload(initRequest);
uploadId = initResponse.getUploadId();
```


#### 参数说明

| 参数名称                       | 描述 | 类型                           |
| ------------------------------ | ---- | ------------------------------ |
| initiateMultipartUploadRequest | 请求 | InitiateMultipartUploadRequest |

Request 成员说明：

| 参数名称   | 设置方法            | 描述                                                         | 类型   |
| ---------- | ------------------- | ------------------------------------------------------------ | ------ |
| bucketName | 构造函数或 set 方法 | Bucket 的命名格式为 BucketName-APPID ，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String |
| key        | 构造函数或 set 方法 | 存储于 COS 上 Object 的 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String |

#### 返回结果说明

- 成功：返回 InitiateMultipartUploadResult ，包含标志本次分块上传的 uploadId。
- 失败：发生错误（如身份认证失败），抛出异常 CosClientException 或者 CosServiceException。详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。


### 上传分块

上传分块（Upload Part）。

#### 方法原型

```java
public UploadPartResult uploadPart(UploadPartRequest uploadPartRequest) throws CosClientException, CosServiceException;
```

#### 请求示例

[//]: # (.cssg-snippet-upload-part)
```java
// 上传分块, 最多10000个分块, 分块大小支持为1M - 5G。
// 分块大小设置为4M。如果总计 n 个分块, 则 1 ~ n-1 的分块大小一致，最后一块小于等于前面的分块大小。
partETags = new ArrayList<PartETag>();
int partNumber = 1;
int partSize = 4 * 1024 * 1024;
String bucketName = "examplebucket-1250000000";
String key = "exampleobject";
byte data[] = new byte[partSize];
ByteArrayInputStream partStream = new ByteArrayInputStream(data);
// partStream 代表 part 数据的输入流, 流长度为 partSize
UploadPartRequest uploadRequest = new UploadPartRequest().withBucketName(bucketName).
        withUploadId(uploadId).withKey(key).withPartNumber(partNumber).
        withInputStream(partStream).withPartSize(partSize);
UploadPartResult uploadPartResult = cosClient.uploadPart(uploadRequest);
// 获取分块的 Etag
String etag = uploadPartResult.getETag();
// 获取分块的 CRC64
String crc64Ecma = uploadPartResult.getCrc64Ecma();
partETags.add(new PartETag(partNumber, etag));  // partETags 记录所有已上传的 part 的 Etag 信息
// ... 上传 partNumber 第2个到第 n 个分块
```


#### 参数说明

| 参数名称          | 描述 | 类型              |
| ----------------- | ---- | ----------------- |
| uploadPartRequest | 请求 | UploadPartRequest |

Request 成员说明：

| 参数名称    | 设置方法 | 描述                                                         | 类型        |
| ----------- | -------- | ------------------------------------------------------------ | ----------- |
| bucketName  | set 方法  | Bucket 的命名格式为 BucketName-APPID ，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String      |
| key         | set 方法  | 存储于 COS 上 Object 的 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String      |
| uploadId    | set 方法  | 标识指定分块上传的 uploadId                                  | String      |
| partNumber  | set 方法  | 标识指定分块的编号，必须 >= 1                                | int         |
| inputStream | set 方法  | 待上传分块的输入流                                           | InputStream |
|trafficLimit| set 方法| 用于对上传分块进行流量控制，单位：bit/s，默认不进行流量控制  | Int|


#### 返回结果说明

- 成功：返回 UploadPartResult，包含上传分块的 eTag 信息。
- 失败：发生错误（如身份认证失败），抛出异常 CosClientException 或者 CosServiceException。详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。


#### 返回参数说明

UploadPartResult 类用于返回结果信息，其主要成员说明如下：

| 成员名称        | 描述                                                | 类型                |
| --------------- | --------------------------------------------------- | ------------------- |
| partNumber | 标识指定分块的编号 | String                |
| eTag        | 返回上传分块的 MD5 值                   | String |
|crc64Ecma| 服务端根据分块内容计算出来的 CRC64| String |


### 复制分块

#### 功能说明

将一个对象的分块内容从源路径复制到目标路径（Upload Part - Copy）。

#### 方法原型

```java
public CopyPartResult copyPart(CopyPartRequest copyPartRequest) throws CosClientException, CosServiceException
```

#### 请求示例

[//]: # (.cssg-snippet-upload-part-copy)
```java
// 存储桶名称，格式为：BucketName-APPID
// 设置目标存储桶名称，对象名称和分块上传 ID
String destinationBucketName = "examplebucket-1250000000";
String destinationTargetKey = "exampleobject";
int partNumber = 1;
CopyPartRequest copyPartRequest = new CopyPartRequest();
copyPartRequest.setDestinationBucketName(destinationBucketName);
copyPartRequest.setDestinationKey(destinationTargetKey);
copyPartRequest.setUploadId(uploadId);
copyPartRequest.setPartNumber(partNumber);
// 设置源存储桶的区域和名称，以及对象名称，偏移量区间
String sourceBucketRegion = "COS_REGION";
String sourceBucketName = "sourcebucket-1250000000";
String sourceKey = "sourceObject";
Long firstByte = 1L;
Long lastByte = 1048576L;
copyPartRequest.setSourceBucketRegion(new Region(sourceBucketRegion));
copyPartRequest.setSourceBucketName(sourceBucketName);
copyPartRequest.setSourceKey(sourceKey);
copyPartRequest.setFirstByte(firstByte);
copyPartRequest.setLastByte(lastByte);

CopyPartResult copyPartResult = cosClient.copyPart(copyPartRequest);
partETags = new ArrayList<PartETag>();
partETags.add(copyPartResult.getPartETag());
```


#### 参数说明

| 参数名称        | 描述 | 类型            |
| --------------- | ---- | --------------- |
| copyPartRequest | 请求 | CopyPartRequest |

Request 成员说明：

| 参数名称              | 设置方法 | 描述                                                         | 类型   |
| --------------------- | -------- | ------------------------------------------------------------ | ------ |
| destinationBucketName | set 方法 | 目标存储桶名称，Bucket 的命名格式为 BucketName-APPID ，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String |
| destinationKey        | set 方法 | 目标对象名称，存储于 COS 上 Object 的 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String |
| uploadId              | set 方法 | 标识指定分块上传的 uploadId                                  | String |
| partNumber            | set 方法 | 标识指定分块的编号，必须 >= 1                                | int    |
| sourceBucketRegion    | set 方法 | 源存储桶的区域                                               | Region |
| sourceBucketName      | set 方法 | 源存储桶的名称                                               | String |
| sourceKey             | set 方法 | 源对象名称，存储于 COS 上 Object 的 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String |
| firstByte             | set 方法 | 源对象的首字节偏移                                           | Long   |
| lastByte              | set 方法 | 源对象的最后一字节偏移                                       | Long   |

#### 返回结果说明

- 成功：返回 CopyPartResult，包含分块的 ETag 信息。
- 失败：发生错误（如身份认证失败），抛出异常 CosClientException 或者 CosServiceException。详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。


### 查询已上传块

#### 功能说明

查询特定分块上传操作中的已上传的块（List Parts）。

#### 方法原型

```java
public PartListing listParts(ListPartsRequest request)
            throws CosClientException, CosServiceException;
```

#### 请求示例

[//]: # (.cssg-snippet-list-parts)
```java
// ListPart 用于在 complete 分块上传前或者 abort 分块上传前获取 uploadId 对应的已上传的分块信息, 可以用来构造 partEtags
List<PartETag> partETags = new ArrayList<PartETag>();
String bucketName = "examplebucket-1250000000";
String key = "exampleobject";
ListPartsRequest listPartsRequest = new ListPartsRequest(bucketName, key, uploadId);
PartListing partListing = null;
do {
    partListing = cosClient.listParts(listPartsRequest);
    for (PartSummary partSummary : partListing.getParts()) {
        partETags.add(new PartETag(partSummary.getPartNumber(), partSummary.getETag()));
    }
    listPartsRequest.setPartNumberMarker(partListing.getNextPartNumberMarker());
} while (partListing.isTruncated());
```


#### 参数说明

| 参数名称         | 设置方法            | 描述                                                         | 类型   |
| ---------------- | ------------------- | ------------------------------------------------------------ | ------ |
| bucketName       | 构造函数或 set 方法 | Bucket 的命名格式为 BucketName-APPID ，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String |
| key              | 构造函数或 set 方法 | 对象的名称                                                   | String |
| uploadId         | 构造函数或 set 方法 | 本次要查询的分块上传的 uploadId                               | String |
| maxParts         | set 方法            | 单次返回最大的条目数量，默认1000                             | String |
| partNumberMarker | set 方法            | 默认以 UTF-8 二进制顺序列出条目，所有列出条目从 marker 开始  | String |
| encodingType     | set 方法            | 规定返回值的编码方式                                         | String |

#### 返回结果说明

- 成功：返回 PartListing，包含每一分块的 ETag 和编号，以及下一次 list 的起点 marker。
- 失败：发生错误（如身份认证失败），抛出异常 CosClientException 或者 CosServiceException。详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。


### 完成分块上传 

#### 功能说明

完成整个文件的分块上传（Complete Multipart Upload）。

#### 方法原型

```java
public CompleteMultipartUploadResult completeMultipartUpload(CompleteMultipartUploadRequest request) throws CosClientException, CosServiceException;
```

#### 请求示例

[//]: # (.cssg-snippet-complete-multi-upload)
```java
// complete 完成分块上传.
String bucketName = "examplebucket-1250000000";
String key = "exampleobject";
CompleteMultipartUploadRequest compRequest = new CompleteMultipartUploadRequest(bucketName, key, uploadId, partETags);
CompleteMultipartUploadResult result = cosClient.completeMultipartUpload(compRequest);
```


#### 参数说明

| 参数名称   | 设置方法&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;            | 描述                                                         | 类型              |
| ---------- | ------------------- | ------------------------------------------------------------ | ----------------- |
| bucketName | 构造函数或 set 方法 | Bucket 的命名格式为 BucketName-APPID ，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String            |
| key        | 构造函数或 set 方法 | 存储于 COS 上 Object 的 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String            |
| uploadId   | 构造函数或 set 方法 | 标识指定分块上传的 uploadId                                  | String            |
| partETags  | 构造函数或 set 方法 | 标识分块块的编号和上传返回的 eTag                            | ` List<PartETag>` |

#### 返回结果说明

- 成功：返回 CompleteMultipartUploadResult，包含完成对象的 eTag 信息。
- 失败：发生错误（如身份认证失败），抛出异常 CosClientException 或者 CosServiceException。详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。


### 终止分块上传

#### 功能说明

终止一个分块上传操作并删除已上传的块（Abort Multipart Upload）。

#### 方法原型

```java
public void abortMultipartUpload(AbortMultipartUploadRequest request)  throws CosClientException, CosServiceException;
```

#### 请求示例

[//]: # (.cssg-snippet-abort-multi-upload)
```java
// abortMultipartUpload 用于终止一个还未 complete 的分块上传
String bucketName = "examplebucket-1250000000";
String key = "exampleobject";
AbortMultipartUploadRequest abortMultipartUploadRequest = new AbortMultipartUploadRequest(bucketName, key, uploadId);
cosClient.abortMultipartUpload(abortMultipartUploadRequest);
```


#### 参数说明

| 参数名称   | 设置方法            | 描述                                                         | 类型   |
| ---------- | ------------------- | ------------------------------------------------------------ | ------ |
| bucketName | 构造函数或 set 方法 | Bucket 的命名格式为 BucketName-APPID，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String |
| key        | 构造函数或 set 方法 | 存储于 COS 上 Object 的 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String |
| uploadId   | 构造函数或 set 方法 | 标识指定分块上传的 uploadId                                  | String |

#### 返回结果说明

- 成功：无返回值。
- 失败：发生错误（例如身份认证失败），抛出异常 CosClientException 或者 CosServiceException。 详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。



## 高级接口（推荐）

高级 API 由类 TransferManger 通过封装上传以及下载接口，内部有一个线程池，接受用户的上传和下载请求，因此用户可选择异步的提交任务。

[//]: # (.cssg-snippet-transfer-init)
```java
// 线程池大小，建议在客户端与 COS 网络充足（例如使用腾讯云的 CVM，同地域上传 COS）的情况下，设置成16或32即可，可较充分的利用网络资源
// 对于使用公网传输且网络带宽质量不高的情况，建议减小该值，避免因网速过慢，造成请求超时。
ExecutorService threadPool = Executors.newFixedThreadPool(32);
// 传入一个 threadpool, 若不传入线程池，默认 TransferManager 中会生成一个单线程的线程池。
TransferManager transferManager = new TransferManager(cosClient, threadPool);
// 设置高级接口的分块上传阈值和分块大小为10MB
TransferManagerConfiguration transferManagerConfiguration = new TransferManagerConfiguration();
transferManagerConfiguration.setMultipartUploadThreshold(10 * 1024 * 1024);
transferManagerConfiguration.setMinimumUploadPartSize(10 * 1024 * 1024);
transferManager.setConfiguration(transferManagerConfiguration);
```

在不需要使用 transferManager 之后，请手动关闭，防止资源泄漏。

```java
// 关闭 TransferManger
transferManager.shutdownNow();
```

#### 参数说明

TransferManagerConfiguration 类用于记录高级接口的配置信息，其主要成员说明如下：

|  成员名 | 设置方法            | 描述                                                         | 类型           |
| ------------ | ------------------- | ------------------------------------------------------------ | -------------- |
| minimumUploadPartSize   | set 方法 | 分块上传的块大小，单位：字节（Byte），默认为5MB | long         |
| multipartUploadThreshold   | set 方法 | 大于等于该值则并发的分块上传文件，单位：字节（Byte），默认为5MB | long         |
| multipartCopyThreshold         | set 方法 | 大于等于该值则并发的分块复制文件，单位：字节（Byte），默认为5GB | long           |
| multipartCopyPartSize        | set 方法 | 分块复制的块大小，单位：字节（Byte），默认为100MB           | long    |

### 上传对象

#### 功能说明

高级上传接口根据用户文件的长度和数据类型，自动选择简单上传或分块上传。具体实现如下所述：
- 对小于分块上传阈值或未带 Content-Length 头部的流上传，高级接口会选择简单上传；
- 对大于分块上传阈值但未带 Content-Length 头部的流上传，高级接口会选择分块上传；
- 对数据类型是 File 类型的文件上传，高级接口会多线程并发同时上传多个分块。

>?有关其他一些设置属性，存储类别，MD5 校验等可参见 [PUT Object API](https://intl.cloud.tencent.com/document/product/436/7749)。

#### 方法原型

```java
// 上传对象
public Upload upload(final PutObjectRequest putObjectRequest)
            throws CosServiceException, CosClientException;
```

#### 请求示例

[//]: # (.cssg-snippet-transfer-upload-file)
```java
// 示例1：
// 存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式
String bucketName = "examplebucket-1250000000";
String key = "exampleobject";
File localFile = new File(localFilePath);
PutObjectRequest putObjectRequest = new PutObjectRequest(bucketName, key, localFile);
// 本地文件上传
Upload upload = transferManager.upload(putObjectRequest);
// 等待传输结束（如果想同步的等待上传结束，则调用 waitForCompletion）
UploadResult uploadResult = upload.waitForUploadResult();

```


#### 参数说明

| 参数名称         | 描述         | 类型             |
| ---------------- | ------------ | ---------------- |
| putObjectRequest | 上传文件请求 | PutObjectRequest |

Request 成员说明：

| Request 成员 | 设置方法&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;            | 描述                                                         | 类型           |
| ------------ | ------------------- | ------------------------------------------------------------ | -------------- |
| bucketName   | 构造函数或 set 方法 | 存储桶的命名格式为 BucketName-APPID，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String         |
| key          | 构造函数或 set 方法 | 对象键（Key）是对象在存储桶中的唯一标识。<br>例如，在对象的访问域名 `examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/doc/picture.jpg` 中，对象键为 doc/picture.jpg，详情请参见 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String         |
| file         | 构造函数或 set 方法 | 本地文件                                                     | File           |
| input        | 构造函数或 set 方法 | 输入流                                                       | InputStream    |
| metadata     | 构造函数或 set 方法 | 文件的元数据                                                 | ObjectMetadata |
|trafficLimit | set 方法| 用于对上传对象进行流量控制，单位：bit/s，默认不进行流量控制 | Int|否|

>?并发上传多个分块时，trafficLimit 限制的是每个分块的上传速度，此时需要调整线程池中的线程数，以控制文件的上传速度。

#### 返回值

- 成功：返回 Upload，可以查询上传是否结束，也可同步的等待上传结束。
- 失败：发生错误（如身份认证失败），抛出异常 CosClientException 或者 CosServiceException。详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。



#### 返回参数说明

通过调用 Upload 的 waitForUploadResult() 方法获取的上传对象信息记录在类 UploadResult 中，类 UploadResult 主要成员说明如下：

| 成员名称   | 描述                                                         | 类型   |
| ---------- | ------------------------------------------------------------ | ------ |
| bucketName | 存储桶的命名格式为 BucketName-APPID，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String |
| key        | 对象键（Key）是对象在存储桶中的唯一标识。<br/>例如，在对象的访问域名 `examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/doc/picture.jpg` 中，对象键为 doc/picture.jpg，详情请参见 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String |
| requestId  | 请求 Id                                                       | String |
| dateStr    | 当前服务端时间                                               | String |
| versionId  | 开启多版本的存储桶，返回对象的版本号 Id                      | String |
| crc64Ecma  | 服务端根据对象内容计算出来的 CRC64                           | String |




### 下载对象

#### 功能说明

将 COS 上的对象下载到本地。

#### 方法原型

```java
// 下载对象
public Download download(final GetObjectRequest GetObjectRequest, final File file);
```

#### 请求示例

[//]: # (.cssg-snippet-transfer-download-object)
```java
// Bucket 的命名格式为 BucketName-APPID ，此处填写的存储桶名称必须为此格式
String bucketName = "examplebucket-1250000000";
String key = "exampleobject";
File localDownFile = new File(localFilePath);
GetObjectRequest getObjectRequest = new GetObjectRequest(bucketName, key);
// 限流使用的单位是bit/s, 这里设置下载带宽限制为 10MB/s
getObjectRequest.setTrafficLimit(80*1024*1024);
// 下载文件
Download download = transferManager.download(getObjectRequest, localDownFile);
// 等待传输结束（如果想同步的等待上传结束，则调用 waitForCompletion）
download.waitForCompletion();
```


#### 参数说明

| 参数名称         | 描述               | 类型             |
| ---------------- | ------------------ | ---------------- |
| getObjectRequest | 下载对象请求       | GetObjectRequest |
| file             | 要下载到的本地文件 | File             |

Request 成员说明：

| Request 成员 | 设置方法&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;            | 描述                                                         | 类型   |
| ------------ | ------------------- | ------------------------------------------------------------ | ------ |
| bucketName   | 构造函数或 set 方法 | 存储桶的命名格式为 BucketName-APPID，详情请参见 [命名规范](https://intl.cloud.tencent.com/document/product/436/13312?lang=en&pg=#bucket-naming-conventions) | String |
| key          | 构造函数或 set 方法 | 对象键（Key）是对象在存储桶中的唯一标识。例如，在对象的访问域名 `examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/doc/picture.jpg` 中，对象键为 doc/picture.jpg，详情请参见 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String |
| range    | set 方法            | 下载的 range 范围                       | Long[] |
| trafficLimit | set 方法    | 用于对下载对象进行流量控制，单位：bit/s，默认不进行流量控制  | int |

#### 返回值

- 成功：返回 Download，可以查询下载是否结束，也可同步的等待下载结束。
- 失败：发生错误（如身份认证失败），抛出异常 CosClientException 或者 CosServiceException。详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。


### 复制对象

#### 功能说明

Copy 接口支持根据对象大小自动选择简单复制或者分块复制，用户无需关心复制的文件大小。

#### 方法原型

```java
// 上传对象
public Copy copy(final CopyObjectRequest copyObjectRequest);
```

#### 请求示例

[//]: # (.cssg-snippet-transfer-copy-object)
```java
// 要拷贝的 bucket region, 支持跨地域拷贝
String secretId = "COS_SECRETID";
String secretKey = "COS_SECRETKEY";
COSCredentials srcCredentials = new BasicCOSCredentials(secretId, secretKey);
Region srcBucketRegion = new Region("COS_REGION");
// 源 Bucket, 存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式
String srcBucketName = "sourcebucket-1250000000";
// 要拷贝的源文件
String srcKey = "sourceObject";
// 目的 Bucket, 存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式
String destBucketName = "examplebucket-1250000000";
// 要拷贝的目的文件
String destKey = "exampleobject";
// 生成用于获取源文件信息的 srcCOSClient
COSClient srcCOSClient = new COSClient(srcCredentials, new ClientConfig(srcBucketRegion));
CopyObjectRequest copyObjectRequest = new CopyObjectRequest(srcBucketRegion, srcBucketName,
        srcKey, destBucketName, destKey);
try {
    Copy copy = transferManager.copy(copyObjectRequest, srcCOSClient, null);
    // 返回一个异步结果 copy, 可同步的调用 waitForCopyResult 等待 copy 结束, 成功返回 CopyResult, 失败抛出异常.
    CopyResult copyResult = copy.waitForCopyResult();
    // 获取拷贝生成对象的CRC64
    String crc64Ecma = copyResult.getCrc64Ecma();
} catch (CosServiceException e) {
    e.printStackTrace();
} catch (CosClientException e) {
    e.printStackTrace();
} catch (InterruptedException e) {
    e.printStackTrace();
}
```


#### 参数说明

| 参数名称          | 描述         | 类型              |
| ----------------- | ------------ | ----------------- |
| copyObjectRequest | 拷贝文件请求 | CopyObjectRequest |

Request 成员说明：

| 参数名称              | 描述                                                         | 类型   |
| --------------------- | ------------------------------------------------------------ | ------ |
| sourceBucketRegion    | 源 Bucket Region 。默认值：与当前 clientconfig 的 region 一致，表示统一地域拷贝 | String |
| sourceBucketName      | 源存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String |
| sourceKey             | 源对象键，对象键（Key）是对象在存储桶中的唯一标识。例如，在对象的访问域名 `examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/doc/picture.jpg` 中，对象键为 doc/picture.jpg，详情请参见 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String |
| sourceVersionId       | 源文件 version id（适用于开启了版本控制的源 Bucket）。默认值：源文件当前最新版本 | String |
| destinationBucketName | 目标存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String |
| destinationKey        | 目的对象键，对象键（Key）是对象在存储桶中的唯一标识。例如，在对象的访问域名 `examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/doc/picture.jpg` 中，对象键为 doc/picture.jpg，详情请参见 [对象键](https://intl.cloud.tencent.com/document/product/436/13324) | String |
| storageClass          | 拷贝的目的文件的存储类型。枚举值：Standard，Standard_IA。默认值：Standard。更多存储类型请参见 [存储类型概述](https://intl.cloud.tencent.com/document/product/436/30925) | String |

#### 返回值

- 成功：返回 Copy，可以查询 Copy 是否结束，也可同步的等待上传结束。
- 失败：发生错误（如身份认证失败），抛出异常 CosClientException 或者 CosServiceException。详情请参见 [异常处理](https://intl.cloud.tencent.com/document/product/436/31537)。

