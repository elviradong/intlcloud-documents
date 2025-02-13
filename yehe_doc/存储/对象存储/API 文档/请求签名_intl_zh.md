> !
> 1. 此文档仅适用于 COS XML 版本。
> 2. 此文档不适用于 POST Object 的 HTTP 请求。

## 简介

使用腾讯云对象存储服务 COS 时，可通过 RESTful API 对 COS 发起 HTTP 匿名请求或 HTTP 签名请求，对于签名请求，COS 服务器端将会进行对请求发起者的身份验证。

- 匿名请求：HTTP 请求不携带任何身份标识和鉴权信息，通过 RESTful API 进行 HTTP 请求操作。
- 签名请求：HTTP 请求时携带签名，COS 服务器端收到消息后，进行身份验证，验证成功则可接受并执行请求，否则将会返回错误信息并丢弃此请求。

对象存储 COS 基于密钥 HMAC（Hash Message Authentication Code）的自定义方案进行身份验证。

同时各语言 [SDK](https://intl.cloud.tencent.com/document/product/436/6474) 提供**预签名 URL**，方便用户获取携带签名的 URL，并自行处理相关的请求，详情请参见各语言 SDK 的**预签名 URL**章节。

## 签名使用场景

在 COS 对象存储服务使用的场景中，对于需要对外发布类的数据，通常可将对象设置为公有读私有写。即所有人可查看，通过 ACL 策略指定账号可写入。此时，可将 ACL 策略与 API 请求签名相结合，对访问进行身份验证，并对操作进行权限和有效期的控制。

>! 本文所描述的 API 请求签名，如果您使用 SDK 进行开发，则已包含在内。**仅在您希望通过原始 API 进行二次开发时，需要根据本文所描述步骤进行操作**。

在以上场景中，可对 API 请求进行多方面的安全防护：

1. **请求者身份验证**。通过访问者唯一 ID 和密钥确定请求者身份。
2. **防止传输数据篡改**。对数据签名并检验，保障传输内容完整性。
3. **防止签名被盗用**。对签名设置时效，避免签名盗用并重复使用。

## 准备工作

1. APPID、SecretId 和 SecretKey。 
在访问管理控制台的 [API 密钥管理](https://console.cloud.tencent.com/cam/capi) 页面中获取。
2. 确定开发语言：
支持但不限于 Java、PHP、.NET、C++、Node.js、Python，根据不同的开发语言，确定对应的 HMAC-SHA1、SHA1 和 UrlEncode 函数。
其中，HMAC-SHA1 和 SHA1 函数以 UTF-8 编码字符串为输入，以16进制小写字符串为输出。UrlEncode 基于 UTF-8 编码，此外对于 ASCII 范围内的可打印字符，下列特殊符号也应被编码：

| 字符 | 十进制 | 十六进制 | 字符 | 十进制 | 十六进制 |
| ------ | ------ | -------- | ---- | ------ | -------- |
| (空格) | 32     | 20       | ;    | 59     | 3B       |
| !      | 33     | 21       | <    | 60     | 3C       |
| "      | 34     | 22       | =    | 61     | 3D       |
| #      | 35     | 23       | >    | 62     | 3E       |
| $      | 36     | 24       | ?    | 63     | 3F       |
| %      | 37     | 25       | @    | 64     | 40       |
| &      | 38     | 26       | [    | 91     | 5B       |
| '      | 39     | 27       | \    | 92     | 5C       |
| (      | 40     | 28       | ]    | 93     | 5D       |
| )      | 41     | 29       | ^    | 94     | 5E       |
| *      | 42     | 2A       | \`    | 96     | 60       |
| +      | 43     | 2B       | {    | 123    | 7B       |
| ,      | 44     | 2C       |  \|    | 124    | 7C       |
| /      | 47     | 2F       | }    | 125    | 7D       |
| :      | 58     | 3A       |      |        |          |

## 签名步骤

### 步骤1：生成 KeyTime
1. 获取当前时间对应的 Unix 时间戳 StartTimestamp，Unix 时间戳是从 UTC（协调世界时，或 GMT 格林威治时间）1970年1月1日0时0分0秒（北京时间 1970年1月1日8时0分0秒）起至现在的总秒数。
2. 根据上述时间戳和期望的签名有效时长算出签名过期时间对应的 Unix 时间戳 EndTimestamp。
3. 拼接签名有效时间，格式为`StartTimestamp;EndTimestamp`，即为 KeyTime。例如：`1557902800;1557910000`。

### 步骤2：生成 SignKey
使用 [HMAC-SHA1](#.E5.87.86.E5.A4.87.E5.B7.A5.E4.BD.9C) 以 [SecretKey](#.E5.87.86.E5.A4.87.E5.B7.A5.E4.BD.9C) 为密钥，以 [KeyTime](#.E6.AD.A5.E9.AA.A41.EF.BC.9A.E7.94.9F.E6.88.90-keytime) 为消息，计算消息摘要（哈希值，16进制小写形式），即为 SignKey，例如：`eb2519b498b02ac213cb1f3d1a3d27a3b3c9bc5f`。


### 步骤3：生成 UrlParamList 和 HttpParameters
1. 遍历 HTTP 请求参数，生成 key 到 value 的映射 Map 及 key 的列表 KeyList：
 - key 使用 [UrlEncode](#.E5.87.86.E5.A4.87.E5.B7.A5.E4.BD.9C) 编码并转换为小写形式。
 - value 使用 [UrlEncode](#.E5.87.86.E5.A4.87.E5.B7.A5.E4.BD.9C) 编码。若无 value 的参数，则认为 value 为空字符串。例如请求路径为`/?acl`，则认为是`/?acl=`。
>? HTTP 请求参数，即请求路径中`?`以后的部分，例如请求路径为`/?versions&prefix=example-folder%2F&delimiter=%2F&max-keys=10`，则请求参数为`versions&prefix=example-folder%2F&delimiter=%2F&max-keys=10`。
2. 将 KeyList 按照字典序排序。
3. 按照 KeyList 的顺序拼接 Map 中的每一个键值对，格式为`key1=value1&key2=value2&key3=value3`，即为 HttpParameters。
4. 按照 KeyList 的顺序拼接 KeyList 中的每一项，格式为`key1;key2;key3`，即为 UrlParamList。

#### 示例：
- 示例一：
请求路径：`/?prefix=example-folder%2F&delimiter=%2F&max-keys=10`
UrlParamList：`delimiter;max-keys;prefix`
HttpParameters：`delimiter=%2F&max-keys=10&prefix=example-folder%2F`
>!请求路径中的请求参数在实际发送请求时也会进行 UrlEncode，因此要注意不要重复执行 UrlEncode。
- 示例二：
请求路径：`/exampleobject?acl`
UrlParamList：`acl`
HttpParameters：`acl=`

### 步骤4：生成 HeaderList 和 HttpHeaders
1. 遍历 HTTP 请求头部，生成 key 到 value 的映射 Map 及 key 的列表 KeyList，key 使用 [UrlEncode](#.E5.87.86.E5.A4.87.E5.B7.A5.E4.BD.9C) 编码并转换为小写形式，value 使用 [UrlEncode](#.E5.87.86.E5.A4.87.E5.B7.A5.E4.BD.9C) 编码。
2. 将 KeyList 按照字典序排序。
3. 按照 KeyList 的顺序拼接 Map 中的每一个键值对，格式为`key1=value1&key2=value2&key3=value3`，即为 HttpHeaders。
4. 按照 KeyList 的顺序拼接 KeyList 中的每一项，格式为`key1;key2;key3`，即为 HeaderList。

#### 示例：
请求头：
```plaintext
Host: examplebucket-1250000000.cos.ap-shanghai.myqcloud.com
Date: Thu, 16 May 2019 03:15:06 GMT
x-cos-acl: private
x-cos-grant-read: uin="100000000011"
```
计算得到：
- HeaderList = `date;host;x-cos-acl;x-cos-grant-read`
- HttpHeaders = `date=Thu%2C%2016%20May%202019%2003%3A15%3A06%20GMT&host=examplebucket-1250000000.cos.ap-shanghai.myqcloud.com&x-cos-acl=private&x-cos-grant-read=uin%3D%22100000000011%22`

### 步骤5：生成 HttpString
根据 HTTP 方法（HttpMethod）、HTTP 请求路径（UriPathname）、[HttpParameters](#.E6.AD.A5.E9.AA.A43.EF.BC.9A.E7.94.9F.E6.88.90-urlparamlist-.E5.92.8C-httpparameters) 和 [HttpHeaders](#.E6.AD.A5.E9.AA.A44.EF.BC.9A.E7.94.9F.E6.88.90-headerlist-.E5.92.8C-httpheaders) 生成 HttpString，格式为`HttpMethod\nUriPathname\nHttpParameters\nHttpHeaders\n`。

其中：
- HttpMethod 转换为小写，例如 get 或 put。
- UriPathname 为请求路径，例如`/`或`/exampleobject`。
- `\n`为换行符。如果其中有字符串为空，前后的换行符需要保留，例如`get\n/exampleobject\n\n\n`。


### 步骤6：生成 StringToSign
根据 [KeyTime](#.E6.AD.A5.E9.AA.A41.EF.BC.9A.E7.94.9F.E6.88.90-keytime) 和 [HttpString](#.E6.AD.A5.E9.AA.A45.EF.BC.9A.E7.94.9F.E6.88.90-httpstring) 生成 StringToSign，格式为`sha1\nKeyTime\nSHA1(HttpString)\n`。
其中：
- sha1 为固定字符串。
- `\n`为换行符。
- SHA1(HttpString) 为使用 [SHA1](#.E5.87.86.E5.A4.87.E5.B7.A5.E4.BD.9C) 对 [HttpString](#.E6.AD.A5.E9.AA.A45.EF.BC.9A.E7.94.9F.E6.88.90-httpstring) 计算的消息摘要，16进制小写形式，例如：`54ecfe22f59d3514fdc764b87a32d8133ea611e6`。

### 步骤7：生成 Signature
使用 [HMAC-SHA1](#.E5.87.86.E5.A4.87.E5.B7.A5.E4.BD.9C) 以 [SignKey](#.E6.AD.A5.E9.AA.A42.EF.BC.9A.E7.94.9F.E6.88.90-signkey) 为密钥（字符串形式，非原始二进制），以 [StringToSign](#.E6.AD.A5.E9.AA.A46.EF.BC.9A.E7.94.9F.E6.88.90-stringtosign) 为消息，计算消息摘要，即为 Signature，例如：`01681b8c9d798a678e43b685a9f1bba0f6c0e012`。

### 步骤8：生成签名
根据 [SecretId](#.E5.87.86.E5.A4.87.E5.B7.A5.E4.BD.9C)、[KeyTime](#.E6.AD.A5.E9.AA.A41.EF.BC.9A.E7.94.9F.E6.88.90-keytime)、[HeaderList](#.E6.AD.A5.E9.AA.A44.EF.BC.9A.E7.94.9F.E6.88.90-headerlist-.E5.92.8C-httpheaders)、[UrlParamList](#.E6.AD.A5.E9.AA.A43.EF.BC.9A.E7.94.9F.E6.88.90-urlparamlist-.E5.92.8C-httpparameters) 和 [Signature](#.E6.AD.A5.E9.AA.A47.EF.BC.9A.E7.94.9F.E6.88.90-signature) 生成签名，格式为：
```plaintext
q-sign-algorithm=sha1
&q-ak=SecretId
&q-sign-time=KeyTime
&q-key-time=KeyTime
&q-header-list=HeaderList
&q-url-param-list=UrlParamList
&q-signature=Signature
```

>!上述格式中的换行仅用于更好的阅读，实际格式并不包含换行。

## 签名使用
通过 RESTful API 对 COS 发起的 HTTP 签名请求，可以通过以下几种方式传递签名：
1. 通过标准的 HTTP Authorization 头，例如`Authorization: q-sign-algorithm=sha1&q-ak=...&q-sign-time=1557989753;1557996953&...&q-signature=...`
2. 作为 HTTP 请求参数，请注意 UrlEncode，例如`/exampleobject?q-sign-algorithm=sha1&q-ak=...&q-sign-time=1557989753%3B1557996953&...&q-signature=...`

>?上述示例中使用`...`省略了部分具体签名内容。

## 使用临时安全凭证（临时密钥）
如果在计算签名时使用了临时安全凭证，那么在发送请求时还应传入安全令牌字段 x-cos-security-token，根据签名使用方式不同，安全令牌字段的传入方式也有所不同：
1. 当通过标准的 HTTP Authorization 头传入签名时，应同时通过 x-cos-security-token 请求头部传入安全令牌字段，例如：
```plaintext
Authorization: q-sign-algorithm=sha1&q-ak=...&q-sign-time=1557989753;1557996953&...&q-signature=...
x-cos-security-token: ...
```
2. 当通过 HTTP 请求参数传入签名时，应同时通过 x-cos-security-token 请求参数传入安全令牌字段，例如：
```plaintext
/exampleobject?q-sign-algorithm=sha1&q-ak=...&q-sign-time=1557989753%3B1557996953&...&q-signature=...&x-cos-security-token=...
```

>?上述示例中使用`...`省略了部分具体签名和安全访问令牌内容。

## 代码示例

### 伪代码
```plaintext
KeyTime = [Now];[Expires]
SignKey = HMAC-SHA1([SecretKey], KeyTime)
HttpString = [HttpMethod]\n[HttpURI]\n[HttpParameters]\n[HttpHeaders]\n
StringToSign = sha1\nKeyTime\nSHA1(HttpString)\n
Signature = HMAC-SHA1(SignKey, StringToSign)
```

### 消息摘要算法示例

不同语言如何调用 HMAC-SHA1 可以参考下面的示例：

#### PHP

```php
$sha1HttpString = sha1('ExampleHttpString');

$signKey = hash_hmac('sha1', 'ExampleKeyTime', 'YourSecretKey');
```

#### Java

```java
import org.apache.commons.codec.digest.DigestUtils;
import org.apache.commons.codec.digest.HmacUtils;

String sha1HttpString = DigestUtils.sha1Hex("ExampleHttpString");

String signKey = HmacUtils.hmacSha1Hex("YourSecretKey", "ExampleKeyTime");
```

#### Python

```python
import hmac
import hashlib

sha1_http_string = hashlib.sha1('ExampleHttpString'.encode('utf-8')).hexdigest()

sign_key = hmac.new('YourSecretKey'.encode('utf-8'), 'ExampleKeyTime'.encode('utf-8'), hashlib.sha1).hexdigest()
```

#### Node.js

```node
var crypto = require('crypto');

var sha1HttpString = crypto.createHash('sha1').update('ExampleHttpString').digest('hex');
var signKey = crypto.createHmac('sha1', 'YourSecretKey').update('ExampleKeyTime').digest('hex');
```

#### Go

```go
import (
	"crypto/hmac"
	"crypto/sha1"
)

h := sha1.New()
h.Write([]byte("ExampleHttpString"))
sha1HttpString := h.Sum(nil)

var hashFunc = sha1.New
h = hmac.New(hashFunc, []byte("YourSecretKey"))
h.Write([]byte("ExampleKeyTime"))
signKey := h.Sum(nil)
```

## 实际案例

### 准备工作

登录访问管理控制台的 [API 密钥管理](https://console.cloud.tencent.com/cam/capi) 页面获取其 APPID、SecretId 和 SecretKey，举例如下：

| APPID      | SecretId                             | SecretKey                        |
| ---------- | ------------------------------------ | -------------------------------- |
| 1250000000 | AKIDQjz3ltompVjBni5LitkWHFlFpwkn9U5q | BQYIM75p8x0iWVFSIgqEKwFprpRSVHlz |

### 上传对象

#### 原始请求

```plaintext
PUT /exampleobject(%E8%85%BE%E8%AE%AF%E4%BA%91) HTTP/1.1
Date: Thu, 16 May 2019 06:45:51 GMT
Host: examplebucket-1250000000.cos.ap-beijing.myqcloud.com
Content-Type: text/plain
Content-Length: 13
Content-MD5: mQ/fVh815F3k6TAUm8m0eg==
x-cos-acl: private
x-cos-grant-read: uin="100000000011"

ObjectContent
```

#### 中间变量

- **KeyTime** = `1557989151;1557996351`
- **SignKey** = `eb2519b498b02ac213cb1f3d1a3d27a3b3c9bc5f`
- **UrlParamList** = `(empty string)`
- **HttpParameters** = `(empty string)`
- **HeaderList** = `content-length;content-md5;content-type;date;host;x-cos-acl;x-cos-grant-read`
- **HttpHeaders** = `content-length=13&content-md5=mQ%2FfVh815F3k6TAUm8m0eg%3D%3D&content-type=text%2Fplain&date=Thu%2C%2016%20May%202019%2006%3A45%3A51%20GMT&host=examplebucket-1250000000.cos.ap-beijing.myqcloud.com&x-cos-acl=private&x-cos-grant-read=uin%3D%22100000000011%22`
- **HttpString** = `put\n/exampleobject(腾讯云)\n\ncontent-length=13&content-md5=mQ%2FfVh815F3k6TAUm8m0eg%3D%3D&content-type=text%2Fplain&date=Thu%2C%2016%20May%202019%2006%3A45%3A51%20GMT&host=examplebucket-1250000000.cos.ap-beijing.myqcloud.com&x-cos-acl=private&x-cos-grant-read=uin%3D%22100000000011%22\n`
- **StringToSign** = `sha1\n1557989151;1557996351\n8b2751e77f43a0995d6e9eb9477f4b685cca4172\n`
- **Signature** = `3b8851a11a569213c17ba8fa7dcf2abec6935172`

其中，(empty string) 代表长度为0的空字符串，`\n`代表换行符。

#### 签名后的请求

```plaintext
PUT /exampleobject(%E8%85%BE%E8%AE%AF%E4%BA%91) HTTP/1.1
Date: Thu, 16 May 2019 06:45:51 GMT
Host: examplebucket-1250000000.cos.ap-beijing.myqcloud.com
Content-Type: text/plain
Content-Length: 13
Content-MD5: mQ/fVh815F3k6TAUm8m0eg==
x-cos-acl: private
x-cos-grant-read: uin="100000000011"
Authorization: q-sign-algorithm=sha1&q-ak=AKIDQjz3ltompVjBni5LitkWHFlFpwkn9U5q&q-sign-time=1557989151;1557996351&q-key-time=1557989151;1557996351&q-header-list=content-length;content-md5;content-type;date;host;x-cos-acl;x-cos-grant-read&q-url-param-list=&q-signature=3b8851a11a569213c17ba8fa7dcf2abec6935172

ObjectContent
```

### 下载对象

#### 原始请求

```plaintext
GET /exampleobject(%E8%85%BE%E8%AE%AF%E4%BA%91)?response-content-type=application%2Foctet-stream&response-cache-control=max-age%3D600 HTTP/1.1
Date: Thu, 16 May 2019 06:55:53 GMT
Host: examplebucket-1250000000.cos.ap-beijing.myqcloud.com
```

#### 中间变量

- **KeyTime** = `1557989753;1557996953`
- **SignKey** = `937914bf490e9e8c189836aad2052e4feeb35eaf`
- **UrlParamList** = `response-cache-control;response-content-type`
- **HttpParameters** = `response-cache-control=max-age%3D600&response-content-type=application%2Foctet-stream`
- **HeaderList** = `date;host`
- **HttpHeaders** = `date=Thu%2C%2016%20May%202019%2006%3A55%3A53%20GMT&host=examplebucket-1250000000.cos.ap-beijing.myqcloud.com`
- **HttpString** = `get\n/exampleobject(腾讯云)\nresponse-cache-control=max-age%3D600&response-content-type=application%2Foctet-stream\ndate=Thu%2C%2016%20May%202019%2006%3A55%3A53%20GMT&host=examplebucket-1250000000.cos.ap-beijing.myqcloud.com\n`
- **StringToSign** = `sha1\n1557989753;1557996953\n54ecfe22f59d3514fdc764b87a32d8133ea611e6\n`
- **Signature** = `01681b8c9d798a678e43b685a9f1bba0f6c0e012`

其中，`\n`代表换行符。

#### 签名后的请求

```plaintext
GET /exampleobject(%E8%85%BE%E8%AE%AF%E4%BA%91)?response-content-type=application%2Foctet-stream&response-cache-control=max-age%3D600 HTTP/1.1
Date: Thu, 16 May 2019 06:55:53 GMT
Host: examplebucket-1250000000.cos.ap-beijing.myqcloud.com
Authorization: q-sign-algorithm=sha1&q-ak=AKIDQjz3ltompVjBni5LitkWHFlFpwkn9U5q&q-sign-time=1557989753;1557996953&q-key-time=1557989753;1557996953&q-header-list=date;host&q-url-param-list=response-cache-control;response-content-type&q-signature=01681b8c9d798a678e43b685a9f1bba0f6c0e012
```
