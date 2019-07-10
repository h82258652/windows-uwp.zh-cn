---
title: 通过松散文件注册部署应用
description: 本指南展示了如何使用松散文件布局来验证和共享 Windows 10 应用，不需要将它们进行打包。
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10、 uwp、 设备门户、 应用程序管理器、 部署、 sdk
ms.localizationpriority: medium
ms.openlocfilehash: 3369f3a982efec258fb5ac2358b2962e84e6cefb
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2019
ms.locfileid: "67713763"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>通过松散文件注册部署应用 

本指南展示了如何使用松散文件布局来验证和共享 Windows 10 应用，不需要将它们进行打包。 注册松散文件布局允许开发人员能够快速验证自己的应用而无需打包和安装的应用。 

## <a name="what-is-a-loose-file-layout"></a>什么是松散文件布局？

松散文件布局是只需在不经过打包过程的文件夹中放置应用内容的行为。 包内容位于"松散"可用文件夹中并不打包。 

> [!WARNING]
> 松散文件布局注册是用于开发人员和设计人员可以快速在活动开发过程中验证自己的应用。 这种方法不应使用到"dogfood"或网络应用程序。 我们建议使用受信任的证书签名的打包应用程序执行最终验证。 

## <a name="advantages-of-loose-file-registration"></a>松散文件注册的优点

- **快速验证**-因为应用程序文件已解包后，用户可以快速注册松散文件布局和启动该应用程序。 就像一个常规的应用，用户将能够使用应用程序，因为其设计初衷。 
- **简单网络中分发**-如果的松散文件位于网络共享，而不是本地驱动器中，开发人员可以将网络共享位置发送到其他用户有权访问网络，并且他们可以注册松散文件布局，并运行应用程序。 这允许多个用户同时验证该应用程序。 
- **协作**的松散文件注册允许开发人员和设计人员可以注册应用程序时继续处理视觉对象资产。 在启动应用时，用户将看到这些更改。 请注意，您可以只更改静态资产以这种方式。 如果需要修改任何代码或动态创建的内容，则必须重新编译应用程序。

## <a name="how-to-register-a-loose-file-layout"></a>如何注册松散文件布局

Windows 提供了多个开发人员工具来注册在本地和远程设备上的松散文件布局。 您可以从中`WinDeployAppCmd`（Windows SDK 工具），Windows Device Portal、 PowerShell，并[Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network)。 下面我们将详细阐述如何注册使用这些工具的松散文件。 但首先，确保您具有以下设置：

- 你的设备必须在 Windows 10 创意者更新 (生成 14965) 或更高版本。
- 您将需要启用[开发人员模式](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)并[设备发现](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#device-discovery)在所有设备上。

> [!IMPORTANT]
> 松散文件注册才支持网络共享 (SMB) 协议的设备上可用：桌面和 Xbox。 

### <a name="register-with-windeployappcmd"></a>注册 WinDeployAppCmd

如果使用对应于 Windows 10 创意者更新 (生成 14965) 或更高版本的 SDK 工具，则可以使用`WinDeployAppCmd`命令，在命令提示符。

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**网络路径**– 应用程序的松散文件的路径。

**IP 地址**– 目标计算机的 IP 地址。

**目标计算机的 PIN** – PIN，如果需要，以建立与目标设备的连接。 系统会提示使用重试`-pin`选项则是必需的身份验证。 请参阅[设备发现](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery)若要了解如何获取 PIN。

### <a name="windows-device-portal"></a>Windows Device Portal

Windows Device Portal 可在所有 Windows 10 设备上，并由开发人员用于测试和验证他们的工作。 它适用于所有用户使用其浏览器 UX 开发人员社区和 REST 终结点。 设备门户的详细信息，请参阅[Windows Device Portal 概述](device-portal.md)。

若要在设备门户中注册的松散文件布局，请按照下列步骤。

1. 连接到设备门户中的步骤**安装程序**一部分[Windows Device Portal 概述](device-portal.md)。
1. 在应用程序管理器选项卡，选择**从网络共享注册**。
1. 网络共享路径输入到松散文件布局中。 
1. 如果主机设备不具有访问网络共享位置，将提示输入所需的凭据。
1. 注册完成后，可以启动应用。

在设备门户应用程序管理器页上，您也可以注册可选松散文件布局在主要应用通过选择**我想要指定可选包**复选框，然后指定网络共享路径的可选应用程序. 

### <a name="powershell"></a>PowerShell 

Windows PowerShell 还可以注册松散文件布局，但只能在本地设备上。 如果你需要注册到远程设备的布局，您需要使用其他方法之一。 

若要注册的松散文件布局，请启动 PowerShell 并输入以下信息。

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>疑难解答

### <a name="mapped-network-drives"></a>映射网络驱动器
目前，映射的网络驱动器不支持的松散文件注册。 请参阅与完整的映射驱动器的网络共享路径。

### <a name="registration-failure"></a>注册失败
在其正在进行注册。 设备需要有权访问文件布局。 如果文件布局位于网络共享上，请确保该设备具有访问权限。 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>对视觉对象资产的修改不是在应用中加载 
应用程序将在启动时加载其视觉对象资产。 如果启动应用后，对视觉对象资产进行修改，必须重新启动应用程序以查看最新更改。
