本文主要介绍如何快速运行腾讯云 TRTC Demo（Windows）。

## 环境要求
### Windows（C++）开发环境
- Microsoft Visual Studio 2015及以上版本，推荐使用 Microsoft Visual Studio 2015 
- Windows SDK 8.0及以上版本，推荐使用 Windows SDK 8.1

### Windows（C#）开发环境
- Microsoft Visual Studio 2015及以上版本，推荐使用 Microsoft Visual Studio 2017
- .Net Framework 4.0及以上版本，推荐使用 .Net Framework 4.0

### Windows（QT）开发环境
- Microsoft Visual Studio 2015 及以上版本，推荐使⽤ Microsoft Visual Studio 2015。
- 下载并安装 [.vsix](https://download.qt.io/official_releases/vsaddin/) 插件⽂件，官⽹上找对应插件版本安装即可。
- 打开 VS 并在⼯具栏找到 `QT VS Tools -> Qt Options -> Qt Versions`，add 添加我们⾃⼰的 Qt 编译器 msvc。
- 需要将 `SDK/CPlusPlus/Win32/lib` 下的所有的 `.dll` ⽂件拷⻉到⼯程⽬录下的 `debug` / `release` ⽂件夹下。
>! `debug/release` ⽂件夹均是在 VS 上的环境配置完后⾃动⽣成。

## 前提条件
您已 [注册腾讯云](https://intl.cloud.tencent.com/document/product/378/17985) 账号，并完成 [实名认证](https://intl.cloud.tencent.com/document/product/378/3629)，未进行实名认证的用户无法购买中国境内的实时音视频 TRTC 服务。

## 操作步骤
[](id:step1)
### 步骤1：创建新的应用
1. 登录实时音视频控制台，选择【开发辅助】>【[快速跑通Demo](https://console.cloud.tencent.com/trtc/quickstart)】。
2. 输入应用名称，例如 TestTRTC，单击【创建】。

[](id:step2)
### 步骤2：下载 SDK 和 Demo 源码
1. 根据实际业务需求下载 SDK 及配套的 Demo 源码。
2. 下载完成后，单击【已下载，下一步】。

[](id:step3)
### 步骤3：配置 Demo 工程文件
1. 进入修改配置页，根据您下载的源码包，选择相应的开发环境。
2. 找到并打开 `GenerateTestUserSig` 文件：
 <table>
     <tr>
         <th nowrap="nowrap">适用平台</th>  
         <th nowrap="nowrap">文件相对路径</th>  
     </tr>
   <tr>      
         <td>Windows(C++)</td>   
       <td>Windows/DuilibDemo/GenerateTestUserSig.h</td>   
     </tr> 
   <tr>
       <td>Windows(C#)</td>   
       <td>Windows/CSharpDemo/GenerateTestUserSig.cs</td>
     </tr> 
 </table>
3. 设置 `GenerateTestUserSig.js` 文件中的相关参数：
  <ul><li>SDKAPPID：默认为0，请设置为实际的 SDKAppID。</li>
  <li>SECRETKEY：默认为空字符串，请设置为实际的密钥信息。</li></ul> 
4. 粘贴完成后，单击【已复制粘贴，下一步】即创建成功。
5. 编译完成后，单击【回到控制台概览】即可。

>!
>- 本文提到的生成 UserSig 的方案是在客户端代码中配置 SECRETKEY，该方法中 SECRETKEY 很容易被反编译逆向破解，一旦您的密钥泄露，攻击者就可以盗用您的腾讯云流量，因此**该方法仅适合本地跑通 Demo 和功能调试**。
>- 正确的 UserSig 签发方式是将 UserSig 的计算代码集成到您的服务端，并提供面向 App 的接口，在需要 UserSig 时由您的 App 向业务服务器发起请求获取动态 UserSig。更多详情请参见 [服务端生成 UserSig](https://intl.cloud.tencent.com/document/product/647/35166)。

[](id:step4)
### 步骤4：编译运行
- **Windows（C++）：**
使用 Visual Studio（建议 VS2015）打开源码目录下的`DuilibDemo\TRTCDuilibDemo.sln`工程文件，推荐选择 Release/X86 构建平台，编译并运行 Demo 工程即可。
- **Windows（C#）：**
使用 Visual Studio（建议 VS2017）打开源码目录下的`CSharpDemo\TRTCCSharpDemo.sln`工程文件，推荐选择 Release/X86 构建平台，编译并运行 Demo 工程即可。
- **Windows（QT）：** 
使⽤ Visual Studio（建议 VS2015 或以上）打开源码⽬录下的
QTDemo\QTDemo.pro⼯程⽂件，编译并运⾏ QTDemo ⼯程即可。

## 常见问题

### 1. 查看密钥时只能获取公钥和私钥信息，要如何获取密钥？
TRTC SDK 6.6 版本（2019年08月）开始启用新的签名算法 HMAC-SHA256。在此之前已创建的应用，需要先升级签名算法才能获取新的加密密钥。如不升级，您也可以继续使用 [老版本算法 ECDSA-SHA256](https://intl.cloud.tencent.com/document/product/647/35166)，如已升级，您按需切换为新旧算法。

升级/切换操作：
 1. 登录 [实时音视频控制台](https://console.cloud.tencent.com/trtc)。
 2. 在左侧导航栏选择【应用管理】，单击目标应用所在行的【应用信息】。
 3. 选择【快速上手】页签，单击【第二步 获取签发UserSig的密钥】区域的【点此升级】、【非对称式加密】或【HMAC-SHA256】。
  - 升级：
  - 切换回老版本算法 ECDSA-SHA256：
      ![](https://main.qcloudimg.com/raw/96a974bcc25d0fab17db839be12fdb5e/%E8%B7%91%E9%80%9ADemo(Windows)2-%E8%BF%94%E8%BF%98.png)
  - 切换为新版本算法 HMAC-SHA256：
      ![](https://main.qcloudimg.com/raw/c3d53c405bb074ef4758cb12aa01e024/%E8%B7%91%E9%80%9ADemo(Windows)3-%E8%BF%94%E8%BF%98.png)

### 2. 两台设备同时运行 Demo，为什么看不到彼此的画面？
请确保两台设备在运行 Demo 时使用的是不同的 UserID，TRTC 不支持同一个 UserID （除非 SDKAppID 不同）在两个设备同时使用。

### 3. 防火墙有什么限制？
由于 SDK 使用 UDP 协议进行音视频传输，所以对 UDP 有拦截的办公网络下无法使用，如遇到类似问题，请参见 [应对公司防火墙限制](https://intl.cloud.tencent.com/document/product/647/35164)。
