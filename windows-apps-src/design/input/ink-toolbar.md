---
Description: 向 Windows 应用墨迹应用添加默认 InkToolbar，将自定义笔按钮添加到 InkToolbar，并将自定义笔按钮绑定到自定义笔定义。
title: 将 InkToolbar 添加到 Windows 应用
label: Add an InkToolbar to a Windows app
template: detail.hbs
keywords: Windows Ink，Windows 墨迹书写，DirectInk，InkPresenter，InkCanvas，InkToolbar，通用 Windows 平台，UWP，用户交互，输入
ms.date: 02/08/2017
ms.topic: article
ms.assetid: d888f75f-c2a0-4134-81db-907b5e24fcc5
ms.localizationpriority: medium
ms.openlocfilehash: 4052ac6daddcfecabb839d16fd5f81c3d207d01b
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/12/2020
ms.locfileid: "83233687"
---
# <a name="add-an-inktoolbar-to-a-windows-app"></a>将 InkToolbar 添加到 Windows 应用



可以通过两种不同的控件在 Windows 应用程序中实现墨迹书写： [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)和[**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)。

[**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 控件提供基本 Windows Ink 功能。 使用它将笔输入呈现为笔划墨迹（使用颜色和粗细的默认设置）或擦除笔划。

> 有关 InkCanvas 实现的详细信息，请参阅[Windows 应用中的笔和触笔交互](pen-and-stylus-interactions.md)。

作为完全透明的覆盖层，InkCanvas 不提供任何用于设置笔划墨迹属性的内置 UI。 如果你希望更改默认的墨迹书写体验，请让用户设置笔划墨迹属性，并支持其他自定义墨迹书写功能，你有两个选项：

- 在代码隐藏中，请使用绑定到 InkCanvas 的 [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter) 对象。

  InkPresenter API 支持墨迹书写体验的广泛自定义。 有关更多详细信息，请参阅[Windows 应用中的笔和触笔交互](pen-and-stylus-interactions.md)。

- 将 [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 绑定到 InkCanvas。 默认情况下，InkToolbar 会提供一组可自定义和可扩展的按钮，用于激活与墨迹相关的功能，如笔划大小、墨迹颜色和笔尖形状。

  我们将在本主题中讨论 InkToolbar。

> **重要的 api**： [**InkCanvas 类**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)、 [**InkToolbar 类**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)、 [**InkPresenter 类**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter) [**、**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking)

## <a name="default-inktoolbar"></a>默认 InkToolbar

默认情况下，[**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 包括用于绘制、擦除、突出显示和显示模板（标尺或量角器）的按钮。 根据功能，在浮出控件中提供其他设置和命令，如墨迹颜色、笔划粗细、擦除所有墨迹。

![InkToolbar](./images/ink/ink-tools-invoked-toolbar-small.png)  
*默认的 Windows Ink 工具栏*

若要在墨迹书写应用中添加默认的 [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)，只需将其放在与 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 页面相同的页面上，并关联这两个控件。

1. 在 MainPage.xaml 中，为墨迹书写图面声明一个容器对象（对于此示例，我们使用 Grid 控件）。
2. 声明一个 InkCanvas 对象作为容器的子项。 （InkCanvas 大小继承自该容器。）
3. 声明一个 InkToolbar 并使用 TargetInkCanvas 属性将其绑定到 InkCanvas。

> [!NOTE]
> 确保在 InkCanvas 之后声明 InkToolbar。 否则，InkCanvas 覆盖层会使 InkToolbar 不可访问。

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
    </Grid>
</Grid>
```

## <a name="basic-customization"></a>基本自定义

在此部分中，我们将介绍一些基本的 Windows Ink 工具栏自定义方案。

### <a name="specify-location-and-orientation"></a>指定位置和方向

在向应用中添加墨迹工具栏时，你可以接受工具栏的默认位置和方向，也可以按照应用或用户的要求设置位置和方向。

**XAML**

通过工具栏的 [VerticalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.VerticalAlignment)、[HorizontalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.HorizontalAlignment) 和 [Orientation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar?branch=rs3.Orientation) 属性显式指定其位置和方向。

| 默认 | 显式 |
| --- | --- |
| ![默认墨迹工具栏位置和方向](./images/ink/location-default-small.png) | ![显式墨迹工具栏位置和方向](./images/ink/location-explicit-small.png) |
| *Windows Ink 工具栏默认位置和方向* | *Windows Ink 工具栏显式位置和方向* |

下面是在 XAML 中显式设置墨迹工具栏位置和方向的代码。
```xaml
<InkToolbar x:Name="inkToolbar" 
    VerticalAlignment="Center" 
    HorizontalAlignment="Right" 
    Orientation="Vertical" 
    TargetInkCanvas="{x:Bind inkCanvas}" />
```

**根据用户首选项或设备状态进行初始化**

在某些情况下，你可能想要根据用户首选项或设备状态设置墨迹工具栏的位置和方向。 下面的示例演示了如何根据通过**设置 > 设备 > 笔和 Windows Ink > 笔 > 选择书写时使用哪只手**指定的左手或右手书写首选项设置墨迹工具栏的位置和方向。

![惯用手设置](./images/ink/location-handedness-setting.png)  
*惯用手设置*

你可以通过 Windows.UI.ViewManagement 的 HandPreference 属性查询此设置，并根据返回的值设置 [HorizontalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.HorizontalAlignment)。 在此示例中，对于惯用左手的人，我们将工具栏定位在应用的左侧，对于惯用右手的人，则定位在右侧。

**从[墨迹工具栏位置和方向示例下载此示例（基本）](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)**

```csharp
public MainPage()
{
    this.InitializeComponent();

    Windows.UI.ViewManagement.UISettings settings = 
        new Windows.UI.ViewManagement.UISettings();
    HorizontalAlignment alignment = 
        (settings.HandPreference == 
            Windows.UI.ViewManagement.HandPreference.LeftHanded) ? 
            HorizontalAlignment.Left : HorizontalAlignment.Right;
    inkToolbar.HorizontalAlignment = alignment;
}
```

**根据用户或设备状态动态调整**

你还可以根据用户首选项、设备设置或设备状态的更改，使用绑定来管理 UI 更新。 在下面的示例中，我们进一步扩展前一个示例并显示如何根据使用绑定的设备方向、ViewMOdel 对象和 [INotifyPropertyChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.inotifypropertychanged) 接口动态定位墨迹工具栏。 

**从[墨迹工具栏位置和方向示例（动态）](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)下载此示例**

1. 首先，我们添加 ViewModel。
    1. 向你的项目中添加一个新文件夹并将其命名为 **ViewModels**。
    1. 向 ViewModels 文件夹中添加一个新类（对于本例，我们将其命名为 **InkToolbarSnippetHostViewModel.cs**）。
        > [!NOTE] 
        > 我们使用了[单一实例模式](https://docs.microsoft.com/previous-versions/msp-n-p/ff650849(v=pandp.10))，因为在应用程序的生命周期内，我们只需要一个这种类型的对象

    1. 向文件中添加 `using System.ComponentModel` 命名空间。
    1. 添加一个名为 **instance** 的静态成员变量和一个名为 **Instance** 的静态只读属性。 将构造函数设置为私有以确保只能通过 Instance 属性访问此类。   
        > [!NOTE] 
        > 此类继承 [INotifyPropertyChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.inotifypropertychanged) 接口，该接口用于通知客户端（通常为绑定客户端）某个属性值已更改。 我们将使用此类来处理设备方向的变化（我们将在后面的步骤中展开此代码并进一步进行说明）。  

        ```csharp
        using System.ComponentModel;

        namespace locationandorientation.ViewModels
        {
            public class InkToolbarSnippetHostViewModel : INotifyPropertyChanged
            {
                private static InkToolbarSnippetHostViewModel instance;
                
                public static InkToolbarSnippetHostViewModel Instance
                {
                    get
                    {
                        if (null == instance)
                        {
                            instance = new InkToolbarSnippetHostViewModel();
                        }
                        return instance;
                    }
                }
            }

            private InkToolbarSnippetHostViewModel() { }
        }
        ```

    1. 向 InkToolbarSnippetHostViewModel 类中添加两个布尔属性：**LeftHandedLayout**（功能与前一个仅 XAML 示例相同）和 **PortraitLayout**（设备的方向）。
        >[!NOTE] 
        > PortraitLayout 属性可以设置，并且包括 [PropertyChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 事件的定义。

        ```csharp
        public bool LeftHandedLayout
        {
            get
            {
                bool leftHandedLayout = false;
                Windows.UI.ViewManagement.UISettings settings =
                    new Windows.UI.ViewManagement.UISettings();
                leftHandedLayout = (settings.HandPreference ==
                    Windows.UI.ViewManagement.HandPreference.LeftHanded);
                return leftHandedLayout;
            }
        }

        public bool portraitLayout = false;
        public bool PortraitLayout
        {
            get
            {
                Windows.UI.ViewManagement.ApplicationViewOrientation winOrientation = 
                    Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
                portraitLayout = 
                    (winOrientation == 
                        Windows.UI.ViewManagement.ApplicationViewOrientation.Portrait);
                return portraitLayout;
            }
            set
            {
                if (value.Equals(portraitLayout)) return;
                portraitLayout = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("PortraitLayout"));
            }
        }
        ```

1. 现在，我们向项目中添加几个转换器类。 每个类均包含一个可返回对齐值（[HorizontalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.horizontalalignment) 或 [VerticalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.verticalalignment)）的 Convert 对象。
    1. 向你的项目中添加一个新文件夹并将其命名为 **Converters**。
    1. 向 Converters 文件夹中添加两个新类（对于本示例，我们将其命名为 **HorizontalAlignmentFromHandednessConverter.cs** 和 **VerticalAlignmentFromAppViewConverter.cs**）。
    1. 向每个文件中添加 `using Windows.UI.Xaml` 和 `using Windows.UI.Xaml.Data` 命名空间。
    1. 将每个类更改为 `public` 并指定其实现 [IValueConverter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.ivalueconverter) 接口。
    1. 向每个文件中添加 [Convert](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.ivalueconverter.convert) 和 [ConvertBack](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.ivalueconverter.convertback) 方法，如此处所示（我们保持不实现 ConvertBack 方法）。
        - 对于惯用右手的用户，HorizontalAlignmentFromHandednessConverter 将墨迹工具栏定位到应用的右侧，对于惯用左手的用户，则定位到应用的左侧。
        ```csharp
        using System;

        using Windows.UI.Xaml;
        using Windows.UI.Xaml.Data;

        namespace locationandorientation.Converters
        {
            public class HorizontalAlignmentFromHandednessConverter : IValueConverter
            {
                public object Convert(object value, Type targetType,
                    object parameter, string language)
                {
                    bool leftHanded = (bool)value;
                    HorizontalAlignment alignment = HorizontalAlignment.Right;
                    if (leftHanded)
                    {
                        alignment = HorizontalAlignment.Left;
                    }
                    return alignment;
                }

                public object ConvertBack(object value, Type targetType,
                    object parameter, string language)
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

        - 对于纵向方向，VerticalAlignmentFromAppViewConverter 将墨迹工具栏定位到应用的中心，对于横向方向，则定位到应用的顶部（虽然旨在提高可用性，但这只是出于演示目的的任意选择）。
        ```csharp
        using System;

        using Windows.UI.Xaml;
        using Windows.UI.Xaml.Data;

        namespace locationandorientation.Converters
        {
            public class VerticalAlignmentFromAppViewConverter : IValueConverter
            {
                public object Convert(object value, Type targetType,
                    object parameter, string language)
                {
                    bool portraitOrientation = (bool)value;
                    VerticalAlignment alignment = VerticalAlignment.Top;
                    if (portraitOrientation)
                    {
                        alignment = VerticalAlignment.Center;
                    }
                    return alignment;
                }

                public object ConvertBack(object value, Type targetType,
                    object parameter, string language)
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1. 现在，打开 MainPage.xaml.cs 文件。
    1. 添加 `using using locationandorientation.ViewModels` 到命名空间列表以将 ViewModel 关联起来。
    1. 添加 `using Windows.UI.ViewManagement` 到命名空间列表以启用对设备方向的更改的侦听。
    1. 添加[WindowSizeChangedEventHandler](https://docs.microsoft.com/uwp/api/windows.ui.xaml.windowsizechangedeventhandler)代码。
    1. 将视图的[DataContext](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement.DataContext)设置为 InkToolbarSnippetHostViewModel 类的单一实例。 
    ```csharp
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;

    using locationandorientation.ViewModels;
    using Windows.UI.ViewManagement;

    namespace locationandorientation
    {
        public sealed partial class MainPage : Page
        {
            public MainPage()
            {
                this.InitializeComponent();

                Window.Current.SizeChanged += (sender, args) =>
                {
                    ApplicationView currentView = ApplicationView.GetForCurrentView();

                    if (currentView.Orientation == ApplicationViewOrientation.Landscape)
                    {
                        InkToolbarSnippetHostViewModel.Instance.PortraitLayout = false;
                    }
                    else if (currentView.Orientation == ApplicationViewOrientation.Portrait)
                    {
                        InkToolbarSnippetHostViewModel.Instance.PortraitLayout = true;
                    }
                };

                DataContext = InkToolbarSnippetHostViewModel.Instance;
            }
        }
    }
    ```

1. 接下来，打开 MainPage 文件。
    1. 添加 `xmlns:converters="using:locationandorientation.Converters"` 到元素，以 `Page` 绑定到转换器。
        ```xaml
        <Page
        x:Class="locationandorientation.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:locationandorientation"
        xmlns:converters="using:locationandorientation.Converters"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">
        ```

    1. 添加 `PageResources` 元素并指定对转换器的引用。
        ```xaml
        <Page.Resources>
            <converters:HorizontalAlignmentFromHandednessConverter x:Key="HorizontalAlignmentConverter"/>
            <converters:VerticalAlignmentFromAppViewConverter x:Key="VerticalAlignmentConverter"/>
        </Page.Resources>
        ```

    1. 添加 InkCanvas 和 InkToolbar 元素，并绑定 InkToolbar 的 VerticalAlignment 和 HorizontalAlignment 属性。
        ```xaml
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar" 
                    VerticalAlignment="{Binding PortraitLayout, Converter={StaticResource VerticalAlignmentConverter} }" 
                    HorizontalAlignment="{Binding LeftHandedLayout, Converter={StaticResource HorizontalAlignmentConverter} }" 
                    Orientation="Vertical" 
                    TargetInkCanvas="{x:Bind inkCanvas}" />
        ```

1. 返回到 InkToolbarSnippetHostViewModel.cs 文件，以便将 `PortraitLayout` 和 `LeftHandedLayout` bool 属性添加到 `InkToolbarSnippetHostViewModel` 类，并支持 `PortraitLayout` 在属性值更改时重新绑定。 
    ```csharp
    public bool LeftHandedLayout
    {
        get
        {
            bool leftHandedLayout = false;
            Windows.UI.ViewManagement.UISettings settings =
                new Windows.UI.ViewManagement.UISettings();
            leftHandedLayout = (settings.HandPreference ==
                Windows.UI.ViewManagement.HandPreference.LeftHanded);
            return leftHandedLayout;
        }
    }

    public bool portraitLayout = false;
    public bool PortraitLayout
    {
        get
        {
            Windows.UI.ViewManagement.ApplicationViewOrientation winOrientation = 
                Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
            portraitLayout = 
                (winOrientation == 
                    Windows.UI.ViewManagement.ApplicationViewOrientation.Portrait);
            return portraitLayout;
        }
        set
        {
            if (value.Equals(portraitLayout)) return;
            portraitLayout = value;
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("PortraitLayout"));
        }
    }

    #region INotifyPropertyChanged Members

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string property)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(property));
    }

    #endregion
    ```

现在，你应具有可适应用户的主导首选项的墨迹应用，并动态响应用户设备的方向。

### <a name="specify-the-selected-button"></a>指定所选的按钮  
![在初始化时选择的铅笔按钮](./images/ink/ink-tools-default-toolbar.png)  
*带有在初始化时选择的铅笔按钮的 Windows Ink 工具栏*

默认情况下，当你的应用启动并且工具栏初始化时，将选择第一个（或最左边的）按钮。 在默认的 Windows Ink 工具栏中，这是圆珠笔按钮。

由于该框架定义了内置按钮的顺序，因此第一个按钮可能不是你默认要激活的笔或工具。

你可以替代此默认行为，并在工具栏上指定所选的按钮。

对于此示例，我们初始化将选择铅笔按钮并激活铅笔（而不是圆珠笔）的默认工具栏。

1. 为上一个示例中的 InkCanvas 和 InkToolbar 使用 XAML 声明。
2. 在代码隐藏中，为 [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) 对象的 [Loaded](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 事件设置处理程序。

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loaded += inkToolbar_Loaded;
  }
  ```

