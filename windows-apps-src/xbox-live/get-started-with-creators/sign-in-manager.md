---
title: 使用在 Unity 中 SignInManager 登录
description: Unity 插件登录 Manager 概述
ms.date: 05/08/2018
ms.topic: get-started-article
keywords: xbox live、 xbox、 游戏、 uwp、 windows 10 中，一个 xbox、 unity
ms.openlocfilehash: e6d066fe7792912f8918cb139d45ff05d105feaa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641722"
---
# <a name="scripting-sign-in"></a>在登录脚本

若要将登录添加到您自己自定义的游戏对象，将需要一个 GameObject 到对其编写脚本。 让我们假设，PlayerAuthentication 预设不适合您的游戏和你想要有自己的登录面板，本文将引导您完成向你的标题添加登录逻辑的基本步骤。

## <a name="sign-in-with-the-signinmanager"></a>使用 SignInManager 登录

Xbox Live Unity 插件包含一个脚本，以便`SignInManager`下的文件路径**资产 >> XboxLive >> 脚本 >> SignInManager.cs**。 管理器是一个单独的类可调用该窗体任意位置在您的标题中的标题的引用*实例*的`SignInManager`。 这*实例*不需要进行初始化和您可以使用 it 只要您的游戏开始。 您可以访问的所有其公共属性和通过引用函数*实例*作为`SignInManager.Instance`。

`SignInManager`包含所有所需的代码用于管理你的标题的身份验证，这包括登录、 注销，并获取有关哪些用户的信息的播放机以登录。

### <a name="calls-and-results"></a>调用和结果

`SignInManager`有三个异步协同例程函数`SignInPlayer(int playerNumber)`， `SignOutPlayer(int playerNumber)`，和`SwitchUser(int playerNumber)`，该触发器事件的函数收集的调用的结果并执行相应操作。 可以将相应的函数添加到您的脚本并将其分配给`SignInManager.Instance`的回调列表。 事件的功能`OnPlayerSignIn(int playerNumber, UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`， `OnPlayerSignOut(int playerNumber, UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`， `OnAnyPlayerSignIn(UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`，和`OnAnyPlayerSignOut(UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`。 每个上事件函数侦听其名称中所述的事件。 可以向管理器的回调列表添加你自己的函数，在标题的`Start()`用下面的代码的函数。

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

void Start () {
    try
    {
        SignInManager.Instance.OnPlayerSignOut(this.playerNumber, this.OnPlayerSignOut);
        SignInManager.Instance.OnPlayerSignIn(this.playerNumber, this.OnPlayerSignIn);
    }
    catch (Exception ex)
    {
        Debug.LogWarning(ex.Message);
    }

}
```

此代码片段将添加与此 GameObject playerNumber 相关联的播放器的单一登录和注销侦听器。 此 GameObject`OnPlayerSignIn`函数时将会调用`SignInManager`检测到的登录尝试已完成并将其`OnPlayerSignOut`SignInManager 检测到注销时，将调用的函数。在你的 GameObject 事件函数必须具有返回类型和参数以匹配由 SignInManager 调用的函数类型。 同时`OnPlayerSignIn`并`OnPlayerSignOut`是其中需要 void 函数`XboxLiveUser`， `XboxLiveAuthStatus`，和一个字符串作为其参数。 函数的 shell 可能看起来如下所示：

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

private void OnPlayerSignIn(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
}

private void OnPlayerSignOut(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
}
```

在这两个函数中检查`XboxLiveAuthStatus`，确保您调用`SignInManager.Instance`是否成功。 在成功调用`XboxLiveUser`将是`XboxLiveUser`，在我们退出已签名的`SignInManager`。 当调用不会成功`errorMessage`字符串将包含失败的原因的详细信息。

添加几行代码来检查成功的调用将导致代码如下所示：

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

private void OnPlayerSignIn(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
    if(authStatus == XboxLiveAuthStatus.Succeeded)
    {
        this.xboxLiveUser = xboxLiveUserParam; //store the xboxLiveUser SignedIn
        this.signedIn = true;
    }
    else
    {
        if (XboxLiveServicesSettings.Instance.DebugLogsOn)
        {
            Debug.LogError(errorMessage); //Log the error message in case of unsuccessful call. 
        }
    }
}

