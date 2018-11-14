---
author: Karl-Bridge-Microsoft
Description: Create Universal Windows Platform (UWP) apps with intuitive and distinctive user interaction experiences that are optimized for touch but are functionally consistent across input devices.
title: 触控交互
ms.assetid: DA6EBC88-EB18-4418-A98A-457EA1DEA88A
label: Touch interactions
template: detail.hbs
keywords: 键盘, 指针, 输入, 用户交互
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fbb2b6e5edee47d75d7115a38f95abf5ae71529a
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6256586"
---
# <a name="touch-interactions"></a>触摸交互


以“触摸将是用户的主要输入方法”为初衷设计应用。 如果你使用 UWP 控件，不要求额外编程，即可支持触摸板、鼠标和笔/触笔，因为 UWP 应用免费提供此类支持。

但是请记住，针对触摸优化的 UI 并非总是优于传统 UI。 两者都具有特定于技术和应用的优缺点。 在转换到主要使用触摸的 UI 时，了解触摸（包括触摸板）、笔/触笔、鼠标和键盘输入之间的核心差别很重要。

> **重要 API**：[**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)、[**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)、[**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)


许多设备具有多点触控屏幕，它们支持使用一根或多根手指（或触摸接触）作为输入。 触摸接触以及其移动会被解释为触摸手势和操作，以支持各种用户交互。

通用 Windows 平台 (UWP) 包括多种处理触摸输入的不同机制，从而你可以创建用户能够放心浏览的沉浸式体验。 下面我们将介绍在 UWP 应用中使用触摸输入的基本知识。

触摸交互需要满足以下三项：

-   触摸式屏幕。
-   单指或多指直接接触该屏幕（或近距离接触，如果显示器具有邻近感应传感器并支持悬停检测）。
-   触摸接触时移动（或不移动，具体取决于时间阈值）。

触摸传感器提供的输入数据可以：

-   解释为直接操作一个或多个 UI 元素的物理手势（例如平移、旋转、调整大小或移动）。 相比之下，通过某个元素属性窗口、对话框或其他 UI 提示与该元素交互被认为是间接操作。
-   作为另一种输入方法识别，例如鼠标或笔。
-   用于补充或修改其他输入方法的方面，例如涂抹用笔绘制的笔划墨迹。

触摸输入通常涉及屏幕上元素的直接操作。 该元素会立即响应其点击测试区内的任何触摸接触，并相应地响应该触摸接触的任何后续移动，包括删除。

应该仔细设计自定义触摸手势和交互。 它们应该是直观、响应且易于发现的，并且应让用户放心浏览你的应用。

确保应用功能在每个受支持的输入设备类型上显示一致。 如有必要，请使用某种形式的间接输入模式，例如用于键盘交互的文本输入，或用于鼠标和笔的 UI 提示。

请记住，传统输入设备（例如鼠标和键盘）为许多用户所熟悉并且具有吸引力。 它们可以提供快速、准确的触觉反馈，这是触摸无法提供的。

为所有输入设备提供独特鲜明的交互体验将支持最广泛的功能和首选项、吸引尽可能最多的受众，并为你的应用吸引更多客户。

## <a name="compare-touch-interaction-requirements"></a>对比触摸交互要求

下表显示了设计为触摸而优化的 UWP 应用时应该考虑的一些输入设备之间的不同。