3. 在 [Loaded](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) 事件的处理程序中：
    1. 获取对内置 [InkToolbarPencilButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton) 的引用。

    传递 [GetToolButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbartool) 方法中的 [InkToolbarTool.Pencil](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar.gettoolbutton) 对象会为 [InkToolbarPencilButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbartoolbutton) 返回 [InkToolbarToolButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton) 对象。

    2. 将 [ActiveTool](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar.activetool) 设置为上一步中返回的对象。

```CSharp
/// <summary>
/// Handle the Loaded event of the InkToolbar.
/// By default, the active tool is set to the first tool on the toolbar.
/// Here, we set the active tool to the pencil button.
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void inkToolbar_Loaded(object sender, RoutedEventArgs e)
{
    InkToolbarToolButton pencilButton = inkToolbar.GetToolButton(InkToolbarTool.Pencil);
    inkToolbar.ActiveTool = pencilButton;
}
```

### <a name="specify-the-built-in-buttons"></a>指定内置按钮

![指定在初始化时包含的按钮](./images/ink/ink-tools-specific.png)  
*指定在初始化时包含的按钮*

如上所示，Windows Ink 工具栏包含默认内置按钮的集合。 这些按钮按照以下顺序（从左到右）显示：

- [InkToolbarBallpointPenButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarballpointpenbutton)
- [InkToolbarPencilButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton)
- [InkToolbarHighlighterButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarhighlighterbutton)
- [InkToolbarEraserButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbareraserbutton)
- [InkToolbarRulerButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarrulerbutton)

