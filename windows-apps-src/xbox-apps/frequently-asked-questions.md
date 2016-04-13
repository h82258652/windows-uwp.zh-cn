---
title: 常见问题
description: 有关 Xbox 上的 UWP 的常见问题。
area: Xbox
---

# 常见问题

是否未按预期工作？ 
查看此常见问题页面。 
另外，请查看[已知问题](known-issues.md)主题和[开发通用 Windows 应用](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop)论坛。 

### 为什么我的游戏和应用不工作？

如果你的游戏和应用不工作，或者如果你无法访问应用商店或 Live 服务，你可能在开发人员模式下运行。 
当你选择“主页”并在你屏幕的右侧看到较大的“开发人员主页”磁贴（而不是常规的 Gold/Live 内容）时，可以得知你是在开发人员模式下运行。 
如果想要玩游戏，你可以打开“开发人员主页”，然后通过使用**“退出开发人员模式”**按钮切换回零售模式。

### 为什么我无法使用 Visual Studio 连接到 Xbox One？

首先要确认是在开发人员模式下运行，而不是在零售模式下运行。 
你无法连接到在零售模式下运行的 Xbox One。 
只需按**“主页”**按钮并查看你屏幕右侧的“开发人员主页”磁贴，即可对此问题进行检查。 
如果你看到的不是该磁贴，而是金会员/Live 内容，这表明你处于零售模式。 
若要切换到开发人员模式，需要运行“开发人员模式激活”应用。

