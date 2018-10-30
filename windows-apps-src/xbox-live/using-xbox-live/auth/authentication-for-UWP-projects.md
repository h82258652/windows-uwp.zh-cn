---
title: UWP 项目的身份验证
author: aablackm
description: 了解如何在通用 Windows 平台 (UWP) 游戏中登录 Xbox Live 用户。
ms.assetid: e54c98ce-e049-4189-a50d-bb1cb319697c
ms.author: aablackm
ms.date: 03/14/2018
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 身份验证, 登录
ms.localizationpriority: medium
ms.openlocfilehash: 36373fbc25c7f478bda9bbbf14f996f3997e91d2
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5740838"
---
# <a name="authentication-for-uwp-projects"></a>UWP 项目的身份验证

为了在游戏中充分利用 Xbox Live 功能，用户需要创建一个 Xbox Live 个人资料，以在 Xbox Live 社区标识自己的身份。  Xbox Live 服务将会跟踪使用该 Xbox Live 个人资料的游戏相关活动，例如用户的玩家代号和玩家图片、用户的游戏好友、用户所玩的游戏、用户已解锁的成就、用户在特定游戏的排行榜中的排名等。

如果想要在特定设备上访问特定游戏的 Xbox Live 服务，则用户需要先进行身份验证。  游戏可以通过调用 Xbox Live API 来启动身份验证过程。  在某些情况下，用户将会看到一个用于提供其他信息的界面，如输入要使用的 Microsoft 帐户的用户名和密码、提供游戏的授权同意、解决帐户问题、接受新的使用条款等。

经过身份验证后，用户将与设备关联在一起，直到他们通过 Xbox 应用明确注销 Xbox Live。  在一台设备上一次只允许对一个玩家进行身份验证（适用于所有 Xbox Live 游戏）；如果要在设备上对新玩家进行身份验证，则经过身份验证的现有玩家必须先注销。

## <a name="steps-to-sign-in"></a>登录步骤

在高级别上，可以通过以下步骤使用 Xbox Live API：

1. 创建一个代表用户的 XboxLiveUser 对象
2. 在启动时静默登录 Xbox Live
3. 需要时，尝试使用 UX 登录
4. 创建一个基于交互用户的 Xbox Live 上下文
5. 使用 Xbox Live 上下文访问 Xbox Live 服务
6. 在退出游戏或用户注销时，发布 XboxLiveUser 对象和 XboxLiveContext 对象，方法是将其设为 null

### <a name="creating-an-xboxliveuser-object"></a>创建 XboxLiveUser 对象

大部分 Xbox Live 活动与 Xbox Live 用户相关。  作为游戏开发人员，你需要先创建一个代表本地用户的 XboxLiveUser 对象。

C++：

```cpp
auto xboxUser = std::make_shared<xbox_live_user>(Windows::System::User^ windowsSystemUser);
```

C++/CX (WinRT)：

```cpp
XboxLiveUser xboxUser = ref new XboxLiveUser(Windows::System::User^ windowsSystemUser);
```

C# (WinRT)：

```csharp
XboxLiveUser xboxUser = new XboxLiveUser(Windows.System.User windowsSystemUser);
```

