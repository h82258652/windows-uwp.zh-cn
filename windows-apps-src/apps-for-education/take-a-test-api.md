---
Description: The JavaScript API for the Microsoft Take a Test app allows you to do secure assessments. Take a Test provides a secure browser that prevents students from using other computer or internet resources during a test.
title: 参加测验 JavaScript API。
author: PatrickFarley
ms.author: pafarley
ms.assetid: 9bff6318-504c-4d0e-ba80-1a5ea45743da
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，教育版
ms.localizationpriority: medium
ms.openlocfilehash: 38596ad12ac309db5dc60e4a5183eee9bf8c7b7c
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2018
ms.locfileid: "4499973"
---
# <a name="take-a-test-javascript-api"></a>参加测验 JavaScript API

[参加测验](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10)是基于浏览器的 UWP 应用，呈现锁定的在线评估为高利害关系测试，使教师能够专注于评估内容，而不是如何提供安全的测试环境。 为了实现此目的，它使用任何 Web 应用程序都可以利用的 JavaScript API。 “参加测验”API 支持高利害关系通用核心测试的 [SBAC 浏览器 API 标准](http://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf)。

有关应用本身的详细信息，请参阅[参加测验应用技术参考](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396)。 有关疑难解答帮助，请参阅[使用事件查看器对 Microsoft 参加测验进行疑难解答](troubleshooting.md)。

## <a name="reference-documentation"></a>参考文档
参加测验 API 存在于以下命名空间中。 注意，所有 API 都依赖于全局 `SecureBrowser` 对象。

| 命名空间 | 说明 |
|-----------|-------------|
|[安全命名空间](#security-namespace)|包含能够锁定用于测试的设备和强制执行测试环境的 API。 |

### <a name="security-namespace"></a>安全命名空间

安全命名空间可以锁定设备、 检查用户和系统进程的列表、 获取 MAC 和 IP 地址以及清除缓存的 web 资源。

| 方法 | 描述   |
|--------|---------------|
|[lockDown](#lockDown) | 锁定用于测试的设备。 |
|[isEnvironmentSecure](#isEnvironmentSecure) | 确定锁定上下文是否仍然应用于设备。 |
|[getDeviceInfo](#getDeviceInfo) | 获取有关运行测试应用程序的平台的详细信息。 |
|[examineProcessList](#examineProcessList)|获取正在运行的用户和系统进程的列表。|
|[close](#close) | 关闭浏览器并解锁设备。 |
|[getPermissiveMode](#getPermissiveMode)|检查许可模式是处于打开状态还是关闭状态。|
|[setPermissiveMode](#setPermissiveMode)|打开或关闭许可模式。|
|[emptyClipBoard](#emptyClipBoard)|清除系统剪贴板。|
|[getMACAddress](#getMACAddress)|获取设备的 MAC 地址列表。|
|[getStartTime](#getStartTime) | 获取启动测试应用的时间。 |
|[getCapability](#getCapability) | 查询功能是处于启用状态还是禁用状态。 |
|[setCapability](#setCapability)|启用或禁用指定功能。| 
|[isRemoteSession](#isRemoteSession) | 检查当前会话是否是远程登录。 |
|[isVMSession](#isVMSession) | 检查当前会话是否在虚拟机上运行。 |

---

<span id="lockDown"/>

### <a name="lockdown"></a>lockDown
锁定设备。 也用于解锁设备。 在允许学生开始测试之前，测试 Web 应用程序将调执行此调用。 实施者需执行任何必要的操作来保护测试环境。 为保护环境所采取的步骤特定于设备，例如，包括禁用屏幕截图、在安全模式下禁用语音聊天、清除系统剪贴板、进入展台模式、禁用 OSX 10.7+ 设备中的空间等方面。测试应用程序将在评估开始之前启用锁定，并在学生完成评估并退出安全测试时禁用锁定。

**语法**  
`void SecureBrowser.security.lockDown(Boolean enable, Function onSuccess, Function onError);`

**参数**  
* `enable` - 若为 **true**，将在锁屏界面上运行“参加测验”应用并应用此[文档](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396)中讨论的策略。 若为 **false**，将停止在锁屏界面上运行“参加测验”并关闭它，除非该应用不处于锁定状态；在此情况下没有影响。  
* `onSuccess` - [可选] 要在锁定已成功启用或禁用后调用的函数。 其形式必须是 `Function(Boolean currentlockdownstate)`。  
* `onError` - [可选] 要在锁定操作失败时调用的函数。 其形式必须是 `Function(Boolean currentlockdownstate)`。  

**要求**  
Windows 10 版本 1709

---

<span id="isEnvironmentSecure" />

### <a name="isenvironmentsecure"></a>isEnvironmentSecure
确定锁定上下文是否仍然应用于设备。 在允许学生开始测试之前，测试 Web 应用程序将调用此方法，并在测试中定期调用。

**语法**  
`void SecureBrowser.security.isEnvironmentSecure(Function callback);`

**参数**  
* `callback` - 要在此函数完成时调用的函数。 其形式必须是 `Function(String state)`，其中 `state` 是包含两个字段的 JSON 字符串。 第一个是 `secure` 字段，只有在启用了所有必要的锁定(或禁用了功能)以启用安全测试环境时，才会显示 `true`，而且因为应用进入锁定模式，所以这些都没有受到威胁。 另一个字段 `messageKey`，包括其他详细信息或特定于供应商的信息。 此处的目的是，使供应商可添加其他能够增强布尔 `secure` 标志的信息：

```JSON
{
    'secure' : "true/false",
    'messageKey' : "some message"
}
```

**要求**  
Windows 10 版本 1709

---

<span id="getDeviceInfo" />

### <a name="getdeviceinfo"></a>getDeviceInfo
获取有关运行测试应用程序的平台的详细信息。 这用于增加用户代理中任何可识别的信息。

**语法**  
`void SecureBrowser.security.getDeviceInfo(Function callback);`

**参数**  
* `callback` - 要在此函数完成时调用的函数。 其形式必须是 `Function(String infoObj)`，其中 `infoObj` 是包含几个字段的 JSON 字符串。 必须支持以下字段：
    * `os` 表示操作系统类型（例如：Windows、macOS、Linux、iOS、Android 等。）
    * `name` 表示操作系统发布名称（如果有）（例如：Sierra、Ubuntu）。
    * `version` 表示操作系统版本（例如：10.1、10 专业版等。）
    * `brand` 表示安全的浏览器品牌（例如：OAKS、CA、SmarterApp 等。）
    * `model` 表示仅适用于移动设备的设备模型；对于桌面浏览器则为 null/unused。

**要求**  
Windows 10 版本 1709

---

<span id="examineProcessList" />

### <a name="examineprocesslist"></a>examineProcessList
获取在用户所拥有的客户端计算机上运行的所有进程的列表。 测试应用程序将对调用此方法来检查列表，并将其与在测试周期期间被列入禁止列表的进程列表进行比较。 应该在评估开始时和学生进行评估期间（定期）执行此调用。 如果检测到已列入禁止列表的进程，则应停止评估以保持测试完整性。

**语法**  
`void SecureBrowser.security.examineProcessList(String[] blacklistedProcessList, Function callback);`

**参数**  
* `blacklistedProcessList` - 测试应用程序已列入禁止列表的进程列表。  
`callback` - 要在找到活动进程后调用的函数。 其形式必须为：`Function(String foundBlacklistedProcesses)`，其中 `foundBlacklistedProcesses` 的形式为：`"['process1.exe','process2.exe','processEtc.exe']"`。 如果未找到已列入禁止列表的进程，它将为空。 如果为 null，则指示原始函数调用时发生错误。

**备注** 该列表不包括系统进程。

**要求**  
Windows 10 版本 1709

---

<span id="close"/>

### <a name="close"></a>close
关闭浏览器并解锁设备。 当用户选择退出浏览器时，测试应用程序应调用此方法。

**语法**  
`void SecureBrowser.security.close(restart);`

**参数**  
* `restart` - 此参数将被忽略，但必须提供。

**备注** 在 Windows 10 版本 1607 中，必须最初便锁定设备。 在更高版本中，此方法会关闭浏览器，无论设备是否锁定。

**要求**  
Windows 10 版本 1709

---

<span id="getPermissiveMode" />

### <a name="getpermissivemode"></a>getPermissiveMode
测试 Web 应用程序应调用此方法来确定许可模式是处于打开状态还是关闭状态。 在许可模式中，浏览器将放松其部分严格的安全挂钩，从而允许辅助技术与安全浏览器协同工作。 例如，那些强行阻止其他应用程序 UI 在其顶部演示的浏览器可能想要在许可模式中放松这一点。 

**语法**  
`void SecureBrowser.security.getPermissiveMode(Function callback)`

**参数**  
* `callback` - 在此调用完成时要调用的函数。 其形式必须为：`Function(Boolean permissiveMode)`，其中 `permissiveMode` 指示浏览器目前是否为许可模式。 如果为 undefined 或 null，则在 get 操作时出现了错误。

**要求**  
Windows 10 版本 1709

---

<span id="setPermissiveMode" />

### <a name="setpermissivemode"></a>setPermissiveMode
测试 Web 应用程序应调用此方法来打开或关闭许可模式。 在许可模式中，浏览器将放松其部分严格的安全挂钩，从而允许辅助技术与安全浏览器协同工作。 例如，那些强行阻止其他应用程序 UI 在其顶部演示的浏览器可能想要在许可模式中放松这一点。 

**语法**  
`void SecureBrowser.security.setPermissiveMode(Boolean enable, Function callback)`

**参数**  
* `enable` - 指示预期许可模式状态的布尔值。  
* `callback` - 在此调用完成时要调用的函数。 其形式必须为：`Function(Boolean permissiveMode)`，其中 `permissiveMode` 指示浏览器目前是否为许可模式。 如果为 undefined 或 null，则在 set 操作时出现了错误。

**要求**  
Windows 10 版本 1709

---

<span id="emptyClipBoard"/>

### <a name="emptyclipboard"></a>emptyClipBoard
清除系统剪贴板。 测试应用程序应调用此方法，强制清除可能存储在系统剪贴板中的任何数据。 **[lockDown](#lockDown)** 函数也执行此操作。

**语法**  
`void SecureBrowser.security.emptyClipBoard();`

**要求**  
Windows 10 版本 1709

---

<span id="getMACAddress" />

### <a name="getmacaddress"></a>getMACAddress
获取设备的 MAC 地址列表。 测试应用程序应调用此方法来帮助诊断。 

**语法**  
`void SecureBrowser.security.getMACAddress(Function callback);`

**参数**  
* `callback` - 在此调用完成时要调用的函数。 其形式必须为：`Function(String addressArray)`，其中 `addressArray` 的形式为：`"['00:11:22:33:44:55','etc']"`。

**备注**  
很难依靠源 IP 地址来区分测试服务器中的最终用户计算机，因为学校通常会使用防火墙/NAT/代理。 出于诊断目的，MAC 地址允许应用区分常见防火墙之后的最终客户端计算机处。

**要求**  
Windows 10 版本 1709

---

<span id="getStartTime" />

### <a name="getstarttime"></a>getStartTime
获取启动测试应用的时间。

**语法**  
`DateTime SecureBrowser.settings.getStartTime();`

**返回**  
指示测试应用启动时间的 DateTime 对象。

**要求**  
Windows 10 版本 1709

---

<span id="getCapability"/>

### <a name="getcapability"></a>getCapability
查询功能是处于启用状态还是禁用状态。 

**语法**  
`Object SecureBrowser.security.getCapability(String feature)`

**参数**  
`feature` - 用于确定要查询的功能的字符串。 有效功能字符串有“screenMonitoring”、“printing”和“textSuggestions”（不区分大小写）。

**返回值**  
此函数返回 JavaScript 对象或形式为 `{<feature>:true|false}` 的文本。 如果查询的功能已启用，则为 **true**，如果功能未启用或功能字符串无效，则为 **false**。

**要求** Windows 10 版本 1703

---

<span id="setCapability"/>

### <a name="setcapability"></a>setCapability
启用或禁用浏览器上的特定功能。

**语法**  
`void SecureBrowser.security.setCapability(String feature, String value, Function onSuccess, Function onError)`

**参数**  
* `feature` - 用于确定要设置的功能的字符串。 有效功能字符串有 `"screenMonitoring"`、`"printing"` 和 `"textSuggestions"`（不区分大小写）。  
* `value` - 该功能的预期设置。 它必须是 `"true"` 或 `"false"`。  
* `onSuccess` - [可选] 要在 set 操作已成功完成后调用的函数。 其形式必须为：`Function(String jsonValue)`，其中 *jsonValue* 的形式为：`{<feature>:true|false|undefined}`。  
* `onError` - [可选] 要在 set 操作失败时调用的函数。 其形式必须为：`Function(String jsonValue)`，其中 *jsonValue* 的形式为：`{<feature>:true|false|undefined}`。

**备注**  
如果浏览器不知道目标功能，则此函数将向回调函数传递值 `undefined`。

**要求** Windows 10 版本 1703

---

<span id="isRemoteSession"/>

### <a name="isremotesession"></a>isRemoteSession
检查当前会话是否是远程登录。

**语法**  
`Boolean SecureBrowser.security.isRemoteSession();`

**返回值**  
如果当前会话为远程，则为 **true**，否则为 **false**。

**要求**  
Windows 10 版本 1709

---

<span id="isVMSession"/>

### <a name="isvmsession"></a>isVMSession
检查当前会话是否在虚拟机内运行。

**语法**  
`Boolean SecureBrowser.security.isVMSession();`

**返回值**  
如果当前会话在虚拟机中运行，则为 **true**，否则为 **false**。

**备注**  
此 API 检查只可以检测在实现适当 API 的特定虚拟机监控程序中运行的 VM 会话

**要求**  
Windows 10 版本 1709

---