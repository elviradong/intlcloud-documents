SDK 3.0 is a companion tool for the TencentCloud API 3.0 platform. You can use all [SMS APIs](https://intl.cloud.tencent.com/document/product/382/34689) through the SDK. The new SDK version is unified and features the same SDK usage, API call methods, error codes, and returned packet formats for different programming languages.
>!
>- SMS sending APIs
>One message can be sent to up to 200 numbers at a time.
>- Signature and body template APIs
>Individual users have no permission to use signature and body template APIs and can [manage SMS signatures](https://intl.cloud.tencent.com/document/product/382/35456) and SMS body templates only in the SMS Console. To use the APIs, change "Individual Identity" to "Organizational Identity".



## Prerequisites

- You have activated SMS. For detailed directions, please see [Getting Started with Mainland China SMS](https://intl.cloud.tencent.com/document/product/382/35449).
- If you need to send SMS messages in Mainland China, you need to purchase a Mainland China SMS package first.
- You have prepared the dependent environment: Python v2.7–3.6.
- You have obtained the `SecretID` and `SecretKey` on the **[API Key Management](https://console.cloud.tencent.com/cam/capi)** page in the CAM Console.
 - `SecretID` is used to identify the API caller.
 - `SecretKey` is used to encrypt the string to sign that can be verified on the server. **You should keep it private and avoid disclosure.**
- The call address of the SMS service is `sms.tencentcloudapi.com`.

## Relevant Documents
- For more information on the APIs and their parameters, please see [API Documentation](https://intl.cloud.tencent.com/document/product/382/34689).
- You can download the SDK source code [here](https://github.com/TencentCloud/tencentcloud-sdk-python).

## Installing SDK

### Installation through Pip (recommended)
1. Download and install [pip](https://pip.pypa.io/en/stable/installing/?spm=a3c0i.o32026zh.a3.6.74134958lLSo6o).
2. Run the following command to install the SDK:
```bash
pip install tencentcloud-sdk-python
```

### Installing through source package
1. Go to the [GitHub code hosting page](https://github.com/tencentcloud/tencentcloud-sdk-python) or [quick download address](https://tencentcloud-sdk-1253896243.file.myqcloud.com/tencentcloud-sdk-python/tencentcloud-sdk-python.zip) to download the latest code.
2. After decompressing, run the following commands in sequence to install the SDK.
```
    $ cd tencentcloud-sdk-python
    $ python setup.py install
```

<span id="example"></span>
## Sample Code
>?All samples are for reference only and cannot be directly compiled and executed. You need to modify them based on your actual needs. You can also use [API 3.0 Explorer](https://console.cloud.tencent.com/api/explorer?Product=sms&Version=2019-07-11&Action=SendSms) to automatically generate the demo code as needed.

Each API has a corresponding request structure and a response structure. This document only lists the sample code of several common features. For more samples, please see [SDK for Python Samples](https://github.com/TencentCloud/tencentcloud-sdk-python/tree/master/examples/sms).

### Applying for SMS template

```
# -*- coding: utf-8 -*-
from tencentcloud.common import credential
from tencentcloud.common.exception.tencent_cloud_sdk_exception import TencentCloudSDKException
# Import the client models of the SMS module
from tencentcloud.sms.v20190711 import sms_client, models

# Import the optional configuration classes
from tencentcloud.common.profile.client_profile import ClientProfile
from tencentcloud.common.profile.http_profile import HttpProfile
try:
    # Required steps:
    # Instantiate an authentication object. The Tencent Cloud account key pair `secretId` and `secretKey` need to be passed in as the input parameters
    # This example uses the way to read from the environment variable, so you need to set these two values in the environment variable in advance
    # You can also write the key pair directly into the code, but be careful not to copy, upload, or share the code to others
    # Query the CAM key: https://console.cloud.tencent.com/cam/capi
		
    cred = credential.Credential("secretId", "secretKey")
    # cred = credential.Credential(
    #     os.environ.get(""),
    #     os.environ.get("")
    # )

    # Instantiate an HTTP option (optional; skip if there are no special requirements)
    httpProfile = HttpProfile()
    httpProfile.reqMethod = "POST"  # POST request (POST request by default)
    httpProfile.reqTimeout = 30    # Request timeout period in seconds (60 seconds by default)
    httpProfile.endpoint = "sms.tencentcloudapi.com"  # Specify the access region domain name (nearby access by default)

    # Optional steps:
    # Instantiate a client configuration object. You can specify the timeout period and other configuration items
    clientProfile = ClientProfile()
    clientProfile.signMethod = "TC3-HMAC-SHA256"  # Specify the signature algorithm
    clientProfile.language = "en-US"
    clientProfile.httpProfile = httpProfile

    # Instantiate an SMS client object
    # The second parameter is the region information. You can directly enter the string `ap-guangzhou` or import the preset constant
    client = sms_client.SmsClient(cred, "ap-guangzhou", clientProfile)

    # Instantiate a request object. You can further set the request parameters according to the API called and actual conditions
    # You can directly check the SDK source code to determine which attributes of `SendSmsRequest` can be set
    # An attribute may be of a basic type or import another data structure
    # You are recommended to use the IDE for development where you can easily redirect to and view the documentation of each API and data structure
    req = models.AddSmsTemplateRequest()

    # Settings of a basic parameter:
    # The SDK uses the pointer style to specify parameters, so even for basic parameters, you need to use pointers to assign values to them
    # The SDK provides encapsulation functions for importing the pointers of basic parameters
    # Help link:
    # SMS Console: https://console.cloud.tencent.com/smsv2
    # SMS Helper: https://intl.cloud.tencent.com/document/product/382/3773

    # Template name 
	$req.TemplateName = "Tencent Cloud"
	# Template content 
	$req.TemplateContent = "Your login verification code is {1}. Please enter it within {2} minutes. If the login was not initiated by you, please ignore this message."
	# SMS type. 0: general SMS; 1: marketing SMS 
	$req.SmsType = 0;
	# Whether it is Global SMS:
	# 0: Mainland China SMS
	# 1: global SMS 
	$req.International = 0
	# Template remarks, such as reason for application and use case 
	$req.Remark = "xxx"

    # Initialize the request by calling the `AddSmsTemplate` method on the client object. Note: the request method name corresponds to the request object
    resp = client.AddSmsTemplate(req)

    # A string return packet in JSON format is output
    print(resp.to_json_string(indent=2))

except TencentCloudSDKException as err:
    print(err)
```


### Sending SMS message

```
# -*- coding: utf-8 -*-
from tencentcloud.common import credential
from tencentcloud.common.exception.tencent_cloud_sdk_exception import TencentCloudSDKException
# Import the client models of the SMS module
from tencentcloud.sms.v20190711 import sms_client, models

# Import the optional configuration classes
from tencentcloud.common.profile.client_profile import ClientProfile
from tencentcloud.common.profile.http_profile import HttpProfile
try:
    # Required steps:
    # Instantiate an authentication object. The Tencent Cloud account key pair `secretId` and `secretKey` need to be passed in as the input parameters
    # This example uses the way to read from the environment variable, so you need to set these two values in the environment variable in advance
    # You can also write the key pair directly into the code, but be careful not to copy, upload, or share the code to others
    # Query the CAM key: https://console.cloud.tencent.com/cam/capi
		
    cred = credential.Credential("secretId", "secretKey")
    # cred = credential.Credential(
    #     os.environ.get(""),
    #     os.environ.get("")
    # )

    # Instantiate an HTTP option (optional; skip if there are no special requirements)
    httpProfile = HttpProfile()
    httpProfile.reqMethod = "POST"  # POST request (POST request by default)
    httpProfile.reqTimeout = 30    # Request timeout period in seconds (60 seconds by default)
    httpProfile.endpoint = "sms.tencentcloudapi.com"  # Specify the access region domain name (nearby access by default)

    # Optional steps:
    # Instantiate a client configuration object. You can specify the timeout period and other configuration items
    clientProfile = ClientProfile()
    clientProfile.signMethod = "TC3-HMAC-SHA256"  # Specify the signature algorithm
    clientProfile.language = "en-US"
    clientProfile.httpProfile = httpProfile

    # Instantiate an SMS client object
    # The second parameter is the region information. You can directly enter the string `ap-guangzhou` or import the preset constant
    client = sms_client.SmsClient(cred, "ap-guangzhou", clientProfile)

    # Instantiate a request object. You can further set the request parameters according to the API called and actual conditions
    # You can directly check the SDK source code to determine which attributes of `SendSmsRequest` can be set
    # An attribute may be of a basic type or import another data structure
    # You are recommended to use the IDE for development where you can easily redirect to and view the documentation of each API and data structure
    req = models.SendSmsRequest()

    # Settings of a basic parameter:
    # The SDK uses the pointer style to specify parameters, so even for basic parameters, you need to use pointers to assign values to them
    # The SDK provides encapsulation functions for importing the pointers of basic parameters
    # Help link:
    # SMS Console: https://console.cloud.tencent.com/smsv2
    # SMS Helper: https://intl.cloud.tencent.com/document/product/382/3773

    # SMS application ID, which is the actual `SDKAppID` generated after an application is added in the [SMS Console], such as 1400006666
    req.SmsSdkAppid = "1400787878"
    # The content of SMS signature should be encoded in UTF-8. You must enter an approved signature, which can be viewed in the [SMS Console]
    req.Sign = "xxx"
    # SMS code number extension, which is not activated by default. If you need to activate it, please contact [SMS Helper]
    req.ExtendCode = ""
    # User session content, which can carry context information such as user-side ID and will be returned as-is by the server
    req.SessionContext = "xxx"
    # `senderid` for global SMS, which is not activated by default. If you need to activate it, please contact [SMS Helper] for assistance. This parameter should be left empty for Mainland China SMS
    req.SenderId = ""
    # Target mobile number in the e.164 standard (+[country/region code][mobile number])
    # Example: +8613711112222, which has a + sign followed by 86 (country/region code) and then by 13711112222 (mobile number). Up to 200 mobile numbers are supported
    req.PhoneNumberSet = ["+8613711112222", "+8613711333222", "+8613711144422"]
    # Template ID. You must enter the ID of an approved template, which can be viewed in the [SMS Console]
    req.TemplateID = "449739"
    # Template parameters. If there are no template parameters, leave it empty
    req.TemplateParamSet = ["666"]


    # Initialize the request by calling the `SendSms` method on the client object. Note: the request method name corresponds to the request object
    resp = client.SendSms(req)

    # A string return packet in JSON format is output
    print(resp.to_json_string(indent=2))

except TencentCloudSDKException as err:
    print(err)
```



### Pulling receipt status

```
# -*- coding: utf-8 -*-
from tencentcloud.common import credential
from tencentcloud.common.exception.tencent_cloud_sdk_exception import TencentCloudSDKException
# Import the client models of the SMS module
from tencentcloud.sms.v20190711 import sms_client, models

# Import the optional configuration classes
from tencentcloud.common.profile.client_profile import ClientProfile
from tencentcloud.common.profile.http_profile import HttpProfile
try:
    # Required steps:
    # Instantiate an authentication object. The Tencent Cloud account key pair `secretId` and `secretKey` need to be passed in as the input parameters
    # This example uses the way to read from the environment variable, so you need to set these two values in the environment variable in advance
    # You can also write the key pair directly into the code, but be careful not to copy, upload, or share the code to others
    # Query the CAM key: https://console.cloud.tencent.com/cam/capi
		
    cred = credential.Credential("secretId", "secretKey")
    # cred = credential.Credential(
    #     os.environ.get(""),
    #     os.environ.get("")
    # )

    # Instantiate an HTTP option (optional; skip if there are no special requirements)
    httpProfile = HttpProfile()
    httpProfile.reqMethod = "POST"  # POST request (POST request by default)
    httpProfile.reqTimeout = 30    # Request timeout period in seconds (60 seconds by default)
    httpProfile.endpoint = "sms.tencentcloudapi.com"  # Specify the access region domain name (nearby access by default)

    # Optional steps:
    # Instantiate a client configuration object. You can specify the timeout period and other configuration items
    clientProfile = ClientProfile()
    clientProfile.signMethod = "TC3-HMAC-SHA256"  # Specify the signature algorithm
    clientProfile.language = "en-US"
    clientProfile.httpProfile = httpProfile

    # Instantiate an SMS client object
    # The second parameter is the region information. You can directly enter the string `ap-guangzhou` or import the preset constant
    client = sms_client.SmsClient(cred, "ap-guangzhou", clientProfile)

    # Instantiate a request object. You can further set the request parameters according to the API called and actual conditions
    # You can directly check the SDK source code to determine which attributes of `SendSmsRequest` can be set
    # An attribute may be of a basic type or import another data structure
    # You are recommended to use the IDE for development where you can easily redirect to and view the documentation of each API and data structure
    req = models.PullSmsSendStatusRequest()

    # Settings of a basic parameter:
    # The SDK uses the pointer style to specify parameters, so even for basic parameters, you need to use pointers to assign values to them
    # The SDK provides encapsulation functions for importing the pointers of basic parameters
    # Help link:
    # SMS Console: https://console.cloud.tencent.com/smsv2
    # SMS Helper: https://intl.cloud.tencent.com/document/product/382/3773

    # SMS application ID, which is the actual `SDKAppID` generated after an application is added in the [SMS Console], such as 1400006666
    req.SmsSdkAppid = "1400787878"
    # Maximum number of pulled entries. Maximum value: 100
    req.Limit = 10

    # Initialize the request by calling the `PullSmsSendStatus` method on the client object. Note: the request method name corresponds to the request object
    resp = client.PullSmsSendStatus(req)

    # A string return packet in JSON format is output
    print(resp.to_json_string(indent=2))


except TencentCloudSDKException as err:
    print(err)
```


### Collecting SMS message sending data

```
# -*- coding: utf-8 -*-
from tencentcloud.common import credential
from tencentcloud.common.exception.tencent_cloud_sdk_exception import TencentCloudSDKException
# Import the client models of the SMS module
from tencentcloud.sms.v20190711 import sms_client, models

# Import the optional configuration classes
from tencentcloud.common.profile.client_profile import ClientProfile
from tencentcloud.common.profile.http_profile import HttpProfile
try:
    # Required steps:
    # Instantiate an authentication object. The Tencent Cloud account key pair `secretId` and `secretKey` need to be passed in as the input parameters
    # This example uses the way to read from the environment variable, so you need to set these two values in the environment variable in advance
    # You can also write the key pair directly into the code, but be careful not to copy, upload, or share the code to others
    # Query the CAM key: https://console.cloud.tencent.com/cam/capi
		
    cred = credential.Credential("secretId", "secretKey")
    # cred = credential.Credential(
    #     os.environ.get(""),
    #     os.environ.get("")
    # )

    # Instantiate an HTTP option (optional; skip if there are no special requirements)
    httpProfile = HttpProfile()
    httpProfile.reqMethod = "POST"  # POST request (POST request by default)
    httpProfile.reqTimeout = 30    # Request timeout period in seconds (60 seconds by default)
    httpProfile.endpoint = "sms.tencentcloudapi.com"  # Specify the access region domain name (nearby access by default)

    # Optional steps:
    # Instantiate a client configuration object. You can specify the timeout period and other configuration items
    clientProfile = ClientProfile()
    clientProfile.signMethod = "TC3-HMAC-SHA256"  # Specify the signature algorithm
    clientProfile.language = "en-US"
    clientProfile.httpProfile = httpProfile

    # Instantiate an SMS client object
    # The second parameter is the region information. You can directly enter the string `ap-guangzhou` or import the preset constant
    client = sms_client.SmsClient(cred, "ap-guangzhou", clientProfile)

    # Instantiate a request object. You can further set the request parameters according to the API called and actual conditions
    # You can directly check the SDK source code to determine which attributes of `SendSmsRequest` can be set
    # An attribute may be of a basic type or import another data structure
    # You are recommended to use the IDE for development where you can easily redirect to and view the documentation of each API and data structure
    req = models.SendStatusStatisticsRequest()

    # Settings of a basic parameter:
    # The SDK uses the pointer style to specify parameters, so even for basic parameters, you need to use pointers to assign values to them
    # The SDK provides encapsulation functions for importing the pointers of basic parameters
    # Help link:
    # SMS Console: https://console.cloud.tencent.com/smsv2
    # SMS Helper: https://intl.cloud.tencent.com/document/product/382/3773

    # SMS application ID, which is the actual `SDKAppID` generated after an application is added in the [SMS Console], such as 1400006666
    req.SmsSdkAppid = "1400787878"
    # Maximum number of pulled entries. Maximum value: 100
    req.Limit = 10
    # Offset. Note: this parameter is currently fixed at 0
    req.Offset = 0
    # Start time of pull in the format of `yyyymmddhh` accurate to the hour
    req.StartDateTime = 2019122400
    # End time of pull in the format of `yyyymmddhh` accurate to the hour
    # Note: `EndDataTime` must be later than `StartDateTime`
    req.EndDataTime = 2019122523

    # Initialize the request by calling the `SendStatusStatistics` method on the client object. Note: the request method name corresponds to the request object
    resp = client.SendStatusStatistics(req)

    # A string return packet in JSON format is output
    print(resp.to_json_string(indent=2))

except TencentCloudSDKException as err:
    print(err)
```