对于此示例，我们仅使用内置圆珠笔、铅笔和橡皮擦按钮初始化工具栏。

使用 XAML 或代码隐藏执行此操作。

**XAML**

为第一个示例中的 InkCanvas 和 InkToolbar 修改 XAML 声明。
- 添加 [InitialControls](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar.initialcontrols) 属性并将其值设置为“[None](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarinitialcontrols)”。 这将清除内置按钮的默认集合。
- 添加你的应用所需的特定 InkToolbar 按钮。 此处，我们将仅添加 [InkToolbarBallpointPenButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarballpointpenbutton)、[InkToolbarPencilButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton) 和 [InkToolbarEraserButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbareraserbutton)。
> [!NOTE]
> 按钮将按照框架定义的顺序（而不是此处指定的顺序）添加到工具栏。

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <!-- Clear the default InkToolbar buttons by setting InitialControls to None. -->
        <!-- Set the active tool to the pencil button. -->
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
                    VerticalAlignment="Top"
                    TargetInkCanvas="{x:Bind inkCanvas}"
                    InitialControls="None">
            <!--
             Add only the ballpoint pen, pencil, and eraser.
             Note that the buttons are added to the toolbar in the order
             defined by the framework, not the order we specify here.
            -->
            <InkToolbarEraserButton />
            <InkToolbarBallpointPenButton />
            <InkToolbarPencilButton/>
        </InkToolbar>
    </Grid>
