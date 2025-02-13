## 소개

객체의 업로드, 복사 작업 관련 API 개요 및 SDK 예시 코드에 대한 설명입니다.


**간단한 조작**

| API                                                          | 작업명         | 설명                                  |
| ------------------------------------------------------------ | -------------- | ----------------------------------------- |
| [PUT Object](https://intl.cloud.tencent.com/document/product/436/7749) | 간단한 객체 업로드       | 객체 하나를 버킷에 업로드     |
| [POST Object](https://intl.cloud.tencent.com/document/product/436/14690) | 객체 업로드 포맷   | 포맷을 사용한 객체 업로드 요청                      |
| [PUT Object - Copy](https://intl.cloud.tencent.com/document/product/436/10881) | 객체 복사 설정(객체 속성 수정)   | 파일을 타깃 경로에 복사                       |

**파트 작업**

| API                                                          | 작업명         | 설명                             |
| ------------------------------------------------------------ | -------------- | ------------------------------------ |
| [List Multipart Uploads](https://intl.cloud.tencent.com/document/product/436/7736) | 멀티파트 업로드 조회   | 현재 진행 중인 멀티파트 업로드 정보 조회         |
| [Initiate Multipart Upload](https://intl.cloud.tencent.com/document/product/436/7746) | 멀티파트 업로드 초기화 | 멀티파트 업로드 작업 초기화     |
| [Upload Part](https://intl.cloud.tencent.com/document/product/436/7750) | 멀티파트 업로드       | 멀티파트 업로드 객체                        |
| [Upload Part - Copy](https://intl.cloud.tencent.com/document/product/436/8287) | 멀티파트 복사       | 다른 객체를 한 파트로 복사             |
| [List Parts](https://intl.cloud.tencent.com/document/product/436/7747) | 업로드된 파트 조회   | 특정 멀티파트 업로드 작업에서 업로드된 멀티파트 조회   |
| [Complete Multipart Upload](https://intl.cloud.tencent.com/document/product/436/7742) | 멀티파트 업로드 완료   | 전체 파일의 멀티파트 업로드 완료               |
| [Abort Multipart Upload](https://intl.cloud.tencent.com/document/product/436/7740) | 멀티파트 업로드 중지   | 멀티파트 업로드 작업 중지 및 업로드된 멀티파트 제거 |

## SDK API 참조

SDK 모든 인터페이스의 구체적인 매개변수와 방법 설명입니다. [SDK API](https://cos-ios-sdk-doc-1253960454.file.myqcloud.com/)를 참조하십시오.

## 고급 인터페이스(권장)

### 객체 업로드

고급 인터페이스의 간단한 업로드, 멀티파트 업로드 인터페이스를 캡슐화합니다. 파일 크기에 따라 스마트한 업로드 방식을 선택하며, 동시에 중단 시 재개 기능을 지원합니다.

#### 예시 코드1: 로컬 파일 업로드
**Objective-C**

[//]: # (.cssg-snippet-transfer-upload-file)
```objective-c
QCloudCOSXMLUploadObjectRequest* put = [QCloudCOSXMLUploadObjectRequest new];
/** 로컬 파일 경로. URL이 file:// 로 시작되는지 확인하십시오. 형식은 다음과 같습니다.
1. [NSURL URLWithString:@"file:////var/mobile/Containers/Data/Application/DBPF7490-D5U8-4ABF-A0AF-CC49D6A60AEB/Documents/exampleobject"]
2. [NSURL fileURLWithPath:@"/var/mobile/Containers/Data/Application/DBPF7490-D5U8-4ABF-A0AF-CC49D6A60AEB/Documents/exampleobject"]
*/
NSURL* url = [NSURL fileURLWithPath:@"파일의 URL"];
// 버킷 이름. 형식은 BucketName-APPID
put.bucket = @"examplebucket-1250000000";
// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
put.object = @"exampleobject";
// 업로드가 필요한 객체 콘텐츠입니다. NSData* 또는 NSURL* 유형의 변수를 전달할 수 있습니다.
put.body =  url;
// 업로드 진행도 수신
[put setSendProcessBlock:^(int64_t bytesSent,
                           int64_t totalBytesSent,
                           int64_t totalBytesExpectedToSend) {
    // bytesSent                   증가 바이트 수
    // totalBytesSent              이번 업로드의 총 바이트 수
    // totalBytesExpectedToSend    로컬 업로드의 타깃 바이트 수
}];
// 업로드 결과 수신
[put setFinishBlock:^(id outputObject, NSError *error) {
    // outputObject에서 response의 etag 혹은 사용자 정의 헤더 등 정보 획득 가능
    NSDictionary * result = (NSDictionary *)outputObject;
}];
[put setInitMultipleUploadFinishBlock:^(QCloudInitiateMultipartUploadResult *
                                        multipleUploadInitResult,
                                        QCloudCOSXMLUploadObjectResumeData resumeData) {
    // 멀티파트 업로드 초기화 완료 후 해당 block 콜백. 이곳에서 resumeData, uploadid를 획득할 수 있습니다.
    NSString* uploadId = multipleUploadInitResult.uploadId;
}];
[[QCloudCOSTransferMangerService defaultCOSTransferManager] UploadObject:put];
```

>?
>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/TransferUploadObject.m)을 참조하십시오.
>- 업로드 후 같은 Key를 사용해 파일 다운로드 링크를 생성할 수 있습니다. 자세한 방법은 **미리 서명된 링크 생성** 문서를 참조하십시오. 문서의 권한이 개인 읽기일 경우, 다운로드 링크에 유효 기간이 있습니다.

**Swift**

[//]: # (.cssg-snippet-transfer-upload-file)
```swift
let put:QCloudCOSXMLUploadObjectRequest = QCloudCOSXMLUploadObjectRequest<AnyObject>();

// 버킷 이름. 형식은 BucketName-APPID
put.bucket = "examplebucket-1250000000";

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
put.object = "exampleobject";

// 업로드가 필요한 객체 콘텐츠입니다. NSData* 또는 NSURL* 유형의 변수를 전달할 수 있습니다.
put.body = NSURL.fileURL(withPath: "Local File Path") as AnyObject;

// 업로드 결과 수신
put.setFinish { (result, error) in
    // 업로드 결과 획득
    if let result = result {
        // 파일의 etag
        let eTag = result.eTag
    } else {
        print(error!);
    }
}

// 업로드 진행도 수신
put.sendProcessBlock = { (bytesSent, totalBytesSent,
    totalBytesExpectedToSend) in
    // bytesSent                   증가 바이트 수
    // totalBytesSent              이번 업로드의 총 바이트 수
    // totalBytesExpectedToSend    로컬 업로드의 타깃 바이트 수
};
// 업로드 매개변수 설정
put.initMultipleUploadFinishBlock = {(multipleUploadInitResult, resumeData) in
    // 멀티파트 업로드 초기화 완료 후 해당 block 콜백, 이곳에서 resumeData 및 uploadId를 획득할 수 있습니다.
    if let multipleUploadInitResult = multipleUploadInitResult {
        let uploadId = multipleUploadInitResult.uploadId
    }
}

QCloudCOSTransferMangerService.defaultCOSTransferManager().uploadObject(put);
```

>?
>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/TransferUploadObject.swift)를 참조하십시오.
>- 업로드 후 같은 Key를 사용해 파일 다운로드 링크를 생성할 수 있습니다. 자세한 방법은 **미리 서명된 링크 생성** 문서를 참조하십시오. 문서의 권한이 개인 읽기일 경우, 다운로드 링크에 유효 기간이 있습니다.

#### 예시 코드2: 이진법 데이터 업로드
**Objective-C**

[//]: # (.cssg-snippet-transfer-upload-bytes)
```objective-c
QCloudCOSXMLUploadObjectRequest* put = [QCloudCOSXMLUploadObjectRequest new];

// 버킷 이름. 형식은 BucketName-APPID
put.bucket = @"examplebucket-1250000000";

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
put.object = @"exampleobject";

// 업로드가 필요한 객체 콘텐츠입니다. NSData* 또는 NSURL* 유형의 변수를 전달할 수 있습니다.
put.body = [@"My Example Content" dataUsingEncoding:NSUTF8StringEncoding];

// 업로드 진행도 수신
[put setSendProcessBlock:^(int64_t bytesSent,
                           int64_t totalBytesSent,
                           int64_t totalBytesExpectedToSend) {
    // bytesSent                   증가 바이트 수
    // totalBytesSent              이번 업로드의 총 바이트 수
    // totalBytesExpectedToSend    로컬 업로드의 타깃 바이트 수
}];

// 업로드 결과 수신
[put setFinishBlock:^(id outputObject, NSError *error) {
    // outputObject는 모든 상응하는 http 헤더 포함
    NSDictionary* info = (NSDictionary *) outputObject;
}];
[[QCloudCOSTransferMangerService defaultCOSTransferManager] UploadObject:put];
```

>?
>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/TransferUploadObject.m)을 참조하십시오.
>- 업로드 후 같은 Key를 사용해 파일 다운로드 링크를 생성할 수 있습니다. 자세한 방법은 **미리 서명된 링크 생성** 문서를 참조하십시오. 문서의 권한이 개인 읽기일 경우, 다운로드 링크에 유효 기간이 있습니다.

**Swift**

[//]: # (.cssg-snippet-transfer-upload-bytes)
```swift
let put:QCloudCOSXMLUploadObjectRequest = QCloudCOSXMLUploadObjectRequest<AnyObject>();

// 버킷 이름. 형식은 BucketName-APPID
put.bucket = "examplebucket-1250000000";

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
put.object = "exampleobject";

// 업로드가 필요한 객체 콘텐츠
let dataBody:NSData = "wrwrwrwrwrw".data(using: .utf8)! as NSData;
put.body = dataBody;

// 업로드 결과 수신
put.setFinish { (result, error) in
    // 업로드 결과 획득
    if let result = result {
        // 파일의 etag
        let eTag = result.eTag
    } else {
        print(error!);
    }
}

// 업로드 진행도 수신
put.sendProcessBlock = { (bytesSent, totalBytesSent,
    totalBytesExpectedToSend) in
    
    // bytesSent                   증가 바이트 수
    // totalBytesSent              이번 업로드의 총 바이트 수
    // totalBytesExpectedToSend    로컬 업로드의 타깃 바이트 수
};

QCloudCOSTransferMangerService.defaultCOSTransferManager().uploadObject(put);
```

>?
>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/TransferUploadObject.swift)를 참조하십시오.
>- 업로드 후 같은 Key를 사용해 파일 다운로드 링크를 생성할 수 있습니다. 자세한 방법은 **미리 서명된 링크 생성** 문서를 참조하십시오. 문서의 권한이 개인 읽기일 경우, 다운로드 링크에 유효 기간이 있습니다.

#### 예시 코드3: 업로드 일시 정지, 계속 및 취소
**Objective-C**

업로드 작업은 다음 방식으로 일시 정지할 수 있습니다.

[//]: # (.cssg-snippet-transfer-upload-pause)
```objective-c
NSError *error;
NSData *resmeData = [put cancelByProductingResumeData:&error];
```

일시 중지 후 다음 방식으로 업로드를 재개할 수 있습니다.

[//]: # (.cssg-snippet-transfer-upload-resume)
```objective-c
QCloudCOSXMLUploadObjectRequest *resumeRequest = [QCloudCOSXMLUploadObjectRequest requestWithRequestData:resmeData];
[[QCloudCOSTransferMangerService defaultCOSTransferManager] UploadObject:resumeRequest];
```

다음 방식으로 업로드를 취소할 수 있습니다.

[//]: # (.cssg-snippet-transfer-upload-cancel)
```objective-c
//해당 업로드 삭제
[put abort:^(id outputObject, NSError *error) {
    
}];
```

>?
>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/TransferUploadObject.m)을 참조하십시오.

**Swift**

업로드 작업은 다음 방식으로 일시 정지할 수 있습니다.

[//]: # (.cssg-snippet-transfer-upload-pause)
```swift
var error : NSError?;
var uploadResumeData:Data = put.cancel(byProductingResumeData:&error) as Data;
```

일시 중지 후 다음 방식으로 업로드를 재개할 수 있습니다.

[//]: # (.cssg-snippet-transfer-upload-resume)
```swift
var resumeRequest = QCloudCOSXMLUploadObjectRequest<AnyObject>.init(request: uploadResumeData);
QCloudCOSTransferMangerService.defaultCOSTransferManager().uploadObject(resumeRequest);
```

다음 방식으로 업로드를 취소할 수 있습니다.

[//]: # (.cssg-snippet-transfer-upload-cancel)
```swift
//해당 업로드 삭제
put.abort { (outputObject, error) in
    
}
```

>?
>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/TransferUploadObject.swift)를 참조하십시오.

#### 예시 코드4: 일괄 업로드
**Objective-C**

[//]: # (.cssg-snippet-transfer-batch-upload-objects)
```objective-c
for (int i = 0; i<20; i++) {
    QCloudCOSXMLUploadObjectRequest* put = [QCloudCOSXMLUploadObjectRequest new];
    
    // 버킷 이름. 형식은 BucketName-APPID
    put.bucket = @"examplebucket-1250000000";
    
    // 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
    put.object = [NSString stringWithFormat:@"exampleobject-%d",i];
    
    // 업로드가 필요한 객체 콘텐츠입니다. NSData* 또는 NSURL* 유형의 변수를 전달할 수 있습니다.
    put.body = [@"My Example Content" dataUsingEncoding:NSUTF8StringEncoding];
    
    // 업로드 진행도 수신
    [put setSendProcessBlock:^(int64_t bytesSent,
                               int64_t totalBytesSent,
                               int64_t totalBytesExpectedToSend) {
        // bytesSent                   증가 바이트 수
        // totalBytesSent              이번 업로드의 총 바이트 수
        // totalBytesExpectedToSend    로컬 업로드의 타깃 바이트 수
    }];
    
    // 업로드 결과 수신
    [put setFinishBlock:^(id outputObject, NSError *error) {
        // outputObject는 모든 상응하는 http 헤더 포함
        NSDictionary* info = (NSDictionary *) outputObject;
    }];
    [[QCloudCOSTransferMangerService defaultCOSTransferManager] UploadObject:put];
}
```

**Swift**

[//]: # (.cssg-snippet-transfer-batch-upload-objects)
```swift
for i in 1...10 {
    let put:QCloudCOSXMLUploadObjectRequest = QCloudCOSXMLUploadObjectRequest<AnyObject>();
    
    // 버킷 이름. 형식은 BucketName-APPID
    put.bucket = "examplebucket-1250000000";
    
    // 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
    put.object = "exampleobject-".appendingFormat("%d", i);
    
    // 업로드가 필요한 객체 콘텐츠
    let dataBody:NSData = "wrwrwrwrwrw".data(using: .utf8)! as NSData;
    put.body = dataBody;
    
    // 업로드 결과 수신
    put.setFinish { (result, error) in
        // 업로드 결과 획득
        if let result = result {
            // 파일의 etag
            let eTag = result.eTag
        } else {
            print(error!);
        }
    }

    // 업로드 진행도 수신
    put.sendProcessBlock = { (bytesSent, totalBytesSent,
        totalBytesExpectedToSend) in
        
        // bytesSent                   증가 바이트 수
        // totalBytesSent              이번 업로드의 총 바이트 수
        // totalBytesExpectedToSend    로컬 업로드의 타깃 바이트 수
    };
    
    QCloudCOSTransferMangerService.defaultCOSTransferManager().uploadObject(put);
}
```

### 객체 복사

고급 인터페이스의 간편한 복사, 파트 복사 인터페이스의 비동기화 요청을 캡슐화합니다. 일시 정지, 복구 및 복사 요청 취소를 지원합니다.

#### 예시 코드
**Objective-C**

[//]: # (.cssg-snippet-transfer-copy-object)
```objective-c
QCloudCOSXMLCopyObjectRequest* request = [[QCloudCOSXMLCopyObjectRequest alloc] init];

// 버킷 이름. 형식은 BucketName-APPID
request.bucket = @"examplebucket-1250000000";

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
request.object = @"exampleobject";

// 파일 출처 버킷, 공개 읽기 혹은 현재 계정에 권한이 있어야 합니다.
request.sourceBucket = @"sourcebucket-1250000000";

// 소스 파일 이름
request.sourceObject = @"sourceObject";

// 소스 파일의 APPID
request.sourceAPPID = @"1250000000";

// 출처 리전
request.sourceRegion= @"COS_REGION";

[request setFinishBlock:^(QCloudCopyObjectResult* result, NSError* error) {
    // outputObject에서 response의 etag 혹은 사용자 정의 헤더 등 정보 획득 가능
}];

// 리전 간 복제를 할 경우 이곳에 사용한 transferManager가 있는 region은 반드시 타깃 버킷이 있는 region이어야 합니다.
[[QCloudCOSTransferMangerService defaultCOSTransferManager] CopyObject:request];

// copy 취소
// copy를 취소할 경우 cancel 호출 방식
[request cancel];
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/TransferCopyObject.m)을 참조하십시오.

**Swift**

[//]: # (.cssg-snippet-transfer-copy-object)
```swift
let copyRequest =  QCloudCOSXMLCopyObjectRequest.init();

// 버킷 이름. 형식은 BucketName-APPID
copyRequest.bucket = "examplebucket-1250000000";

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
copyRequest.object = "exampleobject";

// 파일 출처 버킷, 공개 읽기 혹은 현재 계정에 권한이 있어야 합니다.
// 버킷 이름. 형식은 BucketName-APPID
copyRequest.sourceBucket = "sourcebucket-1250000000";

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
copyRequest.sourceObject = "sourceObject";

// 소스 파일의 APPID
copyRequest.sourceAPPID = "1250000000";

// 출처 리전
copyRequest.sourceRegion = "COS_REGION";

copyRequest.setFinish { (copyResult, error) in
    if let copyResult = copyResult {
        // 파일의 etag
        let eTag = copyResult.eTag
    } else {
        print(error!);
    }
    
}
// 리전 간 복제를 할 경우 이곳에 사용한 transferManager가 있는 region은 반드시 타깃 버킷이 있는 region이어야 합니다.
QCloudCOSTransferMangerService.defaultCOSTransferManager().copyObject(copyRequest);

// copy 취소
// copy를 취소할 경우 cancel 호출 방식
copyRequest.cancel();
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/TransferCopyObject.swift)을 참조하십시오.

## 간단한 작업

### 간단한 객체 업로드

#### 기능 설명

PUT Object 인터페이스는 한 객체를 지정 버킷으로 업로드할 수 있습니다. 해당 작업 요청자는 버킷 WRITE 권한이 있어야 합니다. 최대 5GB를 초과하지 않는 객체까지 업로드할 수 있으며, 5GB 이상의 객체는 [멀티파트 업로드](#.E5.88.86.E5.9D.97.E6.93.8D.E4.BD.9C) 혹은 [고급 인터페이스](#.E9.AB.98.E7.BA.A7.E6.8E.A5.E5.8F.A3.EF.BC.88.E6.8E.A8.E8.8D.90.EF.BC.89)로 업로드 하십시오.

> ! Key(파일명) `/`로 끝날 수 없습니다. 그렇지 않을 경우 폴더로 식별됩니다.

#### 예시 코드
**Objective-C**

[//]: # (.cssg-snippet-put-object)
```objective-c
QCloudPutObjectRequest* put = [QCloudPutObjectRequest new];

// 버킷 이름. 형식은 BucketName-APPID
put.bucket = @"examplebucket-1250000000";

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
put.object = @"exampleobject";

// 파일 내용, NSData* 혹은 NSURL* 유형의 변수를 전달할 수 있습니다.
put.body =  [@"testFileContent" dataUsingEncoding:NSUTF8StringEncoding];

[put setFinishBlock:^(id outputObject, NSError *error) {
    // outputObject는 모든 상응하는 http 헤더 포함
    NSDictionary* info = (NSDictionary *) outputObject;
}];

[[QCloudCOSXMLService defaultCOSXML] PutObject:put];
```

>?
>- 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/PutObject.m)를 참조하십시오.
>- 업로드 후 같은 Key를 사용해 파일 다운로드 링크를 생성할 수 있습니다. 자세한 방법은 **미리 서명된 링크 생성** 문서를 참조하십시오. 문서의 권한이 개인 읽기일 경우, 다운로드 링크에 유효 기간이 있습니다.

**Swift**

[//]: # (.cssg-snippet-put-object)
```swift
let putObject = QCloudPutObjectRequest<AnyObject>.init();

// 버킷 이름. 형식은 BucketName-APPID
putObject.bucket = "examplebucket-1250000000";
// 업로드가 필요한 객체 콘텐츠입니다. NSData* 또는 NSURL* 유형의 변수를 전달할 수 있습니다.
let dataBody:NSData? = "wrwrwrwrwrw".data(using: .utf8) as NSData?;
putObject.body =  dataBody!;

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
putObject.object = "exampleobject";
putObject.finishBlock = {(result,error) in
    if let result = result {
        // result에 상응하는 header 정보 포함
    } else {
        print(error!);
    }
}
QCloudCOSXMLService.defaultCOSXML().putObject(putObject);
```

>?
>- 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/PutObject.swift)을 참조하십시오.
>- 업로드 후 같은 Key를 사용해 파일 다운로드 링크를 생성할 수 있습니다. 자세한 방법은 **미리 서명된 링크 생성** 문서를 참조하십시오. 문서의 권한이 개인 읽기일 경우, 다운로드 링크에 유효 기간이 있습니다.

### 객체 복사(속성 수정)

타깃 경로에 파일을 복사합니다(PUT Object-Copy).

#### 예시 코드1: 객체 복사 시 객체의 속성 보관
**Objective-C**

[//]: # (.cssg-snippet-copy-object)
```objective-c
QCloudPutObjectCopyRequest* request = [[QCloudPutObjectCopyRequest alloc] init];
// 버킷 이름. 형식은 BucketName-APPID
request.bucket = @"examplebucket-1250000000";
// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
request.object = @"exampleobject";
// 메타데이터 복사 여부, 열거 값: Copy, Replaced, 기본값 Copy
// Copy로 표기할 경우 Header의 사용자 메타데이터 정보를 무시하고 복사
// 표시가 Replaced일 경우 Header 정보에 따라 메타데이터를 수정합니다. 타깃 경로와 소스 경로가 일치하고 사용자가 메타데이터 수정을 시도할 경우 반드시 Replaced됩니다.
request.metadataDirective = @"Copy";
// Object의 ACL 속성 정의, 유효값: private, public-read, default
// 기본값: default(Bucket의 권한 상속)
// 주의사항: Object ACL 컨트롤이 필요한 경우 default를 입력하십시오.
// 혹은 이 옵션을 설정하지 않으면 기본적으로 Bucket 권한을 상속합니다.
request.accessControlList = @"default";
// 소스 객체 소재 경로
request.objectCopySource =
@"sourcebucket-1250000000.cos.ap-guangzhou.myqcloud.com/sourceObject";
// 소스 파일의 versionID 지정, 활성화되거나 활성화 후 일시 정지된 버킷만 해당 매개변수에 영향을 줍니다.
request.versionID = @"objectVersion1";
[request setFinishBlock:^(QCloudCopyObjectResult * _Nonnull result,
                          NSError * _Nonnull error) {
    // result 반환 구체 정보
 
}];
[[QCloudCOSXMLService defaultCOSXML]  PutObjectCopy:request];
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/CopyObject.m)을 참조하십시오.

**Swift**

[//]: # (.cssg-snippet-copy-object)
```swift
let putObjectCopy = QCloudPutObjectCopyRequest.init();
// 버킷 이름. 형식은 BucketName-APPID
putObjectCopy.bucket = "examplebucket-1250000000";
// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
putObjectCopy.object = "exampleobject";
// 소스 객체 소재 경로
putObjectCopy.objectCopySource = "sourcebucket-1250000000.cos.ap-guangzhou.myqcloud.com/sourceObject";
// 메타데이터 복사 여부, 열거 값: Copy, Replaced, 기본값 Copy
// Copy로 표기할 경우 Header의 사용자 메타데이터 정보를 무시하고 복사
// 표시가 Replaced일 경우 Header 정보에 따라 메타데이터를 수정합니다. 타깃 경로와 소스 경로가 일치하고 사용자가 메타데이터 수정을 시도할 경우 반드시 Replaced됩니다.
putObjectCopy.metadataDirective = "Copy";
// Object의 ACL 속성 정의, 유효값: private, public-read, default
// 기본값: default(Bucket의 권한 상속)
// 주의사항: Object ACL 컨트롤이 필요한 경우 default를 입력하십시오.
// 혹은 이 옵션을 설정하지 않으면 기본적으로 Bucket 권한을 상속합니다.
putObjectCopy.accessControlList = "default";
// 소스 파일의 versionID 지정, 활성화되거나 활성화 후 일시 정지된 버킷만 해당 매개변수에 영향을 줍니다.
putObjectCopy.versionID = "versionID";
putObjectCopy.setFinish { (result, error) in
    if let result = result {
        let eTag = result.eTag
    } else {
        print(error!);
    }
}
QCloudCOSXMLService.defaultCOSXML().putObjectCopy(putObjectCopy);
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/CopyObject.swift)을 참조하십시오.

#### 예시 코드2: 객체 복사 시 객체의 속성 변경
**Objective-C**

[//]: # (.cssg-snippet-copy-object-replaced)
```objective-c
QCloudPutObjectCopyRequest* request = [[QCloudPutObjectCopyRequest alloc] init];
// 버킷 이름. 형식은 BucketName-APPID
request.bucket = @"examplebucket-1250000000";
// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
request.object = @"exampleobject";
// 메타데이터 복사 여부, 열거 값: Copy, Replaced, 기본값 Copy
// Copy로 표기할 경우 Header의 사용자 메타데이터 정보를 무시하고 복사
// 표시가 Replaced일 경우 Header 정보에 따라 메타데이터를 수정합니다. 타깃 경로와 소스 경로가 일치하고 사용자가 메타데이터 수정을 시도할 경우 반드시 Replaced됩니다.
request.metadataDirective = @"Replaced";
// 메타데이터 수정
[request.customHeaders setValue:@"newValue" forKey:@"x-cos-meta-*"];
// 객체 스토리지 유형, 열거 값은 스토리지 유형 개요 문서를 참조하십시오. 예시: MAZ_STANDARD, MAZ_STANDARD_IA,
// STANDARD_IA, ARCHIVE. 객체가 표준 스토리지가 아닐 경우에만(STANDARD) 해당 헤더가 반환됩니다.
// 스토리지 유형 수정
[request.customHeaders setValue:@"newValue" forKey:@"x-cos-storage-class"];
// Object의 ACL 속성 정의, 유효값: private, public-read, default
// 기본값: default(Bucket의 권한 상속)
// 주의사항: Object ACL 컨트롤이 필요한 경우 default를 입력하십시오.
// 혹은 이 옵션을 설정하지 않으면 기본적으로 Bucket 권한을 상속합니다.
// acl 수정
request.accessControlList = @"private";
// 소스 객체 소재 경로
request.objectCopySource =
    @"sourcebucket-1250000000.cos.ap-guangzhou.myqcloud.com/sourceObject";

// 소스 파일의 versionID 지정, 활성화되거나 활성화 후 일시 정지된 버킷만 해당 매개변수에 영향을 줍니다.
request.versionID = @"objectVersion1";

[request setFinishBlock:^(QCloudCopyObjectResult * _Nonnull result,
                          NSError * _Nonnull error) {
    // result 반환 구체 정보
    
}];
[[QCloudCOSXMLService defaultCOSXML]  PutObjectCopy:request];
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/CopyObject.m)을 참조하십시오.

**Swift**

[//]: # (.cssg-snippet-copy-object-replaced)
```swift
let request : QCloudPutObjectCopyRequest  = QCloudPutObjectCopyRequest();
// 버킷 이름. 형식은 BucketName-APPID
request.bucket = "examplebucket-1250000000";
// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
request.object = "exampleobject";
// 메타데이터 복사 여부, 열거 값: Copy, Replaced, 기본값 Copy
// Copy로 표기할 경우 Header의 사용자 메타데이터 정보를 무시하고 복사
// 표시가 Replaced일 경우 Header 정보에 따라 메타데이터를 수정합니다. 타깃 경로와 소스 경로가 일치하고 사용자가 메타데이터 수정을 시도할 경우 반드시 Replaced됩니다.
request.metadataDirective = "Replaced";
// 메타데이터 수정
request.customHeaders.setValue("newValue", forKey: "x-cos-meta-*");
// 객체 스토리지 유형, 열거 값은 스토리지 유형 개요 문서를 참조하십시오. 예시: MAZ_STANDARD, MAZ_STANDARD_IA,
// STANDARD_IA, ARCHIVE. 객체가 표준 스토리지가 아닐 경우에만(STANDARD) 해당 헤더가 반환됩니다.
// 스토리지 유형 수정
request.customHeaders.setValue("newValue", forKey: "x-cos-storage-class");
// Object의 ACL 속성 정의, 유효값: private, public-read, default
// 기본값: default(Bucket의 권한 상속)
// 주의사항: Object ACL 컨트롤이 필요한 경우 default를 입력하십시오.
// 혹은 이 옵션을 설정하지 않으면 기본적으로 Bucket 권한을 상속합니다.
// acl 수정
request.accessControlList = "소스 파일 acl";
// 소스 객체 소재 경로
request.objectCopySource = "sourcebucket-1250000000.cos.ap-guangzhou.myqcloud.com/sourceObject";
// 소스 파일의 versionID 지정, 활성화되거나 활성화 후 일시 정지된 버킷만 해당 매개변수에 영향을 줍니다.
request.versionID = "versionID";
request.setFinish { (result, error) in
    if let result = result {
        let eTag = result.eTag
    } else {
        print(error!);
    }
       
}
QCloudCOSXMLService.defaultCOSXML().putObjectCopy(request);
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/CopyObject.swift)을 참조하십시오.

#### 예시 코드3: 객체 메타데이터 수정
**Objective-C**

[//]: # (.cssg-snippet-modify-object-metadata)
```objective-c
QCloudPutObjectCopyRequest* request = [[QCloudPutObjectCopyRequest alloc] init];
// 버킷 이름. 형식은 BucketName-APPID
request.bucket = @"examplebucket-1250000000";
// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
request.object = @"exampleobject";
// 메타데이터 복사 여부, 열거 값: Copy, Replaced, 기본값 Copy
// Copy로 표기할 경우 Header의 사용자 메타데이터 정보를 무시하고 복사
// 표시가 Replaced일 경우 Header 정보에 따라 메타데이터를 수정합니다. 타깃 경로와 소스 경로가 일치하고 사용자가 메타데이터 수정을 시도할 경우 반드시 Replaced됩니다.
request.metadataDirective = @"Replaced";
// 사용자 정의 객체 header
[request.customHeaders setValue:@"newValue" forKey:@"x-cos-meta-*"];
// Object의 ACL 속성 정의, 유효값: private, public-read, default
// 기본값: default(Bucket의 권한 상속)
// 주의사항: Object ACL 컨트롤이 필요한 경우 default를 입력하십시오.
// 혹은 이 옵션을 설정하지 않으면 기본적으로 Bucket 권한을 상속합니다.
request.accessControlList = @"default";
// 소스 객체 소재 경로
request.objectCopySource =
    @"examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/exampleobject";

[request setFinishBlock:^(QCloudCopyObjectResult * _Nonnull result,
                          NSError * _Nonnull error) {
    // result 반환 구체 정보
    
}];
[[QCloudCOSXMLService defaultCOSXML]  PutObjectCopy:request];
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/ModifyObjectProperty.m)을 참조하십시오.

**Swift**

[//]: # (.cssg-snippet-modify-object-metadata)
```swift
let request : QCloudPutObjectCopyRequest = QCloudPutObjectCopyRequest();

// 버킷 이름. 형식은 BucketName-APPID
request.bucket = "examplebucket-1250000000";

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
request.object = "exampleobject";

// 메타데이터 복사 여부, 열거 값: Copy, Replaced, 기본값 Copy
// Copy로 표기할 경우 Header의 사용자 메타데이터 정보를 무시하고 복사
// 표시가 Replaced일 경우 Header 정보에 따라 메타데이터를 수정합니다. 타깃 경로와 소스 경로가 일치하고
//  사용자가 메타데이터 수정을 시도할 경우 반드시 Replaced됩니다.
request.metadataDirective = "Replaced";

// 사용자 정의 객체 header
request.customHeaders.setValue("newValue", forKey: "x-cos-meta-*")

// Object의 ACL 속성 정의, 유효값: private, public-read, default
// 기본값: default(Bucket의 권한 상속)
// 주의사항: Object ACL 컨트롤이 필요한 경우 default를 입력하십시오.
// 혹은 이 옵션을 설정하지 않으면 기본적으로 Bucket 권한을 상속합니다.
request.accessControlList = "default";

// 소스 객체 소재 경로
request.objectCopySource =
    "examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/exampleobject";

request.setFinish { (result, error) in
    if let result = result {
        // 생성한 새 파일의 etag
        let eTag = result.eTag
    } else {
        print(error!);
    }
}

QCloudCOSXMLService.defaultCOSXML().putObjectCopy(request);
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/ModifyObjectProperty.swift)을 참조하십시오.

#### 예시 코드4: 객체 스토리지 유형 수정
**Objective-C**

[//]: # (.cssg-snippet-modify-object-storage-class)
```objective-c
QCloudPutObjectCopyRequest* request = [[QCloudPutObjectCopyRequest alloc] init];

// 버킷 이름. 형식은 BucketName-APPID
request.bucket = @"examplebucket-1250000000";

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
request.object = @"exampleobject";

// 객체 스토리지 유형, 열거 값은 스토리지 유형 개요 문서를 참조하십시오. 예시: MAZ_STANDARD, MAZ_STANDARD_IA,
// STANDARD_IA, ARCHIVE. 객체가 표준 스토리지가 아닐 경우에만(STANDARD) 해당 헤더가 반환됩니다.
[request.customHeaders setValue:@"ARCHIVE" forKey:@"x-cos-storage-class"];

// 소스 객체 소재 경로
request.objectCopySource =
    @"examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/exampleobject";

// 소스 파일의 versionID 지정, 활성화되거나 활성화 후 일시 정지된 버킷만 해당 매개변수에 영향을 줍니다.
request.versionID = @"";

[request setFinishBlock:^(QCloudCopyObjectResult * _Nonnull result,
                          NSError * _Nonnull error) {
    // result 반환 구체 정보
   
}];
[[QCloudCOSXMLService defaultCOSXML]  PutObjectCopy:request];
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/ModifyObjectProperty.m)을 참조하십시오.

**Swift**

[//]: # (.cssg-snippet-modify-object-storage-class)
```swift
let request : QCloudPutObjectCopyRequest = QCloudPutObjectCopyRequest();

// 버킷 이름. 형식은 BucketName-APPID
request.bucket = "examplebucket-1250000000";

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
request.object = "exampleobject";

// 객체 스토리지 유형, 열거 값은 스토리지 유형 개요 문서를 참조하십시오. 예시: MAZ_STANDARD, MAZ_STANDARD_IA,
// STANDARD_IA, ARCHIVE. 객체가 표준 스토리지가 아닐 경우에만(STANDARD) 해당 헤더가 반환됩니다.
request.customHeaders.setValue("newValue", forKey: "x-cos-storage-class");
// 소스 객체 소재 경로
request.objectCopySource =
    "examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/exampleobject";

request.setFinish { (result, error) in
    if let result = result {
        // 생성한 새 파일의 etag
        let eTag = result.eTag
    } else {
        print(error!);
    }
}

QCloudCOSXMLService.defaultCOSXML().putObjectCopy(request);
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/ModifyObjectProperty.swift)을 참조하십시오.

## 멀티파트 작업

멀티파트 업로드의 프로세스를 설명합니다.

#### 멀티파트 업로드와 복사 프로세스

1. 멀티파트 업로드를 초기화(Initiate Multipart Upload)하고 UploadId를 획득합니다.
2. UploadId를 사용해 파트를 업로드(Upload Part)하거나 파트를 복사(Upload Part Copy)합니다.
3. 멀티파트 업로드 완료(Complete Multipart Upload)

#### 멀티파트 업로드 계속하기와 복사 프로세스

1. UploadId를 기록하지 않은 경우 멀티파트 업로드 작업을 조회(List Multipart Uploads)하여 대응되는 파일의 UploadId를 조회합니다.
2. UploadId를 사용해 업로드 된 파트를 나열합니다(List Parts).
2. UploadId를 사용해 남은 파트를 업로드(Upload Part)하거나 남은 파트를 복사(Upload Part Copy)합니다.
3. 멀티파트 업로드 완료(Complete Multipart Upload)

#### 멀티파트 업로드 중지와 복사 프로세스

1. UploadId를 기록하지 않은 경우 멀티파트 업로드 작업을 조회(List Multipart Uploads)하여 대응되는 파일의 UploadId를 조회합니다.
2. 멀티파트 업로드를 중지하고 업로드 된 부분을 삭제합니다(Abort Multipart Upload).

### 멀티파트 업로드 조회

#### 기능 설명

지정 버킷에서 진행 중인 멀티파트 업로드를 조회(List Multipart Uploads)합니다.

#### 예시 코드
**Objective-C**

[//]: # (.cssg-snippet-list-multi-upload)
```objective-c
QCloudListBucketMultipartUploadsRequest* uploads = [QCloudListBucketMultipartUploadsRequest new];
// 버킷 이름. 형식은 BucketName-APPID
uploads.bucket = @"examplebucket-1250000000";
// 반환되는 최대 multipart 수량 설정, 값 범위는 1부터 1,000까지
uploads.maxUploads = 100;
[uploads setFinishBlock:^(QCloudListMultipartUploadsResult* result,
                          NSError *error) {
    // result에서 파트 정보를 반환할 수 있습니다.
    // 진행 중인 멀티파트 업로드 객체
    NSArray<QCloudListMultipartUploadContent*> *uploads = result.uploads;
}];
[[QCloudCOSXMLService defaultCOSXML] ListBucketMultipartUploads:uploads];
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/MultiPartsUploadObject.m)을 참조하십시오.

**Swift**

[//]: # (.cssg-snippet-list-multi-upload)
```swift
let listParts = QCloudListBucketMultipartUploadsRequest.init();

// 버킷 이름. 형식은 BucketName-APPID
listParts.bucket = "examplebucket-1250000000";

// 반환되는 최대 multipart 수량 설정, 값 범위는 1부터 1,000까지
listParts.maxUploads = 100;

listParts.setFinish { (result, error) in
    if let result = result {
        // 미완료된 모든 멀티파트 업로드 작업
        let uploads = result.uploads;
    } else {
        print(error!);
    }
}
QCloudCOSXMLService.defaultCOSXML().listBucketMultipartUploads(listParts);
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/MultiPartsUploadObject.swift)을 참조하십시오.

### 멀티파트 업로드 초기화

#### 기능 설명

Multipart Upload 업로드 작업을 초기화, 상응하는 uploadId(Initiate Multipart Upload)를 획득합니다.

#### 예시 코드
**Objective-C**

[//]: # (.cssg-snippet-init-multi-upload)
```objective-c
QCloudInitiateMultipartUploadRequest* initRequest = [QCloudInitiateMultipartUploadRequest new];
// 버킷 이름. 형식은 BucketName-APPID
initRequest.bucket = @"examplebucket-1250000000";
// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
initRequest.object = @"exampleobject";
// 객체로 메타데이터 반환
initRequest.cacheControl = @"cacheControl";
initRequest.contentDisposition = @"contentDisposition";
// Object의 ACL 속성 정의. 유효값: private, public-read-write, public-read;기본값: private
initRequest.accessControlList = @"public";
// 읽기 권한을 부여합니다.
initRequest.grantRead = @"grantRead";
// 쓰기 권한을 부여합니다.
initRequest.grantWrite = @"grantWrite";
// 읽고 쓰기 권한을 부여합니다. grantFullControl == grantWrite + grantRead
initRequest.grantFullControl = @"grantFullControl";
[initRequest setFinishBlock:^(QCloudInitiateMultipartUploadResult* outputObject,
                              NSError *error) {
    // 멀티파트 업로드의 uploadId 획득, 후속 업로드 시마다 이 ID를 사용하니 저장하십시오.
    self->uploadId = outputObject.uploadId;
    
}];

[[QCloudCOSXMLService defaultCOSXML] InitiateMultipartUpload:initRequest];
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/MultiPartsUploadObject.m)을 참조하십시오.

**Swift**

[//]: # (.cssg-snippet-init-multi-upload)
```swift
let initRequest = QCloudInitiateMultipartUploadRequest.init();

// 버킷 이름. 형식은 BucketName-APPID
initRequest.bucket = "examplebucket-1250000000";

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
initRequest.object = "exampleobject";

initRequest.setFinish { (result, error) in
    if let result = result {
        // 멀티파트 업로드의 uploadId 획득, 후속 업로드 시마다 이 ID를 사용하니 저장하십시오.
        self.uploadId = result.uploadId;
    } else {
        print(error!);
    }
}
QCloudCOSXMLService.defaultCOSXML().initiateMultipartUpload(initRequest);
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/MultiPartsUploadObject.swift)을 참조하십시오.

### 멀티파트 업로드

객체를 각 부분별로 업로드합니다(Upload Part). 

#### 예시 코드
**Objective-C**

[//]: # (.cssg-snippet-upload-part)
```objective-c
QCloudUploadPartRequest* request = [QCloudUploadPartRequest new];
// 버킷 이름. 형식은 BucketName-APPID
request.bucket = @"examplebucket-1250000000";
// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
request.object = @"exampleobject";
// 파트 번호
request.partNumber = 1;
// 이번 멀티파트 업로드의 ID 표시, Initiate Multipart Upload 인터페이스를 사용해 멀티파트 업로드 초기화 시 uploadId를 획득합니다.
request.uploadId = uploadId;
// 업로드 데이터: NSData*, NSURL(로컬 URL), QCloudFileOffsetBody * 세 가지 유형 지원
request.body = [@"testFileContent" dataUsingEncoding:NSUTF8StringEncoding];
[request setSendProcessBlock:^(int64_t bytesSent,
                               int64_t totalBytesSent,
                               int64_t totalBytesExpectedToSend) {
    // 업로드 진행도 정보
    // bytesSent                   증가 바이트 수
    // totalBytesSent              이번 업로드의 총 바이트 수
    // totalBytesExpectedToSend    로컬 업로드의 타깃 바이트 수
}];
[request setFinishBlock:^(QCloudUploadPartResult* outputObject, NSError *error) {
    QCloudMultipartInfo *part = [QCloudMultipartInfo new];
    // 업로드한 파트의 etag 획득
    part.eTag = outputObject.eTag;
    part.partNumber = @"1";
    // 저장 후 마지막 업로드 완료 시에 사용
    self.parts = @[part];

}];

[[QCloudCOSXMLService defaultCOSXML]  UploadPart:request];
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/MultiPartsUploadObject.m)을 참조하십시오.

**Swift**

[//]: # (.cssg-snippet-upload-part)
```swift
let uploadPart = QCloudUploadPartRequest<AnyObject>.init();

// 버킷 이름. 형식은 BucketName-APPID
uploadPart.bucket = "examplebucket-1250000000";

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
uploadPart.object = "exampleobject";
uploadPart.partNumber = 1;

// 이번 멀티파트 업로드의 ID 표시
if let uploadId = self.uploadId {
    uploadPart.uploadId = uploadId;
}

// 예시 파일 내용
let dataBody:NSData? = "wrwrwrwrwrwwrwrwrwrwrwwwrwrw"
    .data(using: .utf8) as NSData?;

uploadPart.body = dataBody!;
uploadPart.setFinish { (result, error) in
    if let result = result {
        let mutipartInfo = QCloudMultipartInfo.init();
        // 파트의 etag 획득
        mutipartInfo.eTag = result.eTag;
        mutipartInfo.partNumber = "1";
        // 저장 후 마지막 업로드 완료 시에 사용
        self.parts = [mutipartInfo];
    } else {
        print(error!);
    }
}
uploadPart.sendProcessBlock = {(bytesSent,totalBytesSent,
                                totalBytesExpectedToSend) in
    // 업로드 진행도 정보
    // bytesSent                   증가 바이트 수
    // totalBytesSent              이번 업로드의 총 바이트 수
    // totalBytesExpectedToSend    로컬 업로드의 타깃 바이트 수
    
}
QCloudCOSXMLService.defaultCOSXML().uploadPart(uploadPart);
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/MultiPartsUploadObject.swift)을 참조하십시오.

### 멀티파트 복사

#### 기능 설명

다른 객체를 한 파트로 복사(Upload Part-Copy)

#### 예시 코드
**Objective-C**

[//]: # (.cssg-snippet-upload-part-copy)
```objective-c
QCloudUploadPartCopyRequest* request = [[QCloudUploadPartCopyRequest alloc] init];

// 버킷 이름. 형식은 BucketName-APPID
request.bucket = @"examplebucket-1250000000";

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
request.object = @"exampleobject";

// 소스 파일 URL 경로, versionid 하위 리소스로 역대 버전을 지정할 수 있습니다.
request.source = @"sourcebucket-1250000000.cos.ap-guangzhou.myqcloud.com/sourceObject";

// 멀티파트 업로드의 응답 초기화 중 유일한 설명 부호 1개 반환(upload ID)
request.uploadID = uploadId;

// 현재 파트의 시리얼 넘버 표시
request.partNumber = 1;

[request setFinishBlock:^(QCloudCopyObjectResult* result, NSError* error) {
    QCloudMultipartInfo *part = [QCloudMultipartInfo new];
    
    // 복사한 파트의 etag 획득
    part.eTag = result.eTag;
    part.partNumber = @"1";
    // 저장 후 마지막 업로드 완료 시에 사용
    self.parts=@[part];
    
}];

[[QCloudCOSXMLService defaultCOSXML]UploadPartCopy:request];
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/MultiPartsCopyObject.m)을 참조하십시오.

**Swift**

[//]: # (.cssg-snippet-upload-part-copy)
```swift
let req = QCloudUploadPartCopyRequest.init();

// 버킷 이름. 형식은 BucketName-APPID
req.bucket = "examplebucket-1250000000";

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
req.object = "exampleobject";

// 소스 파일 URL 경로, versionid 하위 리소스로 역대 버전을 지정할 수 있습니다.
req.source = "sourcebucket-1250000000.cos.ap-guangzhou.myqcloud.com/sourceObject";
// 멀티파트 업로드의 응답 초기화 중 유일한 설명 부호 1개 반환(upload ID)
if let uploadId = self.uploadId {
    req.uploadID = uploadId;
}

// 현재 파트의 시리얼 넘버 표시
req.partNumber = 1;
req.setFinish { (result, error) in
    if let result = result {
        let mutipartInfo = QCloudMultipartInfo.init();
        // 복사한 파트의 etag 획득
        mutipartInfo.eTag = result.eTag;
        mutipartInfo.partNumber = "1";
        // 저장 후 마지막 복사 완료 시에 사용
        self.parts = [mutipartInfo];
    } else {
        print(error!);
    }
}
QCloudCOSXMLService.defaultCOSXML().uploadPartCopy(req);
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/MultiPartsCopyObject.swift)을 참조하십시오.

### 업로드된 파트 조회

#### 기능 설명

특정 멀티파트 업로드 작업 중 업로드된 파트(List Parts)를 조회합니다.

#### 예시 코드
**Objective-C**

[//]: # (.cssg-snippet-list-parts)
```objective-c
QCloudListMultipartRequest* request = [QCloudListMultipartRequest new];

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
request.object = @"exampleobject";

// 버킷 이름. 형식은 BucketName-APPID
request.bucket = @"examplebucket-1250000000";

// 멀티파트 업로드의 응답 초기화 중 유일한 설명 부호 1개 반환(upload ID)
request.uploadId = uploadId;

[request setFinishBlock:^(QCloudListPartsResult * _Nonnull result,
                          NSError * _Nonnull error) {
    
    // result에서 업로드한 파트의 정보 획득
    // 파트당 정보 표시에 사용
    NSArray<QCloudMultipartUploadPart*> *parts = result.parts;
}];

[[QCloudCOSXMLService defaultCOSXML] ListMultipart:request];
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/MultiPartsUploadObject.m)을 참조하십시오.

**Swift**

[//]: # (.cssg-snippet-list-parts)
```swift
let req = QCloudListMultipartRequest.init();

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
req.object = "exampleobject";

// 버킷 이름. 형식은 BucketName-APPID
req.bucket = "examplebucket-1250000000";

// 멀티파트 업로드의 응답 초기화 중 유일한 설명 부호 1개 반환(upload ID)
if let uploadId = self.uploadId {
    req.uploadId = uploadId;
}
req.setFinish { (result, error) in
    if let result = result {
        // 완료된 모든 멀티 파트
        let parts = result.parts
    } else {
        print(error!);
    }
}

QCloudCOSXMLService.defaultCOSXML().listMultipart(req);
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/MultiPartsUploadObject.swift)을 참조하십시오.

### 멀티파트 업로드 완료

#### 기능 설명

전체 파일의 멀티파트 업로드를 완료합니다(Complete Multipart Upload).

#### 예시 코드
**Objective-C**

[//]: # (.cssg-snippet-complete-multi-upload)
```objective-c
QCloudCompleteMultipartUploadRequest *completeRequst = [QCloudCompleteMultipartUploadRequest new];
// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
completeRequst.object = @"exampleobject";
// 버킷 이름. 형식은 BucketName-APPID
completeRequst.bucket = @"examplebucket-1250000000";
// 이번에 조회할 멀티파트 업로드의 uploadId는 멀티파트 업로드 초기화의 요청 결과 QCloudInitiateMultipartUploadResult에서 획득
completeRequst.uploadId = uploadId;
// 업로드한 파트의 정보
QCloudCompleteMultipartUploadInfo *partInfo = [QCloudCompleteMultipartUploadInfo new];
NSMutableArray * parts = [self.parts mutableCopy];
// 업로드한 파트 정렬 진행
[parts sortUsingComparator:^NSComparisonResult(QCloudMultipartInfo*  _Nonnull obj1,
                                               QCloudMultipartInfo*  _Nonnull obj2) {
    int a = obj1.partNumber.intValue;
    int b = obj2.partNumber.intValue;
    
    if (a < b) {
        return NSOrderedAscending;
    } else {
        return NSOrderedDescending;
    }
}];
partInfo.parts = [parts copy];
completeRequst.parts = partInfo;

[completeRequst setFinishBlock:^(QCloudUploadObjectResult * _Nonnull result,
                                 NSError * _Nonnull error) {
    // result에서 업로드 결과 획득
}];

[[QCloudCOSXMLService defaultCOSXML] CompleteMultipartUpload:completeRequst];
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/MultiPartsUploadObject.m)을 참조하십시오.

**Swift**

[//]: # (.cssg-snippet-complete-multi-upload)
```swift
let  complete = QCloudCompleteMultipartUploadRequest.init();

// 버킷 이름. 형식은 BucketName-APPID
complete.bucket = "examplebucket-1250000000";

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
complete.object = "exampleobject";

// 이번에 조회할 멀티파트 업로드의 uploadId는 멀티파트 업로드 초기화의 요청 결과
// QCloudInitiateMultipartUploadResult에서 획득
complete.uploadId = "exampleUploadId";
if let uploadId = self.uploadId {
    complete.uploadId = uploadId;
}

// 업로드한 파트의 정보
let completeInfo = QCloudCompleteMultipartUploadInfo.init();
if self.parts == nil {
    print("완료할 파트 없음");
    return;
}
if self.parts != nil {
    completeInfo.parts = self.parts ?? [];
}

complete.parts = completeInfo;
complete.setFinish { (result, error) in
    if let result = result {
        // 파일의 eTag
        let eTag = result.eTag
        // 서명 없는 파일 링크
        let location = result.location
    } else {
        print(error!);
    }
}
QCloudCOSXMLService.defaultCOSXML().completeMultipartUpload(complete);
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/MultiPartsUploadObject.swift)을 참조하십시오.

### 멀티파트 업로드 중지

#### 기능 설명

멀티파트 업로드 작업을 중지하고 업로드된 파트는 삭제합니다(Abort Multipart Upload). 

#### 예시 코드
**Objective-C**

[//]: # (.cssg-snippet-abort-multi-upload)
```objective-c
QCloudAbortMultipfartUploadRequest *abortRequest = [QCloudAbortMultipfartUploadRequest new];
// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
abortRequest.object = @"exampleobject";
// 버킷 이름. 형식은 BucketName-APPID
abortRequest.bucket = @"examplebucket-1250000000";
// 이번에 중지할 멀티파트 업로드의 uploadId
// 멀티파트 업로드 초기화의 요청 결과 QCloudInitiateMultipartUploadResult에서 획득
abortRequest.uploadId = @"exampleUploadId";
[abortRequest setFinishBlock:^(id outputObject, NSError *error) {
    // outputObject에서 response의 etag 혹은 사용자 정의 헤더 등 정보 획득 가능
    NSDictionary * result = (NSDictionary *)outputObject;
}];
[[QCloudCOSXMLService defaultCOSXML]AbortMultipfartUpload:abortRequest];
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/AbortMultiPartsUpload.m)을 참조하십시오.

**Swift**

[//]: # (.cssg-snippet-abort-multi-upload)
```swift
let abort = QCloudAbortMultipfartUploadRequest.init();

// 버킷 이름. 형식은 BucketName-APPID
abort.bucket = "examplebucket-1250000000";

// 객체 키, 객체의 COS에서의 전체 경로. 디렉터리가 있을 경우 형식은 "dir1/object1"
abort.object = "exampleobject";

// 이번에 조회할 멀티파트 업로드의 uploadId는 멀티파트 업로드 초기화의 요청 결과
// QCloudInitiateMultipartUploadResult에서 획득
abort.uploadId = self.uploadId!;

abort.finishBlock = {(result,error)in
    if let result = result {
        // result에서 서버 반환의 header 정보를 획득할 수 있습니다.
    } else {
        print(error!)
    }
}
QCloudCOSXMLService.defaultCOSXML().abortMultipfartUpload(abort);
```

>? 전체 예시는 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/AbortMultiPartsUpload.swift)을 참조하십시오.