* **windowsSystemUser** 将用于关联 xbox live 用户的 Windows 系统用户对象。 如果应用为单用户应用程序 (SUA)，则可能为 nullptr。
  * 有关单用户应用程序 (SUA) 和多用户应用程序 (MUA) 的更多信息，请参阅[多用户应用程序简介](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/multi-user-applications#single-user-applications)
  * 有关如何获取 Windows 中的 Windows::System::User^ 的更多信息，请参阅[在 UWP 上检索 windows 系统用户](retrieving-windows-system-user-on-UWP.md)

### <a name="sign-in-silently-to-xbox-live-at-startup"></a>在启动时静默登录 Xbox Live ###

你的游戏应在启动之后尽可能早地（即便是在你看到用户界面之前）对用户能否登录 Xbox Live 进行身份验证，以通过 Xbox Live 服务预提取数据。

若要对本地用户进行静默身份验证，请调用

C++：

```cpp
pplx::task<xbox_live_result<sign_in_result>> xbox_live_user::signin_silently(Platform::Object^ coreDispatcher)
```

C++/CX (WinRT)：

```cpp
Windows::Foundation::IAsyncOperation<SignInResult^>^ XboxLiveUser::SignInSilentlyAsync(Platform::Object^ coreDispatcher)
```

C# (WinRT)：

```csharp
Microsoft.Xbox.Services.System.SignInResult XboxLiveUser.SignInSilentlyAsync(Windows.UI.Core.CoreDispatcher coreDispatcher);
```

* **coreDispatcher**

  线程调度程序用于在线程之间进行通信。 尽管静默登录 API 不会显示任何 UI，但 XSAPI 仍需要使用 UI 线程调度程序来获取与 appx 区域设置相关的信息。 你可以通过调用 UI 线程中的 Windows::UI::Core::CoreWindow::GetForCurrentThread()->Dispatcher 来获取静态 UI 线程调度程序。 或者，如果你确定该 UI 将在 UI 线程上调用，则可以传递 nullptr（例如在 JS UWA 上）。


静默登录尝试存在 3 种可能结果

* **成功**

  如果设备处于联机状态，这意味着用户经过身份验证可成功登录 Xbox Live，并且我们可以获得有效的令牌。

  如果设备处于脱机状态，这意味着用户先前已经过身份验证可成功登录 Xbox Live，并且未从此游戏中明确注销。  请注意，在此情况下，无法确保游戏可访问有效的令牌，只能保证用户的身份已知且经过认证。    游戏通过 xbox 用户 id (xuid) 和玩家代号来确定用户的身份。

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

```cpp
pplx::task<xbox_live_result<sign_in_result>> xbox_live_user::signin(Platform::Object^ coreDispatcher)
```


C++/CX (WinRT)：

```cpp
Windows::Foundation::IAsyncOperation<SignInResult^>^ XboxLiveUser::SignInAsync(Platform::Object^ coreDispatcher)
```

C# (WinRT)：

```csharp
Microsoft.Xbox.Services.System.SignInResult  XboxLiveUser.SignInAsync(Windows.UI.Core.CoreDispatcher coreDispatcher);
```

* **coreDispatcher**

  线程调度程序用于在线程之间进行通信。 登录 API 需要使用 UI 调度程序，因此，它可显示登录 UI 并获得与 appx 区域设置相关的信息。 你可以通过调用 UI 线程中的 Windows::UI::Core::CoreWindow::GetForCurrentThread()->Dispatcher 来获取静态 UI 线程调度程序。 或者，如果你确定该 UI 将在 UI 线程上调用，则可以传递 nullptr（例如在 JS UWA 上）。

通过 UX 的登录尝试存在 3 种可能结果：

* **成功**

  如果设备处于联机状态，这意味着用户经过身份验证可成功登录 Xbox Live，并且我们可以获得有效的令牌。

  如果设备处于脱机状态，这意味着用户先前已经过身份验证可成功登录 Xbox Live，并且未从此游戏中明确注销。  请注意，在此情况下，无法确保游戏可访问有效的令牌，只能保证用户的身份已知且经过认证。    游戏通过 xbox 用户 id (xuid) 和玩家代号来确定用户的身份。

* **UserCancel**

  这意味着用户在完成之前已取消登录操作。  出现此情况时，游戏应不会通过 UX 自动重试登录。  相反，它应会显示游戏内 UX，使用户能够重试登录操作。  （例如，登录按钮）

* **其他错误**

  运行时因其他原因无法登录。  通常情况下，这些问题不可由游戏或用户操作。 使用 c++ API 时，需要通过检查 xbox_live_result<>.err() 来检查错误；在 WinRT 上，需要捕获 Platform::Exception^。

## <a name="sign-in-code-examples"></a>登录代码示例

### <a name="c"></a>C++

```cpp

#include "xsapi\services.h" // contains the xbox::services::system namespace

using namespace xbox::services::system; // contains definitions necessary for sign-in

void SignInSample::SignIn()
{
    //1. Create an xbox_live_user object
    m_user = std::make_shared<xbox::services::system::xbox_live_user>(); // m_user declared in header file

    //2. Sign-In silently to Xbox Live at startup
    m_user->signin_silently()
        .then([this](xbox::services::xbox_live_result<xbox::services::system::sign_in_result> result)
    {
        if (!result.err())
        {
            auto rPayload = result.payload();
            switch (rPayload.status())
            {
            case xbox::services::system::sign_in_status::success:
                // sign-in successful
                signIn = true;
                break;
            case xbox::services::system::sign_in_status::user_interaction_required:
                // 3. Attempt to sign-in with UX if required
                m_user->signin(Windows::UI::Core::CoreWindow::GetForCurrentThread()->Dispatcher)
                    .then([this](xbox::services::xbox_live_result<xbox::services::system::sign_in_result> loudResult) // use task_continuation_context::use_current() to make the continuation task running in current apartment 
                {
                    if (!loudResult.err())
                    {
                        auto resPayload = loudResult.payload();
                        switch (resPayload.status())
                        {
                        case xbox::services::system::sign_in_status::success:
                            // sign-in successful
                            signIn = true;
                            break;
                        case xbox::services::system::sign_in_status::user_cancel:
                            // user cancelled sign in 
                            // present in-game UX that allows the user to retry the sign-in operation. (For example, a sign-in button)
                            break;
                        }
                    }
                    else
                    {
                        //login has failed at this point
                    }
                }, concurrency::task_continuation_context::use_current());
                break;
            }
        }
    });
    if (signIn)
    {
        // 4. Create an Xbox Live context based on the interacting user
        m_xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>(m_user); // m_xboxLiveContext declared in header file

        // add sign out event handler
        AddSignOut();
    }
}

void SignInSample::AddSignOut()
{
    xbox::services::system::xbox_live_user::add_sign_out_completed_handler(
        [this](const xbox::services::system::sign_out_completed_event_args&)

    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        m_user = NULL;
        m_xboxLiveContext = NULL;
    });
}

```

### <a name="c-winrt"></a>C# (WinRT)

```csharp

using System.Diagnostics;
using Microsoft.Xbox.Services.System;
using Microsoft.Xbox.Services;

public async Task SignIn()
{
    bool signedIn = false;

    // Get a list of the active Windows users.
    IReadOnlyList<Windows.System.User> users = await Windows.System.User.FindAllAsync();

    // Acquire the CoreDispatcher which will be required for SignInSilentlyAsync and SignInAsync.
    Windows.UI.Core.CoreDispatcher UIDispatcher = Windows.UI.Xaml.Window.Current.CoreWindow.Dispatcher; 

    try
    {
        // 1. Create an XboxLiveUser object to represent the user
        XboxLiveUser primaryUser = new XboxLiveUser(users[0]);

        // 2. Sign-in silently to Xbox Live
        SignInResult signInSilentResult = await primaryUser.SignInSilentlyAsync(UIDispatcher);
        switch (signInSilentResult.Status)
        {
            case SignInStatus.Success:
                signedIn = true;
                break;
            case SignInStatus.UserInteractionRequired:
                //3. Attempt to sign-in with UX if required
                SignInResult signInLoud = await primaryUser.SignInAsync(UIDispatcher);
                switch(signInLoud.Status)
                {
                    case SignInStatus.Success:
                        signedIn = true;
                        break;
                    case SignInStatus.UserCancel:
                        // present in-game UX that allows the user to retry the sign-in operation. (For example, a sign-in button)
                        break;
                    default:
                        break;
                }
                break;
            default:
                break;
        }
        if(signedIn)
        {
            // 4. Create an Xbox Live context based on the interacting user
            Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(user);

            //add the sign out event handler
            XboxLiveUser.SignOutCompleted += OnSignOut;
        }
    }
    catch (Exception e)
    {
        Debug.WriteLine(e.Message);
    }

}

public void OnSignOut(object sender, SignOutCompletedEventArgs e)
    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        primaryUser = null;
        xboxLiveContext = null;
    }
```

## <a name="sign-out"></a>注销

### <a name="handling-user-sign-out-completed-event"></a>处理用户注销完成事件

如果发生以下任一情况，则用户将从游戏中注销：

1. 用户通过 Xbox 应用 (Windows 10) 或主机界面 (Xbox One) 注销。 注销将影响为此用户安装的所有支持 Xbox Live 的应用。
2. 用户已切换至不同的 Microsoft 帐户
3. 用户通过不同设备登录到相同游戏

在所有这些情况下，游戏将收到来自 `xbox_live_user::add_sign_out_completed_handler` 或 `XboxLiveUser::SignOutCompleted` 处理程序的事件。  游戏必须正确处理注销完成事件：

1. 游戏应向用户显示清楚的视觉指示，表明她/他已从 Xbox Live 注销。
2. 游戏无法调用事件处理程序中的任何 Xbox Live 服务 API，因为用户已注销且无可用的授权令牌。

## <a name="sign-out-handler-code-samples"></a>注销处理程序代码示例

### <a name="c"></a>C++

```cpp

xbox::services::system::xbox_live_user::add_sign_out_completed_handler(
        [this](const xbox::services::system::sign_out_completed_event_args&)

    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        m_user = NULL;
        m_xboxLiveContext = NULL;
    });

```

### <a name="c-winrt"></a>C# (WinRT)

```csharp
XboxLiveUser.SignOutCompleted += OnUserSignOut;

public void OnSignOut(object sender, SignOutCompletedEventArgs e)
        {
            // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
            primaryUser = null;
            xboxLiveContext = null;
        }
```

## <a name="determining-if-the-device-is-offline"></a>确定设备是否处于脱机状态

脱机时，如果用户已登录一次，则登录 API 仍成功，并且将会返回最后登录的帐户。

如果可以脱机玩游戏（配套模式等），无论设备是处于联机还是脱机状态，均允许用户玩游戏并且将通过 WriteInGameEvent API 和连接存储 API 记录游戏进度，这两者在设备处于脱机状态时仍可正常工作。

如果无法脱机玩游戏（多人游戏或基于服务器的游戏等），则游戏应调用 GetNetworkConnectivityLevel API 来确定设备是否处于脱机状态，并通知用户相关状态及可能的解决方案（例如，“你需要连接至 Internet 才能继续…”）

## <a name="online-status-code-samples"></a>联机状态代码示例

### <a name="c"></a>C++

```cpp

using namespace Windows::Networking::Connectivity;

//Retrieve the ConnectionProfile
ConnectionProfile^ InternetConnectionProfile = NetworkInformation::GetInternetConnectionProfile();

NetworkConnectivityLevel connectionLevel = InternetConnectionProfile->GetNetworkConnectivityLevel();

switch (connectionLevel)
{
case NetworkConnectivityLevel::InternetAccess:
    // User is connected to the internet.
    break;
case NetworkConnectivityLevel::ConstrainedInternetAccess: //Limited Internet Access Possible Authentication Required
     // display error message for user.
    LogConnectivityLine("Game Offline: Limited internet access, browser authentication may be required. "); //function writes to UI
    break;
default:
    LogConnectivityLine("Game Offline: No internet access.");
    break;
}

```

### <a name="c-winrt"></a>C# (WinRT)

```csharp
using Windows.Networking.Connectivity;

//Retrieve the ConnectionProfile
string connectionProfileInfo = string.Empty;
ConnectionProfile InternetConnectionProfile = NetworkInformation.GetInternetConnectionProfile();

NetworkConnectivityLevel connectionLevel = InternetConnectionProfile.GetNetworkConnectivityLevel();

switch(connectionLevel)
    {
        case NetworkConnectivityLevel.InternetAccess:
            // User is connected to the internet.
            break;
        case NetworkConnectivityLevel.ConstrainedInternetAccess: //Limited Internet Access Possible Authentication Required
            // display error message for user.
            LogConnectivityLine("Game Offline: Limited internet access, browser authentication may be required. "); //function writes to UI
            break;
        default:
            LogConnectivityLine("Game Offline: No internet access.");
            break;
    }
```