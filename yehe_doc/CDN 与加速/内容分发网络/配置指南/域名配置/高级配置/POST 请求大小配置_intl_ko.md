
## 기능 설명
Tencent Cloud CDN의 POST 요청 크기 제한, 즉 요청 body의 최대 크기 기본값은 32MB입니다. 실제 비즈니스 상황에 따라 이 부분의 상한값을 조정할 수 있습니다.

>! 해당 기능은 현재 HTTPS Origin-pull을 지원하지 않습니다. 도메인의 Origin-pull 프로토콜이 HTTP인지 확인하십시오.

## 설정 가이드

### 설정 조회

[CDN 콘솔](https://console.cloud.tencent.com/cdn)에 로그인한 후, 왼쪽 메뉴바에서 [도메인 관리]를 선택한 뒤 도메인 작업 열의 [관리]를 클릭하여 도메인 설정 페이지로 들어갑니다. Tab을 [고급 설정]으로 전환하면 [POST 요청 크기 설정]을 찾을 수 있으며, 최대 **200MB**까지 조정 가능합니다.
![](https://main.qcloudimg.com/raw/e3a25b8ba81fe251e76fbf0deb22e966.png)

>!일부 플랫폼에서는 POST 요청 크기 제한이 없으나 도메인에서는 현재 이 기능을 지원하지 않습니다.