</Grid>
```

**代码隐藏**
1. 为第一个示例中的 InkCanvas 和 InkToolbar 使用 XAML 声明。

  ```xaml
  <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <Grid.RowDefinitions>
          <RowDefinition Height="Auto"/>
          <RowDefinition Height="*"/>
      </Grid.RowDefinitions>
      <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
          <TextBlock x:Name="Header"
                     Text="Basic ink sample"
                     Style="{ThemeResource HeaderTextBlockStyle}"
                     Margin="10,0,0,0" />
      </StackPanel>
      <Grid Grid.Row="1">
          <Image Source="Assets\StoreLogo.png" />
          <InkCanvas x:Name="inkCanvas" />
          <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
      </Grid>
  </Grid>
  ```

2. 在代码隐藏中，为 [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loading) 对象的 [Loading](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 事件设置处理程序。

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loading += inkToolbar_Loading;
  }
  ```

3. 将 [InitialControls](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar.initialcontrols) 设置为“[None](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarinitialcontrols)”。
4. 为你的应用所需的按钮创建对象引用。 此处，我们将仅添加 [InkToolbarBallpointPenButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarballpointpenbutton)、[InkToolbarPencilButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton) 和 [InkToolbarEraserButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbareraserbutton)。
  > [!NOTE]
  > 按钮将按照框架定义的顺序（而不是此处指定的顺序）添加到工具栏。

