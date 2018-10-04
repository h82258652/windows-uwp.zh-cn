---
title: 将 Xbox Live 代码从 XDK 移植到 UWP
author: KevinAsgari
description: 了解如何将 Xbox Live 代码从 Xbox 开发工具包 (XDK) 平台移植到通用 Windows 平台 (UWP)。
ms.assetid: 69939f95-44ad-4ffd-851f-59b0745907c8
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one, xdk, 移植
ms.localizationpriority: medium
ms.openlocfilehash: 9278ee433852bf3ef1eec2570340ef9cb7d64c92
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4318144"
---
# <a name="porting-xbox-live-code-from-the-xbox-developer-kit-xdk-to-universal-windows-platform-uwp"></a>将 Xbox Live 代码从 Xbox 开发人员工具包 (XDK) 移植到通用 Windows 平台 (UWP)

## <a name="introduction"></a>简介

本文旨在帮助使用过 Xbox One XDK 的开发人员开始将其 Xbox Live 代码迁移到 Windows 10 通用 Windows 平台 (UWP)。

此迁移包含从 XSAPI 1.0（Xbox Live 服务 API，包含在于 2015 年 8 月之前推出的 Xbox One XDK 中）迁移到 XSAPI 2.0（包含在于 2015 年 11 月开始推出的 Xbox One XDK 中，它还包含在 Xbox Live SDK 中）。 这些 API 的功能几乎相同，但存在一些重要的实现差异。

本文将涉及的其他主题包括：准备 Windows 开发计算机和安装其他 API，在使用 Xbox Live 服务时通常需要使用这些 API，如安全套接字 API 以及用于管理基于云的游戏保存的连接存储 API。

<a name="_Setting_up_and"></a>

## <a name="setting-up-and-configuring-your-project-in-dev-center-and-xdp"></a>在开发人员中心和 XDP 中设置和配置项目

