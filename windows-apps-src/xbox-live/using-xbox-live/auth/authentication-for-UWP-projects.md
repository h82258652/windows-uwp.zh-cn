---
title: "UWP 项目的身份验证"
author: KevinAsgari
description: "了解如何在通用 Windows 平台 (UWP) 游戏中登录 Xbox Live 用户。"
ms.assetid: e54c98ce-e049-4189-a50d-bb1cb319697c
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "xbox live, xbox, 游戏, uwp, windows 10, xbox one, 身份验证, 登录"
ms.openlocfilehash: 82ea421e8697d0231cd8ab00d42481594aee2145
ms.sourcegitcommit: 90fbdc0e25e0dff40c571d6687143dd7e16ab8a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2017
---
# <a name="authentication-for-uwp-projects"></a>UWP 项目的身份验证

为了在游戏中充分利用 Xbox Live 功能，用户需要创建一个 Xbox Live 个人资料，以在 Xbox Live 社区标识自己的身份。  Xbox Live 服务将会跟踪使用该 Xbox Live 个人资料的游戏相关活动，例如用户的玩家代号和玩家图片、用户的游戏好友、用户所玩的游戏、用户已解锁的成就、用户在特定游戏的排行榜中的排名等。

如果想要在特定设备上访问特定游戏的 Xbox Live 服务，则用户需要先进行身份验证。  游戏可以通过调用 Xbox Live API 来启动身份验证过程。  在某些情况下，用户将会看到一个用于提供其他信息的界面，如输入要使用的 Microsoft 帐户的用户名和密码、提供游戏的授权同意、解决帐户问题、接受新的使用条款等。

经过身份验证后，用户将与设备关联在一起，直到他们通过 Xbox 应用明确注销 Xbox Live。  在一台设备上一次只允许对一个玩家进行身份验证（适用于所有 Xbox Live 游戏）；如果要在设备上对新玩家进行身份验证，则经过身份验证的现有玩家必须先注销。

在高级别，你可以通过以下步骤使用 Xbox Live API：

1. 创建一个代表用户的 XboxLiveUser 对象
2. 在启动时静默登录 Xbox Live
3. 需要时，尝试使用 UX 登录
4. 创建一个基于交互用户的 Xbox Live 上下文
5. 使用 Xbox Live 上下文访问 Xbox Live 服务
6. 在退出游戏或用户注销时，发布 XboxLiveUser 对象和 XboxLiveContext 对象，方法是将其设为 null

### <a name="creating-an-xboxliveuser-object"></a>创建 XboxLiveUser 对象 ###
大部分 Xbox Live 活动与 Xbox Live 用户相关。  作为游戏开发人员，你需要先创建一个代表本地用户的 XboxLiveUser 对象。

C++：
```
auto user = std::make_shared<xbox_live_user>(Windows::System::User^ windowsSystemUser);
```

WinRT：
```
XboxLiveUser user = ref new XboxLiveUser(Windows::System::User^ windowsSystemUser);
```

