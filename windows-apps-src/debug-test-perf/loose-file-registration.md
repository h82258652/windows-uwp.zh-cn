---
author: c-don
title: 通过松散文件注册部署应用
description: 本指南介绍了如何使用松散文件布局进行验证和共享 Windows 10 应用，而无需其打包。
ms.author: cdon
ms.date: 6/1/2018
ms.topic: article
keywords: windows 10、 uwp、 设备门户、 应用管理器、 部署、 sdk
ms.localizationpriority: medium
ms.openlocfilehash: 16dc7c3d8182e249134be941d466574cddc36157
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5944905"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>通过松散文件注册部署应用 

本指南介绍了如何使用松散文件布局进行验证和共享 Windows 10 应用，而无需其打包。 注册 loose 文件布局允许开发人员快速验证他们的应用无需打包和安装应用。 

## <a name="what-is-a-loose-file-layout"></a>松散文件布局是什么？

松散文件布局是只需将应用内容放置在一个文件夹，而不是经过打包过程中的行为。 程序包内容是"松散"文件夹中可用并不打包。 

> [!WARNING]
> 松散文件布局注册为开发人员和设计人员快速在活动的开发过程中验证他们的应用。 此方法不应该用于"试用"或外部测试版应用。 我们建议的受信任的证书进行签名的打包的应用上执行最终的验证。 

## <a name="advantages-of-loose-file-registration"></a>松散文件注册的优势

- **快速验证**-这些应用文件已解包，因为用户可以快速注册 loose 文件布局和启动该应用。 就像常规应用，用户将能够使用该应用，因为它设计。 
- **简单的网络中分发**-如果松散文件的组成部分位于网络共享，而不是本地驱动器中，开发人员可以发送的网络共享位置，可以向其他用户有权访问网络，并且他们可以注册 loose 文件布局和运行该应用。 这允许多个用户同时验证该应用。 
- **协作**-松散文件注册允许开发人员和设计器应用注册时继续处理可视资源。 用户启动应用后，用户将看到这些更改。 请注意，你可以仅更改这种方式中的静态资源。 如果你需要修改任何代码或动态创建的内容，你必须重新编译应用。

## <a name="how-to-register-a-loose-file-layout"></a>有关如何注册 loose 文件布局

Windows 提供了多个开发人员工具注册在本地和远程设备上的松散文件布局。 你可以选择从`WinDeployAppCmd`（Windows SDK 工具），Windows Device Portal、 PowerShell 和[Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network)。 下面我们将介绍如何使用这些工具的松散文件注册。 但是，首先，请确保你有以下设置：

- 你的设备必须在 Windows 10 创意者更新 (生成 14965) 或更高版本。
- 你将需要启用[开发人员模式](https://msdn.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)和[设备发现](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#device-discovery)所有设备上。

> [!IMPORTANT]
> 松散文件注册服务仅适用于支持的网络共享 (SMB) 协议的设备： 台式机和 Xbox。 

### <a name="register-with-windeployappcmd"></a>注册 WinDeployAppCmd

如果你使用对应于 Windows 10 创意者更新 (版本 14965) 或更高版本的 SDK 工具，你可以使用`WinDeployAppCmd`命令的命令提示符。

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**网络路径**-应用的松散文件的路径。

**IP 地址**– 在目标计算机的 IP 地址。

**目标计算机的 PIN** – PIN，如果需要，与目标设备建立连接。 将提示你使用重试`-pin`如果需要身份验证选项。 请参阅[设备发现](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery)，若要了解如何获取 PIN。

### <a name="windows-device-portal"></a>Windows Device Portal

Windows Device Portal 在所有 Windows 10 设备上可用，并由开发人员使用用于测试和验证其工作。 它解决了所有的开发人员社区与 UX 其浏览器的受众和 REST 终结点。 有关 Device Portal 的详细信息，请参阅[Windows Device Portal 概述](device-portal.md)。

若要在设备门户中注册的松散文件布局，请按照以下步骤。

1. 通过遵循[Windows Device Portal 概述](device-portal.md)的**设置**部分中的步骤，连接到 Device Portal。
1. 在应用管理器选项卡中，选择**从网络共享注册**。
1. 输入的松散文件布局的网络共享路径。 
1. 如果主机设备不会有权访问的网络共享，将出现一条提示，以输入所需的凭据。
1. 注册完成后，你可以启动该应用。

在 Device Portal 的应用管理器页面上，你也可以登记可选的松散文件布局的主应用通过选择**我想要指定可选包**复选框，然后指定可选的应用的网络共享路径。 

### <a name="powershell"></a>PowerShell 

Windows PowerShell 还使你可以注册 loose 文件布局，但仅在本地设备上。 如果你需要注册到远程设备的布局，你将需要使用其他方法之一。 

若要注册的松散文件布局，启动 PowerShell，然后输入以下命令。

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>疑难解答

### <a name="mapped-network-drives"></a>映射的网络驱动器
目前，映射的网络驱动器不支持松散文件注册。 请参阅完全映射的驱动器的网络共享路径。

### <a name="registration-failure"></a>注册失败
注册正在发生的设备需要有权访问的文件布局。 如果网络共享上托管的文件布局时，请确保设备具有访问权限。 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>不在应用中加载修改视觉资源 
该应用将在启动时加载其视觉资源。 如果启动应用后，为视觉资源进行修改，你必须重新启动应用以查看最新的更改。