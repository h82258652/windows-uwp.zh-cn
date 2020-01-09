---
title: 通过松散文件注册部署应用
description: 本指南展示了如何使用松散文件布局来验证和共享 Windows 10 应用，不需要将它们进行打包。
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10，uwp，设备门户，应用管理器，部署，sdk
ms.localizationpriority: medium
ms.openlocfilehash: 7bf3dab97be67a3b97aca4b3132bd9fe18691d15
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681928"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>通过松散文件注册部署应用 

本指南展示了如何使用松散文件布局来验证和共享 Windows 10 应用，不需要将它们进行打包。 通过注册松散文件布局，开发人员可快速验证其应用，而无需打包和安装应用。 

## <a name="what-is-a-loose-file-layout"></a>什么是松散文件布局？

松散文件布局只是将应用程序内容放置在文件夹中的操作，而不是通过打包过程。 包内容在文件夹中可用且未打包。 

> [!WARNING]
> 松散文件布局注册面向开发人员和设计人员在活动开发期间快速验证其应用。 此方法不能用于 "预览版" 或飞行应用。 建议在使用受信任证书签名的封装应用上执行最终验证。 

## <a name="advantages-of-loose-file-registration"></a>松散文件注册的优点

- **快速验证**-由于应用程序文件已经解包，因此用户可以快速注册松散文件布局，并启动应用程序。 与常规应用一样，用户能够在设计时使用该应用。 
- **轻松的网络分发**-如果松散文件位于网络共享而不是本地驱动器中，开发人员可以将网络共享位置发送给有权访问网络的其他用户，并且他们可以注册松散文件布局并运行应用。 这允许多个用户同时验证应用。 
- **协作**-松散文件注册使开发人员和设计人员能够在应用程序注册期间继续处理视觉资产。 用户将在启动应用时看到这些更改。 请注意，只能以这种方式更改静态资产。 如果需要修改任何代码或动态创建的内容，则必须重新编译应用程序。

## <a name="how-to-register-a-loose-file-layout"></a>如何注册松散文件布局

Windows 提供多种开发人员工具，用于在本地和远程设备上注册松散文件布局。 可以从 `WinDeployAppCmd` （Windows SDK 工具）、Windows 设备门户、PowerShell 和[Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network)中进行选择。 下面我们将介绍如何使用这些工具注册松散文件。 但首先请确保已执行以下设置：

- 你的设备必须在 Windows 10 创意者更新（版本14965）或更高版本上。
- 你将需要在所有设备上启用[开发人员模式](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)和[设备发现](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery)。

> [!IMPORTANT]
> 只有支持网络共享（SMB）协议的设备才可以使用松散文件注册： Desktop 和 Xbox。 

### <a name="register-with-windeployappcmd"></a>注册到 WinDeployAppCmd

如果你使用的 SDK 工具对应于 Windows 10 创意者更新（版本14965）或更高版本，则可以在命令提示符下使用 `WinDeployAppCmd` 命令。

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**网络路径**–应用程序的松散文件的路径。

**Ip 地址**–目标计算机的 ip 地址。

**目标计算机 pin** –如果需要，可与目标设备建立连接。 如果需要身份验证，系统会提示你用 `-pin` 选项重试。 若要了解如何获取 PIN，请参阅[设备发现](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery)。

### <a name="windows-device-portal"></a>Windows Device Portal

Windows 设备门户在所有 Windows 10 设备上均可用，开发人员可使用它来测试和验证其工作。 它通过其浏览器 UX 和 REST 终结点适用于开发人员社区的所有受众。 有关设备门户的详细信息，请参阅[Windows 设备门户概述](device-portal.md)。

若要在设备门户中注册松散文件布局，请执行以下步骤。

1. 按照[Windows 设备门户概述](device-portal.md)的 "**设置**" 部分中的步骤连接到设备门户。
1. 在 "应用管理器" 选项卡中，选择 "**从网络共享注册**"。
1. 输入松散文件布局的网络共享路径。 
1. 如果主机设备没有网络共享的访问权限，则会提示输入所需的凭据。
1. 注册完成后，可以启动该应用。

在设备门户的 "应用管理器" 页上，你还可以通过选择 "**我想要指定可选包**" 复选框，然后指定可选应用的网络共享路径，为主应用注册可选的松散文件布局。 

### <a name="powershell"></a>PowerShell 

Windows PowerShell 还允许你在本地设备上注册松散文件布局。 如果需要将布局注册到远程设备，则需要使用其他方法之一。 

若要注册松散文件布局，请启动 PowerShell 并输入以下项。

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>“疑难解答”

### <a name="mapped-network-drives"></a>映射网络驱动器
目前，不支持将映射的网络驱动器用于松散文件注册。 请参阅网络共享路径完全相同的映射驱动器。

### <a name="registration-failure"></a>注册失败
进行注册的设备需要有权访问文件布局。 如果文件布局承载在网络共享上，请确保该设备具有访问权限。 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>应用中未加载对视觉对象的修改 
应用会在启动时加载其视觉对象。 如果在启动应用后对视觉对象进行了修改，则必须重新启动应用才能查看最新更改。
