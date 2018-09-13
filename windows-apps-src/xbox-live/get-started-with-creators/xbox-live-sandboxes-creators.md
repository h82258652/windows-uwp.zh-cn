---
title: Xbox Live 沙盒
author: PhillipLucas
description: Xbox Live 沙盒简介
ms.assetid: e7daf845-e6cb-4561-9dfa-7cfba882f494
ms.author: kevinasg
ms.date: 10/30/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b9175eda1d73a7678ac9fd304dc60ef228a57c7f
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3961828"
---
# <a name="xbox-live-sandboxes-introduction"></a>Xbox Live 沙盒简介

[Xbox Live 服务配置](xbox-live-service-configuration-creators.md)文章中说明了你必须在 [Windows 开发人员中心](http://dev.windows.com)中配置与你的作品有关的信息。 此信息包括统计数据、排行榜、本地化等。 对你的 Xbox Live 服务配置进行的更改需要先从开发人员中心发布到你的开发沙盒，然后 Xbox Live 的其余部分才能选取它们，并可以在你的作品中进行访问。

利用开发沙盒，你可以在隔离环境中处理对作品进行的更改。 沙盒具有多个优点：

1. 你可以迭代对作品更新的更改，而不影响创作中正在使用的版本。
2. 为安全起见，某些工具仅适用于开发沙盒。
3. 在未经授权访问你的沙盒的情况下，其他发布者看不到你正在进行的工作。

默认情况下，Xbox One 主机和 Windows 10 电脑均位于 RETAIL 沙盒中。 你将需要将电脑和/或 Xbox One 切换到开发沙盒，然后才能访问 Xbox Live 服务配置的那个版本。 如果你需要在 RETAIL 中进行某项测试，或者想要休息一下玩你最喜爱的 Xbox Live 游戏，请务必记得将设备改回到 RETAIL 沙盒。

## <a name="finding-out-about-your-sandbox"></a>了解你的沙盒

在你创建作品时，系统将为你创建沙盒。 通过在 **Windows 开发人员中心**中打开你的产品，然后导航到**服务** > **Xbox Live**，你可以找到你的沙盒 ID。 将会在页面顶部列出**沙盒 ID**。

![](../images/getting_started/devcenter_sandbox_id.png)

## <a name="switch-your-pcs-development-sandbox"></a>切换电脑的开发沙盒
你可以通过使用 Unity、Windows 设备门户 (WPD) 或通过命令行将电脑切换到开发沙盒。

### <a name="unity"></a>Unity

#### <a name="prerequisites"></a>先决条件
在 Unity 中切入和切出开发沙盒之前，需要执行以下操作：

1. [在 Unity 中配置 Xbox Live](configure-xbox-live-in-unity.md)

#### <a name="switch-sandboxes"></a>切换沙盒
利用内置的“Xbox Live 配置”窗口，你可以在开发沙盒和 RETAIL 沙盒之间轻松切换。 若要开始切换，请在菜单中转到 **Xbox Live** > **配置**。 你可以在**开发人员模式配置**部分看到当前沙盒。

1. 如果**开发人员模式**显示**启用**，则你当前处于与你的游戏相关联的开发沙盒中。 你可以单击**切换回零售模式**按钮切换出去。
2. 如果**开发人员模式**显示**禁用**，则你当前处于 RETAIL 沙盒中。 你可以单击**切换到开发人员模式**按钮切换进去。

![已启用 XBL](../images/unity/unity-xbl-dev-mode.PNG)

### <a name="windows-device-portal"></a>Windows 设备门户

#### <a name="prerequisites"></a>先决条件
在 Windows 设备门户 (WPD) 中切换沙盒之前，需要执行以下操作：

1. [在 Windows 桌面上设置设备门户](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)

#### <a name="switch-sandboxes"></a>切换沙盒

1. 通过在 Web 浏览器中连接到 **Windows 设备门户**以将其打开，如[在 Windows 桌面上设置设备门户](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)文章中所述。
2. 单击 **Xbox Live**。
3. 在文本字段中输入你的开发沙盒，然后单击**更改**。

![](../images/getting_started/wdp_switch_sandbox.png)

若要切换回 RETAIL，你可以在此处输入 RETAIL。

### <a name="command-line"></a>命令行

#### <a name="prerequisites"></a>先决条件
在通过命令行切入和切出开发沙盒之前，需要执行以下操作：

1. 从 [https://aka.ms/xboxliveuwptools](https://aka.ms/xboxliveuwptools) 下载 Xbox Live 工具包并解压缩。

#### <a name="switch-sandboxes"></a>切换沙盒
1. 在**管理员模式**下运行 SwitchSandbox.cmd 批处理文件。

在管理员模式下，运行此文件以切换你的沙盒。 第一个参数是沙盒。 例如，如果你要尝试切换到 MJJSQH.58 沙盒，则要使用此命令：

```cmd
SwitchSandbox.cmd MJJSQH.58
```

若要切换回 RETAIL，你只需提供该沙盒作为第二个参数即可。

```cmd
SwitchSandbox.cmd RETAIL
```

## <a name="switch-your-xbox-one-console-development-sandbox"></a>切换你的 Xbox One 主机开发沙盒

### <a name="using-xbox-dev-portal"></a>使用 Xbox 开发人员门户

你可以使用 Xbox 开发人员门户在主机上更改沙盒。 为此，请在主机上转到[开发人员主页](https://docs.microsoft.com/windows/uwp/xbox-apps/dev-home)并[启用设备门户](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-xbox)。 打开 Xbox 开发人员门户后：

2. 单击 **Xbox Live**。
3. 在文本字段中输入你的开发沙盒，然后单击**更改**。

![](../images/getting_started/xdp_switch_sandbox.png)

### <a name="using-xbox-one-console-ui"></a>使用 Xbox One 主机 UI

你可以直接在你的主机上使用[开发人员主页](https://docs.microsoft.com/windows/uwp/xbox-apps/dev-home)更改沙盒：

1. 单击**快速操作**下面的**更改沙盒**。
2. 输入沙盒 ID，然后单击**保存并重启**。
