---
title: Xbox Live 沙盒
description: Xbox Live 沙盒简介
ms.assetid: e7daf845-e6cb-4561-9dfa-7cfba882f494
ms.date: 10/30/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3f2e5496c9aab2629591a121850685e7f5a1b2fe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590222"
---
# <a name="xbox-live-sandboxes-introduction"></a>Xbox Live 沙盒简介

在中[Xbox Live 服务配置](xbox-live-service-configuration-creators.md)文章中，您必须在标题中有关配置信息已介绍[合作伙伴中心](https://partner.microsoft.com/dashboard)。 此信息包括统计数据、排行榜、本地化等。 对 Xbox Live 服务配置更改需要发布从合作伙伴中心到开发沙盒之前所做的更改获取的 Xbox Live rest 的速度和可访问你的标题中。

利用开发沙盒，你可以在隔离环境中处理对作品进行的更改。 沙盒具有多个优点：

1. 你可以迭代对作品更新的更改，而不影响创作中正在使用的版本。
2. 为安全起见，某些工具仅适用于开发沙盒。
3. 在未经授权访问你的沙盒的情况下，其他发布者看不到你正在进行的工作。

默认情况下，Xbox One 主机和 Windows 10 电脑均位于 RETAIL 沙盒中。 你将需要将电脑和/或 Xbox One 切换到开发沙盒，然后才能访问 Xbox Live 服务配置的那个版本。 如果你需要在 RETAIL 中进行某项测试，或者想要休息一下玩你最喜爱的 Xbox Live 游戏，请务必记得将设备改回到 RETAIL 沙盒。

## <a name="finding-out-about-your-sandbox"></a>了解你的沙盒

在你创建作品时，系统将为你创建沙盒。 可以通过打开您的产品中找到您的沙盒 ID**合作伙伴中心**并导航到**Services** > **Xbox Live**。 将会在页面顶部列出**沙盒 ID**。

![](../images/getting_started/devcenter_sandbox_id.png)

## <a name="switch-your-pcs-development-sandbox"></a>切换电脑的开发沙盒
你可以通过使用 Unity、Windows 设备门户 (WPD) 或通过命令行将电脑切换到开发沙盒。

### <a name="unity"></a>Unity

#### <a name="prerequisites"></a>必备条件
在 Unity 中切入和切出开发沙盒之前，需要执行以下操作：

1. [配置 Xbox Live 在 Unity 中](configure-xbox-live-in-unity.md)

#### <a name="switch-sandboxes"></a>切换沙盒
利用内置的“Xbox Live 配置”窗口，你可以在开发沙盒和 RETAIL 沙盒之间轻松切换。 若要开始切换，请在菜单中转到 **Xbox Live** > **配置**。 你可以在**开发人员模式配置**部分看到当前沙盒。

1. 如果**开发人员模式**显示**启用**，则你当前处于与你的游戏相关联的开发沙盒中。 你可以单击**切换回零售模式**按钮切换出去。
2. 如果**开发人员模式**显示**禁用**，则你当前处于 RETAIL 沙盒中。 你可以单击**切换到开发人员模式**按钮切换进去。

![已启用 XBL](../images/unity/unity-xbl-dev-mode.PNG)

### <a name="windows-device-portal"></a>Windows Device Portal

#### <a name="prerequisites"></a>必备条件
在 Windows 设备门户 (WPD) 中切换沙盒之前，需要执行以下操作：

1. [在 Windows 桌面上安装设备门户](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)

#### <a name="switch-sandboxes"></a>切换沙盒

1. 通过在 Web 浏览器中连接到 **Windows 设备门户**以将其打开，如[在 Windows 桌面上设置设备门户](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)文章中所述。
2. 单击 **Xbox Live**。
3. 在文本字段中输入你的开发沙盒，然后单击**更改**。

![](../images/getting_started/wdp_switch_sandbox.png)

若要切换回 RETAIL，你可以在此处输入 RETAIL。

### <a name="command-line"></a>命令行

#### <a name="prerequisites"></a>必备条件
在通过命令行切入和切出开发沙盒之前，需要执行以下操作：

1. 从 [https://aka.ms/xboxliveuwptools](https://aka.ms/xboxliveuwptools) 下载 Xbox Live 工具包并解压缩。

#### <a name="switch-sandboxes"></a>切换沙盒
1. 在**管理员模式**下运行 SwitchSandbox.cmd 批处理文件。

在管理员模式下，运行此文件以切换你的沙盒。 第一个变量是沙盒。 例如，如果你要尝试切换到 MJJSQH.58 沙盒，则要使用此命令：

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

### <a name="sign-in-with-the-xbox-app"></a>在使用 Xbox 应用登录

切换后进行开发的电脑，要正确沙盒用于你的标题将想要验证，你已登录到 Xbox Live 的符合条件的测试帐户。 这可以通过登录到[Xbox Live 应用](https://www.xbox.com/en-US/xbox-app)。 在开发环境开始使用所需的沙盒 Xbox 应用后将登录用户使用相同的约束作为任何其他 Xbox Live 服务在沙盒上运行。 这使得用于验证在沙箱使用有效的帐户。