<table>
<tbody><tr><th>因素</th><th>触摸交互</th><th>鼠标、键盘、笔/触笔交互</th><th>触摸板</th></tr>
<tr><td rowspan="3">精度</td><td>指尖的接触区域大于单个 x-y 坐标，这样便增加了无意中激活命令的几率。</td><td>鼠标和笔/触笔提供了精确的 x-y 坐标。</td><td>与鼠标相同。</td></tr>
<tr><td>接触区域的形状在整个移动过程中不断变化。  </td><td>鼠标移动和笔/触笔笔划都提供精确的 x-y 坐标。 键盘焦点非常清晰。</td><td>与鼠标相同。</td></tr>
<tr><td>没有鼠标光标来帮助确定目标。</td><td>鼠标光标、笔/触笔光标以及键盘焦点都可以帮助确定目标。</td><td>与鼠标相同。</td></tr>
<tr><td rowspan="3">人体解剖学</td><td>指尖移动并不精确，因为使用一个或多个手指沿直线移动非常困难。 这是由于手关节的曲率和运动涉及的关节数量导致的。</td><td>使用鼠标或笔/触笔进行直线移动就很容易，因为控制它们的手所移动的物理距离要比光标在屏幕上移动的物理距离短。</td><td>与鼠标相同。</td></tr>
<tr><td>由于手指的姿势以及用户对设备的控制，显示设备触摸表面上的某些区域可能很难接触到。</td><td>鼠标和笔/触笔可以达到屏幕的任何一个部分，同时任何控件都应该可以通过键盘按照 Tab 键顺序进行访问。 </td><td>手指姿势和抓握可能产生问题。</td></tr>
<tr><td>一个或多个指尖或用户的手可能会遮住对象。 这称为封闭。</td><td>间接输入设备不会造成封闭。</td><td>与鼠标相同。</td></tr>
<tr><td>对象状态</td><td>触摸使用两个状态的模型：显示设备的触摸表面为已触摸（打开）或未触摸（关闭）。 没有可以触发其他视觉反馈的悬停状态。</td><td>
<p>鼠标、笔/触笔以及键盘全都显示三种状态的模型：向上（关闭）、向下（打开）以及悬停（聚焦）。</p>
<p>悬停允许用户通过与 UI 元素关联的工具提示来了解信息。 悬停和聚焦效果都可以传达哪些对象是交互对象，并且还可以帮助确定目标。 
</p>
</td><td>与鼠标相同。</td></tr>
<tr><td rowspan="2">丰富交互</td><td>支持多点触摸：触摸表面上的多个输入点（指尖）。</td><td>支持单一输入点。</td><td>与触摸相同。</td></tr>
<tr><td>通过手势（点击、拖动、滑动、收缩和旋转）支持对象的直接操作。</td><td>不支持直接操作，因为鼠标、笔/触笔以及键盘为间接输入设备。</td><td>与鼠标相同。</td></tr>
</tbody></table>



**请注意**间接输入已经过 25 年的优化的好处。 设计诸如悬停触发的工具提示之类的功能，是为了解决触摸板、鼠标、笔/触笔以及键盘输入特有的 UI 浏览。 此类 UI 功能已针对触摸输入提供的丰富体验进行了重新设计，不会对这些其他设备的用户体验产生负面影响。

 

## <a name="use-touch-feedback"></a>用户触摸反馈

与你的应用交互期间适当的视觉反馈可帮助用户识别、 了解以及适应应用和 Windowsplatform 如何解释其交互。 视觉反馈可以指示成功交互、延迟系统状态、加强控制感觉、减少错误、帮助用户了解系统和输入设备并鼓励交互。

当用户依赖触摸屏输入来进行要求基于位置的准确活动时，视觉反馈非常重要。 无论何时何地检测到触摸输入都显示反馈，以帮助用户了解应用及其控件定义的任何自定义定向规则。


## <a name="targeting"></a>定向

通过以下方式优化目标：

-   触摸目标大小

    清晰的大小指南确保应用程序提供舒适的 UI，即 UI 中包含的对象和控件都很容易确定目标并且非常安全。

-   接触几何图形

    手指的整个接触区域可以确定最可能的目标对象。

-   推移

    通过将手指在组中的项之间拖动可以很容易改变目标（例如，单选按钮）。 释放触摸时激活当前项。

-   摇摆

    密集项目（如超链接）可以通过下压手指（无滑动），然后来回摇摆并停在项目上方来轻松改变目标。 由于存在封闭，当前项通过工具提示或状态栏来标识，并且释放触摸时会被激活。

## <a name="accuracy"></a>准确性

使用以下方式设计草率交互：

