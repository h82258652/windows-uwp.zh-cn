---
Description: 设计你的应用，以使其在电视上外观良好且运行正常。
title: 针对 Xbox 和电视进行设计
ms.assetid: 780209cb-3e8a-4cf7-8f80-8b8f449580bf
label: Designing for Xbox and TV
template: detail.hbs
isNew: true
---
<!--Note to Eliot from Linda: see my comments by searching on v-lcap; after your review, I recommend removing all commented out text unless you think you may need it later; it just gets in the way of an already long doc-->

> \[本文介绍了尚不可用的功能。 在进行商业发行之前，我们可能会对此功能进行大量修改。 Microsoft 不对此处提供的信息作任何明示或默示的担保。\]

# 针对 Xbox 和电视进行设计

设计你的通用 Windows 平台 (UWP) 应用，以便它在 Xbox One 和电视屏幕上外观良好且运行正常。

## 概述

通用 Windows 平台可使你在多台 Windows 10 设备上创建令人愉快的体验。 
UWP 框架提供的大部分功能使应用能够在这些设备上使用相同的用户界面 (UI)，而无需附加工作。 
但是，定制和优化你的应用以使其在 Xbox One 和电视屏幕上良好工作需要考虑特殊的注意事项。

坐在房间中的沙发上使用游戏板或遥控器与电视交互的体验称为 **10 英尺体验**。 
如此命名是因为用户通常坐在离屏幕大约 10 英尺的位置。 
这提出了 *2 英尺*体验（假设）中或与电脑交互时所不存在的独特挑战。 
如果你要为 Xbox One 或任何输出到电视屏幕和使用控制器进行输入的其他设备开发应用，应始终牢记这一点。

若要使你的应用非常适合 10 英尺体验，并不需要执行本文中的所有步骤，但了解这些步骤并为应用进行相应的决策可使 10 英尺体验更符合应用的特定需求。 
在将应用实际应用到 10 英尺环境中时，请考虑以下设计原则。

### 简单

针对 10 英尺环境进行设计提出了一组独特的挑战。 分辨率和观看距离可能使人难以处理太多信息。 
尝试使设计保持干净，尽可能减少到最简单的组件。 电视上所显示的信息量应与你在移动电话上（而不是桌面上）看到的内容相当。

![Xbox One 主屏幕](images/designing-for-tv/xbox-home-screen.png)

### 一致

10 英尺环境中的 UWP 应用应当直观且易于使用。 是重点清晰明显。 
安排内容，以使跨空间移动一致且可预测。 为用户提供指向所需操作的最短路径。

![Xbox One 电影应用](images/designing-for-tv/xbox-movies-app.png)

_**Microsoft 电影和电视中提供了屏幕截图中所示的所有电影。**_  

### 引人入胜

在大屏幕上可享受最沉浸式的电影体验。 边对边场景、优美的动作以及颜色和版式的灵活使用可将你的应用提升到下一层次。 兼具大胆和美观。

![Xbox One 虚拟形象应用](images/designing-for-tv/xbox-avatar-app.png)

### 针对 10 英尺体验的优化

现在你已了解了适用于 10 英尺体验的良好 UWP 应用设计的原则，请阅读以下关于优化应用和实现出色用户体验的特定方法的概述。
<!--[v-lcap] I recommend shortening the descriptions to the bare minimum, and focusing on the rest in the actual sections. I also rearranged the order of this table to map to the order you have in the document.-->


