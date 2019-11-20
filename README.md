# Talk SDK for Unity3D 使用指南

## 适用范围

本文档适用于游密实时语音引擎（Talk SDK）Unity3D平台下接入，API接口说明部分同样适用于其他平台，可互相参考。

## SDK目录概述

YMRTC SDK提供了以下文件：

- Plugins库文件，分为Android平台和iOS平台。
>`AndroidManifest.xml`：可用来配置安卓下SDK所需要的权限、服务等。
`Plugins/Android`： Android平台使用的动态库，包括ARMv5、ARMv7和 X86 三种 CPU 架 构下的libyoume_voice_engine.so文件,还包括youme_voice_engine.jar。
`Plugins/iOS`：iOS平台使用的静态库，包含libyoume_voice_engine.a文件。

- YouMeVoiceEngine文件夹，内含封装SDK的C#接口文件。
>`YouMeVoiceAPI.cs`：封装了Talk SDK 的全部功能接口。
>`YouMeConstDefine.cs`：包含Talk SDK 错误码等枚举类型定义。

## 开发环境集成

### 从Unity3D开发环境集成

- 双击 `unitypackage`包;
- 在弹出的`Import Unity Package`对话框中，所有的复选框打勾(如下图所示)，但要格外注意AndroidManifest.xml可能与工程已有或将有的同名文件直接覆盖，应先比对合并；

![1.png](https://youme.im/doc/images/1.png)

- 为应用选择要支持的CPU架构：
在Unity3D的`Project View`中选中`Assets/Plugins/Android/libs`下特定的CPU架构，比如`armeabi-v7a`，勾选右侧的`Inspector View`中的`Select platforms for plugin`下的`Android`右侧的复选框，此处`注意armeabi和armeabi-v7a两种ARM架构的库文件，只能勾选其中一种架构，否则会出现库文件冲突的错误`。

![2.png](https://youme.im/doc/images/2.jpg)

### 导出Android工程

- 在`File`->`Build Settings…`的`Platform`列表中选择`Android`，酌情点击`Switch Platform`按钮，然后勾选右侧`Google Android Project`；
- 点击`Build Settings`->`Export`，选择输出的Android工程路径，导出Android工程；
![3.gif](https://youme.im/doc/images/3.gif)

- 检查权限配置 AndroidManifest.xml 配置
  ``` xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
    <uses-permission android:name="android.permission.CHANGE_NETWORK_STATE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" /> <!-- 可选 -->
    <uses-permission android:name="android.permission.BLUETOOTH" />
  ```

### 导出iOS工程

- 在`File`->`Build Settings…`的Platform列表中选择iOS，酌情点击`Switch Platform`按钮；
- 点击`Build Settings`->`Build`，选择输出iOS工程的路径，输入工程名字，导出iOS工程；
- 在Xcode中打开上一步输出的iOS工程，在工程配置中`Build Phases`->`Link Binary With Libraries`下拉菜单中添加添加四个框架文件:`libc++.tbd`、`libsqlite3.0.tbd`、`libz.1.2.5.tbd`、`libresolv.9.tbd` 和 `CoreTelephony.framework`。
- **为iOS10以上版本添加录音权限配置**
iOS10系统使用录音权限，需要在target的`info.plist`中新加`NSMicrophoneUsageDescription`键，值为字符串(授权弹窗出现时提示给用户)。首次录音时会向用户申请权限。配置方式如下：
![iOS10录音权限配置](https://youme.im/doc/images/im_iOS_record_config.jpg)

## API接口调用流程
API调用的基本流程如下图所示，具体接口说明参见API接口手册。
![](https://www.youme.im/doc/images/talk_shixutu.png)


## 使用talkSDK实现语音通话的基本流程 

`设置回调（SetCallback）-> 初始化（init）->收到初始化成功回调通知（YOUME_EVENT_INIT_OK）`->`加入语音单频道（joinChannelSingleMode）->收到加入频道成功回调通知（YOUME _EVENT_JOIN_OK）`->`打开麦克风（SetMicrophoneMute（false））-> 收到麦克风已打开回调通知(YOUME_EVENT_LOCAL_MIC_ON)->打开扬声器（SetSpeakerMute（false））-> 收到扬声器已打开回调通知（YOUME_EVENT_LOCAL_SPEAKER_ON）`->`设置音量（SetVolume（70）（该音量建议70））->(到了前面一步已经可以和当前进入同一频道的人进行实时通话了)`->`使用其他接口`
->`结束`

### 备注：
[详细接口介绍可查看“unity-API手册.md”文档](https://github.com/youmesdk/YoumeTalkSDK_Unity/blob/master/unity-API%E6%89%8B%E5%86%8C.md)

实际Demo可点击此处下载->[Talk SDK for Unity3D](https://github.com/youmesdk/YoumeTalkDemo_Unity)