-   用户与内容交互时使用吸附点可以轻松在所需位置停止。
-   使用方向“围栏”可帮助进行垂直或水平平移，甚至手出现轻微弧度的移动时都可以。有关详细信息，请参阅[平移指南](guidelines-for-panning.md)。

## <a name="occlusion"></a>封闭

通过以下方式避免出现手指和手封闭：

-   UI 的大小和位置

    使 UI 元素足够大，以便指尖接触区域无法完全覆盖。

    将菜单和弹出窗口尽可能放在接触区域上方。

-   工具提示

    当用户在对象上保持手指接触时，显示工具提示。 这对于描述对象功能非常有用。 用户可将指尖拖动到对象外，以避免调用工具提示。

    对于小型对象，偏移工具提示以便指尖接触区域不会将对象覆盖。 这对于确定目标非常有用。

-   精确句柄

    如果对精度有要求（例如文本选择），请提供偏移选择句柄以提高准确性。 有关详细信息，请参阅[选择文本和图像的指南（Windows 运行时应用）](guidelines-for-textselection.md)。

## <a name="timing"></a>定时

在直接操作时，避免定时模式更改。 直接操作模拟对象的直接、实时的物理处理。 对象随着手指移动作出响应。

另一方面，定时交互发生在触摸交互之后。 定时交互通常依赖看不见的阈值（如时间、距离或速度）来确定要执行的命令。 定时交互在系统执行操作之前没有视觉反馈。

与定时交互相比，直接操作提供了很多优势：

-   交互期间即时视觉反馈使用户感觉更吸引人、更自信并且控制力更好。
-   直接操作比浏览系统更安全，因为这些操作是可逆的—用户可以采用逻辑和直观的方式轻松倒退其操作。
-   直接影响对象的交互以及模拟现实的交互更直观、更容易发现并且更不容易忘记。 它们不依赖于模糊或抽象的交互。
-   定时交互可能难以执行，因为用户数量必须达到任意且不可见的阈值。

此外，还强烈建议遵循以下规则：

-   不应该按使用的手指数量来区分操作。
-   交互应该支持复合操作。 例如，在拖动手指进行平移时收缩即可缩放。
-   不应按时间来区分交互。 相同的交互应该具有相同的结果，而与执行该操作所花费的时间无关。 基于时间的激活功能为用户引入了强制延迟，因此影响了直接操作的沉浸式属性和系统响应的感知。

    **请注意**例外情况是，使用特定的定时的交互来帮助了解和探究 （如示例，请按住）。

     

-   正确的描述以及视觉提示对高级交互的使用有巨大影响。


## <a name="app-views"></a>应用视图


通过应用视图的平移/滚动和缩放设置来调整用户交互体验。 应用视图指示用户访问和操作你的应用及其内容的方式。 视图还提供一些行为，如惯性、内容边界回弹和吸附点。

[**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527) 控件的平移和滚动设置指示用户在视图内容太多而无法放在视口中时，如何在单个视图内进行导航。 例如，单个视图可以是杂志或书的某一页、计算机的文件夹结构、文档库，也可以是某个相册。

缩放设置适用于光学缩放（受 [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527) 控件支持）和 [**Semantic Zoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) 控件。 语义式缩放是一种触摸优化技术，它在一个视图内呈现和导航大量的相关数据或内容集。 它使用两个不同的分类模式（或缩放级别）进行工作。 这类似于在单个视图中平移和滚动。 平移和滚动可以与语义式缩放一起使用。

使用应用视图和事件来修改平移/滚动和缩放行为。 这可以比处理指针和手势事件提供更流畅的交互体验。

