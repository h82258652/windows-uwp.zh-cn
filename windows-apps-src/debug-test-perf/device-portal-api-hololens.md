---
ms.assetid: 41ac0142-4d86-4bb3-b580-36d0d6956091
title: 适用于 HoloLens 的 Device Portal API 参考
description: 了解适用于 HoloLens 的 Windows Device Portal REST API，该 API 可用于访问数据和以编程方式控制设备。
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, 设备门户
ms.localizationpriority: medium
ms.openlocfilehash: 3aeb068908adf6d6c40a50cee3aececba1861ee8
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "63801391"
---
# <a name="device-portal-api-reference-for-hololens"></a>适用于 HoloLens 的 Device Portal API 参考

Windows Device Portal 中的所有内容都是基于 REST API 创建的，该 API 可用于访问数据和以编程方式控制设备。

## <a name="holographic-os"></a>全息操作系统

### <a name="get-https-requirements-for-the-device-portal"></a>获取 Device Portal 的 HTTPS 要求

**请求**

可以通过使用以下请求格式来获取 Device Portal 的 HTTPS 要求。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/os/webmanagement/settings/https |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="get-the-stored-interpupillary-distance-ipd"></a>获取已存储的瞳孔间距 (IPD)

**请求**

可以通过使用以下请求格式来获取已存储的 IPD 值。 值将以毫米为单位返回。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/os/settings/ipd |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="get-a-list-of-hololens-specific-etw-providers"></a>获取 HoloLens 特定的 ETW 提供程序列表

**请求**

可以通过使用以下请求格式来获取未向系统注册的 HoloLens 特定的 ETW 提供程序列表。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/os/etw/customproviders |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。


### <a name="return-the-state-for-all-active-services"></a>返回所有活动服务的状态

**请求**

可以通过使用以下请求格式来获取当前正在运行的所有服务的状态。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/os/services |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。


### <a name="set-the-https-requirement-for-the-device-portal"></a>设置 Device Portal 的 HTTPS 要求。

**请求**

可以通过使用以下请求格式来设置 Device Portal 的 HTTPS 要求。
 
| 方法      | 请求 URI |
| :------     | :----- |
| POST | /api/holographic/management/settings/https |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| 必需的   | （**必需**）确定 Device Portal 是否需要 HTTPS。 可能的值包括 **yes**、**no** 和 **default**。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。


### <a name="set-the-interpupillary-distance-ipd"></a>设置瞳孔间距 (IPD)

**请求**

可以通过使用以下请求格式来设置已存储的 IPD。
 
| 方法      | 请求 URI |
| :------     | :----- |
| POST | /api/holographic/os/settings/ipd |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| ipd   | （**必需**）要存储的新 IPD 值。 此值应以毫米为单位。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。


## <a name="holographic-perception"></a>全息感知

### <a name="accept-websocket-upgrades-and-run-a-mirage-client-that-sends-updates"></a>接受 WebSocket 升级并运行发送更新的虚拟客户端

**请求**

可以通过使用以下请求格式来接受 WebSocket 升级，并运行以 30 帧/秒的速度发送更新的虚拟客户端。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET/WebSocket | /api/holographic/perception/client |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| clientmode   | （**必需**）确定跟踪模式。 视觉跟踪模式无法被动建立时，值为 **active** 会强制执行该模式。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。


## <a name="holographic-thermal"></a>全息热

### <a name="get-the-thermal-stage-of-the-device"></a>获取设备的热阶段

**请求**

可以通过使用以下请求格式来获取设备的热阶段。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/ |

**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

可能的值如下表中所示。

| 值 | 说明 |
| --- | --- |
| 1 | 正常 |
| 2 | 暧 |
| 3 | 关键 |

**状态代码**

- 标准状态代码。

## <a name="hsimulation-control"></a>HSimulation 控制
### <a name="create-a-control-stream-or-post-data-to-a-created-stream"></a>创建控制流或将数据发布到已创建的流

**请求**

可以通过使用以下请求格式来创建控制流或将数据发布到已创建的流。 发布的数据应该是 **application/octet-stream** 类型。
 
| 方法      | 请求 URI |
| :------     | :----- |
| POST | /api/holographic/simulation/control/stream |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| priority   | （**必需，如果要创建控制流**）指示该流的优先级。 |
| streamid   | （**必需，如果要发布到已创建的流**）要发布到的流的标识符。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="delete-a-control-stream"></a>删除控制流

**请求**

可以通过使用以下请求格式来删除控制流。
 
| 方法      | 请求 URI |
| :------     | :----- |
| DELETE | /api/holographic/simulation/control/stream |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="get-a-control-stream"></a>获取控制流

**请求**

可以通过使用以下请求格式来打开用于控制流的 Web 套接字连接。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET/WebSocket | /api/holographic/simulation/control/stream |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="get-the-simulation-mode"></a>获取模拟模式

**请求**

可以通过使用以下请求格式来获取模拟模式。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/simulation/control/mode |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="set-the-simulation-mode"></a>设置模拟模式

**请求**

可以通过使用以下请求格式来设置模拟模式。
 
