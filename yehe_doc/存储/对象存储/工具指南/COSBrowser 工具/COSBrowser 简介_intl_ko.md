COSBrowser는 Tencent Cloud COS가 출시한 시각화 인터페이스 툴로, COS 리소스 조회, 전송, 관리를 간편하고 인터랙티브하게 실현할 수 있습니다. 현재 COSBrowser는 데스크톱 버전과 모바일 버전 두 가지 버전이 있으며, 자세한 내용은 다음을 참고하십시오.

- [데스크톱 버전 사용 설명](https://intl.cloud.tencent.com/document/product/436/32565)
- [모바일 버전 사용 설명](https://intl.cloud.tencent.com/document/product/436/32566)

## 다운로드 주소

<table>
   <tr>
      <th>COSBrowser 분류</td>
      <th>지원 플랫폼</td>
      <th>시스템 요구사항</td>
      <th>다운로드 주소</td>
   </tr>
   <tr>
      <td rowspan=3>데스크톱</td>
      <td>Windows</td>
      <td>Windows 7 32/64비트 이상, Windows Server 2008 R2 64비트 이상</td>
      <td><a href="https://cos5.cloud.tencent.com/cosbrowser/cosbrowser-setup-latest.exe">Windows</a></td>
   </tr>
   <tr>
      <td>macOS</td>
      <td>macOS 10.13 이상</td>
      <td><a href="https://cos5.cloud.tencent.com/cosbrowser/cosbrowser-latest.dmg">macOS</a></td>
   </tr>
   <tr>
      <td>Linux</td>
      <td>그래픽 인터페이스가 있고 <a href="https://appimage.org">AppImage</a> 포맷을 지원해야 함</td>
          주의사항: CentOS에서 클라이언트 실행 시 단말에서 <code>./cosbrowser.AppImage --no-sandbox</code>를 실행해야 함</td>
      <td><a href="https://cos5.cloud.tencent.com/cosbrowser/cosbrowser-latest-linux.zip">Linux</a></td>
   </tr>
   <tr>
      <td rowspan=2>모바일</td>
      <td>Android</td>
      <td>Android 4.4 이상</td>
      <td><a href="https://cos5.cloud.tencent.com/cosbrowser/cosbrowser-latest.apk">Android</a></td>
   </tr>
   <tr>
      <td>iOS</td>
      <td>iOS 11 이상</td>
      <td><a href="https://apps.apple.com/cn/app/id1469323992">iOS</a></td>
   </tr>
   <tr>
      <td>웹 페이지 버전</td>
      <td>Web</td>
      <td>Chrome/FireFox/Safari/IE10 이상 등의 브라우저</td>
      <td><a href="https://cosbrowser.cloud.tencent.com/web">Web</a></td>
   </tr>
</table>

## 데스크톱 기능 리스트

COSBrowser 데스크톱 버전은 주로 리소스 관리에 중점을 두고 있으며, 사용자는 COSBrowser를 통해 데이터를 일괄 업로드 및 다운로드를 진행할 수 있습니다.

> !COSBrowser 데스크톱 버전은 시스템에 설정된 프록시를 이용해 네트워크 연결을 시도합니다. 프록시가 정상적으로 설정되어 있는지 확인하시고, 인터넷에 연결할 수 없는 프록시 설정은 사용을 중지하시기 바랍니다.
>
> - Windows 사용자는 운영 체제의 “Internet 선택 항목”에서 확인할 수 있습니다.
> - macOS 사용자는 “선호 네트워크 설정”에서 확인할 수 있습니다.
> - Linux 사용자는 시스템 설정>네트워크>네트워크 프록시에서 확인할 수 있습니다.

COSBrowser 데스크톱 버전의 지원 기능

| 기능                                                         | 기능 설명                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [버킷 생성/삭제](https://intl.cloud.tencent.com/document/product/436/32565#createordelete) | 버킷 생성 및 삭제 지원                                         |
| [버킷 상세 정보 조회](https://intl.cloud.tencent.com/document/product/436/32565#viewbucket) | 버킷 기본 정보 조회 지원                                       |
| [통계 데이터 조회](https://intl.cloud.tencent.com/document/product/436/32565#count)           | 버킷의 현재 스토리지 용량 및 객체 총 개수 조회 지원                         |
| [권한 관리](https://intl.cloud.tencent.com/document/product/436/32565#viewbucket) | 버킷 및 객체 관련 권한 수정 지원                               |
| [버전 제어 설정](https://intl.cloud.tencent.com/document/product/436/32565#viewbucket) | 버킷 버전 제어 활성화 및 일시 중지 지원                                 |
| [액세스 경로 추가](https://intl.cloud.tencent.com/document/product/436/32565#addaccess) | 액세스 경로 추가 지원                                             |
| [파일/폴더 업로드](https://intl.cloud.tencent.com/document/product/436/32565#upload) | 파일 또는 폴더를 버킷에 단일 업로드, 일괄 업로드, 증분 업로드 지원        |
| [파일/폴더 다운로드](https://intl.cloud.tencent.com/document/product/436/32565#download) | 파일 또는 폴더를 로컬에 단일 다운로드, 일괄 다운로드, 증분 다운로드 지원           |
| [파일/폴더 삭제](https://intl.cloud.tencent.com/document/product/436/32565#delete) | 버킷에 있는 파일 또는 폴더 단일 삭제 및 일괄 삭제 지원                 |
| [파일 동기화](https://intl.cloud.tencent.com/document/product/436/32565#synchronization) | 로컬 파일을 버킷에 실시간 동기화 지원                             |
| [파일 복사 및 붙여넣기](https://intl.cloud.tencent.com/document/product/436/32565#copy) | 한 디렉터리에 있는 파일 또는 폴더를 다른 디렉터리로 단일 복사 및 일괄 복사 지원   |
| [파일 이름 변경](https://intl.cloud.tencent.com/document/product/436/32565#rename) | 버킷에 있는 파일 이름 변경 지원                                     |
| [새 폴더 생성](https://intl.cloud.tencent.com/document/product/436/32565#newfolder) | 버킷에 새 폴더 생성 지원                                     |
| [파일 상세 정보 조회](https://intl.cloud.tencent.com/document/product/436/32565#view) | 버킷에 있는 파일 기본 정보 조회 지원                               |
| [파일 링크 생성](https://intl.cloud.tencent.com/document/product/436/32565#generatelinks) | 임시 서명 요청 방식을 통해 유효 시간이 있는 파일 액세스 링크 생성 지원         |
| [파일 미리보기](https://intl.cloud.tencent.com/document/product/436/32565#preview) | 버킷에 있는 미디어 파일(이미지, 비디오, 오디오) 미리보기 지원               |
| [파일 검색](https://intl.cloud.tencent.com/document/product/436/32565#searchfile) | 버킷에 있는 파일을 접두사 검색 방식으로 검색 지원                 |
| [버킷 검색](https://intl.cloud.tencent.com/document/product/436/32565#searchbuckete) | 생성한 버킷 검색 지원                                       |
| [이전 버전/파일 조각 조회](https://intl.cloud.tencent.com/document/product/436/32565#viewfiles) | <li>버전 제어가 활성화된 버킷에서 파일의 이전 버전 조회 지원<br><li>버킷에 있는 파일 조각 상세 정보 조회 지원           |
| [네트워크 프록시 설정](https://intl.cloud.tencent.com/document/product/436/32565#sets) | 네트워크 프록시를 설정하여 COS에 액세스 지원                                   |
| [동시 접속 전송 수 설정](https://intl.cloud.tencent.com/document/product/436/32565#sets) | 파일의 업로드 및 다운로드에 대한 동시 접속 전송 수 설정 지원                           |
| [멀티파트 전송 블록 수 설정](https://intl.cloud.tencent.com/document/product/436/32565#sets) | 파일 멀티파트 업로드 및 다운로드의 블록 수 설정 지원                           |
| [전송 실패 재시도 횟수 설정](https://intl.cloud.tencent.com/document/product/436/32565#sets) | 파일 업로드 및 다운로드 실패 시 재시도 횟수 설정 지원                       |
| [업로드 2차 인증 설정](https://intl.cloud.tencent.com/document/product/436/32565#sets) | 버킷에 업로드하는 파일에 대한 2차 인증 지원                       |
| [업로드 시 md5 계산 설정](https://intl.cloud.tencent.com/document/product/436/32565#sets) | 버킷에 업로드하는 파일에 대해 md5를 계산하여 사용자 정의 Headers에 추가 지원 |
| [로컬 로그 조회](https://intl.cloud.tencent.com/document/product/436/32565#sets) | 사용자의 COSBrowser에 대한 작업 기록을 로컬 로그 형식으로 저장 지원      |

## 모바일 기능 리스트

COSBrowser 모바일 버전은 주로 리소스 조회 및 모니터링에 중점을 두고 있으며, 사용자는 언제 어디서나 COS의 스토리지 용량 및 트래픽 등의 데이터를 모니터링할 수 있습니다.

COSBrowser 모바일 버전의 지원 기능

| 작업명                                                       | 작업 설명                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [빠른 로그인](https://intl.cloud.tencent.com/document/product/436/32566#dulu) | WeChat 빠른 로그인 지원                                             |
| [데이터 개요](https://intl.cloud.tencent.com/document/product/436/32566#dateview) | 최근 데이터 사용 현황 표시 지원                                   |
| [파일 일괄 작업](https://intl.cloud.tencent.com/document/product/436/32566#filebatch) | 버킷에 있는 파일 일괄 업로드, 다운로드, 삭제, 복사, 이동 지원           |
| [업로드 공유](https://intl.cloud.tencent.com/document/product/436/32566#shareupload) | 3rd party 소프트웨어에 있는 파일을 버킷으로 업로드 공유 지원                       |
| [파일 이름 변경](https://intl.cloud.tencent.com/document/product/436/32566#rename) | 버킷에 있는 파일 이름 변경 지원                                     |
| [새 폴더 생성](https://intl.cloud.tencent.com/document/product/436/32566#newfolder) | 버킷에 새 폴더 생성 지원                                     |
| [파일 상세 정보 조회](https://intl.cloud.tencent.com/document/product/436/32566#view) | 버킷에 있는 파일 기본 정보 조회 지원                               |
| [파일 미리보기](https://intl.cloud.tencent.com/document/product/436/32566#filepreview) | 버킷에 있는 미디어 파일(이미지, 비디오, 오디오) 미리보기 지원               |
| [파일 링크 생성](https://intl.cloud.tencent.com/document/product/436/32566#generatelinks) | 임시 서명 요청 방식을 통해 유효 시간이 있는 파일 액세스 링크 생성 지원       |
| [파일 검색](https://intl.cloud.tencent.com/document/product/436/32566#searchfile) | 버킷에 있는 파일을 접두사 검색 방식으로 검색 지원                 |
| [버킷 검색](https://intl.cloud.tencent.com/document/product/436/32566#searchbuckete) | 생성한 버킷 검색 지원                                       |
| [버킷 상세 정보 조회](https://intl.cloud.tencent.com/document/product/436/32566#viewbucket) | 버킷 기본 정보 및 도메인 정보 조회 지원                             |
| [버킷 생성](https://intl.cloud.tencent.com/document/product/436/32566#createbucket) | 새 버킷 생성 지원                                           |
| [액세스 경로 추가](https://intl.cloud.tencent.com/document/product/436/32566#addaccess) | 버킷 리스트에 대한 액세스 권한이 없는 서브 계정에 액세스 경로를 이용하는 방식으로 버킷에 접속해 리소스를 관리할 수 있도록 지원 |
| [리소스 패키지 상세 정보 조회](https://intl.cloud.tencent.com/document/product/436/32566#package)          | 현재 리소스 패키지의 사용 현황 조회 지원                          |

## 로그 업데이트

- 데스크톱 로그 업데이트: [changelog](https://github.com/tencentyun/cosbrowser/blob/master/changelog.md)
- 모바일 로그 업데이트: [changelog_mobile](https://github.com/tencentyun/cosbrowser/blob/master/changelog_mobile.md)

## 피드백 및 의견

COSBrowser 사용 중 문의사항 또는 의견이 있는 경우 피드백을 보내주십시오.

- 데스크톱 피드백: [issues](https://github.com/tencentyun/cosbrowser/issues)
- 모바일 피드백: [issues_mobile](https://support.qq.com/embed/phone/67467)
