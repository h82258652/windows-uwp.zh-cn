---
title: 常见问题
description: 有关 Xbox 上的 UWP 的常见问题解答。
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 265fe827-bd4a-48d4-b362-8793b9b25705
ms.localizationpriority: medium
ms.openlocfilehash: 036c3c1832bbb3e27a93671f399a9a97c7afaba3
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8922262"
---
# <a name="frequently-asked-questions"></a>常见问题

是否未按预期工作？ 查看此常见问题页面。 另外，请查看[已知问题](known-issues.md)主题和[开发通用 Windows 应用](https://go.microsoft.com/fwlink/?linkid=839446)论坛。 

### <a name="why-arent-my-games-and-apps-working"></a>为什么我的游戏和应用不工作？

如果你的游戏和应用不工作，或者如果你无法访问应用商店或 Live 服务，你可能在开发人员模式下运行。 若要确定你当前所处的模式，请按控制器上的**主页**按钮。 如果这将你导向“开发人员主页”而不是零售主页体验，则你处于开发人员模式。 如果想要玩游戏，你可以打开“开发人员主页”，然后通过使用**退出开发人员模式**按钮切换回零售模式。

### <a name="why-cant-i-connect-to-my-xbox-one-using-visual-studio"></a>为什么我无法使用 Visual Studio 连接到 Xbox One？

首先要确认是在开发人员模式下运行，而不是在零售模式下运行。 你无法连接到在零售模式下运行的 Xbox One。 若要确定你当前所处的模式，请按控制器上的**主页**按钮。 如果你看到金会员/Live 内容，则你处于零售模式并且需要运行“开发人员模式激活”应用以切换至开发人员模式。

> [!NOTE]
> 必须进行用户登录才能部署应用。

有关详细信息，请参阅此页面后面部分的[修复部署失败](#fixing-deployment-failures)。

### <a name="how-do-i-switch-between-retail-mode-and-developer-mode"></a>如何在零售模式和开发人员模式之间切换？

请遵循 [Xbox One 开发人员模式激活](devkit-activation.md)说明以了解有关这些状态的详细信息。

### <a name="how-do-i-know-if-i-am-in-retail-mode-or-developer-mode"></a>我如何知道我是在零售模式下还是在开发人员模式下？

请遵循 [Xbox One 开发人员模式激活](devkit-activation.md)说明以了解有关这些状态的详细信息。 

若要确定你当前所处的模式，请按控制器上的**主页**按钮。 
- 如果这将你导向“开发人员主页”，则你处于开发人员模式。
- 如果显示金会员/Live 内容，则你处于零售模式。

### <a name="will-my-games-and-apps-still-work-if-i-activate-developer-mode"></a>当我激活开发人员模式时，我的游戏和应用是否仍可以运行？

是的，你可以从开发人员模式切换到零售模式，同时可以玩游戏。 有关详细信息，请参阅 [Xbox One 开发人员模式激活](devkit-activation.md)页。 

### <a name="can-i-develop-and-publish-x86-apps-for-xbox"></a>我能否开发和发布适用于 Xbox 的 x86 应用？
Xbox 不再支持 x86 应用开发或将 x86 应用商店提交到应用商店。 

### <a name="will-i-lose-my-games-and-apps-or-saved-changes"></a>我的游戏和应用或保存的更改是否会丢失？

如果决定退出开发人员计划，不会丢失安装的游戏和应用。 此外，只要你在玩游戏时处于联机状态，保存的游戏就全部保存在你的 Live 帐户云配置文件中，因此不会丢失它们。

### <a name="how-do-i-leave-the-developer-program"></a>如何退出开发人员计划？

有关如何退出开发人员计划的详细信息，请参阅 [Xbox One 开发人员模式停用](devkit-deactivation.md)主题。

### <a name="i-sold-my-xbox-one-and-left-it-in-developer-mode-how-do-i-deactivate-developer-mode"></a>我已出售我的 Xbox One，并且将它保留在开发人员模式下。 如何停用开发人员模式？

如果你无法再访问你的 Xbox one，可以在 Windows 合作伙伴中心中停用它。 有关详细信息，请参阅主题中的[Xbox One 开发人员模式停用](devkit-deactivation.md#deactivate-your-console-using-partner-center)**停用主机使用合作伙伴中心**部分。 

### <a name="i-left-the-developer-program-using-partner-center-but-im-in-still-developer-mode-what-do-i-do"></a>我退出开发人员计划使用合作伙伴中心，但我仍处于开发人员模式。 我该怎么办？

启动“开发人员主页”，然后选择**退出开发人员模式**按钮。 这将在零售模式下重新启动主机。 

### <a name="can-i-publish-my-app"></a>我是否可以发布我的应用？

如果你有一个[开发者帐户](https://developer.microsoft.com/store/register)，可以通过合作伙伴中心中[发布应用](../publish/index.md)。 在零售 Xbox One 主机上创建和测试的 UWP 应用将经过当今 Windows 执行的相同引入、审阅和发布过程，以及符合当今 Xbox One 标准的其他审阅。

### <a name="can-i-publish-my-game"></a>我是否可以发布我的游戏？

你可以在开发人员模式下使用 UWP 和你的 Xbox One，在 Xbox One 上生成和测试你的游戏。 若要发布 UWP 游戏，你必须注册 [ID@XBOX](http://www.xbox.com/Developers/id) 或参与 [Xbox Live Creators 计划](https://developer.microsoft.com/games/xbox/xboxlive/creator)。 有关详细信息，请参阅[开发人员计划概述](https://developer.microsoft.com/games/xbox/docs/xboxlive/get-started/developer-program-overview.html)。

### <a name="will-the-standard-game-engines-work"></a>标准游戏引擎是否工作？

请查看此版本的[已知问题](known-issues.md)页。

### <a name="what-capabilities-and-system-resources-are-available-to-uwp-games-on-xbox-one"></a>哪些功能和系统资源将提供给 Xbox One 上的 UWP 游戏？ 

有关信息，请参阅 [Xbox One 上的 UWP 应用和游戏的系统资源](system-resource-allocation.md)。

### <a name="if-i-create-a-directx-12-uwp-game-will-it-run-on-my-xbox-one-in-developer-mode"></a>如果我创建了 DirectX 12 UWP 游戏，那么它是否可以在开发人员模式下在我的 Xbox One 上运行？

有关信息，请参阅 [Xbox One 上的 UWP 应用和游戏的系统资源](system-resource-allocation.md)。

### <a name="will-the-entire-uwp-api-surface-be-available-on-xbox"></a>在 Xbox 上整个 UWP API 图面是否可用？

请查看此版本的[已知问题](known-issues.md)页。

### <a name="fixing-deployment-failures"></a>修复部署失败

如果无法从 Visual Studio 部署你的应用，以下步骤可能有助于你修复该问题。 如果遇到问题，请通过论坛寻求帮助。

> [!NOTE]
> 必须进行用户登录才能部署应用。 如果收到 0x87e10008 错误消息，请确保已进行用户登录，然后再试一次。

如果 Visual Studio 无法连接到你的 Xbox One：

1. 请确保你在开发人员模式下（已在本页的前面部分讨论过）。
2. 确保已正确设置了你的开发电脑。 是否按照 [Xbox One 上的 UWP 应用开发入门](getting-started.md)中的*全部*指示进行操作？ 

3. 如果尚未执行此操作，请通读[开发环境设置](development-environment-setup.md)主题和 [Xbox One 工具简介](introduction-to-xbox-tools.md)主题。

4. 确保可以从你的开发电脑对你的主机 IP 地址执行“ping”操作。
  > [!NOTE]
  > 为了获得最佳部署性能，我们建议你采用有线连接方式连接你的主机。

5. 确保使用的是**调试**选项卡上的“身份验证”下拉列表中的“通用（未加密协议）”。有关更多详细信息，请参阅[开发环境设置](development-environment-setup.md)。


### <a name="if-im-building-an-app-using-htmljavascript-how-do-i-enable-gamepad-navigation"></a>如果我要使用 HTML/JavaScript 生成应用，如何启用手柄导航？

TVHelpers 是一套 JavaScript 和 XAML/C# 示例和库，可帮助你使用 JavaScript 和 C# 生成出色的 Xbox One 和电视体验。 TVJS 是一个库，可帮助你生成适用于 Xbox One 的高级 UWP 应用。 TVJS 包括对自动控制器导航、丰富媒体播放、搜索以及更多内容的支持。 将 TVJS 用于托管的 Web 应用就像将其用于拥有对 Windows 运行时 API 的完全访问权限的打包 Web UWP 应用一样轻松。

有关详细信息，请参阅 [TVHelpers](https://github.com/Microsoft/TVHelpers) 项目和 [wiki](https://github.com/Microsoft/TVHelpers/wiki) 项目。

## <a name="see-also"></a>另请参阅
- [Xbox One 上的 UWP 的已知问题](known-issues.md)
- [Xbox One 上的 UWP](index.md)
- [Xbox One 上的 UWP](index.md)
