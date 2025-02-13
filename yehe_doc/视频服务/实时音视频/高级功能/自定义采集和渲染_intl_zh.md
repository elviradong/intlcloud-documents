## 内容介绍
- **自定义视频采集**
如果您自研（或者购买第三方）美颜和特效处理模块，则需要自己采集和处理摄像头拍摄画面，您可以通过 TRTCCloud 的 `enableCustomVideoCapture` 接口关闭 TRTC SDK 自己的摄像头采集和图像处理逻辑。然后您可以使用 `sendCustomVideoData` 接口向 TRTC SDK 填充您自己的视频数据。
- **自定义视频渲染**
TRTC SDK 使用 OpenGL 进行视频画面的渲染，如果您是用在游戏开发中，或者需要在自己的界面引擎中嵌入 TRTC SDK，那么就要自己渲染视频画面。
- **自定义音频采集**
如果您是在特殊硬件设备上使用 TRTC SDK，当需要外接声音采集设备并自己采集声音数据时，您可以通过 TRTCCloud 的 `enableCustomAudioCapture` 接口关闭 TRTC SDK 默认的声音采集流程。然后您可以使用 `sendCustomAudioData` 接口向 TRTC SDK 填充您自己的声音数据。
>!开启自定义音频采集后，有可能会导致回声抵消（AEC）的功能失效。
- **获取音频原数据**
声音模块是一个高复杂度的模块，SDK 需要严格控制声音设备的采集和播放逻辑。在某些场景下，当您需要获取远程用户的音频数据或者需要获取本地麦克风采集到的音频数据时，可以通过 TRTC SDK 提供的相应的回调接口来实现。

## 支持的平台

| iOS | Android | Mac OS | Windows | Chrome 浏览器|
|:-------:|:-------:|:-------:|:-------:|:-------:|
|   &#10003;  |  &#10003;    | &#10003;   |  &#10003;  |  ×   |

## 自定义视频采集

您可以通过 TRTCCloud 的 `enableCustomVideoCapture` 接口关闭 TRTC SDK 自己的摄像头采集和图像处理逻辑。然后您可以使用 `sendCustomVideoData` 接口向 TRTC SDK 填充您自己的视频数据。

在 `sendCustomVideoData` 接口中有一个叫 `TRTCVideoFrame` 的参数，它表示一帧视频画面。为了避免不必要的性能损失，对于输入 TRTC SDK 的视频数据，在不同平台上有不同的格式要求，具体要求如下：

### iOS 平台

TRTC SDK 支持 NV12 和 i420 两种 iOS 版本的 YUV 数据格式。在 iOS 平台上，比较高性能的图像传递方式是 CVPixelBufferRef，因此我们建议参数格式如下：

| 参数名称  | 参数类型 |  推荐取值 | 备注说明|
|:-------:|:-------:|:-------:| :-------: |
| pixelFormat| TRTCVideoPixelFormat | TRTCVideoPixelFormat_NV12 |  iOS 平台上摄像头原生采集出的视频格式即是 NV12。 |
| bufferType | TRTCVideoBufferType| PixelBuffer | iOS 中原生支持的视频帧格式，性能最佳。|
| pixelBuffer| CVPixelBufferRef | 如果 TRTCVideoBufferType 是 PixelBuffer，则需填写。 | iPhone 摄像头采集的数据是 NV12 格式的 PixelBuffer。|
| data| NSData\* | 如果 TRTCVideoBufferType 是 NSData，则需填写。 | 性能不如 PixelBuffer。 |
| timestamp| uint64_t | 0 | 可以填0，这样 SDK 会自定填充 timestamp 字段，但请**均匀**地控制 sendCustomVideoData 的调用间隔。|
| width    | uint64_t| 视频画面的宽度 | 请严格填写传入画面的像素宽度。 |
| height   | uint32_t| 视频画面的高度 | 请严格填写传入画面的像素高度。 |
| rotation | TRTCVideoRotation|不填写 |  <li/>默认不填写。 <li/>如果需要对画面进行旋转，可以填写`TRTCVideoRotation_0`、`TRTCVideoRotation_90`、`TRTCVideoRotation_180`、`TRTCVideoRotation_270`。SDK  会根据这个值将视频顺时针旋转对应角度。例如一个竖屏画面，传入`TRTCVideoRotation_90`后，SDK 会旋转成横屏显示。 |

