---
Description: Incorporate speech into your apps using Cortana voice commands, speech recognition, and speech synthesis.
title: Surface Dial 交互
label: Surface Dial interactions
template: detail.hbs
keywords: Surface Dial, Windows 滚轮, RadialController, 射线控制器, 用户交互, 输入
ms.date: 02/08/2017
ms.topic: article
ms.assetid: e7deb1d6-feeb-471e-9a83-26386d1aaf37
ms.localizationpriority: medium
ms.openlocfilehash: bcdb8ca6843d126bc245e48f0b50209890740819
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "8338689"
---
# <a name="surface-dial-interactions"></a>Surface Dial 交互

![适配 Surface Studio 的 Surface Dial 的图像](images/windows-wheel/dial-pen-studio-600px.png)  
*适配 Surface Studio 和触控笔的 Surface Dial*（可通过 [Microsoft 官方商城](https://aka.ms/purchasesurfacedial)购买）。

## <a name="overview"></a>概述

Surface Dial 之类的 Windows Wheel 设备是一类全新的输入设备，使主机能为 Windows 和 Windows 应用提供引人入胜和独特的用户交互体验。 

> [!IMPORTANT]
> 在本主题中，我们重点介绍了 Surface Dial 交互，但该信息也同样适用于所有 Windows Wheel 设备。 

| 视频 |   |
| --- | --- |
| <iframe src="https://www.youtube-nocookie.com/embed/WMklcdzcNcU" width="300" height="200" allowFullScreen="true" frameBorder="0"></iframe> | <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="300" height="200" allowFullScreen="true" frameBorder="0"></iframe> |
| *Surface Dial 应用合作伙伴* | *适用于开发人员的 Surface Dial* |

借助基于*旋转*操作（或手势）的外形规格，Surface Dial 旨在成为对主设备输入进行补充的多模态辅助输入设备。 在大多数情况下，用户使用其惯用手执行某个任务（如使用笔进行墨迹书写）期间，该设备由其非惯用手操控。 它不是专为精确指针输入（如触摸、笔或鼠标）而设计。 

此外，Surface Dial 同时支持*长按*操作和*单击*操作。 长按只有一个功能：显示命令的菜单。 如果菜单处于活动状态，则由该菜单处理旋转和单击输入。 否则，该输入会传递给你的应用进行处理。 

**与所有 Windows 输入设备一样，可以自定义和定制适合你的应用中功能的 Surface Dial 交互体验。**

> [!TIP]
> Surface Dial 和全新的 Surface Studio 搭配使用可提供更为独特的用户体验。  
>
>除了所述的默认长按菜单体验之外，还可以将 Surface Dial 直接置于 Surface Studio 的屏幕上。 这会启用一个特殊的“屏幕”菜单。 
>
>系统通过检测 Surface Dial 的接触位置和边界收集的信息，来处理设备的遮挡，并环绕 Surface Dial 的外侧显示更大号的菜单。 应用还可以使用这一相同信息针对设备的存在及其预期用途（例如用户的手和臂的放置）排布 UI。

| Surface Dial 离屏菜单 | | Surface Dial 屏幕菜单 |
| --- | --- | --- |
| ![Surface Dial 离屏菜单](images/windows-wheel/surface-dial-menu-offscreen.png) | | ![Surface Dial 屏幕菜单](images/windows-wheel/surface-dial-menu-onscreen.png) |

## <a name="system-integration"></a>系统集成

Surface Dial 与 Windows 紧密集成，并且支持菜单上的一组内置工具：系统音量、滚动、放大/缩小和撤消/重做。

该内置工具集合适用于系统的当前上下文，其中包括：
- 用户位于 Windows 桌面时的系统亮度工具
- 播放媒体时的上一首/下一首工具

除了这一常规平台支持之外，Surface Dial 还与 Windows Ink 平台控件（[**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas) 和 [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar)）紧密集成。

![适配 Surface 触控笔的 Surface Dial](images/windows-wheel/dial-and-pen-400px.png)  
*适配 Surface 触控笔的 Surface Dial*

当与 Surface Dial 搭配使用时，这些控件可启用用于修改 Ink 属性和控制 Ink 工具栏标尺模具的附加功能。

在使用 Ink 工具栏的墨迹书写应用程序中打开 Surface Dial 菜单时，该菜单中现在包含用于控制笔类型和画笔粗细的工具。 标尺已启用时，会将相应工具添加到该菜单中供设备控制标尺的位置和角度。

![适配 Windows Ink 工具栏的笔选择工具的 Surface Dial 菜单](images/windows-wheel/surface-dial-menu-inktoolbar-pen.png)  
*适配 Windows Ink 工具栏的笔选择工具的 Surface Dial 菜单*

![适配 Windows Ink 工具栏的笔划大小工具的 Surface Dial 菜单](images/windows-wheel/surface-dial-menu-inktoolbar-strokesize.png)  
*适配 Windows Ink 工具栏的笔划大小工具的 Surface Dial 菜单*

![适配 Windows Ink 工具栏的标尺工具的 Surface Dial 菜单](images/windows-wheel/surface-dial-menu-inktoolbar-ruler.png)  
*适配 Windows Ink 工具栏的标尺工具的 Surface Dial 菜单*

## <a name="user-customization"></a>用户自定义

用户可以通过 **Windows 设置 -> 设备 -> Wheel** 自定义其 Dial 体验的某些方面，其中包括默认工具、振动（或触觉反馈）和书写（或惯用）手。 

自定义 Surface Dial 用户体验时，应始终确保特定功能或行为可用，并且受用户支持。

## <a name="custom-tools"></a>自定义工具

下面我们介绍的 UX 和开发人员指南可用于自定义 Surface Dial 菜单上公开的工具。

### <a name="ux-guidance"></a>UX 指南

**确保工具对应于当前上下文**当你彻底了解了工具执行的具体操作和 Surface Dial 交互的工作原理后，可帮助用户快速上手，并使他们专注于自己的任务。

**尽可能减少应用工具的数量**  
Surface Dial 菜单可容纳七个项。 如果有八个或更多项，用户需要转动 Dial 才能在溢出的浮出控件中看到可用的工具，使菜单难以导航，并且使工具难以发现和选择。

我们建议为应用或应用上下文只提供一个自定义工具。 如此可使你基于用户正在执行的操作设置工具，无需用户激活 Surface Dial 菜单并选择工具。 

**动态地更新工具的集合**  
由于 Surface Dial 菜单项不支持禁用状态，因此应基于用户上下文（当前视图或焦点窗口）动态地添加和删除工具（包括默认的内置工具）。 如果工具与当前活动无关，或者它是多余的，请删除它。

> [!IMPORTANT]
> 当向菜单添加项时，请确保菜单中尚不存在该项。

**请勿删除内置的系统音量设置工具**  
通常，用户常常需要使用音量控制。 他们可能会边聆听音乐边使用应用，因此应始终可以从 Surface Dial 菜单访问音量和下一首工具。 （播放媒体时，会将下一首工具自动添加到菜单。）

**与菜单组织保持一致**  
这有助于用户在使用应用时发现并了解哪些工具可用，还有助于提高用户切换工具时的效率。

**提供与内置图标一致的高质量图标**  
图标可以传达专业和卓越水准，使用户产生信任感。
- 提供 64 x 64 像素的高质量 PNG 图像（44 x 44 为受支持的最小像素）
- 确保背景透明
- 图标应填满图像的大部分
- 白色图标应具有黑色边框才能在高对比度模式下可见

|   |   |   |
| --- | --- | --- |
| ![具有 Alpha 背景的图标](images/windows-wheel/surface-dial-menu-icon1.png) | ![使用默认主题图标在 Wheel 菜单上显示的图标](images/windows-wheel/surface-dial-menu-icon2.png) | ![Surface Dial 屏幕菜单](images/windows-wheel/surface-dial-menu-icon3.png) |
| *具有 Alpha 背景的图标* | *使用默认主题在 Wheel 菜单上显示的图标* | *使用高对比度白色主题在 Wheel 菜单上显示的图标* |

**使用描述性的简洁名称**  
工具名称以及工具图标一起显示在工具菜单中，屏幕阅读器也会使用该工具名称。 
- 名称应短到足以放入 Wheel 菜单的中心圆圈内
- 名称应清楚地标识主要操作（可以隐含互补操作）：
  - “滚动”指示两个旋转方向的效果
  - “撤消”指定主要操作，但用户可以推断和轻松地发现“重做”（互补操作）

### <a name="developer-guidance"></a>开发人员指南

可以通过一组全面的 [Windows 运行时 API](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) 自定义 Surface Dial 体验，来补充应用中的功能。 

如前所述，Surface Dial 的默认菜单中预置了一组内置工具，这些工具涵盖广泛的基本系统功能（系统音量、系统亮度、滚动、缩放、撤消和媒体控制（系统检测到正在播放音频或视频时））。 不过，这些默认工具可能不会提供你的应用所需的功能。 

在以下部分中，我们介绍了如何向 Surface Dial 菜单添加自定义工具，以及如何指定哪些内置工具要公开。

**添加自定义工具**

在此示例中，我们添加一个基本的自定义工具，该工具将旋转事件和单击事件中的输入数据传入某些 XAML UI 控件。

1. 首先，我们在 XAML 中声明我们的 UI（仅一个滑块和一个切换按钮）。

   ![示例应用 UI 的图像](images/windows-wheel/surface-dial-snippet-customtool1.png)  
   *示例应用 UI*

    ```Xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
      </Grid.RowDefinitions>
      <StackPanel x:Name="HeaderPanel" 
        Orientation="Horizontal" 
        Grid.Row="0">
          <TextBlock x:Name="Header"
            Text="RadialController customization sample"
            VerticalAlignment="Center"
            Style="{ThemeResource HeaderTextBlockStyle}"
            Margin="10,0,0,0" />
      </StackPanel>
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center"
        Grid.Row="1">
          <!-- Slider for rotation input -->
          <Slider x:Name="RotationSlider"
            Width="300"
            HorizontalAlignment="Left"/>
          <!-- Switch for click input -->
          <ToggleSwitch x:Name="ButtonToggle"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    ```

2. 然后，我们在代码隐藏中将自定义工具添加到 Surface Dial 菜单，并声明 [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) 输入处理程序。 

   我们通过调用 [**CreateForCurrentView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController)，获取对 Surface Dial (myController) 的 [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.CreateForCurrentView) 对象的引用。

   然后，我们通过调用 [**RadialControllerMenuItem.CreateFromIcon**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuItem) 创建 [**RadialControllerMenuItem**](https://msdn.microsoft.com/library/windows/apps/mt759255) (myItem) 的实例。 

   接下来，我们将该项附加到菜单项的集合。

   我们为 [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked) 对象声明输入事件处理程序（[**ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationChanged) 和 [**RotationChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController)）。

   最后，我们定义事件处理程序。

    ```csharp
    public sealed partial class MainPage : Page
    {
        RadialController myController;

        public MainPage()
        {
            this.InitializeComponent();
            // Create a reference to the RadialController.
            myController = RadialController.CreateForCurrentView();

            // Create an icon for the custom tool.
            RandomAccessStreamReference icon =
              RandomAccessStreamReference.CreateFromUri(
                new Uri("ms-appx:///Assets/StoreLogo.png"));

            // Create a menu item for the custom tool.
            RadialControllerMenuItem myItem =
              RadialControllerMenuItem.CreateFromIcon("Sample", icon);

            // Add the custom tool to the RadialController menu.
            myController.Menu.Items.Add(myItem);

            // Declare input handlers for the RadialController.
            myController.ButtonClicked += MyController_ButtonClicked;
            myController.RotationChanged += MyController_RotationChanged;
        }

        // Handler for rotation input from the RadialController.
        private void MyController_RotationChanged(RadialController sender,
          RadialControllerRotationChangedEventArgs args)
        {
            if (RotationSlider.Value + args.RotationDeltaInDegrees > 100)
            {
                RotationSlider.Value = 100;
                return;
            }
            else if (RotationSlider.Value + args.RotationDeltaInDegrees < 0)
            {
                RotationSlider.Value = 0;
                return;
            }
            RotationSlider.Value += args.RotationDeltaInDegrees;
        }

        // Handler for click input from the RadialController.
        private void MyController_ButtonClicked(RadialController sender,
          RadialControllerButtonClickedEventArgs args)
        {
            ButtonToggle.IsOn = !ButtonToggle.IsOn;
        }
    }
    ```

当我们运行该应用时，我们使用 Surface Dial 与之进行交互。 首先，我们长按以打开菜单，然后选择我们的自定义工具。 自定义工具激活后，通过旋转 Dial 可调整滑块控件，通过单击 Dial 可切换开关。

![使用 Surface Dial 自定义工具激活的示例应用 UI 的图像](images/windows-wheel/surface-dial-snippet-customtool2.png)  
*使用 Surface Dial 自定义工具激活的示例应用 UI*

**指定内置工具**

可以使用 [**RadialControllerConfiguration**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerConfiguration) 类为应用自定义内置菜单项的集合。

例如，当你的应用中不存在任何滚动或缩放区域，也不需要撤消/重做功能时，可以将这些工具从菜单中删除。 这将在菜单上打开用于为你的应用添加自定义工具的空间。 

> [!IMPORTANT] 
> Surface Dial 菜单必须包含至少一个菜单项。 在添加你的其中一个自定义工具之前，如果所有默认工具都已删除，会恢复这些默认工具并将你的工具附加到默认集合。

在每个设计指南中，我们不建议删除媒体控制工具（音量和上一首/下一首），因为用户经常一边在后台播放音乐，一边执行其他任务。

我们在下面介绍了如何配置 Surface Dial 菜单，以仅包含用于音量和下一首/上一首的媒体控件。

```csharp
public MainPage()
{
  ...
  //Remove a subset of the default system tools
  RadialControllerConfiguration myConfiguration = 
  RadialControllerConfiguration.GetForCurrentView();
  myConfiguration.SetDefaultMenuItems(new[] 
  {
    RadialControllerSystemMenuItemKind.Volume,
      RadialControllerSystemMenuItemKind.NextPreviousTrack
  });
}
```

## <a name="custom-interactions"></a>自定义交互

如上所述，Surface Dial 支持与默认交互对应的三种手势（即长按、旋转、单击）。 

确保基于这些手势的任何自定义交互对于所选的操作或工具都有意义。 

> [!NOTE]
> 交互体验依赖于 Surface Dial 菜单的状态。 如果菜单处于活动状态，该菜单处理输入；否则，你的应用处理输入。

### <a name="press-and-hold"></a>长按

此手势会激活并显示 Surface Dial 菜单，没有与该手势相关联的任何应用功能。 

默认情况下，该菜单显示在用户屏幕的中心。 不过，用户可以抓取并随意移动它。

> [!NOTE]
> Surface Dial 置于 Surface Studio 的屏幕上时，菜单居于 Surface Dial 屏幕位置的中心。

### <a name="rotate"></a>旋转

Surface Dial 主要设计用于对涉及模拟值或控制的平滑、增量调整的交互支持旋转。

该设备可以顺时针和逆时针旋转，还可以提供触觉反馈以指示离散距离。

> [!NOTE]
> 用户可以在 **Windows 设置 -> 设备 -> Wheel** 页中禁用触觉反馈。

#### <a name="ux-guidance"></a>UX 指南

**具有连续或高旋转灵敏度的工具应禁用触觉反馈**

触觉反馈与活动工具的旋转灵敏度匹配。 我们建议禁用具有连续或高旋转灵敏度的工具的触觉反馈，因为用户体验可能会感到不舒服。 

**惯用手不应该影响基于旋转的交互**

Surface Dial 无法检测到正在使用哪只手，但用户可以在 **Windows 设置 -> 设备 -> 触控笔和 Windows Ink** 中设置书写（或惯用手）。

**应该为所有旋转交互考虑区域设置。**

通过使你的交互适应和适合区域设置以及从右到左的布局，最大程度地提高客户的满意度。

内置工具和 Dial 菜单中的命令遵循适用于基于旋转的交互的以下指南：

|   |   |   |
| --- | --- | --- |
| 向左<br/>向上<br/>缩小 | ![Surface Dial 的图像](images/windows-wheel/surface-dial-rotate.png) | 向右<br/>向下<br/>放大 |
|   |   |   |

| 概念性方向 | 映射到 Surface Dial | 顺时针旋转 | 逆时针旋转 |
| --- | --- | --- | --- |
| 水平 | 基于 Surface Dial 顶部的左右映射 | 向右 | 向左 |
| 垂直 | 基于 Surface Dial 左侧的上下映射 | 向下 | 向上 |
| Z 轴 | 映射到向上/向右的放大（或靠近）<br/>映射到向下/向左的缩小（或远离） | 放大 | 缩小 |

#### <a name="developer-guidance"></a>开发人员指南

用户旋转设备时，将基于相对于旋转方向的增量 ([**RadialControllerRotationChangedEventArgs.RotationDeltaInDegrees**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationChanged)) 触发 [**RadialController.RotationChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerRotationChangedEventArgs.RotationDeltaInDegrees) 事件。 可以使用 [**RadialController.RotationResolutionInDegrees**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationResolutionInDegrees) 属性设置数据的灵敏度（或分辨率）。

> [!NOTE]
> 默认情况下，仅当设备旋转至少 10 度时，才会向 [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) 对象传递旋转输入事件。 每个输入事件都会导致设备振动。

通常，当旋转分辨率设置为低于 5 度时，我们建议禁用触觉反馈。 这将为连续交互提供更流畅的体验。 

可以通过设置 [**RadialController.UseAutomaticHapticFeedback**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.UseAutomaticHapticFeedback) 属性启用和禁用自定义工具的触觉反馈。

> [!NOTE]
> 无法覆盖系统工具（例如音量控件）的触觉行为。 对于这些工具，只可以由用户从“Wheel”设置页禁用触觉反馈。

以下是如何自定义旋转数据的分辨率和启用或禁用反馈触觉的示例。

```csharp
private void MyController_ButtonClicked(RadialController sender, 
  RadialControllerButtonClickedEventArgs args)
{
  ButtonToggle.IsOn = !ButtonToggle.IsOn;

  if(ButtonToggle.IsOn)
  {
    //high resolution mode
    RotationSlider.LargeChange = 1;
    myController.UseAutomaticHapticFeedback = false;
    myController.RotationResolutionInDegrees = 1;
  }
  else
  {
    //low resolution mode
    RotationSlider.LargeChange = 10;
    myController.UseAutomaticHapticFeedback = true;
    myController.RotationResolutionInDegrees = 10;
  }
}
```

### <a name="click"></a>单击

单击 Surface Dial 类似于单击鼠标左键（设备的旋转状态不影响该操作）。

#### <a name="ux-guidance"></a>UX 指南

**如果用户不能轻松地从结果中恢复，请勿将操作或命令映射到该手势**

必须可撤消你的应用基于用户单击 Surface Dial 执行的任何操作。 始终使用户能够轻松地遍历应用后退堆栈和恢复应用的以前状态。

单击手势的二元运算（如静音/取消静音或显示/隐藏）提供良好的用户体验。

**不应该通过单击 Surface Dial 启用或禁用模式工具**

某些应用/工具模式可能会禁用依赖旋转的交互或与其冲突。 Windows Ink 工具栏中的功能（如标尺）应通过其他 UI 提供功能（Ink 工具栏提供了内置的 [**ToggleButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.ToggleButton) 控件）切换开关。

对于模式工具，请将 Surface Dial 活动菜单项映射到目标工具或先前选定的菜单项。

#### <a name="developer-guidance"></a>开发人员指南

单击 Surface Dial 时，会触发 [**RadialController.ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked) 事件。 [ **RadialControllerButtonClickedEventArgs** ](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs) 包括 [**Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs.Contact) 属性，该属性包含 Surface Dial 与 Surface Studio 屏幕接触的位置和边界区域。 Surface Dial 未与屏幕保持接触，则该属性为 null。 

### <a name="on-screen"></a>屏幕

如之前所述，Surface Dial 可以与 Surface Studio 配合使用，以在特殊屏幕模式中显示 Surface Dial 菜单。 

在此模式中时，可以将 Dial 交互体验与你的应用更进一步集成并自定义。 仅在将 Surface Dial 和 Surface Studio 搭配使用时才可能提供的独特体验的示例包括：
- 基于 Surface Dial 的位置显示上下文工具（如调色板），这使查找和使用这些工具更容易
- 基于放置 Surface Dial 的 UI 设置活动工具
- 基于 Surface Dial 的位置放大屏幕区域
- 基于屏幕位置的独特游戏交互

#### <a name="ux-guidance"></a>UX 指南

**屏幕上检测到 Surface Dial 时，应用应做出响应**

视觉反馈可帮助向用户指示你的应用在 Surface Studio 上已检测到设备。

**基于设备位置调整与 Surface Dial 有关的 UI**

设备（和用户的身体）可能会阻挡关键的 UI，具体取决于用户放置设备的位置。

**基于用户交互调整与 Surface Dial 有关的 UI**

除了硬件阻挡，用户的手和臂在用户使用设备时可能会阻挡部分屏幕。 

已阻挡区域取决于正在使用设备的手。 由于设备设计为主要与非惯用手配合使用，因此与 Surface Dial 有关的 UI 应针对用户指定（**Windows 设置 > 设备 > 触控笔和 Windows Ink > 选择书写时使用哪只手**设置）的另一只手进行调整。

**交互应响应 Surface Dial 的位置，而不是移动。**

设备的脚设计为紧跟屏幕而不是滑动，因为它不是精确的指针设备。 因此，我们希望用户使用 Surface Dial 的常见方式是提起、放下它，而不是在屏幕上拖动它。

**使用屏幕位置确定用户意图**

基于 UI 上下文（如邻近的控件、画布或窗口）设置活动工具，可通过减少执行任务所需的步骤来改善用户体验。

#### <a name="developer-guidance"></a>开发人员指南

当 Surface Dial 置于 Surface Studio 的数字化器图面上时，将触发 [**RadialController.ScreenContactStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ScreenContactStarted) 事件，并向你的应用提供接触信息 ([**RadialControllerScreenContactStartedEventArgs.Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs.Contact))。

同样，如果在 Surface Dial 与 Surface Studio 的数字化器图面接触的情况下单击它，将触发 [**RadialController.ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked) 事件，并向你的应用提供接触信息 ([**RadialControllerButtonClickedEventArgs.Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs.Contact))。 

接触信息 ([**RadialControllerScreenContact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact)) 包括 Surface Dial 中心在应用的坐标空间中的 X/Y 坐标 ([**RadialControllerScreenContact.Position**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact.Position))，以及边界矩形 ([**RadialControllerScreenContact.Bounds**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact.Bounds))（以与设备无关的像素 (DIP) 为单位）。 此信息对于向活动工具提供上下文和向用户提供与设备有关的视觉反馈非常有用。

在以下示例中，我们创建了一个四个部分不同的基本应用，其中的每一个都包含一个滑块和一个切换按钮。 然后，我们使用 Surface Dial 的屏幕位置来确定哪组滑块和切换按钮受 Surface Dial 控制。

1. 首先，我们在 XAML 中声明我们的 UI（四个部分，每一个都有一个滑块和一个切换按钮）。

   ![示例应用 UI 的图像](images/windows-wheel/surface-dial-snippet-customtool3.png)  
   *示例应用 UI*

   ```xaml 
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
  <Grid.RowDefinitions>
    <RowDefinition Height="Auto"/>
    <RowDefinition Height="*"/>
  </Grid.RowDefinitions>
  <StackPanel x:Name="HeaderPanel" 
        Orientation="Horizontal" 
        Grid.Row="0">
    <TextBlock x:Name="Header"
      Text="RadialController customization sample"
      VerticalAlignment="Center"
      Style="{ThemeResource HeaderTextBlockStyle}"
      Margin="10,0,0,0" />
  </StackPanel>
  <Grid Grid.Row="1" x:Name="RootGrid">
    <Grid.RowDefinitions>
      <RowDefinition Height="*"/>
      <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
      <ColumnDefinition Width="*"/>
      <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <Grid x:Name="Grid0"
      Grid.Row="0"
      Grid.Column="0">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider0"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle0"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid1"
      Grid.Row="0"
      Grid.Column="1">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider1"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle1"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid2"
      Grid.Row="1"
      Grid.Column="0">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider2"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle2"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid3"
      Grid.Row="1"
      Grid.Column="1">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider3"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle3"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
  </Grid>
</Grid>
   ```

2. 下面介绍为 Surface Dial 屏幕位置定义的处理程序的代码隐藏。

```csharp
Slider ActiveSlider;
ToggleSwitch ActiveSwitch;
Grid ActiveGrid;

public MainPage()
{
  ...

  myController.ScreenContactStarted += 
    MyController_ScreenContactStarted;
  myController.ScreenContactContinued += 
    MyController_ScreenContactContinued;
  myController.ScreenContactEnded += 
    MyController_ScreenContactEnded;
  myController.ControlLost += MyController_ControlLost;

  //Set initial grid for Surface Dial input.
  ActiveGrid = Grid0;
  ActiveSlider = RotationSlider0;
  ActiveSwitch = ButtonToggle0;
}

private void MyController_ScreenContactStarted(RadialController sender, 
  RadialControllerScreenContactStartedEventArgs args)
{
  //find grid at contact location, update visuals, selection
  ActivateGridAtLocation(args.Contact.Position);
}

private void MyController_ScreenContactContinued(RadialController sender, 
  RadialControllerScreenContactContinuedEventArgs args)
{
  //if a new grid is under contact location, update visuals, selection
  if (!VisualTreeHelper.FindElementsInHostCoordinates(
    args.Contact.Position, RootGrid).Contains(ActiveGrid))
  {
    ActiveGrid.Background = new 
      SolidColorBrush(Windows.UI.Colors.White);
    ActivateGridAtLocation(args.Contact.Position);
  }
}

private void MyController_ScreenContactEnded(RadialController sender, object args)
{
  //return grid color to normal when contact leaves screen
  ActiveGrid.Background = new 
  SolidColorBrush(Windows.UI.Colors.White);
}

private void MyController_ControlLost(RadialController sender, object args)
{
  //return grid color to normal when focus lost
  ActiveGrid.Background = new 
    SolidColorBrush(Windows.UI.Colors.White);
}

private void ActivateGridAtLocation(Point Location)
{
  var elementsAtContactLocation = 
    VisualTreeHelper.FindElementsInHostCoordinates(Location, 
      RootGrid);

  foreach (UIElement element in elementsAtContactLocation)
  {
    if (element as Grid == Grid0)
    {
      ActiveSlider = RotationSlider0;
      ActiveSwitch = ButtonToggle0;
      ActiveGrid = Grid0;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid1)
    {
      ActiveSlider = RotationSlider1;
      ActiveSwitch = ButtonToggle1;
      ActiveGrid = Grid1;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid2)
    {
      ActiveSlider = RotationSlider2;
      ActiveSwitch = ButtonToggle2;
      ActiveGrid = Grid2;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid3)
    {
      ActiveSlider = RotationSlider3;
      ActiveSwitch = ButtonToggle3;
      ActiveGrid = Grid3;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
  }
}
```

当我们运行该应用时，我们使用 Surface Dial 与之进行交互。 首先，我们将设备置于 Surface Studio 屏幕上，应用会检查到该设备并将它与右下部分关联（请参阅图像）。 然后，我们长按 Surface Dial 以打开菜单，然后选择我们的自定义工具。 自定义工具激活后，通过旋转 Surface Dial 可调整滑块控件，通过单击 Surface Dial 可切换开关。

![使用 Surface Dial 自定义工具激活的示例应用 UI 的图像](images/windows-wheel/surface-dial-snippet-customtool4.png)  
*使用 Surface Dial 自定义工具激活的示例应用 UI*

## <a name="summary"></a>小结

本主题概述了有关以下方面的 Surface Dial 输入设备与 UX 和开发人员指南：与 Surface Studio 配合使用时，如何针对离屏情形以及屏幕情形自定义用户体验。

## <a name="feedback"></a>反馈

请将你的问题、建议和反馈发送至 [radialcontroller@microsoft.com](mailto:radialcontroller@microsoft.com)。

## <a name="related-articles"></a>相关文章

### <a name="api-reference"></a>API 参考

- [**RadialController** 类](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController)
- [**RadialControllerButtonClickedEventArgs** 类](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs)
- [**RadialControllerConfiguration** 类](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerConfiguration) 
- [**RadialControllerControlAcquiredEventArgs** 类](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerControlAcquiredEventArgs) 
- [**RadialControllerMenu** 类](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenu) 
- [**RadialControllerMenuItem** 类](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuItem) 
- [**RadialControllerRotationChangedEventArgs** 类](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerRotationChangedEventArgs) 
- [**RadialControllerScreenContact** 类](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact) 
- [**RadialControllerScreenContactContinuedEventArgs** 类](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactContinuedEventArgs) 
- [**RadialControllerScreenContactStartedEventArgs** 类](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs)
- [**RadialControllerMenuKnownIcon** 枚举](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuKnownIcon) 
- [**RadialControllerSystemMenuItemKind** 枚举](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerSystemMenuItemKind) 

### <a name="samples"></a>示例

[通用 Windows 平台示例（C# 和 C++）](https://go.microsoft.com/fwlink/?linkid=832713)

[Windows 经典桌面示例](https://aka.ms/radialcontrollerclassicsample)