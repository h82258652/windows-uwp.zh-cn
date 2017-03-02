---
author: TylerMSFT
Description: "适用于 Microsoft 参加测验应用的 JavaScript API 使你可以进行安全的评估。 参加测验提供了安全的浏览器，可防止学生在测试时使用其他计算机或 Internet 资源。"
title: "参加测验 JavaScript API。"
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 9bff6318-504c-4d0e-ba80-1a5ea45743da
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: ac1a9b38a9857ae536025e682f98d01135850a19
ms.lasthandoff: 02/08/2017

---

# <a name="take-a-test-javascript-api"></a>参加测验 JavaScript API

[参加测验](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10)是基于浏览器的应用，可为高利害关系测试提供锁定的在线评估。 它支持高利害关系通用核心测试的 SBAC 浏览器 API 标准，并使你可以专注于评估内容，而不是锁定 Windows 的方法。

Microsoft Edge 浏览器支持的参加测验具有一个 JavaScript API，Web 应用程序可使用它为参加测验提供锁定体验。

该 API（基于[通用核心 SBAC API](http://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf)）提供文本到语音转换功能以及查询设备是否处于锁定状态、正在运行的用户和系统进程等功能。

有关应用本身的信息，请参阅[参加测验应用技术参考](https://technet.microsoft.com/edu/windows/take-a-test-app-technical)。

> [!Important]
> 这些 API 在远程会话中不起作用。  

有关疑难解答帮助，请参阅[使用事件查看器对 Microsoft 参加测验进行疑难解答](troubleshooting.md)。

## <a name="reference-documentation"></a>参考文档
参加测验 API 由以下命名空间组成。 

| 命名空间 | 说明 |
|-----------|-------------|
|[安全命名空间](#security-namespace)|使你可以锁定设备|
|[TTS 命名空间](#tts-namespace)|文本到语音转换功能|


 ### <a name="security-namespace"></a>安全命名空间

安全命名空间使你可以锁定设备、检查用户和系统进程列表、获取 MAC 和 IP 地址以及清除缓存的 Web 资源。

| 方法 | 说明   |
|--------|---------------|
|[clearCache](#clearCache) | 清除缓存的 Web 资源 |
|[关闭](#close) | 关闭浏览器并解锁设备 |
|[enableLockDown](#enableLockDown) | 锁定设备。 也用于解锁设备 |
|[getIPAddressList](#getIPAddressList) | 获取设备的 IP 地址列表 |
|[getMACAddress](#getMACAddress)|获取设备的 MAC 地址列表|
|[getProcessList](#getProcessList)|获取正在运行的用户和系统进程的列表|
|[isEnvironmentSecure](#isEnvironmentSecure)|确定锁定上下文是否仍然应用于设备|  

---
<span id="clearCache"/>
### <a name="void-clearcache"></a>void clearCache()
清除缓存的 Web 资源。

**语法**  
`browser.security.clearCache();`

**参数**  
`None`

**返回值**  
`None`

**要求**  
Windows 10 版本 1607

---

<span id="close"/>
### <a name="closeboolean-restart"></a>close(boolean restart)
关闭浏览器并解锁设备。

**语法**  
`browser.security.close(false);`

**参数**  
`restart` - 此参数将被忽略，但必须提供

**返回值**  
`None`

**要求**  
Windows 10 版本 1607

---

<span id="enableLockDown"/>
### <a name="enablelockdownboolean-lockdown"></a>enableLockdown(boolean lockdown)
锁定设备。 也用于解锁设备。

**语法**  
`browser.security.enableLockDown(true|false);`

**参数**  
`lockdown` - `true` 用于在锁屏界面上运行 Take-a-Test 应用，并应用此[文档](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396)中讨论的策略。 `False` 停止在锁屏界面上运行参加测验 Take-a-Test 并关闭它，除非该应用不处于锁定状态；在此情况下没有影响。

**返回值**  
`None`

**要求**  
Windows 10 版本 1607

---

<span id="getIPAddressList"/>
### <a name="string-getipaddresslist"></a>string[] getIPAddressList()
获取设备的 IP 地址列表。

**语法**  
`browser.security.getIPAddressList();`

**参数**  
`None`

**返回值**  
`An array of IP addresses.`

---

<span id="getMACAddress" />
### <a name="string-getmacaddress"></a>string[] getMACAddress()
获取设备的 MAC 地址列表。

**语法**  
`browser.security.getMACAddress();`

**参数**  
`None`

**返回值**  
`An array of MAC addresses.`

**要求**  
Windows 10 版本 1607

---

<span id="getProcessList" />
### <a name="string-getprocesslist"></a>string[] getProcessList()
获取用户正在运行的进程列表。

**语法**  
`browser.security.getProcessList();`

**参数**  
`None`

**返回值**  
`An array of running process names.`

**备注** 该列表不包括系统进程。

**要求**  
Windows 10 版本 1607

---

<span id="isEnvironmentSecure" />
### <a name="boolean-isenvironmentsecure"></a>boolean isEnvironmentSecure()
确定锁定上下文是否仍然应用于设备。

**语法**  
`browser.security.isEnvironmentSecure();`

**参数**  
`None`

**返回值**  
`True indicates that the lockdown context is applied to the device; otherwise false.`

**要求**  
Windows 10 版本 1607

---

### <a name="tts-namespace"></a>TTS 命名空间

TTS 命名空间处理应用的文本到语音转换功能。

| 方法 | 说明 |
|--------|-------------|
|[getStatus](#getStatus) | 获取语音播放状态|
|[getVoices](#getVoices) | 获取可用声音包列表|
|[pause](#pause)|暂停语音合成|
|[resume](#resume)|恢复暂停的语音合成|
|[speak](#speak)|客户端文本到语音合成|
|[stop](#stop)|停止语音合成|

> [!Tip]
> [Microsoft Edge 语音合成 API](https://blogs.windows.com/msedgedev/2016/06/01/introducing-speech-synthesis-api/) 是 [W3C 语音 API](https://dvcs.w3.org/hg/speech-api/raw-file/tip/webspeechapi.html) 的一个实现，我们建议开发人员尽可能使用此 API。

---

<span id="getStatus" />
### <a name="string-getstatus"></a>string getStatus()
获取语音播放状态。

**语法**  
`browser.tts.getStatus();`

**参数**  
`None`

**返回值**  
`The speech playback status. Possible values are: “available”, “idle”, “paused”, and “speaking”.`

**要求**  
Windows 10 版本 1607

---

<span id="getVoices" />
### <a name="string-getvoices"></a>string[] getVoices()
获取可用声音包列表。

**语法**  
`browser.tts.getVoices();`

**参数**  
`None`

**返回值**  
`The available voice packs. For example: “Microsoft Zira Mobile”, “Microsoft Mark Mobile”`

**要求**  
Windows 10 版本 1607

---

<span id="pause" />
### <a name="void-pause"></a>void pause()

暂停语音合成。

**语法**  
`browser.tts.pause();`

**参数**

`None`

**返回值**

`None`

**要求**  
Windows 10 版本 1607

---

<span id="resume" />
### <a name="void-resume"></a>void resume()
恢复暂停的语音合成。

**语法**  
`browser.tts.resume();`

**参数**
`None`

**返回值**
`None`

**要求**  
Windows 10 版本 1607

---

<span id="speak" />
### <a name="void-speakstring-text-object-options-function-callback"></a>void speak(string text, object options, function callback)
启动客户端文本到语音合成。

**语法**  
`void browser.tts.speak(“Hello world”, options, callback);`

**参数**  
`Speech options such as gender, pitch, rate, volume. For example:`  
```
var options = {
   'gender': this.currentGender,
   'language': this.currentLanguage,
   'pitch': 1,
   'rate': 1,
   'voice': this.currentVoice,
   'volume': 1
};
```

**返回值**  
`None`

**备注** 选项变量必须为小写。 性别、语言和语音参数采用字符串。
音量、音调和语速必须在语音合成标记语言文件 (SSML) 内标记，而不是在选项对象内标记。
选项对象必须遵循上述示例中所示的顺序、命名和大小写。

**要求**  
Windows 10 版本 1607

---

<span id="stop" />
### <a name="void-stop"></a>void stop()
停止语音合成。

**语法**  
`void browser.tts.speak(“Hello world”, options, callback);`

**参数**  
`None`

**返回值**  
`None`

**要求**  
Windows 10 版本 1607