有关详细信息，请参阅本页面后面的[修复部署失败](frequently-asked-questions.md#fixing-deployment-failures)。

### 如何在零售模式和开发人员模式之间切换？

请遵循 [Xbox One 开发人员模式激活](devkit-activation.md)说明以了解有关这些状态的详细信息。

### 我如何知道我是在零售模式下还是在开发人员模式下？

请遵循 [Xbox One 开发人员模式激活](devkit-activation.md)说明以了解有关这些状态的详细信息。 

只需按**“主页”**按钮并查看你的屏幕右侧，即可对此问题进行检查。 
如果你在开发人员模式下，你将在右侧看到“开发人员主页”磁贴。 
如果你在零售模式下，你将看到常规的金会员/Live 内容。

### 当我激活开发人员模式时，我的游戏和应用是否仍可以运行？

是的，你可以从开发人员模式切换到零售模式，同时可以玩游戏。 
有关详细信息，请参阅 [Xbox One 开发人员模式激活](devkit-activation.md)页。 

> **警告** Xbox 开发人员预览版系统更新将包括实验性的软件和提前预发布的软件。 
这意味着某些热门游戏和应用将无法按预期运行，并且可能会遇到偶发的崩溃和数据丢失。

### 我的游戏和应用或保存的更改是否会丢失？

当你决定退出开发人员预览版计划时，可能需要恢复出厂设置，这将擦除你主机上的所有内容。 
如果发生这种情况，需要重新安装所有游戏和应用。 
只要你在玩游戏时处于联机状态，你保存的游戏将全部保存在你的 Live 帐户云配置文件中，因此不会丢失它们。

### 如何退出开发人员预览版？

有关如何退出开发人员预览版的详细信息，请参阅 [Xbox One 开发人员模式停用](devkit-deactivation.md)主题。

### 我已出售我的 Xbox One，并且将它保留在开发人员模式下。 如何停用开发人员模式？

如果你不再能够访问你的 Xbox One，可以在 Windows 开发人员中心中停用它。 
有关详细信息，请参阅 [Xbox One 开发人员模式停用](devkit-deactivation.md#deactivate-your-console-through-windows-dev-center)主题中的**使用 Windows 开发人员中心停用主机**部分。

### 我已使用 Windows 开发人员中心退出了开发人员预览版，但我仍处于开发人员模式下。 我该怎么办？

启动“开发人员主页”，然后选择**“退出开发人员模式”**按钮。 
这将在零售模式下重新启动主机。 

### 我是否可以发布我的应用？

今年晚些时候将可以通过开发人员中心发布应用。 
在零售 Xbox One 上创建和测试的 UWP 应用将经过当今 Windows 执行的相同引入、审阅和发布过程，以及符合当今 Xbox One 标准的其他审阅。

### 我是否可以发布我的游戏？

你可以在开发人员模式下使用 UWP 和你的 Xbox One，以便在 Xbox One 上生成和测试你的游戏。 
若要发布 UWP 游戏，你必须注册 [ID@XBOX](http://www.xbox.com/en-us/Developers/id)。 
[ID@XBOX](http://www.xbox.com/en-us/Developers/id) 向开发人员提供适用于其游戏的 Xbox Live API 的完整访问权限，包括玩家分数和成就 
以及在设备之间畅玩多人游戏、云存储和 Xbox One 上 Xbox Live 的所有功能。 
[ID@XBOX](http://www.xbox.com/en-us/Developers/id) 还可以为需要访问 Xbox One 硬件最大潜能的游戏提供对 Xbox One 开发工具包的访问权限。

### 标准的游戏引擎会奏效吗？

请查看此预览版本的[已知问题](known-issues.md)页。

### 哪些功能和系统资源将提供给 Xbox One 上的 UWP 游戏？ 

有关信息，请参阅 [Xbox One 上 UWP 应用和游戏的系统资源](system-resource-allocation.md)。

### 如果我创建了 DirectX 12 UWP 游戏，那么它是否可以在开发人员模式下在我的 Xbox One 上运行？

有关信息，请参阅 [Xbox One 上 UWP 应用和游戏的系统资源](system-resource-allocation.md)。

### 在 Xbox 上整个 UWP API 图面是否可用？

请查看此预览版本的[已知问题](known-issues.md)页。

### 修复部署失败

如果无法从 Visual Studio 部署你的应用，以下步骤可能有助于你修复该问题。 
如果你遇到问题，请通过论坛寻求帮助。

如果 Visual Studio 无法连接到你的 Xbox One：

1. 请确保你在开发人员模式下（已在本页的前面部分讨论过）。
2. 确保已正确设置了你的开发电脑。 是否按照 [Xbox One 上的 UWP 应用开发入门](getting-started.md)中的*全部*指示进行操作？ 

3. 如果尚未执行此操作，请通读[开发环境设置](development-environment-setup.md)主题和 [Xbox One 工具简介](introduction-to-xbox-tools.md)主题。

4. 确保使用有线网络连接到 Xbox One。 我们看到某些无线终结点发生性能和连接性问题。

5. 确保可以从你的开发电脑“ping”你的主机 IP 地址。

6. 确保使用的是**“调试”**选项卡上身份验证下拉列表中的“通用(未加密协议)”。 有关更多详细信息，请参阅[开发环境设置](development-environment-setup.md)。

7. 确保未遇到 PIN 配对问题；请参阅[常见问题](frequently-asked-questions.md)主题中的“Visual Studio/Xbox PIN 配对失败”。

如果 Visual Studio 可以连接，但部署失败（例如，收到此错误消息：“DEP0700：注册应用失败。(0x80073cf9)”）：

1. 确保未安装你的应用，方法是从 Xbox One shell 的集合应用中卸载它。 

> **注意** 从 Windows Device Portal (WDP) 中卸载你的应用无法解决此问题。

2. 如果问题仍然存在，请在集合应用中卸载你的应用或游戏、退出开发人员模式、重新启动到零售模式，然后切换回开发人员模式。 
这将清除开发人员存储。

3. 如果问题仍然存在，请按照上述步骤进行操作，然后使用**“重置并保留我的游戏和应用”**以删除 Xbox One 上存储的任何状态。 
转到“设置”>“系统”>“主机信息和更新”>“重置主机”，然后选择**“重置并保留我的游戏和应用”**按钮。

> **警告** 执行此操作将删除 Xbox One 上保存的所有设置，包括无线设置、用户帐户和尚未保存到云存储的任何游戏进度。

> **警告** 请勿选择**“重置和删除所有内容”**按钮。
这将删除你的所有游戏、应用、设置和内容、停用开发人员模式，然后从开发人员预览版组中删除你的主机。

### 当我使用 HTML/JavaScript 生成应用时，如何启用游戏手柄导航？

TVHelpers 是一套 JavaScript 和 XAML/C# 示例和库，可帮助你使用 JavaScript 和 C# 生成出色的 Xbox One 和电视体验。 
TVJS 是一个库，可帮助你生成适用于 Xbox One 的高级 UWP 应用。 TVJS 包括对自动控制器导航、丰富媒体播放、搜索以及更多内容的支持。 
可以将 TVJS 与你的托管 Web 应用结合使用，就像使用具有 Windows 运行时 API 的完整访问权限的打包 Web UWP 应用一样简单。

有关详细信息，请参阅 [TVHelpers](https://github.com/Microsoft/TVHelpers) 项目和 [wiki](https://github.com/Microsoft/TVHelpers/wiki) 项目。

## 另请参阅
- [Xbox One 开发人员预览版上的 UWP 已知问题](known-issues.md)
- [Xbox One 上的 UWP](index.md)


<!--HONumber=Mar16_HO5-->


