---
author: Karl-Bridge-Microsoft
Description: "在通用 Windows 平台 (UWP) 应用中接收、处理和管理来自指针设备的输入数据，例如触摸、鼠标、笔/触笔和触摸板。"
title: "处理指针输入"
ms.assetid: BDBC9E33-4037-4671-9596-471DCF855C82
label: Handle pointer input
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 8e3d0fdd97c72c2e7816fbc48738c651fb4f5bbd

---

# 处理指针输入

在通用 Windows 平台 (UWP) 应用中接收、处理和管理来自指针设备的输入数据，例如触摸、鼠标、笔/触笔和触摸板。

**重要的 API**

-   [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br208383)
-   [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)


**重要提示**  
如果你实现自己的交互支持，请记住，用户期望获得直观的体验，包括直接与应用中的 UI 元素交互。 我们建议你在[控件列表](https://msdn.microsoft.com/library/windows/apps/mt185406)上生成你的自定义交互的模型以保持内容一致和易于发现。 平台控件提供完整的通用 Windows 平台 (UWP) 用户交互体验，包括标准交互、动态显示的物理效果、视觉反馈和辅助功能。 仅当要求清楚、定义良好且基本交互不支持你的方案时才创建自定义交互。


## <span id="Pointers"></span><span id="pointers"></span><span id="POINTERS"></span>指针


许多交互体验涉及到用户通过使用输入设备（如触摸、鼠标、笔/触笔和触摸板）指向要与其进行交互的对象来标识该对象。 由于这些输入设备所提供的原始人体学接口设备 (HID) 数据包括许多常用属性，因此该信息升级为一个统一的输入堆栈，并公开为经过整合的、独立于设备的指针数据。 然后，你的 UWP 应用可以使用此数据，而无需担心所使用的输入设备。

**注意** 如果你的应用需要，也可以从原始 HID 数据升级特定于设备的信息。

 

输入堆栈上的每个输入点（或接触点）通过由各种指针事件提供的 [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) 参数公开的 [**Pointer**](https://msdn.microsoft.com/library/windows/apps/br227968) 对象表示。 在多笔或多触摸输入的情况下，每个接触点都视为一个唯一的输入点。

## <span id="Pointer_events"></span><span id="pointer_events"></span><span id="POINTER_EVENTS"></span>指针事件


指针事件可公开诸如检测状态（在范围中或在接触点中）和设备类型之类的基本信息，以及诸如位置、压力和接触几何图形之类的扩展信息。 此外，还提供特定设备属性，例如用户按了哪一个鼠标按钮或是否正在使用笔尖橡皮擦。 如果你的应用需要在输入设备与其功能之间进行区分，请参阅[标识输入设备](identify-input-devices.md)。

UWP 应用可以侦听以下指针事件：

**注意** 调用 [**CapturePointer**](https://msdn.microsoft.com/library/windows/apps/br208918) 以限制对特定 UI 元素的指针输入。 当指针由元素捕获时，仅该对象接收指针输入事件，即使指针在该对象的绑定区域之外移动也是如此。 你通常在 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) 事件处理程序内捕获指针，因为 [**IsInContact**](https://msdn.microsoft.com/library/windows/apps/br227976)（鼠标按钮已按下、触摸或触笔正在接触）必须为 true 才能使 **CapturePointer** 成功。

 

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">事件</th>
<th align="left">说明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><span id="PointerCanceled"></span><span id="pointercanceled"></span><span id="POINTERCANCELED"></span>[<strong>PointerCanceled</strong>](https://msdn.microsoft.com/library/windows/apps/br208964)</p></td>
<td align="left"><p>在指针由平台取消时发生。</p>
<ul>
<li>当在输入图面的范围内检测到笔时，将取消触摸指针。</li>
<li>超过 100 毫秒未检测到活动的接触点。</li>
<li>监视器/显示器已更改（分辨率、设置、多监视器配置）。</li>
<li>桌面已锁定或用户已注销。</li>
<li>同时接触点的数量超出了设备所支持的数量。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><span id="PointerCaptureLost"></span><span id="pointercapturelost"></span><span id="POINTERCAPTURELOST"></span>[<strong>PointerCaptureLost</strong>](https://msdn.microsoft.com/library/windows/apps/br208965)</p></td>
<td align="left"><p>当另一个 UI 元素捕获该指针、释放该指针、或以编程方式捕获另一个指针时发生。</p>
<div class="alert">
<strong>注意</strong> 不存在相应的指针捕获事件。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="PointerEntered"></span><span id="pointerentered"></span><span id="POINTERENTERED"></span>\[<strong>PointerEntered</strong>\](https://msdn.microsoft.com/library/windows/apps/br208968)</p></td>
<td align="left"><p>在指针进入元素的绑定区域时发生。 对于触摸、触摸板、鼠标和笔输入，可能会以略有不同的方式发生此情况。</p>
<ul>
<li>触摸需要手指接触才能引发此事件，无论是从对元素直接触摸按下还是移动到该元素的绑定区域。</li>
<li>鼠标和触摸板都有一个屏幕光标，该光标始终可见并且可引发此事件，即使未按下任何鼠标或触摸板按钮也是如此。</li>
<li>和触摸一样，笔通过对元素直接按下笔或移动到该元素的绑定区域来引发此事件。 但是，笔还有悬停状态 ([<strong>IsInRange</strong>](https://msdn.microsoft.com/library/windows/apps/br227977))，可在为 true 时引发此事件。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><span id="PointerExited"></span><span id="pointerexited"></span><span id="POINTEREXITED"></span>[<strong>PointerExited</strong>](https://msdn.microsoft.com/library/windows/apps/br208969)</p></td>
<td align="left"><p>在指针离开元素的绑定区域时发生。 对于触摸、触摸板、鼠标和笔输入，可能会以略有不同的方式发生此情况。</p>
<ul>
<li>触摸需要手指接触，并在指针移出该元素的绑定区域时引发此事件。</li>
<li>鼠标和触摸板都有一个屏幕光标，该光标始终可见并且可引发此事件，即使未按下任何鼠标或触摸板按钮也是如此。</li>
<li>和触摸一样，笔在移出该元素的绑定区域时引发此事件。 但是，笔还有悬停状态 ([<strong>IsInRange</strong>](https://msdn.microsoft.com/library/windows/apps/br227977))，可在状态从 ture 更改为 false 时引发此事件。</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="PointerMoved"></span><span id="pointermoved"></span><span id="POINTERMOVED"></span>[<strong>PointerMoved</strong>](https://msdn.microsoft.com/library/windows/apps/br208970)</p></td>
<td align="left"><p>在指针更改元素的绑定区域内的坐标、按钮状态、压力、倾斜或接触几何图形（例如宽度和高度）时发生。 对于触摸、触摸板、鼠标和笔输入，可能会以略有不同的方式发生此情况。</p>
<ul>
<li>触摸需要手指接触，并且仅在与该元素的绑定区域接触时引发此事件。</li>
<li>鼠标和触摸板都有一个屏幕光标，该光标始终可见并且可引发此事件，即使未按下任何鼠标或触摸板按钮也是如此。</li>
<li>和触摸一样，笔在与该元素的绑定区域接触时引发此事件。 但是，笔还有悬停状态 ([<strong>IsInRange</strong>](https://msdn.microsoft.com/library/windows/apps/br227977))，可在为 true 且在元素的绑定区域内时引发此事件。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><span id="PointerPressed"></span><span id="pointerpressed"></span><span id="POINTERPRESSED"></span>[<strong>PointerPressed</strong>](https://msdn.microsoft.com/library/windows/apps/br208971)</p></td>
<td align="left"><p>在指针指示元素的绑定区域内发生按下操作（例如触摸按下、鼠标按钮按下、笔按下或触摸板按钮按下）时发生。</p>
<p>[<strong>CapturePointer</strong>](https://msdn.microsoft.com/library/windows/apps/br208918) 必须从此事件的处理程序调用。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="PointerReleased"></span><span id="pointerreleased"></span><span id="POINTERRELEASED"></span>[<strong>PointerReleased</strong>](https://msdn.microsoft.com/library/windows/apps/br208972)</p></td>
<td align="left"><p>在指针指示元素的绑定区域内发生释放事件（例如触摸抬起、鼠标按钮抬起、笔抬起或触摸板按钮抬起）时或在绑定区域外捕获到该指针时发生。</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="PointerWheelChanged"></span><span id="pointerwheelchanged"></span><span id="POINTERWHEELCHANGED"></span>[<strong>PointerWheelChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br208973)</p></td>
<td align="left"><p>在旋转鼠标滚轮时发生。</p>
<p>鼠标输入与第一次检测到鼠标输入时分配的单个指针相关联。 单击鼠标按钮（左键、滚轮或右键）会通过 \[<strong>PointerMoved</strong>\]\(https://msdn.microsoft.com/library/windows/apps/br208970\) 事件在指针和该按钮之间创建一个辅助关联。</p></td>
</tr>
</tbody>
</table>

 

## <span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>示例


以下是一些来自基本指针跟踪应用的代码示例，演示了如何侦听和处理指针事件并获取活动指针的各种属性。

### <span id="Create_the_UI"></span><span id="create_the_ui"></span><span id="CREATE_THE_UI"></span>创建 UI

对于此示例，我们使用一个矩形 (`targetContainer`) 作为指针输入的目标对象。 当指针状态更改时，目标的颜色也将更改。

每个指针的详细信息显示在随指针移动的浮动文本块中。 指针事件本身显示在矩形的左侧（用于报告事件序列）。

这是此示例的 Extensible Application Markup Language (XAML)。

```XAML
<Page
    x:Class="PointerInput.MainPage"
    IsTabStop="false"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:PointerInput"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Name="page">

    <Grid Background="{StaticResource ApplicationForegroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="69.458" />
            <ColumnDefinition Width="80.542"/>
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
        <Button Name="buttonClear"
                Foreground="White"
                Width="100"
                Height="100">
            clear
        </Button>
        <TextBox Name="eventLog" 
                 Grid.Column="1"
                 Grid.Row="0"
                 Grid.RowSpan="3" 
                 Background="#000000" 
                 TextWrapping="Wrap" 
                 Foreground="#FFFFFF" 
                 ScrollViewer.VerticalScrollBarVisibility="Visible" 
                 BorderThickness="0" Grid.ColumnSpan="2"/>
    </Grid>
</Page>
```

### <span id="Listen_for_pointer_events"></span><span id="listen_for_pointer_events"></span><span id="LISTEN_FOR_POINTER_EVENTS"></span>侦听指针事件

在大多数情况下，我们建议你通过事件处理程序的 [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) 获取指针信息。

如果事件参数不公开所需的指针详细信息，则可以获取对通过 [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) 的 [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/hh943077) 和 [**GetIntermediatePoints**](https://msdn.microsoft.com/library/windows/apps/hh943078) 方法公开的扩展 [**PointerPoint**](https://msdn.microsoft.com/library/windows/apps/br242038) 信息的访问权限。

对于此示例，我们使用一个矩形 (`targetContainer`) 作为指针输入的目标对象。 当指针状态更改时，目标的颜色也将更改。

以下代码设置目标对象、声明全局变量，并为该目标标识各个指针事件侦听器。

```CSharp
        // For this example, we track simultaneous contacts in case the 
        // number of contacts has reached the maximum supported by the device.
        // Depending on the device, additional contacts might be ignored 
        // (PointerPressed not fired). 
        uint numActiveContacts;
        Windows.Devices.Input.TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();

        // Dictionary to maintain information about each active contact. 
        // An entry is added during PointerPressed/PointerEntered events and removed 
        // during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
        Dictionary<uint, Windows.UI.Xaml.Input.Pointer> contacts;

        public MainPage()
        {
            this.InitializeComponent();
            numActiveContacts = 0;
            // Initialize the dictionary.
            contacts = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>((int)touchCapabilities.Contacts);
            // Declare the pointer event handlers.
            Target.PointerPressed += new PointerEventHandler(Target_PointerPressed);
            Target.PointerEntered += new PointerEventHandler(Target_PointerEntered);
            Target.PointerReleased += new PointerEventHandler(Target_PointerReleased);
            Target.PointerExited += new PointerEventHandler(Target_PointerExited);
            Target.PointerCanceled += new PointerEventHandler(Target_PointerCanceled);
            Target.PointerCaptureLost += new PointerEventHandler(Target_PointerCaptureLost);
            Target.PointerMoved += new PointerEventHandler(Target_PointerMoved);
            Target.PointerWheelChanged += new PointerEventHandler(Target_PointerWheelChanged);

            buttonClear.Click += new RoutedEventHandler(ButtonClear_Click);
        }

        private void ButtonClear_Click(object sender, RoutedEventArgs e)
        {
            eventLog.Text = "";
        }

```

### <span id="Handle_pointer_events"></span><span id="handle_pointer_events"></span><span id="HANDLE_POINTER_EVENTS"></span>处理指针事件

接下来，我们使用 UI 反馈来演示基本的指针事件处理程序。

-   此处理程序管理 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) 事件。 我们将事件添加到事件日志中，将指针添加到用于跟踪关注的指针的指针数组，并显示指针详细信息。

    **注意** [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) 和 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 事件并不总是成对出现。 你的应用应该侦听和处理可能会结束指针向下操作（如 [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969)、[**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964) 和 [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)）的所有事件。

     

```    CSharp
        // PointerPressed and PointerReleased events do not always occur in pairs. 
            // Your app should listen for and handle any event that might conclude a pointer down action 
            // (such as PointerExited, PointerCanceled, and PointerCaptureLost).
            // For this example, we track the number of contacts in case the 
            // number of contacts has reached the maximum supported by the device.
            // Depending on the device, additional contacts might be ignored 
            // (PointerPressed not fired). 
            void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
            {
                Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

                // Update event sequence.
                eventLog.Text += "\nDown: " + ptr.PointerId;

                // Change background color of target when pointer contact detected.
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

                // Prevent most handlers along the event route from handling the same event again.
                e.Handled = true;

                // Lock the pointer to the target.
                Target.CapturePointer(e.Pointer);

                // Update event sequence.
                eventLog.Text += "\nPointer captured: " + ptr.PointerId;

                // Check if pointer already exists (for example, enter occurred prior to press).
                if (contacts.ContainsKey(ptr.PointerId))
                {
                    return;
                }
                // Add contact to dictionary.
                contacts[ptr.PointerId] = ptr;
                ++numActiveContacts;

                // Display pointer details.
                createInfoPop(e);
            }
```

-   此处理程序管理 [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968) 事件。 我们将事件添加到事件日志、将指针添加指针集合，并显示指针详细信息。

```    CSharp
        private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
            {
                Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

                // Update event sequence.
                eventLog.Text += "\nEntered: " + ptr.PointerId;

                if (contacts.Count == 0)
                {
                    // Change background color of target when pointer contact detected.
                    Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
                }

                // Check if pointer already exists (if enter occurred prior to down).
                if (contacts.ContainsKey(ptr.PointerId))
                {
                    return;
                }

                // Add contact to dictionary.
                contacts[ptr.PointerId] = ptr;
                ++numActiveContacts;

                // Prevent most handlers along the event route from handling the same event again.
                e.Handled = true;

                // Display pointer details.
                createInfoPop(e);
            }
```

-   此处理程序管理 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970) 事件。 我们将事件添加到事件日志并更新指针详细信息。

    **重要提示** 鼠标输入与第一次检测到鼠标输入时分配的单个指针相关联。 单击鼠标按钮（左键、滚轮或右键）会通过 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) 事件在指针和该按钮之间创建一个辅助关联。 仅当释放该鼠标按钮时才引发 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 事件（在完成该事件之前，其他按钮无法与指针关联）。 由于此排他性关联，会通过 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970) 事件路由其他鼠标按钮单击。

     

```    CSharp
private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

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
        if (ptr.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
        {
            // To get mouse state, we need extended pointer details.
            // We get the pointer info through the getCurrentPoint method
            // of the event argument. 
            Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
            if (ptrPt.Properties.IsLeftButtonPressed)
            {
                eventLog.Text += "\nLeft button: " + ptrPt.PointerId;
            }
            if (ptrPt.Properties.IsMiddleButtonPressed)
            {
                eventLog.Text += "\nWheel button: " + ptrPt.PointerId;
            }
            if (ptrPt.Properties.IsRightButtonPressed)
            {
                eventLog.Text += "\nRight button: " + ptrPt.PointerId;
            }
        }

        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;

        // Display pointer details.
        updateInfoPop(e);
    }
```

-   此处理程序管理 [**PointerWheelChanged**](https://msdn.microsoft.com/library/windows/apps/br208973) 事件。 我们将事件添加到事件日志、将指针添加到指针数组（如有必要），并显示指针详细信息。

```    CSharp
private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nMouse wheel: " + ptr.PointerId;

        // Check if pointer already exists (for example, enter occurred prior to wheel).
        if (contacts.ContainsKey(ptr.PointerId))
        {
            return;
        }

        // Add contact to dictionary.
        contacts[ptr.PointerId] = ptr;
        ++numActiveContacts;

        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;

        // Display pointer details.
        createInfoPop(e);
    }
```

-   在已终止与数字化器的接触的情况下，此处理程序管理 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 事件。 我们将事件添加到事件日志、从指针集合删除指针，并更新指针详细信息。

```    CSharp
void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nUp: " + ptr.PointerId;

        // If event source is mouse or touchpad and the pointer is still 
        // over the target, retain pointer and pointer details.
        // Return without removing pointer from pointers dictionary.
        // For this example, we assume a maximum of one mouse pointer.
        if (ptr.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
        {
            // Update target UI.
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

            destroyInfoPop(ptr);

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            // Release the pointer from the target.
            Target.ReleasePointerCapture(e.Pointer);

            // Update event sequence.
            eventLog.Text += "\nPointer released: " + ptr.PointerId;

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }
        else
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
        }

    }
```

-   在维护与数字化器的接触的情况下，此处理程序管理 [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) 事件。 我们将事件添加到事件日志、从指针数组删除指针，并更新指针详细信息。

```    CSharp
private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nPointer exited: " + ptr.PointerId;

        // Remove contact from dictionary.
        if (contacts.ContainsKey(ptr.PointerId))
        {
            contacts[ptr.PointerId] = null;
            contacts.Remove(ptr.PointerId);
            --numActiveContacts;
        }

        // Update the UI and pointer details.
        destroyInfoPop(ptr);

        if (contacts.Count == 0)
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
        }
        
        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;
    }
```

-   此处理程序管理 [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964) 事件。 我们将事件添加到事件日志、从指针数组删除指针，并更新指针详细信息。

```    CSharp
// Fires for for various reasons, including: 
    //    - Touch contact canceled by pen coming into range of the surface.
    //    - The device doesn&#39;t report an active contact for more than 100ms.
    //    - The desktop is locked or the user logged off. 
    //    - The number of simultaneous contacts exceeded the number supported by the device.
    private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nPointer canceled: " + ptr.PointerId;

        // Remove contact from dictionary.
        if (contacts.ContainsKey(ptr.PointerId))
        {
            contacts[ptr.PointerId] = null;
            contacts.Remove(ptr.PointerId);
            --numActiveContacts;
        }

        destroyInfoPop(ptr);

        if (contacts.Count == 0)
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
        }
        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;
    }
```

-   此处理程序管理 [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965) 事件。 我们将事件添加到事件日志、从指针数组删除指针，并更新指针详细信息。

    **注意** 可能会发生 [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)，而不是 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972)。 指针捕获可能由于各种原因而丢失。

     

```    CSharp
// Fires for for various reasons, including: 
    //    - User interactions
    //    - Programmatic capture of another pointer
    //    - Captured pointer was deliberately released
    // PointerCaptureLost can fire instead of PointerReleased. 
    private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nPointer capture lost: " + ptr.PointerId;

        // Remove contact from dictionary.
        if (contacts.ContainsKey(ptr.PointerId))
        {
            contacts[ptr.PointerId] = null;
            contacts.Remove(ptr.PointerId);
            --numActiveContacts;
        }

        destroyInfoPop(ptr);

        if (contacts.Count == 0)
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
        }
        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;
    }
```

### <span id="Get_pointer_properties"></span><span id="get_pointer_properties"></span><span id="GET_POINTER_PROPERTIES"></span>获取指针属性

如前面所述，你必须从通过 [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) 的 [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/hh943077) 和 [**GetIntermediatePoints**](https://msdn.microsoft.com/library/windows/apps/hh943078) 方法获取的 [**Windows.UI.Input.PointerPoint**](https://msdn.microsoft.com/library/windows/apps/br242038) 对象获取最扩展的指针。

-   首先，我们为每个指针创建一个新的 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)。

```    CSharp
        void createInfoPop(PointerRoutedEventArgs e)
            {
                TextBlock pointerDetails = new TextBlock();
                Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                pointerDetails.Name = ptrPt.PointerId.ToString();
                pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
                pointerDetails.Text = queryPointer(ptrPt);

                TranslateTransform x = new TranslateTransform();
                x.X = ptrPt.Position.X + 20;
                x.Y = ptrPt.Position.Y + 20;
                pointerDetails.RenderTransform = x;

                Container.Children.Add(pointerDetails);
            }
```

-   然后，我们在与该指针关联的现有 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 中，提供一种更新指针信息的方式。

```    CSharp
        void updateInfoPop(PointerRoutedEventArgs e)
            {
                foreach (var pointerDetails in Container.Children)
                {
                    if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
                    {
                        TextBlock _TextBlock = (TextBlock)pointerDetails;
                        if (_TextBlock.Name == e.Pointer.PointerId.ToString())
                        {
                            // To get pointer location details, we need extended pointer info.
                            // We get the pointer info through the getCurrentPoint method
                            // of the event argument. 
                            Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                            TranslateTransform x = new TranslateTransform();
                            x.X = ptrPt.Position.X + 20;
                            x.Y = ptrPt.Position.Y + 20;
                            pointerDetails.RenderTransform = x;
                            _TextBlock.Text = queryPointer(ptrPt);
                        }
                    }
                }
            }
```

-   最后，我们将查询各种指针属性。

```    CSharp
         String queryPointer(PointerPoint ptrPt)
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

                 GeneralTransform gt = Target.TransformToVisual(page);
                 Point screenPoint;

                 screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
                 details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
                     "\nPointer location (parent): " + ptrPt.Position.X + ", " + ptrPt.Position.Y +
                     "\nPointer location (screen): " + screenPoint.X + ", " + screenPoint.Y;
                 return details;
             }
```

### <span id="Complete_example"></span><span id="complete_example"></span><span id="COMPLETE_EXAMPLE"></span>完整示例

下面是此示例的 C\# 代码。 有关更复杂示例的链接，请参阅此页面底部的相关文章。

```CSharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Input;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

// The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=234238

namespace PointerInput
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // For this example, we track simultaneous contacts in case the 
        // number of contacts has reached the maximum supported by the device.
        // Depending on the device, additional contacts might be ignored 
        // (PointerPressed not fired). 
        uint numActiveContacts;
        Windows.Devices.Input.TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();

        // Dictionary to maintain information about each active contact. 
        // An entry is added during PointerPressed/PointerEntered events and removed 
        // during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
        Dictionary<uint, Windows.UI.Xaml.Input.Pointer> contacts;

        public MainPage()
        {
            this.InitializeComponent();
            numActiveContacts = 0;
            // Initialize the dictionary.
            contacts = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>((int)touchCapabilities.Contacts);
            // Declare the pointer event handlers.
            Target.PointerPressed += new PointerEventHandler(Target_PointerPressed);
            Target.PointerEntered += new PointerEventHandler(Target_PointerEntered);
            Target.PointerReleased += new PointerEventHandler(Target_PointerReleased);
            Target.PointerExited += new PointerEventHandler(Target_PointerExited);
            Target.PointerCanceled += new PointerEventHandler(Target_PointerCanceled);
            Target.PointerCaptureLost += new PointerEventHandler(Target_PointerCaptureLost);
            Target.PointerMoved += new PointerEventHandler(Target_PointerMoved);
            Target.PointerWheelChanged += new PointerEventHandler(Target_PointerWheelChanged);

            buttonClear.Click += new RoutedEventHandler(ButtonClear_Click);
        }

        private void ButtonClear_Click(object sender, RoutedEventArgs e)
        {
            eventLog.Text = "";
        }


        // PointerPressed and PointerReleased events do not always occur in pairs. 
        // Your app should listen for and handle any event that might conclude a pointer down action 
        // (such as PointerExited, PointerCanceled, and PointerCaptureLost).
        // For this example, we track the number of contacts in case the 
        // number of contacts has reached the maximum supported by the device.
        // Depending on the device, additional contacts might be ignored 
        // (PointerPressed not fired). 
        void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nDown: " + ptr.PointerId;

            // Change background color of target when pointer contact detected.
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Lock the pointer to the target.
            Target.CapturePointer(e.Pointer);

            // Update event sequence.
            eventLog.Text += "\nPointer captured: " + ptr.PointerId;

            // Check if pointer already exists (for example, enter occurred prior to press).
            if (contacts.ContainsKey(ptr.PointerId))
            {
                return;
            }
            // Add contact to dictionary.
            contacts[ptr.PointerId] = ptr;
            ++numActiveContacts;

            // Display pointer details.
            createInfoPop(e);
        }

        void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nUp: " + ptr.PointerId;

            // If event source is mouse or touchpad and the pointer is still 
            // over the target, retain pointer and pointer details.
            // Return without removing pointer from pointers dictionary.
            // For this example, we assume a maximum of one mouse pointer.
            if (ptr.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
            {
                // Update target UI.
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

                destroyInfoPop(ptr);

                // Remove contact from dictionary.
                if (contacts.ContainsKey(ptr.PointerId))
                {
                    contacts[ptr.PointerId] = null;
                    contacts.Remove(ptr.PointerId);
                    --numActiveContacts;
                }

                // Release the pointer from the target.
                Target.ReleasePointerCapture(e.Pointer);

                // Update event sequence.
                eventLog.Text += "\nPointer released: " + ptr.PointerId;

                // Prevent most handlers along the event route from handling the same event again.
                e.Handled = true;
            }
            else
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
            }

        }

        private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

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
            if (ptr.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
            {
                // To get mouse state, we need extended pointer details.
                // We get the pointer info through the getCurrentPoint method
                // of the event argument. 
                Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                if (ptrPt.Properties.IsLeftButtonPressed)
                {
                    eventLog.Text += "\nLeft button: " + ptrPt.PointerId;
                }
                if (ptrPt.Properties.IsMiddleButtonPressed)
                {
                    eventLog.Text += "\nWheel button: " + ptrPt.PointerId;
                }
                if (ptrPt.Properties.IsRightButtonPressed)
                {
                    eventLog.Text += "\nRight button: " + ptrPt.PointerId;
                }
            }

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Display pointer details.
            updateInfoPop(e);
        }

        private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nEntered: " + ptr.PointerId;

            if (contacts.Count == 0)
            {
                // Change background color of target when pointer contact detected.
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
            }

            // Check if pointer already exists (if enter occurred prior to down).
            if (contacts.ContainsKey(ptr.PointerId))
            {
                return;
            }

            // Add contact to dictionary.
            contacts[ptr.PointerId] = ptr;
            ++numActiveContacts;

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Display pointer details.
            createInfoPop(e);
        }

        private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nMouse wheel: " + ptr.PointerId;

            // Check if pointer already exists (for example, enter occurred prior to wheel).
            if (contacts.ContainsKey(ptr.PointerId))
            {
                return;
            }

            // Add contact to dictionary.
            contacts[ptr.PointerId] = ptr;
            ++numActiveContacts;

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Display pointer details.
            createInfoPop(e);
        }

        // Fires for for various reasons, including: 
        //    - User interactions
        //    - Programmatic capture of another pointer
        //    - Captured pointer was deliberately released
        // PointerCaptureLost can fire instead of PointerReleased. 
        private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nPointer capture lost: " + ptr.PointerId;

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            destroyInfoPop(ptr);

            if (contacts.Count == 0)
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
            }
            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }

        // Fires for for various reasons, including: 
        //    - Touch contact canceled by pen coming into range of the surface.
        //    - The device doesn&#39;t report an active contact for more than 100ms.
        //    - The desktop is locked or the user logged off. 
        //    - The number of simultaneous contacts exceeded the number supported by the device.
        private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nPointer canceled: " + ptr.PointerId;

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            destroyInfoPop(ptr);

            if (contacts.Count == 0)
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
            }
            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }

        private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nPointer exited: " + ptr.PointerId;

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            // Update the UI and pointer details.
            destroyInfoPop(ptr);

            if (contacts.Count == 0)
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
            }
            
            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }

        void createInfoPop(PointerRoutedEventArgs e)
        {
            TextBlock pointerDetails = new TextBlock();
            Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
            pointerDetails.Name = ptrPt.PointerId.ToString();
            pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
            pointerDetails.Text = queryPointer(ptrPt);

            TranslateTransform x = new TranslateTransform();
            x.X = ptrPt.Position.X + 20;
            x.Y = ptrPt.Position.Y + 20;
            pointerDetails.RenderTransform = x;

            Container.Children.Add(pointerDetails);
        }

        void destroyInfoPop(Windows.UI.Xaml.Input.Pointer ptr)
        {
            foreach (var pointerDetails in Container.Children)
            {
                if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
                {
                    TextBlock _TextBlock = (TextBlock)pointerDetails;
                    if (_TextBlock.Name == ptr.PointerId.ToString())
                    {
                        Container.Children.Remove(pointerDetails);
                    }
                }
            }
        }

        void updateInfoPop(PointerRoutedEventArgs e)
        {
            foreach (var pointerDetails in Container.Children)
            {
                if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
                {
                    TextBlock _TextBlock = (TextBlock)pointerDetails;
                    if (_TextBlock.Name == e.Pointer.PointerId.ToString())
                    {
                        // To get pointer location details, we need extended pointer info.
                        // We get the pointer info through the getCurrentPoint method
                        // of the event argument. 
                        Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                        TranslateTransform x = new TranslateTransform();
                        x.X = ptrPt.Position.X + 20;
                        x.Y = ptrPt.Position.Y + 20;
                        pointerDetails.RenderTransform = x;
                        _TextBlock.Text = queryPointer(ptrPt);
                    }
                }
            }
        }

         String queryPointer(PointerPoint ptrPt)
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

             GeneralTransform gt = Target.TransformToVisual(page);
             Point screenPoint;

             screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
             details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
                 "\nPointer location (parent): " + ptrPt.Position.X + ", " + ptrPt.Position.Y +
                 "\nPointer location (screen): " + screenPoint.X + ", " + screenPoint.Y;
             return details;
         }
    }
}
```

## <span id="related_topics"></span>相关文章


**示例**
* [基本输入示例](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延迟输入示例](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [用户交互模式示例](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [焦点视觉示例](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**存档示例**
* [输入：XAML 用户输入事件示例](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [输入：设备功能示例](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [输入：操作和手势 (C++) 示例](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [输入：触摸点击测试示例](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML 滚动、平移以及缩放示例](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [输入：简化的墨迹示例](http://go.microsoft.com/fwlink/p/?linkid=246570)
 

 







<!--HONumber=Jun16_HO3-->


