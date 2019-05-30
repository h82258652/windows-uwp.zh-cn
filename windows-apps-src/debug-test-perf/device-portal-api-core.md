---
ms.assetid: bfabd3d5-dd56-4917-9572-f3ba0de4f8c0
title: Device Portal 核心 API 参考
description: 了解 Windows Device Portal 核心 REST API，可用于访问数据和以编程方式控制设备。
ms.custom: 19H1
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10，uwp，设备门户
ms.localizationpriority: medium
ms.openlocfilehash: b2e1e2dfdb1dd52e1dd07a146badd78a6bb809fa
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359930"
---
# <a name="device-portal-core-api-reference"></a>Device Portal 核心 API 参考

所有设备门户功能均基于 REST API 构建，开发人员可用来直接调用以访问资源并以编程方式控制其设备。

## <a name="app-deployment"></a>应用部署

### <a name="install-an-app"></a>安装应用

**请求**

可以使用以下请求格式安装应用。

| 方法      | 请求 URI |
| :------     | :----- |
| 发布 | /api/app/packagemanager/package |

**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| 包   | （**必需**）要安装的程序包的文件名。 |

**请求标头**

- 无

**请求正文**

- .appx 或 .appxbundle 文件，以及应用需要的任何依赖关系。 
- 用于为应用签名的证书（如果设备是 IoT 或 Windows 台式机）。 其他平台不需要证书。 

**响应**

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
| 200 | 已接受和正在处理的部署请求 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="install-a-related-set"></a>安装相关集

**请求**

可以使用以下请求格式安装[相关集](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/)。

| 方法      | 请求 URI |
| :------     | :------ |
| 发布 | /api/app/packagemanager/package |

**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| 包   | （**必需**）要安装的程序包的文件名。 |

**请求标头**

- 无

**请求正文** 
- 在将可选程序包文件名指定为参数时向其添加“.opt”，例如：“foo.appx.opt”或“bar.appxbundle.opt”。 
- .appx 或 .appxbundle 文件，以及应用需要的任何依赖关系。 
- 用于为应用签名的证书（如果设备是 IoT 或 Windows 台式机）。 其他平台不需要证书。 

**响应**

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
| 200 | 已接受和正在处理的部署请求 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="register-an-app-in-a-loose-folder"></a>注册 Loose 文件夹中的应用

**请求**

你可以利用以下请求格式来注册 Loose 文件夹中的应用。

| 方法      | 请求 URI |
| :------     | :----- |
| 发布 | /api/app/packagemanager/networkapp |

**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

```json
{
    "mainpackage" :
    {
        "networkshare" : "\\some\share\path",
        "username" : "optional_username",
        "password" : "optional_password"
    }
}
```

**响应**

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
| 200 | 已接受和正在处理的部署请求 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="register-a-related-set-in-loose-file-folders"></a>注册 Loose 文件夹中的相关集

**请求**

可以使用以下请求格式注册 Loose 文件夹中的[相关集](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/)。

| 方法      | 请求 URI |
| :------     | :----- |
| 发布 | /api/app/packagemanager/networkapp |

**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

```json
{
    "mainpackage" :
    {
        "networkshare" : "\\some\share\path",
        "username" : "optional_username",
        "password" : "optional_password"
    },
    "optionalpackages" :
    [
        {
            "networkshare" : "\\some\share\path2",
            "username" : "optional_username2",
            "password" : "optional_password2"
        },
        ...
    ]
}
```

**响应**

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
| 200 | 已接受和正在处理的部署请求 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="get-app-installation-status"></a>获取应用安装状态

**请求**

可以通过使用以下请求格式来获取当前正在进行的应用安装状态。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/app/packagemanager/state |

**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
| 200 | 最后一个部署的结果 |
| 204 | 安装正在运行 |
| 404 | 未找到任何安装操作 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="uninstall-an-app"></a>卸载应用

**请求**

可以通过使用以下请求格式来卸载应用。
 
| 方法      | 请求 URI |
| :------     | :----- |
| DELETE | /api/app/packagemanager/package |

**URI 参数**

| URI 参数 | 描述 |
| :------          | :------ |
| 包   | （**必需**）目标应用的 PackageFullName（来自 GET /api/app/packagemanager/packages） |

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
|  200 | 确定 | 
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="get-installed-apps"></a>获取已安装应用

**请求**

可以通过使用以下请求格式来获取系统上已安装应用的列表。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/app/packagemanager/packages |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包含已安装程序包列表以及相关详细信息。 此响应的模板如下所示。
```json
{"InstalledPackages": [
    {
        "Name": string,
        "PackageFamilyName": string,
        "PackageFullName": string,
        "PackageOrigin": int, (https://msdn.microsoft.com/en-us/library/windows/desktop/dn313167(v=vs.85).aspx)
        "PackageRelativeId": string,
        "Publisher": string,
        "Version": {
            "Build": int,
            "Major": int,
            "Minor": int,
            "Revision": int
     },
     "RegisteredUsers": [
     {
        "UserDisplayName": string,
        "UserSID": string
     },...
     ]
    },...
]}
```
**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
|  200 | 确定 | 
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

## <a name="bluetooth"></a>蓝牙

<hr>

### <a name="get-the-bluetooth-radios-on-the-machine"></a>获取计算机上的蓝牙无线收发器

**请求**

可以通过使用以下请求格式来获取计算机上的蓝牙无线收发器的列表。 这可以升级到 WebSocket 连接，使用相同的 JSON 数据。
 
| 方法        | 请求 URI |
| :------          | :------ |
| GET           | /api/bt/getradios |
| GET/WebSocket | /api/bt/getradios |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包括连接到该设备的蓝牙无线收发器的 JSON 数组。
```json
{"BluetoothRadios" : [
    {
        "BluetoothAddress" : int64,
        "DisplayName" : string,
        "HasUnknownUsbDevice" : boolean,
        "HasProblem" : boolean,
        "ID" : string,
        "ProblemCode" : int,
        "State" : string
    },...
]}
```
**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码 | 描述 |
| :------             | :------ |
| 200              | 确定 |
| 4XX              | 错误代码 |
| 5XX              | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="turn-the-bluetooth-radio-on-or-off"></a>打开或关闭蓝牙无线收发器

**请求**

将特定的蓝牙无线收发器设置为开或关。
 
| 方法 | 请求 URI |
| :------   | :------ |
| 发布   | /api/bt/setradio |

