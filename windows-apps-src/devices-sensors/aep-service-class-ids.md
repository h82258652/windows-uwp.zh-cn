---
author: muhsinking
ms.assetid: 23001DA5-C099-4C02-ACE9-3597F06ECBF4
title: AEP 服务类 ID
description: 关联终结点 (AEP) 服务为设备通过给定协议支持的服务提供编程合约。 其中多个服务具有现成的标识符，应在引用它们时使用。
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5f103ee3c281ca95abcaee76cdc6f88b74a49eb1
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6201562"
---
# <a name="aep-service-class-ids"></a>AEP 服务类 ID



**重要的 API**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

关联终结点 (AEP) 服务为设备通过给定协议支持的服务提供编程合约。 其中多个服务具有现成的标识符，应在引用它们时使用。 这些合约通过 **System.Devices.AepService.ServiceClassId** 属性标识。 本主题列出了几个众所周知的 AEP 服务类 ID。 AEP 服务类 ID 也可应用于具有客户类 ID 的协议。

应用开发人员应使用基于这些类 ID 的高级查询语法 (AQS) 筛选器来将其查询限制于其打算使用的 AEP 服务。 这既会将查询限制于相关服务，又会显著提高设备的性能、电池寿命和服务质量。 例如，应用程序可以通过这些服务类 ID 将设备用作 Miracast 同步或 DLNA 苏自媒体呈现器 (DMR)。 有关其他设备和服务彼此如何交互的详细信息，请参阅[**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)。

## <a name="bluetooth-and-bluetooth-le-services"></a>蓝牙和蓝牙 LE 服务

蓝牙服务属于蓝牙协议或蓝牙 LE 协议这两种协议之一。 这些协议的标识符是：

-   蓝牙协议 ID：{e0cbf06c-cd8b-4647-bb8a-263b43f0f974}
-   蓝牙 LE 协议 ID：{bb7bb05e-5972-42b5-94fc-76eaa7084d49}

蓝牙协议支持多个服务，它们全都遵循相同的基本格式。 GUID 的前四个数字因服务而异，但所有蓝牙 GUID 都以 **0000-0000-1000-8000-00805F9B34FB** 结尾。 例如，RFCOMM 服务开头含有 0x0003，因此完整的 ID 会是 **00030000-0000-1000-8000-00805F9B34FB**。 下表列出了一些常见的蓝牙服务。

| 服务名称                         | GUID                                     |
|--------------------------------------|------------------------------------------|
| RFCOMM                               | **00030000-0000-1000-8000-00805F9B34FB** |
| GATT - 警报通知服务    | **18110000-0000-1000-8000-00805F9B34FB** |
| GATT - 自动化 IO                 | **18150000-0000-1000-8000-00805F9B34FB** |
| GATT - 电池服务               | **180F0000-0000-1000-8000-00805F9B34FB** |
| GATT - 血压                | **18100000-0000-1000-8000-00805F9B34FB** |
| GATT - 身体组成              | **181B0000-0000-1000-8000-00805F9B34FB** |
| GATT - 证券管理               | **181E0000-0000-1000-8000-00805F9B34FB** |
| GATT - 连续血糖监测 | **181F0000-0000-1000-8000-00805F9B34FB** |
| GATT - 当前时间服务          | **18050000-0000-1000-8000-00805F9B34FB** |
| GATT - 骑行功率                 | **18180000-0000-1000-8000-00805F9B34FB** |
| GATT - 骑行速度和节奏     | **18160000-0000-1000-8000-00805F9B34FB** |
| GATT - 设备信息            | **180A0000-0000-1000-8000-00805F9B34FB** |
| GATT - 环境感知         | **181A0000-0000-1000-8000-00805F9B34FB** |
| GATT - 通用访问                | **18000000-0000-1000-8000-00805F9B34FB** |
| GATT - 通用属性             | **18010000-0000-1000-8000-00805F9B34FB** |
| GATT - 血糖                       | **18080000-0000-1000-8000-00805F9B34FB** |
| GATT - 健康温度计            | **18090000-0000-1000-8000-00805F9B34FB** |
| GATT - 心率                    | **180D0000-0000-1000-8000-00805F9B34FB** |
| GATT - 人机接口设备        | **18120000-0000-1000-8000-00805F9B34FB** |
| GATT - 即时警报               | **18020000-0000-1000-8000-00805F9B34FB** |
| GATT - 室内定位            | **18210000-0000-1000-8000-00805F9B34FB** |
| GATT - Internet 协议支持     | **18200000-0000-1000-8000-00805F9B34FB** |
| GATT - 链接丢失                     | **18030000-0000-1000-8000-00805F9B34FB** |
| GATT - 位置和导航       | **18190000-0000-1000-8000-00805F9B34FB** |
| GATT - 下一个夏时制更改服务       | **18070000-0000-1000-8000-00805F9B34FB** |
| GATT - 手机警报状态服务    | **180E0000-0000-1000-8000-00805F9B34FB** |
| GATT - 脉动血氧计                | **18220000-0000-1000-8000-00805F9B34FB** |
| GATT - 参考时间更新服务 | **18060000-0000-1000-8000-00805F9B34FB** |
| GATT - 跑步速度和节奏     | **18140000-0000-1000-8000-00805F9B34FB** |
| GATT - 扫描参数               | **18130000-0000-1000-8000-00805F9B34FB** |
| GATT - Tx 电源                      | **18040000-0000-1000-8000-00805F9B34FB** |
| GATT - 用户数据                     | **181C0000-0000-1000-8000-00805F9B34FB** |
| GATT - 体重秤                  | **181D0000-0000-1000-8000-00805F9B34FB** |

 

