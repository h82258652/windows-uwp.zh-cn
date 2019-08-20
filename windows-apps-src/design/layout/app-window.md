---
Description: 使用 AppWindow 类在单独的窗口中查看应用程序的不同部分。
title: 使用 AppWindow 类显示应用的辅助窗口
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9b89d9100157cf40266bb983e258aa187f65dc93
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867466"
---
# <a name="show-multiple-views-with-appwindow"></a>显示具有 AppWindow 的多个视图

[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)及其相关 api 通过让你在辅助窗口中显示应用内容, 并在每个窗口中同时处理同一 UI 线程, 简化了多窗口应用的创建。

> [!NOTE]
> AppWindow 目前处于预览阶段。 这意味着你可以将使用 AppWindow 的应用提交到应用商店, 但知道某些平台和框架组件不能使用 AppWindow (请参阅[限制](/uwp/api/windows.ui.windowmanagement.appwindow#limitations))。

在这里, 我们将显示多个窗口的一些方案, 其中`HelloAppWindow`包含一个名为的示例应用。 示例应用演示了以下功能:

- 从主页取消停靠控件并在新窗口中打开它。
- 在新窗口中打开页面的新实例。
- 在应用程序中以编程方式调整新窗口的大小和位置。
- 将 ContentDialog 与应用程序中的相应窗口关联。

![使用单个窗口的示例应用程序](images/hello-app-window-single.png)
  
> _使用单个窗口的示例应用程序_

![具有未停靠颜色选取器和辅助窗口的示例应用](images/hello-app-window-multi.png)

> _具有未停靠颜色选取器和辅助窗口的示例应用_

> **重要的 API**：[WindowManagement 命名空间](/uwp/api/windows.ui.windowmanagement), [AppWindow 类](/uwp/api/windows.ui.windowmanagement.appwindow)

## <a name="api-overview"></a>API 概述

从 Windows 10 版本 1903 (SDK 18362) 开始, [WindowManagement](/uwp/api/windows.ui.windowmanagement)命名空间中的[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)类和其他 api 可用。 如果你的应用面向 Windows 10 的早期版本, 则必须[使用 w 创建辅助窗口](application-view.md)。 WindowManagement Api 仍处于开发阶段, 并且具有 API 参考文档中所述的[限制](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)。

下面是一些重要的 Api, 可用于在 AppWindow 中显示内容。

### <a name="appwindow"></a>AppWindow

[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)类可用于在辅助窗口中显示部分 Windows 运行时应用。 它在概念上类似于[w](/uwp/api/windows.ui.viewmanagement.applicationview), 但行为和生存期不相同。 AppWindow 的一项主要功能是, 每个实例共享从中创建它们的同一 UI 处理线程 (包括事件调度程序), 从而简化了多窗口应用。

只能将 XAML 内容连接到 AppWindow, 不支持本机 DirectX 或全息内容。 但是, 您可以显示一个承载 DirectX 内容的 XAML [SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel) 。

### <a name="windowingenvironment"></a>WindowingEnvironment

利用[WindowingEnvironment](/uwp/api/windows.ui.windowmanagement.windowingenvironment) API, 你可以了解应用程序的提供环境, 以便可以根据需要调整应用。 它介绍了环境支持的窗口类型;例如, `Overlapped`如果应用在计算机上运行, 或者`Tiled`如果应用在 Xbox 上运行, 则为。 它还提供了一组 DisplayRegion 对象, 这些对象描述可在逻辑显示器上显示应用的区域。

### <a name="displayregion"></a>DisplayRegion

[DisplayRegion](/uwp/api/windows.ui.windowmanagement.displayregion) API 描述可在逻辑显示器上向用户显示视图的区域;例如, 在台式计算机上, 这是完全显示, 而不是任务栏的面积。 它不一定是使用后备监视器的物理显示区域的1:1 映射。 同一监视器中可以有多个显示区域, 如果这些监视器在所有方面都是相同的, 则可以将 DisplayRegion 配置为跨多个监视器。

### <a name="appwindowpresenter"></a>AppWindowPresenter

借助[AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter) API, 可以轻松地将 windows 切换到预定义的`FullScreen`配置`CompactOverlay`(如或)。 这些配置可让用户在任何支持配置的设备上保持一致的体验。

### <a name="uicontext"></a>UIContext

[UIContext](/uwp/api/windows.ui.uicontext)是应用程序窗口或视图的唯一标识符。 它是自动创建的, 您可以使用[UIContext](/uwp/api/windows.ui.xaml.uielement.uicontext)属性来检索 UIContext。 XAML 树中的每个 UIElement 都具有相同的 UIContext。

 UIContext 非常重要, 因为类似于[Window](/uwp/api/Windows.UI.Xaml.Window.Current)的`GetForCurrentView` api 和模式依赖于具有单个 w/CoreWindow, 每个线程都需要一个 XAML 树来处理。 当你使用 AppWindow 时, 不会出现这种情况, 因此你可以使用 UIContext 来识别特定的窗口。

### <a name="xamlroot"></a>XamlRoot

[XamlRoot](/uwp/api/windows.ui.xaml.xamlroot)类包含 XAML 元素树, 并将其连接到窗口宿主对象 (例如, [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)或[w](/uwp/api/windows.ui.viewmanagement.applicationview)), 并提供大小和可见性等信息。 不会直接创建 XamlRoot 对象。 而是在将 XAML 元素附加到 AppWindow 时创建一个。 然后, 可以使用[XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot)属性来检索 XamlRoot。

有关 UIContext 和 XamlRoot 的详细信息, 请参阅[使代码可在窗口化主机之间移植](show-multiple-views.md#make-code-portable-across-windowing-hosts)。

## <a name="show-a-new-window"></a>显示新窗口

让我们看看在新的 AppWindow 中显示内容的步骤。

**显示新窗口**

1. 调用静态[AppWindow TryCreateAsync](/uwp/api/windows.ui.windowmanagement.appwindow.trycreateasync)方法来创建新的[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)。

    ```csharp
    AppWindow appWindow = await AppWindow.TryCreateAsync();
    ```

1. 创建窗口内容。

    通常, 你创建 XAML[帧](/uwp/api/Windows.UI.Xaml.Controls.Frame), 然后将该框架导航到 xaml[页面](/uwp/api/Windows.UI.Xaml.Controls.Page), 你已在其中定义应用内容。 有关框架和页面的详细信息, 请参阅[两个页面之间的对等导航](../basics/navigate-between-two-pages.md)。

    ```csharp
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowMainPage));
    ```

    但是, 可以在 AppWindow 中显示任何 XAML 内容, 而不仅仅是框架和页面。 例如, 可以只显示单个控件 (如[ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker)), 也可以显示承载 DirectX 内容的[SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel) 。

1. 调用[ElementCompositionPreview. SetAppWindowContent](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent)方法将 XAML 内容附加到 AppWindow。

    ```csharp
    ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);
    ```

    对此方法的调用会创建一个[XamlRoot](/uwp/api/windows.ui.xaml.xamlroot)对象, 并将其设置为指定 UIElement 的[XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot)属性。

    每个 AppWindow 实例只能调用一次此方法。 设置内容后, 对此 AppWindow 实例的 SetAppWindowContent 的进一步调用将失败。 此外, 如果尝试通过传入 null UIElement 对象断开 AppWindow 内容的连接, 则调用将失败。

1. 调用[AppWindow TryShowAsync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync)方法以显示新窗口。

    ```csharp
    await appWindow.TryShowAsync();
    ```

## <a name="release-resources-when-a-window-is-closed"></a>关闭窗口时释放资源

应始终处理[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow.closed)事件, 以释放 XAML 资源 (AppWindow 内容) 和对 AppWindow 的引用。

```csharp
appWindow.Closed += delegate
{
    appWindowContentFrame.Content = null;
    appWindow = null;
};
```

## <a name="track-instances-of-appwindow"></a>跟踪 AppWindow 的实例

根据你在应用程序中使用多个窗口的方式, 你可能需要也可能不需要跟踪创建的 AppWindow 实例。 该`HelloAppWindow`示例演示了一些通常使用[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)的不同方法。 在这里, 我们将探讨应跟踪这些窗口的原因, 以及如何执行此操作。

### <a name="simple-tracking"></a>简单跟踪

颜色选取器窗口承载单个 XAML 控件, 而与颜色选取器进行交互的代码全部驻留在`MainPage.xaml.cs`文件中。 颜色选取器窗口仅允许单个实例, 本质上是的`MainWindow`扩展。 若要确保只创建一个实例, 则使用页面级别变量跟踪颜色选取器窗口。 在创建新的颜色选取器窗口之前, 请先检查实例是否存在, 如果存在, 则跳过创建新窗口的步骤, 只需在现有窗口上调用[TryShowAsync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync) 。

```csharp
AppWindow colorPickerAppWindow;

// ...

private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
{
    // Create the color picker window.
    if (colorPickerAppWindow == null)
    {
        // ...
        // Create a new window
        colorPickerAppWindow = await AppWindow.TryCreateAsync();
        // ...
    }
    // Show the window.
    await colorPickerAppWindow.TryShowAsync();
}
```

### <a name="track-an-appwindow-instance-in-its-hosted-content"></a>跟踪其托管内容中的 AppWindow 实例

窗口承载完整的 XAML 页, 与页交互的代码位于中`AppWindowPage.xaml.cs`。 `AppWindowPage` 它允许多个实例, 每个实例单独运行。

页面功能使你可以操作窗口、将其设置为`FullScreen`或`CompactOverlay`, 还可以侦听[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow.changed)事件以显示有关窗口的信息。 若要调用这些 api, `AppWindowPage`需要引用托管它的 AppWindow 实例。

如果需要, 可以在中`AppWindowPage`创建一个属性, 并在创建时将[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)实例分配给它。

**AppWindowPage.xaml.cs**

在`AppWindowPage`中, 创建用于保存 AppWindow 引用的属性。

```csharp
public sealed partial class AppWindowPage : Page
{
    public AppWindow MyAppWindow { get; set; }

    // ...
}
```

**MainPage.xaml.cs**

在`MainPage`中, 获取对页面实例的引用, 并将新创建的 AppWindow 分配到中`AppWindowPage`的属性。

```csharp
private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
{
    // Create a new window.
    AppWindow appWindow = await AppWindow.TryCreateAsync();

    // Create a Frame and navigate to the Page you want to show in the new window.
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowPage));

    // Get a reference to the page instance and assign the
    // newly created AppWindow to the MyAppWindow property.
    AppWindowPage page = (AppWindowPage)appWindowContentFrame.Content;
    page.MyAppWindow = appWindow;

    // ...
}
```

### <a name="tracking-app-windows-using-uicontext"></a>使用 UIContext 跟踪应用程序窗口

你可能还想要从应用的其他部分访问[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)实例。 例如, `MainPage`可以有一个 "全部关闭" 按钮, 用于关闭所有跟踪的 AppWindow 实例。

在这种情况下, 应使用[UIContext](/uwp/api/windows.ui.uicontext)唯一标识符来跟踪[字典](/dotnet/api/system.collections.generic.dictionary-2?view=dotnet-uwp-10.0)中的窗口实例。

**MainPage.xaml.cs**

在`MainPage`中, 将字典创建为静态属性。 然后, 在创建该页面时将其添加到字典中, 并在页面关闭时将其删除。 调用[ElementCompositionPreview](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent)后, 可以从内容[框架](/uwp/api/Windows.UI.Xaml.Controls.Frame)(`appWindowContentFrame.UIContext`) 获取 UIContext。

```csharp
public sealed partial class MainPage : Page
{
    // Track open app windows in a Dictionary.
    public static Dictionary<UIContext, AppWindow> AppWindows { get; set; }
        = new Dictionary<UIContext, AppWindow>();

    // ...

    private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
    {
        // Create a new window.
        AppWindow appWindow = await AppWindow.TryCreateAsync();

        // Create a Frame and navigate to the Page you want to show in the new window.
        Frame appWindowContentFrame = new Frame();
        appWindowContentFrame.Navigate(typeof(AppWindowPage));

        // Attach the XAML content to the window.
        ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);

        // Add the new page to the Dictionary using the UIContext as the Key.
        AppWindows.Add(appWindowContentFrame.UIContext, appWindow);
        appWindow.Title = "App Window " + AppWindows.Count.ToString();

        // When the window is closed, be sure to release
        // XAML resources and the reference to the window.
        appWindow.Closed += delegate
        {
            MainPage.AppWindows.Remove(appWindowContentFrame.UIContext);
            appWindowContentFrame.Content = null;
            appWindow = null;
        };

        // Show the window.
        await appWindow.TryShowAsync();
    }

    private async void CloseAllButton_Click(object sender, RoutedEventArgs e)
    {
        while (AppWindows.Count > 0)
        {
            await AppWindows.Values.First().CloseAsync();
        }
    }
    // ...
}
```

**AppWindowPage.xaml.cs**

若要在 `AppWindowPage`代码中使用 [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) 实例, 请使用页的[UIContext](/uwp/api/windows.ui.uicontext)在中`MainPage`从静态字典检索它。 应在页面的[加载](/uwp/api/windows.ui.xaml.frameworkelement.loaded)事件处理程序而不是构造函数中执行此操作, 以使 UIContext 不为 null。 可以从页面获取 UIContext: `this.UIContext`。

```csharp
public sealed partial class AppWindowPage : Page
{
    AppWindow window;

    // ...
    public AppWindowPage()
    {
        this.InitializeComponent();

        Loaded += AppWindowPage_Loaded;
    }

    private void AppWindowPage_Loaded(object sender, RoutedEventArgs e)
    {
        // Get the reference to this AppWindow that was stored when it was created.
        window = MainPage.AppWindows[this.UIContext];

        // Set up event handlers for the window.
        window.Changed += Window_Changed;
    }
    // ...
}
```

> [!NOTE]
> 该`HelloAppWindow`示例演示了在中`AppWindowPage`跟踪窗口的两种方法, 但通常使用其中一种方法, 而不是同时使用这两种方法。

## <a name="request-window-size-and-placement"></a>请求窗口的大小和位置

[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)类有几种方法可用于控制窗口的大小和位置。 如方法名称所隐含, 系统根据环境因素可能会或不遵循所请求的更改。

调用[RequestSize](/uwp/api/windows.ui.windowmanagement.appwindow.requestsize)以指定所需的窗口大小, 如下所示。

```csharp
colorPickerAppWindow.RequestSize(new Size(300, 428));
```

用于管理窗口位置的方法名为_RequestMove *_ :[RequestMoveAdjacentToCurrentView](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttocurrentview)、 [RequestMoveAdjacentToWindow](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttowindow)、 [RequestMoveRelativeToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoverelativetodisplayregion)、 [RequestMoveToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmovetodisplayregion)。

在此示例中, 此代码将窗口移到从中产生窗口的主视图旁边。

```csharp
colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
```

若要获取有关窗口的当前大小和位置的信息, 请调用[GetPlacement](/uwp/api/windows.ui.windowmanagement.appwindow.getplacement)。 这会返回一个[AppWindowPlacement](/uwp/api/windows.ui.windowmanagement.appwindowplacement)对象, 该对象提供窗口的当前[DisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindowplacement.displayregion)、[偏移量](/uwp/api/windows.ui.windowmanagement.appwindowplacement.offset)和[大小](/uwp/api/windows.ui.windowmanagement.appwindowplacement.size)。

例如, 可以调用此代码将窗口移到显示的右上角。 此代码必须在窗口显示后调用;否则, 由对 GetPlacement 的调用返回的窗口大小将是 0, 0, 偏移量将不正确。

```csharp
DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
double displayRegionWidth = displayRegion.WorkAreaSize.Width;
double windowWidth = window.GetPlacement().Size.Width;
int horizontalOffset = (int)(displayRegionWidth - windowWidth);
window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
```

## <a name="request-a-presentation-configuration"></a>请求演示配置

使用[AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter)类, 你可以使用适用于所显示设备的预定义配置来显示[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) 。 可以使用[AppWindowPresentationConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresentationconfiguration)值将窗口`FullScreen`置于或`CompactOverlay`模式。

此示例演示如何执行以下操作:

- 使用[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow.changed)事件可在可用窗口呈现发生变化时得到通知。
- 使用[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow.presenter)属性获取当前[AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter)。
- 调用[IsPresentationSupported](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.ispresentationsupported)可查看是否支持特定的[AppWindowPresentationKind](/uwp/api/windows.ui.windowmanagement.appwindowpresentationkind) 。
- 调用[GetConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.getconfiguration)来检查当前使用的配置类型。
- 调用[RequestPresentation](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.requestpresentation)以更改当前配置。

```csharp
private void Window_Changed(AppWindow sender, AppWindowChangedEventArgs args)
{
    if (args.DidAvailableWindowPresentationsChange)
    {
        EnablePresentationButtons(sender);
    }

    if (args.DidWindowPresentationChange)
    {
        ConfigText.Text = window.Presenter.GetConfiguration().Kind.ToString();
    }

    if (args.DidSizeChange)
    {
        SizeText.Text = window.GetPlacement().Size.ToString();
    }
}

private void EnablePresentationButtons(AppWindow window)
{
    // Check whether the current AppWindowPresenter supports CompactOverlay.
    if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.CompactOverlay))
    {
        // Show the CompactOverlay button...
        compactOverlayButton.Visibility = Visibility.Visible;
    }
    else
    {
        // Hide the CompactOverlay button...
        compactOverlayButton.Visibility = Visibility.Collapsed;
    }

    // Check whether the current AppWindowPresenter supports FullScreen?
    if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.FullScreen))
    {
        // Show the FullScreen button...
        fullScreenButton.Visibility = Visibility.Visible;
    }
    else
    {
        // Hide the FullScreen button...
        fullScreenButton.Visibility = Visibility.Collapsed;
    }
}

private void CompactOverlayButton_Click(object sender, RoutedEventArgs e)
{
    if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.CompactOverlay)
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.CompactOverlay);
        fullScreenButton.IsChecked = false;
    }
    else
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
    }
}

private void FullScreenButton_Click(object sender, RoutedEventArgs e)
{
    if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.FullScreen)
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.FullScreen);
        compactOverlayButton.IsChecked = false;
    }
    else
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
    }
}
```

## <a name="reuse-xaml-elements"></a>重用 XAML 元素

[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)使你可以有多个具有同一 UI 线程的 XAML 树。 但是, XAML 元素只能添加到一次 XAML 树中。 如果要将部分 UI 从一个窗口移到另一个窗口, 则必须管理它在 XAML 树中的位置。

此示例演示如何在主窗口和辅助窗口之间移动[ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker)控件时重用它。

在的 xaml `MainPage`中声明了颜色选取器, 后者将其置于`MainPage` xaml 树中。

```xaml
<StackPanel x:Name="colorPickerContainer" Grid.Column="1" Background="WhiteSmoke">
    <Button Click="DetachColorPickerButton_Click" HorizontalAlignment="Right">
        <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE2B4;" />
    </Button>
    <ColorPicker x:Name="colorPicker" Margin="12" Width="288"
                 IsColorChannelTextInputVisible="False"
                 ColorChanged="ColorPicker_ColorChanged"/>
</StackPanel>
```

当颜色选取器分离以放置在新的 AppWindow 中时, 您首先必须从`MainPage` XAML 树中删除该颜色选取器, 方法是将其从其父容器中删除。 尽管这不是必需的, 但本示例还隐藏了父容器。

```csharp
colorPickerContainer.Children.Remove(colorPicker);
colorPickerContainer.Visibility = Visibility.Collapsed;
```

然后, 可以将其添加到新的 XAML 树中。 在这里, 您首先创建一个将作为 ColorPicker 的父容器的[网格](/uwp/api/windows.ui.xaml.controls.grid), 并将 ColorPicker 添加为网格的子元素。 (这样, 以后便可以轻松地从此 XAML 树中删除 ColorPicker。)然后, 在新窗口中将网格设置为 XAML 树的根。

```csharp
Grid appWindowRootGrid = new Grid();
appWindowRootGrid.Children.Add(colorPicker);

// Create a new window
colorPickerAppWindow = await AppWindow.TryCreateAsync();

// Attach the XAML content to our window
ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);
```

关闭[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)时, 将撤消此过程。 首先, 从[网格](/uwp/api/windows.ui.xaml.controls.grid)中删除[ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker) , 然后将其作为[system.windows.controls.stackpanel>](/uwp/api/windows.ui.xaml.controls.stackpanel)的子元素添加到中`MainPage`。

```csharp
// When the window is closed, be sure to release XAML resources
// and the reference to the window.
colorPickerAppWindow.Closed += delegate
{
    appWindowRootGrid.Children.Remove(colorPicker);
    appWindowRootGrid = null;
    colorPickerAppWindow = null;

    colorPickerContainer.Children.Add(colorPicker);
    colorPickerContainer.Visibility = Visibility.Visible;
};
```

```csharp
private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
{
    ColorPickerContainer.Visibility = Visibility.Collapsed;

    // Create the color picker window.
    if (colorPickerAppWindow == null)
    {
        ColorPickerContainer.Children.Remove(colorPicker);

        Grid appWindowRootGrid = new Grid();
        appWindowRootGrid.Children.Add(colorPicker);

        // Create a new window
        colorPickerAppWindow = await AppWindow.TryCreateAsync();
        colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
        colorPickerAppWindow.RequestSize(new Size(300, 428));
        colorPickerAppWindow.Title = "Color picker";

        // Attach the XAML content to our window
        ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);

        // When the window is closed, be sure to release XAML resources
        // and the reference to the window.
        colorPickerAppWindow.Closed += delegate
        {
            appWindowRootGrid.Children.Remove(colorPicker);
            appWindowRootGrid = null;
            colorPickerAppWindow = null;

            ColorPickerContainer.Children.Add(colorPicker);
            ColorPickerContainer.Visibility = Visibility.Visible;
        };
    }
    // Show the window.
    await colorPickerAppWindow.TryShowAsync();
}
```

## <a name="show-a-dialog-box"></a>显示对话框

默认情况下，内容对话框相对于根 [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) 按模式显示。 在[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)中使用[ContentDialog](/uwp/api/windows.ui.xaml.controls.contentdialog)时, 需要将对话框上的 XAMLROOT 手动设置为 XAML 主机的根。

为此, 请将 ContentDialog 的[XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot)属性设置为与 AppWindow 中的元素相同的[XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) 。 此处, 此代码位于按钮的[单击](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)事件处理程序内, 因此可以使用_发送方_(单击的按钮) 获取 XamlRoot。

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
{
    simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
}
```

如果除了主窗口 (w) 外, 还存在一个或多个 AppWindows, 则每个窗口都可以尝试打开一个对话框, 因为模式对话框仅会阻止它所根的窗口。 但是, 每个线程一次只能打开一个[ContentDialog](/uwp/api/windows.ui.xaml.controls.contentdialog) 。 尝试打开两个 ContentDialog 会引发异常，即使尝试在独立的 AppWindow 中打开。

若要管理此项, 应该至少在`try/catch`块中打开对话框以捕获异常, 以防已经打开了另一个对话框。

```csharp
try
{
    ContentDialogResult result = await simpleDialog.ShowAsync();
}
catch (Exception)
{
    // The dialog didn't open, probably because another dialog is already open.
}
```

管理对话的另一种方法是跟踪当前打开的对话框, 并在尝试打开新对话框之前将其关闭。 此处, 将创建一个为此目的`MainPage`而`CurrentDialog`调用的静态属性。

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

   // ...
}
```

然后, 检查是否有当前打开的对话框, 如果有, 请调用[Hide](/uwp/api/windows.ui.xaml.controls.contentdialog.hide)方法将其关闭。 最后, 将新对话框分配给`CurrentDialog`, 并尝试显示它。

```csharp
private async void DialogButton_Click(object sender, RoutedEventArgs e)
{
    ContentDialog simpleDialog = new ContentDialog
    {
        Title = "Content dialog",
        Content = "Dialog box for " + window.Title,
        CloseButtonText = "Ok"
    };

    if (MainPage.CurrentDialog != null)
    {
        MainPage.CurrentDialog.Hide();
    }
    MainPage.CurrentDialog = simpleDialog;

    // Use this code to associate the dialog to the appropriate AppWindow by setting
    // the dialog's XamlRoot to the same XamlRoot as an element that is already
    // present in the AppWindow.
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
    {
        simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
    }

    try
    {
        ContentDialogResult result = await simpleDialog.ShowAsync();
    }
    catch (Exception)
    {
        // The dialog didn't open, probably because another dialog is already open.
    }
}
```

如果不希望以编程方式关闭对话框, 请不要将其分配为`CurrentDialog`。 此处显示了一个只应在使用单击`Ok`时消除的重要对话框。 `MainPage` 由于未将其指定为`CurrentDialog`, 因此将不会尝试以编程方式关闭它。

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

    // ...
    private async void DialogButton_Click(object sender, RoutedEventArgs e)
    {
        ContentDialog importantDialog = new ContentDialog
        {
            Title = "Important dialog",
            Content = "This dialog can only be dismissed by clicking Ok.",
            CloseButtonText = "Ok"
        };

        if (MainPage.CurrentDialog != null)
        {
            MainPage.CurrentDialog.Hide();
        }
        // Do not track this dialog as the MainPage.CurrentDialog.
        // It should only be closed by clicking the Ok button.
        MainPage.CurrentDialog = null;

        try
        {
            ContentDialogResult result = await importantDialog.ShowAsync();
        }
        catch (Exception)
        {
            // The dialog didn't open, probably because another dialog is already open.
        }
    }
    // ...
}
```

## <a name="complete-code"></a>完成代码

### <a name="mainpagexaml"></a>MainPage.xaml

```xaml
<Page
    x:Class="HelloAppWindow.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloAppWindow"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button x:Name="NewWindowButton" Content="Open new window" 
                    Click="ShowNewWindowButton_Click" Margin="0,12"/>
            <Button Content="Open dialog" Click="DialogButton_Click" 
                    HorizontalAlignment="Stretch"/>
            <Button Content="Close all" Click="CloseAllButton_Click" 
                    Margin="0,12" HorizontalAlignment="Stretch"/>
        </StackPanel>

<StackPanel x:Name="colorPickerContainer" Grid.Column="1" Background="WhiteSmoke">
    <Button Click="DetachColorPickerButton_Click" HorizontalAlignment="Right">
        <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE2B4;" />
    </Button>
            <ColorPicker x:Name="colorPicker" Margin="12" Width="288"
                 IsColorChannelTextInputVisible="False"
                 ColorChanged="ColorPicker_ColorChanged"/>
        </StackPanel>
    </Grid>
</Page>

```

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Windows.Foundation;
using Windows.UI;
using Windows.UI.WindowManagement;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Hosting;
using Windows.UI.Xaml.Media;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=402352&clcid=0x409

namespace HelloAppWindow
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        AppWindow colorPickerAppWindow;

        // Track open app windows in a Dictionary.
        public static Dictionary<UIContext, AppWindow> AppWindows { get; set; }
            = new Dictionary<UIContext, AppWindow>();

        // Track the last opened dialog so you can close it if another dialog tries to open.
        public static ContentDialog CurrentDialog { get; set; } = null;

        public MainPage()
        {
            this.InitializeComponent();
        }

        private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
        {
            // Create a new window.
            AppWindow appWindow = await AppWindow.TryCreateAsync();

            // Create a Frame and navigate to the Page you want to show in the new window.
            Frame appWindowContentFrame = new Frame();
            appWindowContentFrame.Navigate(typeof(AppWindowPage));

            // Get a reference to the page instance and assign the
            // newly created AppWindow to the MyAppWindow property.
            AppWindowPage page = (AppWindowPage)appWindowContentFrame.Content;
            page.MyAppWindow = appWindow;
            page.TextColorBrush = new SolidColorBrush(colorPicker.Color);

            // Attach the XAML content to the window.
            ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);

            // Add the new page to the Dictionary using the UIContext as the Key.
            AppWindows.Add(appWindowContentFrame.UIContext, appWindow);
            appWindow.Title = "App Window " + AppWindows.Count.ToString();

            // When the window is closed, be sure to release XAML resources
            // and the reference to the window.
            appWindow.Closed += delegate
            {
                MainPage.AppWindows.Remove(appWindowContentFrame.UIContext);
                appWindowContentFrame.Content = null;
                appWindow = null;
            };

            // Show the window.
            await appWindow.TryShowAsync();
        }

        private async void DialogButton_Click(object sender, RoutedEventArgs e)
        {
            ContentDialog importantDialog = new ContentDialog
            {
                Title = "Important dialog",
                Content = "This dialog can only be dismissed by clicking Ok.",
                CloseButtonText = "Ok"
            };

            if (MainPage.CurrentDialog != null)
            {
                MainPage.CurrentDialog.Hide();
            }
            // Do not track this dialog as the MainPage.CurrentDialog.
            // It should only be closed by clicking the Ok button.
            MainPage.CurrentDialog = null;

            try
            {
                ContentDialogResult result = await importantDialog.ShowAsync();
            }
            catch (Exception)
            {
                // The dialog didn't open, probably because another dialog is already open.
            }
        }

        private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
        {
            // Create the color picker window.
            if (colorPickerAppWindow == null)
            {
                colorPickerContainer.Children.Remove(colorPicker);
                colorPickerContainer.Visibility = Visibility.Collapsed;

                Grid appWindowRootGrid = new Grid();
                appWindowRootGrid.Children.Add(colorPicker);

                // Create a new window
                colorPickerAppWindow = await AppWindow.TryCreateAsync();
                colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
                colorPickerAppWindow.RequestSize(new Size(300, 428));
                colorPickerAppWindow.Title = "Color picker";

                // Attach the XAML content to our window
                ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);

                // Make sure to release the reference to this window, 
                // and release XAML resources, when it's closed
                colorPickerAppWindow.Closed += delegate
                {
                    appWindowRootGrid.Children.Remove(colorPicker);
                    appWindowRootGrid = null;
                    colorPickerAppWindow = null;

                    colorPickerContainer.Children.Add(colorPicker);
                    colorPickerContainer.Visibility = Visibility.Visible;
                };
            }
            // Show the window.
            await colorPickerAppWindow.TryShowAsync();
        }

        private void ColorPicker_ColorChanged(ColorPicker sender, ColorChangedEventArgs args)
        {
            NewWindowButton.Background = new SolidColorBrush(args.NewColor);
        }

        private async void CloseAllButton_Click(object sender, RoutedEventArgs e)
        {
            while (AppWindows.Count > 0)
            {
                await AppWindows.Values.First().CloseAsync();
            }
        }
    }
}

```

### <a name="appwindowpagexaml"></a>AppWindowPage

```xaml
<Page
    x:Class="HelloAppWindow.AppWindowPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloAppWindow"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <TextBlock x:Name="TitleTextBlock" Text="Hello AppWindow!" FontSize="24" HorizontalAlignment="Center" Margin="24"/>

        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button Content="Open dialog" Click="DialogButton_Click"
                    Width="200" Margin="0,4"/>
            <Button Content="Move window" Click="MoveWindowButton_Click"
                    Width="200" Margin="0,4"/>
            <ToggleButton Content="Compact Overlay" x:Name="compactOverlayButton" Click="CompactOverlayButton_Click"
                          Width="200" Margin="0,4"/>
            <ToggleButton Content="Full Screen" x:Name="fullScreenButton" Click="FullScreenButton_Click"
                          Width="200" Margin="0,4"/>
            <Grid>
                <TextBlock Text="Size:"/>
                <TextBlock x:Name="SizeText" HorizontalAlignment="Right"/>
            </Grid>
            <Grid>
                <TextBlock Text="Presentation:"/>
                <TextBlock x:Name="ConfigText" HorizontalAlignment="Right"/>
            </Grid>
        </StackPanel>
    </Grid>
</Page>
```

### <a name="appwindowpagexamlcs"></a>AppWindowPage.xaml.cs

```csharp
using System;
using Windows.Foundation;
using Windows.Foundation.Metadata;
using Windows.UI;
using Windows.UI.WindowManagement;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=234238

namespace HelloAppWindow
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class AppWindowPage : Page
    {
        AppWindow window;

        public AppWindow MyAppWindow { get; set; }

        public SolidColorBrush TextColorBrush { get; set; } = new SolidColorBrush(Colors.Black);

        public AppWindowPage()
        {
            this.InitializeComponent();

            Loaded += AppWindowPage_Loaded;
        }

        private void AppWindowPage_Loaded(object sender, RoutedEventArgs e)
        {
            // Get the reference to this AppWindow that was stored when it was created.
            window = MainPage.AppWindows[this.UIContext];

            // Set up event handlers for the window.
            window.Changed += Window_Changed;

            TitleTextBlock.Foreground = TextColorBrush;
        }

        private async void DialogButton_Click(object sender, RoutedEventArgs e)
        {
            ContentDialog simpleDialog = new ContentDialog
            {
                Title = "Content dialog",
                Content = "Dialog box for " + window.Title,
                CloseButtonText = "Ok"
            };

            if (MainPage.CurrentDialog != null)
            {
                MainPage.CurrentDialog.Hide();
            }
            MainPage.CurrentDialog = simpleDialog;

            // Use this code to associate the dialog to the appropriate AppWindow by setting
            // the dialog's XamlRoot to the same XamlRoot as an element that is already 
            // present in the AppWindow.
            if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
            {
                simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
            }

            try
            {
                ContentDialogResult result = await simpleDialog.ShowAsync();
            }
            catch (Exception)
            {
                // The dialog didn't open, probably because another dialog is already open.
            }
        }

        private void Window_Changed(AppWindow sender, AppWindowChangedEventArgs args)
        {
            if (args.DidAvailableWindowPresentationsChange)
            {
                EnablePresentationButtons(sender);
            }

            if (args.DidWindowPresentationChange)
            {
                ConfigText.Text = window.Presenter.GetConfiguration().Kind.ToString();
            }

            if (args.DidSizeChange)
            {
                SizeText.Text = window.GetPlacement().Size.ToString();
            }
        }

        private void EnablePresentationButtons(AppWindow window)
        {
            // Check whether the current AppWindowPresenter supports CompactOverlay.
            if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.CompactOverlay))
            {
                // Show the CompactOverlay button...
                compactOverlayButton.Visibility = Visibility.Visible;
            }
            else
            {
                // Hide the CompactOverlay button...
                compactOverlayButton.Visibility = Visibility.Collapsed;
            }

            // Check whether the current AppWindowPresenter supports FullScreen?
            if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.FullScreen))
            {
                // Show the FullScreen button...
                fullScreenButton.Visibility = Visibility.Visible;
            }
            else
            {
                // Hide the FullScreen button...
                fullScreenButton.Visibility = Visibility.Collapsed;
            }
        }

        private void CompactOverlayButton_Click(object sender, RoutedEventArgs e)
        {
            if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.CompactOverlay)
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.CompactOverlay);
                fullScreenButton.IsChecked = false;
            }
            else
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
            }
        }

        private void FullScreenButton_Click(object sender, RoutedEventArgs e)
        {
            if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.FullScreen)
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.FullScreen);
                compactOverlayButton.IsChecked = false;
            }
            else
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
            }
        }

        private void MoveWindowButton_Click(object sender, RoutedEventArgs e)
        {
            DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
            double displayRegionWidth = displayRegion.WorkAreaSize.Width;
            double windowWidth = window.GetPlacement().Size.Width;
            int horizontalOffset = (int)(displayRegionWidth - windowWidth);
            window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
        }
    }
}
```

## <a name="related-topics"></a>相关主题

- [显示多个视图](show-multiple-views.md)
- [显示具有 w 的多个视图](application-view.md)