**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| ID            | （**必需**）蓝牙无线收发器的设备 ID，必须经过 base 64 编码。 |
| 状态         | (**必需**) 这可能是`"On"`或`"Off"`。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码 | 描述 |
| :------             | :------ |
| 200              | 确定 |
| 4XX              | 错误代码 |
| 5XX              | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* HoloLens
* IoT

---
### <a name="get-a-list-of-paired-bluetooth-devices"></a>获取配对的蓝牙设备的列表

**请求**

使用以下请求格式，可以获取当前配对的蓝牙设备的列表。 这可以升级为使用相同的 JSON 数据的 WebSocket 连接。 在 WebSocket 连接的生存期内，可以更改设备列表。 通过 WebSocket 连接每次更新的时，将发送设备的完整列表。

| 方法        | 请求 URI       |
| :---          | :---              |
| GET           | /api/bt/getpaired |
| GET/WebSocket | /api/bt/getpaired |

**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

响应包括当前配对的蓝牙设备的 JSON 数组。
```json
{"PairedDevices": [
    {
        "Name" : string,
        "ID" : string,
        "AudioConnectionStatus" : string
    },...
]}
```
*AudioConnectionStatus*字段将显示该设备可用于在此系统上的音频。 （策略和可选组件可能会影响此。）*AudioConnectionStatus*将为"已连接"断开连接"。

---
### <a name="get-a-list-of-available-bluetooth-devices"></a>获取可用的蓝牙设备的列表

**请求**

可以获取有关通过使用以下请求格式配对的可用蓝牙设备的列表。 这可以升级为使用相同的 JSON 数据的 WebSocket 连接。 在 WebSocket 连接的生存期内，可以更改设备列表。 通过 WebSocket 连接每次更新的时，将发送设备的完整列表。

| 方法        | 请求 URI          |
| :---          | :---                 |
| GET           | /api/bt/getavailable |
| GET/WebSocket | /api/bt/getavailable |

**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

响应包括当前可用于配对的蓝牙设备的 JSON 数组。
```json
{"AvailableDevices": [
    {
        "Name" : string,
        "ID" : string
    },...
]}
```

---
### <a name="connect-a-bluetooth-device"></a>蓝牙设备连接

**请求**

如果该设备可用于在此系统上的音频，将连接到设备。 （策略和可选组件可能会影响此。）

| 方法       | 请求 URI           |
| :---         | :---                  |
| 发布         | /api/bt/connectdevice |

**URI 参数**

| URI 参数 | 描述 |
| :---          | :--- |
| ID            | (**需**) 的关联终结点的蓝牙设备 ID 和必须采用 Base64 编码。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码 | 描述 |
| :---             | :--- |
| 200              | 确定 |
| 4XX              | 错误代码 |
| 5XX              | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* HoloLens
* IoT


---
### <a name="disconnect-a-bluetooth-device"></a>蓝牙设备断开连接

**请求**

如果该设备可用于在此系统上的音频，则将断开设备。 （策略和可选组件可能会影响此。）

| 方法       | 请求 URI              |
| :---         | :---                     |
| 发布         | /api/bt/disconnectdevice |

**URI 参数**

| URI 参数 | 描述 |
| :---          | :--- |
| ID            | (**需**) 的关联终结点的蓝牙设备 ID 和必须采用 Base64 编码。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码 | 描述 |
| :---             | :--- |
| 200              | 确定 |
| 4XX              | 错误代码 |
| 5XX              | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* HoloLens
* IoT

---
## <a name="device-manager"></a>设备管理器
<hr>

### <a name="get-the-installed-devices-on-the-machine"></a>获取计算机上已安装的设备

**请求**

可以通过使用以下请求格式来获取计算机上已安装设备的列表。

| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/devicemanager/devices |

**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包括连接到该设备的设备的 JSON 数组。
```json
{"DeviceList": [
    {
        "Class": string,
        "Description": string,
        "ID": string,
        "Manufacturer": string,
        "ParentID": string,
        "ProblemCode": int,
        "StatusCode": int
    },...
]}
```

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
|  200 | 确定 | 
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* IoT

<hr>

### <a name="get-data-on-connected-usb-deviceshubs"></a>在已连接的 USB 设备/集线器上获取数据

**请求**

可以通过使用以下请求格式来获取已连接的 USB 设备和集线器的 USB 描述符的列表。

| 方法      | 请求 URI |
| :------     | :----- |
| GET | /ext/devices/usbdevices |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

响应为包含 USB 设备的 DeviceID 以及集线器的 USB 描述符和端口信息的 JSON。
```json
{
    "DeviceList": [
        {
        "ID": string,
        "ParentID": string, // Will equal an "ID" within the list, or be blank
        "Description": string, // optional
        "Manufacturer": string, // optional
        "ProblemCode": int, // optional
        "StatusCode": int // optional
        },
        ...
    ]
}
```

**示例返回数据**
```json
{
    "DeviceList": [{
        "ID": "System",
        "ParentID": ""
    }, {
        "Class": "USB",
        "Description": "Texas Instruments USB 3.0 xHCI Host Controller",
        "ID": "PCI\\VEN_104C&DEV_8241&SUBSYS_1589103C&REV_02\\4&37085792&0&00E7",
        "Manufacturer": "Texas Instruments",
        "ParentID": "System",
        "ProblemCode": 0,
        "StatusCode": 25174026
    }, {
        "Class": "USB",
        "Description": "USB Composite Device",
        "DeviceDriverKey": "{36fc9e60-c465-11cf-8056-444553540000}\\0016",
        "ID": "USB\\VID_045E&PID_00DB\\8&2994096B&0&1",
        "Manufacturer": "(Standard USB Host Controller)",
        "ParentID": "USB\\VID_0557&PID_8021\\7&2E9A8711&0&4",
        "ProblemCode": 0,
        "StatusCode": 25182218
    }]
}
```

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
|  200 | 确定 | 
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* IoT

<hr>

## <a name="dump-collection"></a>转储集合

<hr>

### <a name="get-the-list-of-all-crash-dumps-for-apps"></a>获取应用的所有故障转储列表

**请求**

可以通过使用以下请求格式来获取所有旁加载应用的所有可用故障转储列表。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/debug/dump/usermode/dumps |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包括每个旁加载应用程序的故障转储列表。

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
|  200 | 确定 | 
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 移动版（在 Windows 预览体验计划计划中）
* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="get-the-crash-dump-collection-settings-for-an-app"></a>获取应用的故障转储集合设置

