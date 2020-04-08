---
Description: 教学提示是一个提供上下文信息的半持久型浮出控件，其中可显示丰富的内容。
title: 教学提示
template: detail.hbs
ms.date: 04/01/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
ms.custom: 19H1
ms.localizationpriority: medium
ms.openlocfilehash: 06734c854f0097db5fa96e35d4123dde8bda8a95
ms.sourcegitcommit: 8be8ed1ef4e496055193924cd8cea2038d2b1525
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2020
ms.locfileid: "80614142"
---
# <a name="teaching-tip"></a>教学提示

教学提示是一个提供上下文信息的半持久型浮出控件，其中可显示丰富的内容。 它通常用于通知、提醒以及教授用户可改进用户体验的重要功能和新功能。

教学提示可能是轻型消除提示，或需要显式操作才能关闭。 教学提示可通过其尾部指明作为其目标的特定 UI 元素，也可在没有尾部或目标的情况下使用。

**获取 Windows UI 库**

|  |  |
| - | - |
| ![WinUI 徽标](../images/winui-logo-64x64.png) | **TeachingTip** 控件需要 Windows UI 库，该库是一个 Nuget 包，包含 UWP 应用的新控件和 UI 功能。 有关详细信息（包括安装说明），请参阅 [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **Windows UI 库 API：** [TeachingTip 类](/uwp/api/microsoft.ui.xaml.controls.teachingtip)

> [!TIP]
> 在本文档中，我们使用 XAML 中的 **muxc** 别名表示我们已包含在项目中的 Windows UI 库 API。 我们已将此项添加到我们的[页](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page)元素：`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>在后面的代码中，我们还使用 C# 中的 **muxc** 别名表示我们已包含在项目中的 Windows UI 库 API。 我们在文件顶部添加了此 **using** 语句：`using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

 使用 TeachingTip 控件，将用户注意力集中到新的或重要的更新和功能上，提醒可改进其体验的非必要选项，或者指导用户应如何完成任务。

由于教学提示是暂时性的，因此它不会成为提示用户错误或重要状态更改的推荐控件。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="../images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 XAML 控件库<strong style="font-weight: semi-bold"></strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/TeachingTip">打开应用并查看教学提示的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

教学提示可采用多种配置，包括显眼的配置。

教学提示可通过其尾部指明作为其目标的特定 UI 元素，以更清楚地表明其所呈现信息是属于哪个元素的上下文。

![目标为“保存”按钮的教学提示的示例应用。 该提示的标题为“自动保存”，副标题为“我们将即时保存你的所有更改 - 你无需亲自动手。” 教学提示的右上角有一个关闭按钮。](../images/teaching-tip-targeted.png)

提供的信息与特定 UI 元素不相关时，可通过删除尾部创建非定向教学提示。

![教学提示位于右下角的示例应用。 该提示的标题为“自动保存”，副标题为“我们将即时保存你的所有更改 - 你无需亲自动手。” 教学提示的右上角有一个关闭按钮。](../images/teaching-tip-non-targeted.png)

教学提示可能要求用户通过位于顶部的“X”按钮或位于底部的“关闭”按钮进行关闭。 没有关闭按钮时，教育提示可启用轻型消失，当用户滚动到其他位置或与应用程序的其他元素进行交互时，教学提示将会消失。 由于这种行为，当需要将提示放在可滚动区域中时，轻型消除提示是最佳解决方案。

![轻型消除教学提示位于右下角的示例应用。 该提示的标题为“自动保存”，副标题为“我们将即时保存你的所有更改 - 你无需亲自动手。”](../images/teaching-tip-light-dismiss.png)

### <a name="create-a-teaching-tip"></a>创建教学提示

下面是定向教学提示控件的 XAML，演示了带有标题和副标题的教学提示的默认外观。
请注意，教学提示可以出现在元素树或隐藏代码中的任何位置。 在以下示例中，它位于 ResourceDictionary 中。

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Save automatically"
            Subtitle="When you save your file to OneDrive, we save your changes as you go - so you never have to.">
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    if(!HaveExplainedAutoSave())
    {
        AutoSaveTip.IsOpen = true;
        SetHaveExplainedAutoSave();
    }
}
```

下面显示了包含按钮和教学提示的页面：

![目标为“保存”按钮的教学提示的示例应用。 该提示的标题为“自动保存”，副标题为“我们将即时保存你的所有更改 - 你无需亲自动手。” 教学提示的右上角有一个关闭按钮。](../images/teaching-tip-targeted.png)

### <a name="non-targeted-tips"></a>非定向提示

并非所有提示都与屏幕上的某个元素关联。 在这些情况下，请不要设置 Target 属性，教学提示将显示在 xaml 根的边缘附近。 但是，通过将 TailVisibility 属性设置为“折叠”，可让删除了尾部的教学提示仍置于某个 UI 元素附近。 以下是非定向教学提示的示例。

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to.">
</muxc:TeachingTip>
```

