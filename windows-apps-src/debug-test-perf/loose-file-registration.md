---
title: 通过松散文件注册部署应用
description: 本指南展示了如何使用松散文件布局来验证和共享 Windows 10 应用，不需要将它们进行打包。
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10, uwp, 设备门户, 应用管理器, 部署, sdk
ms.localizationpriority: medium
ms.openlocfilehash: 7bf3dab97be67a3b97aca4b3132bd9fe18691d15
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681928"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>通过松散文件注册部署应用 

本指南展示了如何使用松散文件布局来验证和共享 Windows 10 应用，不需要将它们进行打包。 通过注册宽松文件布局，开发人员可以快速验证他们的应用，而无需打包和安装应用。 

## <a name="what-is-a-loose-file-layout"></a>什么是宽松文件布局？

简单来说，宽松文件布局会将应用内容放置于文件夹中，而不必经过打包过程。 包内容在文件夹中是“宽松的”可用且未被打包。 

> [!WARNING]
> 宽松文件布局使开发人员和设计人员能够在主动开发期间快速验证他们的应用。 此方法不应用于对应用进行 dogfood 或外部测试。 我们建议对使用受信任证书签名的打包应用执行最终验证。 

## <a name="advantages-of-loose-file-registration"></a>宽松文件注册的优点

- **快速验证** - 由于应用文件已经解包，因此用户可以快速注册宽松文件布局并启动应用。 与常规应用一样，用户能够按设计目的来使用应用。 
- **轻松的网络内分发** - 如果宽松文件位于网络共享而不是本地驱动器内，开发人员则可以将网络共享位置发送给其他可以访问该网络的用户，并且他们可以注册宽松文件布局和运行应用。 这可使多名用户同时对应用进行验证。 
- **协作** - 通过宽松文件注册，开发人员和设计人员能够在应用注册期间继续处理可视资产。 用户在启动应用时会看到这些更改。 请注意，你只能以这种方式更改静态资产。 如果需要修改任何代码或动态创建的内容，则必须重新编译应用。

## <a name="how-to-register-a-loose-file-layout"></a>如何注册宽松文件布局

Windows 提供有多种用于在本地和远程设备上注册宽松文件布局的开发人员工具。 可以选择 `WinDeployAppCmd`（Windows SDK 工具）、Windows 设备门户、PowerShell 或 [Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network)。 下面我们将介绍如何使用这些工具注册宽松文件。 首先，请确保你具有以下设置：

- 你的设备必须运行 Windows 10 创意者更新（内部版本 14965）或更高版本。
- 你需要在所有设备上启用[开发者模式](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)和[设备发现](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery)。

> [!IMPORTANT]
> 只有在支持网络共享 (SMB) 协议的设备上才可以使用宽松文件注册：台式电脑和 Xbox。 

### <a name="register-with-windeployappcmd"></a>使用 WinDeployAppCmd 注册

如果使用与 Windows 10 创意者更新（内部版本 14965）或更高版本相应的 SDK 工具，则可以在命令提示符中使用 `WinDeployAppCmd` 命令。

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**网络路径** – 应用的宽松文件的路径。

**IP 地址** – 目标计算机的 IP 地址。

**目标计算机 PIN** - 与目标设备建立连接时所需的 PIN（如果需要）。 如果需要身份验证，系统将提示你使用 `-pin` 选项重试。 如需了解如何获取 PIN，请参阅[设备发现](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery)。

### <a name="windows-device-portal"></a>Windows 设备门户

Windows 设备门户在所有 Windows 10 设备上均可用，开发人员可使用它来测试和验证他们的工作。 它向开发者社区的所有受众提供了其浏览器 UX 和 REST 终结点。 有关设备门户的详细信息，请参阅 [Windows 设备门户概述](device-portal.md)。

若要在设备门户中注册宽松文件布局，请执行以下步骤。

1. 按照 [Windows 设备门户概述](device-portal.md)“设置”  部分中的步骤连接到设备门户。
1. 在“应用管理器”选项卡中，选择“从网络共享注册”  。
1. 输入宽松文件布局的网络共享路径。 
1. 如果主机设备没有网络共享的访问权限，系统则会提示输入所需的凭据。
1. 注册完成后，你就可以启动应用了。

在设备门户的“应用管理器”页面，还可以为你的主要应用注册可选的宽松文件布局：选中“我要指定可选包”复选框，然后指定可选应用的网络共享路径  。 

### <a name="powershell"></a>PowerShell 

通过 Windows PowerShell 还可注册宽松文件布局，但仅支持在本地设备上注册。 如需将布局注册到远程设备，则需要使用其他方法。 

若要注册宽松文件布局，请启动 PowerShell 并输入以下命令。

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>疑难解答

### <a name="mapped-network-drives"></a>映射网络驱动器
目前，宽松文件注册不支持映射的网络驱动器。 如需查看完整的网络共享路径，请参阅映射驱动器。

### <a name="registration-failure"></a>注册失败
进行注册的设备需要有权访问文件布局。 如果文件布局托管在网络共享上，请确保该设备具有访问权限。 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>应用中未加载对可视对象的修改 
应用在启动时会加载其可视资产。 如果在启动应用后对可视对象进行了修改，则必须重新启动应用才能查看最新更改。
