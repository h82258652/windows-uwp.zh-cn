---
title: 使用应用安装程序文件遇到的安装问题的疑难解答
description: 使用应用安装程序文件旁加载应用程序时的常见问题。
ms.date: 5/2/2018
ms.topic: article
keywords: windows 10, uwp, 应用安装程序, AppInstaller, 旁加载
ms.localizationpriority: medium
ms.openlocfilehash: d4c3aa690dd45a50e6f33d664fbc6cc4503e93f8
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7701568"
---
# <a name="troubleshoot-installation-issues-with-the-app-installer-file"></a>使用应用安装程序文件遇到的安装问题的疑难解答

如果在通过应用安装程序文件安装应用程序时发现任何问题，本主题将提供一些可能有帮助的故障排除指南。

## <a name="prerequisites"></a>必备条件

为了能在 Windows 10 中旁加载应用，用户设备必须满足下面的要求：

- 设备必须启用开发人员模式或旁加载应用。 有关详细信息，请参阅[启用设备进行开发](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)。
- 用于对程序包签名的证书必须受设备信任。 有关更多详细信息，请参阅下面的**受信任的证书**部分。
- Windows 10 版本必须支持 `.appinstaller` 文件方案和分发协议。

## <a name="common-issues"></a>常见问题

在用户计算机中第一次旁加载应用程序时会遇到一些常见问题。 接下来的几个部分介绍了最常见的问题及其解决方案。

### <a name="windows-version"></a>Windows 版本

每个 Windows 10 版本都改进了旁加载体验，在下表中可以找到每个主要版本中提供哪些功能。 如果你尝试使用你的 Windows 10 版本不支持的方法旁加载应用，将出现部署错误。

| 版本 | 旁加载注意事项 |
|---------|----------------|
| 内部版本 17134（2018 年 4 月更新，版本 1804）    | 通过 UNC/共享文件夹可以访问 `.appinstaller` 文件。 还提供可配置的更新检查。 |
| 内部版本 16299（Fall Creators Update 1709 版） | 引入 `.appinstaller` 文件以便为你的应用提供自动更新。 此版本仅支持 HTTP 终结点。 更新检查不可配置，每 24 小时进行一次。 |
| 内部版本 15063（Creators Update 1703 版）      | 应用安装程序应用能够从 Microsoft Store 下载应用依赖项（仅在发布模式下）。 |
| 内部版本 14393（周年更新 1607 版）   | 引入应用安装程序应用来安装 .appx 和 .appxbundle 文件，.appinstaller 文件不受支持。 |
| 内部版本 10586（11 月更新 1511 版）      | 只有通过 PowerShell 使用 [Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) 命令才能使用旁加载。 |
| 内部版本 10240（Windows 10 1507 版）           | 只有通过 PowerShell 使用 [Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) 命令才能使用旁加载。 |

### <a name="trusted-certificates"></a>受信任的证书

必须使用设备信任的证书对应用包进行签名。 默认情况下，Windows 操作系统信任常见证书颁发机构提供的证书，但如果证书不受信任，则必须在安装应用**之前**先在设备中安装证书。 若要信任证书，该证书必须存在于你的设备上的以下本地计算机证书存储区之一：

- 受信任的发布者
- 受信任人
- 受信任的根证书颁发机构（不推荐）

 >[!IMPORTANT]
 > 在本地计算机存储区中安装证书需要拥有管理权限。

### <a name="dependencies-not-installed"></a>未安装依赖项 

根据用于生成应用的应用程序平台，UWP 应用程序可以具有框架依赖项。 如果你使用的是 C# 或 VB，应用将需要 .NET 运行时和 .NET 框架包。 C++ 应用程序需要 VCLibs。

>[!IMPORTANT] 
> 如果应用包在发布模式配置中生成，将从 Microsoft Store 获取框架依赖项。 但是，如果应用是在调试模式配置中生成的，将从 `.appinstaller` 文件中指定的位置获取依赖项。

### <a name="files-not-accessible"></a>无法访问文件

在从 HTTP 终结点安装时，务必验证是否可以访问具有正确 MIME 类型的所有文件。 验证这些文件的最简单方法是遵循 Visual Studio 所生成 HTML 页面中提供的链接。 你必须检查这些文件：

- `.appinstaller` 文件，提供形式为 `application/xml`
- `.appx` 和 `.appxbundle` 文件，提供形式为 `application/vns.ms-appx`

## <a name="isolate-app-installer-app-issues"></a>隔离应用安装程序应用问题

如果应用安装程序应用无法安装应用，这些步骤有助于确定安装问题。

### <a name="verify-app-package-file-installation"></a>验证应用包文件安装

- 将应用包文件下载到本地文件夹，并尝试使用[Add-appxpackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) PowerShell 命令安装它。

- 将 `.appinstaller` 文件下载到本地文件夹，并尝试使用 `Add-AppxPackage -Appinstaller` PowerShell 命令进行安装。

## <a name="related-logs"></a>相关日志

应用部署基础结构提供 Windows 事件查看器中的调试日志。 这些日志位于： `Application and Services Logs->Microsoft->Windows->AppxDeployment-Server`