请注意，在此示例中，教学提示位于元素树中而不是 ResourceDictionary 或隐藏代码中。 这对行为没有影响，教学提示仅在打开时显示，并且不会占用布局空间。

![教学提示位于右下角的示例应用。 该提示的标题为“自动保存”，副标题为“我们将即时保存你的所有更改 - 你无需亲自动手。” 教学提示的右上角有一个关闭按钮。](../images/teaching-tip-non-targeted.png)

### <a name="preferred-placement"></a>首选位置

教学提示通过 TeachingTipPlacementMode 属性复制浮出控件的 [FlyoutPlacementMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode) 放置行为。 默认放置模式尝试将定向教学提示放置在其目标上方，将非定向教学提示置于 xaml 根底部的中心位置。 与浮出控件相同，如果首选放置模式无法留出显示教学提示的空间，将自动选择另一种放置模式。

对于预测游戏板输入的应用程序，请参阅[游戏板和远程控制交互]( https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction)。 建议使用一个应用 UI 所有可能的配置对每个教学提示的游戏板辅助功能进行测试。

对于 PreferredPlacement 设置“BottomLeft”的定向教学提示，其尾部位于目标底部的中心位置，其正文位于左侧。

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            PreferredPlacement="BottomLeft">
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![一个示例应用，其中的教学提示位于其目标（“保存”按钮）的左下方。 该提示的标题为“自动保存”，副标题为“我们将即时保存你的所有更改 - 你无需亲自动手。” 教学提示的右上角有一个关闭按钮。](../images/teaching-tip-targeted-preferred-placement.png)

将 PreferredPlacement 设置为“BottomLeft”的非定向教学提示将显示在 xaml 根的左下角。

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft">
</muxc:TeachingTip>
```

![教学提示位于左下角的示例应用。 该提示的标题为“自动保存”，副标题为“我们将即时保存你的所有更改 - 你无需亲自动手。” 教学提示的右上角有一个关闭按钮。](../images/teaching-tip-non-targeted-preferred-placement.png)

下图描绘了所有 13 个可为定向教学提示设置的 PreferredPlacement 模式的结果。
![包含 13 个教学提示的插图，每个教学提示都演示了不同的定向放置模式。 每个教学提示都通过标签表明了其所代表的模式。 放置模式的第一个单词指示教学提示将居中显示在其目标哪一侧。 教学提示的尾部始终位于其目标一侧的中心位置并指向该目标。 如果放置模式中有第二个单词，则教学提示的正文不会居中，而是移向指定方向。 例如，放置模式为“TopRight”时，教学提示显示在其目标上方并移向右侧，其尾部向下指向该目标上边缘的中心位置。 由于正文移向右侧，其尾部几乎位于教学提示正文最左侧的边缘上，且该教学提示超出了其目标的右边缘。 放置模式为“Center”可确定一个独特的位置，使教学提示的尾部指向目标的中心，正文位于其目标上半部分的居中位置。](../images/teaching-tip-targeted-preferred-placement-modes.png)

下图描绘了所有 13 个可为非定向教学提示设置的 PreferredPlacement 模式的结果。
![包含 9 个教学提示的插图，每个教学提示都演示了不同的非定向放置模式。 每个教学提示都通过标签表明了其所代表的模式。 放置模式的第一个单词指示教学提示将居中显示在 xaml 根的哪一侧。 如果放置模式中有第二个单词，则教学提示将位于 xaml 根的指定角落。 例如，放置模式为“TopRight”时，教学提示显示在 xaml 根的右上角。 对于非定向放置模式，两个单词的顺序不会影响其位置。 TopRight 等效于 RightTop。 放置模式为“Center”可确定一个独特的位置，使该教学提示显示在 xaml 根的垂直和水平中心位置。](../images/teaching-tip-non-targeted-preferred-placement-modes.png)

### <a name="add-a-placement-margin"></a>添加位置边距

通过使用 PlacementMargin 属性，可以控制定向教学提示与其目标的距离以及非定向教学提示与 xaml 根边缘的距离。 与[边距](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin)一样，PlacementMargin 具有四个值（左、右、上和下），因此仅使用相关值。 例如，当提示位于目标左侧或 xaml 根的左边缘时，PlacementMargin.Left 适用。

以下示例显示 PlacementMargin 的 Left/Top/Right/Bottom 均设置为 80 的非定向提示。

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft"
    PlacementMargin="80">
</muxc:TeachingTip>
```

