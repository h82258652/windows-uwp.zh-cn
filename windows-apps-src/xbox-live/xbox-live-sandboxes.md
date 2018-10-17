---
title: Xbox Live 沙盒
author: KevinAsgari
description: 了解用于 Xbox Live 开发的沙盒。
ms.assetid: a5acb5bf-dc11-4dff-aa94-6d1f01472d2a
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 63447a9423ab65f79f034877a1c74c1eea75c78c
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2018
ms.locfileid: "4750129"
---
# <a name="xbox-live-sandboxes-intro"></a>Xbox Live 沙盒简介

[Xbox Live 服务配置](xbox-live-service-configuration.md)中说明了你必须联机配置与你的作品有关的信息，此操作通常在 [Windows 开发人员中心](http://dev.windows.com)上执行。  此信息包括你的作品要显示的排行榜和玩家可解锁的成就，以及匹配配置等内容。

在对你的服务配置进行更改时，你需要先通过开发人员中心发布这些更改，然后 Xbox Live 的其余部分才能选取它们，并显示给你的作品。

你可以发布到所谓的开发沙盒。  利用这些沙盒，你可以在隔离环境中执行对作品的更改。  这些沙盒具有很多优点，如以下部分中所述。

默认情况下，Xbox One 主机和 Windows 10 电脑均位于 RETAIL 沙盒中。

## <a name="benefits"></a>优点

开发沙盒具有以下几个优点：


1. 你可以迭代对作品更新的更改，而不影响当前可用的版本。
2. 为安全起见，某些工具仅适用于开发沙盒。
3. 在你的团队中，某些开发人员可能需要为服务配置更改创建分支并对其进行测试，而不影响开发中的主要服务配置。
4. 在未经授权访问你的沙盒的情况下，其他发布者看不到你正在进行的工作。

你还可以**有选择**地创建测试帐户。  如果你不想使用常规 Xbox Live 帐户测试你的作品，或者需要多个帐户测试方案，例如社交（如查看好友的统计数据）或多人游戏，则可以使用这些测试帐户。

测试帐户只能登录到开发沙盒，它们将在以下部分中进行介绍。

## <a name="finding-out-about-your-sandbox"></a>了解你的沙盒

绝大多数的开发人员都只需要一个沙盒。  幸运的是，在你创建作品时，系统将为你创建沙盒。

1. 你可通过转到下面的“开发人员中心”仪表板了解沙盒：
![](images/getting_started/first_xbltitle_dashboard.png)

1. 然后，单击你的作品：
![](images/getting_started/first_xbltitle_dashboard_overview.png)

1. 最后，在左侧菜单中，单击“服务->Xbox Live”
![](images/getting_started/first_xbltitle_leftnav.png)

1. 现在，你便可以看到沙盒，如下所列
![](images/getting_started/devcenter_sandbox_id.png)

## <a name="how-your-sandbox-impacts-your-workflow"></a>沙盒对工作流有何影响

通常，可以采用以下方式使用沙盒：

1. （一次性）将电脑或 Xbox One 切换到你的开发沙盒。
2. （多次）对服务配置进行更改时，你要将更改发布到开发沙盒。  服务配置更改就是诸如定义成就、添加排行榜或修改多人游戏会话模板之类的操作。
3. （几次）如果你要与其他团队成员合作，则可以为他们提供访问你的沙盒的权限
4. （一次性）如果你需要在 RETAIL 中进行某项测试，或者想要休息一下玩你最喜爱的 Xbox 游戏，则需要将沙盒切换回 RETAIL。

这些方案将在下文中进行更详细的介绍。  该过程在电脑和主机上有所不同，因此，将针对每个方案分不同部分进行介绍。

## <a name="switch-your-pcs-development-sandbox"></a>切换电脑的开发沙盒

如果你要切换电脑的开发沙盒，则建议的做法是使用 Windows Device Portal (WDP)。  你还可以通过命令行执行此操作。  我们将介绍这两种方法。

### <a name="windows-device-portal"></a>Windows Device Portal

如果你尚未在电脑上启用 WDP，请按照以下说明进行操作。 [在 Windows 桌面上设置 Device Portal](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)

执行完此操作后，在 Web 浏览器中连接 Windows 开发人员门户以将其打开，如上文中所述。

然后，单击“Xbox Live”，以转到相应部分，如下所示。

![](images/getting_started/wdp_switch_sandbox.png)

然后，你可以输入通过执行*查找你的沙盒*中的步骤获得的沙盒，并单击“更改”。

若要切换回 RETAIL，你可以在此处输入 RETAIL。

### <a name="powershell-module"></a>PowerShell 模块

[Xbox Live PowerShell 模块](https://github.com/Microsoft/xbox-live-powershell-module/blob/master/docs/XboxLivePsModule.md)XboxlivePSModule 包含各种实用程序，可为 Xbox Live 开发提供帮助，包括在电脑或主机上更改沙盒。

* 若要从 [PowerShell 库](https://www.powershellgallery.com/packages/XboxlivePSModule)使用该模块，请打开 PowerShell 窗口：
    1. 下载并安装该模块： `Install-Module XboxlivePSModule -Scope CurrentUser`
    2. 通过运行开始使用 `Import-Module XboxlivePSModule`
    3. 运行 cmdlet，即 Set-XblSandbox XDKS.1 或 Get-XblSandbox

* 若要使用它从 zip 文件在[https://aka.ms/xboxliveuwptools](https://aka.ms/xboxliveuwptools)，打开 PowerShell 窗口，
    1. 运行 `Import-Module <path to unzipped folder>\XboxLivePsModule\XboxLivePsModule.psd1`
    2. 运行 cmdlet，即 Set-XblSandbox XDKS.1 或 Get-XblSandbox

### <a name="command-prompt-script"></a>命令提示符脚本

从 [https://aka.ms/xboxliveuwptools](https://aka.ms/xboxliveuwptools) 下载 Xbox Live 工具包并解压缩。  你将会在其中找到 SwitchSandbox.cmd 批处理文件。

在管理员模式下，运行此文件以切换你的沙盒。  第一个参数是沙盒。  例如，如果你要尝试切换到 XDKS.1 沙盒，则应执行以下操作：

```
SwitchSandbox.cmd XDKS.1
```

若要切换回 RETAIL，你只需提供该沙盒作为第二个参数即可。

```
SwitchSandbox.cmd RETAIL
```

## <a name="switch-your-xbox-one-console-development-sandbox"></a>切换你的 Xbox One 主机开发沙盒

### <a name="using-windows-dev-portal"></a>使用 Windows 开发人员门户

你可以使用 Windows 开发人员门户在主机上更改沙盒。  为此，请转到主机上的“开发人员主页”，然后启用该沙盒。

此后，你可以在电脑上的 Web 浏览器中键入 IP 地址，以连接到你的主机。  然后，你可以单击“Xbox Live”，并在此处的文本框中输入该沙盒。

### <a name="using-xbox-one-manager"></a>使用 Xbox One 管理器

使用 Xbox One 管理器，你可以通过电脑对主机的某些方面进行管理。  其中包括重新启动、管理已安装的应用，以及更改你的沙盒。

右键单击你要更改其沙盒的主机，然后转到“设置...”

然后，你可以在此处输入沙盒。

### <a name="using-xbox-one-console-ui"></a>使用 Xbox One 主机 UI

如果你要直接通过主机更改你的开发沙盒，则可以转到“设置”。  然后，转到“开发人员设置”，此时，将显示一个用于更改沙盒的选项。

## <a name="sandbox-uses"></a>沙盒的使用

### <a name="data-that-is-sandboxed"></a>沙盒化的数据
在开发过程中，可以使用沙盒功能管理团队中的开发人员之间的访问权限。  例如，你可能需要在开发团队与测试人员之间隔离数据。

沙盒数据包括：
- 用户的成就、排行榜和统计数据。  在一个沙盒中为用户累积的成就不会转换到另一个沙盒中。
- 多人游戏和匹配。  用户不能与其他沙盒中的玩家一起玩多人游戏。
- 服务配置  如果将新成就添加到一个沙盒中的作品，则该成就不显示在另一个沙盒中。  这适用于所有的服务配置数据。

非沙盒化的数据主要是社交信息。  因此，例如，如果用户关注另一个用户，则该关系与沙盒无关。

### <a name="examples"></a>示例
下面将列举一些示例，以帮助说明你可能需要使用多个沙盒的原因以及所带来的一些好处。

> **注意**：如果你加入 Xbox 创意者计划，则只能拥有一个沙盒。  如果你需要创建多个沙盒，请申请加入 ID@Xbox 计划。

#### <a name="service-config-isolation"></a>服务配置隔离
如上所述，服务配置特定于沙盒。  因此，你可能既有*开发*沙盒，又有*测试*沙盒。  向测试人员提供你的作品版本时，你需要将[服务配置](xbox-live-service-configuration.md)发布到*测试*沙盒。

与此同时，你可以向*开发*沙盒中添加成就，或不同的多人游戏会话类型，而不影响测试人员正在查看的服务配置。

#### <a name="multiplayer"></a>多人游戏
以上述*开发*沙盒和*测试*沙盒为例。  或许，你的服务配置在沙盒之间是相同的，但是，你的开发人员要创建多人游戏功能，并希望测试相互之间是否匹配。  你的测试人员也在测试多人游戏。

在这种情况下，你的开发人员可能不希望 Xbox Live 匹配服务将他们与测试人员匹配，因为他们要单独调试问题。  为了防止出现这种情况，最好的办法是让你的开发人员位于*开发*沙盒中，而测试人员位于单独的*测试*沙盒中。  这样可确保两组处于隔离状态。

## <a name="advanced"></a>高级

为了简化你的开发过程，可从默认沙盒入手，并审慎地添加新沙盒。

在了解了访问控制之后，如果需要进一步隔离数据，你可以参阅[高级 Xbox Live 沙盒](advanced-xbox-live-sandboxes.md)一文。  
