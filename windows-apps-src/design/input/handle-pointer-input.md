---
Description: Receive, process, and manage input data from pointing devices such as touch, mouse, pen/stylus, and touchpad, in your Universal Windows Platform (UWP) applications.
title: 处理指针输入
ms.assetid: BDBC9E33-4037-4671-9596-471DCF855C82
label: Handle pointer input
template: detail.hbs
keywords: 笔、鼠标、触摸板、触摸、指针、输入、用户交互
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 596e9221fac686964b4faaa8a75f112dbb8ddf5a
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2019
ms.locfileid: "9048394"
---
# <a name="handle-pointer-input"></a>处理指针输入

在通用 Windows 平台 (UWP) 应用程序中接收、处理和管理来自指针设备（例如触控、鼠标、笔/触笔和触摸板）的输入数据。

> [!Important]
> 仅当要求清楚、定义良好且平台控件支持的交互不支持你的方案时才创建自定义交互。  
> 如果你在 Windows 应用程序中自定义交互体验，用户需要的是一致、直观且可发现的体验。 出于这些原因，我们建议你基于[平台控件](../controls-and-patterns/controls-by-function.md)支持的交互进行自定义交互建模。 平台控件提供完整的通用 Windows 平台 (UWP) 用户交互体验，包括标准交互、动态显示的物理效果、视觉反馈和辅助功能。 