| 功能        | 说明           |
| -------------------------------------------------------------- |--------------------------------|
| [UI 元素大小调整](#ui-element-sizing)  | 通用 Windows 平台使用[缩放和有效像素](..\layout\design-and-ui-intro.md#effective-pixels-and-scaling)来根据观看距离缩放 UI。 当 UWP 应用在 Xbox One 上运行时，它使用相应的默认比例系数来使所有 UI 元素（如文本和常用控件）在房间中的沙发处轻松可见。 通过了解大小调整并在 UI 上应用它，有助于针对 10 英尺环境优化你的应用。  |
|  [电视安全区域](#tv-safe-area) | 由于历史和技术原因，并非所有电视都将内容一直显示到屏幕边缘。 默认情况下，UWP 将自动避免在不安全的区域（接近屏幕边缘的区域）显示任何 UI。 但是，这将导致“箱中”效果，即 UI 看起来以宽屏显示。 为了使你的应用在电视上提供真正的沉浸式体验，你将要对其进行修改，以使其延伸到电视上的屏幕边缘（如果电视支持此功能）。 |
| [颜色](#colors)  |  UWP 支持颜色主题，并且遵循系统主题的应用在 Xbox One 上将默认为**深色**。 如果你的应用具有特定的颜色主题，你应考虑到某些颜色不适用于电视，应避免使用它们。 为了尽可能得到最佳结果，请考虑为你的应用创建一个特定于电视的调色板。 |
| [游戏板和遥控器](#gamepad-and-remote-control)      | 确保你的应用适用于游戏板和遥控器是针对 10 英尺体验进行优化中最重要的步骤。 正确支持键盘交互和导航有助于使游戏板和遥控器输入相对良好地工作。 但是，你可以进行某些特定于游戏板和遥控器的改进，执行这些改进可优化用户操作在某种程度上受限的设备上的用户交互体验。 |
| [鼠标模式](#mouse-mode)|在某些用户界面（如地图和绘图图面）中，使用 XY 焦点导航不可行或不现实。 对于这些界面，UWP 提供了**鼠标模式**来使游戏板/遥控器自由导航，就像桌面计算机上的鼠标一样。|
| [焦点视觉对象](#focus-visual)  | 焦点视觉对象是当前具有焦点的 UI 元素周围的边框。 这有助于使用户定位，以便他们可以轻松导航你的 UI而不会迷失。 当焦点视觉对象在多台 Windows 10 设备上工作时，你将要确保它在电视上轻松可见，并且默认位于用户将与其经常交互的位置。 如果焦点不清晰可见，用户可能在 UI 中迷失，并且无法得到出色的体验。 另外，充分利用[轻型消除覆盖层](#light-dismiss-overlay)来将注意力吸引到用户当前正在与其交互的 UI。  |
| [XY 焦点导航和交互](#xy-focus-navigation-and-interaction) | UWP 提供 **XY 焦点导航**，该导航允许用户在应用的 UI 中四处导航。 但是，这会限制用户只能向上、向下、向左和向右导航。 本部分概述了处理此情况的建议和其他注意事项。 你可能还需要利用[焦点占用](#focus-engagement)（通过按 **Enter/“选择”**按钮来与 UI 元素交互）以使用特定控件，例如滑块。 这将减少从屏幕的一侧到另一侧获取的所需单击数。 | 
| [UI 控件指南](#guidelines-for-ui-controls)  |  你可以对应用进行某些对所有 Windows 10 设备（而不仅仅是 Xbox One 或其他 10 英尺体验）都有意义的改进。 请考虑在受 UWP 支持的所有设备上应用这些最佳做法（对电视尤其有益），而不仅仅是改进 10 英尺体验。  |

<!--[v-lcap] "Focus engagement" is an H2 section that precedes "XY focus"; I recommend either putting it in its own row in the table ahead of XY focus, or changing it to an H3 section under "XY focus," whichever makes more sense; just as long as the overview table  matches what you do-->

<!--| [Sound](../style/sound.md)  |  Sounds play a key role in the 10-foot experience, helping to immerse and give feedback to the user. The UWP provides functionality that automatically turns on sounds for common controls when the app is running on Xbox One. Find out more about the sound support built into the UWP and learn how to take advantage of it. |-->


<!--[v-lcap] I had to put this table into markdown because the links weren't rendering in HTML.
<table>
    <tr>
        <td> [Gamepad and remote control](#gamepad-and-remote-control)</td>
        <td>Making sure that your app works well with gamepad and remote is the most important step in optimizing for 10-foot experiences. Properly supporting keyboard interaction and navigation helps get gamepad and remote control input working relatively well. However, there are gamepad and remote-specific improvements that you can make to optimize the user interaction experience on a device where their actions are somewhat limited.
        </td>
    </tr>
    <tr>
        <td>[XY focus navigation and interaction](#xy-focus-navigation-and-interaction)</td>
        <td>The UWP provides **XY focus navigation** that allows the user to navigate around your app's UI. However, this limits the user to navigating up, down, left, and right. Recommendations for dealing with this and other considerations are outlined in this section. You may also need to utilize [focus engagement](#focus-engagement) (by pressing the **Enter/Select** button to interact with a UI element) to use certain controls, such as sliders. This cuts down on the number of clicks required to get from one side of the screen to the other.
        </td>
    </tr>
    <tr>
        <td>[Mouse mode](#mouse-mode)</td>
        <td>In some user interfaces, such as maps and drawing surfaces, it is not possible or practical to use XY focus navigation. For these interfaces, the UWP provides **mouse mode** to let the gamepad/remote navigate freely, like a mouse on a desktop computer.
        </td>
    </tr>
    <tr>
        <td>[Focus visual](#focus-visual)</td>
        <td>The focus visual is the border around the UI element that currently has focus. This helps orient the user so that they can easily navigate your UI without getting lost. While focus visual works across multiple Windows 10 devices, you will want to make sure that it is easy to see on TV and that it defaults to a location that the user will interact with often. If the focus is not clearly visible, the user could get lost in your UI and not have a great experience. Also, take advantage of [Light dismiss overlay](#light-dismiss-overlay) to call attention to UI that a user is currently interacting with.
        </td>
    </tr>
    <tr>
        <td>[UI element sizing](#ui-element-sizing)</td>
        <td>
            The Universal Windows Platform uses [scaling and effective pixels](..\layout\design-and-ui-intro.md#effective-pixels-and-scaling) to scale the UI according to the viewing distance. 
            When a UWP app is running on Xbox One, it uses an appropriate default scale factor in order to make all the UI elements, such as text and common controls, easily visible from your couch across the room. 
            Understanding sizing and applying it across your UI will help optimize your app for the 10-foot environment.
        </td>
    </tr>
    <tr>
        <td>[TV-safe area](#tv-safe-area)</td>
        <td>
            Not all TVs display content all the way to the edge of the screen due to historical as well as technological reasons. 
            The UWP will automatically avoid displaying any UI in unsafe areas (areas close to the edges of the screen) by default. However, this creates a "boxed-in" effect in which the UI looks letterboxed. 
            For your app to be truly immersive on TV, you will want to modify it so that it extends to the edges of the screen on TVs that support it.
        </td>
    </tr>
    <tr>
        <td>[Colors](#colors)</td>
        <td>
            The UWP supports color themes, and an app that respects the system theme will default to **dark** on Xbox One. 
            If your app has a specific color theme, you should consider that some colors don't work well for TV and should be avoided. 
            For the best possible results, consider creating a TV-specific color palette for your app.
        </td>
    </tr>
    <tr>
        <td>[Sound](../style/sound.md)</td>
        <td>
            Sounds play a key role in the 10-foot experience, helping to immerse and give feedback to the user. The UWP provides functionality that automatically turns on sounds for common controls when the app is running on Xbox One. Find out more about the sound support built into the UWP and learn how to take advantage of it.
        </td>
    </tr>
    <tr>
        <td>[Guidelines for UI controls](#guidelines-for-ui-controls)</td>
        <td>
            There are certain improvements you can make to your app that make sense for all Windows 10 devices, not just Xbox One or other 10-foot experiences. 
            Rather than only improving the 10-foot experience, consider applying these best practices across all devices supported by the UWP, which are particularly beneficial for TV.
        </td>
    </tr>
</table>-->

<!--### Gamepad and remote interaction

Users of your TV app won't be interacting with the UI with a high-precision input device such as a mouse, and this needs to be taken into account when designing your app's interface. Visibility of the **focus visual** (the border highlighting the UI element that the user is currently navigated to) is also key to making sure that the user doesn't get lost.

#### [Gamepad and remote control](#gamepad-and-remote-control)

Making sure that your app works well with gamepad and remote is the most important step in optimizing for TV. As mentioned in [Running UWP apps on Xbox One](#running-uwp-apps-on-xbox-one), properly supporting keyboard interaction and navigation helps get gamepad and remote control input working relatively well. However, there are gamepad- and remote-specific improvements that you can make to optimize the user interaction experience on a device where their actions are somewhat limited.

#### [Focus visual](#focus-visual)

While focus visual works across multiple Windows 10 devices, you will want to make sure that it is easy to see on TV and that it defaults to a location that the user will interact with often. If the focus is not clearly visible, the user could get lost in your UI and not have a great experience.

#### [2D focus navigation and interaction](#2d-focus-navigation-and-interaction)

UWP provides 2D navigation that allows the user to navigate around your app's UI. However, this limits the user to navigating up, down, left, and right. Recommendations for dealing with this and other considerations are outlined in [2D navigation best practices](#2d-navigation-best-practices). You may also need to utilize [focus engagement](#focus-engagement) (pressing the **Enter/Accept** button to interact with a UI element) to use certain controls, such as sliders. This cuts down on the number of clicks required to get from one side of the screen to the other.

#### [Mouse mode](#mouse-mode)

In some user interfaces, such as maps and drawing surfaces, it is not possible or practical to use 2D navigation. For these interfaces, UWP provides **mouse mode** to let the gamepad/remote navigate freely, like a mouse on a desktop computer.

### Layout and content density

Unlike with desktop apps, in which the user is close to the screen, a user interacting with your app on TV will likely be sitting in a couch across the room. Because of this, reducing content density is important so that the user can easily navigate your app. Remember: simplicity is key.

#### [UI element sizing](#ui-element-sizing)

The Universal Windows Platform uses [scaling and effective pixels](../layout/grid.md) to scale the UI according to the viewing distance. When a UWP app is running on Xbox One, it uses an appropriate default scale factor in order to make all the UI elements, such as text and common controls, easily visible from your couch across the room. Understanding sizing and applying it across your UI will help optimize your app for TV.

#### [TV-safe area](#tv-safe-area)

Not all TVs display content all the way to the edge of the screen due to historical as well as technological reasons. UWP will automatically avoid displaying any UI in unsafe areas (areas close to the edges of the screen) by default. However, this creates a "boxed-in" effect in which the UI looks letterboxed. For your app to be truly immersive on TV, you will want to modify it so that it extends to the edges of the screen on TVs that support it.

### Styles for TV

When designing for TV, there are special considerations in regards to color and sound. The styles you use may work great for other devices, but might need some optimizing to make them shine on TV.

#### [Colors](#colors)

UWP supports color themes, and an app that respects the system theme will default to **dark** on Xbox One. If your app has a specific color theme, you should consider that some colors don't work well for TV and should be avoided. For the best possible results, consider creating a TV-specific color palette for your app.

#### [Sound](../style/sound.md)

Sounds play a key role in TV apps, helping immerse the user in the interactive experience. Find out more about the sound support built into UWP and learn how to take advantage of it.

### [Improvements for all platforms](#improvements-for-all-platforms)

There are certain improvements you can make to your app that make sense for all Windows 10 platforms, not just Xbox One or other TV experiences. Rather than only improving the TV experience, consider applying these best practices that apply across all platforms UWP supports, and are particularly beneficial for TV.
-->

<!--## Running UWP apps on Xbox One

Some features for UWP in the 10-foot environment are built-in, and you will get these without any additional work once you properly port to Xbox One. However, there are other optimizations you may want to consider making so that your app performs as well as it can on Xbox. Learn about these features in the following sections.

### Gamepad and remote control support

Arrow key and tab stop behavior (pressing **Tab** to get to the next UI element) on keyboard informs how 2D navigation moves focus through the **D-pad** on a game controller or remote. The **Enter** and **Space** keys are also automatically mapped to the **Enter/Accept** key (see [Gamepad and remote control](#gamepad-and-remote-control)).

The quality of gamepad and remote behavior that you get out of the box depends on how well keyboard is supported in your app, and thus may vary greatly from app to app. A good way to ensure that your app will work well with gamepad/remote is to make sure that it works well with keyboard on PC, and then test with gamepad/remote to find weak spots in your UI.

### TV-safe area

Using the [VisibleBounds](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.applicationview.visiblebounds.aspx) functionality of UWP (for more information, see [TV-safe area](#tv-safe-area)), the following will be done automatically for your app:

* All UI elements are drawn inside the TV-safe area
* The page's background colors are drawn all the way to the edges of the TV

![TV-safe area](images/designing-for-tv/tv-safe-area.png)

### UI scaling

UWP supports proper scaling of UI based on the settings of the system on which the app is currently running. On desktop, this setting can be found in **Settings > System > Display** as a sliding value. This same setting exists on phone as well if the device supports it.

![Change the size of text, apps, and other items](images/designing-for-tv/ui-scaling.png)

On Xbox One there is no such system setting, however in order for UWP UI elements to be sized appropriately for TV, they are scaled at a default of **200%**. As long as UI elements are appropriately sized for other platforms, they will be appropriately sized for TV as well. For more information, see [UI element sizing](#ui-element-sizing).


### Focus visual

The same focus visual can be used for keyboard as well as game controller input, and will always be highly visible on any platform that supports focus visual, including Xbox One and the 10-foot experience. For more information, see [Focus visual](#focus-visual).

### Sound

UWP provides functionality that automatically turns on sounds for common controls when the app is running on Xbox One. This helps provide feedback to the user that they have interacted with the app in some way. For more information, see [Sound](../style/sound.md).

### Accent color and high contrast colors

As long as your app uses a brush resource such as **SystemControlForegroundAccentBrush**, or a color resource (**SystemAccentColor**), or instead calls accent colors directly through the [UIColorType.Accent*](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx) API, those colors are replaced with accent colors appropriate for TV. High contrast brush colors are also pulled in from the system the same way as on PC and phone, but with TV-appropriate colors.

### Light dismiss overlay

In order to call the user's attention to the UI elements that the user is currently manipulating with the game controller or remote control, UWP automatically adds a "smoke" layer that covers areas outside of the popup UI when the app is running on Xbox One. This requires no extra work, but is something to keep in mind when designing your UI.
-->

## UI 元素大小调整

由于 10 英尺环境中的应用的用户使用遥控器或游戏板，并且坐在离屏幕数英尺远的位置，因此需要将一些 UI 注意事项纳入设计中。 
确保 UI 具有相应的内容密度并且不过于凌乱，以便用户可以轻松地导航和选择元素。 记住：关键在于简洁性。

### 比例系数和自适应布局

**比例系数**有助于确保 UI 元素以适合正在运行应用的设备的大小显示。 
在桌面上，此设置在“设置”>“系统”>“显示”****中以滑动值的形式提供。 
如果设备支持，此相同设置也存在于手机上。

![更改文本、应用和其他项的大小](images/designing-for-tv/ui-scaling.png) 

在 Xbox One 上，没有此类系统设置；但是，对于要针对电视设置相应大小的 UWP UI 元素，将以 **200%** 的默认值缩放它们。 
只要 UI 元素针对其他设备设置相应的大小，也将针对电视设置相应的大小。 
Xbox One 以 1080p（1920 x 1080 像素）呈现你的应用。 因此，在从其他设备（如电脑）移植应用时， 
利用[自适应技术](https://msdn.microsoft.com/en-us/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design)确保 UI 在为 100% 缩放的 960 x 540 像素时外观出色。

针对 Xbox 进行设计与针对电脑进行设计稍有不同，因为你只需要考虑一个分辨率，即 1920 x 1080。 
用户的电视是否具有较高的分辨率不重要，UWP 应用将始终缩放为 1080p。

无论电视分辨率如何，当在 Xbox One 上运行时，还将为你的应用从 200% 集中提取正确的资源大小。

### 内容密度

在设计应用时，请记住用户将从一段距离外观看 UI 并使用遥控器或游戏控制器与其交互，这花费的导航时间比使用鼠标或触摸输入要多。

#### UI 控件的大小

交互式 UI 元素的大小应设置为最低高度 32 epx（有效像素）。 这是常用 UWP 控件的默认值，并且当以 200% 缩放使用时，它可确保 UI 元素在一段距离外可见，并且有助于降低内容密度。 

<!--For more information about effective pixels, see [Effective pixels](../layout/grid.md#effective-pixels).-->

![100% 和 200% 缩放的 UWP 按钮](images/designing-for-tv/button-100-200.png)

#### 单击数

当用户从电视屏幕的一侧导航到另一侧时，此操作不应超过**六次单击**，以便简化你的 UI。 同样，**简洁性**原则也适用于此处。 有关更多详细信息，请参阅[最少单击路径](#path-of-least-clicks)。

![跨 6 个图标](images/designing-for-tv/six-clicks.png)

### 文本大小

若要使你的 UI 在一段距离外可见，请使用以下经验法则：

* 主要文本和阅读内容：最低 15 epx
* 非关键文本和补充文本：最低 12 epx

当在 UI 中使用较大的文本时，请选取一个没有过分限制屏幕空间的大小，以免占用其他内容可能填充的空间。

### 选择退出比例系数

我们建议你的应用充分利用比例系数支持，这将有助于它通过针对每个设备类型进行缩放在所有设备上以相应方式运行。 
但是，可以选择退出此行为并以 100% 缩放设计你的所有 UI。 请注意，你无法将比例系数更改为任何 100% 以外的值。

你可以通过使用以下代码段选择退出比例系数：

```csharp
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```

`result` 将通知你是否已成功选择退出。

请务必计算 UI 元素的相应大小，方法是将本主题中提到的*有效*像素值乘以 2，即为*实际*像素值。

## 电视安全区域

由于历史和技术原因，并非所有电视都将内容一直显示到屏幕边缘。 默认情况下，UWP 将避免在电视不安全区域中显示任何 UI 内容，并将改为仅绘制页面背景。

电视不安全区域在下图中由蓝色区域表示。

![电视不安全区域](images/designing-for-tv/tv-unsafe-area.png)

你可以将背景设置为静态或主题色，或设置为图像，如以下代码段所示。

### 主题色

```xml
<Page x:Class="Sample.MainPage"
      Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"/>
```

### 图像

```xml
<Page x:Class="Sample.MainPage"
      Background="\Assets\Background.png"/>
```

这是你的应用在未进行附加工作时的外观。

![电视安全区域](images/designing-for-tv/tv-safe-area.png)

这不是最佳的外观，因为它使应用具有“箱中”效果，其中部分 UI（如导航窗格和网格）看似被切断。 但是，你可以进行优化以将部分 UI 延伸到屏幕边缘，以为应用提供更加电影化的效果。

### 将 UI 绘制到边缘

我们建议你使用特定 UI 元素来延伸到屏幕边缘，以向用户提供更多的沉浸式体验。 这些 UI 元素包括 [ScrollViewers](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx)、[nav panes](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/nav-pane) 和 [CommandBars](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx)。

另一方面，交互式元素和文本始终避开屏幕边缘以确保它们在某些电视上不会被切断，这一点也很重要。 我们建议你在屏幕边缘的 5% 内仅绘制非必要的视觉对象。 如 [UI 元素大小调整](#ui-element-sizing)中所述，遵循 Xbox One 控制台的默认比例系数 200% 的 UWP 应用将利用 960 x 540 epx 的区域，因此在应用的 UI 中，你应避免在以下区域中放置必需 UI：

- 距离顶部和底部 27 epx
- 距离左侧和右侧 48 epx

有两种方法可使 UI 延伸到屏幕边缘：*核心窗口边界*和*负边距*。

### 核心窗口边界

对于仅面向 10 英尺体验的 UWP 应用，使用核心窗口边界是更简单的选项。

在 `App.xaml.cs` 的 `OnLaunched` 方法中，添加以下代码：

```csharp
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode
    (Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```

通过此代码行，应用窗口将延伸到屏幕边缘，因此你需要将所有交互式和必需 UI 移动到之前所述的电视安全区域中。 临时 UI（如上下文菜单和打开的 [ComboBoxes](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.combobox.aspx)）将自动保留在电视安全区域内。

![核心窗口边界](images/designing-for-tv/core-window-bounds.png)

### 负边距

对于面向移动设备、桌面设备和 Xbox One 等各种设备的 UWP 应用，负边距可能是定制自适应布局的更直观的方法。 
我们建议你创建一个[自定义触发器](#custom-visual-state-trigger-for-xbox-one)并针对电视布局修改边距。

#### 窗格背景 

<!--[v-lcap] Do you mean "panel" or "pane" in this title and paragraph? 03-29-16 - updated to "pane" per Chigusa-->

导航窗格通常绘制在屏幕边缘附近，因此背景应延伸到电视不安全区域中，以免引入不可取的间距。 
你可以在 [SplitView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.splitview.aspx) 控件上对负边距执行此操作， 
（该控件通常用作导航窗格构建基块），而 `SplitView` 的内容上的正边距可将其保留在电视安全区域内。

![延伸到屏幕边缘的导航窗格](images/designing-for-tv/tv-safe-areas-2.png)

在此处，导航窗格的背景已延伸到屏幕边缘，而其导航项会保留在电视安全区域内。 
`SplitView` 的内容（在本例中为项的网格）已延伸到屏幕底部，以便它看起来会持续显示且不会被切断，而网格顶部仍然位于电视安全区域内。 你将在本部分的后面部分了解如何将具有焦点的项同样保留在电视安全区域中。

以下代码段可实现此效果：

```xml
<SplitView x:Name="RootSplitView"
           Margin="-48, -27">
            <SplitView.Pane>
                 <ListView x:Name="NavMenuList"
                           Margin="0,75,0,27"
                           ContainerContentChanging=
                                "NavMenuItemContainerContentChanging"
                           ItemContainerStyle="{StaticResource 
                                NavMenuItemContainerStyle}"
                           ItemTemplate="{StaticResource NavMenuItemTemplate}"
                           ItemInvoked="NavMenuList_ItemInvoked"/>
            </SplitView.Pane>
            <Frame x:Name="frame"
                   Margin="0,27,48,27"
                   Navigating="OnNavigatingToPage"
                   Navigated="OnNavigatedToPage"/>
    </SplitView>
```

[CommandBar](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) 是另一个通常定位在应用的一个或多个边缘附近的窗格示例，因此在电视上，它的背景应延伸到屏幕边缘。 它通常还包含**“更多”**按钮（由右侧的“...”表示）， 该按钮应保留在电视安全区域中。 以下是一些实现所需交互和视觉效果的不同策略。

**选项 1**：将 `CommandBar` 背景色更改为透明或与页面背景相同的颜色：

```xml
<CommandBar x:Name="topbar" 
            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
            ...
</CommandBar>
```

执行此操作将使 `CommandBar` 看起来像与页面其余部分位于同一个背景顶部，因此背景将无缝流动到屏幕边缘。

**选项 2**：添加一个背景矩形，该矩形的填充与 `CommandBar` 背景颜色相同，然后将矩形延伸到带有负边距的屏幕边缘：

```xml
<Rectangle VerticalAlignment="Top" 
            HorizontalAlignment="Stretch" 
            Margin="0,-27,-48,0"      
            Fill="{ThemeResource SystemControlBackgroundChromeMediumBrush}"/>
<CommandBar x:Name="topbar" 
            VerticalAlignment="Top" 
            HorizontalContentAlignment="Stretch">
            ...
</CommandBar>
```

> **注意** 如果使用此方法，请注意**“更多”**按钮将更改打开的 `CommandBar` 的高度（如有必要），以便在其图标下方显示 `AppBarButton` 的标签。 我们建议你将标签移动到其图标*右侧*以避免此大小调整。

#### 背景图像和媒体元素

大部分图像和其他媒体元素不在其边缘包含关键信息，因此可以安全地将这些 UI 元素绘制到屏幕边缘以提供沉浸式体验。 以下代码段显示将图像绘制到屏幕边缘的示例：

```xml
<Image Source="\Assets\HeaderBackground.png" 
       Stretch="Uniform" 
       Height="227" 
       VerticalAlignment="Top" 
       Margin="-48,-27,-48,0"/>
```

当然，你可以对媒体（如视频）执行相同的操作。

#### 滚动列表和网格的末端

列表和网格所包含的项通常多于屏幕上同时可以容纳的数量。 在这种情况下，我建议你将列表或网格延伸到屏幕边缘。 水平滚动列表和网格应延伸到右边缘，而垂直滚动应延伸到底部。

![电视安全区域网格切断](images/designing-for-tv/tv-safe-area-grid-cutoff.png)

当列表或网格进行此类延伸时，将焦点视觉对象及其关联项保留在电视安全区域内很重要。

![滚动网格焦点应保留在电视安全区域中。](images/designing-for-tv/scrolling-grid-focus.png)

UWP 具有将焦点视觉对象保留在 [VisibleBounds](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.applicationview.visiblebounds.aspx) 内部的功能，但你需要添加填充以确保列表/网格项可以滚动到安全区域的视图中。 具体来说，将正边距添加到 [ListView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listview.aspx) 或 [GridView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.gridview.aspx) 的 [ItemsPresenter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.itemspresenter.aspx)，如以下代码段所示：

```xml
<Style x:Key="TitleSafeListViewStyle" 
       TargetType="ListView">
    <Setter Property="Margin" 
            Value="0,0,0,-27"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="ListView">
                    <Border BorderBrush="{TemplateBinding BorderBrush}" 
                            Background="{TemplateBinding Background}" 
                            BorderThickness="{TemplateBinding BorderThickness}">
                        <ScrollViewer x:Name="ScrollViewer"
                                      TabNavigation="{TemplateBinding TabNavigation}"
                                      HorizontalScrollMode="{TemplateBinding ScrollViewer.HorizontalScrollMode}"
                                      HorizontalScrollBarVisibility="{TemplateBinding ScrollViewer.HorizontalScrollBarVisibility}"
                                      IsHorizontalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsHorizontalScrollChainingEnabled}"
                                      VerticalScrollMode="{TemplateBinding ScrollViewer.VerticalScrollMode}"
                                      VerticalScrollBarVisibility="{TemplateBinding ScrollViewer.VerticalScrollBarVisibility}"
                                      IsVerticalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsVerticalScrollChainingEnabled}"
                                      IsHorizontalRailEnabled="{TemplateBinding ScrollViewer.IsHorizontalRailEnabled}"
                                      IsVerticalRailEnabled="{TemplateBinding ScrollViewer.IsVerticalRailEnabled}"
                                      ZoomMode="{TemplateBinding ScrollViewer.ZoomMode}"
                                      IsDeferredScrollingEnabled="{TemplateBinding ScrollViewer.IsDeferredScrollingEnabled}"
                                      BringIntoViewOnFocusChange="{TemplateBinding ScrollViewer.BringIntoViewOnFocusChange}"
                                      AutomationProperties.AccessibilityView="Raw">
                            <ItemsPresenter Header="{TemplateBinding Header}"
                                            HeaderTemplate="{TemplateBinding HeaderTemplate}"
                                            HeaderTransitions="{TemplateBinding HeaderTransitions}"
                                            Footer="{TemplateBinding Footer}"
                                            FooterTemplate="{TemplateBinding FooterTemplate}"
                                            FooterTransitions="{TemplateBinding FooterTransitions}"
                                            Padding="{TemplateBinding Padding}" 
                                            Margin="0,27,0,27"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

你将之前的代码段放置在页面或应用资源中，然后通过以下方式访问它：

```xml
<Page>
    <Grid>
        <ListView Style="{StaticResource TitleSafeListViewStyle}"
                  ... />
```

> **注意** 此代码段专门用于 `ListView`；对于 `GridView` 样式，请将两个 [ControlTemplate](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.controltemplate.aspx) 和 [Style](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.style.aspx) 的 [TargetType](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.controltemplate.targettype.aspx) 属性均设置为 `GridView`。


### Xbox One 的自定义视觉状态触发器 <a name="custom-visual-state-trigger-for-xbox-one"></a>

若要针对 10 英尺体验定制 UWP 应用，我们建议你在应用检测到它已在 Xbox 控制台上启动时更改布局。 这可以通过使用自定义视觉状态触发器来完成，如以下代码段所示：

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState>
            <VisualState.StateTriggers>
                <triggers:DeviceFamilyTrigger DeviceFamily="Windows.Xbox"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="RootSplitView.Margin" 
                        Value="-48,-27"/>
                <Setter Target="RootSplitView.OpenPaneLength" 
                        Value="368"/>
                <Setter Target="RootSplitView.CompactPaneLength" 
                        Value="96"/>
                <Setter Target="NavMenuList.Margin" 
                        Value="0,75,0,27"/>
                <Setter Target="Frame.Margin" 
                        Value="0,27,48,27"/>
                <Setter Target="NavMenuList.ItemContainerStyle" 
                        Value="{StaticResource NavMenuItemContainerXboxStyle}"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

若要创建触发器，请将以下类添加到应用。 这是之前由 XAML 代码引用的类：

```csharp
class DeviceFamilyTrigger : StateTriggerBase
{
    private string _currentDeviceFamily, _queriedDeviceFamily;

    public string DeviceFamily
    {
        get
        {
            return _queriedDeviceFamily;
        }
        
        set
        {
            _queriedDeviceFamily = value;
            _currentDeviceFamily = AnalyticsInfo.VersionInfo.DeviceFamily;
            SetActive(_queriedDeviceFamily == _currentDeviceFamily);
        }
    }
}
```

在你添加了自定义触发器后，每当你的应用检测到它正在 Xbox One 控制台上运行时，你的应用将自动进行你在 XAML 代码中指定的布局修改。

## 颜色

默认情况下，通用 Windows 平台不会执行任何操作来更改应用的颜色。 也就是说，你可以对你的应用使用的颜色集进行一些改进以改进电视上的视觉体验。

### 应用程序主题

你可以根据应用的需求选择**应用程序主题**（深色或浅色），也可以选择退出主题。 阅读有关[颜色主题](../style/color.md#color-themes)的主题中的一般建议的详细信息。

UWP 还允许应用基于运行这些应用的设备所提供的系统设置动态设置主题。 
尽管 UWP 始终遵循用户指定的主题设置，但每台设备还提供相应的默认主题。 
由于 Xbox One 的性质，即预期其*媒体*体验多于*办公*体验，因此它默认为深色系统主题。 
如果你的应用主题基于系统设置，则预期它在 Xbox One 上默认为深色。

### 主题色

UWP 提供一种便捷方式来公开用户从其系统设置中选择的**主题色**。

在 Xbox One 上，用户能够选择用户颜色，就像他们可以在电脑上选择主题色一样。 
只要你的应用通过画笔或颜色资源调用这些主题色，就会使用用户在系统设置中选择的颜色。 请注意，Xbox One 上的主题色按用户而不是按系统设置。

另请注意，Xbox One 上的用户颜色集与电脑、手机和其他设备上的不同。 
部分原因归咎于以下事实：遵循本文中所述的相同方法和策略，精选这些颜色以在 Xbox One 上实现最佳的 10 英尺体验。

只要你的应用使用画笔资源（如 **SystemControlForegroundAccentBrush**）或颜色资源 (**SystemAccentColor**)，或者直接通过 [UIColorType.Accent*](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx) API 调用主题色，这些颜色就将替换为适合电视的主题色。 还通过与电脑和手机上相同的方式从系统中提取高对比画笔颜色，但使用适合电视的颜色。

若要了解有关主题色的一般详细信息，请参阅[主题色](../style/color.md#accent-color)。

### 电视之间的颜色变化

在针对电视进行设计时，请注意，根据呈现颜色的电视，颜色会以非常不同的方式显示。 不要假设颜色看起来会与在监视器上完全相同。 如果你的应用依赖细微的颜色差异来区分 UI 的各个部分，则颜色可能会混合在一起，并且用户可能会混淆。 无论用户使用何种电视，尝试使用差异足够大的颜色，以便用户能够清楚地区分它们。

### 电视安全颜色

颜色的 RGB 值表示红色、绿色和蓝色的浓度。 电视无法很好地处理极强的浓度；因此，在针对 10 英尺体验进行设计时应避免使用这些颜色。 它们会产生奇怪的色带效果，或在某些电视上显示为褪色。 此外，高浓度颜色可能导致高度曝光（附近的像素开始绘制相同的颜色）。 

尽管对于哪些颜色应视为电视安全颜色存在不同的学说，但 RGB 值为 16-235（或十六进制的 10-EB）内的颜色一般可安全用于电视。

![电视安全颜色范围](images/designing-for-tv/tv-safe-colors.png)

### 修复电视不安全颜色

通过将 RGB 值调整到电视安全范围内来单独修复电视不安全颜色通常称为**颜色固定**。 此方法可能适用于不使用丰富调色板的应用。 但是，通过此方式修复颜色可能导致颜色互相冲突，这不利于实现最佳的 10 英尺体验。

在你已使用颜色固定等方法针对电视优化颜色，从而确保颜色是电视安全颜色后，我们建议你使用**缩放**。 

<!--[v-lcap] This seems to contradict what you just said in the previous sentence-->

这涉及到通过某个系数缩放所有颜色的 RGB 值以使它们位于电视安全范围内。 
缩放应有的所有颜色有助于防止颜色冲突，并且有利于实现更好的 10 英尺体验。

![固定与缩放](images/designing-for-tv/clamping-vs-scaling.png)

### 资源

在对颜色进行更改时，请确保还相应地更新资源。 如果你的应用在 XAML 中使用某个颜色，并且该颜色应当看起来与资源颜色相同，但你仅更新 XAML 代码，则你的资源将看起来颜色不佳。

### UWP 颜色示例

[UWP 颜色主题](../style/color.md#color-themes)围绕应用的背景进行设计，该背景在深色主题中为**黑色**，在浅色主题中为**白色**。 由于黑色和白色都不是电视安全颜色，因此这些颜色需要使用*固定*修复。 修复这些颜色后，所有其他颜色都需要通过*缩放*进行调整，以保留必要的对比度。

<!--[v-lcap to eliot]why is the above paragraph in the past tense?-->

以下示例代码提供已针对电视用途进行优化的颜色主题：

```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="ApplicationPageBackgroundThemeBrush" 
                                 Color="#FF101010"/>
                <Color x:Key="SystemAltHighColor">#FF101010</Color>
                <Color x:Key="SystemAltLowColor">#33101010</Color>
                <Color x:Key="SystemAltMediumColor">#99101010</Color>
                <Color x:Key="SystemAltMediumHighColor">#CC101010</Color>
                <Color x:Key="SystemAltMediumLowColor">#66101010</Color>
                <Color x:Key="SystemBaseHighColor">#FFEBEBEB</Color>
                <Color x:Key="SystemBaseLowColor">#33EBEBEB</Color>
                <Color x:Key="SystemBaseMediumColor">#99EBEBEB</Color>
                <Color x:Key="SystemBaseMediumHighColor">#CCEBEBEB</Color>
                <Color x:Key="SystemBaseMediumLowColor">#66EBEBEB</Color>
                <Color x:Key="SystemChromeAltLowColor">#FFDDDDDD</Color>
                <Color x:Key="SystemChromeBlackHighColor">#FF101010</Color>
                <Color x:Key="SystemChromeBlackLowColor">#33101010</Color>
                <Color x:Key="SystemChromeBlackMediumLowColor">#66101010</Color>
                <Color x:Key="SystemChromeBlackMediumColor">#CC101010</Color>
                <Color x:Key="SystemChromeDisabledHighColor">#FF333333</Color>
                <Color x:Key="SystemChromeDisabledLowColor">#FF858585</Color>
                <Color x:Key="SystemChromeHighColor">#FF767676</Color>
                <Color x:Key="SystemChromeLowColor">#FF1F1F1F</Color>
                <Color x:Key="SystemChromeMediumColor">#FF262626</Color>
                <Color x:Key="SystemChromeMediumLowColor">#FF2B2B2B</Color>
                <Color x:Key="SystemChromeWhiteColor">#FFEBEBEB</Color>
                <Color x:Key="SystemListLowColor">#19EBEBEB</Color>
                <Color x:Key="SystemListMediumColor">#33EBEBEB</Color>
            </ResourceDictionary>
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="ApplicationPageBackgroundThemeBrush" 
                                 Color="#FFEBEBEB" /> 
                <Color x:Key="SystemAltHighColor">#FFEBEBEB</Color>
                <Color x:Key="SystemAltLowColor">#33EBEBEB</Color>
                <Color x:Key="SystemAltMediumColor">#99EBEBEB</Color>
                <Color x:Key="SystemAltMediumHighColor">#CCEBEBEB</Color>
                <Color x:Key="SystemAltMediumLowColor">#66EBEBEB</Color>
                <Color x:Key="SystemBaseHighColor">#FF101010</Color>
                <Color x:Key="SystemBaseLowColor">#33101010</Color>
                <Color x:Key="SystemBaseMediumColor">#99101010</Color>
                <Color x:Key="SystemBaseMediumHighColor">#CC101010</Color>
                <Color x:Key="SystemBaseMediumLowColor">#66101010</Color>
                <Color x:Key="SystemChromeAltLowColor">#FF1F1F1F</Color>
                <Color x:Key="SystemChromeBlackHighColor">#FF101010</Color>
                <Color x:Key="SystemChromeBlackLowColor">#33101010</Color>
                <Color x:Key="SystemChromeBlackMediumLowColor">#66101010</Color>
                <Color x:Key="SystemChromeBlackMediumColor">#CC101010</Color>
                <Color x:Key="SystemChromeDisabledHighColor">#FFCCCCCC</Color>
                <Color x:Key="SystemChromeDisabledLowColor">#FF7A7A7A</Color>
                <Color x:Key="SystemChromeHighColor">#FFB2B2B2</Color>
                <Color x:Key="SystemChromeLowColor">#FFDDDDDD</Color>
                <Color x:Key="SystemChromeMediumColor">#FFCCCCCC</Color>
                <Color x:Key="SystemChromeMediumLowColor">#FFDDDDDD</Color>
                <Color x:Key="SystemChromeWhiteColor">#FFEBEBEB</Color>
                <Color x:Key="SystemListLowColor">#19101010</Color>
                <Color x:Key="SystemListMediumColor">#33101010</Color>
            </ResourceDictionary> 
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

> **注意** 浅色主题 **SystemChromeMediumLowColor** 和 **SystemChromeMediumLowColor** 有意为相同的颜色，并且不是因固定而生成的。 

<!--[v-lcap] Double check that you didn't mean to say something else in the sentence above-->

> **注意** 十六进制颜色在 **ARGB**（Alpha 红绿蓝）中指定。

在不固定的情况下，我们不建议在能够显示完整范围的监视器上使用电视安全颜色，因为颜色将看起来褪色。 当你的应用在 Xbox 而*不是*其他平台上运行时，改为加载资源字典（上一示例）。 在 `App.xaml.cs` 的 `OnLaunched` 方法中，添加以下检查：

```csharp
if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
{ 
    this.Resources.MergedDictionaries.Add(new ResourceDictionary 
    { 
        Source = new Uri("ms-appx:///TenFootStylesheet.xaml") 
    }); 
}
```

这将确保无论应用在哪台设备上运行都显示正确的颜色，从而为用户提供更好、在审美上更令人愉悦的体验。

<!--### Light dismiss overlay

In order to call the user's attention to the UI elements that the user is currently manipulating with the game controller or remote control, UWP automatically adds a "smoke" layer that covers areas outside of the popup UI when the app is running on Xbox One. This requires no extra work, but is something to keep in mind when designing your UI.
-->

## 游戏板和遥控器

就像键盘和鼠标适用于电脑、而触摸适用于手机和平板电脑一样，游戏板和遥控器是 10 英尺体验的主要输入设备。 
本部分介绍了硬件按钮的定义和作用。 
在 [XY 焦点导航和交互](#xy-focus-navigation-and-interaction)和[鼠标模式](#mouse-mode)中，你将了解如何在使用这些输入设备时优化你的应用。

全新的游戏板和遥控器行为的质量取决于键盘在应用中受到多大的支持。 确保你的应用适用于游戏板/遥控器的一个好方法是，确保它在电脑上适用于键盘，然后使用游戏板/遥控器进行测试以查找 UI 中的缺陷。

### 硬件按钮

在整个文档中，将使用下图中给定的名称指代按钮。

![游戏板和遥控器按钮图](images/designing-for-tv/hardware-buttons-gamepad-remote.png)

如该图中所示，某些按钮在游戏板上受支持，但在遥控器上不受支持，反之亦然。 尽管你可以使用仅在一台输入设备上受支持的按钮来加快导航 UI 的速度，但请注意将这些按钮用于关键交互可能会产生用户无法与特定 UI 部分交互的情况。

下表列出了所有受 UWP 应用支持的硬件按钮，以及哪些输入设备支持它们。

| 按钮                    | 游戏板   | 遥控器    |
|---------------------------|-----------|-------------------|
| A/“选择”按钮           | 是       | 是               |
| B/“后退”按钮             | 是       | 是               |
| 方向键（方向键）   | 是       | 是               |
| “菜单”按钮               | 是       | 是               |
| “视图”按钮               | 是       | 是               |
| X 和 Y 按钮           | 是       | 否                |
| 左摇杆                | 是       | 否                |
| 右摇杆               | 是       | 否                |
| 左和右扳机键   | 是       | 否                |
| 左和右缓冲键    | 是       | 否                |
| OneGuide 按钮           | 否        | 是               |
| 音量按钮             | 否        | 是               |
| 频道按钮            | 否        | 是               |
| 媒体控制按钮     | 否        | 是               |
| 静音按钮               | 否        | 是               |

### 内置按钮支持

UWP 将现有键盘输入行为自动映射到游戏板和遥控器输入。 下表列出了这些内置映射。

| 键盘              | 游戏板/遥控器                        |
|-----------------------|---------------------------------------|
| 箭头键            | 方向键（以及游戏板上的左摇杆）    |
| 空格键              | A/“选择”按钮                       |
| Enter                 | A/“选择”按钮                       |
| Escape                | B/“后退”按钮*                        |

\*当应用不处理 B 按钮的 [KeyDown](https://msdn.microsoft.com/library/windows/apps/br208941) 和 [KeyUp](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.uielement.keyup.aspx) 事件时，将引发 [SystemNavigationManager.BackRequested](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.core.systemnavigationmanager.backrequested.aspx) 事件，从而导致应用内的向后导航。

### 加速器支持

加速器按钮是可用于加快在 UI 中导航的按钮。 但是，这些按钮可能是特定输入设备所独有的，因此请记住并非所有用户都能够使用这些功能。 事实上，游戏板当前是在 Xbox One 上针对 UWP 应用支持加速器功能的唯一输入设备。

下表列出了内置于 UWP 以及可自行实现的加速器支持。 在自定义 UI 中利用这些行为提供一致且友好的用户体验。

| 交互   | 键盘   | 游戏板      | 内置用于：  | 建议用于： |
|---------------|------------|--------------|----------------|------------------|
| 平移       | 无       | 右摇杆  | 无           |      [ScrollViewer](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) |
| 向上/向下翻页  | 向上/向下翻页 | 左/右扳机键 | 无 | `ScrollViewer` 和列表/网格
| 向左/向右翻页 | 无 | 左/右缓冲键 | [透视](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.pivot.aspx) | `ScrollViewer`
| 放大/缩小        | Ctrl +/- | 左/右扳机键 | `ScrollViewer` | 支持放大和缩小的视图

<!--[v-lcap] Had to move this table into markdown to get the links to work
<table>
    <tr>
        <th>Interaction</th>
        <th>Keyboard</th>
        <th>Gamepad</th>
        <th>Built-in for:</th>
        <th>Recommended for:</th>
    </tr>
    <tr>
        <td>Panning</td>
        <td>None</td>
        <td>Right stick</td>
        <td>None</td>
        <td>[ScrollViewer](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx)</td>
    </tr>
    <tr>
        <td>Page up/down</td>
        <td>Page up/down</td>
        <td>Left/right triggers</td>
        <td>None</td>
        <td>`ScrollViewer` and list/grid</td>
    </tr>
    <tr>
        <td>Page left/right</td>
        <td>None</td>
        <td>Left/right bumpers</td>
        <td>[Pivot](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.pivot.aspx).</td>
        <td>`ScrollViewer`</td>
    </tr>
    <tr>
        <td>Zoom in/out</td>
        <td>Ctrl +/-</td>
        <td>Left/right triggers</td>
        <td>`ScrollViewer`</td>
        <td>Views that support zooming in and out
    </tr>
</table>-->

## 鼠标模式

如 [XY 焦点导航和交互](#xy-focus-navigation-and-interaction)中所述，在 Xbox One 上，焦点通过使用 XY 导航系统进行移动，从而允许用户通过向下、向下、向左和向右移动在控件之间转移焦点。 
但是，某些控件（如 [WebView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.webview.aspx) 和 
[MapControl](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx)） 
需要类似于鼠标的交互，在该交互中，用户可以在控件的边界内自由移动指针。 
另外，在某些应用中，用户能够将指针在整个页面上移动，并且此操作有意义，该游戏板/遥控器体验类似于用户可在带有鼠标的电脑上找到的体验。

对于这些方案，你应为整个页面或者在页面内部的控件上请求指针（鼠标模式）。 
例如，你的应用可能具有一个包含 `WebView` 控件的页面，它仅在该控件内部使用鼠标模式，而在任何其他位置使用 XY 焦点导航。 
若要请求一个指针，你可以指定**在控件或页面已占用时**还是**在页面具有焦点时**需要它。

> **注意** 不支持在控件得到焦点时请求指针。

下图显示游戏板/遥控器在鼠标模式下的按钮映射。

![游戏板/遥控器在鼠标模式下的按钮映射](images/designing-for-tv/mouse-mode.png)

> **注意** 鼠标模式仅在带有游戏板/遥控器的 Xbox One 上受支持。 在其他设备系列和输入类型上，将静默忽略它。

在控件或页面上使用 `RequiresPointer` 属性在其上激活鼠标模式。 `RequiresPointer` 具有三个可能的值：`Never`（默认值）、`WhenEngaged` 和 `WhenFocused`。

> **注意** `RequiresPointer` 是新的 API，尚未介绍。 

<!--TODO: Link to doc-->

### 在控件上激活鼠标模式

当用户使用 `RequiresPointer="WhenEngaged"` 占用控件时，将在该控件上激活鼠标模式，直到用户脱离它。 以下代码段演示了在已占用时激活鼠标模式的简单 `MapControl`：

```xml
<Page>
    <Grid>
        <MapControl IsEngagementRequired="true" 
                    RequiresPointer="WhenEngaged"/>
    </Grid>
</Page> 
```

> **注意** 如果控件在已占用时激活鼠标模式，则它必须通过 `IsEngagementRequired="true"` 要求占用；否则，将永远不激活鼠标模式。

当控件处于鼠标模式下时，其嵌套控件也将处于鼠标模式下。 将忽略它的子元素的请求模式，当父元素处于鼠标模式下时，子元素也必须处于鼠标模式下。

此外，仅在控件得到焦点时检查控件的请求模式，因此模式不会在其具有焦点时动态更改。

### 在页面上激活鼠标模式

当页面具有属性 `RequiresPointer="WhenFocused"` 时，将在该页面得到焦点时为整个页面激活鼠标模式。 以下代码段演示了如何为页面提供此属性：

```xml
<Page RequiresPointer="WhenFocused">
    ...
</Page> 
```

> **注意** `WhenFocused` 值仅在 [Page](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.page.aspx) 对象上受支持。 如果你尝试在控件上设置此值，将引发异常。

## 焦点视觉对象

焦点视觉对象是当前具有焦点的 UI 元素周围的边框。 这有助于使用户定位，以便他们可以轻松导航你的 UI而不会迷失。

借助添加到焦点视觉对象的视觉更新和大量自定义选项，开发人员可以相信，单个焦点视觉对象将适用于电脑和 Xbox One 以及支持键盘和/或游戏板/遥控器的任何其他 Windows 10 设备。

尽管可以在不同的平台上使用相同的焦点视觉对象，但对于 10 英尺体验，用户遇到此情况的上下文稍有不同。 你应假设用户不会对整个电视屏幕投入完全的注意力，因此当前具有焦点的元素始终对用户清晰可见以避免其在搜寻视觉对象时感到沮丧，这一点很重要。

请记住在使用游戏板或遥控器（而*不是*键盘）时，默认情况下将显示焦点视觉对象，这一点也很重要。 因此，即使你不实现它，当你在 Xbox One 上运行你的应用时，它仍会出现。

### 初始焦点视觉对象放置

在启动应用或导航到某个页面时，将焦点放置在作为用户会执行操作的第一个元素很合理的 UI 元素上。 例如，照片应用可能会将焦点放置在库中的第一个项上，而导航到歌曲详细视图的音乐应用可能将焦点放置在播放按钮上以便于播放音乐。

尝试将初始焦点放置在应用的左上区域（对于从右向左流，则为右上区域）。 大多数用户倾向于先将焦点放置在该角度，因为应用内容流通常在此位置开始。

### 使焦点清晰可见

一个焦点视觉对象应始终在屏幕上可见，以便用户可以从离开的位置继续，而无需搜索焦点。 同样，屏幕上应始终存在可聚焦的项，例如，不要使用仅带有文本而没有可聚焦元素的弹出窗口。

### 轻型消除覆盖层

为了将用户的注意力吸引到用户当前正在使用游戏控制器或遥控器操作的 UI 元素，当应用在 Xbox One 上运行时，UWP 自动添加“烟”层，该层覆盖弹出窗口 UI 之外的区域。 这无需任何额外工作，但却是在设计 UI 时需要牢记的内容。

## 焦点占用

焦点占用旨在更轻松地使用游戏板或遥控器与应用交互。 

> **注意** 设置焦点占用不会影响键盘或其他输入设备。

当 [FrameworkElement](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.aspx) 对象上的属性 `IsFocusEngagementEnabled` 设置为 `True` 时，它会将控件标记为需要焦点占用。 这意味着用户必须按 **A/“选择”**按钮来“占用”该控件并与其交互。 当用户完成操作时，他们可以按 **B/“后退”**按钮脱离该控件并导航离开它。

> **注意** `IsFocusEngagementEnabled` 是新的 API，尚未介绍。

### 焦点捕获

焦点捕获是指用户尝试导航应用 UI 但被“捕获”在控件内，从而难以甚至无法移出该控件时发生的情况。

以下示例显示产生焦点捕获的 UI。

![水平滑块左侧和右侧的按钮](images/designing-for-tv/focus-engagement-focus-trapping.png)

如果用户要从左按钮导航到右按钮，则符合逻辑的假设是他们只需按方向键/左摇杆上的向右键两次。 
但是，如果[滑块](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.slider.aspx)不需要占用，则将发生以下行为：当用户第一次按向右键时，焦点将转移到 `Slider`，并且当用户再次按向右键时，`Slider` 的手柄将移到右侧。 用户不断地将手柄移到右侧，从而无法到达按钮。

有多种方法解决此问题。 一个方法是设计一个不同的布局，类似于 [XY 焦点导航和交互](#xy-focus-navigation-and-interaction)中的房地产应用示例，在该示例中我们将**“上一步”**和**“下一步”**按钮重新放置在 `ListView` 上方。 如下图所示垂直堆叠而不是水平堆叠这些控件将解决该问题。

![水平滑块上方和下方的按钮](images/designing-for-tv/focus-engagement-focus-trapping-2.png)

现在用户可以通过按方向键/左摇杆上的向上和向下键来导航到每个控件，并且当 `Slider` 具有焦点时，他们可以按预期通过按向左和向右键来移动 `Slider` 手柄。

解决此问题的另一个方法是在 `Slider` 上要求占用。 如果你设置 `IsFocusEngagementEnabled="True"`，这将导致以下行为。

![在滑块上要求焦点占用，以便用户可以导航到右侧的按钮。](images/designing-for-tv/focus-engagement-slider.png)

当 `Slider` 要求焦点占用时，用户只需按方向键/左摇杆上的向右键两次即可到达右侧的按钮。 此解决方案很出色，因为它无需进行 UI 调整即可产生预期的行为。

### 项控件

除了 [Slider](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.slider.aspx) 控件，还有其他可能要求占用的控件，例如：

- [ListBox](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listbox.aspx)
- [ListView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [GridView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)
- [FlipView](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.flipview)

与 `Slider` 控件不同，这些控件不会将焦点捕获在自身内部；但是，当它们包含大量数据，它们可能会导致可用性问题。 以下是一个包含大量数据的 `ListView` 的示例。

![带有大量数据的 ListView 以及上方和下方的按钮](images/designing-for-tv/focus-engagement-list-and-grid-controls.png)

与 `Slider` 示例类似，让我们尝试使用游戏板/遥控器从顶部的按钮导航到底部的按钮。 
从顶部按钮的焦点开始，按方向键/摇杆上的向下键会将焦点放置在 `ListView` 中的第一个项上（“项 1”）。 
当用户再次按向下键时，列表中的下一个项将得到焦点，而不是底部的按钮。 
若要到达该按钮，用户必须先导航 `ListView` 中的每一项。 
如果 `ListView` 包含大量数据，这可能造成不便且非最佳的用户体验。

若要解决此问题，请在 `ListView` 上设置属性 `IsFocusEngagementEnabled="True"` 以在该控件上要求占用。 
这将允许用户只需按向下键即可跳过 `ListView`。 但是， 
他们将无法滚动浏览列表或从中选择项，除非他们占用该列表，方法是在具有焦点时按 **A/“选择”**按钮，然后按 **B/“后退”**按钮来脱离。

![要求占用的 ListView](images/designing-for-tv/focus-engagement-list-and-grid-controls-2.png)

#### ScrollViewer

与这些控件稍有不同的是 [ScrollViewer](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx)， 
该控件有其自己的特点需要考虑。 如果你有包含可聚焦内容的 `ScrollViewer`，默认情况下导航到 `ScrollViewer` 将允许你移动其可聚焦元素。 和在 `ListView` 中相同，你必须滚动浏览每一项才能导航到 `ScrollViewer` 外部。 

如果 `ScrollViewer` *没有*可聚焦内容（例如，如果它仅包含文本），则可以设置 `IsFocusEngagementEnabled="True"`，以便用户可以通过使用 **A/“选择”**按钮占用 `ScrollViewer`。 在用户已占用后，他们可以通过使用**方向键/左摇杆**滚动浏览文本，然后在他们完成后按 **B/“后退”**按钮来脱离。

另一个方法是在 `ScrollViewer` 上设置 `IsTabStop="True"`，以便用户无需占用该控件，当 
`ScrollViewer` 内不存在可聚焦元素时，他们只需将焦点放置在该控件上，然后使用**方向键/左摇杆**进行滚动。

### 焦点占用默认值

某些控件导致焦点捕获的频率足以保证其默认设置要求焦点占用，而其他控件默认关闭焦点占用，但可能因打开它而受益。 下表列出了这些控件及其默认焦点占用行为。

| 控件               | 焦点占用默认值  |
|-----------------------|---------------------------|
| CalendarDatePicker    | 开                        |
| FlipView              | 关                       |
| GridView              | 关                       |
| ListBox               | 关                       |
| ListView              | 关                       |
| ScrollViewer          | 关                       |
| SemanticZoom          | 关                       |
| Slider                | 开                        |

当 `IsFocusEngagementEnabled="True"` 时，所有其他 UWP 控件都不会导致任何行为或视觉更改。

## XY 焦点导航和交互

如果应用支持键盘的适当焦点导航，这将很好地转换到游戏板和遥控器。 
使用箭头键的导航映射到**“方向键”**（以及游戏板上的**“左摇杆”**），而与 UI 元素的交互映射到 **Enter/“选择”**键。 
（请参阅[游戏板和遥控器](#gamepad-and-remote-control)）。 有关键盘设计指南，请参阅[键盘交互](keyboard-interactions.md)。

如果已正确实现键盘支持，你的应用理应良好地工作；但是，若要支持每一种方案，可能需要一些额外的工作。 思考你的应用的具体需求以尽可能提供最佳的用户体验。

<!--### Focus placement

The focus visual should be initially placed on the UI element that makes sense as the first element on which the user would take action. For more information, see [Focus visual](#focus-visual).
-->

### 无法访问的 UI

由于 XY 焦点导航限制用户向上、向下、向左和向右移动，因此你最终可能得到部分 UI 无法访问的方案。 
下图演示了 XY 焦点导航不支持的 UI 布局类型的示例。 
请注意，中间的元素无法通过使用游戏板/遥控器来访问，因为垂直和水平导航将设置优先级，而中间元素永远不会是足以得到焦点的高优先级。

![四个角的元素，中间的元素无法访问](images/designing-for-tv/2d-navigation-best-practices-ui-layout-to-avoid.png)

如果由于某些原因无法重新排列 UI，请使用下一部分中所述的技术之一来替代默认焦点行为。

### 替代默认导航 <a name="overriding-the-default-navigation"></a>

尽管 UWP 尝试确保方向键/左摇杆导航对用户有意义，但无法保证行为已针对应用的意图进行优化。 
确保导航已针对应用进行优化的最佳方式是使用游戏板测试它，然后确认用户可以通过对应用方案有意义的方式访问每个 UI 元素。 如果你的应用方案需要并非通过所提供的 XY 焦点导航实现的行为，请考虑以下部分中的以下建议和/或替代该行为以将焦点放置在逻辑项上。

以下代码段显示了你可以如何替代 XY 焦点导航行为：

```xml
<StackPanel>
    <Button x:Name="MyBtnLeft" 
            Content="Search" />
    <Button x:Name="MyBtnRight" 
            Content="Delete"/>
    <Button x:Name="MyBtnTop" 
            Content="Update" />
    <Button x:Name="MyBtnDown" 
            Content="Undo" />
    <Button Content="Home"  
            XYFocusLeft="{x:Bind MyBtnLeft}" 
            XYFocusRight="{x:Bind MyBtnRight}"
            XYFocusDown="{x:Bind MyBtnDown}"
            XYFocusUp="{x:Bind MyBtnTop}" />
</StackPanel>
```

在此情况下，当焦点在 `Home` 按钮上且用户导航到左侧时，焦点将移动到 `MyBtnLeft` 按钮；如果用户导航到右侧，焦点将移动到 `MyBtnRight` 按钮；依此类推。

若要防止焦点从控件以特定方向移动，请使用 `XYFocus*` 属性将其指向相同的控件：

```xml
<Button Name="HomeButton"  
        Content="Home"  
        XYFocusLeft ="{x:Bind HomeButton}" />
```

### 最少单击路径 <a name="path-of-least-clicks"></a>

尝试允许用户以最少的单击数执行最常见的任务。 在以下示例中，[TextBlock](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) 放置在**“播放”**按钮（最先得到焦点）和常用元素之间，因此不必要的元素放置在优先任务之间。

![导航最佳做法可提供最少单击路径。](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

在以下示例中，[TextBlock](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) 改为放置在“播放****按钮上方。 
只需重新排列 UI 以使优先任务之间不放置不必要的元素即可大幅提高应用的可用性。

![TextBlock 已移到“播放”按钮上方，以使其不再位于优先任务之间。](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

<!--### Nested UI elements

When UI elements are nested inside other UI elements, the default behavior is that the user will not be able to access the nested UI elements.

One of the main scenarios is when there is UI that displays when the user hovers over a nested UI element with the mouse, but does not display otherwise.

![UI elements displaying when mouse hovers over them](images/designing-for-tv/2d-navigation-best-practices-ui-elements-display-on-mouse-hover.png)

The recommended way to handle this scenario for gamepad/remote input is to place these UI elements in a `ContextFlyout` that displays when the user presses the **Menu** button.
-->
<!--#### UI elements that always display

The second scenario is when there is UI that always displays. **TODO: Fill in this section when I get more info**
-->

### CommandBar 和 ContextFlyout

在使用 [CommandBar](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) 时，请记住[问题：位于长滚动列表/网格之后的 UI 元素](#problem-ui-elements-located-after-long-scrolling-list-grid)中所提到的滚动浏览列表的问题。 下图显示了列表/网格底部带有 `CommandBar` 的 UI 布局。 用户需要向下一直滚动浏览列表/网格才能到达 `CommandBar`。

![位于列表/网格底部的 CommandBar](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

如果你将 `CommandBar` 放置在列表/网格*上方*会怎么样？ 尽管向下滚动列表/网格的用户必须重新向上滚动才能到达 `CommandBar`，但与之前的配置相比，导航数略少。 请注意，这假设你的应用的初始焦点放置在 `CommandBar` 旁边或上方；如果初始焦点在列表/网格下方，则此方法不起作用。 如果这些 `CommandBar` 项是无需经常访问的全局操作项（如**“同步”**按钮），则可以将它们放置在列表/网格上方。

如果你的应用的 `CommandBar` 具有需要便于用户访问的项，你可能要考虑将这些项放置在 `ContextFlyout` 内部，并将它们从 `CommandBar` 中删除。 

<!--The `ContextFlyout` can be accessed by pressing the **Menu** button, providing a very convenient way for the user to access these actions quickly.-->

尽管你无法垂直堆叠 `CommandBar` 的项，但相对于滚动方向放置它们（例如，垂直滚动列表的左侧或右侧，或者水平滚动列表的上方或下方）是另一个你可能要考虑的选项（如果它适用于你的 UI 布局）。

<!--### Navigation pane
A navigation pane (also known as a *hamburger menu*) is a navigation control commonly used in UWP apps. Typically it is a pane with several options to choose from in a list-style menu that will take the user to different pages. Generally this pane starts out collapsed to save space, and the user can open it by clicking on a button.
While nav panes are very accessible with mouse and touch, gamepad/remote makes them less accessible since the user has to navigate to a button to open the pane. Therefore, a good practice is to have the **Menu** button open the nav pane, as well as allow the user to open it by navigating all the way to the left of the page. This will provide the user with very easy access to the contents of the pane. See [Nav panes](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/nav-pane). TO DO: Is this the right link?--> 
<!--TODO: Image of nav pane-->

### UI 布局挑战

由于 XY 焦点导航的性质，某些 UI 布局更具挑战性，应根据具体情况进行评估。 尽管不存在单一的“正确”方法，并且选择哪个解决方案取决于应用的具体需求，但你可以利用某些技术来创造出色的电视体验。

为了更好地了解这一点，让我们查看一个虚构的应用，该应用演示了其中的一些问题以及克服它们的技术。

> **注意** 此虚构应用旨在演示 UI 问题及其潜在解决方案，而非旨在介绍你的特定应用的最佳用户体验。

以下是一个虚构的房地产应用，该应用显示可供销售的房屋列表、地图、房产说明以及其他信息。 此应用提出了三项可通过使用以下技术克服的挑战：

- [UI 重新排列](#ui-rearrange)
- [焦点占用](#engagement)
- [鼠标模式](#mouse-mode)

![虚构房地产应用](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app.png)

#### 问题：位于长滚动列表/网格之后的 UI 元素 <a name="problem-ui-elements-located-after-long-scrolling-list-grid"></a>

下图所示的房产 [ListView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listview.aspx) 是一个非常长的滚动列表。 如果 `ListView` 上*不*要求[占用](#focus-engagement)，当用户导航到该列表时，焦点将放置在列表中的第一个项上。 若要使用户到达**“上一步”**或**“下一步”**按钮，他们必须浏览列表中的所有项。 在这种难以要求用户遍历整个列表（即，当列表太长，无法接受此体验时）的情况下，你可能希望考虑其他选项。

![房地产应用：带有 50 个项的列表，需要单击 51 次才能到达下方的按钮](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app-list.png)

#### 解决方案

##### UI 重新排列 <a name="ui-rearrange"></a>

除非你的初始焦点放置在页面底部，否则放置在长滚动列表上方的 UI 元素通常比放置在下方更易于访问。 
如果此新布局适用于其他设备，则针对所有设备系列更改布局而不仅仅针对 Xbox One 进行特殊 UI 更改，此方法的成本可能更低。 
此外，针对滚动方向放置 UI元素（即对垂直滚动列表水平放置，或对水平滚动列表垂直放置）将更便于访问。

![房地产应用：将按钮放置在长滚动列表上方](images/designing-for-tv/2d-focus-navigation-and-interaction-ui-rearrange.png)

##### 焦点占用 <a name="engagement"></a>

当要求*占用*时，整个 `ListView` 将变为一个焦点目标。 用户将能够绕过列表的内容到达下一个可聚焦元素。 在[焦点占用](#focus-engagement)中阅读有关哪些控件支持占用以及如何使用它们的详细信息。

![房地产应用：设置要求的占用，以便只需 1 次单击即可到达“上一步”/“下一步”按钮](images/designing-for-tv/2d-focus-navigation-and-interaction-engagement.png)

#### 问题：不带有任何可聚焦元素的 ScrollViewer

由于 XY 焦点导航依赖于一次导航到一个可聚焦 UI 元素， 
不包含任何可聚焦元素的 [ScrollViewer](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx)（例如本示例中的仅带有文本的一个元素）可能导致用户无法查看 `ScrollViewer` 中的所有内容的方案。 
有关此问题的解决方案和其他相关方案，请参阅[焦点占用](#focus-engagement)。

![房地产应用：仅带有文本的 ScrollViewer](images/designing-for-tv/2d-focus-navigation-and-interaction-scrollviewer.png)

#### 问题：自由滚动 UI

当你的应用需要自由滚动的 UI（例如绘图图面，或本示例中的地图）时，XY 焦点导航不起作用。 
在这种情况下，你可以打开[鼠标模式](#mouse-mode)以允许用户在 UI 元素内部自由导航。

![使用鼠标模式的 UI 元素](images/designing-for-tv/map-mouse-mode.png)

<!--## 2D navigation best practices

Since 2D navigation only lets the user navigate up, down, left, and right, but not diagonally, certain UI layouts work better than others. This section describes some common issues related to navigation and their recommended solutions. Please note that there is no single "right" way to solve these problems, and always think about how to solve them for your app's specific scenarios.

### UI layouts to avoid

The following diagram illustrates an example of the kind of UI layout that 2D navigation doesn't support. Note that the element in the middle is not accessible using gamepad/remote because the vertical and horizontal navigation will be prioritized and the middle element will never be high enough priority to get focus.

![Elements in four corners with inaccessible element in middle](images/designing-for-tv/2d-navigation-best-practices-ui-layout-to-avoid.png)

If for some reason rearranging the UI is not possible, use one of the techniques discussed in [Overriding the default navigation](#overriding-the-default-navigation) to override the default focus behavior.

### CommandBar and ContextFlyout

When using a [CommandBar](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx), keep in mind the issue of scrolling through a list as mentioned in [Problem: UI elements located after long scrolling list/grid](#problem-ui-elements-located-after-long-scrolling-list-grid). The following image shows a UI layout with the `CommandBar` on the bottom of a list/grid. The user would need to scroll all the way down through the list/grid to reach the `CommandBar`.

![CommandBar at bottom of list/grid](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

Now compare this to the following configuration, which puts the `CommandBar` *above* the list/grid. While a user who scrolled down the list/grid would have to scroll back up to reach the `CommandBar`, it is slightly less navigation than the previous configuration. Note that this is assuming that your app's initial focus is placed next to or above the `CommandBar`; this approach won't work as well if the initial focus is below the list/grid. If these `CommandBar` items are global action items that don't have to be accessed very often (such as a **Sync** button), it may be acceptable to have them above the list/grid as in this example.

![CommandBar above list/grid](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout-2.png)

If your app has a `CommandBar` whose items need to be readily accessible by users, you may want to consider placing these items inside a `ContextFlyout` and removing them from the `CommandBar`. The `ContextFlyout` can be accessed by pressing the **Menu** button, providing a very convenient way for the user to access these actions quickly.

While you can't stack a `CommandBar`'s items vertically, placing them against the scroll direction (for example, to the left or right of a vertically scrolling list, or the top or bottom of a horizontally scrolling list) is another option you may want to consider if it works well for your UI layout.

### Path of least clicks <a name="path-of-least-clicks"></a>

Try to allow the user to perform the most common tasks in the least number of clicks. In the following example, there is a [TextBlock](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) between the **Play** button and a commonly used element, adding an extra click to get from the initially focused element to the commonly used element.

![TextBlock adds extra click to get to commonly used element](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

Simply rearranging the UI so that unnecessary elements are not placed in between priority tasks will greatly improve your app's usability.

![TextBlock moved above Play button so that it is no longer between priority tasks](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

### Nested UI elements

When UI elements are nested inside other UI elements, the default behavior is that the user will not be able to access the nested UI elements. There are two main scenarios involving nested UI, and the solutions for them are slightly different.

#### UI elements that display on mouse hover

The first scenario is when there is UI that displays when the user hovers over an element with the mouse, but does not display otherwise.

![UI elements displaying when mouse hovers over them](images/designing-for-tv/2d-navigation-best-practices-ui-elements-display-on-mouse-hover.png)

The recommended way to handle this scenario for gamepad/remote input is to place these UI elements in a `ContextFlyout` that displays when the user presses the **Menu** button.

#### UI elements that always display

The second scenario is when there is UI that always displays. **TODO: Fill in this section when I get more info**

### Navigation pane

A navigation pane (also known as a *hamburger menu*) is a navigation control commonly used in UWP apps. Typically it is a pane with several options to choose from in a list-style menu that will take the user to different pages. Generally this pane starts out collapsed to save space, and the user can open it by clicking on a button.

While nav panes are very accessible with mouse and touch, gamepad/remote makes them less accessible since the user has to navigate to a button to open the pane. Therefore, a good practice is to have the **Menu** button open the nav pane, as well as allow the user to open it by navigating all the way to the left of the page. This will provide the user with very easy access to the contents of the pane. See [Nav panes](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/nav-pane) TODO: Is this the right link? for more information.

TODO: Image of nav pane
-->

## UI 控件指南

通用 Windows 平台提供许多可在所有设备上改进用户体验的功能，其中一些功能尤其有利于 10 英尺体验。 以下列表介绍了可对应用 UI 进行的此类改进的示例。

### 透视表控件

[透视表](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.pivot.aspx)控件具有可设置的属性，可使标题不会像在手机和平板电脑上那样在屏幕上换行。 这对大屏幕（如电视）来说是更佳的体验，因为标题换行可能会干扰用户。 有关详细信息，请参阅[表和透视表](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tabs-pivot)。

### 导航窗格

UWP 允许在所有设备上实现一致的外观。 有关导航窗格在不同的屏幕大小中的行为以及游戏板/遥控器导航的最佳做法的详细信息，请参阅[导航窗格](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/nav-pane)。

### CommandBar 标签

[CommandBar](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) 控件具有可导致图标旁边的标签始终显示的属性。 这适用于 10 英尺体验，因为它最大程度减少了用户查看按钮功能所需的单击数。 这也是可供其他设备类型遵循的绝佳模型。

### 工具提示

引入了[工具提示](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.tooltip.aspx)控件，可作为一种在用户将鼠标悬停在元素上或点击并按住元素上的图片时在 UI 中提供更多信息的方法。 对于游戏板和遥控器，`Tooltip` 将在元素得到焦点的短暂时间后显示、在屏幕上保留一小段时间，然后消失。 如果使用了太多 `Tooltip`，此行为可能令人分心。 在针对电视进行设计时，尝试避免使用工具提示。

### 按钮样式

当标准 UWP 按钮适用于电视时，某些按钮视觉样式能更好地将注意力吸引到 UI，你可能希望针对所有平台考虑这些样式，这对于 10 英尺体验尤其有利，因为可以受益于清晰传达焦点所在的位置。 若要阅读有关这些样式的详细信息，请参阅[按钮](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/buttons)。

<!--### Sort and filter lists and grids

The visual style for sorting and filtering list and grid items on UWP has been updated for 10-foot scenarios. Read more about how to use this UI functionality [here](TODO: Add link).
-->

### 嵌套 UI 元素

当 UI 元素嵌套在其他 UI 元素内时，默认行为是用户将无法访问嵌套 UI 元素。

主要方案之一是：UI 在用户使用鼠标悬停在嵌套 UI 元素上方时显示，但它不会以其他方式显示。

![鼠标悬停在其上方时显示的 UI 元素](images/designing-for-tv/2d-navigation-best-practices-ui-elements-display-on-mouse-hover.png)

处理此游戏板/遥控器输入方案的建议方法是将这些 UI 元素放置在 `ContextFlyout` 中。

<!--[v-lcap] This doc just ends abruptly. I recommended adding a short summary as well as links to related topics, or at least to the parent topic in this set or the next level up-->

<!--### Drag and drop

TODO: Are we including this?
-->


<!--HONumber=Mar16_HO5-->


