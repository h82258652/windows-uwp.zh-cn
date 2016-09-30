---
author: Mtoepke
title: "Xbox One 开发者预览版上的 UWP 已知问题"
description: 
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: e016be20af9a0d7a67fa383cbdc93083d12a1113

---

# Xbox 开发者预览版上的 UWP 已知问题

本主题介绍 Xbox 开发者预览版上的 UWP 的已知问题。 有关此开发者预览版的详细信息，请参阅 [Xbox 上的 UWP](index.md)。 

\[如果你通过某个 API 参考主题中的链接转到此处，并且要查找通用设备系列 API 信息，请参阅 [Xbox 上尚不支持的 UWP 功能](http://go.microsoft.com/fwlink/?LinkID=760755)。\]

Xbox 开发者预览版系统更新将包括实验性的软件和提前预发布的软件。 这意味着某些热门游戏和应用将无法按预期运行，并且可能会遇到偶发的崩溃和数据丢失。 当你退出开发者预览版时，主机将恢复出厂设置，使得你必须重新安装所有游戏、应用和内容。

对于开发人员而言，这意味着并非所有开发人员工具和 API 都能按预期运行。 并非所有适用于最终版本的功能都包含在内或都具有发行质量。 
**特别是，此预览版中的系统性能并不反映最终版本的系统性能。**

下表重点介绍你可能在此版本中遇到的某些已知问题，但该列表并没有包括所有问题。 

**我们希望收到你的反馈**，因此请在[开发通用 Windows 应用](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop)论坛上报告你发现的任何问题。 

如果遇到问题，请阅读本主题中的信息、参阅[常见问题](frequently-asked-questions.md)以及使用论坛来寻求帮助。


<!--## Developing games-->

## 鼠标模式支持

从此预览版开始，默认情况下 XAML 和托管的 Web 应用的_鼠标模式_处于启用状态。 未选择退出的所有应用程序都将收到鼠标指针，类似于 Xbox Edge 浏览器中的情形。

**我们强烈建议开发人员应关闭鼠标模式，然后针对控制器 (X-Y) 导航进行优化。**

若要关闭 XAML 中的鼠标模式，请遵循此示例：

```code
public App() {
    this.InitializeComponent();
    this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
    this.Suspending += OnSuspending;
}
```

若要关闭 HTML/JavaScript 应用中的鼠标模式，请遵循此示例：

```code
// Turn off mouse mode
navigator.gamepadInputEmulation = "keyboard";
```

有关详细信息，包括如何在 HTML/JavaScript 应用中打开方向导航，请参阅[如何禁用鼠标模式](how-to-disable-mouse-mode.md#html)。

> **注意**&nbsp;&nbsp;在此开发者预览版中，当鼠标模式处于打开状态时，使用控制器上的右操纵杆执行平移可能会导致你的主机挂起。 如果遇到此问题，则必须重启你的主机。

有关鼠标模式支持的信息，请参阅[针对 Xbox 和电视进行设计](https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv?f=255&MSPPError=-2147217396#mouse-mode)主题。 此主题包含有关如何启用和禁用鼠标模式的信息，以便你可以选择适合你的应用的行为。

## 必须进行用户登录才可以部署应用（错误 0x87e10008）

应用现在需要用户先进行登录才可以启动（你必须先进行用户登录，然后才可以从 VS 2015 开始调试 (F5)）。从 Visual Studio 中收到的当前错误消息并不直观：
 
![无法激活 Windows 应用商店应用](images/windows-store-app-activation-error.jpg)
 
若要解决此问题，请从 Xbox Shell 或 DevHome 进行用户登录，然后再部署你的应用。
 
## 后台应用的内存限制尚未强制执行
 
在此预览版中，并未针对后台运行的应用强制执行 128 MB 限制。 这意味着，如果你的应用在后台运行时超过 128 MB，它仍可以分配内存。
 
对于此问题，目前尚没有解决方法。 你应该相应地控制你的内存使用量；在将来的预览版中，如果你的应用超过 128 MB 限制，它会收到内存分配失败消息。
 
## 在“家长控制”处于打开状态的情况下，无法通过 VS 进行部署

如果主机在“设置”中打开了“家长控制”，则通过 VS 启动你的应用会失败。

若要解决该问题，可以临时禁用“家长控制”，也可以：
1. 在“家长控制”处于关闭状态的情况下，将你的应用部署到该主机
2. 打开“家长控制”
3. 从主机启动你的应用
4. 输入 PIN 或密码以允许应用启动
5. 应用将启动
6. 关闭应用
7. 从 VS 中使用 F5 进行启动，然后应用将在没有任何提示的情况下启动

此时，权限是_粘连的_，除非你注销用户，否则即使卸载并重新安装该应用也是如此。
 
有另外一种仅适用于子女帐户的免除类型。 子女帐户需要家长登录以授予权限，但当家长将选项选择为“始终”****时，他们就可以允许孩子启动该应用。 该免除存储在云中且持续有效，即使孩子注销并重新登录也是如此。   

<!--### x86 vs. x64

By the time we release later this year, we will have great support for both x86 and x64, and we do support x86 in this preview. 
However, x64 has had much more testing to date (the Xbox shell and all of the apps running on the console today are x64), and so we recommend using x64 for your projects. 
This is particularly true for games.

If you decide to use x86, please report any issues you see on the forum.

Also see [Switching build flavors can cause deployment failures](known-issues.md#switching-build-flavors-can-cause-deployment-failures) later on this page.-->

<!--### Game engines

We have tested some popular game engines, but not all of them, and our test coverage for this preview has not been comprehensive. 
Your mileage may vary. 

The following game engines have been confirmed to work:
* [Construct 2](https://www.scirra.com/)

There are likely others that are working too. We would love to get your feedback on what you find. 
Please use the forum to report any issues you see.-->

## DirectX 12 支持

Xbox One 上的 UWP 支持 DirectX 11 功能级别 10。 DirectX 12 在此情况下不受支持。 Xbox One（类似于所有的传统游戏主机）是一个专门的硬件，需要特定 SDK 才可以充分发挥其潜能。 如果你正在开发需要最大限度发挥 Xbox One 硬件潜能的游戏，你可以注册 [ID@XBOX](http://www.xbox.com/Developers/id) 计划来获取该 SDK 的访问权限（包括 DirectX 12 支持）。

<!-- ### Xbox One Developer Preview disables game streaming to Windows 10

Activating the Xbox One Developer Preview on your console will prevent you from streaming games from your Xbox One to the Xbox app on Windows 10, even if your console is set to retail mode. 
To restore the game streaming feature, you must leave the developer preview. -->

## 电视安全区域的已知问题

默认情况下，Xbox 上适用于 UWP 应用的屏幕区域应插入电视安全区域。 但是，Xbox One 开发者预览版包含一个已知 Bug，这会导致电视安全区域在 [0, 0] 启动，而非在 [_offset_, _offset_] 启动。

> **注意**&nbsp;&nbsp;这仅适用于使用 JavaScript 的 UWP 应用。

解决此问题的最简单方式是禁用电视安全区域，如以下 JavaScript 示例所示。

    var applicationView = Windows.UI.ViewManagement.ApplicationView.getForCurrentView();

    applicationView.setDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.useCoreWindow);

有关电视安全区域的详细信息，请参阅[针对 Xbox 和电视进行设计](https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv)。

<!--## System resources for UWP apps and games on Xbox One

UWP apps and games running on Xbox One share resources with the system and other apps, and so the system governs the resources that are available to any one game or app. 
If you are running into memory or performance issues, this may be why. 
For more details, see [System resources for UWP apps and games on Xbox One](system-resource-allocation.md).-->

<!--
## Networking using traditional sockets

In this developer preview, inbound and outbound network access from the console that uses traditional TCP/UDP sockets (WinSock, Windows.Networking.Sockets) is not available. 
Developers can still use HTTP and WebSockets.
--> 


## UWP API 覆盖范围

并非所有 UWP API 在 Xbox 上都受支持。 有关我们已知不起作用的 API 列表，请参阅 [Xbox 上尚不支持的 UWP 功能](http://go.microsoft.com/fwlink/p/?LinkId=760755)。 如果你发现其他 API 的问题，请通过论坛报告它们。 

<!--## XAML controls do not look like or behave like the controls in the Xbox One shell

In this developer preview, the XAML controls are not in their final form. In particular:
* Gamepad X-Y navigation does not work reliably for all controls.
* Controls do not look like controls in the Xbox shell. This includes the control focus rectangle.
* Navigating between controls does not automatically make “navigation sounds.”

These issues will be addressed in a future developer preview.-->

<!--## Visual Studio and deployment issues

### Switching build flavors can cause deployment failures

Switching between Debug and Release builds, or between x86 and x64, or between Managed and .Net Native builds, can cause deployment failures. 

The simplest way to avoid these issues for this preview is to stick to Debug and one architecture. 

If you do hit this issue, uninstalling your app in the Collections app on your Xbox One will typically resolve it.

> ****&nbsp;&nbsp;Uninstalling your app from Windows Device Portal (WDP) will not resolve the issue.

If your issues persist, uninstall your app or game in the Collections app, leave Developer Mode, restart to Retail Mode and then switch back to Developer Mode.
You may also need to restart Visual Studio and clean your solution.

For more information, see the “Fixing deployment failures” section in [Frequently asked questions](frequently-asked-questions.md).

### Uninstalling an app while you are debugging it in Visual Studio will cause it to fail silently

Attempting to uninstall an app that is running under the debugger via the WDP “Installed Apps” tool will cause it to silently fail. 
The workaround is to stop debugging the app in Visual Studio before attempting to remove it via WDP.

### Visual Studio/Xbox PIN pairing failures

It is possible to get into a state where the PIN pairing between Visual Studio and your Xbox One gets out of sync. 
If PIN pairing fails, use the “Remove all pairings” button in Dev Home, restart Xbox One, restart your development PC, and then try again.--> 


## Windows Device Portal (WDP) 预览版

<!--### Starting WDP from Dev Home crashes Dev Home

When you start WDP in Dev Home, it will cause Dev Home to crash after you have entered your user name and password and selected **Save**. 
The credentials are saved but WDP is not started. 
You can start WDP by restarting Xbox One.--> 

<!--### Disabling WDP in Dev Home does not work

If you disable WDP in Dev Home, it will be turned off. 
However, when you restart your Xbox One, WDP will be started again. 
You can work around this issue by using **Reset and keep my games & apps** to delete any stored state on your Xbox One. 
Go to Settings > System > Console info & updates > Reset console, and then select the **Reset and keep my games & apps** button.

> **Caution**&nbsp;&nbsp;Doing this will delete all saved settings on your Xbox One including wireless settings, user accounts and any game progress that has not been saved to cloud storage.

> **Caution**&nbsp;&nbsp;DO NOT select the **Reset and remove everything** button.
This will delete all of your games, apps, settings and content, deactivate Developer Mode, and remove you console from the Developer Preview group.

### The columns in the “Running Apps” table do not update predictably. 

Sometimes this is resolved by sorting a column on the table.-->

### WDP UI 在 Internet Explorer 7 中无法正确显示 

默认情况下，当使用 Internet Explorer 7 时，WDP UI 在该浏览器中无法正确显示。 可以通过关闭 Internet Explorer 7 用于 WDP 的兼容性视图来修复此问题。

### 导航到 WDP 导致证书警告

你将收到已提供证书的警告（类似于以下屏幕截图），因为 Xbox One 主机签名的安全证书不被视为众所周知的受信任发布者。 单击“继续浏览此网站”可访问 Windows Device Portal。

![网站安全证书警告](images/security_cert_warning.jpg)

<!--## Dev Home

Occasionally, selecting the “Manage Windows Device Portal” option in Dev Home will cause Dev Home to silently exit to the Home screen. 
This is caused by a failure in the WDP infrastructure on the console and can be resolved by restarting the console.-->

## 另请参阅
- [常见问题](frequently-asked-questions.md)
- [Xbox One 上的 UWP](index.md)



<!--HONumber=Jul16_HO2-->