## <a name="important-apis"></a>重要的 API
- [Windows.Devices.Input](https://docs.microsoft.com/uwp/api/Windows.Devices.Input)
- [Windows.UI.Input](https://docs.microsoft.com/uwp/api/Windows.UI.Core)
- [Windows.UI.Xaml.Input](https://docs.microsoft.com/uwp/api/Windows.UI.Input)

## <a name="pointers"></a>指针
大多数交互体验通常涉及到用户通过使用输入设备（如触控、鼠标、笔/触笔和触摸板）指向要与其进行交互的对象来标识该对象。 由于这些输入设备所提供的原始人体学接口设备 (HID) 数据包括许多常用属性，因此将该数据升级并整合到一个统一的输入堆栈，并公开为独立于设备的指针数据。 然后，你的 UWP 应用程序可以使用此数据，而无需担心所使用的输入设备。

> [!NOTE]
> 如果你的应用需要，也可以从原始 HID 数据升级特定于设备的信息。
 

输入堆栈上的每个输入点（或接触点）通过由各种指针事件处理程序中的 [**PointerRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.Pointer) 参数公开的 [**Pointer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) 对象表示。 在多笔或多点触控输入的情况下，每个接触点都视为一个唯一的输入指针。

## <a name="pointer-events"></a>指针事件


指针事件可公开诸如输入设备类型和检测状态（范围内或相互接触）之类的基本信息，以及诸如位置、压力和接触几何图形之类的扩展信息。 此外，还提供特定设备属性，例如用户按了哪一个鼠标按钮或是否正在使用笔尖橡皮擦。 如果你的应用需要在输入设备与其功能之间进行区分，请参阅[标识输入设备](identify-input-devices.md)。

UWP 应用可以侦听以下指针事件：

> [!NOTE]
> 通过在指针事件处理程序内对该元素调用 [**CapturePointer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.capturepointer)，将指针输入约束到特定的 UI 元素。 当指针由元素捕获时，仅该对象接收指针输入事件，即使指针移动到该对象的边界区域之外也是如此。 若要使 **CapturePointer** 成功，[**IsInContact**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointer.isincontact)（鼠标按钮已按下，触控或触笔相互接触）必须为 true。
 

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">事件</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled"><strong>PointerCanceled</strong></a></p></td>
<td align="left"><p>在指针由平台取消时发生。 在以下条件下，可能会发生这种情况：</p>
<ul>
<li>当在输入图面的范围内检测到笔时，将取消触控指针。</li>
<li>超过 100 毫秒未检测到活动的接触点。</li>
<li>监视器/显示器已更改（分辨率、设置、多监视器配置）。</li>
<li>桌面已锁定或用户已注销。</li>
<li>同时接触点的数量超出了设备所支持的数量。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost"><strong>PointerCaptureLost</strong></a></p></td>
<td align="left"><p>当另一个 UI 元素捕获该指针、释放该指针、或以编程方式捕获另一个指针时发生。</p>
<div class="alert">
<strong>注意</strong>没有相应指针捕获事件。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered"><strong>PointerEntered</strong></a></p></td>
<td align="left"><p>在指针进入元素的边界区域时发生。 对于触摸、触摸板、鼠标和笔输入，可能会以略有不同的方式发生此情况。</p>
<ul>
<li>触摸需要手指接触才能引发此事件，无论是从对元素直接触摸按下还是移动到该元素的绑定区域。</li>
<li>鼠标和触摸板都有一个屏幕光标，该光标始终可见并且可引发此事件，即使未按下任何鼠标或触摸板按钮也是如此。</li>
<li>和触摸一样，笔通过对元素直接按下笔或移动到该元素的绑定区域来引发此事件。 但是，笔也有悬停状态 (<a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointer.isinrange">IsInRange</a>)，当为 true 时，将会触发此事件。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited"><strong>PointerExited</strong></a></p></td>
<td align="left"><p>在指针离开元素的边界区域时发生。 对于触摸、触摸板、鼠标和笔输入，可能会以略有不同的方式发生此情况。</p>
<ul>
<li>触摸需要手指接触，并在指针移出该元素的绑定区域时引发此事件。</li>
<li>鼠标和触摸板都有一个屏幕光标，该光标始终可见并且可引发此事件，即使未按下任何鼠标或触摸板按钮也是如此。</li>
<li>和触控一样，笔在移出该元素的边界区域时触发此事件。 但是，笔也有悬停状态 (<a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointer.isinrange">IsInRange</a>)，当状态从 true 更改为 false 时触发此事件。</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved"><strong>PointerMoved</strong></a></p></td>
<td align="left"><p>在指针更改元素的绑定区域内的坐标、按钮状态、压力、倾斜或接触几何图形（例如宽度和高度）时发生。 对于触摸、触摸板、鼠标和笔输入，可能会以略有不同的方式发生此情况。</p>
<ul>
<li>触摸需要手指接触，并且仅在与该元素的绑定区域接触时引发此事件。</li>
<li>鼠标和触摸板都有一个屏幕光标，该光标始终可见并且可引发此事件，即使未按下任何鼠标或触摸板按钮也是如此。</li>
<li>和触控一样，笔在与该元素的边界区域接触时触发此事件。 但是，笔也有悬停状态 (<a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointer.isinrange">IsInRange</a>)，当为 true 且位于元素的边界区域内时，将触发此事件。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed"><strong>PointerPressed</strong></a></p></td>
<td align="left"><p>在指针指示元素的绑定区域内发生按下操作（例如触摸按下、鼠标按钮按下、笔按下或触摸板按钮按下）时发生。</p>
<p>必须从处理程序中对此事件调用 <a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.capturepointer">CapturePointer</a>。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased"><strong>PointerReleased</strong></a></p></td>
<td align="left"><p>在指针指示元素的绑定区域内发生释放事件（例如触摸抬起、鼠标按钮抬起、笔抬起或触摸板按钮抬起）时或在绑定区域外捕获到该指针时发生。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged"><strong>PointerWheelChanged</strong></a></p></td>
<td align="left"><p>在旋转鼠标滚轮时发生。</p>
<p>鼠标输入与第一次检测到鼠标输入时分配的单个指针相关联。 单击鼠标按钮（左键、滚轮或右键）会通过 <a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved">PointerMoved</a> 事件在指针和该按钮之间创建一个辅助关联。</p></td>
</tr>
</tbody>
</table> 

## <a name="pointer-event-example"></a>指针事件示例

以下是一些来自基本指针跟踪应用的代码段，演示了如何侦听和处理多个指针的事件并获取关联指针的各种属性。

![指针应用程序 UI](images/pointers/pointers1.gif)

**从[指针输入示例（基本）](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)下载此示例**

### <a name="create-the-ui"></a>创建 UI

对于此示例，我们使用一个[矩形](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.rectangle) (`Target`) 作为使用指针输入的对象。 当指针状态更改时，目标的颜色也将更改。

每个指针的详细信息显示在随指针移动的浮动 [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 中。 指针事件本身在矩形右侧的 [RichTextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) 中报告。

这是此示例中的 UI 的可扩展应用程序标记语言 (XAML)。 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*" />
        <ColumnDefinition Width="250"/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="320" />
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Canvas Name="Container" 
            Grid.Column="0"
            Grid.Row="1"
            HorizontalAlignment="Center" 
            VerticalAlignment="Center" 
            Margin="245,0" 
            Height="320"  Width="640">
        <Rectangle Name="Target" 
                    Fill="#FF0000" 
                    Stroke="Black" 
                    StrokeThickness="0"
                    Height="320" Width="640" />
    </Canvas>
    <Grid Grid.Column="1" Grid.Row="0" Grid.RowSpan="3">
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        <Button Name="buttonClear" 
                Grid.Row="0"
                Content="Clear"
                Foreground="White"
                HorizontalAlignment="Stretch" 
                VerticalAlignment="Stretch">
        </Button>
        <ScrollViewer Name="eventLogScrollViewer" Grid.Row="1" 
                        VerticalScrollMode="Auto" 
                        Background="Black">                
            <RichTextBlock Name="eventLog"  
                        TextWrapping="Wrap" 
                        Foreground="#FFFFFF" 
                        ScrollViewer.VerticalScrollBarVisibility="Visible" 
                        ScrollViewer.HorizontalScrollBarVisibility="Disabled"
                        Grid.ColumnSpan="2">
            </RichTextBlock>
        </ScrollViewer>
    </Grid>
</Grid>
```

### <a name="listen-for-pointer-events"></a>侦听指针事件

在大多数情况下，我们建议你通过事件处理程序的 [**PointerRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) 获取指针信息。

如果事件参数不公开所需的指针详细信息，则可以获取对通过[**GetCurrentPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getcurrentpoint)和 [**GetIntermediatePoints**] 公开的扩展[**PointerPoint**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.PointerPoint)信息的访问https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getintermediatepoints)的[**方法PointerRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs)。

以下代码设置用于跟踪每个活动指针的全局字典对象，并为该目标对象标识各个指针事件侦听器。

```CSharp
// Dictionary to maintain information about each active pointer. 
// An entry is added during PointerPressed/PointerEntered events and removed 
// during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
Dictionary<uint, Windows.UI.Xaml.Input.Pointer> pointers;

public MainPage()
{
    this.InitializeComponent();

    // Initialize the dictionary.
    pointers = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>();

    // Declare the pointer event handlers.
    Target.PointerPressed += 
        new PointerEventHandler(Target_PointerPressed);
    Target.PointerEntered += 
        new PointerEventHandler(Target_PointerEntered);
    Target.PointerReleased += 
        new PointerEventHandler(Target_PointerReleased);
    Target.PointerExited += 
        new PointerEventHandler(Target_PointerExited);
    Target.PointerCanceled += 
        new PointerEventHandler(Target_PointerCanceled);
    Target.PointerCaptureLost += 
        new PointerEventHandler(Target_PointerCaptureLost);
    Target.PointerMoved += 
        new PointerEventHandler(Target_PointerMoved);
    Target.PointerWheelChanged += 
        new PointerEventHandler(Target_PointerWheelChanged);

    buttonClear.Click += 
        new RoutedEventHandler(ButtonClear_Click);
}
```

### <a name="handle-pointer-events"></a>处理指针事件

接下来，我们使用 UI 反馈来演示基本的指针事件处理程序。

-   此处理程序管理 [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) 事件。 我们将事件添加到事件日志、将指针添加活动指针字典，并显示指针详细信息。

    > [!NOTE]
    > [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) 和 [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased) 事件并不总是成对出现。 你的应用应该侦听和处理可能会结束指针向下（如 [**PointerExited**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)、[**PointerCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled) 和 [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost)）的所有事件。      

```csharp
/// <summary>
/// The pointer pressed event handler.
/// PointerPressed and PointerReleased don't always occur in pairs. 
/// Your app should listen for and handle any event that can conclude 
/// a pointer down (PointerExited, PointerCanceled, PointerCaptureLost).
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Down: " + ptrPt.PointerId);

    // Lock the pointer to the target.
    Target.CapturePointer(e.Pointer);

    // Update event log.
    UpdateEventLog("Pointer captured: " + ptrPt.PointerId);

    // Check if pointer exists in dictionary (ie, enter occurred prior to press).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    // Change background color of target when pointer contact detected.
    Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   此处理程序管理 [**PointerEntered**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered) 事件。 我们将事件添加到事件日志、将指针添加指针集合，并显示指针详细信息。

```csharp
/// <summary>
/// The pointer entered event handler.
/// We do not capture the pointer on this event.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Entered: " + ptrPt.PointerId);

    // Check if pointer already exists (if enter occurred prior to down).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    if (pointers.Count == 0)
    {
        // Change background color of target when pointer contact detected.
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
    }

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   此处理程序管理 [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved) 事件。 我们将事件添加到事件日志并更新指针详细信息。

    > [!Important]
    > 鼠标输入与第一次检测到鼠标输入时分配的单个指针相关联。 单击鼠标按钮（左键、滚轮或右键）会通过 [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) 事件在指针和该按钮之间创建一个辅助关联。 仅当释放该鼠标按钮时才引发 [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased) 事件（在完成该事件之前，其他按钮无法与指针关联）。 由于此排他性关联，会通过 [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved) 事件路由其他鼠标按钮单击。     

```csharp
/// <summary>
/// The pointer moved event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Multiple, simultaneous mouse button inputs are processed here.
    // Mouse input is associated with a single pointer assigned when 
    // mouse input is first detected. 
    // Clicking additional mouse buttons (left, wheel, or right) during 
    // the interaction creates secondary associations between those buttons 
    // and the pointer through the pointer pressed event. 
    // The pointer released event is fired only when the last mouse button 
    // associated with the interaction (not necessarily the initial button) 
    // is released. 
    // Because of this exclusive association, other mouse button clicks are 
    // routed through the pointer move event.          
    if (ptrPt.PointerDevice.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
    {
        if (ptrPt.Properties.IsLeftButtonPressed)
        {
            UpdateEventLog("Left button: " + ptrPt.PointerId);
        }
        if (ptrPt.Properties.IsMiddleButtonPressed)
        {
            UpdateEventLog("Wheel button: " + ptrPt.PointerId);
        }
        if (ptrPt.Properties.IsRightButtonPressed)
        {
            UpdateEventLog("Right button: " + ptrPt.PointerId);
        }
    }

    // Display pointer details.
    UpdateInfoPop(ptrPt);
}
```

-   此处理程序管理 [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged) 事件。 我们将事件添加到事件日志、将指针添加到指针数组（如有必要），并显示指针详细信息。

```csharp
/// <summary>
/// The pointer wheel event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Mouse wheel: " + ptrPt.PointerId);

    // Check if pointer already exists (for example, enter occurred prior to wheel).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   在已终止与数字化器的接触的情况下，此处理程序管理 [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased) 事件。 我们将事件添加到事件日志、从指针集合删除指针，并更新指针详细信息。

```csharp
/// <summary>
/// The pointer released event handler.
/// PointerPressed and PointerReleased don't always occur in pairs. 
/// Your app should listen for and handle any event that can conclude 
/// a pointer down (PointerExited, PointerCanceled, PointerCaptureLost).
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Up: " + ptrPt.PointerId);

    // If event source is mouse or touchpad and the pointer is still 
    // over the target, retain pointer and pointer details.
    // Return without removing pointer from pointers dictionary.
    // For this example, we assume a maximum of one mouse pointer.
    if (ptrPt.PointerDevice.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
    {
        // Update target UI.
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

        DestroyInfoPop(ptrPt);

        // Remove contact from dictionary.
        if (pointers.ContainsKey(ptrPt.PointerId))
        {
            pointers[ptrPt.PointerId] = null;
            pointers.Remove(ptrPt.PointerId);
        }

        // Release the pointer from the target.
        Target.ReleasePointerCapture(e.Pointer);

        // Update event log.
        UpdateEventLog("Pointer released: " + ptrPt.PointerId);
    }
    else
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
    }
}
```

-   此处理程序管理 [**PointerExited**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited) 事件（在保持与数字化器的接触的情况下）。 我们将事件添加到事件日志、从指针数组删除指针，并更新指针详细信息。

```csharp
/// <summary>
/// The pointer exited event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer exited: " + ptrPt.PointerId);

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
    }

    // Update the UI and pointer details.
    DestroyInfoPop(ptrPt);
}
```

-   此处理程序管理 [**PointerCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled) 事件。 我们将事件添加到事件日志、从指针数组删除指针，并更新指针详细信息。

```csharp
/// <summary>
/// The pointer canceled event handler.
/// Fires for various reasons, including: 
///    - Touch contact canceled by pen coming into range of the surface.
///    - The device doesn't report an active contact for more than 100ms.
///    - The desktop is locked or the user logged off. 
///    - The number of simultaneous contacts exceeded the number supported by the device.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer canceled: " + ptrPt.PointerId);

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
    }

    DestroyInfoPop(ptrPt);
}
```

-   此处理程序管理 [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost) 事件。 我们将事件添加到事件日志、从指针数组删除指针，并更新指针详细信息。

    > [!NOTE]
    > 可能会发生 [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost)，而不是 [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased)。 指针捕获可能由于各种原因而丢失，包括用户交互、以编程方式捕获另一个指针、调用 [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased)。     

```csharp
/// <summary>
/// The pointer capture lost event handler.
/// Fires for various reasons, including: 
///    - User interactions
///    - Programmatic capture of another pointer
///    - Captured pointer was deliberately released
// PointerCaptureLost can fire instead of PointerReleased. 
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer capture lost: " + ptrPt.PointerId);

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
    }

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    DestroyInfoPop(ptrPt);
}
```

### <a name="get-pointer-properties"></a>获取指针属性

如前面所述，你必须从通过[**GetCurrentPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getcurrentpoint)和 [**GetIntermediatePoints**] 获得[**Windows.UI.Input.PointerPoint**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.PointerPoint)对象获取最扩展的指针信息https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getintermediatepoints) [**PointerRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs)的方法。 下面的代码段显示操作步骤。

-   首先，我们为每个指针创建一个新的 [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)。

```csharp
/// <summary>
/// Create the pointer info popup.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
void CreateInfoPop(PointerPoint ptrPt)
{
    TextBlock pointerDetails = new TextBlock();
    pointerDetails.Name = ptrPt.PointerId.ToString();
    pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
    pointerDetails.Text = QueryPointer(ptrPt);

    TranslateTransform x = new TranslateTransform();
    x.X = ptrPt.Position.X + 20;
    x.Y = ptrPt.Position.Y + 20;
    pointerDetails.RenderTransform = x;

    Container.Children.Add(pointerDetails);
}
```

-   然后，我们在与该指针关联的现有 [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 中，提供一种更新指针信息的方式。

```csharp
/// <summary>
/// Update the pointer info popup.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
void UpdateInfoPop(PointerPoint ptrPt)
{
    foreach (var pointerDetails in Container.Children)
    {
        if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
        {
            TextBlock textBlock = (TextBlock)pointerDetails;
            if (textBlock.Name == ptrPt.PointerId.ToString())
            {
                // To get pointer location details, we need extended pointer info.
                // We get the pointer info through the getCurrentPoint method
                // of the event argument. 
                TranslateTransform x = new TranslateTransform();
                x.X = ptrPt.Position.X + 20;
                x.Y = ptrPt.Position.Y + 20;
                pointerDetails.RenderTransform = x;
                textBlock.Text = QueryPointer(ptrPt);
            }
        }
    }
}
```

-   最后，我们将查询各种指针属性。

```csharp
/// <summary>
/// Get pointer details.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
/// <returns>A string composed of pointer details.</returns>
String QueryPointer(PointerPoint ptrPt)
{
    String details = "";

    switch (ptrPt.PointerDevice.PointerDeviceType)
    {
        case Windows.Devices.Input.PointerDeviceType.Mouse:
            details += "\nPointer type: mouse";
            break;
        case Windows.Devices.Input.PointerDeviceType.Pen:
            details += "\nPointer type: pen";
            if (ptrPt.IsInContact)
            {
                details += "\nPressure: " + ptrPt.Properties.Pressure;
                details += "\nrotation: " + ptrPt.Properties.Orientation;
                details += "\nTilt X: " + ptrPt.Properties.XTilt;
                details += "\nTilt Y: " + ptrPt.Properties.YTilt;
                details += "\nBarrel button pressed: " + ptrPt.Properties.IsBarrelButtonPressed;
            }
            break;
        case Windows.Devices.Input.PointerDeviceType.Touch:
            details += "\nPointer type: touch";
            details += "\nrotation: " + ptrPt.Properties.Orientation;
            details += "\nTilt X: " + ptrPt.Properties.XTilt;
            details += "\nTilt Y: " + ptrPt.Properties.YTilt;
            break;
        default:
            details += "\nPointer type: n/a";
            break;
    }

    GeneralTransform gt = Target.TransformToVisual(this);
    Point screenPoint;

    screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
    details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
        "\nPointer location (target): " + Math.Round(ptrPt.Position.X) + ", " + Math.Round(ptrPt.Position.Y) +
        "\nPointer location (container): " + Math.Round(screenPoint.X) + ", " + Math.Round(screenPoint.Y);

    return details;
}
```

## <a name="primary-pointer"></a>主指针
某些输入设备（例如触控式数字化器或触摸板）并不仅仅支持鼠标或笔的典型单一指针（在大多数情况下，就像 Surface Hub 那样支持两个笔输入）。 

使用 **[PointerPointerProperties](https://docs.microsoft.com/uwp/api/windows.ui.input.pointerpointproperties)** 类的只读 **[IsPrimary](https://docs.microsoft.com/uwp/api/windows.ui.input.pointerpointproperties.IsPrimary)** 属性可以标识并区分单一主指针（主指针始终是输入序列期间检测到的第一个指针）。 

通过标识主指针，你可以用它来模拟鼠标或笔输入、自定义交互或提供一些其他的特定功能或 UI。

> [!NOTE]
> 如果在某个输入序列期间释放、取消或丢失了主指针，则在初始化新的输入序列之前，不会创建主输入指针（释放、取消或丢失了所有指针后，输入序列将结束）。

## <a name="primary-pointer-animation-example"></a>主指针动画示例

这些代码段显示了如何提供专门的视觉反馈以帮助用户区分应用程序中的指针输入。

这个特定应用使用颜色和动画突出显示主指针。

![具有动画视觉反馈的指针应用程序](images/pointers/pointers-usercontrol-animation.gif)

**从[指针输入示例（具有动画的 UserControl）](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)下载此示例**

### <a name="visual-feedback"></a>视觉反馈

我们基于 XAML **[椭圆形](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.ellipse)** 对象定义一个 **[UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol)**，突出显示每个指针在画布上的位置，并使用**[故事板](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard)** 来动画显示与主指针对应的椭圆形。

**下面是 XAML：**

```xaml
<UserControl
    x:Class="UWP_Pointers.PointerEllipse"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:UWP_Pointers"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="100"
    d:DesignWidth="100">

    <UserControl.Resources>
        <Style x:Key="EllipseStyle" TargetType="Ellipse">
            <Setter Property="Transitions">
                <Setter.Value>
                    <TransitionCollection>
                        <ContentThemeTransition/>
                    </TransitionCollection>
                </Setter.Value>
            </Setter>
        </Style>
        
        <Storyboard x:Name="myStoryboard">
            <!-- Animates the value of a Double property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <DoubleAnimation
              Storyboard.TargetName="ellipse"
              Storyboard.TargetProperty="(RenderTransform).(ScaleTransform.ScaleY)"  
              Duration="0:0:1" 
              AutoReverse="True" 
              RepeatBehavior="Forever" From="1.0" To="1.4">
            </DoubleAnimation>

            <!-- Animates the value of a Double property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <DoubleAnimation
              Storyboard.TargetName="ellipse"
              Storyboard.TargetProperty="(RenderTransform).(ScaleTransform.ScaleX)"  
              Duration="0:0:1" 
              AutoReverse="True" 
              RepeatBehavior="Forever" From="1.0" To="1.4">
            </DoubleAnimation>

            <!-- Animates the value of a Color property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <ColorAnimation 
                Storyboard.TargetName="ellipse" 
                EnableDependentAnimation="True" 
                Storyboard.TargetProperty="(Fill).(SolidColorBrush.Color)" 
                From="White" To="Red"  Duration="0:0:1" 
                AutoReverse="True" RepeatBehavior="Forever"/>
        </Storyboard>
    </UserControl.Resources>

    <Grid x:Name="CompositionContainer">
        <Ellipse Name="ellipse" 
        StrokeThickness="2" 
        Width="{x:Bind Diameter}" 
        Height="{x:Bind Diameter}"  
        Style="{StaticResource EllipseStyle}" />
    </Grid>
</UserControl>
```

下面是代码隐藏：
```csharp
using Windows.Foundation;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

// The User Control item template is documented at 
// https://go.microsoft.com/fwlink/?LinkId=234236

namespace UWP_Pointers
{
    /// <summary>
    /// Pointer feedback object.
    /// </summary>
    public sealed partial class PointerEllipse : UserControl
    {
        // Reference to the application canvas.
        Canvas canvas;

        /// <summary>
        /// Ellipse UI for pointer feedback.
        /// </summary>
        /// <param name="c">The drawing canvas.</param>
        public PointerEllipse(Canvas c)
        {
            this.InitializeComponent();
            canvas = c;
        }

        /// <summary>
        /// Gets or sets the pointer Id to associate with the PointerEllipse object.
        /// </summary>
        public uint PointerId
        {
            get { return (uint)GetValue(PointerIdProperty); }
            set { SetValue(PointerIdProperty, value); }
        }
        // Using a DependencyProperty as the backing store for PointerId.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PointerIdProperty =
            DependencyProperty.Register("PointerId", typeof(uint), 
                typeof(PointerEllipse), new PropertyMetadata(null));


        /// <summary>
        /// Gets or sets whether the associated pointer is Primary.
        /// </summary>
        public bool PrimaryPointer
        {
            get { return (bool)GetValue(PrimaryPointerProperty); }
            set
            {
                SetValue(PrimaryPointerProperty, value);
            }
        }
        // Using a DependencyProperty as the backing store for PrimaryPointer.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PrimaryPointerProperty =
            DependencyProperty.Register("PrimaryPointer", typeof(bool), 
                typeof(PointerEllipse), new PropertyMetadata(false));


        /// <summary>
        /// Gets or sets the ellipse style based on whether the pointer is Primary.
        /// </summary>
        public bool PrimaryEllipse 
        {
            get { return (bool)GetValue(PrimaryEllipseProperty); }
            set
            {
                SetValue(PrimaryEllipseProperty, value);
                if (value)
                {
                    SolidColorBrush fillBrush = 
                        (SolidColorBrush)Application.Current.Resources["PrimaryFillBrush"];
                    SolidColorBrush strokeBrush = 
                        (SolidColorBrush)Application.Current.Resources["PrimaryStrokeBrush"];

                    ellipse.Fill = fillBrush;
                    ellipse.Stroke = strokeBrush;
                    ellipse.RenderTransform = new CompositeTransform();
                    ellipse.RenderTransformOrigin = new Point(.5, .5);
                    myStoryboard.Begin();
                }
                else
                {
                    SolidColorBrush fillBrush = 
                        (SolidColorBrush)Application.Current.Resources["SecondaryFillBrush"];
                    SolidColorBrush strokeBrush = 
                        (SolidColorBrush)Application.Current.Resources["SecondaryStrokeBrush"];
                    ellipse.Fill = fillBrush;
                    ellipse.Stroke = strokeBrush;
                }
            }
        }
        // Using a DependencyProperty as the backing store for PrimaryEllipse.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PrimaryEllipseProperty =
            DependencyProperty.Register("PrimaryEllipse", 
                typeof(bool), typeof(PointerEllipse), new PropertyMetadata(false));


        /// <summary>
        /// Gets or sets the diameter of the PointerEllipse object.
        /// </summary>
        public int Diameter
        {
            get { return (int)GetValue(DiameterProperty); }
            set { SetValue(DiameterProperty, value); }
        }
        // Using a DependencyProperty as the backing store for Diameter.  This enables animation, styling, binding, etc...
        public static readonly DependencyProperty DiameterProperty =
            DependencyProperty.Register("Diameter", typeof(int), 
                typeof(PointerEllipse), new PropertyMetadata(120));
    }
}
```

### <a name="create-the-ui"></a>创建 UI
此示例中的 UI 限制为输入**[画布](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas)**，我们在其中跟踪所有指针并呈现指针指示器和主指针动画（如果适用），同时显示一个包含指针计数器和主指针标识符的标题栏。

下面是 MainPage.xaml：

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" 
                Orientation="Horizontal" 
                Grid.Row="0">
        <StackPanel.Transitions>
            <TransitionCollection>
                <AddDeleteThemeTransition/>
            </TransitionCollection>
        </StackPanel.Transitions>
        <TextBlock x:Name="Header" 
                    Text="Basic pointer tracking sample - IsPrimary" 
                    Style="{ThemeResource HeaderTextBlockStyle}" 
                    Margin="10,0,0,0" />
        <TextBlock x:Name="PointerCounterLabel"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="Number of pointers: " 
                    Margin="50,0,0,0"/>
        <TextBlock x:Name="PointerCounter"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="0" 
                    Margin="10,0,0,0"/>
        <TextBlock x:Name="PointerPrimaryLabel"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="Primary: " 
                    Margin="50,0,0,0"/>
        <TextBlock x:Name="PointerPrimary"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="n/a" 
                    Margin="10,0,0,0"/>
    </StackPanel>
    
    <Grid Grid.Row="1">
        <!--The canvas where we render the pointer UI.-->
        <Canvas x:Name="pointerCanvas"/>
    </Grid>
</Grid>
```

### <a name="handle-pointer-events"></a>处理指针事件

最后，我们在 MainPage.xaml.cs 代码隐藏文件中定义基本指针事件处理程序。 我们不会在此处复制代码，因为上一个示例已经介绍了基本知识，但你可以从[指针输入示例（具有动画的 UserControl）](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)下载工作示例。

## <a name="related-articles"></a>相关文章

**主题示例**
* [指针输入示例（基本）](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)
* [指针输入示例（具有动画的 UserControl）](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)

**其他示例**
* [基本输入示例](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延迟输入示例](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [用户交互模式示例](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [焦点视觉示例](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**存档示例**
* [输入：XAML 用户输入事件示例](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [输入：设备功能示例](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [输入：操作和手势 (C++) 示例](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [输入：触摸点击测试示例](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML 滚动、平移以及缩放示例](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [输入：简化的墨迹示例](https://go.microsoft.com/fwlink/p/?linkid=246570)