![教学提示靠近但未完全紧靠右下角的示例应用。 该提示的标题为“自动保存”，副标题为“我们将即时保存你的所有更改 - 你无需亲自动手。” 教学提示的右上角有一个关闭按钮。](../images/teaching-tip-placement-margin.png)


### <a name="add-content"></a>添加内容

可以使用 Content 属性向教学提示添加内容。 如果要显示的内容超出了教学提示允许的大小，则将自动启用滚动条，以便用户滚动内容区域。

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![目标为“保存”按钮的教学提示的示例应用。 该提示的标题为“自动保存”，副标题为“我们将即时保存你的所有更改 - 你无需亲自动手。” 在教学提示的内容区域中，有一个“启动时不显示提示”复选框，其下方的文本为“如果改变主意，可在设置中更改提示首选位置”，其中的“设置”是指向应用设置页面的链接。 教学提示的右上角有一个关闭按钮。](../images/teaching-tip-content.png)

### <a name="add-buttons"></a>添加按钮

默认情况下，标准的“X”关闭按钮显示在教学提示的标题旁边。 可使用 CloseButtonContent 属性自定义关闭按钮，此时，按钮移到教学提示的底部。

**注意：已启用轻型消除的提示上不会出现关闭按钮**

可通过设置 ActionButtonContent 属性（以及可选的 ActionButtonCommand 和 ActionButtonCommandParameter 属性）来添加自定义操作按钮。

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            ActionButtonContent="Disable"
            ActionButtonCommand="{x:Bind DisableAutoSaveCommand}"
            CloseButtonContent="Got it!">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![目标为“保存”按钮的教学提示的示例应用。 该提示的标题为“自动保存”，副标题为“我们将即时保存你的所有更改 - 你无需亲自动手。” 在教学提示的内容区域中，有一个“启动时不显示提示”复选框，其下方的文本为“如果改变主意，可在设置中更改提示首选位置”，其中的“设置”是指向应用设置页面的链接。 在教学提示的底部有两个按钮，左侧的灰色按钮为“禁用”，右侧的蓝色按钮为“明白了！”](../images/teaching-tip-buttons.png)

### <a name="hero-content"></a>主图内容

可通过设置 HeroContent 属性将无边框内容添加到教学提示。 可通过设置 HeroContentPlacement 属性将主图内容的位置设置为教学提示的顶部或底部。

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
            <muxc:TeachingTip.HeroContent>
                <Image Source="Assets/cloud.png" />
            </muxc:TeachingTip.HeroContent>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![目标为“保存”按钮的教学提示的示例应用。 该提示的标题为“自动保存”，副标题为“我们将即时保存你的所有更改 - 你无需亲自动手。” 教学提示的底部是一张无边框的图像，其内容是卡通人物将文件放入云中。 教学提示的右上角有一个关闭按钮。](../images/teaching-tip-hero-content.png)

### <a name="add-an-icon"></a>添加图标

可使用 IconSource 属性在标题和副标题的旁边添加一个图标。 建议图标大小为 16px、24px 和 32px。

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            <muxc:TeachingTip.IconSource>
                <muxc:SymbolIconSource Symbol="Save" />
            </muxc:TeachingTip.IconSource>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![目标为“保存”按钮的教学提示的示例应用。 该提示的标题为“自动保存”，副标题为“我们将即时保存你的所有更改 - 你无需亲自动手。” 标题和副标题的左侧是软盘图标。 教学提示的右上角有一个关闭按钮。](../images/teaching-tip-icon.png)

### <a name="enable-light-dismiss"></a>启用轻型消除

默认情况下，轻型消除功能处于禁用状态，但可以启用该功能来关闭教学提示，例如，在用户滚动到其他位置或与应用程序的其他元素进行交互时关闭。 由于这种行为，当需要将提示放在可滚动区域中时，轻型消除提示是最佳解决方案。