有关应用视图的详细信息，请参阅[控件、布局和文本](https://msdn.microsoft.com/library/windows/apps/mt228348)。

## <a name="custom-touch-interactions"></a>自定义触摸交互


如果你实现自己的交互支持，请记住，用户期望获得直观的体验，包括直接与应用中的 UI 元素交互。 我们建议你根据“平台控件库”创建自定义交互的模型，以使内容保持一致且易于发现。 这些库中的控件提供完整的用户交互体验，包括标准交互、动态显示的物理效果、视觉反馈和辅助功能。 仅当要求清楚、定义良好且基本交互不支持你的方案时才创建自定义交互。

若要提供自定义触摸支持，应处理各种 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 事件。 这些事件分组为三个抽象级别。

-   静态手势事件是在交互完成之后触发的。 手势事件包括 [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985)、[**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/br208922)、[**RightTapped**](https://msdn.microsoft.com/library/windows/apps/br208984) 和 [**Holding**](https://msdn.microsoft.com/library/windows/apps/br208928)。

    可以通过将 [**IsTapEnabled**](https://msdn.microsoft.com/library/windows/apps/br208939)、[**IsDoubleTapEnabled**](https://msdn.microsoft.com/library/windows/apps/br208931)、[**IsRightTapEnabled**](https://msdn.microsoft.com/library/windows/apps/br208937) 和 [**IsHoldingEnabled**](https://msdn.microsoft.com/library/windows/apps/br208935) 设置为 **false** 来禁用针对特定元素的手势事件。

-   诸如 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) 和 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970) 的指针事件会提供每个触摸接触的低级别详细信息，包括指针动作以及区分按下和释放事件的能力。

    指针是具有统一事件机制的通用输入类型。 它将显示活动输入源（触摸屏、触摸板、鼠标或笔）的基本信息，例如屏幕位置。

-   [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950) 等操作手势事件表明一个持续的交互。 它们在用户触摸元素时开始引发，一直持续到用户抬起手指或者操作被取消时。

    操作事件包括多点触控交互（例如，缩放、平移或旋转）和使用惯性和速度数据的交互（例如拖动）。 操作事件提供的信息并不标识所执行的交互的形式，而是包括诸如位置、转换增量和速度等数据。 你可以使用此触摸数据来确定应执行的交互类型。

下面是一组基本的受 UWP 支持的触摸手势。

| 名称           | 类型                 | 说明                                                                            |
|----------------|----------------------|----------------------------------------------------------------------------------------|
| 点击            | 静态手势       | 用一个手指触摸屏幕，然后抬起手指。                                            |
| 长按 | 静态手势       | 用一个手指触摸屏幕并保持不动。                                      |
| 滑动          | 操作手势 | 用一个或多个手指触摸屏幕并向着同一方向移动。                   |
| 轻扫          | 操作手势 | 用一个或多个手指触摸屏幕并向着同一方向移动较短距离。  |
| 转动           | 操作手势 | 用两个或多个手指触摸屏幕并沿着顺时针或逆时针的弧线移动。 |
| 收缩          | 操作手势 | 用两个或多个手指触摸屏幕，然后将手指并拢在一起。                         |
| 拉伸        | 操作手势 | 用两个或多个手指触摸屏幕，然后将手指分开。                           |

 

<!-- mijacobs: Removing for now. We don't have a real page to link to yet. 
For more info about gestures, manipulations, and interactions, see [Custom user interactions](custom-user-input-portal.md).
-->

## <a name="gesture-events"></a>手势事件


有关个别控件的详细信息，请参阅[控件列表](https://msdn.microsoft.com/library/windows/apps/mt185406)。

## <a name="pointer-events"></a>指针事件


指针事件由各种活动输入源引发，包括触摸、触摸板、笔和鼠标（它们替代传统的鼠标事件）。

指针事件基于单一输入点（手指、笔尖、鼠标光标），但不支持基于速度的交互。

下面提供指针事件列表及其相关的事件参数。

| 事件或类                                                       | 说明                                                   |
|----------------------------------------------------------------------|---------------------------------------------------------------|
| [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971)             | 在单根手指触摸屏幕时发生。               |
| [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972)           | 在该同一触摸接触抬起时发生。                |
| [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970)                 | 在屏幕上拖动指针时发生。         |
| [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968)             | 在指针进入元素的点击测试区时发生。 |
| [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969)               | 在指针退出元素的点击测试区时发生。  |
| [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964)           | 在异常丢失触摸接触时发生。               |
| [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)     | 在另一个元素进行指针捕获时发生。    |
| [**PointerWheelChanged**](https://msdn.microsoft.com/library/windows/apps/br208973)   | 在鼠标滚轮的增量值更改时发生。         |
| [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) | 为所有指针事件提供数据。                         |

 

以下示例显示如何使用 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971)、[**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 和 [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) 事件来处理 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 对象上的点击交互。

首先，在 Extensible Application Markup Language (XAML) 中创建名为 `touchRectangle` 的 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)。

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
           Height="100" Width="200" Fill="Blue" />
</Grid>
```
接下来，指定用于 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971)、[**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 和 [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) 事件的侦听器。

```cpp
MainPage::MainPage()
{
    InitializeComponent();

    // Pointer event listeners.
    touchRectangle->PointerPressed += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerPressed);
    touchRectangle->PointerReleased += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerReleased);
    touchRectangle->PointerExited += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerExited);
}
```

```cs
public MainPage()
{
    this.InitializeComponent();

    // Pointer event listeners.
    touchRectangle.PointerPressed += touchRectangle_PointerPressed;
    touchRectangle.PointerReleased += touchRectangle_PointerReleased;
    touchRectangle.PointerExited += touchRectangle_PointerExited;
}
```

```vb
Public Sub New()

    ' This call is required by the designer.
    InitializeComponent()

    ' Pointer event listeners.
    AddHandler touchRectangle.PointerPressed, AddressOf touchRectangle_PointerPressed
    AddHandler touchRectangle.PointerReleased, AddressOf Me.touchRectangle_PointerReleased
    AddHandler touchRectangle.PointerExited, AddressOf touchRectangle_PointerExited

