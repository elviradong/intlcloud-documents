## 简介

本文档提供关于移动对象的相关的 API 概览以及 SDK 示例代码。

| API                                                          | 操作名                       | 操作描述               |
| ------------------------------------------------------------ | ---------------------------- | ---------------------- |
| [PUT Object - Copy](https://intl.cloud.tencent.com/document/product/436/10881) | 设置对象复制（修改对象属性） | 复制文件到目标路径     |
| [DELETE Object](https://intl.cloud.tencent.com/document/product/436/7743) | 删除单个对象                 | 在存储桶中删除指定对象 |

## SDK API 参考

SDK 所有接口的具体参数与方法说明，请参考 [SDK API](https://cos-dotnet-sdk-doc-1253960454.file.myqcloud.com/)。

## 移动对象

移动对象主要包括两个操作：复制源对象到目标位置，删除源对象。

COS 上的对象通过 'bucket+key' 这个名字来标识，移动对象也就意味着对这个对象改名字，SDK 目前没有提供给对象改名字的单独接口，但是可以通过基本操作，达到这个效果。

例如 'mybucket' 这个桶中的 'mykey' 这个对象要移动到 'mybucket' 这个桶的 'prefix/mykey'，就可以先复制一个 'prefix/mykey' 这个对象，然后删除 'mykey' 这个对象。

同样的，如果想把 'mykey' 这个对象移动到 'myanothorbucket' 这个桶里，也可以先复制对象到新的存储桶，然后删除原来的对象。

#### 示例代码

[//]: #	".cssg-snippet-move-object"

```cs
string sourceAppid = "1250000000"; //账号 appid
string sourceBucket = "sourcebucket-1250000000"; //"源对象所在的存储桶
string sourceRegion = "COS_REGION"; //源对象的存储桶所在的地域
string sourceKey = "sourceObject"; //源对象键
//构造源对象属性
CopySourceStruct copySource = new CopySourceStruct(sourceAppid, sourceBucket, 
    sourceRegion, sourceKey);

string bucket = "examplebucket-1250000000"; //目标存储桶，格式：BucketName-APPID
string key = "exampleobject"; //目标对象的对象键

COSXMLCopyTask copyTask = new COSXMLCopyTask(bucket, key, copySource);

try {
  // 拷贝对象
  COSXML.Transfer.COSXMLCopyTask.CopyTaskResult result = await 
    transferManager.CopyAsync(copyTask);
  Console.WriteLine(result.GetResultInfo());

  // 删除对象
  DeleteObjectRequest request = new DeleteObjectRequest(sourceBucket, sourceKey);
  DeleteObjectResult deleteResult = cosXml.DeleteObject(request);
  // 打印结果
  Console.WriteLine(deleteResult.GetResultInfo());
} catch (Exception e) {
    Console.WriteLine("CosException: " + e);
}
```

> ?更多完整示例，请前往 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/dotnet/dist/MoveObject.cs) 查看。
