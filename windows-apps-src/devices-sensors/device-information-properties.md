---
author: muhsinking
ms.assetid: 4A4C2802-E674-4C04-8A6D-D7C1BBF1BD20
title: 设备信息属性
description: 每台设备都具有相关联的 DeviceInformation 属性，在你需要特定的信息或生成设备选择器时可以使用这些属性。
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2ad87bdfa468c98a2e10d57f81b8e8d187bfbcef
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5826145"
---
# <a name="device-information-properties"></a>设备信息属性



**重要的 API**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

每台设备都具有相关联的 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 属性，在你需要特定的信息或生成设备选择器时可以使用这些属性。 这些属性可以指定为 AQS 筛选器来限制你要枚举的设备，以便查找带有指定特征的设备。 你还可以使用这些属性指示你想要为每个设备返回的信息。 这样可以让你指定返回到应用程序的设备信息。

有关在设备选择器中使用 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 属性的详细信息，请参阅[生成设备选择器](build-a-device-selector.md)。 本主题将讨论如何请求信息属性，而且还列出了一些常用属性及其用途。

[**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 对象由标识 ([**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id))、种类 ([**DeviceInformation.Kind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.kind.aspx)) 和属性包 ([**DeviceInformation.Properties**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.properties.aspx)) 组成。 **DeviceInformation** 对象的所有其他属性都派生自 **Properties** 属性包。 例如，[**Name**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.name) 派生自 **System.ItemNameDisplay**。 这意味着属性包始终包含用于确定其他属性的必要信息。

## <a name="requesting-properties"></a>请求属性

[**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 对象具有一些基本属性（如 [**Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id) 和 [**Kind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.kind.aspx)），但其中大部分属性都存储在 [**Properties**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.properties.aspx) 下的属性包中。 因此，该属性包包含用于查找属性包中属性来源的属性。 例如，使用 [System.ItemNameDisplay](https://msdn.microsoft.com/library/windows/desktop/Bb760770) 查找 [**Name**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.name) 属性来源。 这就是具有用户友好名称的常用和已知属性的一种情况。 为了更容易查询属性，Windows 提供了若干个用户友好名称。

当请求属性时，你并不局限于具有用户友好名称的常用属性。 你可以通过指定基础 GUID 和属性 ID (PID) 来请求任何可用的属性，甚至请求由单个设备或驱动程序提供的自定义属性。 用于指定自定义属性的格式是“`{GUID} PID`”。 例如:"`{744e3bed-3684-4e16-9f8a-07953a8bf2ab} 7`"。 

> [!Note]
> 你可以在设备驱动程序的设备属性密钥标头文件中找到属性 Guid 的列表。

某些属性对于所有 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind) 对象都是常用属性，但大多数属性是特定种类的唯一属性。 下面部分列出了一些常用属性，按单个 **DeviceInformationKind** 排序。 有关不同种类之间的相关方式的详细信息，请参阅 **DeviceInformationKind**。

## <a name="deviceinterface-properties"></a>DeviceInterface 属性

**DeviceInterface** 是应用方案中使用的最常用的默认 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind) 对象。 除非设备 API 表示另一种特定的 **DeviceInformationKind**，否则这就是你应使用的对象种类。

| 名称                                  | 类型    | 说明                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **System.Devices.ContainerId**        | GUID    | 包含 **Device**（含有此 **DeviceInterface**）的 **DeviceInformationKind.DeviceContainer** 的标识。 你可以将此值与 **DeviceInformationKind.DeviceContainer** 一起传递到 [**CreateFromIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createfromidasync) 以查找合适的容器。                                                                                    |
| **System.Devices.InterfaceClassGuid** | GUID    | 此接口表示的接口类 GUID。                                                                                                                                                                                                                                                                                                                                                       |
| **System.Devices.DeviceInstanceId**   | 字符串  | 父 **DeviceInformationKind.Device** 的标识。 你可以将此值与 **DeviceInformationKind.Device** 一起传递到 [**CreateFromIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createfromidasync) 以查找合适的设备。                                                                                                                                                                   |
| **System.Devices.InterfaceEnabled**   | 布尔值 | 表明接口是否已启用。 [**DeviceInformation.IsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.isenabled) 派生自此属性。                                                                                                                                                                                                                                                           |
| **System.Devices.GlyphIcon**          | 字符串  | 字形的图标路径。                                                                                                                                                                                                                                                                                                                                                                                  |
| **System.Devices.IsDefault**          | 布尔值 | 指示此设备是否为 **System.Devices.InterfaceClassGuid** 的默认设备。 这主要用于打印机。 这不适用于音频，因为存在多个默认音频设备。 使用 [**GetDefaultAudioRenderId**](https://msdn.microsoft.com/library/windows/apps/BR226819) 或 [**GetDefaultAudioCaptureId**](https://msdn.microsoft.com/library/windows/apps/BR226818) 以获取默认音频设备。 |
| **System.Devices.Icon**               | 字符串  | 图标路径。                                                                                                                                                                                                                                                                                                                                                                                                |
| **System.ItemNameDisplay**            | 字符串  | 设备对象的最佳显示名称。                                                                                                                                                                                                                                                                                                                                                              |

 

## <a name="device-properties"></a>设备属性

| 名称                                  | 类型       | 说明                                                                                                                                                                                                                                                                              |
|---------------------------------------|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **System.Devices.ClassGuid**          | GUID       | 设备安装期间使用的设备类。 有关详细信息，请参阅[设备安装程序类](https://msdn.microsoft.com/library/windows/hardware/Ff541509)。                                                                                                                                                            |
| **System.Devices.CompatibleIds**      | String\[\] | 设备的兼容 ID。 当 Windows 确定要在设备上安装的最佳驱动程序时，会使用它们。 有关详细信息，请参阅[兼容 ID](https://msdn.microsoft.com/library/windows/hardware/Ff539950)。                                                                                                |
| **System.Devices.ContainerId**        | GUID       | 包含此设备的 **DeviceInformationKind.DeviceContainer** 的标识。 你可以将此值与 **DeviceInformationKind.DeviceContainer** 一起传递到 [**CreateFromIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createfromidasync) 以查找合适的容器。          |
| **System.Devices.DeviceCapabilities** | UInt32     | **CfgMgr32.h** 中定义的 CM\_DEVCAP\_X 功能标志的按位 OR。 有关详细信息，请参阅 [**DEVPKEY\_Device\_Capabilities**](https://msdn.microsoft.com/library/windows/hardware/Ff542373)。                                                                                             |
| **System.Devices.DeviceHasProblem**   | 布尔值    | 设备当前有问题，可能无法正常工作。 此问题可能是由于过时、丢失或无效的驱动程序而引起的。                                                                                                                                                |
| **System.Devices.DeviceInstanceId**   | 字符串     | 设备的标识。 这也是 [**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id) 的值。                                                                                                                                                                       |
| **System.Devices.DeviceManufacturer** | 字符串     | 设备制造商。                                                                                                                                                                                                                                                          |
| **System.Devices.HardwareIds**        | String\[\] | 设备的硬件 ID。 Windows 使用这些 ID 确定要安装的最佳驱动程序。 设备供应商可以使用此属性来从其应用中标识其设备。 有关详细信息，请参阅[硬件 ID](https://msdn.microsoft.com/library/windows/hardware/Ff546152)。                                         |
| **System.Devices.Parent**             | 字符串     | 父设备的 [**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id)。 这是连接父级，不是 **DeviceContainer** 父级。                                                                                                                                 |
| **System.Devices.Present**            | 布尔值    | 指示设备当前是否存在并可用。                                                                                                                                                                                                                         |
| **System.ItemNameDisplay**            | 字符串     | 此设备对象的最佳显示名称。 在此情况下，这不一定是用户的最佳名称。 通过引用相关联的 **DeviceContainer** 或 **DeviceInterface** 的 **System.ItemNameDisplay**，可能会找到更合适的用户友好候选名称。 |

 

## <a name="devicecontainer-properties"></a>DeviceContainer 属性

| 名称                              | 类型       | 说明                                                                                                                                                        |
|-----------------------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **System.Devices.Category**       | String\[\] | 设备所属类别的描述列表。 将以单数类别的形式提供此列表。 例如，“Display”、“Phone”或“Audio device”。  |
| **System.Devices.CategoryIds**    | String\[\] | 包含此设备所属类别的列表。 例如，**Audio.Headphone**、**Display.Monitor** 或 **Input.Gaming**。                                  |
| **System.Devices.CategoryPlural** | String\[\] | 设备所属类别的描述列表。 将以复数类别的形式提供此列表。 例如，“Displays”、“Phones”或“Audio devices”。 |
| **System.Devices.CompatibleIds**  | String\[\] | 所有子 **DeviceInformationKind.Device** 对象的兼容 ID 集合。                                                                       |
| **System.Devices.Connected**      | 布尔值    | 指示设备当前是否已连接到系统。                                                                                          |
| **System.Devices.GlyphIcon**      | 字符串     | 字形的图标路径。                                                                                                                                           |
| **System.Devices.HardwareIds**    | String\[\] | 所有子 **DeviceInformationKind.Device** 对象的硬件 ID 集合。                                                                         |
| **System.Devices.Icon**           | 字符串     | 图标路径。                                                                                                                                                         |
| **System.Devices.LocalMachine**   | 布尔值    | 如果此 **DeviceContainer** 表示系统本身，则为 **True**；如果设备是系统的外部设备，则为 **false**。                                              |
| **System.Devices.Manufacturer**   | 字符串     | 设备制造商。                                                                                                                                    |
| **System.Devices.ModelName**      | 字符串     | 设备容器的型号名称。                                                                                                                                |
| **System.Devices.Paired**         | 布尔值    | 指示任何子 **DeviceInformationKind.Device** 对象是无线设备还是已与当前系统配对的网络设备。             |
| **System.ItemNameDisplay**        | 字符串     | 此设备的最佳显示名称。                                                                                                                             |

 

## <a name="deviceinterfaceclass-properties"></a>DeviceInterfaceClass 属性

| 名称                       | 类型   | 说明                            |
|----------------------------|--------|----------------------------------------|
| **System.ItemNameDisplay** | 字符串 | 此设备的最佳显示名称。 |

 

## <a name="associationendpoint-properties"></a>AssociationEndpoint 属性

| 名称                                  | 类型       | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|---------------------------------------|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **System.Devices.Aep.AepId**          | 字符串     | 此设备的标识。 这也是 [**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id) 的值。                                                                                                                                                                                                                                                                                                                                                                        |
| **System.Devices.Aep.CanPair**        | 布尔值    | 指示设备是否可以与系统匹配。 [**DeviceInformationPairing.CanPair**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationpairing.canpair.aspx) 派生自此属性。                                                                                                                                                                                                                                                                                                       |
| **System.Devices.Aep.Category**       | String\[\] | 设备所属类别。 例如，打印机或相机。                                                                                                                                                                                                                                                                                                                                                                                                             |
| **System.Devices.Aep.ContainerId**    | GUID       | 父 **AssociationEndpointContainer** 对象的 ID。                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **System.Devices.Aep.DeviceAddress**  | 字符串     | 设备地址。 如果设备是网络设备，则这是 IP 地址。                                                                                                                                                                                                                                                                                                                                                                                                  |
| **System.Devices.Aep.IsConnected**    | 布尔值    | 指示设备当前是否已连接到系统。                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **System.Devices.Aep.IsPaired**       | 布尔值    | 表明设备当前是否已配对。 [**DeviceInformationPairing.IsPaired**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationpairing.ispaired.aspx) 派生自此属性。                                                                                                                                                                                                                                                                                                                      |
| **System.Devices.Aep.IsPresent**      | 布尔值    | 指示设备当前是否存在，意味着设备为动态设备并且是通过网络或无线协议发现了该设备。 只要设备已与系统配对，就会缓存该设备。 在此之后，在查询 **AssociationEndpoint** 对象时，将会自动发现该设备。 因此，不能仅依赖通过查询发现设备来指示其当前是否可用。 这就是此属性很重要的原因。 |
| **System.Devices.Aep.Manufacturer**   | 字符串     | 设备制造商。                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| **System.Devices.Aep.ModelId**        | GUID       | 设备的型号 ID。                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| **System.Devices.Aep.ModelName**      | 字符串     | 设备的型号名称。                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **System.Devices.Aep.ProtocolId**     | GUID       | 指示用于发现此 **AssocationEndpoint** 设备的协议。                                                                                                                                                                                                                                                                                                                                                                                                            |
| **System.Devices.Aep.SignalStrength** | Int32      | 设备的信号强度。 此属性仅适用于某些协议。                                                                                                                                                                                                                                                                                                                                                                                                |
| **System.ItemNameDisplay**            | 字符串     | 设备的最佳显示名称。                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

 

## <a name="associationendpointcontainer-properties"></a>AssociationEndpointContainer 属性

| 名称                                                | 类型       | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|-----------------------------------------------------|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **System.Devices.AepContainer.Categories**          | String\[\] | 设备所属类别。 例如，打印机或相机。                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| **System.Devices.AepContainer.Children**            | String\[\] | 属于此容器的 **AssocationEndpoint** 对象的 ID 集合。                                                                                                                                                                                                                                                                                                                                                                                                                                |
| **System.Devices.AepContainer.CanPair**             | 布尔值    | 指示子 **AssociationEndpoint** 设备之一是否可以与系统匹配。 [**DeviceInformationPairing.CanPair**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationpairing.canpair.aspx) 派生自此属性。                                                                                                                                                                                                                                                                                                       |
| **System.Devices.AepContainer.ContainerId**         | GUID       | 此设备的标识。 这也是 [**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id) 的值，但采用 GUID 格式。                                                                                                                                                                                                                                                                                                                                                                                            |
| **System.Devices.AepContainer.IsPaired**            | 布尔值    | 指示子 **AssociationEndpoint** 设备之一当前是否已配对。 [**DeviceInformationPairing.IsPaired**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationpairing.ispaired.aspx) 派生自此属性。                                                                                                                                                                                                                                                                                                                      |
| **System.Devices.AepContainer.IsPresent**           | 布尔值    | 指示当前是否存在一个子 **AssociationEndpoint** 设备，意味着设备为动态设备并且已通过网络或无线协议被发现。 只要设备已与系统配对，就会缓存该设备。 在此之后，在查询 **AssociationEndpoint** 对象时，将会自动发现该设备。 因此，不能仅依赖通过查询发现设备来指示其当前是否可用。 这就是此属性很重要的原因。 |
| **System.Devices.AepContainer.Manufacturer**        | 字符串     | 设备制造商。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| **System.Devices.AepContainer.ModelIds**            | String\[\] | 设备的型号 ID 列表。 每个型号都是采用字符串形式的 GUID。                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| **System.Devices.AepContainer.ModelName**           | 字符串     | 设备的型号名称。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| **System.Devices.AepContainer.ProtocolIds**         | GUID\[\]   | 有助于生成此 **AssociationEndpointContainer** 对象的协议 ID 列表。 请牢记，通过收集所有通过相同物理设备的不同协议发现的 **AssociationEndpoint** 设备来创建 **AssociationEndpointContainer** 设备。                                                                                                                                                                                                                           |
| **System.Devices.AepContainer.SupportedUriSchemes** | String\[\] | 此设备支持的强制转换 URI 方案列表。                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| **System.Devices.AepContainer.SupportsAudio**       | 布尔值    | 指示此设备是否支持音频强制转换。                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| **System.Devices.AepContainer.SupportsImages**      | 布尔值    | 指示此设备是否支持图像强制转换。                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| **System.Devices.AepContainer.SupportsVideo**       | 布尔值    | 指示此设备是否支持视频强制转换。                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| **System.ItemNameDisplay**                          | 字符串     | 设备的最佳显示名称。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |

 

## <a name="associationendpointservice-properties"></a>AssociationEndpointService 属性

| 名称                                            | 类型    | 说明                                                                                                      |
|-------------------------------------------------|---------|------------------------------------------------------------------------------------------------------------------|
| **System.Devices.AepService.AepId**             | 字符串  | 父 **AssociationEndpoint** 对象的标识符。                                                     |
| **System.Devices.AepService.ContainerId**       | GUID    | 父 **AssociationEndpointContainer** 对象的标识符。                                            |
| **System.Devices.AepService.ParentAepIsPaired** | 布尔值 | 指示父 **AssociationEndpoint** 对象是否与系统配对。                           |
| **System.Devices.AepService.ProtocolId**        | GUID    | 用于发现此设备的协议的标识。                                                           |
| **System.Devices.AepService.ServiceClassId**    | GUID    | 由此设备表示的服务的标识。                                                             |
| **System.Devices.AeoService.ServiceId**         | 字符串  | 此服务的标识。 这也是 [**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id) 的值。 |
| **System.ItemNameDisplay**                      | 字符串  | 该服务的最佳显示名称。                                                                           |

 

 

 