End Sub
```

最后，[**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) 事件处理程序增加 [**Rectangle**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 的 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 和 [**Width**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)，同时 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 和 [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) 事件处理程序将 **Height** 和 **Width** 设置回其初始值。

```cpp
// Handler for pointer exited event.
void MainPage::touchRectangle_PointerExited(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Pointer moved outside Rectangle hit test area.
    // Reset the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 200;
        rect->Height = 100;
    }
}

// Handler for pointer released event.
void MainPage::touchRectangle_PointerReleased(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Reset the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 200;
        rect->Height = 100;
    }
}

// Handler for pointer pressed event.
void MainPage::touchRectangle_PointerPressed(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Change the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 250;
        rect->Height = 150;
    }
}
```

```cs
// Handler for pointer exited event.
private void touchRectangle_PointerExited(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Pointer moved outside Rectangle hit test area.
    // Reset the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 200;
        rect.Height = 100;
    }
}
// Handler for pointer released event.
private void touchRectangle_PointerReleased(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Reset the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 200;
        rect.Height = 100;
    }
}

// Handler for pointer pressed event.
private void touchRectangle_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Change the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 250;
        rect.Height = 150;
    }
}
```

```vb
' Handler for pointer exited event.
Private Sub touchRectangle_PointerExited(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Pointer moved outside Rectangle hit test area.
    ' Reset the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 200
        rect.Height = 100
    End If
End Sub

' Handler for pointer released event.
Private Sub touchRectangle_PointerReleased(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Reset the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 200
        rect.Height = 100
    End If
End Sub

' Handler for pointer pressed event.
Private Sub touchRectangle_PointerPressed(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Change the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 250
        rect.Height = 150
    End If
End Sub
```

## <a name="manipulation-events"></a>操作事件


如果你需要在应用中支持多个手指交互或需要速度数据的交互，请使用操作事件。

你可以使用操作事件来检测拖动、缩放和按住之类的交互。

下面提供操作事件列表及其相关的事件参数。

| 事件或类                                                                                               | 说明                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| [**ManipulationStarting 事件**](https://msdn.microsoft.com/library/windows/apps/br208951)                                   | 在首次创建操作处理器时发生。                                                                                  |
| [**ManipulationStarted 事件**](https://msdn.microsoft.com/library/windows/apps/br208950)                                     | 在输入设备在 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 上开始操作时发生。                                            |
| [**ManipulationDelta 事件**](https://msdn.microsoft.com/library/windows/apps/br208946)                                         | 在输入设备在操作期间更改位置时发生。                                                                      |
| [**ManipulationInertiaStarting 事件**](https://msdn.microsoft.com/library/windows/apps/hh702425)                | 在输入设备在操作期间与 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 对象失去联系和延迟开始时发生。 |
| [**ManipulationCompleted 事件**](https://msdn.microsoft.com/library/windows/apps/br208945)                                 | 在 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 上的操作和延迟完成时发生。                                          |
| [**ManipulationStartingRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702132)               | 提供 [**ManipulationStarting**](https://msdn.microsoft.com/library/windows/apps/br208951) 事件的数据。                                         |
| [**ManipulationStartedRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702101)                 | 提供 [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950) 事件的数据。                                           |
| [**ManipulationDeltaRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702051)                     | 提供 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) 事件的数据。                                               |
| [**ManipulationInertiaStartingRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702074) | 提供 [**ManipulationInertiaStarting**](https://msdn.microsoft.com/library/windows/apps/br208947) 事件的数据。                           |
| [**ManipulationVelocities**](https://msdn.microsoft.com/library/windows/apps/br242032)                                              | 描述操作发生的速度。                                                                                         |
| [**ManipulationCompletedRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702035)             | 提供 [**ManipulationCompleted**](https://msdn.microsoft.com/library/windows/apps/br208945) 事件的数据。                                       |

 

手势由一系列操作事件组成。 每个手势都从 [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950) 事件开始，如用户触摸屏幕时。

接下来，引发一个或多个 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) 事件。 例如，先触摸屏幕，然后在屏幕上拖动手指。 最后，在完成交互时引发 [**ManipulationCompleted**](https://msdn.microsoft.com/library/windows/apps/br208945) 事件。

**请注意**如果你没有触摸屏监视器，则可以使用鼠标和鼠标滚轮界面在模拟器中测试你的操作事件代码。

 

以下示例演示如何使用 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) 事件来处理 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 上的滑动交互以及在屏幕上移动它。

首先，采用 XAML 格式创建一个名为 `touchRectangle` 的 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)，其 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 和 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 为 200。

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
               Width="200" Height="200" Fill="Blue" 
               ManipulationMode="All"/>
</Grid>
```

接下来，创建一个名为 `dragTranslation` 的全局 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/br243027)，用于转换 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)。 在 **Rectangle** 上指定一个 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) 事件侦听器，并将 `dragTranslation` 添加到 **Rectangle** 的 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/br208980)。