**请求**

可以通过使用以下请求格式来获取旁加载应用的故障转储集合设置。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/debug/dump/usermode/crashcontrol |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| packageFullname   | （**必需**）旁加载的应用的程序包的完整名称。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应具有以下格式：
```json
{"CrashDumpEnabled": bool}
```

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
|  200 | 确定 | 
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 移动版（在 Windows 预览体验计划计划中）
* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="delete-a-crash-dump-for-a-sideloaded-app"></a>删除旁加载应用的故障转储

**请求**

可以通过使用以下请求格式来删除旁加载应用的故障转储。
 
| 方法      | 请求 URI |
| :------     | :----- |
| DELETE | /api/debug/dump/usermode/crashdump |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :---          | :--- |
| packageFullname   | （**必需**）旁加载的应用的程序包的完整名称。 |
| fileName   | （**必需**）应该删除的转储文件的名称。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
|  200 | 确定 | 
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 移动版（在 Windows 预览体验计划计划中）
* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="disable-crash-dumps-for-a-sideloaded-app"></a>禁用旁加载应用的故障转储

**请求**

可以通过使用以下请求格式来禁用旁加载应用的故障转储。
 
| 方法      | 请求 URI |
| :------     | :----- |
| DELETE | /api/debug/dump/usermode/crashcontrol |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :---          | :--- |
| packageFullname   | （**必需**）旁加载的应用的程序包的完整名称。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
|  200 | 确定 | 
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 移动版（在 Windows 预览体验计划计划中）
* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="download-the-crash-dump-for-a-sideloaded-app"></a>下载旁加载应用的故障转储

**请求**

可以通过使用以下请求格式来下载旁加载应用的故障转储。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/debug/dump/usermode/crashdump |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| packageFullname   | （**必需**）旁加载的应用的程序包的完整名称。 |
| fileName   | （**必需**）要下载的转储文件的名称。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包括转储文件。 可以使用 WinDbg 或 Visual Studio 来检查转储文件。

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
|  200 | 确定 | 
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 移动版（在 Windows 预览体验计划计划中）
* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="enable-crash-dumps-for-a-sideloaded-app"></a>启用旁加载应用的故障转储

**请求**

可以通过使用以下请求格式来启用旁加载应用的故障转储。
 
| 方法      | 请求 URI |
| :------     | :----- |
| 发布 | /api/debug/dump/usermode/crashcontrol |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :---          | :--- |
| packageFullname   | （**必需**）旁加载的应用的程序包的完整名称。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
|  200 | 确定 | 

**可用的设备系列**

* Windows 移动版（在 Windows 预览体验计划计划中）
* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="get-the-list-of-bugcheck-files"></a>获取检测错误文件列表

**请求**

可以通过使用以下请求格式来获取检测错误小型转储文件列表。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/debug/dump/kernel/dumplist |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包括转储文件名称列表和这些文件的大小。 此列表将采用以下格式。 
```json
{"DumpFiles": [
    {
        "FileName": string,
        "FileSize": int
    },...
]}
```

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
|  200 | 确定 | 

**可用的设备系列**

* Windows 桌面版
* IoT

<hr>

### <a name="download-a-bugcheck-dump-file"></a>下载检测错误转储文件

**请求**

可以通过使用以下请求格式来下载检测错误转储文件。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/debug/dump/kernel/dump |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| filename   | （**必需**）转储文件的文件名。 通过使用该 API 获取转储列表，可以找到此文件名。 |


**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包括转储文件。 可以使用 WinDbg 检查此文件。

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
|  200 | 确定 | 
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* IoT

<hr>

### <a name="get-the-bugcheck-crash-control-settings"></a>获取检测错误故障控制设置

**请求**

