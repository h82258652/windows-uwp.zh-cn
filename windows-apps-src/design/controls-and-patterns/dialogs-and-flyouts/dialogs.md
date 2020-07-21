---
Description: 对话框和浮出控件显示当用户请求这些元素或发生需要通知或批准的操作时出现的瞬态 UI 元素。
title: 对话框控件
label: Dialogs
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9208bb85f34e1ad0fd28a89e780ebfca536a8bad
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82969892"
---
# <a name="dialog-controls"></a>对话框控件

对话框控件是用于提供上下文应用信息的模式 UI 覆盖。 除非显式取消这些控件，否则它们会阻止与应用窗口的交互。 它们通常会请求用户进行某种类型的操作。

![对话框示例](../images/dialogs/dialog_RS2_delete_file.png)

**获取 Windows UI 库**

|  |  |
| - | - |
| ![WinUI 徽标](../images/winui-logo-64x64.png) | Windows UI 库 2.2 或更高版本包含此控件的使用圆角的新模板。 有关详细信息，请参阅[圆角半径](/windows/uwp/design/style/rounded-corner)。 WinUI 是一种 NuGet 包，其中包含用于 Windows 应用的新控件和 UI 功能。 有关详细信息（包括安装说明），请参阅 [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **平台 API：** [ContentDialog 类](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用对话框通知用户重要信息或在可以完成某个操作之前请求确认或其他信息。

有关何时使用对话框以及何时使用浮出控件（类似控件）的建议，请参阅[对话框和浮出控件](index.md)。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="../images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装了 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处打开该应用，了解 <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> 或 <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> 的实际应用。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="general-guidelines"></a>一般指南

-   在对话框的第一行文本中清楚地标识问题或用户的目标。
-   对话框标题是主要说明并且是可选的。
    -   使用简短标题说明用户需要怎样处理对话框。
    -   如果你使用对话框来传达简单的消息、错误或问题，则可以省略标题。 可依赖内容文本来传达这样的核心信息。
    -   确保标题与按钮选项直接相关。
-   对话框内容包含描述性文本，并且是必需的。
    -   提供尽可能简单的消息、错误或阻止问题。
    -   如果使用对话框标题，请使用内容区域提供更多详情或定义术语。 不要只是修改几个措词来重复标题。
-   必须至少显示一个对话框按钮。
    -   确保你的对话框至少有一个按钮对应于安全、没有破坏性的操作，如“明白了!”、“关闭”或“取消”。 使用 CloseButton API 可添加该按钮。
    -   使用主要说明或内容的特定响应作为按钮文本。 例如，“你是否希望允许 AppName 访问你的位置”，后跟“允许”和“拒绝”按钮。 具体的响应可以使用户更快速的理解，以便进行高效的决策。
    - 确保操作按钮的文本保持简明。 简短的字符串使用户能够快速、自信地做出选择。
    - 除了安全、无破坏性的操作外，你还可以选择为用户提供一个或两个与主要说明相关的操作按钮。 这些“执行”操作按钮用于确认对话框的重点。 使用 PrimaryButton 和 SecondaryButton API 可添加这些“执行”操作。
    - “执行”操作按钮应显示为最左侧按钮。 安全、无破坏性的操作应显示为最右侧的按钮。
    - 可以选择三个按钮之一作为对话框的默认按钮。 使用 DefaultButton API 可区分其中一个按钮。
-   不要为与页面上的特定位置具有上下文关系的错误（例如，密码字段等位置的验证错误）使用对话框，请使用应用的画布本身显示内联错误。
- 使用 [ContentDialog 类](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)生成对话框体验。 不要使用已弃用的 MessageDialog API。

## <a name="how-to-create-a-dialog"></a>如何创建对话框
若要创建对话框，你使用 [ContentDialog 类](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)。 你可以使用代码或标记创建对话框。 尽管使用 XAML 定义 UI 元素通常更容易，但对于简单对话框，实际上只使用代码更容易。 此示例创建一个对话框来通知用户没有 WiFi 连接，然后使用 [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法显示它。

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

当用户单击某个对话框按钮时，[ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法返回一个 [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) 来通知你用户单击了哪个按钮。

此示例中的对话框提出一个问题，并使用返回的 [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) 确定用户的响应。

```csharp
private async void DisplayDeleteFileDialog()
{
    ContentDialog deleteFileDialog = new ContentDialog
    {
        Title = "Delete file permanently?",
        Content = "If you delete this file, you won't be able to recover it. Do you want to delete it?",
        PrimaryButtonText = "Delete",
        CloseButtonText = "Cancel"
    };

    ContentDialogResult result = await deleteFileDialog.ShowAsync();

    // Delete the file if the user clicked the primary button.
    /// Otherwise, do nothing.
    if (result == ContentDialogResult.Primary)
    {
        // Delete the file.
    }
    else
    {
        // The user clicked the CLoseButton, pressed ESC, Gamepad B, or the system back button.
        // Do nothing.
    }
}
```

## <a name="provide-a-safe-action"></a>提供安全操作
由于对话框会阻止用户交互，而且按钮是用户消除对话框的主要机制，因此请确保你的对话框至少包含一个“安全”且无破坏性的按钮，如“关闭”或“明白!”。 **所有对话应都至少应包含一个安全操作按钮来关闭对话框。** 这可以确保用户能自信地关闭对话框，而未执行操作。<br>![一个按钮对话框](../images/dialogs/dialog_RS2_one_button.png)

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

当对话框用于显示阻止问题时，你的对话框应向用户显示与该问题相关的操作按钮。 这个“安全”且无破坏性的按钮可能会伴随一个或两个“执行”操作按钮。 在向用户提供多个选项时，请确保按钮可清晰地说明与所提出问题相关的“执行”和安全的“不执行”操作。

![两个按钮对话框](../images/dialogs/dialog_RS2_two_button.png)

```csharp
private async void DisplayLocationPromptDialog()
{
    ContentDialog locationPromptDialog = new ContentDialog
    {
        Title = "Allow AppName to access your location?",
        Content = "AppName uses this information to help you find places, connect with friends, and more.",
        CloseButtonText = "Block",
        PrimaryButtonText = "Allow"
    };

    ContentDialogResult result = await locationPromptDialog.ShowAsync();
}
```

向用户提供两个“执行”操作和一个“不执行”操作时，可以使用三个按钮对话框。 应慎用三个按钮对话框，辅助操作与安全/关闭操作之间应有清晰的区别。

![包含三个按钮的对话框](../images/dialogs/dialog_RS2_three_button.png)

```csharp
private async void DisplaySubscribeDialog()
{
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it"
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

## <a name="the-three-dialog-buttons"></a>三个对话框按钮
ContentDialog 有三种不同类型的按钮可用于构建对话框体验。

- **CloseButton** - 必需 - 表示允许用户退出对话框的安全、无破坏性操作。 显示为最右侧的按钮。
- **PrimaryButton** - 可选 - 表示第一个“执行”操作。 显示为最左侧的按钮。
- **SecondaryButton** - 可选 - 表示第二个“执行”操作。 显示为中间的按钮。

使用内置按钮可相应放置按钮、确保按钮可正确响应键盘事件、确保在屏幕键盘启动时命令区域仍保持可见并使该对话框与其他对话框看起来一致。

### <a name="closebutton"></a>CloseButton
每个对话框都应包含一个可使用户安心退出对话框的安全、无破坏性的操作按钮。

使用 ContentDialog.CloseButton API 可创建此按钮。 这样便可以为包括鼠标、键盘、触控和游戏板在内的所有输入创建适当的用户体验。 此体验将在以下情况下发生：
<ol>
    <li>用户单击或点击关闭按钮 </li>
    <li>用户按下系统后退按钮 </li>
    <li>用户按下键盘上的 ESC 按钮 </li>
    <li>用户按下游戏板 B </li>
</ol>

当用户单击某个对话框按钮时，[ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法返回一个 [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) 来通知你用户单击了哪个按钮。 按 CloseButton 返回 ContentDialogResult.None。

### <a name="primarybutton-and-secondarybutton"></a>PrimaryButton 和 SecondaryButton
除了 CloseButton，还可以选择向用户提供与主要说明相关的一个或两个操作按钮。
将 PrimaryButton 用于第一个“执行”操作，将 SecondaryButton 用于第二个“执行”操作。 在三个按钮的对话框中，PrimaryButton 通常表示肯定的“执行”操作，而 SecondaryButton 通常表示中性或辅助“执行”操作。
例如，应用可能会提示用户订阅服务。 作为肯定“执行”操作的 PrimaryButton 将托管“订阅”文本，而作为中性“执行”操作的 SecondaryButton 将托管“尝试”文本。 CloseButton 允许用户取消，而不执行任何操作。

当用户单击 PrimaryButton 时，[ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法将返回 ContentDialogResult.Primary。
当用户单击 SecondaryButton 时，[ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法将返回 ContentDialogResult.Secondary。

![包含三个按钮的对话框](../images/dialogs/dialog_RS2_three_button.png)

### <a name="defaultbutton"></a>DefaultButton
可以选择区分三个按钮之一，使其作为默认按钮。 指定默认按钮将导致发生以下情况：
- 该按钮接收“突出”按钮视觉处理
- 该按钮将自动响应 ENTER 键
    - 当用户在键盘上按 ENTER 键时，与“默认”按钮关联的单击处理程序将激发，并且 ContentDialogResult 将返回与“默认”按钮关联的值
    - 如果用户已将键盘焦点放在处理 ENTER 的控件上，默认按钮将不响应 ENTER 按下操作
- 打开对话框时，按钮会自动接收焦点，除非对话框内容包含可聚焦的 UI

使用 ContentDialog.DefaultButton 属性指示默认按钮。 默认情况下，不设置默认按钮。

![具有默认按钮的三个按钮对话框](../images/dialogs/dialog_RS2_three_button_default.png)

```csharp
private async void DisplaySubscribeDialog()
{
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it",
        DefaultButton = ContentDialogButton.Primary
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

## <a name="confirmation-dialogs-okcancel"></a>确认对话框（确定/取消）
通过确认对话框，用户可以确认他们是否要执行操作。 他们可以确认操作，也可以选择取消。
典型的确认对话框有两个按钮：一个确认（“确定”）按钮和一个取消按钮。

<ul>
    <li>
        <p>一般情况下，确认按钮（主要按钮）应位于左侧，取消按钮（辅助按钮）应位于右侧。</p>
        <img alt="An OK/cancel dialog" src="../images/dialogs/dialog_RS2_delete_file.png" />
    </li>
    <li>如一般建议部分中所述，使用带有文本的按钮，该文本可标识特定于主要说明或内容的响应。
    </li>
</ul>

> 某些平台将确认按钮放置在右侧，而不是左侧。 那么，为什么我们建议将它放在左侧呢？  如果你假设大多数用户惯用右手并且他们用右手拿着手机，按位于左侧的确认按钮实际上更为舒适，因为该按钮更有可能处于用户的拇指弧范围内。位于屏幕右侧的按钮需要用户将拇指向内收缩到不太舒适的位置。

## <a name="contentdialog-in-appwindow-or-xaml-islands"></a>AppWindow 或 Xaml 岛中的 ContentDialog

> 注意：本部分仅适用于面向 Windows 10 版本 1903 或更高版本的应用。 AppWindow 和 XAML 岛在早期的版本中不可用。 有关版本的详细信息，请参阅[版本自适应应用](../../../debug-test-perf/version-adaptive-apps.md)。

默认情况下，内容对话框相对于根 [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) 按模式显示。 使用 [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) 或 [XAML 岛](/windows/apps/desktop/modernize/xaml-islands)中的 ContentDialog 时，需要手动将对话框中的 [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) 设置为 XAML 宿主的根。

为此，请将 ContentDialog 的 XamlRoot 属性设置为 AppWindow 或 XAML 岛中已包含的某个元素的同一 XamlRoot，如下所示。

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    // Use this code to associate the dialog to the appropriate AppWindow by setting
    // the dialog's XamlRoot to the same XamlRoot as an element that is already present in the AppWindow.
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
    {
        noWifiDialog.XamlRoot = elementAlreadyInMyAppWindow.XamlRoot;
    }

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

> [!WARNING]
> 每次只能在每个线程中打开一个 ContentDialog。 尝试打开两个 ContentDialog 会引发异常，即使尝试在独立的 AppWindow 中打开。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章
- [工具提示](../tooltips.md)
- [菜单和上下文菜单](../menus.md)
- [Flyout 类](/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [ContentDialog 类](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)