**示例代码**：在 [Demo]文件夹中有一个叫做 `TestSendCustomVideoData.m` 的文件，它展示了如何从一个本地视频文件中读取出 NV12 格式的 PixelBuffer，并通过 SDK 进行后续处理。

```objectiveC
//组装一个 TRTCVideoFrame 并将其送给 trtcCloud 对象
TRTCVideoFrame* videoFrame = [TRTCVideoFrame new];
videoFrame.bufferType = TRTCVideoBufferType_PixelBuffer;
videoFrame.pixelFormat = TRTCVideoPixelFormat_NV12;
videoFrame.pixelBuffer = CMSampleBufferGetImageBuffer(sampleBuffer);
        
[trtcCloud sendCustomVideoData:videoFrame];      
```

### Android 平台

Android 平台有两种方案：

**1. buffer 方案**：对接起来比较简单，但是性能较差，不适合分辨率较高的场景。

buffer 方案要求直接向 TRTC SDK 塞入 byte[] 格式的数组，支持 i420 和 NV21 两种 YUV 格式。

| 参数名称  | 参数类型 |  推荐取值 | 备注说明|
|:-------:|:-------:|:-------:| :-------: |
| pixelFormat| int | TRTC_VIDEO_PIXEL_FORMAT_I420 或<br> TRTC_VIDEO_PIXEL_FORMAT_NV21  | i420 和 NV21 是两种不同的 YUV 数据格式。 |
| bufferType | int | TRTC_VIDEO_BUFFER_TYPE_BYTE_ARRAY | 指明使用 data 的方式传递 YUV 数据。|
| texture  | TRTCTexture | buffer 方案不填写| 如果选择 i420 或者 NV21 两种格式，则此参数不填写。|
| data| byte[] | YUV 格式的数据 buffer | 内部包装的是 Java 类型的内存，适合在 Java 层使用。|
| buffer| ByteBuffer | buffer 方案不填写 | 内部包装的是 C/C++ 类型的内存，适合在 JNI 层使用。 |
| width    | uint64_t| 视频画面的宽度 | 请严格填写传入画面的像素宽度。 |
| height   | uint32_t| 视频画面的高度 | 请严格填写传入画面的像素高度。 |
| timestamp| long | 视频帧的采集时间 | 可以填0，这样 SDK 会自定填充 timestamp 字段，但请**均匀**地控制 sendCustomVideoData 的调用间隔。|
| rotation | int| 不填写 | <li/>默认不填写。 <li/>如果需要对画面进行旋转，可以填写0、90、180、270。SDK  会根据这个值将视频顺时针旋转对应角度。例如一个竖屏画面，传入90后，SDK 会旋转成横屏显示。 |

**2. texture 方案**：对接需要一定的 OpenGL 基础，但是性能较好，尤其是画面分辨率较高的时候。

