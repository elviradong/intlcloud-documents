## 下载与安装

#### 相关资源
- 对象存储的 XML Python SDK 源码下载地址：[XML Python SDK](https://github.com/tencentyun/cos-python-sdk-v5)。
- SDK 快速下载地址：[XML Python SDK](https://cos-sdk-archive-1253960454.file.myqcloud.com/cos-python-sdk-v5/latest/cos-python-sdk-v5.zip)。
- 演示示例 Demo 下载地址：[XML Python Demo](https://github.com/tencentyun/cos-python-sdk-v5/blob/master/qcloud_cos/demo.py)。
- SDK 文档中的所有示例代码请参见 [SDK 代码示例](https://github.com/tencentyun/cos-snippets/tree/master/Python)。
- SDK 更新日志请参见 [ChangeLog](https://github.com/tencentyun/cos-python-sdk-v5/blob/master/CHANGELOG.md)。

#### 环境依赖

对象存储的 XML Python SDK  目前可以支持 Python 2.7 以及 Python 3.4 及以上。
>?关于文章中出现的 SecretId、SecretKey、Bucket、Region 等名称的含义和获取方式请参见 [COS 术语信息](https://intl.cloud.tencent.com/document/product/436/7751)。

#### 安装 SDK

安装 SDK 有三种安装方式：pip 安装、手动安装和离线安装。

- 使用 pip 安装（推荐）
```sh
 pip install -U cos-python-sdk-v5
```

- 手动安装
从 [XML Python SDK](https://github.com/tencentyun/cos-python-sdk-v5) 下载源码，通过 setup 手动安装，执行以下命令。
```python
 python setup.py install
```

- 离线安装
```python
# 在有外网的机器下运行如下命令
mkdir cos-python-sdk-packages
pip download cos-python-sdk-v5 -d cos-python-sdk-packages
tar -czvf cos-python-sdk-packages.tar.gz cos-python-sdk-packages
# 将安装包拷贝到没有外网的机器后运行如下命令
# 请确保两台机器的 python 版本保持一致，否则会出现安装失败的情况
tar -xzvf cos-python-sdk-packages.tar.gz
pip install cos-python-sdk-v5 --no-index -f cos-python-sdk-packages
```



## 开始使用
下面为您介绍如何使用 COS Python SDK 完成一个基础操作，例如初始化客户端、创建存储桶、查询存储桶列表、上传对象、查询对象列表、下载对象和删除对象。

### 初始化
请参考以下示例代码：

[//]: # (.cssg-snippet-global-init)
```python
# -*- coding=utf-8
# appid 已在配置中移除,请在参数 Bucket 中带上 appid。Bucket 由 BucketName-APPID 组成
# 1. 设置用户配置, 包括 secretId，secretKey 以及 Region
from qcloud_cos import CosConfig
from qcloud_cos import CosS3Client
import sys
import logging

logging.basicConfig(level=logging.INFO, stream=sys.stdout)

secret_id = 'COS_SECRETID'      # 替换为用户的 secretId
secret_key = 'COS_SECRETKEY'      # 替换为用户的 secretKey
region = 'COS_REGION'     # 替换为用户的 Region
token = None                # 使用临时密钥需要传入 Token，默认为空，可不填
scheme = 'https'            # 指定使用 http/https 协议来访问 COS，默认为 https，可不填
config = CosConfig(Region=region, SecretId=secret_id, SecretKey=secret_key, Token=token, Scheme=scheme)
# 2. 获取客户端对象
client = CosS3Client(config)
# 参照下文的描述。或者参照 Demo 程序，详见 https://github.com/tencentyun/cos-python-sdk-v5/blob/master/qcloud_cos/demo.py
```


>?关于临时密钥如何生成和使用，请参见 [临时密钥生成及使用指引](https://intl.cloud.tencent.com/document/product/436/14048)。

设置代理：

[//]: # (.cssg-snippet-global-init-proxy)
```python
# APPID 已在配置中移除,请在参数 Bucket 中带上 APPID。Bucket 由 BucketName-APPID 组成
# 1. 设置用户配置, 包括 secretId，secretKey 以及 Region
secret_id = 'COS_SECRETID'      # 替换为用户的 secretId
secret_key = 'COS_SECRETKEY'      # 替换为用户的 secretKey
region = 'COS_REGION'     # 替换为用户的 Region
proxies = {
    'http': '127.0.0.1:80', # 替换为用户的 HTTP代理地址
    'https': '127.0.0.1:443' # 替换为用户的 HTTPS代理地址
}
config = CosConfig(Region=region, SecretId=secret_id, SecretKey=secret_key, Proxies=proxies)
# 2. 获取客户端对象
client = CosS3Client(config)
# 参照下文的描述。或者参照 Demo 程序，详见 https://github.com/tencentyun/cos-python-sdk-v5/blob/master/qcloud_cos/demo.py
```

设置 Endpoint：

[//]: # (.cssg-snippet-global-init-endpoint)
```python
# APPID 已在配置中移除,请在参数 Bucket 中带上 APPID。Bucket 由 BucketName-APPID 组成
# 1. 设置用户配置, 包括 secretId，secretKey 以及 Region
secret_id = 'COS_SECRETID'      # 替换为用户的 secretId
secret_key = 'COS_SECRETKEY'      # 替换为用户的 secretKey
region = 'COS_REGION'     # 替换为用户的 Region
endpoint = 'cos.accelerate.myqcloud.com' # 替换为用户的 endpoint
config = CosConfig(Region=region, SecretId=secret_id, SecretKey=secret_key, Endpoint=endpoint)
# 2. 获取客户端对象
client = CosS3Client(config)
# 参照下文的描述。或者参照 Demo 程序，详见 https://github.com/tencentyun/cos-python-sdk-v5/blob/master/qcloud_cos/demo.py
```

设置全球加速域名:
```python
# APPID 已在配置中移除，请在参数 Bucket 中带上 APPID。Bucket 由 BucketName-APPID 组成
# 1. 设置用户配置, 包括 secretId，secretKey 以及 Region
secret_id = 'COS_SECRETID'      # 替换为用户的 secretId
secret_key = 'COS_SECRETKEY'      # 替换为用户的 secretKey
region = 'COS_REGION'     # 替换为用户的 Region

# domain 自定义域名，通常不用设置，如果使用全球加速域名，则设置成对应的域名，例如 examplebucket-1250000000.cos.accelerate.myqcloud.com，
# 开启全球加速请参考 https://cloud.tencent.com/document/product/436/38864
domain = 'examplebucket-1250000000.cos.accelerate.myqcloud.com'
config = CosConfig(Region=region, SecretId=secret_id, SecretKey=secret_key, Domain=domain)
# 2. 获取客户端对象
client = CosS3Client(config)
# 参照下文的描述。或者参照 Demo 程序，详见 https://github.com/tencentyun/cos-python-sdk-v5/blob/master/qcloud_cos/demo.py
```


### 创建存储桶

[//]: # (.cssg-snippet-put-bucket)
```python
response = client.create_bucket(
    Bucket='examplebucket-1250000000'
)
```

### 查询存储桶列表

[//]: # (.cssg-snippet-get-service)
```python
response = client.list_buckets(
)
```

### 上传对象

>!简单上传不支持超过5G的文件，推荐使用下方高级上传接口。参数说明可参见 [对象操作](https://intl.cloud.tencent.com/document/product/436/31546) 文档。

[//]: # (.cssg-snippet-put-object-comp-comp)
```python
#### 文件流简单上传（不支持超过5G的文件，推荐使用下方高级上传接口）
# 强烈建议您以二进制模式(binary mode)打开文件,否则可能会导致错误
with open('picture.jpg', 'rb') as fp:
    response = client.put_object(
        Bucket='examplebucket-1250000000',
        Body=fp,
        Key='picture.jpg',
        StorageClass='STANDARD',
        EnableMD5=False
    )
print(response['ETag'])

#### 字节流简单上传
response = client.put_object(
    Bucket='examplebucket-1250000000',
    Body=b'bytes',
    Key='picture.jpg',
    EnableMD5=False
)
print(response['ETag'])


#### chunk 简单上传
import requests
stream = requests.get('https://cloud.tencent.com/document/product/436/7778')

# 网络流将以 Transfer-Encoding:chunked 的方式传输到 COS
response = client.put_object(
    Bucket='examplebucket-1250000000',
    Body=stream,
    Key='picture.jpg'
)
print(response['ETag'])

#### 高级上传接口（推荐）
# 根据文件大小自动选择简单上传或分块上传，分块上传具备断点续传功能。
response = client.upload_file(
    Bucket='examplebucket-1250000000',
    LocalFilePath='local.txt',
    Key='picture.jpg',
    PartSize=1,
    MAXThread=10,
    EnableMD5=False
)
print(response['ETag'])
```

### 查询对象列表

[//]: # (.cssg-snippet-get-bucket)
```python
response = client.list_objects(
    Bucket='examplebucket-1250000000',
    Prefix='folder1'
)
```

单次调用`list_objects`接口一次只能查询1000个对象，如需要查询所有的对象，则需要循环调用。

[//]: # (.cssg-snippet-get-bucket-recursive)
```python
marker = ""
while True:
    response = client.list_objects(
        Bucket='examplebucket-1250000000',
        Prefix='folder1',
        Marker=marker
    )
    print(response['Contents'])
    if response['IsTruncated'] == 'false':
        break 
    marker = response['NextMarker']
```

### 下载对象

[//]: # (.cssg-snippet-get-object-comp)
```python
####  获取文件到本地
response = client.get_object(
    Bucket='examplebucket-1250000000',
    Key='picture.jpg',
)
response['Body'].get_stream_to_file('output.txt')

#### 获取文件流
response = client.get_object(
    Bucket='examplebucket-1250000000',
    Key='picture.jpg',
)
fp = response['Body'].get_raw_stream()
print(fp.read(2))

#### 设置 Response HTTP 头部
response = client.get_object(
    Bucket='examplebucket-1250000000',
    Key='picture.jpg',
    ResponseContentType='text/html; charset=utf-8'
)
print(response['Content-Type'])
fp = response['Body'].get_raw_stream()
print(fp.read(2))

#### 指定下载范围
response = client.get_object(
    Bucket='examplebucket-1250000000',
    Key='picture.jpg',
    Range='bytes=0-10'
)
fp = response['Body'].get_raw_stream()
print(fp.read())
```

### 删除对象

[//]: # (.cssg-snippet-delete-object-comp)
```python
# 删除object
## deleteObject
response = client.delete_object(
    Bucket='examplebucket-1250000000',
    Key='exampleobject'
)

# 删除多个object
## deleteObjects
response = client.delete_objects(
    Bucket='examplebucket-1250000000',
    Delete={
        'Object': [
            {
                'Key': 'exampleobject1',
            },
            {
                'Key': 'exampleobject2',
            },
        ],
        'Quiet': 'true'|'false'
    }
)
```