可以通过使用以下请求格式来获取检测错误故障控制设置。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/debug/dump/kernel/crashcontrol |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包括故障控制设置。 有关 CrashControl 的详细信息，请参阅 [CrashControl](https://technet.microsoft.com/library/cc951703.aspx) 文章。 该响应的模板如下所示。
```json
{
    "autoreboot": bool (0 or 1),
    "dumptype": int (0 to 4),
    "maxdumpcount": int,
    "overwrite": bool (0 or 1)
}
```

**转储类型**

0:Disabled

1：完全内存转储 （收集所有使用中内存）

2：内核内存转储 （忽略用户模式内存）

3：有限的内核小型转储

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
|  200 | 确定 | 
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* IoT

<hr>

### <a name="get-a-live-kernel-dump"></a>获取实时内核转储

**请求**

可以通过使用以下请求格式来获取实时内核转储。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/debug/dump/livekernel |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包括完整内核模式转储。 可以使用 WinDbg 检查此文件。

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
|  200 | 确定 | 
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* IoT

<hr>

### <a name="get-a-dump-from-a-live-user-process"></a>从实时用户进程中获取转储

**请求**

可以通过使用以下请求格式来获取实时用户进程的转储。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/debug/dump/usermode/live |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| pid   | （**必需**）关注的进程的唯一进程 ID。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包括进程转储。 可以使用 WinDbg 或 Visual Studio 检查此文件。

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
|  200 | 确定 | 
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* IoT

<hr>

### <a name="set-the-bugcheck-crash-control-settings"></a>设置检测错误故障控制设置

**请求**

可以通过使用以下请求格式来设置收集检测错误数据的设置。
 
| 方法      | 请求 URI |
| :------     | :----- |
| 发布 | /api/debug/dump/kernel/crashcontrol |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :---          | :--- |
| autoreboot   | （**可选**）True 或 False。 这指示系统在出现故障或锁定后是否自动重新启动。 |
| dumptype   | （**可选**）转储类型。 有关支持的值，请参阅 [CrashDumpType 枚举](https://docs.microsoft.com/previous-versions/azure/reference/dn802457(v=azure.100))。|
| maxdumpcount   | （**可选**）要保存的最大转储数。 |
| overwrite   | （**可选**）True 或 False。 这指示在达到 *maxdumpcount* 指定的转储计数器限制时是否覆盖旧转储。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
|  200 | 确定 | 
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* IoT

<hr>

## <a name="etw"></a>ETW

<hr>

### <a name="create-a-realtime-etw-session-over-a-websocket"></a>通过 WebSocket 创建实时 ETW 会话

**请求**

可以通过使用以下请求格式来创建实时 ETW 会话。 这将通过 WebSocket 进行管理。  ETW 事件会在服务器上进行批处理，并以每秒一次的速度发送到客户端。 
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET/WebSocket | /api/etw/session/realtime |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包括已启用提供程序中的 ETW 事件。  请参阅下面的 ETW WebSocket 命令。 

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
|  200 | 确定 | 
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* IoT

### <a name="etw-websocket-commands"></a>ETW WebSocket 命令
这些命令将从客户端发送到服务器。

| Command | 描述 |
| :----- | :----- |
| 提供程序 *{guid}* 启用 *{level}* | 在指定的级别上启用由 *{guid}* （不带括号）标记的提供程序。 *{level}* 是从 1（最少细节）到 5（详细）的 **int**。 |
| 提供程序 *{guid}* 禁用 | 禁用由 *{guid}* 标记（不带括号）的提供程序。 |

此响应将从服务器发送到客户端。 此响应以文本形式发送，并且你通过解析 JSON 来获取以下格式。
```json
{
    "Events":[
        {
            "Timestamp": int,
            "ProviderName": string,
            "ID": int, 
            "TaskName": string,
            "Keyword": int,
            "Level": int,
            payload objects...
        },...
    ],
    "Frequency": int
}
```

负载对象是在原始 ETW 事件中提供的额外键值对 (string:string)。

例如：
```json
{
    "ID" : 42, 
    "Keyword" : 9223372036854775824, 
    "Level" : 4, 
    "Message" : "UDPv4: 412 bytes transmitted from 10.81.128.148:510 to 132.215.243.34:510. ",
    "PID" : "1218", 
    "ProviderName" : "Microsoft-Windows-Kernel-Network", 
    "TaskName" : "KERNEL_NETWORK_TASK_UDPIP", 
    "Timestamp" : 131039401761757686, 
    "connid" : "0", 
    "daddr" : "132.245.243.34", 
    "dport" : "500", 
    "saddr" : "10.82.128.118", 
    "seqnum" : "0", 
    "size" : "412", 
    "sport" : "500"
}
```

<hr>

### <a name="enumerate-the-registered-etw-providers"></a>枚举已注册的 ETW 提供程序

**请求**

可以通过使用以下请求格式来枚举已注册的提供程序。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/etw/providers |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包括 ETW 提供程序列表。 该列表将包含采用以下格式的每个提供程序的友好名称和 GUID。
```json
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
|  200 | 确定 | 

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="enumerate-the-custom-etw-providers-exposed-by-the-platform"></a>枚举由平台公开的自定义 ETW 提供程序。

**请求**

可以通过使用以下请求格式来枚举已注册的提供程序。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/etw/customproviders |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

200 正常。 该响应包括 ETW 提供程序列表。 该列表将包含每个提供程序的友好名称和 GUID。

```json
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**状态代码**

- 标准状态代码。

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* IoT

<hr>

## <a name="location"></a>Location

<hr>

### <a name="get-location-override-mode"></a>获取位置替代模式

**请求**

可以使用以下请求格式来获取设备的位置堆栈替代状态。 开发人员模式必须处于打开状态才能成功进行此调用。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /ext/location/override |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包含以下格式的设备替代状态。 

```json
{"Override" : bool}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
|  200 | 确定 | 
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

### <a name="set-location-override-mode"></a>设置位置替代模式

**请求**

可以使用以下请求格式来设置设备的位置堆栈替代状态。 启用后，位置堆栈允许执行位置插入。 开发人员模式必须处于打开状态才能成功进行此调用。

| 方法      | 请求 URI |
| :------     | :----- |
| PUT | /ext/location/override |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

```json
{"Override" : bool}
```

**响应**

该响应包含设置为以下格式的设备替代状态。 

```json
{"Override" : bool}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

### <a name="get-the-injected-position"></a>获取插入的位置

**请求**

可以使用以下请求格式来获取设备的插入（伪）位置。 必须设置插入的位置，否则将引发错误。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /ext/location/position |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

响应包含以下格式的当前插入纬度和经度值。 

```json
{
    "Latitude" : double,
    "Longitude" : double
}
```

**状态代码**

此 API 具有以下预期状态代码。

|  HTTP 状态代码      | 描述 | 
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

### <a name="set-the-injected-position"></a>设置插入的位置

**请求**

可以使用以下请求格式来设置设备的插入（伪）位置。 必须首先在设备上启用位置替代模式，并且设置的位置必须是有效位置，否则将引发错误。

| 方法      | 请求 URI |
| :------     | :----- |
| PUT | /ext/location/override |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

```json
{
    "Latitude" : double,
    "Longitude" : double
}
```

**响应**

该响应包含采用以下格式设置的位置。 

```json
{
    "Latitude" : double,
    "Longitude" : double
}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

## <a name="os-information"></a>操作系统信息

<hr>

### <a name="get-the-machine-name"></a>获取计算机名称

**请求**

可以通过使用以下请求格式来获取计算机的名称。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/os/machinename |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包含采用以下格式的计算机名称。 

```json
{"ComputerName": string}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="get-the-operating-system-information"></a>获取操作系统信息

**请求**

可以通过使用以下请求格式来获取计算机的操作系统信息。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/os/info |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包含采用以下格式的操作系统信息。

```json
{
    "ComputerName": string,
    "OsEdition": string,
    "OsEditionId": int,
    "OsVersion": string,
    "Platform": string
}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="get-the-device-family"></a>获取设备系列 

**请求**

可以使用以下请求格式来获取设备系列（Xbox、手机、台式计算机等）。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/os/devicefamily |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

响应包括设备系列（SKU - 台式计算机、Xbox 等）。

```json
{
   "DeviceType" : string
}
```

DeviceType 将类似于“Windows.Xbox”、“Windows.Desktop”等。 

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="set-the-machine-name"></a>设置计算机名称

**请求**

可以通过使用以下请求格式来设置计算机的名称。
 
| 方法      | 请求 URI |
| :------     | :----- |
| 发布 | /api/os/machinename |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| name | （**必需**）计算机的新名称。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

## <a name="user-information"></a>用户信息

<hr>

### <a name="get-the-active-user"></a>获取活动用户

**请求**

可以通过使用以下请求格式来获取设备上的活动用户的名称。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/users/activeuser |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包含采用以下格式的用户信息。 

成功时： 
```json
{
    "UserDisplayName" : string, 
    "UserSID" : string
}
```
失败时：
```json
{
    "Code" : int, 
    "CodeText" : string, 
    "Reason" : string, 
    "Success" : bool
}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* HoloLens
* IoT

<hr>

## <a name="performance-data"></a>性能数据

<hr>

### <a name="get-the-list-of-running-processes"></a>获取正在运行的进程列表

**请求**

可以通过使用以下请求格式来获取当前正在运行的进程列表。  这也可以升级到 WebSocket 连接，其中相同的 WebSocket 数据将以每秒一次的速度推送到客户端。 
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/resourcemanager/processes |
| GET/WebSocket | /api/resourcemanager/processes |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包括进程列表以及每个进程的详细信息。 该信息采用 JSON 格式，并且具有以下模板。
```json
{"Processes": [
    {
        "CPUUsage": float,
        "ImageName": string,
        "PageFileUsage": long,
        "PrivateWorkingSet": long,
        "ProcessId": int,
        "SessionId": int,
        "UserName": string,
        "VirtualSize": long,
        "WorkingSetSize": long
    },...
]}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="get-the-system-performance-statistics"></a>获取系统性能统计信息

**请求**

可以通过使用以下请求格式来获取系统性能统计信息。 这包括诸如读写周期和已使用的内存等信息。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/resourcemanager/systemperf |
| GET/WebSocket | /api/resourcemanager/systemperf |

这还可以升级到 WebSocket 连接。  它以每秒一次的速度提供以下相同的 JSON 数据。 

**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包括 CPU 和 GPU 使用率、内存访问以及网络访问之类的系统性能统计信息。 此信息采用 JSON 格式，并且具有以下模板。
```json
{
    "AvailablePages": int,
    "CommitLimit": int,
    "CommittedPages": int,
    "CpuLoad": int,
    "IOOtherSpeed": int,
    "IOReadSpeed": int,
    "IOWriteSpeed": int,
    "NonPagedPoolPages": int,
    "PageSize": int,
    "PagedPoolPages": int,
    "TotalInstalledInKb": int,
    "TotalPages": int,
    "GPUData": 
    {
        "AvailableAdapters": [{ (One per detected adapter)
            "DedicatedMemory": int,
            "DedicatedMemoryUsed": int,
            "Description": string,
            "SystemMemory": int,
            "SystemMemoryUsed": int,
            "EnginesUtilization": [ float,... (One per detected engine)]
        },...
    ]},
    "NetworkingData": {
        "NetworkInBytes": int,
        "NetworkOutBytes": int
    }
}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

## <a name="power"></a>电源

<hr>

### <a name="get-the-current-battery-state"></a>获取当前的电池状态

**请求**

可以通过使用以下请求格式来获取电池的当前状态。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/power/battery |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

使用以下格式返回当前电池状态信息。
```json
{
    "AcOnline": int (0 | 1),
    "BatteryPresent": int (0 | 1),
    "Charging": int (0 | 1),
    "DefaultAlert1": int,
    "DefaultAlert2": int,
    "EstimatedTime": int,
    "MaximumCapacity": int,
    "RemainingCapacity": int
}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="get-the-active-power-scheme"></a>获取活动的电源方案

**请求**

可以通过使用以下请求格式来获取活动的电源方案。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/power/activecfg |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

活动的电源方案具有以下格式。
```json
{"ActivePowerScheme": string (guid of scheme)}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* IoT

<hr>

### <a name="get-the-sub-value-for-a-power-scheme"></a>获取电源方案的子值

**请求**

可以通过使用以下请求格式来获取电源方案的子值。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/power/cfg/ *<power scheme path>* |

选项：
- SCHEME_CURRENT

**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

可用电源状态的完整列表以应用程序和标志各种电源状态的设置（如电量低和电量严重不足）为基准。 

**响应**

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* IoT

<hr>

### <a name="get-the-power-state-of-the-system"></a>获取系统的电源状态

**请求**

可以通过使用以下请求格式来检查系统的电源状态。 这可让你查看它是否处于低功耗状态。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/power/state |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

电源状态信息具有以下模板。
```json
{"LowPowerState" : false, "LowPowerStateAvailable" : true }
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="set-the-active-power-scheme"></a>设置活动的电源方案

**请求**

可以通过使用以下请求格式来设置活动的电源方案。
 
| 方法      | 请求 URI |
| :------     | :----- |
| 发布 | /api/power/activecfg |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :---          | :--- |
| scheme | （**必需**）要设置为系统的活动电源方案的方案 GUID。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* IoT

<hr>

### <a name="set-the-sub-value-for-a-power-scheme"></a>设置电源方案的子值

**请求**

可以通过使用以下请求格式来设置电源方案的子值。
 
| 方法      | 请求 URI |
| :------     | :----- |
| 发布 | /api/power/cfg/ *<power scheme path>* |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| valueAC | （**必需**）用于交流电源的值。 |
| valueDC | （**必需**）用于电池电源的值。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |

**可用的设备系列**

* Windows 桌面版
* IoT

<hr>

### <a name="get-a-sleep-study-report"></a>获取睡眠分析报告

**请求**

| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/power/sleepstudy/report |

可以通过使用以下请求格式来获取睡眠分析报告。

**URI 参数**
| URI 参数 | 描述 |
| :------          | :------ |
| FileName | （**必需**）要下载的文件的完整名称。 此值应采用 hex64 编码。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应是包含睡眠分析的文件。 

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* IoT

<hr>

### <a name="enumerate-the-available-sleep-study-reports"></a>枚举可用的睡眠分析报告

**请求**

可以通过使用以下请求格式来枚举可用的睡眠分析报告。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/power/sleepstudy/reports |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

可用报告的列表具有以下模板。

```json
{"Reports": [
    {
        "FileName": string
    },...
]}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* IoT

<hr>

### <a name="get-the-sleep-study-transform"></a>获取睡眠分析转换

**请求**

可以通过使用以下请求格式来获取睡眠分析转换。 此转换为 XSLT，可将睡眠分析报告转换为用户可以读取的 XML 格式。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/power/sleepstudy/transform |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包含睡眠研究转换。

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* IoT

<hr>

## <a name="remote-control"></a>遥控器

<hr>

### <a name="restart-the-target-computer"></a>重新启动目标计算机

**请求**

可以通过使用以下请求格式来重新启动目标计算机。
 
| 方法      | 请求 URI |
| :------     | :----- |
| 发布 | /api/control/restart |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="shut-down-the-target-computer"></a>关闭目标计算机

**请求**

可以通过使用以下请求格式来关闭目标计算机。
 
| 方法      | 请求 URI |
| :------     | :----- |
| 发布 | /api/control/shutdown |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

## <a name="task-manager"></a>任务管理器

<hr>

### <a name="start-a-modern-app"></a>启动现代应用

**请求**

可以通过使用以下请求格式来启动现代应用。
 
| 方法      | 请求 URI |
| :------     | :----- |
| 发布 | /api/taskmanager/app |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :---          | :--- |
| appid   | （**必需**）要启动的应用的 PRAID。 此值应采用 hex64 编码。 |
| 包   | （**必需**）要启动的应用包的完整名称。 此值应采用 hex64 编码。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="stop-a-modern-app"></a>停止现代应用

**请求**

可以通过使用以下请求格式来停止现代应用。
 
| 方法      | 请求 URI |
| :------     | :----- |
| DELETE | /api/taskmanager/app |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :---          | :--- |
| 包   | （**必需**）要停止的应用包的完整名称。 此值应采用 hex64 编码。 |
| forcestop   | （**可选**）值为 **yes** 指示系统应强制停止所有进程。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="kill-process-by-pid"></a>终止 PID 进程

**请求**

可以使用以下请求格式终止进程。
 
| 方法      | 请求 URI |
| :------     | :----- |
| DELETE | /api/taskmanager/process |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| pid   | （**必需**）要终止的进程的唯一进程 ID。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* HoloLens
* IoT

<hr>

## <a name="networking"></a>网络

<hr>

### <a name="get-the-current-ip-configuration"></a>获取当前的 IP 配置

**请求**

可以通过使用以下请求格式来获取当前的 IP 配置。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/networking/ipconfig |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

该响应包含采用以下模板的 IP 配置。

```json
{"Adapters": [
    {
        "Description": string,
        "HardwareAddress": string,
        "Index": int,
        "Name": string,
        "Type": string,
        "DHCP": {
            "LeaseExpires": int, (timestamp)
            "LeaseObtained": int, (timestamp)
            "Address": {
                "IpAddress": string,
                "Mask": string
            }
        },
        "WINS": {(WINS is optional)
            "Primary": {
                "IpAddress": string,
                "Mask": string
            },
            "Secondary": {
                "IpAddress": string,
                "Mask": string
            }
        },
        "Gateways": [{ (always 1+)
            "IpAddress": "10.82.128.1",
            "Mask": "255.255.255.255"
            },...
        ],
        "IpAddresses": [{ (always 1+)
            "IpAddress": "10.82.128.148",
            "Mask": "255.255.255.0"
            },...
        ]
    },...
]}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="set-a-static-ip-address-ipv4-configuration"></a>设置静态 IP 地址 （IPV4 配置）

**请求**

设置了静态 IP 和 DNS 的 IPV4 配置。 如果未指定静态 IP，然后它将启用 DHCP。 如果指定静态 IP，则必须还指定 DNS。
 
| 方法      | 请求 URI |
| :------     | :----- |
| PUT | /api/networking/ipv4config |


**URI 参数**

| URI 参数 | 描述 |
| :---          | :--- |
| 适配器名称 | (**需**) 网络接口的 GUID。 |
| IPAddress | 要设置的静态 IP 地址。 |
| SubnetMask | (**必需**如果*IPAddress*不为 null) 的静态子网掩码。 |
| DefaultGateway | (**必需**如果*IPAddress*不为 null) 的静态默认网关。 |
| PrimaryDNS | (**必需**如果*IPAddress*不为 null) 将静态的主 DNS 设置。 |
| SecondayDNS | (**必需**如果*PrimaryDNS*不为 null) 的静态辅助 DNS 设置。 |

为清楚起见，若要设置为 DHCP 的接口，序列化只`AdapterName`在网络上：

```json
{
    "AdapterName":"{82F86C1B-2BAE-41E3-B08D-786CA44FEED7}"
}
```

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="enumerate-wireless-network-interfaces"></a>枚举无线网络接口

**请求**

可以通过使用以下请求格式来枚举可用的无线网络接口。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/wifi/interfaces |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

采用以下格式的可用无线接口列表以及详细信息。

```json 
{"Interfaces": [{
    "Description": string,
    "GUID": string (guid with curly brackets),
    "Index": int,
    "ProfilesList": [
        {
            "GroupPolicyProfile": bool,
            "Name": string, (Network currently connected to)
            "PerUserProfile": bool
        },...
    ]
    }
]}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="enumerate-wireless-networks"></a>枚举无线网络

**请求**

可以通过使用以下请求格式来枚举指定接口上的无线网络列表。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/wifi/networks |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| 接口   | （**必需**）用于搜索无线网络的网络接口的 GUID，不带括号。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

在提供的*接口*上找到的无线网络列表。 这包括采用以下格式的网络详细信息。

```json
{"AvailableNetworks": [
    {
        "AlreadyConnected": bool,
        "AuthenticationAlgorithm": string, (WPA2, etc)
        "Channel": int,
        "CipherAlgorithm": string, (e.g. AES)
        "Connectable": int, (0 | 1)
        "InfrastructureType": string,
        "ProfileAvailable": bool,
        "ProfileName": string,
        "SSID": string,
        "SecurityEnabled": int, (0 | 1)
        "SignalQuality": int,
        "BSSID": [int,...],
        "PhysicalTypes": [string,...]
    },...
]}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="connect-and-disconnect-to-a-wi-fi-network"></a>建立和断开与 WLAN 网络的连接。

**请求**

可以通过使用以下请求格式来建立或断开与 WLAN 网络的连接。
 
| 方法      | 请求 URI |
| :------     | :----- |
| 发布 | /api/wifi/network |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| 接口   | （**必需**）用于连接到网络的网络接口的 GUID。 |
| op   | （**必需**）指示要执行的操作。 可能的值为 connect 或 disconnect。|
| ssid   | （**如果 *op* == connect，则为必需项**）要连接到的 SSID。 |
| 键   | （**如果 *op* == connect 并且网络需要身份验证，则为必需项**）共享的密钥。 |
| createprofile | （**必需**）在设备上为网络创建配置文件。  这将导致设备在将来自动连接到该网络。 这可以是**是**或**否**。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="delete-a-wi-fi-profile"></a>删除 WLAN 配置文件

**请求**

可以通过使用以下请求格式来删除与特定接口上的网络关联的配置文件。
 
| 方法      | 请求 URI |
| :------     | :----- |
| DELETE | /api/wifi/profile |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| 接口   | （**必需**）与要删除的配置文件关联的网络接口的 GUID。 |
| profile   | （**必需**）要删除的配置文件名称。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

## <a name="windows-error-reporting-wer"></a>Windows 错误报告 (WER)

<hr>

### <a name="download-a-windows-error-reporting-wer-file"></a>下载 Windows 错误报告 (WER) 文件

**请求**

可以通过使用以下请求格式来下载 WER 相关的文件。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/wer/report/file |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| 用户   | （**必需**）与报告关联的用户名。 |
| type   | （**必需**）报告的类型。 这可以是 **queried**，也可以是 **archived**。 |
| name   | （**必需**）报告的名称。 这应采用 base64 编码。 |
| 文件   | （**必需**）要从报告下载的文件名称。 这应采用 base64 编码。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 响应包含所请求的文件。 

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="enumerate-files-in-a-windows-error-reporting-wer-report"></a>枚举 Windows 错误报告 (WER) 报告中的文件

**请求**

可以通过使用以下请求格式来枚举 WER 报告中的文件。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/wer/report/files |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| 用户   | （**必需**）与报告关联的用户。 |
| type   | （**必需**）报告的类型。 这可以是 **queried**，也可以是 **archived**。 |
| name   | （**必需**）报告的名称。 这应采用 base64 编码。 |

**请求标头**

- 无

**请求正文**

```json
{"Files": [
    {
        "Name": string, (Filename, not base64 encoded)
        "Size": int (bytes)
    },...
]}
```

**响应**

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="list-the-windows-error-reporting-wer-reports"></a>列出 Windows 错误报告 (WER) 报告

**请求**

可以通过使用以下请求格式来获取 WER 报告。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/wer/reports |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

采用以下格式的 WER 报告。

```json
{"WerReports": [
    {
        "User": string,
        "Reports": [
            {
                "CreationTime": int,
                "Name": string, (not base64 encoded)
                "Type": string ("Queue" or "Archive")
            },
    },...
]}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows 桌面版
* HoloLens
* IoT

<hr>

## <a name="windows-performance-recorder-wpr"></a>Windows Performance Recorder (WPR) 

<hr>

### <a name="start-tracing-with-a-custom-profile"></a>使用自定义配置文件开始跟踪

**请求**

可以通过使用以下请求格式来上载 WPR 配置文件，并使用该配置文件开始跟踪。  一次只能运行一个跟踪。 配置文件将不保留在设备上。 
 
| 方法      | 请求 URI |
| :------     | :----- |
| 发布 | /api/wpr/customtrace |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 包含自定义的 WPR 配置文件的多部分一致 http 正文。

**响应**

采用以下格式的 WPR 会话状态。

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="start-a-boot-performance-tracing-session"></a>开始启动性能跟踪会话

**请求**

可以通过使用以下请求格式来开始启动 WPR 跟踪会话。 这也称为性能跟踪会话。
 
| 方法      | 请求 URI |
| :------     | :----- |
| 发布 | /api/wpr/boottrace |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| profile   | （**必需**）开始时必须使用此参数。 开始性能跟踪会话应使用的配置文件的名称。 可能的配置文件存储在 perfprofiles/profiles.json 中。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

开始时，此 API 会返回采用以下格式的 WPR 会话状态。

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (boot)
}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="stop-a-boot-performance-tracing-session"></a>停止启动性能跟踪会话

**请求**

可以通过使用以下请求格式来停止启动 WPR 跟踪会话。 这也称为性能跟踪会话。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/wpr/boottrace |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

-  无。  **注意：** 这是长时间运行操作。  当 ETL 完成写入到磁盘时，它将返回。

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="start-a-performance-tracing-session"></a>开始性能跟踪会话

**请求**

可以通过使用以下请求格式来开始 WPR 跟踪会话。 这也称为性能跟踪会话。  一次只能运行一个跟踪。 
 
| 方法      | 请求 URI |
| :------     | :----- |
| 发布 | /api/wpr/trace |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| profile   | （**必需**）开始性能跟踪会话应使用的配置文件的名称。 可能的配置文件存储在 perfprofiles/profiles.json 中。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

开始时，此 API 会返回采用以下格式的 WPR 会话状态。

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal)
}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="stop-a-performance-tracing-session"></a>停止性能跟踪会话