texture 需要向 TRTC SDK 中传递 OpenGL 纹理，为了保证该方案能够正常运作，需要您提前设置好 OpenGL 环境，因此该方案的对接难度非常高。如果您没有学习过 OpenGL，建议直接使用我们提供的示例代码，它在 [Demo](https://github.com/tencentyun/TRTCSDK/tree/master/Android/TRTC-API-Example/Advanced/LocalVideoShare/src/main/java/com/tencent/trtc/mediashare) 中的 `customCapture` 文件夹下，包含以下文件：

| 文件名 | 源码逻辑 |
|---------|---------|
|TestSendCustomData.java   |     演示如何通过 TRTCloud 的 sendCustomVideoData 函数向 SDK 投喂视频纹理。|
|TestRenderVideoFrame.java  |      演示如何跳开 TRTCloud 原本的渲染逻辑，自己用 OpenGL 渲染画面。|
|VideoFrameReader.java    |    从本地的一个视频文件中，一帧帧地读取出画面。|
|decoder 文件夹|  解码模块|
|opengl 文件夹   |     封装了 OpenGL 的基础函数，使其更好用一些。|
|render 文件夹    |    封装了 Egl 的基础函数，使其更好用一些。|

>! 对于 640 x 360 以上的分辨率，我们建议使用 texture 方案，以避免过高的 CPU 使用率。


**示例代码**：`TestSendCustomVideoData.java` 中的代码原理比较复杂：
1. 代码首先先启动一个 GLThread 线程，这个线程大部分时间都处于等待状态，直到它关联的“画板（SurfaceTexture）”被绘制了内容。
2. 代码接下来使用了一个叫 MovieVideoFrameReader 的模块，用于从一个本地视频文件中读取出一帧帧的视频画面，并将一帧帧的画面绘制到上一步创建的“画板”上。
3. 每绘制一帧，我们在第一步中创建的 GLThread 线程就被唤醒一次，于是就有了下面的 `onTextureProcess` 回调，在这个回调函数里，我们将获得的 texture 通过 sendCustomVideoData 函数传递给 SDK：

```java
public int onTextureProcess(int textureId, EGLContext eglContext) {
        if (!mIsSending) return textureId;

        //将视频帧通过纹理方式塞给SDK
        TRTCCloudDef.TRTCVideoFrame videoFrame = new TRTCCloudDef.TRTCVideoFrame();
        videoFrame.texture = new TRTCCloudDef.TRTCTexture();
        videoFrame.texture.textureId = textureId;
        videoFrame.texture.eglContext14 = eglContext;
        videoFrame.width = mPlayThread.getVideoWidth();
        videoFrame.height = mPlayThread.getVideoHeight();
        videoFrame.pixelFormat = TRTCCloudDef.TRTC_VIDEO_PIXEL_FORMAT_Texture_2D;
        videoFrame.bufferType = TRTCCloudDef.TRTC_VIDEO_BUFFER_TYPE_TEXTURE;
        mTRTCCloud.sendCustomVideoData(videoFrame);
        return textureId;
    }
```


## 自定义视频渲染

TRTC SDK 使用 OpenGL 进行视频画面的渲染，如果您是用在游戏开发中，或者需要在自己的界面引擎中嵌入 TRTC SDK，那么就要自己渲染视频画面。

### iOS 平台（包括 Mac）

通过 TRTCCloud 的 `setLocalVideoRenderDelegate` 和 `setRemoteVideoRenderDelegate` 可以设置本地和远程画面的自定义渲染回调，相关参数如下：

| 参数名称  | 参数类型 |  推荐取值 | 备注说明|
|:-------:|:-------:|:-------:| :-------: |
| pixelFormat| TRTCVideoPixelFormat | TRTCVideoPixelFormat_NV12 |  - |
| bufferType | TRTCVideoBufferType| TRTCVideoBufferType_PixelBuffer | iOS 中原生支持的视频帧格式，性能最佳。|

[](id:example_ios)
#### 示例代码
如果 pixelFormat 选择了  TRTCVideoPixelFormat_NV12，bufferType 选择了 TRTCVideoBufferType_PixelBuffer，那么整个您可以很方便地将一帧 NV12 格式的 PixelBuffer 转成一副视频画面。在 Demo 文件夹中有一个叫做 `TestRenderVideoFrame.m` 的文件，其中就用如下的示例代码展示了如何使用该方法。

```objectiveC
- (void)onRenderVideoFrame:(TRTCVideoFrame *)frame 
                                userId:(NSString *)userId 
						        streamType:(TRTCVideoStreamType)streamType
{
    //userId是nil时为本地画面，否则为远端画面
    CFRetain(frame.pixelBuffer);
    __weak __typeof(self) weakSelf = self;
    dispatch_async(dispatch_get_main_queue(), ^{
        TestRenderVideoFrame* strongSelf = weakSelf;
        UIImageView* videoView = nil;
        if (userId) {
            videoView = [strongSelf.userVideoViews objectForKey:userId];
        }
        else {
            videoView = strongSelf.localVideoView;
        }
        videoView.image = [UIImage imageWithCIImage:[CIImage imageWithCVImageBuffer:frame.pixelBuffer]];
        videoView.contentMode = UIViewContentModeScaleAspectFit;
        CFRelease(frame.pixelBuffer);
    });
}
```

### Android 平台
通过 TRTCCloud 的 `setLocalVideoRenderListener` 和 `setRemoteVideoRenderListener` 可以设置本地和远程画面的自定义渲染回调，相关参数如下：

| 参数名称  | 参数类型 |  推荐取值 | 备注说明|
|:-------:|:-------:|:-------:| :-------: |
| pixelFormat| TRTCVideoPixelFormat | TRTC_VIDEO_PIXEL_FORMAT_NV21 | 自定义渲染暂时不支持 texture 方案。  |
| bufferType | TRTCVideoBufferType| TRTC_VIDEO_BUFFER_TYPE_BYTE_ARRAY 或<br> TRTC_VIDEO_BUFFER_TYPE_BYTE_BUFFER  | BYTE_BUFFER 适合在 jni 层使用，BYTE_ARRAY 则可用于 Java 层的直接操作。|

[](id:example_android)
#### 示例代码
```
    public void onRenderVideoFrame(String userId, int streamType, final TRTCCloudDef.TRTCVideoFrame frame) {
        if (!userId.equals(mUserId) || mSteamType != streamType) {
            // 如果渲染回调的id或者steamtype对不上
            return;
        }
        if (frame.texture != null) {
            // 等待frame.texture的纹理绘制完成
            GLES20.glFinish();
        }
        Size mSurfaceSize; //这里设置Surface的宽高
        GpuImageI420Filter mYUVFilter;
        GLES20.glViewport(0, 0, mSurfaceSize.width, mSurfaceSize.height);
        GLES20.glBindFramebuffer(GLES20.GL_FRAMEBUFFER, 0);
        GLES20.glClearColor(0, 0, 0, 1.0f);
        GLES20.glClear(GLES20.GL_DEPTH_BUFFER_BIT | GLES20.GL_COLOR_BUFFER_BIT);
        if (frame.data != null) {
            mYUVFilter.loadYuvDataToTexture(frame.data, frame.width, frame.height);
        } else {
            mYUVFilter.loadYuvDataToTexture(frame.buffer, frame.width, frame.height);
        }         
        mYUVFilter.onDraw(NO_TEXTURE, mGLCubeBuffer, mGLTextureBuffer);
        
    }
```

## 自定义音频采集

您可以通过 TRTCCloud 的 enableCustomAudioCapture 接口关闭 TRTC SDK 默认的声音采集流程。然后您可以使用 sendCustomAudioData 接口向 TRTC SDK 填充您自己的声音数据。

在 `sendCustomAudioData` 接口中有一个叫 `TRTCAudioFrame` 的参数，它表示一帧 20ms 长度的音频数据：
- 通过 `sendCustomAudioData`  向 SDK 投送的数据必须是 PCM 格式的未经压缩的音频裸数据，不可以是 AAC 或者其他的压缩格式。
- sampleRate 和 channels 分辨代表声音采样率和声道数，请保持跟传入的 PCM 数据严格一致。
- 每帧音频数据的时长推荐是20ms，我们可以计算一下，如果 sampleRate 为48000，声道数是1（单声道），那么每次调用 sendCustomAudioData 时传入的 buffer 长度应该是：48000 * 0.02s * 1 * 16bit = 15360bit = 1920字节。
- timestamp 可以为0，当 timestamp 为0时，SDK 会自动填充音频时间戳。因此，为了确保音频时间戳的稳定，请**均匀**地调用 `sendCustomAudioData`，保持20ms一次的频率，否则会导致声音出现断断续续的效果。

>! 使用 `sendCustomAudioData` 有可能会导致回声抵消（AEC）的功能失效。

## 获取音频原数据

声音模块是一个高复杂度的模块，SDK 需要严格控制声音设备的采集和播放逻辑。在某些场景下，当您需要获取远程用户的音频数据或者需要获取本地麦克风采集到的音频数据时，可以通过 TRTCCloud 对应的不同平台的接口，来给 SDK 集成三个回调函数。

>? TRTCCloud 对应的不同平台的接口分别为：
>- iOS：setAudioFrameDelegate。
>- Android：setAudioFrameListener。
>- Windows：setAudioFrameCallback。

| 接口 | 说明 |
|---------|---------|
| onCapturedAudioFrame | 获取本地麦克风采集到的音频源数据。在非自定义采集模式下，SDK 会负责麦克风的声音采集工作，但您可能也需要拿到 SDK 采集到的声音源数据，通过此回调函数就可以获取。 |
| onPlayAudioFrame | 该函数会回调每一路远程用户的声音数据，这是混音前的数据。如果您要对某一路的语音进行语音识别，就可以用到这个回调。 |
| onMixedPlayAudioFrame | 各路音频数据混合后，在送到扬声器播放之前，会通过此函数回调出来。 |

>!
>- 不要在上述回调函数中做任何耗时操作，建议直接拷贝，并通过另一线程进行处理，否则会导致声音断断续续或者回声抵消（AEC）失效的问题。
>- 上述回调函数中回调出来的数据都只允许读取和拷贝，不能修改，否则会导致各种不确定的后果。
