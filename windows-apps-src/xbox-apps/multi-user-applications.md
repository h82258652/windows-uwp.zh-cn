---
author: Mtoepke
title: 多用户应用程序简介
description: 对 Xbox 多用户模型进行一次高级别的简单介绍。
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 2dde6ed3-7f53-48a6-aebe-2605230decb8
ms.localizationpriority: medium
ms.openlocfilehash: 7534b6764bc98c415b557d100d869df186453626
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5687219"
---
# <a name="introduction-to-multi-user-applications"></a>多用户应用程序简介

本主题旨在对 Xbox 多用户模型进行一次高级别的简单介绍。

微调 Xbox One 用户模型，以达到游戏控制台可支持多个用户在一台设备上以协作方式玩游戏的要求。 它使多个用户（每位用户都配有自己的控制器）登录并在单个交互式会话中同时使用控制台。 这与其他 Windows 设备不同。 例如：
* **Windows 台式机**允许多个用户使用相同的设备，但每个用户都有其自己的交互式会话，并且每个会话完全独立于设备上的其他会话。
* **Windows 手机**仅允许单个用户使用该设备。 在 OOBE（全新体验）期间确定该单个用户，并且该用户在登录后无法注销。 实际上，如果其他用户想要使用该设备，则设备必须重置。 
* **Xbox One** 允许多个用户登录，并在单个交互式会话中同时使用该设备。

使用 Xbox One 用户模型的每位用户都受本地用户帐户支持。 此本地用户帐户与 Xbox Live 帐户（以及 Microsoft 帐户）相关联。 这意味着从 Xbox 用户帐户到 Xbox Live 帐户和 Microsoft 帐户存在严格的一对一映射。

## <a name="single-user-applications"></a>单用户应用程序
默认情况下，通用 Windows 平台 (UWP) 应用在启动该应用程序的用户的上下文中运行。 这些“单用户应用程序”**(SUA) 仅了解此单个用户，并在与其他 Windows 设备上的用户模型兼容的模式下运行。 Xbox 用户模型对与应用相关联的用户进行管理，并且保证在应用启动时用户可进行登录。 在此模型中，UWP 应用和游戏创作人员无需执行任何特殊操作便可在 Xbox 上运行。 

## <a name="multi-user-applications"></a>多用户应用程序
UWP 游戏可以选择采用 Xbox One 多用户模型。 这些“多用户应用程序”**(MUA) 在系统帐户（称为“默认帐户”）的上下文中运行，并且可以充分利用 Xbox One 用户模型的灵活性和强大功能。 对于这些游戏，Xbox 用户模型不会对与游戏相关联的用户进行管理，甚至不需要用户登录即可使游戏运行。 这意味着必须写明用户要求，以使用户明确了解其内容并对内容进行管理，例如，是否需要用户登录、是否实现当前用户的概念以及是否允许多个用户同时输入等等。
   
若要选择进入多用户模型：   
1. 在 Visual Studio 中打开你的项目。   
2. 选择 package.appxmanifest.xml 文件。   
3. 右键单击并选择“查看代码”****。   
4. 在 `<Properties></Properties>` 部分中添加以下行：

```
<uap:SupportedUsers>multiple</uap:SupportedUsers>
```

### <a name="identifying-users-and-inputs"></a>标识用户和输入
开发人员可以使用 KeyUp 和 KeyDown 路由事件所用的 KeyRoutedEventArgs.DeviceId，来区分不同输入生成的事件。
使用 Windows.System.UserDeviceAssociation.FindUserFromDeviceId 方法将有助于识别与特定输入关联的用户。

有关详细信息，请参阅 [KeyRoutedEventArgs.DeviceId](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.keyroutedeventargs.deviceid) 主题。


## <a name="guidance-on-which-model-to-choose"></a>有关要选择的模型的指南
所有 UWP 应用和大多数单用户游戏可以编写成 SUA。 我们建议仅合作的多玩家游戏考虑选择采用 Xbox One 多用户模型。

## <a name="see-also"></a>另请参阅
- [Xbox One 上的 UWP](index.md)