```cpp
// Global translation transform used for changing the position of 
// the Rectangle based on input data from the touch contact.
Windows::UI::Xaml::Media::TranslateTransform^ dragTranslation;
```

```cs
// Global translation transform used for changing the position of 
// the Rectangle based on input data from the touch contact.
private TranslateTransform dragTranslation;
```

```vb
' Global translation transform used for changing the position of 
' the Rectangle based on input data from the touch contact.
Private dragTranslation As TranslateTransform
```

```cpp
MainPage::MainPage()
{
    InitializeComponent();

    // Listener for the ManipulationDelta event.
    touchRectangle->ManipulationDelta += 
        ref new ManipulationDeltaEventHandler(
            this, 
            &MainPage::touchRectangle_ManipulationDelta);
    // New translation transform populated in 
    // the ManipulationDelta handler.
    dragTranslation = ref new TranslateTransform();
    // Apply the translation to the Rectangle.
    touchRectangle->RenderTransform = dragTranslation;
}
```

```cs
public MainPage()
{
    this.InitializeComponent();

    // Listener for the ManipulationDelta event.
    touchRectangle.ManipulationDelta += touchRectangle_ManipulationDelta;
    // New translation transform populated in 
    // the ManipulationDelta handler.
    dragTranslation = new TranslateTransform();
    // Apply the translation to the Rectangle.
    touchRectangle.RenderTransform = this.dragTranslation;
}
```