5. 将这些按钮[添加](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobjectcollection.add)到 InkToolbar。

  ```csharp
  /// <summary>
  /// Handles the Loading event of the InkToolbar.
  /// Here, we identify the buttons to include on the InkToolbar.
  /// </summary>
  /// <param name="sender">The InkToolbar</param>
  /// <param name="args">The InkToolbar event data.
  /// If there is no event data, this parameter is null</param>
  private void inkToolbar_Loading(FrameworkElement sender, object args)
  {
      // Clear all built-in buttons from the InkToolbar.
      inkToolbar.InitialControls = InkToolbarInitialControls.None;

      // Add only the ballpoint pen, pencil, and eraser.
      // Note that the buttons are added to the toolbar in the order
      // defined by the framework, not the order we specify here.
      InkToolbarBallpointPenButton ballpoint = new InkToolbarBallpointPenButton();
      InkToolbarPencilButton pencil = new InkToolbarPencilButton();
      InkToolbarEraserButton eraser = new InkToolbarEraserButton();
      inkToolbar.Children.Add(eraser);
      inkToolbar.Children.Add(ballpoint);
      inkToolbar.Children.Add(pencil);
  }
  ```

<!--
### Support touch input
By default, the InkToolbar supports both pen and mouse input, you have to enable support for touch input.
-->

## <a name="custom-buttons-and-inking-features"></a>自定义按钮和墨迹书写功能

你可以自定义和扩展通过 InkToolbar 提供的按钮（以及关联的墨迹书写功能）集合。

InkToolbar 由两组不同的按钮类型组成：

1. 一组“工具”按钮，包含内置绘制、擦除和突出显示按钮。 在此处添加自定义的笔和工具。
> **Note** &nbsp; 注意 &nbsp;功能选择是互斥的。

2. 一组“切换”按钮，包含内置标尺按钮。 在此处添加自定义切换。
> **Note** &nbsp; 注意 &nbsp;功能并不相互排斥，可与其他活动工具同时使用。

根据你的应用程序和所需的墨迹书写功能，你可以将以下任意按钮（绑定到你的自定义墨迹功能）添加到 InkToolbar：

- 自定义笔：由主机应用为其定义墨迹调色板和笔尖属性（如形状、旋转和大小）的笔。
- 自定义工具：非笔工具，由主机应用定义。
- 自定义切换：将应用定义的功能状态设置为开或关。 当打开时，功能将与活动工具结合使用。

> **Note** &nbsp; 注意 &nbsp;不能更改内置按钮的显示顺序。 默认的显示顺序为：圆珠笔、铅笔、荧光笔、橡皮擦和标尺。 自定义笔附加到最后一个默认笔，自定义工具按钮添加到最后一个笔按钮和橡皮擦按钮之间，而自定义切换按钮添加到标尺按钮之后。 （自定义按钮按照指定它们的顺序添加。）

### <a name="custom-pen"></a>自定义笔

你可以在定义墨迹调色板和笔尖属性（如形状、旋转和大小）的位置创建自定义笔（通过自定义笔按钮激活）。

![自定义书法笔按钮](./images/ink/ink-tools-custompen.png)  
*自定义书法笔按钮*

对于此示例，我们定义一个带有宽笔尖的自定义笔，可支持基本书法笔划墨迹。 我们还自定义按钮浮出控件中显示的调色板中的画笔集合。

**代码隐藏**

首先，我们定义自定义笔，并在代码隐藏中指定绘图属性。 稍后，我们从 XAML 引用此自定义笔。

1. 在解决方案资源管理器中右键单击项目，并依次选择“添加”&gt;“新建项目”。
2. 在“Visual C#”-&gt;“代码”下，添加新的类文件，并称它为 CalligraphicPen.cs。
3. 在 Calligraphic.cs 中，将默认 using 块替换为以下内容：
```csharp
using System.Numerics;
using Windows.UI;
using Windows.UI.Input.Inking;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
```

4. 指定 CalligraphicPen 类派生自 [InkToolbarCustomPen](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarcustompen)。
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
}
```

5. 替代 [CreateInkDrawingAttributesCore](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarcustompen.createinkdrawingattributescore) 以指定你自己的画笔和笔划大小。
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
    }
}
```

6. 创建 [InkDrawingAttributes](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkdrawingattributes) 对象，并设置[笔尖形状](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkdrawingattributes.pentip)、[笔尖旋转](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkdrawingattributes.pentiptransform)、[笔划大小](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkdrawingattributes.size)和[墨迹颜色](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkdrawingattributes.color)。
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
        InkDrawingAttributes inkDrawingAttributes =
          new InkDrawingAttributes();
        inkDrawingAttributes.PenTip = PenTipShape.Circle;
        inkDrawingAttributes.Size =
          new Windows.Foundation.Size(strokeWidth, strokeWidth * 20);
        SolidColorBrush solidColorBrush = brush as SolidColorBrush;
        if (solidColorBrush != null)
        {
            inkDrawingAttributes.Color = solidColorBrush.Color;
        }
        else
        {
            inkDrawingAttributes.Color = Colors.Black;
        }

        Matrix3x2 matrix = Matrix3x2.CreateRotation(45);
        inkDrawingAttributes.PenTipTransform = matrix;

        return inkDrawingAttributes;
    }
}
```

**XAML**

接下来，我们在 MainPage.xaml 中将必要的引用添加到自定义笔。

1. 我们声明一个本地页面资源字典，用于创建对 CalligraphicPen.cs 中定义的自定义笔 (`CalligraphicPen`) 的引用和该自定义笔所支持的[画笔集合](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.BrushCollection) (`CalligraphicPenPalette`)。
```xaml
<Page.Resources>
    <!-- Add the custom CalligraphicPen to the page resources. -->
    <local:CalligraphicPen x:Key="CalligraphicPen" />
    <!-- Specify the colors for the palette of the custom pen. -->
    <BrushCollection x:Key="CalligraphicPenPalette">
        <SolidColorBrush Color="Blue" />
        <SolidColorBrush Color="Red" />
    </BrushCollection>
