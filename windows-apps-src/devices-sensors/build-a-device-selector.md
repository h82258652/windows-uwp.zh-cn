---
ms.assetid: D06AA3F5-CED6-446E-94E8-713D98B13CAA
title: 生成设备选择器
description: 生成设备选择器将使你可以在枚举设备时限制要搜索的设备。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 01a4bfc2ec4c1d442058dbb6009065541f93cc7f
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7718801"
---
# <a name="build-a-device-selector"></a>生成设备选择器



**重要的 API**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

生成设备选择器将使你可以在枚举设备时限制要搜索的设备。 这将使你能够仅获取相关结果，还将提高系统性能。 在大多数情况下，你都可以从设备堆栈获取设备选择器。 例如，你可能将 [**GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/Dn264015) 用于在 USB 上发现的设备。 这些设备选择器将返回一个高级查询语法 (AQS) 字符串。 如果你不熟悉 AQS 格式，则可以访问[以编程方式使用高级查询语法](https://msdn.microsoft.com/library/windows/desktop/Bb266512)来阅读更多内容。

## <a name="building-the-filter-string"></a>生成筛选器字符串

在某些情况下，你需要枚举设备，而提供的设备选择器并不适用于你的方案。 设备选择器是包含以下信息的 AQS 筛选器字符串。 在创建筛选器字符串前，需要了解有关要枚举的设备的一些关键信息。

-   你感兴趣的设备的 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)。 有关 **DeviceInformationKind** 如何影响枚举设备的详细信息，请参阅[枚举设备](enumerate-devices.md)。
-   如何生成 AQS 筛选器字符串，这将在本主题中进行介绍。
-   你感兴趣的属性。 可用属性将取决于 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)。 有关详细信息，请参阅[设备信息属性](device-information-properties.md)。
-   执行查询要使用的协议。 这仅在你通过无线或有线网络搜索设备时才会需要。 有关执行此操作的详细信息，请参阅[通过网络枚举设备](enumerate-devices-over-a-network.md)。

在使用 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API 时，你经常将设备选择器与你感兴趣的设备类型相结合。 设备类型的可用列表由 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 枚举定义。 此组因素可帮助你限制可用于你感兴趣的类型的设备。 如果未指定 **DeviceInformationKind** 或你要使用的方法不提供 **DeviceInformationKind** 参数，则默认类型为 **DeviceInterface**。

[**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API 使用规范 AQS 语法，但并非所有运营商都受支持。 有关构造筛选器字符串时可用的属性列表，请参阅[设备信息属性](device-information-properties.md)。

**警告**定义的自定义属性`{GUID} PID`构造 AQS 筛选器字符串时，不能使用格式。 这是因为属性类型派生自已知的属性名称。

 

下表列出了 AQS 运营商及其支持的参数类型。

| 运算符                       | 支持的类型                                                             |
|--------------------------------|-----------------------------------------------------------------------------|
| **COP\_EQUAL**                 | 字符串、布尔值、GUID、UInt16、UInt32                                       |
| **COP\_NOTEQUAL**              | 字符串、布尔值、GUID、UInt16、UInt32                                       |
| **COP\_LESSTHAN**              | UInt16、UInt32                                                              |
| **COP\_GREATERTHAN**           | UInt16、UInt32                                                              |
| **COP\_LESSTHANOREQUAL**       | UInt16、UInt32                                                              |
| **COP\_GREATERTHANOREQUAL**    | UInt16、UInt32                                                              |
| **COP\_VALUE\_CONTAINS**       | 字符串、字符串数组、布尔值数组、GUID 数组、UInt16 数组、UInt32 数组 |
| **COP\_VALUE\_CONTAINS**    | 字符串、字符串数组、布尔值数组、GUID 数组、UInt16 数组、UInt32 数组 |
| **COP\_VALUE\_STARTSWITH**     | 字符串                                                                      |
| **COP\_VALUE\_ENDSWITH**       | 字符串                                                                      |
| **COP\_DOSWILDCARDS**          | 不支持                                                               |
| **COP\_WORD\_EQUAL**           | 不支持                                                               |
| **COP\_WORD\_STARTSWITH**      | 不支持                                                               |
| **COP\_APPLICATION\_SPECIFIC** | 不支持                                                               |


> **提示**你可以指定**NULL** **COP\_EQUAL**或**COP\_NOTEQUAL**。 这将转换为一个没有值或值不存在的属性。 在 AQS 中，可以使用空括号 \[\] 指定 **NULL**。

> **重要提示**在使用**COP\_VALUE\_CONTAINS**和**COP\_VALUE\_NOTCONTAINS**运算符时，它们的行为不同的字符串和字符串数组。 如果是字符串，系统将执行不区分大小写的搜索，以查看设备是否将指定字符串作为子字符串包含起来。 如果是字符串数组，则不会搜索子字符串。 对于字符串数组，将搜索该数组，查看它是否包含整个指定字符串。 无法通过搜索字符串数组查看数组中的元素是否包含一个子字符串。

如果你无法创建可相应地设置结果范围的单个 AQS 筛选器字符串，则可以在接收结果后进行筛选。 但是，如果你选择执行此操作，我们建议你在将结果提供给 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API 时，尽量从初始 AQS 筛选器字符串限制结果。 这将有助于提高你的应用程序的性能。

## <a name="aqs-string-examples"></a>AQS 字符串示例

以下示例演示如何使用 AQS 语法来限制要枚举的设备。 所有这些筛选器字符串都与 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 配对才能创建完整的筛选器。 如果未指定任何类型，请记住默认类型为 **DeviceInterface**。

当此筛选器与 **DeviceInterface** 的 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 配对时，它将枚举所有包含音频捕获接口类并且当前已启用的对象。 **=** 转换为 **COP\_EQUALS**。

``` syntax
System.Devices.InterfaceClassGuid:="{2eef81be-33fa-4800-9670-1cd474972c3f}" AND
System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True
```

当此筛选器与 **Device** 的 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 配对时，它将枚举所有具有至少一个 GenCdRom 硬件 ID 的对象。 **~~** 转换为 **COP\_VALUE\_CONTAINS**。

``` syntax
System.Devices.HardwareIds:~~"GenCdRom"
```

当此筛选器与 **DeviceContainer** 的 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 配对时，它将枚举所有模型名称中包含子字符串 Microsoft 的对象。 **~~** 转换为 **COP\_VALUE\_CONTAINS**。

``` syntax
System.Devices.ModelName:~~"Microsoft"
```

当此筛选器与 **DeviceInterface** 的 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 配对时，它将枚举所有名称以子字符串 Microsoft 开头的对象。 **~&lt;** 转换为 **COP\_STARTSWITH**。

``` syntax
System.ItemNameDisplay:~<"Microsoft"
```

当此筛选器与 **Device** 的 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 配对时，它将枚举所有具有 **System.Devices.IpAddress** 属性集的对象。 **&lt;&gt;\[\]** 转换为结合了 **NULL** 值的 **COP\_NOTEQUALS**。

``` syntax
System.Devices.IpAddress:<>[]
```

当此筛选器与 **Device** 的 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) 配对时，它将枚举所有不具有 **System.Devices.IpAddress** 属性集的对象。 **=\[\]** 转换为结合了 **NULL** 值的 **COP\_EQUALS**。

``` syntax
System.Devices.IpAddress:=[]
```

 

 