```vb
Public Sub New()

    ' This call is required by the designer.
    InitializeComponent()

    ' Listener for the ManipulationDelta event.
    AddHandler touchRectangle.ManipulationDelta,
        AddressOf testRectangle_ManipulationDelta
    ' New translation transform populated in 
    ' the ManipulationDelta handler.
    dragTranslation = New TranslateTransform()
    ' Apply the translation to the Rectangle.
    touchRectangle.RenderTransform = dragTranslation

End Sub
```

最后，在 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) 事件处理程序中，使用 [**Delta**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 属性上的 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/br243027) 更新 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/hh702058) 的位置。

```cpp
// Handler for the ManipulationDelta event.
// ManipulationDelta data is loaded into the
// translation transform and applied to the Rectangle.
void MainPage::touchRectangle_ManipulationDelta(Object^ sender,
    ManipulationDeltaRoutedEventArgs^ e)
{
    // Move the rectangle.
    dragTranslation->X += e->Delta.Translation.X;
    dragTranslation->Y += e->Delta.Translation.Y;
    
}
```

```cs
// Handler for the ManipulationDelta event.
// ManipulationDelta data is loaded into the
// translation transform and applied to the Rectangle.
void touchRectangle_ManipulationDelta(object sender,
    ManipulationDeltaRoutedEventArgs e)
{
    // Move the rectangle.
    dragTranslation.X += e.Delta.Translation.X;
    dragTranslation.Y += e.Delta.Translation.Y;
}
```

```vb
' Handler for the ManipulationDelta event.
' ManipulationDelta data Is loaded into the
' translation transform And applied to the Rectangle.
Private Sub testRectangle_ManipulationDelta(
    sender As Object,
    e As ManipulationDeltaRoutedEventArgs)

    ' Move the rectangle.
    dragTranslation.X = (dragTranslation.X + e.Delta.Translation.X)
    dragTranslation.Y = (dragTranslation.Y + e.Delta.Translation.Y)

End Sub
```

## <a name="routed-events"></a>路由事件


此处提及的所有指针事件、手势事件和操作事件都将作为*路由事件*实现。 这意味着该事件可能由对象（而不是最初引起该事件的对象）处理。 对象树中的连续父对象（例如 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 元素的父容器或你的应用的根 [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503)）可以选择处理这些对象，即使原始元素未执行此操作也是如此。 相反，处理该事件的任何对象都可以标记处理的事件，以使其不再达到任何父元素。 有关路由事件概念以及它如何影响你为路由事件编写处理程序的方式的详细信息，请参阅[事件和路由事件概述](https://msdn.microsoft.com/library/windows/apps/hh758286)。

## <a name="dos-and-donts"></a>应做事项和禁止事项


-   设计将触摸交互作为预期的主要输入方法的应用程序。
-   为所有类型的交互（触摸、笔、触笔和鼠标等）提供视觉反馈
-   通过调整触摸目标大小、接触几何体、清理和摇动来优化定位。
-   使用吸附点和带方向性的“围栏”优化精确度。
-   对于紧凑的 UI 项目，提供工具提示和句柄以帮助提高触摸精确度。
-   尽量不要使用计时的交互（适当使用的示例：长按）。
-   尽量不要使用用于区别操作的手指个数。


## <a name="related-articles"></a>相关文章

* [处理指针输入](handle-pointer-input.md)
* [标识输入设备](identify-input-devices.md)

**示例**

* [基本输入示例](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延迟输入示例](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [用户交互模式示例](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [焦点视觉示例](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**存档示例**

* [输入：设备功能示例](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [输入：XAML 用户输入事件示例](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [XAML 滚动、平移以及缩放示例](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [输入：使用 GestureRecognizer 的笔势和操作](http://go.microsoft.com/fwlink/p/?LinkID=231605)
 

 