| 方法      | 请求 URI |
| :------     | :----- |
| POST | /api/holographic/simluation/control/mode |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| mode   | （**必需**）指示模拟模式。 可能的值包括 **default**、**simulation**、**remote** 和 **legacy**。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

## <a name="hsimulation-playback"></a>HSimulation 播放

### <a name="delete-a-recording"></a>删除录制

**请求**

可以通过使用以下请求格式来删除录制。
 
| 方法      | 请求 URI |
| :------     | :----- |
| DELETE | /api/holographic/simulation/playback/file |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| 记录   | （**必需**）要删除的录制的名称。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="get-all-recordings"></a>获取所有录制

**请求**

可以通过使用以下请求格式来获取所有可用录制。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/simulation/playback/files |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="get-the-types-of-data-in-a-loaded-recording"></a>获取已加载录制中的数据类型

**请求**

可以通过使用以下请求格式来获取已加载录制中的数据类型。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/simulation/playback/session/types |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| 记录   | （**必需**）感兴趣的录制的名称。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="get-all-the-loaded-recordings"></a>获取所有已加载的录制

**请求**

可以通过使用以下请求格式来获取所有已加载的录制。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/simulation/playback/session/files |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="get-the-current-playback-state-of-a-recording"></a>获取录制的当前播放状态 

**请求**

可以通过使用以下请求格式来获取录制的当前播放状态。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/simulation/playback/session |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| 记录   | （**必需**）感兴趣的录制的名称。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="load-a-recording"></a>加载录制

**请求**

可以通过使用以下请求格式来加载录制。
 
| 方法      | 请求 URI |
| :------     | :----- |
| POST | /api/holographic/simulation/playback/session/file |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| 记录   | （**必需**）要加载的录制的名称。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="pause-a-recording"></a>暂停录制

**请求**

可以通过使用以下请求格式来暂停录制。
 
| 方法      | 请求 URI |
| :------     | :----- |
| POST | /api/holographic/simulation/playback/session/pause |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| 记录   | （**必需**）要暂停的录制的名称。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="play-a-recording"></a>播放录制

**请求**

可以通过使用以下请求格式来播放录制。
 
| 方法      | 请求 URI |
| :------     | :----- |
| POST | /api/holographic/simulation/playback/session/play |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| 记录   | （**必需**）要播放的录制的名称。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="stop-a-recording"></a>停止录制

**请求**

可以通过使用以下请求格式来停止录制。
 
| 方法      | 请求 URI |
| :------     | :----- |
| POST | /api/holographic/simulation/playback/session/stop |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| 记录   | （**必需**）要停止的录制的名称。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="unload-a-recording"></a>卸载录制

**请求**

可以通过使用以下请求格式来卸载录制。
 
| 方法      | 请求 URI |
| :------     | :----- |
| DELETE | /api/holographic/simulation/playback/session/file |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| 记录   | （**必需**）要卸载的录制的名称。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="upload-a-recording"></a>上载录制

**请求**

可以通过使用以下请求格式来上载录制。
 
| 方法      | 请求 URI |
| :------     | :----- |
| POST | /api/holographic/simulation/playback/file |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

## <a name="hsimulation-recording"></a>HSimulation 录制

### <a name="get-the-recording-state"></a>获取录制状态

**请求**

可以通过使用以下请求格式来获取当前的录制状态。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/simulation/recording/status |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="start-a-recording"></a>开始录制

**请求**

可以通过使用以下请求格式来开始录制。 一次仅可以存在一个活动录制。 
 
| 方法      | 请求 URI |
| :------     | :----- |
| POST | /api/holographic/simulation/recording/start |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| head   | （**如下所示**）将此值设置为 1，以指示系统应记录头数据。 |
| hands   | （**如下所示**）将此值设置为 1，以指示系统应记录手数据。 |
| spatialMapping   | （**如下所示**）将此值设置为 1，以指示系统应记录空间映射数据。 |
| environment   | （**如下所示**）将此值设置为 1，以指示系统应记录环境数据。 |
| name   | （**必需**）录制的名称。 |
| singleSpatialMappingFrame   | （**可选**）将此值设置为 1，以指示应只记录单个空间映射框架。 |

对于这些参数，必须将以下参数之一设置为 1：*head*、*hands*、*spatialMapping* 或 *environment*。

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="stop-the-current-recording"></a>停止当前录制

**请求**

可以通过使用以下请求格式来停止当前录制。 录制将以文件的形式返回。
 
| 方法      | 请求 URI |
| :------     | :----- |
| POST | /api/holographic/simulation/recording/stop |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

## <a name="mixed-reality-capture"></a>混合现实捕获

### <a name="delete-a-mixed-reality-capture-mrc-recording-from-the-device"></a>从设备中删除混合现实捕获 (MRC) 录制

**请求**

可以通过使用以下请求格式来删除 MRC 录制。
 
| 方法      | 请求 URI |
| :------     | :----- |
DELETE | /api/holographic/mrc/file |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| filename   | （**必需**）要删除的视频文件名称。 该名称应采用 hex64 编码。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="download-a-mixed-reality-capture-mrc-file"></a>下载混合现实捕获 (MRC) 文件