**请求**

可以通过使用以下请求格式来停止 WPR 跟踪会话。 这也称为性能跟踪会话。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/wpr/trace |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无。  **注意：** 这是长时间运行操作。  当 ETL 完成写入到磁盘时，它将返回。  

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="retrieve-the-status-of-a-tracing-session"></a>检索跟踪会话的状态

**请求**

可以通过使用以下请求格式来检索当前 WPR 会话的状态。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/wpr/status |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

采用以下格式的 WPR 跟踪会话的状态。

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="list-completed-tracing-sessions-etls"></a>列出已完成的跟踪会话 (ETL)

**请求**

可以使用以下请求格式来获取设备上的 ETL 跟踪列表。 

| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/wpr/tracefiles |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

将采用以下格式提供已完成的跟踪会话列表。

```json
{"Items": [{
    "CurrentDir": string (filepath),
    "DateCreated": int (File CreationTime),
    "FileSize": int (bytes),
    "Id": string (filename),
    "Name": string (filename),
    "SubPath": string (filepath),
    "Type": int
}]}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="download-a-tracing-session-etl"></a>下载跟踪会话 (ETL)

**请求**

可以使用以下请求格式来下载跟踪文件（启动跟踪或用户模式跟踪）。 

| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/wpr/tracefile |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| filename   | （**必需**）要下载的 ETL 跟踪的名称。  可以在 /api/wpr/tracefiles 中找到它们 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 返回跟踪 ETL 文件。

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* IoT

<hr>

### <a name="delete-a-tracing-session-etl"></a>删除跟踪会话 (ETL)

**请求**

可以使用以下请求格式来删除跟踪文件（启动跟踪或用户模式跟踪）。 

| 方法      | 请求 URI |
| :------     | :----- |
| DELETE | /api/wpr/tracefile |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 描述 |
| :------          | :------ |
| filename   | （**必需**）要删除的 ETL 跟踪的名称。  可以在 /api/wpr/tracefiles 中找到它们 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 返回跟踪 ETL 文件。

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* IoT

<hr>

## <a name="dns-sd-tags"></a>DNS-SD 标记 

<hr>

### <a name="view-tags"></a>查看标记

**请求**

查看当前应用的设备标记。  这些标记通过 T 项中的 DNS-SD TXT 记录公布。  
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/dns-sd/tags |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应** 采用以下格式的当前应用的标记。 
```json
 {
    "tags": [
        "tag1", 
        "tag2", 
        ...
     ]
}
```

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 5XX | 服务器错误 |


**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="delete-tags"></a>删除标记

**请求**

删除当前由 DNS-SD 公布的所有标记。   
 
| 方法      | 请求 URI |
| :------     | :----- |
| DELETE | /api/dns-sd/tags |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**
 - 无

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 5XX | 服务器错误 |


**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

<hr>

### <a name="delete-tag"></a>删除标记

**请求**

删除当前由 DNS-SD 公布的标记。   
 
| 方法      | 请求 URI |
| :------     | :----- |
| DELETE | /api/dns-sd/tag |


**URI 参数**

| URI 参数 | 描述 |
| :------     | :----- |
| tagValue | （**必需**）要删除的标记。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**
 - 无

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |


**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT
 
<hr>

### <a name="add-a-tag"></a>添加标记

**请求**

将标记添加到 DNS-SD 广告。   
 
| 方法      | 请求 URI |
| :------     | :----- |
| 发布 | /api/dns-sd/tag |


**URI 参数**

| URI 参数 | 描述 |
| :------     | :----- |
| tagValue | （**必需**）要添加的标记。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**
 - 无

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 401 | 标记空间溢出。  当建议的标记对于生成的 DNS-SD 服务记录而言过长时，将出现此情形。 |


**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* Xbox
* HoloLens
* IoT

## <a name="app-file-explorer"></a>应用文件资源管理器

<hr>

### <a name="get-known-folders"></a>获取已知文件夹

**请求**

获取可访问的顶级文件夹列表。

| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/filesystem/apps/knownfolders |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应** 采用以下格式的可用文件夹。 
```json
 {"KnownFolders": [
    "folder0",
    "folder1",...
]}
```
**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 已接受和正在处理的部署请求 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |


**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* Xbox
* IoT

<hr>

### <a name="get-files"></a>获取文件

**请求**

获取文件夹中的文件列表。

| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/filesystem/apps/files |


**URI 参数**

| URI 参数 | 描述 |
| :------     | :----- |
| knownfolderid | （**必需**）要获取文件列表的顶级目录。 将 **LocalAppData** 用于访问旁加载的应用。 |
| packagefullname | （**如果 *knownfolderid* == LocalAppData，则为必需项**）你感兴趣的应用的程序包全名。 |
| path | （**可选**）上面指定的文件夹或程序包内的子目录。 |

**请求标头**

- 无

**请求正文**

- 无

**响应** 采用以下格式的可用文件夹。 
```json
{"Items": [
    {
        "CurrentDir": string (folder under the requested known folder),
        "DateCreated": int,
        "FileSize": int (bytes),
        "Id": string,
        "Name": string,
        "SubPath": string (present if this item is a folder, this is the name of the folder),
        "Type": int
    },...
]}
```
**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* Xbox
* IoT

<hr>

### <a name="download-a-file"></a>下载文件

**请求**

从已知文件夹或 appLocalData 中获取文件。

| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/filesystem/apps/file |

**URI 参数**

| URI 参数 | 描述 |
| :------     | :----- |
| knownfolderid | （**必需**）要下载文件的顶级目录。 将 **LocalAppData** 用于访问旁加载的应用。 |
| filename | （**必需**）要下载的文件名称。 |
| packagefullname | （**如果 *knownfolderid* == LocalAppData，则为必需项**）你感兴趣的程序包全名。 |
| path | （**可选**）上面指定的文件夹或程序包内的子目录。 |

**请求标头**

- 无

**请求正文**

- 请求的文件（如果存在）

**响应**

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 请求的文件 |
| 404 | 找不到文件 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* Xbox
* IoT

<hr>

### <a name="rename-a-file"></a>重命名文件

**请求**

重命名文件夹中的文件。

| 方法      | 请求 URI |
| :------     | :----- |
| 发布 | /api/filesystem/apps/rename |


**URI 参数**

| URI 参数 | 描述 |
| :------     | :----- |
| knownfolderid | （**必需**）文件所在的顶级目录。 将 **LocalAppData** 用于访问旁加载的应用。 |
| filename | （**必需**）要重命名的文件的原始名称。 |
| newfilename | （**必需**）文件的新名称。|
| packagefullname | （**如果 *knownfolderid* == LocalAppData，则为必需项**）你感兴趣的应用的程序包全名。 |
| path | （**可选**）上面指定的文件夹或程序包内的子目录。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |. 文件已重命名
| 404 | 找不到文件 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* Xbox
* IoT

<hr>

### <a name="delete-a-file"></a>删除文件

**请求**

删除文件夹中的文件。

| 方法      | 请求 URI |
| :------     | :----- |
| DELETE | /api/filesystem/apps/file |

**URI 参数**

| URI 参数 | 描述 |
| :------     | :----- |
| knownfolderid | （**必需**）要删除文件的顶级目录。 将 **LocalAppData** 用于访问旁加载的应用。 |
| filename | （**必需**）要删除的文件名称。 |
| packagefullname | （**如果 *knownfolderid* == LocalAppData，则为必需项**）你感兴趣的应用的程序包全名。 |
| path | （**可选**）上面指定的文件夹或程序包内的子目录。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无 

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |. 文件已删除 |
| 404 | 找不到文件 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* Xbox
* IoT

<hr>

### <a name="upload-a-file"></a>上载文件

**请求**

将文件上载到文件夹。  这将覆盖具有相同名称的现有文件，但不会创建新的文件夹。 

| 方法      | 请求 URI |
| :------     | :----- |
| 发布 | /api/filesystem/apps/file |

**URI 参数**

| URI 参数 | 描述 |
| :------     | :----- |
| knownfolderid | （**必需**）要上载文件的顶级目录。 将 **LocalAppData** 用于访问旁加载的应用。 |
| packagefullname | （**如果 *knownfolderid* == LocalAppData，则为必需项**）你感兴趣的应用的程序包全名。 |
| path | （**可选**）上面指定的文件夹或程序包内的子目录。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码      | 描述 |
| :------     | :----- |
| 200 | 确定 |. 文件已上载 |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用的设备系列**

* Windows Mobile
* Windows 桌面版
* HoloLens
* Xbox
* IoT