* **windowsSystemUser** 将用于关联 xbox live 用户的 Windows 系统用户对象。 如果应用为单用户应用程序 (SUA)，则可能为 nullptr。
  * 有关单用户应用程序 (SUA) 和多用户应用程序 (MUA) 的更多信息，请参阅[多用户应用程序简介](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/multi-user-applications#single-user-applications)
  * 有关如何获取 Windows 中的 Windows::System::User^ 的更多信息，请参阅[在 UWP 上检索 windows 系统用户](retrieving-windows-system-user-on-UWP.md)

### <a name="sign-in-silently-to-xbox-live-at-startup"></a>在启动时静默登录 Xbox Live ###
你的游戏应在启动之后尽可能早地（即便是在你看到用户界面之前）对用户能否登录 Xbox Live 进行身份验证，以通过 Xbox Live 服务预提取数据。

若要对本地用户进行静默身份验证，请调用

C++：
```
pplx::task<xbox_live_result<sign_in_result>> xbox_live_user::signin_silently(Platform::Object^ coreDispatcher)
```

WinRT：
```
Windows::Foundation::IAsyncOperation<SignInResult^>^ XboxLiveUser::SignInSilentlyAsync(Platform::Object^ coreDispatcher)
```


* **coreDispatcher**

  线程调度程序用于在线程之间进行通信。 尽管静默登录 API 不会显示任何 UI，但 XSAPI 仍需要使用 UI 线程调度程序来获取与 appx 区域设置相关的信息。 你可以通过调用 UI 线程中的 Windows::UI::Core::CoreWindow::GetForCurrentThread()->Dispatcher 来获取静态 UI 线程调度程序。 或者，如果你确定该 UI 将在 UI 线程上调用，则可以传递 nullptr（例如在 JS UWA 上）。


静默登录尝试存在 3 种可能结果

* **成功**

  如果设备处于联机状态，这意味着用户经过身份验证可成功登录 Xbox Live，并且我们可以获得有效的令牌。

  如果设备处于脱机状态，这意味着用户先前已经过身份验证可成功登录 Xbox Live，并且未从此游戏中明确注销。  请注意，在此情况下，无法确保游戏可访问有效的令牌，只能保证用户的身份已知且经过认证。  游戏通过 xbox 用户 id (xuid) 和玩家代号来确定用户的身份。

* **UserInteractionRequired**

  这意味着运行时无法静默登录用户。  游戏应调用 `xbox_live_user::sign_in`，后者将通过调用 Xbox 标识提供程序来显示用户注册/登录所需的 UX 流。  常见问题包括：

  * 用户没有 Microsoft 帐户
  * 用户未设置游戏首选 Microsoft 帐户
  * 选定的 Microsoft 帐户没有 Xbox Live 个人资料
  * 用户需要接受 Microsoft 帐户同意


* **其他错误**

  运行时因其他原因无法登录。  通常情况下，这些问题不可由游戏或用户操作。 使用 c++ API 时，你需要通过检查 xbox_live_result<>.err() 来检查错误；在 WinRT 上，你需要捕获 Platform::Exception^。


### <a name="attempt-to-sign-in-with-ux-if-required"></a>需要时，尝试使用 UX 登录 ###
UX 启用时，如果静默登录不成功，则游戏应对用户登录 Xbox Live 进行身份验证，并且你已准备好显示用户界面。

若要通过 UX 对本地用户进行身份验证，请调用

C++：
```
pplx::task<xbox_live_result<sign_in_result>> xbox_live_user::signin(Platform::Object^ coreDispatcher)
```

WinRT：
```
Windows::Foundation::IAsyncOperation<SignInResult^>^ XboxLiveUser::SignInAsync(Platform::Object^ coreDispatcher)
```


* **coreDispatcher**

  线程调度程序用于在线程之间进行通信。 登录 API 需要使用 UI 调度程序，因此，它可显示登录 UI 并获得与 appx 区域设置相关的信息。 你可以通过调用 UI 线程中的 Windows::UI::Core::CoreWindow::GetForCurrentThread()->Dispatcher 来获取静态 UI 线程调度程序。 或者，如果你确定该 UI 将在 UI 线程上调用，则可以传递 nullptr（例如在 JS UWA 上）。

通过 UX 的登录尝试存在 3 种可能结果：

* **成功**

  如果设备处于联机状态，这意味着用户经过身份验证可成功登录 Xbox Live，并且我们可以获得有效的令牌。

  如果设备处于脱机状态，这意味着用户先前已经过身份验证可成功登录 Xbox Live，并且未从此游戏中明确注销。  请注意，在此情况下，无法确保游戏可访问有效的令牌，只能保证用户的身份已知且经过认证。  游戏通过 xbox 用户 id (xuid) 和玩家代号来确定用户的身份。

* **UserCancel**

  这意味着用户在完成之前已取消登录操作。  出现此情况时，游戏应不会通过 UX 自动重试登录。  相反，它应会显示游戏内 UX，使用户能够重试登录操作。  （例如，登录按钮）

* **其他错误**

  运行时因其他原因无法登录。  通常情况下，这些问题不可由游戏或用户操作。 使用 c++ API 时，你需要通过检查 xbox_live_result<>.err() 来检查错误；在 WinRT 上，你需要捕获 Platform::Exception^。

### <a name="handling-user-sign-out-completed-event"></a>处理用户注销完成事件 ###

如果发生以下任一情况，则用户将从游戏中注销：

1.  用户通过 Xbox 应用 (Windows 10) 或主机界面 (Xbox One) 注销。 注销将影响为此用户安装的所有支持 Xbox Live 的应用。
2.  用户已切换至不同的 Microsoft 帐户
3.  用户通过不同设备登录到相同游戏

在所有这些情况下，游戏将收到来自 `xbox_live_user::add_sign_out_completed_handler` 或 `XboxLiveUser::SignOutCompleted` 处理程序的事件。  游戏必须正确处理注销完成事件：
1. 游戏应向用户显示清楚的视觉指示，表明她/他已从 Xbox Live 注销。
2. 游戏无法调用事件处理程序中的任何 Xbox Live 服务 API，因为用户已注销且无可用的授权令牌。

### <a name="determining-if-the-device-is-offline"></a>确定设备是否处于脱机状态 ###

脱机时，如果用户已登录一次，则登录 API 仍成功，并且将会返回最后登录的帐户。

如果可以脱机玩游戏（配套模式等），无论设备是处于联机还是脱机状态，均允许用户玩游戏并且将通过 WriteInGameEvent API 和连接存储 API 记录游戏进度，这两者在设备处于脱机状态时仍可正常工作。

如果无法脱机玩游戏（多人游戏或基于服务器的游戏等），则游戏应调用 GetNetworkConnectivityLevel API 来确定设备是否处于脱机状态，并通知用户相关状态及可能的解决方案（例如，“你需要连接至 Internet 才能继续…”）
