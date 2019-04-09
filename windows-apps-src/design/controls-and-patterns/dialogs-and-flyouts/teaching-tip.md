---
ms.openlocfilehash: 15379e51f8c272d0cc1e888684322104186bb200
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2019
ms.locfileid: "59249499"
---
# <a name="teaching-tip"></a>教学提示

教学提示是提供的上下文信息的半持久性和大量内容的弹出窗口。 它通常用于通知，提醒，并讲授有关可能会增强用户体验的重要和新功能的用户。

**重要的 Api:**[TeachingTip 类](https://docs.microsoft.com/en-us/uwp/api/microsoft.ui.xaml.controls.teachingtip)

教学提示可能是轻量接触或需要显式关闭的操作。 教学提示可以针对它尾巴的特定 UI 元素，也使用不带结尾或目标。

## <a name="is-this-the-right-control"></a>这是正确的控件吗？ 

使用**TeachingTip**控件可以专注于用户注意新的或重要更新和功能，提醒会提高他们的体验，或指导用户应如何完成任务的不必要的内容选项的用户。 

教学提示是暂时性的因为它不会提示用户有关的错误或重要状态更改的建议的控件。


## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果有<strong style="font-weight: semi-bold">XAML 控件库</strong>应用程序安装，请单击此处<a href="xamlcontrolsgallery:/item/TeachingTip">打开应用，请参阅操作中的 TeachingTip</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

教学提示可以具有多个配置，包括这些值得注意的。

教学提示可以为目标来增强它所呈现的信息的上下文清晰度它尾巴的特定 UI 元素。 

![包含以保存为目标的教学提示的示例应用程序按钮。 提示标题将为"自动保存"和子标题读取"我们保存所做的更改意愿-因此永远不会有对"。 在右上角的教学提示上没有关闭按钮。](../images/teaching-tip-targeted.png)

提供的信息与特定的 UI 元素不相关，即可通过删除尾部创建 nontargeted 教学提示。

![使用右下角中的教学提示的示例应用。 提示标题将为"自动保存"和子标题读取"我们保存所做的更改意愿-因此永远不会有对"。 在右上角的教学提示上没有关闭按钮。](../images/teaching-tip-non-targeted.png)

教学提示可要求用户可关闭该消息通过左上角中的"X"按钮或底部的"关闭"按钮。 教学提示可能也是轻量解除这种情况下启用是没有取消按钮，当用户滚动或与应用程序的其他元素进行交互，而是会消除教学提示。 由于此行为，轻量解除提示都是最佳解决方案，当提示需要放在可滚动区域。 

![示例应用使用轻量解除右下角中的教学提示。 提示标题将为"自动保存"和子标题读取"我们保存所做的更改意愿-因此永远不会有对"。](../images/teaching-tip-light-dismiss.png)


### <a name="create-a-teaching-tip"></a>创建教学提示

下面是为目标的教学 XAML 提示演示使用的标题和副标题 TeachingTip 的默认外观的控件。 请注意，教学提示可以出现在任何位置的元素树或代码隐藏。 在此示例中，它位于的 ResourceDictionary 中。

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Save automatically"
            Subtitle="When you save your file to OneDrive, we save your changes as you go - so you never have to.">
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

C#
```C#
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

显示页面包含按钮和提供相关指导的提示时，下面是结果：

![包含以保存为目标的教学提示的示例应用程序按钮。 提示标题将为"自动保存"和子标题读取"我们保存所做的更改意愿-因此永远不会有对"。 在右上角的教学提示上没有关闭按钮。](../images/teaching-tip-targeted.png)

### <a name="non-targeted-tips"></a>针对非的提示

并非所有的提示与屏幕上的元素。 这些情况下，未设置目标属性和教学提示将改为显示相对于 xaml 根的边缘。 但是，教学提示可以删除并保留相对于 UI 元素的位置将 TailVisibility 属性设置为"折叠"结尾。 下面的示例是针对非的教学提示。

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to.">
</controls:TeachingTip>
```

请注意，在此示例中 TeachingTip 是元素树中而不是在 ResourceDictionary 中或在代码隐藏中。 此属性的行为; 上无效TeachingTip 仅显示时打开，并将占用布局空间。

![使用右下角中的教学提示的示例应用。 提示标题将为"自动保存"和子标题读取"我们保存所做的更改意愿-因此永远不会有对"。 在右上角的教学提示上没有关闭按钮。](../images/teaching-tip-non-targeted.png)

### <a name="preferred-placement"></a>首选的位置

教学提示复制浮出控件的[FlyoutPlacementMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode) TeachingTipPlacementMode 属性放置行为。 默认位置模式会尝试将超出其目标值的目标的教学提示和非目标教学小费底部的 xaml 根居中放置。 作为与浮出控件，如果首选的位置模式会不留出空间的教学提示若要显示，另一个位置模式将被自动选择。 

对于预测游戏板输入的应用程序，请参阅[游戏手柄和远程控制交互]( https://docs.microsoft.com/en-us/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction)。 建议使用应用程序的 UI 的所有可能的配置每个教学提示的游戏板辅助功能进行测试。

具有设置为"BottomLeft"其 PreferredPlacement 的目标的教学提示将出现尾部的教学提示正文转向左侧居中显示在其目标的底部。

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            PreferredPlacement="BottomLeft">
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![使用为目标的教学提示下其左上角的"保存"按钮的示例应用。 提示标题将为"自动保存"和子标题读取"我们保存所做的更改意愿-因此永远不会有对"。 在右上角的教学提示上没有关闭按钮。](../images/teaching-tip-targeted-preferred-placement.png)


中的 xaml 根左下角会显示其设置为"BottomLeft"PreferredPlacement 与非目标教学提示。

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft">
</controls:TeachingTip>
```

![左下角中的教学提示包含的示例应用。 提示标题将为"自动保存"和子标题读取"我们保存所做的更改意愿-因此永远不会有对"。 在右上角的教学提示上没有关闭按钮。](../images/teaching-tip-non-targeted-preferred-placement.png)

下图描绘了可以在目标教学提示为设置的所有 13 PreferredPlacement 模式的结果。
![三个对象标记为"目标"与目标教学提示围绕它们用于显示教学提示的目标的首选的位置模式。 中心的第一个目标是标记为"Center"向下指向它尾巴的目标的目标的教学提示。 上面的第一个目标中间位置是标记为"Top"指向下在其结尾的目标的目标的教学提示。 居中的第一个目标是标记为"Right"左指向它尾巴的目标的目标的教学提示。 下面的第一个目标的中间位置是标记为"Bottom"指向它尾巴的目标的目标的教学提示。 居中左侧的第一个目标是标记为"左"指坐在它尾巴的目标的目标的教学提示。 左侧第二个目标的教学提示标记为"LeftTop"，指向右目标并具有其正文迁移向上。 上面第二个目标教学提示标记为"、 左边框"向下指向目标并具有其正文迁移 leftward。 右侧的第二个目标是标记为"RightBottom"点保留在目标并具有其正文迁移下一个教学提示。 在第二个目标下方教学提示标记为"BottomRight"指向目标并具有其正文迁移向右。 上面第三个目标教学提示标有"位于"的向下指在目标处，具有其主体移向右。 右侧的第三个目标是标记为"RightTop"点保留在目标并具有其正文迁移向上的教学提示。 下面的第三个目标教学提示标记为"BottomLeft"指向目标并具有其正文迁移 leftward。 左侧的第三个目标教学提示标有"LeftBottom"在目标处的右，具有其主体移向下拖动。](../images/teaching-tip-targeted-preferred-placement-modes.png)

下图描绘了可以为非目标教学提示设置的所有 13 PreferredPlacement 模式的结果。
![应用窗口显示九个非目标教学提示来演示教学提示非目标的首选的位置模式。 在应用的左上角的教学提示标记为"、 左边框或 LeftTop。" 中心应用的上边缘的教学提示标记为"Top"。 在应用程序的右上角的教学提示标记为"TopRight 或 RightTop。" 中心应用的左边缘的教学提示标记为"Left"。 应用中心的教学提示标记为"中心"。
中心应用的右边缘的教学提示标记为"权限。" 在应用程序的左下角的教学提示标记为"BottomLeft 或 LeftBottom。" 中心应用的下边缘的教学提示标记为"底部"。 在应用程序右下角的教学提示标记为"BottomRight 或 RightBottom。"](../images/teaching-tip-non-targeted-preferred-placement-modes.png)

### <a name="add-a-placement-margin"></a>添加放置边距  

可以控制多久除了其目标设置目标的教学提示以及通过使用 PlacementMargin 属性的 xaml 根边缘除了设置非目标教学提示延伸的范围。 像[边距](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.margin)，PlacementMargin 具有四个值 – 左、 右前，和 bottom – 仅使用相关值。 例如，PlacementMargin.Left 适用的目标，或者在左边缘的 xaml 根提示保留时。

下面的示例演示具有 PlacementMargin 的左/上/Right/下的非目标提示都设置为 80。

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft"
    PlacementMargin="80">
</controls:TeachingTip>
```

![使用定位，但不是完全针对，右下角的教学提示的示例应用。 提示标题将为"自动保存"和子标题读取"我们保存所做的更改意愿-因此永远不会有对"。 在右上角的教学提示上没有关闭按钮。](../images/teaching-tip-placement-margin.png)


### <a name="add-content"></a>添加内容

内容可以添加到使用 Content 属性的教学提示。 如果没有要显示比教学提示的大小将允许的其他内容，一个滚动条将自动启用以允许用户滚动内容区域。 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![包含以保存为目标的教学提示的示例应用程序按钮。 提示标题将为"自动保存"和子标题读取"我们保存所做的更改意愿-因此永远不会有对"。 在教学提示的内容区域中一个复选框标记为"不显示提示在启动时"，其下方是"是否改变主意可以更改在设置提示首选项"中读取的文本"的设置"的应用程序设置页面的链接。 在右上角的教学提示上没有关闭按钮。](../images/teaching-tip-content.png)

### <a name="add-buttons"></a>添加按钮

默认情况下，"X"关闭按钮的标准教学提示的标题旁边显示。 使用 CloseButtonContent 属性，用例按钮移到教学提示的底部，可以自定义关闭按钮。

**注意：没有关闭按钮将出现在轻量解除已启用的提示**

可以通过设置 ActionButtonContent 属性 （并根据需要 ActionButtonCommand 和 ActionButtonCommandParameter 属性） 添加自定义操作按钮。

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources> 
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            ActionButtonContent="Disable"
            ActionButtonCommand="DisableAutoSave"
            CloseButtonContent="Got it!">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![包含以保存为目标的教学提示的示例应用程序按钮。 提示标题将为"自动保存"和子标题读取"我们保存所做的更改意愿-因此永远不会有对"。 在教学提示的内容区域中一个复选框标记为"不显示提示在启动时"，其下方是"是否改变主意可以更改在设置提示首选项"中读取的文本"的设置"的应用程序设置页面的链接。 在教学的底部是两个按钮、 一个读取"禁用"左侧的灰色，蓝色一个读取右侧"了 ！"](../images/teaching-tip-buttons.png)

### <a name="hero-content"></a>Hero 内容

边缘到边缘的内容可以设置 HeroContent 属性添加到教学提示。 Hero 内容的位置可以通过设置 HeroContentPlacement 属性设置为顶部或底部的教学提示。

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources> 
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
            <controls:TeachingTip.HeroContent>
                <Image Source="Assets/cloud.png" />
            </controls:TeachingTip.HeroContent>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![包含以保存为目标的教学提示的示例应用程序按钮。 提示标题将为"自动保存"和子标题读取"我们保存所做的更改意愿-因此永远不会有对"。 底部的教学提示是将文件放在云中卡通 man 边框边框图像。 在右上角的教学提示上没有关闭按钮。](../images/teaching-tip-hero-content.png)

### <a name="add-an-icon"></a>添加图标

标题和副标题使用 IconSource 属性旁边，可以添加一个图标。 建议的图标大小包括 16px、 24px 和 32 px。 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            <controls:TeachingTip.IconSource>
                <controls:SymbolIconSource Symbol="Save" />
            </controls:TeachingTip.IconSource>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![包含以保存为目标的教学提示的示例应用程序按钮。 提示标题将为"自动保存"和子标题读取"我们保存所做的更改意愿-因此永远不会有对"。 左侧的标题和副标题是软盘图标。 在右上角的教学提示上没有关闭按钮。](../images/teaching-tip-icon.png)

### <a name="enable-light-dismiss"></a>启用轻量解除

轻量解除功能默认处于禁用状态，但它可以启用，以便消除将教学提示，例如，当用户滚动或与应用程序的其他元素进行交互。 由于此行为，轻量解除提示都是最佳解决方案，当提示需要放在可滚动区域。 

关闭按钮将自动删除的轻量接触启用的教学提示来标识从其轻量解除对用户的行为。 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    IsLightDismissEnabled="True">
</controls:TeachingTip>
```

![示例应用使用轻量解除右下角中的教学提示。 提示标题将为"自动保存"和子标题读取"我们保存所做的更改意愿-因此永远不会有对"。](../images/teaching-tip-light-dismiss.png)

### <a name="escaping-the-xaml-root-bounds"></a>转义的 xaml 根边界

在 Windows 上版本 19 H 1 和更高版本，教学提示可通过设置 ShouldConstrainToRootBounds 属性转义 xaml 根和屏幕的边界。 启用此属性后，将不尝试教学提示可在 xaml 根，也不在屏幕的边界内保持状态，始终会在一组位置 PreferredPlacement 模式。 建议启用 IsLightDismissEnabled 属性并设置 PreferredPlacement 模式最接近的 xaml 根，以确保用户的最佳体验中心。

在早期版本的 Windows 中，将忽略此属性和教学提示始终位于 xaml 根的边界内。

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomRight"
    PlacementMargin="-80,-50,0,0"
    ShouldConstrainToRootBounds="False">
</controls:TeachingTip>
```

![使用外部应用程序的右下角的教学提示的示例应用。 提示标题将为"自动保存"和子标题读取"我们保存所做的更改意愿-因此永远不会有对"。 在右上角的教学提示上没有关闭按钮。](../images/teaching-tip-escape-xaml-root.png)

### <a name="canceling-and-deferring-close"></a>取消和关闭延迟

Closing 事件可用于取消和/或延迟的教学提示关闭。 这可以用于保持教学提示打开或留出时间进行的操作或要发生的自定义动画。 取消教学提示关闭时，IsOpen 将返回为 true，但是，它将保持 false 在延迟期间。 此外可以取消以编程方式关闭。 

**注意：如果没有放置选项将允许完全显示的教学技巧，教学提示将遍历其事件生命周期，以强制关闭而不显示而无需访问关闭按钮。 如果应用程序取消 Closing 事件，则教学提示可能会保持打开状态而无需访问关闭按钮。**

XAML
```XAML
<controls:TeachingTip x:Name="EnableNewSettingsTip"
    Title="New ways to protect your privacy!"
    Subtitle="Please close this tip and review our updated privacy policy and privacy settings."
    Closing="OnTipClosing">
</controls:TeachingTip>
```

C#
```C#
public void OnTipClosing(object sender, TeachingTipClosingEventArgs args)
{
    if (args.Reason == TeachingTipCloseReason.CloseButton)
    {
        using(args.GetDeferral())
        {
            bool success = await UpdateUserSettings(User thisUsersID);
            if(!success)
            {
                //We were not able to update the settings! Don't close the tip and display the reason why.
                args.Cancel = true;
                ShowLastErrorMessage();
            }
        }
    }
}
```

## <a name="remarks"></a>备注

### <a name="related-articles"></a>相关文章 

* [对话框和浮出控件](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/index)

### <a name="recommendations"></a>建议
* 提示是 impermanent 和不应包含的信息或对应用程序的体验至关重要的选项。 
* 尽量避免过于频繁显示教学的提示。 教学提示都是最有可能给每个接收单个关注交错整个长时间的会话或跨多个会话时。    
* 保持简洁的提示和清除其主题。 研究表明用户，一般情况下，仅读取 3-5 个单词和仅在确定是否进行交互的提示信息之前理解 2-3 个单词。
* 游戏手柄的教学提示的可访问性不能保证。 对于预测游戏板输入的应用程序，请参阅[游戏手柄和远程控制交互]( https://docs.microsoft.com/en-us/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction)。 建议使用应用程序的 UI 的所有可能的配置每个教学提示的游戏板辅助功能进行测试。
* 在启用要转义的 xaml 根教学提示时，它被建议还启用 IsLightDismissEnabled 属性并将 PreferredPlacement 模式设置最接近的 xaml 根的中心。 

### <a name="reconfiguring-an-open-teaching-tip"></a>重新打开教学提示配置

可以重新配置某些内容和属性，而教学提示已打开，并且将立即生效。 其他内容和属性，如图标属性、 操作并关闭按钮和重新配置之间轻量接触和显式消除所有需要的教学提示将关闭并重新打开对这些属性，以生效的更改。 请注意，更改从上诉行为手动解除要轻量解除教学提示处于打开状态时将导致要拥有其关闭按钮删除之前的教学提示轻量解除行为，并提示可保持卡滞屏幕上。