private void OnPlayerSignOut(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
    if (authStatus == XboxLiveAuthStatus.Succeeded)
    {
        this.xboxLiveUser = null;
        this.signedIn = false;
    }
    else
    {
        if (XboxLiveServicesSettings.Instance.DebugLogsOn)
        {
            Debug.LogError(errorMessage);
        }
    }
}
```

通过调用登录中，捕获的结果生成的事件，可以为你的标题中处理登录和注销。

## <a name="get-signed-in-player-information"></a>获取登录播放器信息

除了登录到该服务的播放机 SignInManager 跟踪的所有已登录用户。 在 PC 上这将限制为单个登录播放机，并在 Xbox 上它被限制为 16。 你可以检查如何接近该限制您是通过比较的结果`SignInManager.Instance.GetCurrentNumberOfPlayers()`的结果`SignInManager.Instance.GetMaximumNumberOfPlayers()`。 SignInManager 已登录的播放器，通过该播放器编制索引的字典*playerNumber*。 可以使用此播放机可访问的一些基本信息检索其关联`XboxLiveUser`。

```csharp
if (SignInManager.Instance.GetPlayer(this.playerNumber).IsSignedIn) // If there is a player signed in for this gameObjects player number
            {
                this.displayedGamertag = SignInManager.Instance.GetPlayer(this.playerNumber).Gamertag; // Set that users gamertag to the gamertag displayed
            }
```

此小段代码进行检查以查看是否有适用于此 GameObject 登录到播放机数字槽的播放器，然后将存储于在登录时将显示该用户玩家代号。 虽然`XboxLiveUser`等包含已登录用户玩家代号、 Xbox 用户 ID (xuid) 将需要调用其他服务中`SocialManager`gamerpic 和 gamerscore 等的访问信息。

## <a name="destroying-your-sign-in-gameobject"></a>销毁你登录的 GameObject

当销毁使用 Xbox Live 插件管理器之一的游戏对象时喜欢`SignInManager`或`SocialManager`，通常时加载新的场景中，请务必删除添加到该管理器的事件侦听器列表中的任何函数。 在本文的示例代码中我们将需要删除我们添加到登录和注销的事件侦听器的函数。我们将删除这些函数从`SignInManager`在`OnDestroy()`我们 GameObject 的函数。

```csharp
private void OnDestroy()
{
    if (SignInManager.Instance != null)
    {
        SignInManager.Instance.RemoveCallbackFromPlayer(this.PlayerNumber, this.OnPlayerSignOut);
        SignInManager.Instance.RemoveCallbackFromPlayer(this.PlayerNumber, this.OnPlayerSignIn);
    }
```

此代码将删除与此 GameObject 相关联的播放器的单一登录和注销回调函数。

## <a name="testing-you-code-in-visual-studio"></a>在 Visual Studio 中测试你的代码

除了[生成您在 Visual Studio 中的游戏所需的步骤](configure-xbox-live-in-unity.md#build-and-test-the-project)中列出[配置 Unity 在 Xbox Live 标题](configure-xbox-live-in-unity.md)文章，没有其他测试正确地在您的游戏所必需的步骤Visual Studio。 你将需要更新 package.appxmanifest.xml 文件的属性。 要实现此目的，请执行以下操作：

1. 搜索 package.appxmanifest.xml 文件在解决方案资源管理器
2. 右键单击该文件，然后选择查看代码
3. 下`<Properties><\/Properties>`部分中，添加以下行: < uap:SupportedUsers > 多个 <\/uap:SupportedUsers >。
4. 通过从 Visual Studio 中启动远程调试生成将部署到您的 Xbox 游戏。 您可以找到指令，以设置你的标题在 Xbox 上[设置在 Xbox 开发环境在 UWP](../../xbox-apps/development-environment-setup.md)一文。

> [!NOTE]
> 配置已更改的一部分可能看起来它启用多玩家，但仍有必要在单用户方案中运行您的游戏。

## <a name="policies-and-limitations"></a>策略和限制

有几个签入策略和标题的可能需要考虑开发您的登录体验时的限制。

- 标题的初始登录之后必须保持签名中至少一个播放器。 `SignInManager`将引发错误，并且如果尝试注销登录用户的最后一个，呼叫将失败。 还有一点需要注意，不能调用`SignInManager.Instance.SwitchUser(int playerNumber)`上最后一个签名在播放机中，因为它将尝试在新的播放机中签名之前注销播放机。

- PC 可以仅在登录用户时，控制台可能在一次登录最多 16 个玩家。

- 标题实际上不具有权限可注销从操作系统的播放器，因为此注销可能无法按预期工作。 SignInManager in 和 out 标题而言，用户可以登录，但不能注销任何人都从标题部署到计算机。

- 多个用户登录才可在 Xbox 一个控制台上。

- 来宾帐户不可用到 Xbox Live Creators 计划标题。