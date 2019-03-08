---
title: 在 Unity 预设和登录 XBL
description: 介绍了社交预设和适用于 Xbox Live 上的社交服务脚本示例
ms.date: 01/24/2018
ms.topic: get-started-article
keywords: xbox live、 xbox、 游戏、 uwp、 windows 10 中，一个 xbox、 unity
ms.openlocfilehash: a893858dac11fa848c2601df2c1bd6292b72ac6d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660032"
---
# <a name="unity-prefabs-and-scripted-sign-in"></a>Unity 预设和脚本化登录

本文将指导您完成将 Xbox Live 的单一登录添加到您的 Unity 项目。 有两种方法，如果你已下载，可以实现单一登录[Xbox Live Unity 插件](https://github.com/Microsoft/xbox-live-unity-plugin)。 可能会使用该插件中包含预设，也可以使用脚本和包括的库编写的脚本 Xbox Live 的单一登录到你自己自定义的 Gameobject。

> [!IMPORTANT]
> 本文适用于之前在 5 月，于 2018 年 (版本 1804) 所做的更新插件的版本。 如果该时间后安装 Xbox Live 插件或尚未下载可能会有较新版本具有与登录的执行方式大不相同。 除此之外，您会发现此插件中的屏幕截图不匹配的最新版本。 请改为参阅[一文，了解更新登录预设](playerauthentication-prefab-sign-in.md)，以及[一文详细介绍用于在登录脚本编写的更新的方法](sign-in-manager.md)。

## <a name="before-you-begin"></a>开始之前

在开始将 Xbox Live 社交服务添加到你的 Unity 游戏之前有几个步骤都需要先自问完成。 首先，请确保已下载并集成[Xbox Live Unity 插件](https://github.com/Microsoft/xbox-live-unity-plugin)。 其次，你将想要具有保留并通过发布在标题[Microsoft 开发中心](https://developer.microsoft.com/en-us/games/uwp)。 读取[创建一个新的 Xbox Live Creators 计划标题](../get-started-with-creators/create-and-test-a-new-creators-title.md)有关发布在标题上的说明。
最后，读取[配置 Unity 在 Xbox Live](../get-started-with-creators/configure-xbox-live-in-unity.md)正确设置你的 Unity 环境和配置您要使用 Xbox Live 服务的标题。 一旦正确设置你的 Unity 项目，就可以了解如何在你的 Xbox Live 启用标题，可以使用的工具，以及在 Unity 中实现 Xbox Live 的两种主要方法： 预设和脚本。

## <a name="prefabs"></a>预设

Unity 具有预设的资产类型，可用于存储 GameObject 完成，但组件和属性。 预设可充当通过它可以在 Unity 场景中创建新对象实例的模板。
[了解有关从 Unity 网站的预设的详细信息](https://unity3d.com/learn/tutorials/topics/interface-essentials/prefabs-concept-usage)。

Xbox Live Unity 插件提供了可用于在项目中利用的几个预设 Xbox Live 功能。 本文中所述的预设将允许你登录，[添加多用户支持](../get-started-with-creators/add-multi-user-support.md)为标题或显示的签名在 Xbox Live 好友列表配置文件。 您可以找到这些和其他预设下的**项目**按照路径的选项卡：**资产 > Xbox Live > 预设**。

### <a name="the-userprofile-prefab"></a>用户配置文件预设

第一个也是最重要社交预设**UserProfile**预设。 **UserProfile**预设具有允许 Xbox Live 登录所需的一切功能。 这是非常重要，因为必须在登录用户在使用 Xbox Live 服务之前。 预设可包含登录按钮和一个 GameObject，若要在其玩家代号、 Gamerpic，和所有播放机中代表已登录。

> [!NOTE]
> 若要使用任何其他 Xbox Live 预设，必须包括**UserProfile**预设或手动调用登录 API。

![资产和层次结构中的用户配置文件预设](../images/unity/unity-userprofile-views.png)

如果展开**UserProfile**预设中**项目**面板中或在**层次结构**被添加到场景中后，你将看到**用户配置文件**预设可包含在其内部的两个 Gameobject。 第一个对象是**SignInPanel**其中包含登录按钮体验。 第二个对象是**ProfileInfo**他们在登录后，其中将包含有关用户的信息。 **UserProfile**预设是您将用于表示任何 Xbox Live 的用户登录的本地到你的标题的信息。

### <a name="the-xboxliveuser-prefab"></a>XboxLiveUser 预设

**UserProfile**预设可使用在调用其代码中的第二个社交预设**XboxLiveUser**。 此预设可使用不立即显现，因为它不需要添加到场景的层次结构，因为它可能只需实例化代码中。 **XboxLiveUser**没有可视化表示形式，它只包含与 Xbox Live 用户的详细信息。 你将需要的实例**XboxLiveUser**的每个实例**UserProfile**。 这一点很重要时[添加多用户支持](../get-started-with-creators/add-multi-user-support.md)到你的标题。 除了保存有关用户登录后的信息此预设可也是用来登录 Xbox Live 的用户的代码的包装器。

## <a name="sign-in-with-the-userprofile-prefab"></a>使用用户配置文件预设可登录

Xbox Live Unity 插件预设存在是为了使某些开发任务更容易。 若要启用 Xbox Live 登录只需使用 Unity 项目**UserProfile**并**XboxLiveServices**预设以及 Unity **EventSystem**。

第一次拖动**UserProfile**到场景预设。 理想情况下**UserProfile**应放置在你的项目的初始菜单屏幕。

![将用户配置文件拖到层次结构](../images/unity/drag-userprofile.gif)

除了**UserProfile**预设还需要确保**XboxLiveServices**预设是至少你的项目的第一个场景中存在。
**XboxLiveServices**预设，可切换是否某些预设将记录的调试信息。 这是可用于检查 prefab 行为。

![检查 prefab xboxliveservices](../images/unity/check-for-xboxliveservices.gif)

最后， **UserProfile**还要求**EventSystem**才能正常运行。 这可以通过右键单击登录场景再通过单击添加**GameObject--> UI--> EventSystem**。

![添加事件系统](../images/unity/add_event_system.gif)

如果输入 play 模式，该服务将自动登录使用户登录。 在 Unity 中，Xbox Live SDK 模拟对 Xbox Live 服务的调用，并发送有关您可以使用返回假数据。 若要查看实时数据，您需要生成作为 UWP 应用程序项目，并从 Visual Studio 中运行它。 有关详细信息，请参阅[在 Unity 中配置 Xbox Live](../get-started-with-creators/configure-xbox-live-in-unity.md)。 当你输入时播放模式下在 Unity 中，预设可将填充了假数据，这将模拟玩家代号、 Gamerpic 和播放器 Gamerscore 等信息。 这是应显示的信息**UserProfile**预设。

成功登录中将如下所示：![成功 userprofile play](../images/unity/correct-user-profile-play.gif)

如果还没有配置你的项目以连接到 Xbox Live 正确进入播放模式将禁用登录按钮，并显示一条错误消息。

下面是示例的失败登录由于错误的 Xbox Live 应用程序配置。
![失败用户配置文件播放](../images/unity/flawed-user-profile-play.gif)

## <a name="scripting-sign-in"></a>在登录脚本

既然你知道如何使用**UserProfile** prefab 它，最佳做法是看一下基础控制预设的功能的脚本。 如果您看一下**UserProfile**中**Inspector**你将看到它有**UserProfile.cs**脚本附加到它。 此脚本已登录用户并加载您所要显示在登录的配置文件信息所需的一切。 但是，而不在整个预设 （这可能会随着时间的推移更新） 可查看我们要看一看少量的代码，以了解所需登录 Xbox Live 的用户的示例行。

### <a name="the-xboxliveuser-class"></a>XboxLiveUser 类

需要以登录用户的调用包装在`XboxLiveUserInfo`类。 在**UserProfile.cs**脚本，您会看到是实例`XboxLiveUserInfo`类调用`XboxLiveUser`。 在我们的示例代码中，我们将使用相同的变量名称。 `XboxLiveUserInfo`类包含的实例`XboxLiveUser`类调用`User`作为其成员变量中的一个。 `XboxLiveUser`类包含登录所需的登录函数`XboxLiveUser`。 将使用的实例`XboxLiveUser`类`User`登录用户并获取描述了这些功能的信息，如其玩家代号、 Gamerpic 和 Gamerscore。 若要执行此操作必须初始化的实例`XboxLiveUserInfo`类，并使用生成`XboxLiveUserInfo.User`为调用单一登录。

### <a name="initialize-the-xboxliveuser"></a>初始化 XboxLiveUser

初始化 Xbox Live 用户实际上它们登录到 Xbox Live 之前是第一步。 这是非常简单地在代码中使用`XboxLiveUserInfo.Initialize()`函数。
在我们的示例代码，我们使用`XboxLiveUser`成员变量作为我们`XboxLiveUserInfo`实例并初始化，若要使用的单一登录。

```csharp
    void ButtonClickTask()
    {
        this.StartCoroutine(this.InitializeXboxLiveUser());
    }

    public IEnumerator InitializeXboxLiveUserAndCallSignIn()
    {
        // Disable the sign-in button
        SignInButton.interactable = false;

        this.XboxLiveUser.Initialize();

        //Wait until the Xbox User has been initialized to call SignInAsync()
        yield return new WaitUntil(() => this.XboxLiveUser != null && this.XboxLiveUser.User != null);
        this.StartCoroutine(this.SignInAsync());
    }
```

看一下此示例代码将看到的`XboxLiveUserInfo.Initialize()`函数调用以响应按钮单击。 完整**UserProfile.cs** prefab 脚本具有.cs 代码允许自动登录其中`XboxLiveUserInfo.Initialize()`调用而无需交互。
`XboxLiveUserInfo.Initialize()`函数会创建一个新`XboxLiveUserInfo.User`，让我们来调用中包含的登录函数`XboxLiveUserInfo.User`类。

### <a name="call-sign-in"></a>在调用登录

初始化 XboxLiveUser 后它有个调用单一登录的时间。 在中**UserProfile.cs**登录名为`SignInAsync()`UserProfile.cs 函数。 前面的示例代码中我们只需等待`XboxLiveUser`之前，先初始化`SignInAsync()`函数。

> [!NOTE]
> 需要等待`XboxLiveUser`因为调用在登录之前初始化`XboxLiveUser`包含`XboxLiveUser.User`为调用登录使用的属性。

在中**UserProfile.cs** `SignInAsync()`函数包含可用于在用户登录的两个登录函数。 `XboxLiveUser.User.SignInSilentlyAsync()` 和`XboxLiveUser.User.SignInAsync()`它们是将用户登录的函数。 `SignInAsync()`函数是如何适当地使用这些函数的一个很好示例。 下面的代码示例显示了一个适当的方法来调用两个登录函数：

```csharp
SignInStatus signInStatus;
TaskYieldInstruction<SignInResult> signInSilentlyTask = this.XboxLiveUser.User.SignInSilentlyAsync().AsCoroutine();
yield return signInSilentlyTask;

signInStatus = signInSilentlyTask.Result.Status;
if (signInSilentlyTask.Result.Status != SignInStatus.Success)
{
    TaskYieldInstruction<SignInResult> signInTask = this.XboxLiveUser.User.SignInAsync().AsCoroutine();
    yield return signInTask;

    signInStatus = signInTask.Result.Status;
}
```

在此示例中的登录中调用的结果存储在变量`signInStatus`。 这样，我们要检查已登录成功，并执行相应操作。 在此示例中该函数首先尝试登录以无提示方式，然后如果无提示登录失败的函数调用正常登录函数中。 在成功调用其中一个登录函数将在用户登录后。 现在，您可以使用`XboxLiveUser.User`来获取并显示有关已登录用户的详细信息。 看一看`LoadProfileInfo()`函数，在**UserProfile.cs**以举例说明如何使用`XboxLiveUser.User`以显示有关已登录用户的信息。

## <a name="build-and-test-sign-in"></a>生成并测试单一登录

在编辑器中运行你的主题作品时，如果尝试使用 Xbox Live 功能，你将会看到虚假数据。 若要使用实际的配置文件登录并测试你的标题中的 Xbox Live 功能，将需要生成 UWP 解决方案并在 Visual Studio 中运行它。  可以通过执行以下步骤生成 Unity 中的 UWP 项目：

1. 打开**生成设置**通过选择窗口**文件** > **生成设置**。
2. 添加所有你想要在下，在生成中包括在后台**生成中的场景**部分。
3. 切换到**通用 Windows 平台**通过选择**通用 Windows 平台**下**平台**，然后单击**切换平台**.
4. 设置**SDK**到**10.0.15063.0**或更高版本。
5. 若要启用脚本调试检查**UnityC#项目**。
6. 单击**生成**并指定项目的位置。

完成生成后，Unity 会生成新的 UWP 解决方案文件将会需要在 Visual Studio 中运行：

1. 在您指定的文件夹，打开 **&lt;ProjectName&gt;.sln** Visual Studio 中。
2. 在顶部工具栏中，选择**x64**并将其部署到**本地计算机**。

如果启用了**脚本调试**从 Unity 生成 UWP 解决方案，然后你的脚本将位于下面**程序集 c# (通用 Windows)** 项目。

> [!NOTE]
> 在使用之前在 Visual Studio 生成测试您的游戏使用实际数据，请按照[此清单](test-visual-studio-build.md)以帮助确保你的标题将能访问 Xbox Live 服务。

## <a name="troubleshooting"></a>疑难解答

如果遇到问题，在登录到 Xbox Live 尝试读取[故障排除 Xbox Live 登录](../using-xbox-live/troubleshooting/troubleshooting-sign-in.md)。
