---
title: PlayerAuthentication 预设可使用登录
description: Unity 插件 PlayerAuthentication 预设的概述
ms.date: 05/08/2018
ms.topic: get-started-article
keywords: xbox live、 xbox、 游戏、 uwp、 windows 10 中，一个 xbox、 unity
ms.openlocfilehash: ea161ff801e2004569d88d53c78ae963e91b4ce6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605732"
---
# <a name="easy-sign-in-with-the-playerauthentication-prefab"></a>轻松使用登录 PlayerAuthentication 预设

PlayerAuthentication 预设是最简单的方法将 Xbox Live 身份验证添加到你的标题。 只需三个简单步骤，若要从新的场景转到登录页。

1. 将 PlayerAuthentication 预设可拖到场景
2. 将 XboxLiveServices 预设拖到场景
3. 将 EventSystem 添加到场景 （从技术上讲 PlayerAuthentication 将为你创建一个如果 EventSystem 不存在，但将其添加是一个好习惯。）

就是这样。 可以现在登录播放器 XboxLive 在你的标题中通过单击 PlayerAuthentication 预设可在您的场景中。 在 Unity 中测试您的场景，通过单击播放按钮将导致在预设生成模拟数据，这是因为 Unity 播放机无法连接到 Xbox Live 服务。 若要查看实际的单一登录需要生成项目以在 Visual Studio 中本地运行。 如果你的标题已在配置合作伙伴中心和你授权 Microsoft 帐户/玩家代号中登录到你的标题，然后你将能够登录你在 Visual Studio 生成的授权帐户之一。

PlayerAuthentication 预设脚本有几个设置，可以从其视图检查器中进行处理。

![PlayerAuthentication 检查器屏幕截图](../images/unity/playerauthentication_prefab_inspector.JPG)

* 播放机数目-指示播放机链接到登录面板
* 主题-更改时的用户是登录或注销的登录面板的配色方案。此设置有一个浅色或深色选项。
* 启用控制器输入-此复选框的播放机可使用单一登录和注销使用 PlayerAuthentication 预设 Xbox 控制器。
* 游戏杆 Number-指示控制器可以登录这些输出该预设可使用。
* 登录的按钮的下拉列表的可选择用户登录 Xbox 控制器上的按钮。
* 注销的按钮的下拉列表的允许你选择哪个按钮上的 Xbox 控制器注销用户。

## <a name="multiplayer-scenario"></a>多玩家方案

除了单用户登录，你还可以使用多个 PlayerAuthentication 预设 Xbox One 控制台标题上实现本地之多人游戏。 通过添加预设的多个实例，并更改每个播放机号属性，您可以多个用户登录到你的标题。

> [!WARNING]
> 登录多个 gamertags 不允许在 Windows 10 电脑上。 若要在多个用户登录需要测试您的游戏在 Xbox 一个控制台上。

创建允许多玩家场景才略微更难使用 PlayerAuthentication 预设。

1. PlayerAuthentication 预设的实例拖到场景
2. 检查**启用控制器输入**框在预设的检查器中。
3. 请确保**播放机数目**并**游戏杆数**设置为 1。
4. 将分配**符号中按钮**从下拉列表菜单。
5. 将分配**登录按钮**从下拉列表菜单。
6. 拖动*第二个*PlayerAuthentication 预设可拖到场景的实例。
7. 检查**启用控制器输入**框在预设的检查器中。
8. 请确保**播放机数目**并**游戏杆数**设置为 2。
9. 将分配**符号中按钮**从下拉列表菜单。
10. 将分配**登录按钮**从下拉列表菜单。
11. 将 XboxLiveServices 预设拖到场景
12. 向场景添加 EventSystem

检查预设是按 play Unity 播放器中正常工作，并单击预设。 它们将返回模拟数据预计将在 Unity 播放器不能连接到 Xbox Live。 PlayerAuthentication 预设可配置为不同的玩家并已准备好生成您在 Visual Studio 中的游戏，游戏杆的两个实例，并用它可以正确地测试 Xbox 控制台上。 您的游戏构建后，将需要启用对您的游戏的多用户支持的 Visual Studio 中打开解决方案文件。
要实现此目的，请执行以下操作：

1. 搜索 package.appxmanifest.xml 文件在解决方案资源管理器
2. 右键单击该文件，然后选择查看代码
3. 下`<Properties><\/Properties>`部分中，添加以下行: < uap:SupportedUsers > 多个 <\/uap:SupportedUsers >。
4. 通过从 Visual Studio 中启动远程调试生成将部署到您的 Xbox 游戏。 您可以找到指令，以设置你的标题在 Xbox 上[设置在 Xbox 开发环境在 UWP](../../xbox-apps/development-environment-setup.md)一文。

> [!NOTE]
> 配置已更改的一部分可能看起来它启用多玩家，但仍有必要在单用户方案中运行您的游戏。

一旦您购买了 PlayerAuthentication prefab 正常运行，您可以了解有关脚本登录与项目的详细信息[在使用 Unity 中的单一登录管理器登录](sign-in-manager.md)。