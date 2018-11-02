---
author: muhsinking
ms.assetid: E0B9532F-1195-4927-99BE-F41565D891AD
title: 通过网络枚举设备
description: 除了发现本地连接的设备以外，还可以使用 Windows.Devices.Enumeration API 通过无线和网络协议来枚举设备。
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 00f8d4314d67828fa30007d3b8af4c4e1d06c154
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5977152"
---
# <a name="enumerate-devices-over-a-network"></a>通过网络枚举设备



**重要的 API**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

除了发现本地连接的设备以外，还可以使用 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API 通过无线和网络协议来枚举设备。

## <a name="enumerating-devices-over-networked-or-wireless-protocols"></a>通过网络或无线协议枚举设备

有时你需要枚举未进行本地连接而且只有通过无线或网络协议才能发现的设备。 为此，[**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API 拥有三个不同种类的设备对象：**AssociationEndpoint** (AEP)、**AssociationEndpointContainer**（AEP 容器）和 **AssociationEndpointService**（AEP 服务）。 它们统称为 AEP 或 AEP 对象。

某些设备 API 提供了选择器字符串，你可以使用该字符串来枚举可用的 AEP 对象。 与系统已配对和未配对的设备可能都包括在其中。 有些设备可能不需要配对。 如果在与设备交互前需要和其配对，则那些设备 API 可能会尝试与设备配对。 Wi-Fi Direct 就是遵循此模式的 API 的示例。 如果那些设备 API 没有与设备自动配对，你可以从 [**DeviceInformation.Pairing**](https://msdn.microsoft.com/library/windows/apps/Dn705960) 中使用 [**DeviceInformationPairing**](https://msdn.microsoft.com/library/windows/apps/Mt168396) 对象来与其配对。

不过，可能存在你要自己手动发现设备而不使用预定义选择器字符串的情况。 例如，你可能只是需要收集有关 AEP 设备的信息而无需和它们交互，或者你可能想要找到比使用预定义选择器字符串发现的更多的 AEP 设备。 在这种情况下，你将需要按照[生成设备选择器](build-a-device-selector.md)的说明来生成自己的选择器字符串以及使用该字符串。

在你生成自己的选择器时，强烈建议你将枚举范围限制为你感兴趣的协议。 例如，如果你对 UPnP 设备特别感兴趣，则你不会使用 Wi-Fi 无线电来搜索 Wi-Fi Direct 设备。 Windows 已为每个协议都定义了一个标识，你可以用来限制枚举的范围。 下表列出了协议类型和标识符。

| 协议或网络设备类型              | ID                                         |
|----------------------------------------------|--------------------------------------------|
| UPnP（包括 DIAL 和 DLNA）               | **{0e261de4-12f0-46e6-91ba-428607ccef64}** |
| 基于设备的 Web 服务 (WSD)                | **{782232aa-a2f9-4993-971b-aedc551346b0}** |
| WLAN Direct                                 | **{0407d24e-53de-4c9a-9ba1-9ced54641188}** |
| DNS 服务发现 (DNS-SD)               | **{4526e8c1-8aac-4153-9b16-55e86ada0e54}** |
| 服务点                             | **{d4bf61b3-442e-4ada-882d-fa7B70c832d9}** |
| 网络打印机（Active Directory 打印机） | **{37aba761-2124-454c-8d82-c42962c2de2b}** |
| Windows 立即连接 (WNC)                    | **{4c1b1ef8-2f62-4b9f-9bc5-b21ab636138f}** |
| WiGig 扩展坞                                  | **{a277f3a5-8764-4f88-8045-4c5e962640b1}** |
| HP 打印机的 WLAN 预配           | **{c85ef710-f344-4792-bb6d-85a4346f1e69}** |
| 蓝牙                                    | **{e0cbf06c-cd8b-4647-bb8a-263b43f0f974}** |
| 蓝牙 LE                                 | **{bb7bb05e-5972-42b5-94fc-76eaa7084d49}** |

 

## <a name="aqs-examples"></a>AQS 示例

每个 AEP 种类都具有一个可用于将枚举限制到特定协议的属性。 请记住，在 AQS 筛选器中，你可以使用 OR 操作符来合并多个协议。 下面是一些说明如何查询 AEP 设备的 AQS 筛选器字符串示例。

此 AQS 会在 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 设置为 **AsssociationEndpoint** 时查询所有 UPnP **AssociationEndpoint** 对象。

``` syntax
System.Devices.Aep.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

此 AQS 会在 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 设置为 **AsssociationEndpoint** 时查询所有 UPnP 和 WSD **AssociationEndpoint** 对象。

``` syntax
System.Devices.Aep.ProtocolId:="{782232aa-a2f9-4993-971b-aedc551346b0}" OR
System.Devices.Aep.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

如果 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 设置为 **AsssociationEndpointService**，此 AQS 将查询所有 UPnP **AssociationEndpointService** 对象。

``` syntax
System.Devices.AepService.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

此 AQS 会在 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 设置为 **AssociationEndpointContainer** 时查询 **AssociationEndpointContainer** 对象，但只是通过枚举 UPnP 协议查找它们。 通常，不可用于枚举仅来自一个协议的容器。 但是，如果将筛选器限制为你知道在其中可发现你的设备的协议，这可能会有用。

``` syntax
System.Devices.AepContainer.ProtocolIds:~~"{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

 

 