</Page.Resources>
```

2. 然后，我们添加一个带有子 [InkToolbarCustomPenButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarcustompenbutton) 元素的 InkToolbar。

  自定义笔按钮包括页面资源中声明的两个静态资源引用：`CalligraphicPen` 和 `CalligraphicPenPalette`。

  我们还指定笔划大小滑块的范围（[MinStrokeWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.minstrokewidth)、[MaxStrokeWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.maxstrokewidth) 和 [SelectedStrokeWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.selectedstrokewidthproperty)）、选定的画笔 ([SelectedBrushIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.selectedbrushindex)) 和自定义笔按钮的图标 ([SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon))。
```xaml
<Grid Grid.Row="1">
    <InkCanvas x:Name="inkCanvas" />
    <InkToolbar x:Name="inkToolbar"
                VerticalAlignment="Top"
                TargetInkCanvas="{x:Bind inkCanvas}">
        <InkToolbarCustomPenButton
            CustomPen="{StaticResource CalligraphicPen}"
            Palette="{StaticResource CalligraphicPenPalette}"
            MinStrokeWidth="1" MaxStrokeWidth="3" SelectedStrokeWidth="2"
            SelectedBrushIndex ="1">
            <SymbolIcon Symbol="Favorite" />
            <InkToolbarCustomPenButton.ConfigurationContent>
                <InkToolbarPenConfigurationControl />
            </InkToolbarCustomPenButton.ConfigurationContent>
        </InkToolbarCustomPenButton>
    </InkToolbar>
</Grid>
```

### <a name="custom-toggle"></a>自定义切换

你可以创建一个自定义切换（通过自定义切换按钮激活），将应用定义的功能状态设置为开或关。 当打开时，功能将与活动工具结合使用。

在此示例中，我们定义一个启用使用触控输入的墨迹书写的自定义切换按钮（默认情况下，触控墨迹书写不启用）。

> [!NOTE]  
> 如果你需要支持使用触控的墨迹书写，我们建议你使用 CustomToggleButton（附带在此示例中指定的图标和工具提示）启用它。

通常，触控输入用于直接处理对象或应用 UI。 为了演示启用触控墨迹书写时的行为差异，我们将 InkCanvas 放置在 ScrollViewer 容器内，并将 ScrollViewer 尺寸设置为小于 InkCanvas。 

启动应用时，仅支持笔墨迹书写，并使用触控平移或缩放墨迹书写图面。 启用触控墨迹书写时，无法通过触控输入平移或缩放墨迹书写图面。

> [!NOTE]
> 有关 [**InkCanvas**](../controls-and-patterns/inking-controls.md) 和 [**InkToolbar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) UX 指南，请参阅[墨迹书写控件](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkToolbar)。 以下建议与此示例相关：
> - 通常， [**InkToolbar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkToolbar)和墨迹墨迹是通过活动笔体验的。 但是，如果应用需要，可以支持使用鼠标和触控的墨迹书写。 
> - 如果支持使用触控输入的墨迹书写，我们建议为切换按钮使用“Segoe MLD2 Assets”中的“ED5F”图标，并附带“触控书写”工具提示。 

**XAML**

1. 首先，我们声明带有 Click 事件侦听程序的 [**InkToolbarCustomToggleButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkToolbarCustomToggleButton) 元素 (toggleButton)，可指定事件处理程序 (Toggle_Custom)。

```xaml 
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <StackPanel Grid.Row="0" 
                x:Name="HeaderPanel" 
                Orientation="Horizontal">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10" />
    </StackPanel>

    <ScrollViewer Grid.Row="1" 
                  HorizontalScrollBarVisibility="Auto" 
                  VerticalScrollBarVisibility="Auto">
        
        <Grid HorizontalAlignment="Left" VerticalAlignment="Top">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            
            <InkToolbar Grid.Row="0" 
                        Margin="10"
                        x:Name="inkToolbar" 
                        VerticalAlignment="Top"
                        TargetInkCanvas="{x:Bind inkCanvas}">
                <InkToolbarCustomToggleButton 
                x:Name="toggleButton" 
                Click="CustomToggle_Click" 
                ToolTipService.ToolTip="Touch Writing">
                    <SymbolIcon Symbol="{x:Bind TouchWritingIcon}"/>
                </InkToolbarCustomToggleButton>
            </InkToolbar>
            
            <ScrollViewer Grid.Row="1" 
                          Height="500"
                          Width="500"
                          x:Name="scrollViewer" 
                          ZoomMode="Enabled" 
                          MinZoomFactor=".1" 
                          VerticalScrollMode="Enabled" 
                          VerticalScrollBarVisibility="Auto" 
                          HorizontalScrollMode="Enabled" 
                          HorizontalScrollBarVisibility="Auto">
                
                <Grid x:Name="outputGrid" 
                      Height="1000"
                      Width="1000"
                      Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}">
                    <InkCanvas x:Name="inkCanvas"/>
                </Grid>
                
            </ScrollViewer>
        </Grid>
    </ScrollViewer>