关闭按钮将从已启用轻型消除的教学提示上自动删除，以便向用户展现其轻型消除行为。

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    IsLightDismissEnabled="True">
</muxc:TeachingTip>
```

![轻型消除教学提示位于右下角的示例应用。 该提示的标题为“自动保存”，副标题为“我们将即时保存你的所有更改 - 你无需亲自动手。”](../images/teaching-tip-light-dismiss.png)

### <a name="escaping-the-xaml-root-bounds"></a>转义 XAML 根边界

从 Windows 10 版本 1903（内部版本 18362）开始，教学提示可通过设置 `ShouldConstrainToRootBounds` 属性转义 XAML 根和屏幕的边界。 启用此属性后，教学提示不会尝试停留在 XAML 根或屏幕的边界内，而会始终位于 `PreferredPlacement` 模式设置的位置。 建议启用 `IsLightDismissEnabled` 属性并设置与 XAML 根中心位置最接近的 `PreferredPlacement` 模式，确保用户获得最佳体验。

Windows 的早期版本中忽略了此属性，教学提示始终位于 XAML 根的边界内。

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomRight"
    PlacementMargin="-80,-50,0,0"
    ShouldConstrainToRootBounds="False">
</muxc:TeachingTip>
```

![教学提示位于应用右下角外部的示例应用。 该提示的标题为“自动保存”，副标题为“我们将即时保存你的所有更改 - 你无需亲自动手。” 教学提示的右上角有一个关闭按钮。](../images/teaching-tip-escape-xaml-root.png)

### <a name="canceling-and-deferring-close"></a>取消和延迟关闭

可使用 Closing 事件来取消和/或延迟关闭教学提示。 可使用该事件使教学提示保持打开状态，或为操作或自定义动画留出时间。 取消关闭教学提示后，IsOpen 返回到 true 状态，但在延迟期间它将保持 false 状态。 此外，还可取消编程关闭。

> [!NOTE]
> 如果无法通过任何位置选项来完整显示教学提示，教学提示将通过遍历其事件生命周期来强制关闭而不显示，无需可访问的关闭按钮。 如果应用取消 Closing 事件，则教学提示可能因没有可访问的关闭按钮而保持打开状态。

```xaml
<muxc:TeachingTip x:Name="EnableNewSettingsTip"
    Title="New ways to protect your privacy!"
    Subtitle="Please close this tip and review our updated privacy policy and privacy settings."
    Closing="OnTipClosing">
</muxc:TeachingTip>
```

```csharp
private void OnTipClosing(muxc.TeachingTip sender, muxc.TeachingTipClosingEventArgs args)
{
    if (args.Reason == muxc.TeachingTipCloseReason.CloseButton)
    {
        using(args.GetDeferral())
        {
            bool success = UpdateUserSettings(User thisUsersID);
            if(!success)
            {
                // We were not able to update the settings!
                // Don't close the tip and display the reason why.
                args.Cancel = true;
                ShowLastErrorMessage();
            }
        }
    }
}
```

## <a name="recommendations"></a>建议

* 提示应是临时性的，且不应包含对应用程序体验至关重要的信息或选项。
* 尽量避免过于频繁地显示教学提示。 教学提示在长会话或多个会话中交错出现时，每次都可能受到单独关注。
* 保持提示简洁、主题清晰。 研究表明，用户在确定是否与提示进行交互之前平均仅阅读 3-5 个单词且仅理解 2-3 个单词。
* 无法保证教学提示的游戏板辅助功能。 对于预测游戏板输入的应用程序，请参阅[游戏板和远程控制交互]( https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction)。 建议使用一个应用 UI 所有可能的配置对每个教学提示的游戏板辅助功能进行测试。
* 在使用教学提示转义 xaml 根时，建议同时启用 IsLightDismissEnabled 属性并设置与 xaml 根中心位置最接近的 PreferredPlacement 模式。

## <a name="reconfiguring-an-open-teaching-tip"></a>重新配置一个打开的教学提示

教学提示处于打开状态时，可重新配置某些内容和属性，它们可立即生效。 其他内容和属性（如图标属性、操作和关闭按钮以及在轻型消除和显式消除之间进行重新配置）都需要先关闭教学提示再重新打开，才可使对这些属性进行的更改生效。 请注意，教学提示处于打开状态时，将消除行为从手动消除更改为轻型消除将导致教学提示在启用轻型消除行为之前删除其关闭按钮，这可能使该提示在屏幕上保持卡滞状态。

## <a name="related-articles"></a>相关文章

* [对话框和浮出控件](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/index)