**请求**

可以通过使用以下请求格式来从设备下载 MRC 文件。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/mrc/file |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| filename   | （**必需**）要获取的视频文件名称。 该名称应采用 hex64 编码。 |
| op   | （**可选**）如果要下载一个流，请将此值设置为 **stream**。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="get-the-mixed-reality-capture-mrc-settings"></a>获取混合现实捕获 (MRC) 设置

**请求**

可以通过使用以下请求格式来获取 MRC 设置。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/mrc/settings |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="get-the-status-of-the-mixed-reality-capture-mrc-recording"></a>获取混合现实捕获 (MRC) 录制的状态

**请求**

可以通过使用以下请求格式来获取 MRC 录制状态。 可能的值包括 **running** 和 **stopped**。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/mrc/status |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="get-the-list-of-mixed-reality-capture-mrc-files"></a>获取混合现实捕获 (MRC) 文件列表

**请求**

可以通过使用以下请求格式来获取已存储在设备上的 MRC 文件。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/mrc/files |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="set-the-mixed-reality-capture-mrc-settings"></a>设置混合现实捕获 (MRC) 设置

**请求**

可以通过使用以下请求格式来设置 MRC 设置。
 
| 方法      | 请求 URI |
| :------     | :----- |
| POST | /api/holographic/mrc/settings |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="starts-a-mixed-reality-capture-mrc-recording"></a>开始混合现实捕获 (MRC) 录制

**请求**

可以通过使用以下请求格式来开始 MRC 录制。
 
| 方法      | 请求 URI |
| :------     | :----- |
| POST | /api/holographic/mrc/video/control/start |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="stop-the-current-mixed-reality-capture-mrc-recording"></a>停止当前混合现实捕获 (MRC) 录制

**请求**

可以通过使用以下请求格式来停止当前 MRC 录制。
 
| 方法      | 请求 URI |
| :------     | :----- |
| POST | /api/holographic/mrc/video/control/stop |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="take-a-mixed-reality-capture-mrc-photo"></a>拍摄混合现实捕获 (MRC) 照片

**请求**

可以通过使用以下请求格式来拍摄 MRC 照片。 照片将以 JPEG 格式返回。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/mrc/photo |


**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

## <a name="mixed-reality-streaming"></a>混合现实流

### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>启动碎片 mp4 的分块下载

**请求**

可以通过使用以下请求格式来启动碎片 mp4 的分块下载。 此 API 使用的是默认质量。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/stream/live.mp4 |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| pv   | （**可选**）指示是否捕获 PV 相机。 应为 **true** 或 **false**。 |
| holo   | （**可选**）指示是否捕获全息图。 应为 **true** 或 **false**。 |
| mic   | （**可选**）指示是否捕获麦克风。 应为 **true** 或 **false**。 |
| loopback   | （**可选**）指示是否捕获应用程序音频。 应为 **true** 或 **false**。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>启动碎片 mp4 的分块下载

**请求**

可以通过使用以下请求格式来启动碎片 mp4 的分块下载。 此 API 使用的是高质量。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/stream/live_high.mp4 |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| pv   | （**可选**）指示是否捕获 PV 相机。 应为 **true** 或 **false**。 |
| holo   | （**可选**）指示是否捕获全息图。 应为 **true** 或 **false**。 |
| mic   | （**可选**）指示是否捕获麦克风。 应为 **true** 或 **false**。 |
| loopback   | （**可选**）指示是否捕获应用程序音频。 应为 **true** 或 **false**。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>启动碎片 mp4 的分块下载

**请求**

可以通过使用以下请求格式来启动碎片 mp4 的分块下载。 此 API 使用的是低质量。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/stream/live_low.mp4 |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| pv   | （**可选**）指示是否捕获 PV 相机。 应为 **true** 或 **false**。 |
| holo   | （**可选**）指示是否捕获全息图。 应为 **true** 或 **false**。 |
| mic   | （**可选**）指示是否捕获麦克风。 应为 **true** 或 **false**。 |
| loopback   | （**可选**）指示是否捕获应用程序音频。 应为 **true** 或 **false**。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。

### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>启动碎片 mp4 的分块下载

**请求**

可以通过使用以下请求格式来启动碎片 mp4 的分块下载。 此 API 使用的是中等质量。
 
| 方法      | 请求 URI |
| :------     | :----- |
| GET | /api/holographic/stream/live_med.mp4 |


**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数 | 说明 |
| :---          | :--- |
| pv   | （**可选**）指示是否捕获 PV 相机。 应为 **true** 或 **false**。 |
| holo   | （**可选**）指示是否捕获全息图。 应为 **true** 或 **false**。 |
| mic   | （**可选**）指示是否捕获麦克风。 应为 **true** 或 **false**。 |
| loopback   | （**可选**）指示是否捕获应用程序音频。 应为 **true** 或 **false**。 |

**请求标头**

- 无

**请求正文**

- 无

**响应**

- 无

**状态代码**

- 标准状态代码。
