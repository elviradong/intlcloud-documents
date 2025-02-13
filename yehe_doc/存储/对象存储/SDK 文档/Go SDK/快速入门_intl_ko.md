## 다운로드 및 설치

#### 관련 리소스
- COS의 XML Go SDK 소스 코드 다운로드 주소: [XML Go SDK](https://github.com/tencentyun/cos-go-sdk-v5)
- 예시 Demo 다운로드 주소: [COS XML Go SDK 예시](https://github.com/tencentyun/cos-go-sdk-v5/tree/master/example)
- 자세한 내용은 [COS Go SDK API](https://godoc.org/github.com/tencentyun/cos-go-sdk-v5) 문서를 참조하십시오.
- SDK 문서의 모든 예시 코드는 [SDK 코드 예시](https://github.com/tencentyun/cos-snippets/tree/master/Go)를 참조하십시오.
- SDK 로그 업데이트는 [ChangeLog](https://github.com/tencentyun/cos-go-sdk-v5/blob/master/CHANGELOG.md)를 참조하십시오.

#### 환경 종속
- Golang: Go 컴파일 실행 환경을 다운로드 및 설치하는 데 사용합니다. Golang 공식 홈페이지에서 다운로드하십시오.

#### SDK 설치
다음 명령어를 실행하여 COS Go SDK를 설치합니다.
```sh
go get -u github.com/tencentyun/cos-go-sdk-v5
```

## 사용하기
COS Go SDK를 사용한 클라이언트 초기화, 버킷 생성, 버킷 리스트 조회, 객체 업로드, 객체 리스트 조회, 객체 다운로드, 객체 삭제 등의 기본 작업 방법은 아래와 같습니다.

### 초기화
COS 도메인을 사용해 COS GO 클라이언트 구성을 생성합니다.

#### 방법 모델
```Go
func NewClient(uri *BaseURL, httpClient *http.Client) *Client
```

#### 요청 예시
영구 키 사용:

[//]: # (.cssg-snippet-global-init)
```go
// examplebucket-1250000000과 COS_REGION을 실제 정보로 수정합니다.
u, _ := url.Parse("https://examplebucket-1250000000.cos.COS_REGION.myqcloud.com")
// Get Service 쿼리에 사용, 모든 리전 기본값: service.cos.myqcloud.com
su, _ := url.Parse(https://cos.COS_REGION.myqcloud.com")
b := &cos.BaseURL{BucketURL: u, ServiceURL: su}
// 1. 영구 키
client := cos.NewClient(b, &http.Client{
    Transport: &cos.AuthorizationTransport{
        SecretID:  "COS_SECRETID",
        SecretKey: "COS_SECRETKEY",
    },
})
```

임시 키 사용:

[//]: # (.cssg-snippet-global-init-sts)
```go
// examplebucket-1250000000과 COS_REGION을 실제 정보로 수정합니다.
u, _ := url.Parse("https://examplebucket-1250000000.cos.COS_REGION.myqcloud.com")
b := &cos.BaseURL{BucketURL: u}
// 2. 임시 키
client := cos.NewClient(b, &http.Client{
    Transport: &cos.AuthorizationTransport{
        SecretID:     "COS_SECRETID",
        SecretKey:    "COS_SECRETKEY",
        SessionToken: "COS_SECRETTOKEN",
    },
})
if client != nil {
    // COS 요청 호출
}
```

>?임시 키 생성 및 사용에 대한 자세한 내용은 [임시 키 생성 및 사용 가이드](https://intl.cloud.tencent.com/document/product/436/14048)를 참조하십시오.

### 버킷 생성

```Go
package main

import (
    "context"
    "net/http"
    "net/url"
    "os"

    "github.com/tencentyun/cos-go-sdk-v5"
)

func main() {
    // examplebucket-1250000000과 COS_REGION을 실제 정보로 수정합니다.
    u, _ := url.Parse("https://examplebucket-1250000000.cos.COS_REGION.myqcloud.com")
    b := &cos.BaseURL{BucketURL: u}
    c := cos.NewClient(b, &http.Client{
        Transport: &cos.AuthorizationTransport{
            SecretID:  "COS_SECRETID",
            SecretKey: "COS_SECRETKEY",
        },
    })

    _, err := c.Bucket.Put(context.Background(), nil)
    if err != nil {
        panic(err)
    }
}
```


### 버킷 리스트 조회
```go
package main

import (
    "context"
    "fmt"
    "net/http"
    "os"

    "github.com/tencentyun/cos-go-sdk-v5"
)

func main() {
    c := cos.NewClient(nil, &http.Client{
        Transport: &cos.AuthorizationTransport{
            SecretID:  "COS_SECRETID",
            SecretKey: "COS_SECRETKEY",
        },
    })

    s, _, err := c.Service.Get(context.Background())
    if err != nil {
        panic(err)
    }

    for _, b := range s.Buckets {
        fmt.Printf("%#v\n", b)
    }
}
```

### 객체 업로드
```Go
package main

import (
    "context"
    "net/http"
    "net/url"
    "os"
    "strings"

    "github.com/tencentyun/cos-go-sdk-v5"
)

func main() {
    // examplebucket-1250000000과 COS_REGION을 실제 정보로 수정합니다.
    u, _ := url.Parse("https://examplebucket-1250000000.cos.COS_REGION.myqcloud.com")
    b := &cos.BaseURL{BucketURL: u}
    c := cos.NewClient(b, &http.Client{
        Transport: &cos.AuthorizationTransport{
            SecretID:  "COS_SECRETID",
            SecretKey: "COS_SECRETKEY",
        },
    })
    // 객체 키(Key)는 버킷 내 객체의 고유 식별자입니다.
    // 예를 들어, 객체의 액세스 도메인 `examplebucket-1250000000.cos.COS_REGION.myqcloud.com/test/objectPut.go`에서 객체 키는 test/objectPut.go입니다.
    name := "test/objectPut.go"
    // 1. 문자열로 객체 업로드
    f := strings.NewReader("test")

    _, err := c.Object.Put(context.Background(), name, f, nil)
    if err != nil {
        panic(err)
    }
    // 2. 로컬 파일로 객체 업로드
    _, err = c.Object.PutFromFile(context.Background(), name, "../test", nil)
    if err != nil {
        panic(err)
    }
    // 3. 파일 스트림으로 객체 업로드
    fd, err := os.Open("./test")
    if err != nil {
        panic(err)
    }
    defer fd.Close()
    _, err = c.Object.Put(context.Background(), name, fd, nil)
    if err != nil {
        panic(err)
    }
}
```

### 객체 리스트 조회
```go
package main

import (
    "context"
    "fmt"
    "net/http"
    "net/url"
    "os"

    "github.com/tencentyun/cos-go-sdk-v5"
)

func main() {
    // examplebucket-1250000000과 COS_REGION을 실제 정보로 수정합니다.
    u, _ := url.Parse("https://examplebucket-1250000000.cos.COS_REGION.myqcloud.com")
    b := &cos.BaseURL{BucketURL: u}
    c := cos.NewClient(b, &http.Client{
        Transport: &cos.AuthorizationTransport{
            SecretID:  "COS_SECRETID",
            SecretKey: "COS_SECRETKEY",
        },
    })

    opt := &cos.BucketGetOptions{
        Prefix:  "test",
        MaxKeys: 3,
    }
    v, _, err := c.Bucket.Get(context.Background(), opt)
    if err != nil {
        panic(err)
    }

    for _, c := range v.Contents {
        fmt.Printf("%s, %d\n", c.Key, c.Size)
    }
}
```

### 객체 다운로드
```Go
package main

import (
    "context"
    "fmt"
    "io/ioutil"
    "net/http"
    "net/url"
    "os"

    "github.com/tencentyun/cos-go-sdk-v5"
)

func main() {
    // examplebucket-1250000000과 COS_REGION을 실제 정보로 수정합니다.
    u, _ := url.Parse("https://examplebucket-1250000000.cos.COS_REGION.myqcloud.com")
    b := &cos.BaseURL{BucketURL: u}
    c := cos.NewClient(b, &http.Client{
        Transport: &cos.AuthorizationTransport{
            SecretID:  "COS_SECRETID",
            SecretKey: "COS_SECRETKEY",
        },
    })
    // 1. 응답 본문으로 객체 가져오기
    name := "test/objectPut.go"
    resp, err := c.Object.Get(context.Background(), name, nil)
    if err != nil {
        panic(err)
    }
    bs, _ := ioutil.ReadAll(resp.Body)
    resp.Body.Close()
    fmt.Printf("%s\n", string(bs))
    // 2. 로컬 파일에 객체 가져오기
    _, err = c.Object.GetToFile(context.Background(), name, "exampleobject", nil)
    if err != nil {
        panic(err)
    }
}
```

### 객체 삭제
```go
package main

import (
    "context"
    "net/http"
    "net/url"
    "os"

    "github.com/tencentyun/cos-go-sdk-v5"
)

func main() {
    // examplebucket-1250000000과 COS_REGION을 실제 정보로 수정합니다.
    u, _ := url.Parse("https://examplebucket-1250000000.cos.COS_REGION.myqcloud.com")
    b := &cos.BaseURL{BucketURL: u}
    c := cos.NewClient(b, &http.Client{
        Transport: &cos.AuthorizationTransport{
            SecretID:  "COS_SECRETID",
            SecretKey: "COS_SECRETKEY",
        },
    })
    name := "test/objectPut.go"
    _, err := c.Object.Delete(context.Background(), name)
    if err != nil {
        panic(err)
    }
}
```