</Grid>
```

**代码隐藏**

2. 在上面的代码片段中，我们在自定义切换按钮上为触控墨迹书写 (toggleButton) 声明了 Click 事件侦听程序和处理程序 (Toggle_Custom)。 此处理程序仅通过 InkPresenter 的 InputDeviceTypes 属性切换对 CoreInputDeviceTypes.Touch 的支持。

   我们还使用 SymbolIcon 元素和 {x:Bind} 标记扩展（可将它绑定到在代码隐藏文件 (TouchWritingIcon) 中定义的字段）为按钮指定了图标。

   以下代码片段包括 Click 事件处理程序和 TouchWritingIcon 的定义。

```csharp 
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomToggle : Page
    {
        Symbol TouchWritingIcon = (Symbol)0xED5F;

        public MainPage_AddCustomToggle()
        {
            this.InitializeComponent();
        }

        // Handler for the custom toggle button that enables touch inking.
        private void CustomToggle_Click(object sender, RoutedEventArgs e)
        {
            if (toggleButton.IsChecked == true)
            {
                inkCanvas.InkPresenter.InputDeviceTypes |= CoreInputDeviceTypes.Touch;
            }
            else
            {
                inkCanvas.InkPresenter.InputDeviceTypes &= ~CoreInputDeviceTypes.Touch;
            }
        }
    }
}
```

### <a name="custom-tool"></a>自定义工具

你可以创建自定义工具按钮，调用由你的应用定义的非笔工具。

默认情况下，[**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) 将所有输入作为笔划墨迹或擦除笔划进行处理。 这包括由辅助硬件提示（例如笔桶按钮、鼠标右键按钮或类似提示）修改的输入。 但是，[**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) 可以配置为不处理特定输入，它随后可传递到你的应用以供自定义处理。

在此示例中，我们定义自定义工具按钮，选中该按钮后，后续笔划将进行处理并呈现为所选套索（虚线）而非墨迹。 所选区域边界内的所有笔划墨迹都设置为[**已选中**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstroke.selected)。

> [!NOTE]
> 有关 InkCanvas 和 InkToolbar UX 指南，请参阅墨迹书写控件。 以下建议与此示例相关：
> - 如果提供笔划选择，我们建议为工具按钮使用“Segoe MLD2 Assets”字体中的“EF20”图标，并附带“选择工具”工具提示。 
 
**XAML**

1. 首先，我们声明带有 Click 事件侦听程序的 [**InkToolbarCustomToolButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) 元素 (customToolButton)，可在配置笔划选择的情况下指定事件处理程序 (customToolButton_Click)。 （我们还添加了一组用于复制、剪切和粘贴笔划选择的按钮。）

2. 我们还添加用于绘制选择笔划的 Canvas 元素。 使用单独的图层绘制选择笔划可确保 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 及其内容保持不变。 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10,0,0,0" />
    </StackPanel>
    <StackPanel x:Name="ToolPanel" Orientation="Horizontal" Grid.Row="1">
        <InkToolbar x:Name="inkToolbar" 
                    VerticalAlignment="Top" 
                    TargetInkCanvas="{x:Bind inkCanvas}">
            <InkToolbarCustomToolButton 
                x:Name="customToolButton" 
                Click="customToolButton_Click" 
                ToolTipService.ToolTip="Selection tool">
                <SymbolIcon Symbol="{x:Bind SelectIcon}"/>
            </InkToolbarCustomToolButton>
        </InkToolbar>
        <Button x:Name="cutButton" 
                Content="Cut" 
                Click="cutButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="copyButton" 
                Content="Copy"  
                Click="copyButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="pasteButton" 
                Content="Paste"  
                Click="pasteButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
    </StackPanel>
    <Grid Grid.Row="2" x:Name="outputGrid" 
              Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}" 
              Height="Auto">
        <!-- Canvas for displaying selection UI. -->
        <Canvas x:Name="selectionCanvas"/>
        <!-- Canvas for displaying ink. -->
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

**代码隐藏**

2. 然后在 MainPage.xaml.cs 代码隐藏文件中处理 [**InkToolbarCustomToolButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) 的 Click 事件。

   此处理程序配置 [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter)，以将未处理的输入传递到应用。 

   有关此代码的更详细步骤，请参阅[windows 应用中的笔交互和 Windows 墨迹](pen-and-stylus-interactions.md)的传递输入 "高级处理" 部分。

   我们还使用 SymbolIcon 元素和 {x:Bind} 标记扩展（可将它绑定到在代码隐藏文件 (SelectIcon) 中定义的字段）为按钮指定了图标。

   以下代码片段包括 Click 事件处理程序和 SelectIcon 的定义。

```csharp
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomTool : Page
    {
        // Icon for custom selection tool button.
        Symbol SelectIcon = (Symbol)0xEF20;

        // Stroke selection tool.
        private Polyline lasso;
        // Stroke selection area.
        private Rect boundingRect;

        public MainPage_AddCustomTool()
        {
            this.InitializeComponent();

            // Listen for new ink or erase strokes to clean up selection UI.
            inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
                StrokeInput_StrokeStarted;
            inkCanvas.InkPresenter.StrokesErased +=
                InkPresenter_StrokesErased;
        }

        private void customToolButton_Click(object sender, RoutedEventArgs e)
        {
            // By default, the InkPresenter processes input modified by 
            // a secondary affordance (pen barrel button, right mouse 
            // button, or similar) as ink.
            // To pass through modified input to the app for custom processing 
            // on the app UI thread instead of the background ink thread, set 
            // InputProcessingConfiguration.RightDragAction to LeaveUnprocessed.
            inkCanvas.InkPresenter.InputProcessingConfiguration.RightDragAction =
                InkInputRightDragAction.LeaveUnprocessed;

            // Listen for unprocessed pointer events from modified input.
            // The input is used to provide selection functionality.
            inkCanvas.InkPresenter.UnprocessedInput.PointerPressed +=
                UnprocessedInput_PointerPressed;
            inkCanvas.InkPresenter.UnprocessedInput.PointerMoved +=
                UnprocessedInput_PointerMoved;
            inkCanvas.InkPresenter.UnprocessedInput.PointerReleased +=
                UnprocessedInput_PointerReleased;
        }

        // Handle new ink or erase strokes to clean up selection UI.
        private void StrokeInput_StrokeStarted(
            InkStrokeInput sender, Windows.UI.Core.PointerEventArgs args)
        {
            ClearSelection();
        }

        private void InkPresenter_StrokesErased(
            InkPresenter sender, InkStrokesErasedEventArgs args)
        {
            ClearSelection();
        }

        private void cutButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
            inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            ClearSelection();
        }

        private void copyButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
        }

        private void pasteButton_Click(object sender, RoutedEventArgs e)
        {
            if (inkCanvas.InkPresenter.StrokeContainer.CanPasteFromClipboard())
            {
                inkCanvas.InkPresenter.StrokeContainer.PasteFromClipboard(
                    new Point(0, 0));
            }
            else
            {
                // Cannot paste from clipboard.
            }
        }

        // Clean up selection UI.
        private void ClearSelection()
        {
            var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
            foreach (var stroke in strokes)
            {
                stroke.Selected = false;
            }
            ClearBoundingRect();
        }

        private void ClearBoundingRect()
        {
            if (selectionCanvas.Children.Any())
            {
                selectionCanvas.Children.Clear();
                boundingRect = Rect.Empty;
            }
        }

        // Handle unprocessed pointer events from modifed input.
        // The input is used to provide selection functionality.
        // Selection UI is drawn on a canvas under the InkCanvas.
        private void UnprocessedInput_PointerPressed(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Initialize a selection lasso.
            lasso = new Polyline()
            {
                Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                StrokeThickness = 1,
                StrokeDashArray = new DoubleCollection() { 5, 2 },
            };

            lasso.Points.Add(args.CurrentPoint.RawPosition);

            selectionCanvas.Children.Add(lasso);
        }

        private void UnprocessedInput_PointerMoved(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Add a point to the lasso Polyline object.
            lasso.Points.Add(args.CurrentPoint.RawPosition);
        }

        private void UnprocessedInput_PointerReleased(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Add the final point to the Polyline object and 
            // select strokes within the lasso area.
            // Draw a bounding box on the selection canvas 
            // around the selected ink strokes.
            lasso.Points.Add(args.CurrentPoint.RawPosition);

            boundingRect =
                inkCanvas.InkPresenter.StrokeContainer.SelectWithPolyLine(
                    lasso.Points);

            DrawBoundingRect();
        }

        // Draw a bounding rectangle, on the selection canvas, encompassing 
        // all ink strokes within the lasso area.
        private void DrawBoundingRect()
        {
            // Clear all existing content from the selection canvas.
            selectionCanvas.Children.Clear();

            // Draw a bounding rectangle only if there are ink strokes 
            // within the lasso area.
            if (!((boundingRect.Width == 0) ||
                (boundingRect.Height == 0) ||
                boundingRect.IsEmpty))
            {
                var rectangle = new Rectangle()
                {
                    Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                    StrokeThickness = 1,
                    StrokeDashArray = new DoubleCollection() { 5, 2 },
                    Width = boundingRect.Width,
                    Height = boundingRect.Height
                };

                Canvas.SetLeft(rectangle, boundingRect.X);
                Canvas.SetTop(rectangle, boundingRect.Y);

                selectionCanvas.Children.Add(rectangle);
            }
        }
    }
}
```



### <a name="custom-ink-rendering"></a>自定义墨迹呈现

默认情况下，墨迹输入在低延迟后台线程上进行处理，并在绘制时呈现“墨迹未干”。 笔划完成时（抬起笔或手指，或者释放鼠标按钮），笔划将在 UI 线程上进行处理并向 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 图层呈现“墨迹已干”（在应用程序内容之上，并且替换未干墨迹）。

墨迹平台支持你替代此行为并通过自定义烘干墨迹输入来完全自定义墨迹书写体验。

有关自定义晾干的详细信息，请参阅[windows 应用中的笔交互和 Windows 墨迹](https://docs.microsoft.com/windows/uwp/design/input/pen-and-stylus-interactions#custom-ink-rendering)。

> [!NOTE]
> 自定义烘干和 [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)  
> 如果你的应用将 [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) 的默认墨迹呈现行为替代为自定义烘干实现，呈现的笔划墨迹不再可用于 InkToolbar，并且 InkToolbar 的内置擦除命令不会按预期工作。 若要提供擦除功能，必须处理所有指针事件、在每个笔划上执行命中测试，并替代内置的“清除所有墨迹”命令。

## <a name="related-articles"></a>相关文章

- [笔和触笔交互](pen-and-stylus-interactions.md)

### <a name="topic-samples"></a>主题示例

- [墨迹工具栏位置和方向示例（基本）](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)
- [墨迹工具栏位置和方向示例（动态）](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)

### <a name="other-samples"></a>其他示例

- [简单墨迹示例 (C#/C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
- [复杂墨迹示例 (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
- [墨迹示例 (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
- [入门教程： Windows 应用中的支持墨迹](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
- [Coloring Book 示例](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [系列说明示例](https://github.com/Microsoft/Windows-appsample-familynotes)
