---
author: Mtoepke
title: Xbox One 开发者预览版上的 UWP 已知问题
description: 
area: Xbox
---

# Xbox 开发者预览版上的 UWP 已知问题

本主题介绍 Xbox 开发者预览版上的 UWP 的已知问题。 
有关此开发者预览版的详细信息，请参阅 [Xbox 上的 UWP](index.md)。 

\[如果你通过某个 API 参考主题中的链接转到此处，并且要查找通用设备系列 API 信息，请参阅 [Xbox 上尚不支持的 UWP 功能](http://go.microsoft.com/fwlink/?LinkID=760755)。\]

Xbox 开发者预览版系统更新将包括实验性的软件和提前预发布的软件。 
这意味着某些热门游戏和应用将无法按预期运行，并且可能会遇到偶发的崩溃和数据丢失。 
当你退出开发者预览版时，主机将恢复出厂设置，使得你必须重新安装所有游戏、应用和内容。

对于开发人员而言，这意味着并非所有开发人员工具和 API 都能按预期运行。 
并非所有适用于最终版本的功能都包含在内或都具有发行质量。 
**特别是，此预览版中的系统性能并不反映最终版本的系统性能。**

下表重点介绍你可能在此版本中遇到的某些已知问题，但该列表并没有包括所有问题。 

**我们希望收到你的反馈**，因此请在[开发通用 Windows 应用](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop)论坛上报告你发现的任何问题。 

如果遇到问题，请阅读本主题中的信息、参阅[常见问题](frequently-asked-questions.md)以及使用论坛来寻求帮助。


## 开发游戏

### x86 与 x64

等到我们今年晚些时候发布时，我们将对 x86 和 x64 同时提供出色支持，并且我们已在此预览版中提供了对 x86 的支持。 
不过，迄今为止，已对 x64 执行了非常多的测试（当前，Xbox shell 和主机上运行的所有应用都是 x64 的），因此我们建议将 x64 用于你的项目。 
这对游戏而言尤其如此。

如果你决定使用 x86，请在论坛上报告你发现的任何问题。

另请参阅本页面后面的[切换生成风格可能导致部署失败](known-issues.md#switching-build-flavors-can-cause-deployment-failures)。

### 游戏引擎

我们已测试一些热门游戏引擎，但未对所有游戏引擎进行测试，并且我们对此预览版的测试覆盖并不全面。 
实际效果可能会有所不同。 

以下游戏引擎已确认可以运行：
* [Construct 2](https://www.scirra.com/)

其他引擎可能也可以运行。 我们希望收到你发现的问题的反馈。 
请通过论坛报告你发现的任何问题。

### DirectX 12 支持

Xbox One 上的 UWP 支持 DirectX 11 功能级别 10。 
DirectX 12 在此情况下不受支持。 
Xbox One（类似于所有的传统游戏主机）是一个专门的硬件，需要特定 SDK 才可以充分发挥其潜能。 
如果你正在开发需要最大限度发挥 Xbox One 硬件潜能的游戏，你可以注册 [ID@XBOX](http://www.xbox.com/en-us/Developers/id) 计划来获取该 SDK 的访问权限（包括 DirectX 12 支持）。

### Xbox One 开发者预览版禁用 Windows 10 的游戏流式传输。

在主机上激活 Xbox One 开发者预览版阻止你将游戏从 Xbox One 流式传输到 Windows 10 上的 Xbox 应用，即使主机设置为零售模式也不行。 若要还原游戏流式传输功能，则必须退出开发者预览版。

### 电视安全区域的已知问题

默认情况下，Xbox 上适用于 UWP 应用的屏幕区域应插入电视安全区域。 但是，Xbox One 开发者预览版包含一个已知 Bug，会导致电视安全区域在 \[0, 0\] 启动，而非在\ [_offset_, _offset_\] 启动。

有关电视安全区域的详细信息，请参阅 [https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv](https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv)。 

解决此问题的最简单方式是禁用电视安全区域，如以下 JavaScript 示例所示。

    var applicationView = Windows.UI.ViewManagement.ApplicationView.getForCurrentView();

    applicationView.setDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.useCoreWindow);

### 尚不支持鼠标模式

\[https://msdn.microsoft.com/zh-cn/windows/uwp/input-and-devices/designing-for-tv\] \(https://msdn.microsoft.com/zh-cn/windows/uwp/input-and-devices/designing-for-tv?f=255&amp;MSPPError=-2147217396#mouse-mode\) 主题中介绍的_鼠标模式_功能在 Xbox One 开发者预览版中尚不受支持。

## Xbox One 上 UWP 应用和游戏的系统资源

Xbox One 上运行的 UWP 应用和游戏与系统和其他应用共享资源，因此系统将管理可用于任何一个游戏或应用的资源。 
如果你遇到内存或性能问题，这可能就是原因所在。 
有关更多详细信息，请参阅 [Xbox One 上 UWP 应用和游戏的系统资源](system-resource-allocation.md)。


## 使用传统套接字的网络

在此开发人员预览版中，无法使用传统 TCP/UDP 套接字（WinSock、Windows.Networking.Sockets）执行主机的入站和出站网络访问。 
开发人员仍可以使用 HTTP 和 WebSocket。 


## UWP API 覆盖范围

在此预览版中，并非所有 UWP API 在 Xbox 上都能按预期工作。 
有关我们已知不起作用的 API 列表，请参阅 [Xbox 上尚不支持的 UWP 功能](http://go.microsoft.com/fwlink/p/?LinkId=760755)。 
如果你发现其他 API 的问题，请通过论坛报告它们。 

## XAML 控件的外观或行为不同于 Xbox One shell 中的控件

在此开发人员预览版中，这些 XAML 控件并非其最终形态。 尤其是：
* 游戏板 X-Y 导航对于所有控件而言，工作并不可靠。
* 控件的外观不同于 Xbox shell 中的控件。 这包括控件聚焦框。
* 在控件之间导航不会自动发出“导航声音”。

我们将在以后的开发人员预览版中解决这些问题。

## Visual Studio 和部署问题

### 切换生成风格可能导致部署失败

在调试版本和发布版本之间切换、在 x86 和 x64 之间切换或在托管版本和 .Net 本机版本之间切换可能导致部署失败。 

对于此预览版而言，避免这些问题的最简单方法是坚持使用调试和一个体系结构。 

如果遇到此问题，在 Xbox One 的集合应用中卸载你的应用通常可以解决该问题。

> **注意** &nbsp;&nbsp;从 Windows Device Portal (WDP) 中卸载你的应用无法解决此问题。

如果问题仍然存在，请在集合应用中卸载你的应用或游戏、退出开发人员模式、重新启动到零售模式，然后切换回开发人员模式。

有关详细信息，请参阅[常见问题](frequently-asked-questions.md)中的“修复部署失败”部分。

### 如果在 Visual Studio 中调试某个应用过程中卸载该应用，将导致它无提示地失败

如果尝试通过 WDP“已安装应用”工具卸载在调试程序下运行的应用，将导致该应用无提示地失败。 
解决方法是先在 Visual Studio 中停止调试应用，然后尝试通过 WDP 删除该应用。

### Visual Studio/Xbox PIN 配对失败

可能进入某种状态，其中 Visual Studio 和你 Xbox One 之间的 PIN 配对变得不同步。 
如果 PIN 配对失败，请在“开发人员主页”中使用“删除所有配对”按钮、重新启动 Xbox One、重新启动你的开发电脑，然后再试一次。 


## Windows Device Portal (WDP) 预览版

### 从“开发人员主页”启动 WDP 会导致“开发人员主页”崩溃

如果已在“开发人员主页”中启动了 WDP，在你输入用户名和密码并选择“保存”****后，将导致“开发人员主页”崩溃。 
凭据将保存，但 WDP 无法启动。 
可以通过重新启动 Xbox One 来启动 WDP。 

### 在“开发人员主页”中禁用 WDP 不起作用

如果在“开发人员主页”中禁用 WDP，将关闭 WDP。 
然而，当重新启动你的 Xbox One 时，将再次启动 WDP。 
可以通过使用“重置并保留我的游戏和应用”****删除 Xbox One 上存储的任何状态来解决此问题。 
转到“设置”&gt;“系统”&gt;“主机信息和更新”&gt;“重置主机”，然后选择“重置并保留我的游戏和应用”****按钮。

> **警告** &nbsp;&nbsp;执行此操作将删除 Xbox One 上保存的所有设置，包括无线设置、用户帐户和尚未保存到云存储的任何游戏进度。

> **警告** &nbsp;&nbsp;不要选择“重置和删除所有内容”****按钮。
这将删除你的所有游戏、应用、设置和内容、停用开发人员模式，然后从开发者预览版组中删除你的主机。

### “正在运行的应用”表中的列不会以可预见的方式进行更新。 

有时可通过对该表中的某列进行排序来解决此问题。

### WDP UI 在 Internet Explorer 11 中无法正确显示 

默认情况下，当使用 Internet Explorer 11 时，WDP UI 在该浏览器中无法正确显示。 
可以通过关闭 Internet Explorer 11 用于 WDP 的兼容性视图来修复此问题。

### 导航到 WDP 导致证书警告

你将收到已提供证书的警告（类似于以下屏幕截图），因为 Xbox One 主机签名的安全证书不被视为众所周知的受信任发布者。 
单击“继续浏览此网站”可访问 Windows Device Portal。

![网站安全证书警告](images/security_cert_warning.jpg)

## 开发人员主页

有时，在“开发人员主页”中选择“管理 Windows Device Portal”选项将导致“开发人员主页”无提示地退出到主屏幕。 
主机上的 WDP 基础结构中的故障会导致该问题，可通过重新启动该主机来解决该问题。

## 另请参阅
- [常见问题](frequently-asked-questions.md)
- [Xbox One 上的 UWP](index.md)


<!--HONumber=May16_HO2-->