有关可用蓝牙服务的更完整列表，请参阅[此处](http://go.microsoft.com/fwlink/p/?LinkID=619586)和[此处](http://go.microsoft.com/fwlink/p/?LinkID=619587)的蓝牙协议和服务页面。 你还可以使用 [**GattServiceUuids**](https://msdn.microsoft.com/library/windows/apps/Dn297571) API 获取一些常见的 GATT 服务。

## <a name="custom-bluetooth-le-services"></a>自定义蓝牙 LE 服务

自定义蓝牙 LE 服务使用以下协议标识符：{bb7bb05e-5972-42b5-94fc76eaa7084d49}

自定义配置文件通过其各自定义的 GUID 来定义。 此自定义 GUID 应该用于 **System.Devices.AepService.ServiceClassId**。

## <a name="upnp-services"></a>UPnP 服务

UPnP 服务使用以下协议标识符：{0e261de4-12f0-46e6-91ba-428607ccef64}

通常情况下，所有 UPnP 服务都使用在 RFC 4122 中定义的算法将其名称散列为 GUID。 下表列出了在 Windows 中定义的一些常见 UPnP 服务。

| 服务名称                       | GUID                                      |
|------------------------------------|-------------------------------------------|
| 连接管理器                 | **ba36014c-b51f-51cc-bf71-1ad779ced3c6**  |
| AV 传输                       | **deeacb78-707a-52df-b1c6-6f945e7e25bf**  |
| 呈现控件                  | **cc7fe721-a3c7-5a14-8c49-4419dc895513**  |
| 第 3 层转发                 | **97d477fa-f403-577b-a714-b29a9007797f**  |
| WAN 常用接口配置 | **e4c1c624-c3c4-5104-b72e-ac425d9d157c**  |
| WAP IP 连接                  | **e4ac1c23-b5ac-5c27-8814-6bd837d8832c**  |
| WFA WLAN 配置             | **23d5f7db-747f-5099-8f21-3ddfd0c3c688**  |
| 增强的打印机                   | **fb9074da-3d9f-5384-922e-9978ae51ef0c**  |
| 基本打印机                      | **5d2a7252-d45c-5158-87a4-05212da327e1**  |
| 媒体接收器注册机构           | **0b4a2add-d725-5198-b2ba-852b8bf8d183**  |
| 内容目录                  | **89e701dd-0597-5279-a31c-235991d0db1c**  |
| DIAL                               | **085dfa4a-3948-53c7-a0d7-16d8ec26b29b**  |

 

## <a name="wsd-services"></a>WSD 服务

WSD 服务使用以下协议标识符：{782232aa-a2f9-4993-971b-aedc551346b0}

通常情况下，所有 WSD 服务都使用在 RFC 4122 中定义的算法将其名称散列为 GUID。 下表列出了在 Windows 中定义的一些常见 WSD 服务。

| 服务名称 | GUID                                     |
|--------------|------------------------------------------|
| 打印机      | **65dca7bd-2611-583e-9a12-ad90f47749cf** |
| 扫描仪      | **56ec8b9e-0237-5cae-aa3f-d322dd2e6c1e** |

 

## <a name="aqs-sample"></a>AQS 示例

此 AQS 将对支持 DIAL 的所有 UPnP **AssociationEndpointService** 对象进行筛选。 在此情况下，[**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 设置为 **AsssociationEndpointService**。

``` syntax
System.Devices.AepService.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}" AND
System.Devices.AepService.ServiceClassId:="{085DFA4A-3948-53C7-A0D7-16D8EC26B29B}"
```

 

 
