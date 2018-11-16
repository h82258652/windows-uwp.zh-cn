---
author: TerryWarwick
title: PointOfService 设备功能
description: 使用 Windows.Devices.PointOfService 命名空间需要有 PointOfService 功能
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: dbed55af1a9fa3df14f0a7e3e7c6d1f599b40fd3
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2018
ms.locfileid: "6834385"
---
# <a name="pointofservice-device-capability"></a>PointOfService 设备功能
你可以通过声明应用程序包清单中的功能请求访问 PointOfService API] 你可以使用 Microsoft Visual Studio 中的清单设计器声明大多数功能，也可以手动添加。  

> [!Important]
> 如果你不声明应用程序清单中的 **pointOfService** 功能，在尝试使用 Winodws.Devices.PointOfService 命名空间中的 API 时，你会收到错误 **System.UnauthorizedAccessException**。 

## <a name="declare-capability-using-manifest-designer"></a>使用清单设计器声明功能

1. 在**解决方案资源管理器**中，展开你的 UWP 应用程序的项目节点。
2. 双击 **Package.appxmanifest** 文件。  
*如果清单文件已经在 XML 代码视图中打开，Visual Studio 会提示你关闭该文件。*
3. 单击**功能**选项卡
4. 单击功能列表中**服务点**旁边的复选框以启用服务点设备功能


## <a name="declare-capability-manually"></a>手动声明功能

1. 在**解决方案资源管理器**中，展开你的 UWP 应用程序的项目节点。
2. 右键单击 **Package.appxmanifest** 文件并选择**查看代码**
3. 将 PointOfService DeviceCapability 元素添加到你的应用程序清单的“功能”部分。  

```xml
  <Capabilities>
    <DeviceCapability Name="pointOfService" />
  </Capabilities>
   ```