使用 Xbox Live 服务的 UWP 游戏需要在[Windows 开发人员中心](https://dev.windows.com/en-us)或[Xbox 开发人员门户 (XDP)](https://xdp.xboxlive.com/)中配置。 有关最新信息，请参阅 Xbox Live 编程指南中的[将 Xbox Live 添加到新的或现有的 UWP 项目](../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)，该指南包含在 [Xbox Live SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx) 中。

该页面上的主题包含在作品中使用 Xbox Live 服务的以下步骤：

-   在 Windows 开发人员中心中创建 UWP 应用项目。

-   使用 XDP 设置用于 Xbox Live 的项目。

-   将开发人员中心产品链接到 XDP 产品。

-   在 XDP 中创建开发人员帐户（在沙盒中运行 Xbox Live 作品时需要使用它）。

如果你的作品支持多人游戏，则系统可能会要求在多人游戏会话模板中进行一些其他设置。 所有使用 Xbox Live 多人游戏并写入 MPSD（多人游戏会话文档）的 Windows 10 游戏都要求在会话模板的“功能”列表中找到此新字段：```userAuthorizationStyle: true```。

### <a name="enabling-cross-play"></a>启用跨平台联机游戏

如果要支持“跨平台联机游戏”（Xbox One 与电脑游戏之间的一种共享 Xbox Live 配置，允许跨设备进行多人游戏），你还需要将此功能添加到你的会话模板：**crossPlay: true**。

有关在 XDP 支持跨平台联机游戏及其配置要求的其他信息，请参阅 Xbox Live 编程指南中的“在 XDP 中引入 XDK 和 UWP 跨平台联机游戏”。

此外，有关某些编程注意事项，请参阅下一节[支持在 Xbox One 和电脑之间进行多人跨平台联机游戏](#_Supporting_multiplayer_cross-play)。

## <a name="setting-up-your-windows-development-environment"></a>设置 Windows 开发环境

1.  [下载最新的 **Xbox Live SDK**](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx) 并在本地解压缩。

2.  [安装 **Xbox Live 平台扩展 SDK**](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx) - 如果你需要为 UWP 使用安全套接字 API 和/或游戏保存 API（也称为连接存储）。

3.  在 Visual Studio 中为通用 Windows 应用项目添加 Xbox Live 支持。 你可以添加完整源或引用二进制文件，通过 NuGet 程序包安装到 Visual Studio 项目。 程序包以 C++ 和 WinRT 的形式提供。 有关更多详细信息，请参阅[将 Xbox Live 添加到新的或现有的 UWP 项目](../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)

4.  将你的开发计算机配置为使用沙盒。 你可以在管理员命令提示符下运行 Xbox Live SDK 的工具目录中的命令行脚本（例如：SwitchSandbox.cmd XDKS.1）。

  **注意**若要切换回零售沙盒，你可以删除脚本修改的注册表项，也可以切换到名为“RETAIL”的沙盒。

1.  将开发人员帐户添加到开发计算机。 当你在指定的沙盒内进行开发或运行示例时，在 XDP 中创建的开发人员帐户需要在运行时与 Xbox Live 服务进行交互。 若要将一个或多个帐户添加到 Windows：

    1.  打开**设置**（快捷方式：Windows 键 + I）。

    2.  打开**帐户**。

    3.  在**你的帐户**选项卡上，单击**添加 Microsoft 帐户**。

    4.  输入开发人员帐户的电子邮件和密码。

### <a name="appxmanifest-changes"></a>AppxManifest 更改

Xbox 与 UWP 版本的 appxmanifest.xml 文件之间的最常见更改包括：

1. UWP 中的程序包标识非常重要，即使在开发期间也是如此。 标识名称和发布者*必须与*在开发人员中心为 UWP 应用定义的信息相符。

1. 需要填写“程序包依赖关系”部分。 例如：

```xml
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Universal" MinVersion="10.0.10240.0" MaxVersionTested="10.0.10240.0" />
  </Dependencies>
```

1.  有关对 UWP 具有特定要求的应用程序清单的其他部分，例如 ```<VisualElements>```，请参阅示例 UWP 应用程序清单（例如，Xbox Live SDK 或在 Visual Studio 中创建的默认通用 Windows 应用项目所包含的 UWP 示例之一）。

1.  作品和 SCID 是在 xboxservices.config 文件（请参阅[下一节](#_Define_your_title)）而不是“xbox.live”扩展类别中定义的。

1.  不需要“xbox.system.resources”扩展类别。

1.  安全套接字是在 networkmanifest.xml 文件（请参阅[安全套接字](#_Secure_sockets)）而不是“windows.xbox.networking”类别中定义的。

1.  必须定义“windows.protocol”扩展类别，这样才能在 UWP 作品中接收 Xbox Live 邀请（请参阅[发送和接收邀请](#_Sending_and_receiving)）。

1.  如果使用 GameChat API，则你可能希望在 ```<Capabilities>``` 元素内添加麦克风设备功能。 例如：

  ```<DeviceCapability Name="microphone">```

<a name="_Define_your_title"></a>

### <a name="define-your-title-and-scid-for-the-xbox-live-sdk-in-a-config-file"></a>在配置文件中为 Xbox Live SDK 定义作品和 SCID

Xbox Live SDK 需要知道作品 ID 和 SCID，它们不再包含在 UWP 作品的 appxmanifest.xml 文件中。 相反，你需要在项目根目录中创建名为 **xboxservices.config** 的文本文件并添加以下字段，使用作品信息替换值：

```
{
  "TitleId": 123456789,
  "PrimaryServiceConfigId": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
}
```

> [!NOTE]
> Xboxservices.config 中的所有值都区分大小写。

将此配置文件作为内容包含在项目内，以便能够以生成输出的形式提供。

**注意**这些值将使用以下 API 在作品内以编程方式提供：

```cpp
Microsoft::Xbox::Services::XboxLiveAppConfiguration^ xblConfig = xblContext->AppConfig;
unsigned int titleId = xblConfig->TitleId;
Platform::String^ scid = xblConfig->ServiceConfigurationId;
```

### <a name="api-namespace-mapping"></a>API 命名空间映射

表 1.  从 XDK 到 UWP 的命名空间映射。

<table>
  <tr>
    <td></td>
    <td><b>Xbox One XDK</b></td><td><b>UWP</b></td>
    <td><b>API 随以下项目提供...</b></td>
  </tr>
  <tr>
    <td>Xbox 服务 API (XSAPI)</td>
    <td>Microsoft::Xbox::Services</td>
    <td>Microsoft::Xbox::Services（<i>无更改</i>）</td>
    <td>Xbox Live SDK（使用 NuGet 二进制文件或源代码）</td>
  </tr>
  <tr>
    <td>GameChat</td>
    <td>
Microsoft::Xbox::GameChat Windows::Xbox::Chat </td>
    <td>
Microsoft::Xbox::GameChat（*无更改*）Microsoft::Xbox::ChatAudio </td>
    <td>
Xbox Live SDK（使用 NuGet 二进制文件） </td>
  </tr>
  <tr>
    <td>SecureSockets</td>
    <td>Windows::Xbox::Networking</td>
    <td>Windows::Networking::XboxLive</td>
    <td>Xbox Live 平台扩展 SDK</td>
  </tr>
  <tr>
    <td>连接存储</td>
    <td>Windows::Xbox::Storage</td>
    <td>Windows::Gaming::XboxLive::Storage</td>
    <td>Xbox Live 平台扩展 SDK</td>
  </tr>
</table>

### <a name="multiplayer-subscriptions-and-event-handling"></a>多人游戏订阅和事件处理

大部分多人游戏将会遇到的从 XSAPI 1.0 到 XSAPI 2.0 的重大更改之一就是将多种方法和事件从 **RealTimeActivityService** 移动到 **MultiplayerService**。

例如：

-   **EnableMultiplayerSubscriptions()\*** 方法

-   **DisableMultiplayerSubscriptions()** 方法

-   **MultiplayerSessionChanged** 事件

-   **MultiplayerSubscriptionLost** 事件

-   **MultiplayerSubscriptionsEnabled** 属性

**重要实施说明** 即使在将这些事件和方法移动到 **MultiplayerService** 后你可能不会明确使用 **RealTimeActivityService** 中的任何其他项目，你也必须先调用 **xblContext-&gt;RealTimeActivityService-&gt;Activate()**，然后再调用 **EnableMultiplayerSubscriptions()**，因为多人游戏订阅需要 RTA 服务。

## <a name="whats-handled-differently-in-uwp"></a>哪些项目在 UWP 中以不同方式进行处理

以下是 XDK 和 UWP 之间很可能存在差异的代码部分的高级列表，如新的 [NetRumble 示例](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx)（包括 XDK 和 UWP 版本）所示：

-   访问作品 ID 和 SCID 信息

-   预启动激活（UWP 的新功能）

-   暂停/恢复 PLM 处理

-   扩展执行（UWP 的新功能）

-   Xbox **用户**对象和用户处理差异

    -   登录和注销处理

    -   控制器配对（仅在 Xbox 上处理）

    -   游戏板处理

-   检查多人游戏的权限

-   支持在 Xbox One 与电脑之间进行多人跨平台联机游戏

-   发送游戏邀请

    -   用于从游戏内打开相关方应用的功能 - 在 UWP 上不适用

    -   用于从游戏内枚举相关方成员的功能 - 在 UWP 上不适用

-   显示玩家的个人资料

-   安全套接字 API 图面更改

-   QoS 测量启动和结果处理

-   编写游戏事件

-   GameChat：事件、设置和 ChatUser 对象

-   连接存储 API 图面更改

-   PIX 事件（仅在 Xbox 上；本白皮书不进行介绍）

-   一些呈现差异

以下部分将进一步详细介绍其中的许多差异。

### <a name="accessing-title-id-and-scid-info"></a>访问作品 ID 和 SCID 信息

在 UWP 中，可以通过 **XboxLiveContext** 的某个实例上的 AppConfig 属性来访问你的作品 ID 和服务配置 ID。

```cpp
Microsoft::Xbox::Services::XboxLiveAppConfiguration^ xblConfig = xblContext->AppConfig;
unsigned int titleId = xblConfig->TitleId;
Platform::String^ scid = xblConfig->ServiceConfigurationId;
```

**注意** 在 XDK 中，你可以通过使用这些新属性或 **Windows::Xbox::Services::XboxLiveConfiguration** 中的旧静态属性来获取这些 ID。

### <a name="prelaunch-activation"></a>预启动激活

当用户登录时，Windows 10 中的常用作品可能会预启动。 若要解决此问题，作品应包含用于检查 **PreLaunchActivated** 的启动参数的代码。 例如，你可能不希望在此类激活过程中加载所有资源。 有关详细信息，请参阅 MSDN 文章[处理应用预启动](https://msdn.microsoft.com/library/windows/apps/mt593297.ASPx)。

### <a name="suspendresume-plm-handling"></a>暂停/恢复 PLM 处理

一般而言，在通用 Windows 应用和 Xbox One 中，暂停和恢复 PLM 的工作方式类似，你需要谨记一些重要的差异：

-   电脑上没有**受约束**状态 - 这是 Xbox One 的专有概念。

-   当最小化作品时将立即开始暂停；要解决此问题，请参阅[扩展执行](#_Extended_execution)一节。

-   时间有所不同：你有 5 秒钟的时间在电脑上暂停，而在控制台上则只有 1 秒钟。

如果使用连接存储，则需要注意的另一个重要事项就是此 API 的 UWP 版本的新 **ContainersChangedSinceLastSync** 属性。 在处理恢复事件时，你可以检查此属性以查看当作品暂停时云中是否有任何容器发生了更改。 如果玩家在某台电脑上暂停了游戏并到其他电脑上玩游戏，随后重新回到第一台电脑，则可能会发生这种情况。 如果你在暂停之前已将这些容器中的数据读取到内存，则你可能希望重新读取这些数据，以便查看对其所做的更改并相应地处理这些更改。

有关在 Windows 10 上的 UWP 应用中处理 PLM 的详细信息，请参阅 MSDN 文章[启动、恢复和后台任务](https://msdn.microsoft.com/library/windows/apps/xaml/mt227652.aspx)。

你可能还会发现 GDN 上的 [PLM for Xbox One](https://developer.xboxlive.com/en-us/platform/development/education/Documents/PLM%20for%20Xbox%20One.aspx) 白皮书很有用，因为它是根据游戏需要编写的，并且用于处理应用生命周期的大部分概念仍适用于电脑。

<a name="_Extended_execution"></a>

### <a name="extended-execution"></a>扩展执行

最小化电脑上的 UWP 应用通常会导致它立即开始暂停。 通过使用扩展执行，你将有机会延迟此过程。 示例实现：

```cpp
using namespace Windows::ApplicationModel::ExtendedExecution;
//If this goes out of scope the request is nullified
ExtendedExecutionSession^ session;
void App::RequestExtension()
{
       if (!session)
       {
              session = ref new ExtendedExecutionSession();
       }
       session->Reason = ExtendedExecutionReason::Unspecified;
       session->Description = "foo";
       session->Revoked += ref new TypedEventHandler<Platform::Object^, ExtendedExecutionRevokedEventArgs^>(this, &App::ExtensionRevokedHandler);
       IAsyncOperation<ExtendedExecutionResult>^ request = session->RequestExtensionAsync();
       //At this point the request has been made. When the IAsyncOperation request completes, verify that the ExtendedExecutionResult == Allowed and you will not suspend for up to 10 minutes while minimized.
}

void App::ExtensionRevokedHandler(Platform::Object^ obj, ExtendedExecutionRevokedEventArgs^ args)
{
       if (args->Reason == Windows::ApplicationModel::ExtendedExecutionRevokedReason::Resumed)
       {
              //Request the extension again in preparation for the next suspend.
RequestExtension();
       }
       //The app will either complete suspending if the extension was revoked by system policy or resume running if the user has switched back to the app.
}

```

在调用 **ExtensionRevokedHandler** 后，需要请求新的扩展以便应对未来潜在的暂停。 当系统中存在内存压力、已过去 10 分钟或者用户切换回处于最小化状态的游戏时，将调用 **ExtensionRevokedHandler**。 因此 **RequestExtension()** 很可能会在以下时间调用：

-   启动过程中。

-   在 **ExtensionRevokedHandler** 中（如果 args-&gt;原因 == 已恢复（在 10 分钟计时器过期之前，用户在游戏处于最小化状态期间已切换回））。

-   在 **OnResuming** 处理程序中（如果作品由于内存压力或 10 分钟计时器过期而暂停）。

### <a name="handling-users-and-controllers"></a>处理用户和控制器

在 Windows 中，每次只能有一名已登录的用户。 在 Xbox Live SDK 中，你可以先创建一个 **XboxLiveUser** 对象，让用户登录到 Xbox Live，然后为此用户创建 **XboxLiveContext** 对象。

以前，在 Xbox One XDK 上：

1.  获取用户（例如通过游戏板交互）。
2.  为该用户创建一个 **XboxLiveContext**：
  ```
  ref new Microsoft::Xbox::Services::XboxLiveContext( Windows::Xbox::System::User^ user )
  ```
1.  处理**注销**事件：
  ```
  Windows::Xbox::System::User::SignOutStarted
  ```
1.  使用以下功能处理游戏板/控制器配对：
  ```
  Windows::Xbox::Input::Controller::ControllerRemoved
  Windows::Xbox::Input::Controller::ControllerPairingChanged
  ```

现在，对于 UWP/Xbox Live SDK：

1.  创建一个 **XboxLiveUser**：

  ```
  auto xblUser = ref new Microsoft::Xbox::Services::System::XboxLiveUser();
  ```

1.  尝试通过他们使用过的上一个 Microsoft 帐户来使其登录，而不要使用 UI：

  ```
  xblUser->SignInSilentlyAsync();
  ```

1.  如果此异步操作的结果显示 **SignInResult::Success**，请创建 **XboxLiveContext**，你即已完成操作：

  ```
  auto xblContext = ref new Microsoft::Xbox::Services::XboxLiveContext( xblUser );
  ```

1.  如果结果显示 **SignInResult::UserInteractionRequired** ，则需要调用用于显示系统 UI 的交互式登录方法：

  ```
  xblUser->SignInAsync();
  ```

1.  在此处，结果可能会显示 **SignInResult::UserCancel**，这种情况表示没有已登录的用户，你应该考虑提供菜单选项以供他们重新尝试登录。

  **注意** 在提供菜单选项时，最好是为他们提供用于切换到其他 Microsoft 帐户的选项：

  ```
  xblUser->SwitchAccountAsync( nullptr );
  ```

1.  在用户登录后，你可能想要关联 **XboxLiveUser::SignOutCompleted** 事件，以便可以对用户注销作出响应：

  ```
  xblUser->SignOutCompleted += ref new Windows::Foundation::EventHandler<Microsoft::Xbox::Services::System::SignOutCompletedEventArgs^>( &OnSignOutCompleted );
  ```

1.  在 Windows 10 中，没有要处理的控制器配对。

这是 C++ / WinRT 的简化示例。 有关更详细的示例，请参阅 Xbox Live 编程指南中的“Windows 10 中的 Xbox Live 身份验证”。 你可能还会发现“将 Xbox Live 添加到新的 UWP 项目”中的更广泛的示例很有用。

### <a name="checking-multiplayer-privileges"></a>检查多人游戏的权限

在 Xbox Live SDK 中未提供 **CheckPrivilegeAsync()** 的等效项。 现在，你需要在由 **XboxLiveUser** 的**权限**属性返回的字符串列表中搜索所需的权限。 例如，若要检查多人游戏的权限，请查找权限“254”。 通过使用 XDK 文档，你可以在 **Windows::Xbox::ApplicationModel::Store::KnownPrivileges** 枚举中找到所有 Xbox Live 权限的列表。

有关该主题的讨论，请参阅论坛文章 [xsapi 和用户权限](https://forums.xboxlive.com/questions/48513/xsapi-user-privileges.html)。

<a name="_Supporting_multiplayer_cross-play"></a>

### <a name="supporting-multiplayer-cross-play-between-xbox-one-and-pc-uwp"></a>支持在 Xbox One 与电脑 UWP 之间进行多人跨平台联机游戏

除了 XDP 中的新会话模板要求（请参阅[在开发人员中心和 XDP 中设置和配置项目](#_Setting_up_and)）以外，跨平台联机游戏还增加了对会话加入功能的限制。 你不能再将“无”用作会话加入限制。 你必须使用“已关注”或“本地”（默认限制是“本地”）。

此外，由于 Windows 10 多人游戏需要 **userAuthorizationStyle** 功能，加入和读取限制默认为“本地”。

此论坛文章[能否创建公共多人游戏会话](https://forums.xboxlive.com/questions/46781/is-it-possible-to-create-public-multiplayer-sessio.html) 包含其他观点。

可以在更新的多人游戏开发人员流程图、已启用跨平台联机游戏的多人游戏示例 NetRumble 或开发者帐户管理器 (DAM) 中找到更多信息和示例。

<a name="_Sending_and_receiving"></a>

### <a name="sending-and-receiving-invites"></a>发送和接收邀请

可显示用于发送邀请的 UI 的 API 现在为 **Microsoft::Xbox::Services::System::TitleCallableUI::ShowGameInviteUIAsync()**。 你可以从活动会话（通常是大厅）中传入会话 &gt; **SessionReference** 对象。 你也可以选择传入第二个参数，该参数将引用已在 XDP 的服务配置中定义的自定义邀请字符串 ID。 你在此处定义的字符串将显示在已发送给受邀玩家的 Toast 通知中。 请注意，作为参数传入此方法的内容是 ID 号，必须针对该服务对其进行正确格式化。 例如，字符串 ID“1”必须以“///1”的格式进行传入。

如果你希望直接使用多人游戏服务来发送邀请（即不显示任何 UI），你仍可以使用其他邀请方法，即用户 **XboxLiveContext** 中的 **Microsoft:: Xbox::Services::Multiplayer::MultiplayerService::SendInvitesAsync()**。

若要允许邀请进入 Windows 以便通过协议激活你的作品，你需要为 appxmanifest 中的 **&lt;Application&gt;** 元素添加此扩展。

```xml
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-xbl-multiplayer" />
  </uap:Extension>
</Extensions>
```

如果你的 **CoreApplication** 获得**已激活**事件并且激活类型为 **ActivationKind::Protocol**，则你可以按照以前在 Xbox One 中的操作方式来处理邀请。

### <a name="showing-the-gamer-profile-card"></a>显示玩家的个人资料卡片

若要在 UWP 上弹出玩家的个人资料卡片，请使用 **Microsoft:: Xbox::Services::System::TitleCallableUI::ShowProfileCardUIAsync()** 传入目标用户的 XUID。

<a name="_Secure_sockets"></a>

### <a name="secure-sockets"></a>安全套接字

安全套接字 API 包含在单独的 [Xbox Live 平台扩展 SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx) 内。

请参阅此论坛文章以了解 API 的用法：[设置跨平台的 SecureDeviceAssociation](https://forums.xboxlive.com/answers/45722/view.html)。

**注意**对于 UWP，**SocketDescriptions** 部分已移出 appxmanifest，并包含在自身的 [networkmanifest.xml](https://forums.xboxlive.com/storage/attachments/410-networkmanifestxml.txt) 内。 &lt;SocketDescriptions&gt; 元素内的格式几乎相同，只是无 **mx:** 前缀而已。

若要在 Xbox 与 Windows 10 之间进行跨平台联机游戏，请*确保*在这两种不同类型的清单（对于 Xbox One 为 Package.appxmanifest，而对于 Windows 10 则为 networkmanifest.xml）之间以*相同方式*定义一切内容。 套接字名称、协议等内容必须*完全*匹配。

此外，对于跨平台联机游戏，你还需要*同时*在 Xbox One Package.appxmanifest 和 Windows 10 networkmanifest.xml 的 ```<AllowedUsages>``` 元素内定义以下四种 SDA 用法：

```xml
<SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
<SecureDeviceAssociationUsage Type="AcceptOnMicrosoftConsole" />
<SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
<SecureDeviceAssociationUsage Type="AcceptOnWindowsDesktop" />
```

### <a name="multiplayer-qos-measurements"></a>多人游戏 QoS 测量

除了安全套接字 API 中的命名空间更改以外，某些对象名称和值也发生了更改。 可在下表中找到常用测量状态的映射。

表 2.  常用测量状态映射。

| XDK (Windows::Xbox::Networking::QualityOfServiceMeasurementStatus)  | UWP (Windows::Networking::XboxLive::XboxLiveQualityOfServiceMeasurementStatus)  |
|------------------------------------|--------------------------------------------|
| HostUnreachable                    | NoCompatibleNetworkPaths                   |
| MeasurementTimedOut                | TimedOut                                   |
| PartialResults                     | InProgressWithProvisionalResults           |
| 成功                            | 已成功                                  |

在比较 API 的 XDK 和 UWP 版本时，*测量* QoS（服务质量）和*处理结果*中涉及的步骤基本上相同。 但是，由于名称的更改和少量设计更改，在某些位置生成的代码看起来有所不同。

若要测量 **XDK** 的 QoS，你需要创建一组安全设备地址和一组指标，并将它们传入 **MeasureQualityOfServiceAsync()** 方法。

若要测量 **UWP** 的 QoS，你需要创建一个新 **XboxLiveQualityOfServiceMeasurement()** 对象，将 **Append()** 调用到其**指标**和 **DeviceAddresses** 属性，然后调用该对象的 **MeasureAsync()** 方法。

例如：

```cpp
auto qosMeasurement = ref new Windows::Networking::XboxLive::XboxLiveQualityOfServiceMeasurement();
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageInboundBitsPerSecond);
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageOutboundBitsPerSecond);
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageLatencyInMilliseconds);
qosMeasurement->NumberOfProbesToAttempt = myDefaultQosProbeCount;
qosMeasurement->TimeoutInMilliseconds = myDefaultQosMeasurementTimeout;

// Add secure addresses for each session member
for (const auto& member : session->GetMembers())
{
    if (!member->IsCurrentUser)
    {
        auto sda = member->SecureDeviceAddressBase64;

        if (!sda->IsEmpty())
        {
qosMeasurement->DeviceAddresses->Append(Windows::Networking::XboxLive::XboxLiveDeviceAddress::CreateFromSnapshotBase64(sda));
        }
    }
}

if (qosMeasurement->DeviceAddresses->Size > 0)
{
    qosMeasurement->MeasureAsync();
}

```

有关更多示例，请参阅 NetRumble 示例中的 **MatchmakingSession::MeasureQualityOfService()** 和**MatchmakingSession::ProcessQosMeasurements()** 函数。

### <a name="writing-game-events"></a>编写游戏事件

在 UWP 中，将使用不同的 API 来发送在游戏服务配置中配置的游戏事件。 Xbox Live SDK 使用 **EventsService** 和属性包模型。

例如：

```cpp
auto properties = ref new Windows::Foundation::Collections::PropertySet();
properties->Insert("RoundId", m_roundId);
properties->Insert("SectionId", safe_cast<Platform::Object^>(0));
properties->Insert("MultiplayerCorrelationId", m_multiplayerCorrelationId);
properties->Insert("GameplayModeId", safe_cast<Platform::Object^>(0));
properties->Insert("MatchTypeId", safe_cast<Platform::Object^>(0));
properties->Insert("DifficultyLevelId", safe_cast<Platform::Object^>(0));

auto measurements = ref new Windows::Foundation::Collections::PropertySet();

xblContext->EventsService->WriteInGameEvent("MultiplayerRoundStart", properties, measurements);

```

有关详细信息，请参阅 Xbox Live SDK 文档。

**提示** 你可以使用随 Xbox Live SDK 提供的 **xcetool.exe**（位于工具目录下）将从 XDP 下载的 events.man 文件转换成 .h 头文件。 通过新的 v2 属性包架构，使用“-x”选项生成此 C++ 标头。 此标头包含为所有已配置的事件调用的 C++ 函数；例如，**EventWriteMultiplayerRoundStart()**。 如果你更喜欢使用 WinRT 界面，则仍可以参考此头文件以了解如何为每个事件构建属性和测量。

### <a name="game-chat"></a>游戏聊天

UWP 中的 GameChat 作为 NuGet 包二进制文件包含在 Xbox Live SDK 内。 请参阅 Xbox Live 编程指南中的说明，以了解如何将此 NuGet 包添加到项目。

XDK 与 UWP 版本的基本用法几乎相同。 API 中的一些差异包括：

1.  UWP 作品无需关联 **User::AudioDeviceAdded** 事件。 基础聊天库负责处理设备添加和删除操作。

2.  **ChatUser** 现在称为 **GameChatUser**。

3.  **Microsoft::Xbox::GameChat** 命名空间保持不变，但 **Windows::Xbox::Chat** 命名空间已变为 **Microsoft::Xbox::ChatAudio**。

4.  **AddLocalUserToChatChannelAsync()** 采用 XUID 或 **ChatAudio::IChatUser^** ，而不是 **XboxUser**。

5.  **RemoveLocalUserFromChatChannelAsync()** 需要 **ChatAudio::IChatUser^**，而不是 **XboxUser**。 你可以从 **GameChatUser**-&gt;**用户**获取 **IChatUser**。

### <a name="connected-storage"></a>连接存储

连接存储 API 在单独的 [Xbox Live 平台扩展 SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx) 内提供。 相关文档包含在 Xbox Live SDK 文档内。

整个流程与 Xbox One 中的流程相同，但在 UWP 版本中增加了 **ContainersChangedSinceLastSync** 属性。 在重新调用 **GetForUserAsync()** 后，当作品处理恢复事件时应检查该属性，以查看当作品暂停时云中的哪些容器发生了更改。 如果你已将数据从其中一个发生更改的容器载入到内存，则你可能希望重新读取该数据，以便查看对其所做的更改并相应地处理这些更改。

UWP 版本中的其他显著差异包括：

1.  命名空间从 **Windows::Xbox::Storage** 更改为 **Windows::Gaming::XboxLive::Storage**。

2.  **ConnectedStorageSpace** 已重命名为 **GameSaveProvider**。

3.  已在 **GetForUserAsync()** 中使用 **Windows::System::User**，而不是 **XboxUser**，并且现在需要 SCID。

4.  没有本地“计算机”存储（即 **GetForMachineAsync()** 已被删除）。 请考虑对非漫游的本地保存数据改用 **Windows::Storage::ApplicationData**。

5.  将在无异常\*结果-类型对象（例如 **GameSaveProviderGetResult**）中返回异步结果；你可以通过它检查**状态**属性，如果没有任何错误，将从**值**属性读取返回的对象。

6.  **ConnectedStorageErrorStatus enum** 已重命名为 **GameSaveErrorStatus**，并且将在结果的**状态**属性中返回。 所有旧值均存在，并且添加了一些新值：

-   中止

-   ObjectExpired

-   确定

-   UserHasNoXboxLiveInfo

请参阅 GameSave 示例或 NetRumble 示例以了解示例用法。

**注意** Gamesaveutil.exe 等效于 xbstorage.exe（包含在 XDK 内的命令行开发人员实用工具）。 安装 Xbox Live 平台扩展 SDK 后，可以在以下位置找到此实用工具：C:\\Program Files (x86)\\Windows Kits\\10\\Extension SDKs\\XboxLive\\1.0\\Bin\\x64

## <a name="summary"></a>小结

将现有游戏代码从 Xbox One XDK 移植到新的 UWP 时，你很可能会遇到本白皮书中概述的 API 更改和新要求。 它重点介绍了应用程序和环境设置以及与 Xbox Live 服务相关的功能区域，例如多人游戏和连接存储。 有关详细信息，请访问本文和以下参考中提供的链接，并确保访问[开发人员论坛](https://forums.xboxlive.com) 的“Windows 10”部分，以获取更多帮助、答案和资讯。

## <a name="references"></a>参考

-   [从 Xbox One 移植到 Windows10](https://developer.xboxlive.com/en-us/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx)

-   [Xbox One 白皮书](https://developer.xboxlive.com/en-us/platform/development/education/Pages/WhitePapers.aspx)

-   [示例](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx)
