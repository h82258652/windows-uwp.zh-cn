---
author: QuinnRadich
Description: Windows 10 和新开发人员工具提供了受新的通用 Windows 平台 (UWP) 支持的工具、功能和体验。
title: Windows 10 RTM 中面向开发人员的新增功能：2015 年 7 月
---

# Windows 10 RTM 中面向开发人员的新增功能：2015 年 7 月


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Windows 10 和新开发人员工具提供了受新的通用 Windows 平台 (UWP) 支持的工具、功能和体验。 在 Windows 10 上[安装这些工具和 SDK](https://dev.windows.com/downloads) 后，你就可以随时[创建新的通用 Windows 应用](https://msdn.microsoft.com/library/windows/apps/bg124288)或了解如何使用 [Windows 上的现有应用代码](https://msdn.microsoft.com/library/windows/apps/mt238321)。

下面来按功能逐个查看 Windows 10 RTM 中为你新增了哪些功能。

## 自适应布局


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">定制内容的多个视图</td>
<td align="left">XAML 为定义共享同一代码文件的定制视图（.xaml 文件）提供了新的支持。 这使你能够更加轻松地创建和维护针对特定设备系列或方案定制的不同视图。 如果你的应用具有不同的 UI 内容、布局或导航模型（因针对不同的方案而具有明显差异），请生成多个视图。 例如，你可以使用 [<strong>Pivot</strong>](https://msdn.microsoft.com/library/windows/apps/dn608241)（已针对移动应用优化了导航以便于单手使用），也可以使用 [<strong>SplitView</strong>](https://msdn.microsoft.com/library/windows/apps/dn864360)（已针对桌面应用优化了导航菜单以便使用鼠标）。</td>
</tr>
<tr class="even">
<td align="left">StateTriggers</td>
<td align="left">使用新的 [<strong>VisualState.StateTriggers</strong>] (https://msdn.microsoft.com/library/windows/apps/dn890396) 功能，你可以根据窗口高度/宽度或根据自定义触发器有条件地设置属性。 在此之前，你需要处理代码中的 Window [<strong>SizeChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br209055) 事件并调用 [<strong>VisualStateManager.GoToState</strong>](https://msdn.microsoft.com/library/windows/apps/br209025)。</td>
</tr>
<tr class="odd">
<td align="left">Setters</td>
<td align="left"><p>使用新的 [<strong>VisualState.Setters</strong>](https://msdn.microsoft.com/library/windows/apps/dn890395) 语法，你可以使用简化的标记来定义 [<strong>VisualStateManager</strong>](https://msdn.microsoft.com/library/windows/apps/br209021) 中的属性更改。 在此之前，你需要使用情节提要并创建动画才能应用属性更改（例如，将 [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635) 的方向从 Horizontal 更改为 Vertical）。 在通用 Windows 应用中，你可以使用以下简单的 Setter 语法：</p>
<p><code>&lt;setter target=&quot;stackPanel1.Orientation&quot; value=&quot;Vertical&quot; /&gt;</code></p></td>
</tr>
</tbody>
</table>

 

## XAML 功能


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">已编译的数据绑定 (x:Bind)</td>
<td align="left"><p>在通用 Windows 应用中，你可以使用新的基于编译器的绑定机制（通过 x:Bind 属性启用）。 基于编译器的绑定属于强类型，并且会在编译时进行处理，这样速度既快又可以在绑定类型不匹配时提供编译时错误。 而且由于绑定已转换为经过编译的应用代码，因此你现在可以通过在 Visual Studio 中单步调试代码诊断具体的绑定问题来调试绑定。 你也可以使用 x:Bind 绑定到某个方法，如下所示：</p>
<p><code>&lt;textblock text=&quot;{x:Bind Customer.Address.ToString()}&quot; /&gt;</code></p>
<p>对于典型的绑定方案，你可以使用 x:Bind 代替 Binding，并获取改进的性能和可维护性。</p></td>
</tr>
<tr class="even">
<td align="left">列表的声明性增量呈现 (x:Phase)</td>
<td align="left"><p>在通用 Windows 应用中，借助新的 x:Phase 属性，你可以使用 XAML 而不是代码来执行列表的增量或分段呈现。 当平移项目比较复杂的长列表时，你的应用可能无法以足够跟上平移速度的速度来呈现项目，从而为你的用户带来不良的体验。 分阶段呈现使你可以指定列表项目中各个元素的呈现优先级，所以只有列表项目的最重要部分才会在快速平移时呈现。 这将为你的用户带来更为顺畅的平移体验。</p>
<p>在 Windows 8.1 中，你可以处理 [<strong>ContainerContentChanging</strong>](https://msdn.microsoft.com/library/windows/apps/dn298914) 事件，并编写代码分阶段呈现列表项目。 在 UWP 应用中，你可以利用 x:Phase 属性以声明的方式完成分阶段呈现。 将 x:Phase 与已编译的绑定 x:Bind 结合使用使你可以轻松地为数据模板中的每个绑定元素指定呈现优先级。 平移时，呈现项目的工作会根据阶段分成时间片，从而支持增量项目呈现。</p></td>
</tr>
<tr class="odd">
<td align="left">UI 元素的延迟加载 (x: DeferLoadingStrategy)</td>
<td align="left">在通用 Windows 应用中，新的 x:DeferLoadingStrategy 指令使你可以指定要延迟加载的部分用户界面，从而改进应用的启动性能并降低应用的内存使用量。 例如，如果你的应用 UI 具有用于数据验证的元素，并且仅在输入不正确的数据时显示，则可以延迟加载该元素（除非需要显示）。 然后，元素对象不会在加载页面时创建；只有出现数据错误时或者需要添加到页面的可视化树时才会创建它们。</td>
</tr>
<tr class="even">
<td align="left">SplitView</td>
<td align="left">新的 [<strong>SplitView</strong>](https://msdn.microsoft.com/library/windows/apps/dn864360) 控件使你可以轻松地显示和隐藏临时性内容。 该控件通常用于&quot;汉堡包菜单&quot;之类的顶级导航方案，其中导航内容处于隐藏状态，可按需滑入作为用户操作的结果。</td>
</tr>
<tr class="odd">
<td align="left">RelativePanel</td>
<td align="left">[<strong>RelativePanel</strong>](https://msdn.microsoft.com/library/windows/apps/dn879546) 是新的布局面板，允许你放置子对象并使其相对于彼此对齐或与父面板对齐。 例如，你可以指定某些文本应始终放置在该面板的左侧，某个按钮应始终在文本下方对齐。 在创建没有清晰的线性图案（需要使用 [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635) 或 [<strong>Grid</strong>](https://msdn.microsoft.com/library/windows/apps/br242704)）的用户界面时，请使用 <strong>RelativePanel</strong>。</td>
</tr>
<tr class="even">
<td align="left">CalendarView</td>
<td align="left">[<strong>CalendarView</strong>](https://msdn.microsoft.com/library/windows/apps/dn890052) 控件使你能够轻松通过基于月份的可自定义视图查看和选择日期以及日期范围。 <strong>CalendarView</strong> 支持的功能有最小日期、最大日期和不适用日期，以用于限制可以选择的日期。 你还可以设置自定义密度栏，可用于显示某一天的计划的一般&quot;细节&quot;。</td>
</tr>
<tr class="odd">
<td align="left">CalendarDatePicker</td>
<td align="left">[<strong>CalendarDatePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn950083) 是一个下拉控件，已针对从 [<strong>CalendarView</strong>](https://msdn.microsoft.com/library/windows/apps/dn890052)（其中诸如星期几或日历的安排情况等上下文信息很重要）选取某个日期进行了优化。 它与 [<strong>DatePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn298584) 控件类似，但 <strong>DatePicker</strong> 是针对选取已知日期（如出生日期）进行优化的控件。</td>
</tr>
<tr class="even">
<td align="left">MediaTransportControls</td>
<td align="left">新的 [<strong>MediaTransportControls</strong>](https://msdn.microsoft.com/library/windows/apps/dn831962) 类使你能够轻松地自定义 [<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) 的传输控件。 在 Windows 8.1 中，你可以启用 <strong>MediaElement</strong> 的内置传输控件，也可以自行创建可调用 <strong>MediaElement</strong> 方法的传输控件。 现在，你可以使用 <strong>MediaTransportControls</strong> 的内置功能，并且仍可以轻松自定义适合你的应用的外观。</td>
</tr>
<tr class="odd">
<td align="left">属性更改通知</td>
<td align="left"><p>在通用 Windows 应用中，你可以侦听对 [<strong>DependencyObject</strong>](https://msdn.microsoft.com/library/windows/apps/br242356) 进行的属性更改，即使是没有相应更改事件的属性也可以。</p>
<p>该通知会像事件一样运行，但实际上作为回调公开。 该回调会像事件处理程序一样采用发送者参数，但不会采用事件参数。 仅传递属性标识符以指示何种属性。 借助此信息，你的应用可以为多个属性通知定义单个处理程序。 有关详细信息，请参阅 [<strong>RegisterPropertyChangedCallback</strong>](https://msdn.microsoft.com/library/windows/apps/dn831902) 和 [<strong>UnregisterPropertyChangedCallback</strong>](https://msdn.microsoft.com/library/windows/apps/dn831910)。</p></td>
</tr>
<tr class="even">
<td align="left">Maps</td>
<td align="left"><p>[<strong>MapControl</strong>](https://msdn.microsoft.com/library/windows/apps/dn637004) 类已更新，可提供 3D 鸟瞰视图和街景视图。 通用 Windows 应用现在可以使用这些新功能和更早的地图功能。 使用以下 API 向应用添加地图功能：</p>
<ul>
<li>[<strong>Windows.UI.Xaml.Controls.Maps</strong>](https://msdn.microsoft.com/library/windows/apps/dn610751) 命名空间 - 显示地图。</li>
<li>[<strong>Windows.Services.Maps</strong>](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空间 - 查找位置和路线。</li>
</ul>
<p>若要立即开始在通用 Windows 应用中使用这些 API，请从[必应地图开发人员中心](https://www.bingmapsportal.com/)请求一个密钥。 有关详细信息，请参阅[如何验证地图应用](https://msdn.microsoft.com/library/windows/apps/xaml/dn741528)。 Windows 10 还有一个新增功能，即电脑和手机用户可以从“设置”应用下载离线地图。 如果离线地图可用，[<strong>MapControl</strong>](https://msdn.microsoft.com/library/windows/apps/dn637004) 将在无 Internet 访问时使用离线地图来显示地图。</p></td>
</tr>
<tr class="odd">
<td align="left">输入按钮映射</td>
<td align="left">[<strong>Windows.UI.Xaml.Input.KeyRoutedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/hh943072) 类有一个新的 [<strong>OriginalKey</strong>](https://msdn.microsoft.com/library/windows/apps/dn960168) 属性，它连同 [<strong>Windows.System.VirtualKey</strong>](https://msdn.microsoft.com/library/windows/apps/br241812) 的相应更新使你可以获取未经过映射的原始输入按钮（与键盘输入事件相关联）。</td>
</tr>
<tr class="even">
<td align="left">墨迹书写</td>
<td align="left"><p>如今在使用 C++、C# 或 Visual Basic 的 Windows 运行时应用中使用可靠的墨迹书写功能更加方便，这要归功于 [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535) 控件和基本的 [<strong>InkPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn922011) 类。</p>
<p>[<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535) 控件定义用于绘制和呈现笔划墨迹的覆盖区域。 此控件的功能（输入、处理和呈现）来自 [<strong>InkPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn922011)、[<strong>InkStroke</strong>](https://msdn.microsoft.com/library/windows/apps/br208485)、[<strong>InkRecognizer</strong>](https://msdn.microsoft.com/library/windows/apps/br208478) 和 [<strong>InkSynchronizer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903979) 类。</p>
<p></p>
<div class="alert">
<strong>重要提示</strong> 这些类在使用 JavaScript 的 Windows 应用中不受支持。
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## 已更新的 XAML 功能


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">CommandBar 和 AppBar 更新</td>
<td align="left"><p>[<strong>CommandBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn279427) 和 [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) 控件已更新，使 UWP 应用在所有设备系列中都具有一致的 API、行为和用户体验。</p>
<p>适用于通用 Windows 应用的 [<strong>CommandBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn279427) 控件已改进，可提供 [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) 功能的超集，并且使其在应用中的使用方式有了更大的灵活性。 你应当对 Windows 10 中所有新的通用 Windows 应用使用 <strong>CommandBar</strong>。 在 Windows 8.1 的 <strong>CommandBar</strong> 中，你只能使用实现了 [<strong>ICommandBarElement</strong>](https://msdn.microsoft.com/library/windows/apps/dn251969) 的控件，如 [<strong>AppBarButton</strong>](https://msdn.microsoft.com/library/windows/apps/dn279244)。 在通用 Windows 应用中，除 <strong>AppBarButton</strong> 之外，你现在还可以将自定义内容置于 <strong>CommandBar</strong> 中。</p>
<p>[<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) 控件已更新，使你可以更加轻松地将使用 <strong>AppBar</strong> 的 Windows 8.1 应用移动到通用 Windows 平台。 <strong>AppBar</strong> 旨在用于全屏应用，通过边缘手势进行调用。 控件更新考虑到了 Windows 10 中窗口化应用和缺少边缘手势等问题。</p>
<p>隐藏的 [<strong>AppBar.ClosedDisplayMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn633872)（之前仅在 Windows Phone 中受支持），现已在所有设备系列上受支持，从而使你可以在不同级别的命令提示之间进行选择。 当你为了在该平台中不再依赖边缘手势支持而将 Windows 8.1 应用升级到通用 Windows 应用时，[<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) 默认显示最少提示来为你提供一致性。</p>
<p><strong>新的</strong> [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) <strong>API：</strong>[<strong>Closing</strong>](https://msdn.microsoft.com/library/windows/apps/dn996483)、[<strong>OnClosing</strong>](https://msdn.microsoft.com/library/windows/apps/dn996484)、[<strong>Opening</strong>](https://msdn.microsoft.com/library/windows/apps/dn996486)、[<strong>OnOpening</strong>](https://msdn.microsoft.com/library/windows/apps/dn996485)、[<strong>TemplateSettings</strong>](https://msdn.microsoft.com/library/windows/apps/dn996487)。</p>
<p><strong>新的</strong> [<strong>CommandBar</strong>] (https://msdn.microsoft.com/library/windows/apps/dn279427) <strong>API：</strong>[<strong>CommandBarOverflowPresenterStyle</strong>] (https://msdn.microsoft.com/library/windows/apps/dn975227) 和 [<strong>CommandBarOverflowPresenter</strong>] (https://msdn.microsoft.com/library/windows/apps/dn975225)。</p></td>
</tr>
<tr class="even">
<td align="left">GridView 更新</td>
<td align="left">在 Windows 10 之前，默认的 [<strong>GridView</strong>] (https://msdn.microsoft.com/library/windows/apps/br242705) 布局方向如下：在 Windows 中为水平，而在 Windows Phone 中为垂直。 在 UWP 应用中，<strong>GridView</strong> 针对所有设备系列默认使用垂直布局，以确保让你拥有一致的默认体验。</td>
</tr>
<tr class="odd">
<td align="left">AreStickyGroupHeadersEnabled 属性</td>
<td align="left">当你在 [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) 或 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) 中显示经过分组的数据时，组标头会在滚动列表时保持可见。 这在大型数据集中很重要，因为标头会提供用户正在查看的数据的上下文。 但是，如果每组中只有几个元素，你可能需要将这些标头滚动到具有这些项目的屏幕之外。 你可以设置 [<strong>ItemsStackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/dn298795) 和 [<strong>ItemsWrapGrid</strong>](https://msdn.microsoft.com/library/windows/apps/dn298849) 上的 [<strong>AreStickyGroupHeadersEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn932028) 属性，以便控制此行为。</td>
</tr>
<tr class="even">
<td align="left">GroupHeaderContainerFromItemContainer 方法</td>
<td align="left">当你在 [<strong>ItemsControl</strong>](https://msdn.microsoft.com/library/windows/apps/br242803) 中显示经过分组的数据时，你可以调用 [<strong>GroupHeaderContainerFromItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903984) 方法来获取对该组父标头的引用。 例如，如果用户删除组中的最后一项，你可以获取对组标头的引用，并且可以同时删除该项和组标头。</td>
</tr>
<tr class="odd">
<td align="left">ChoosingGroupHeaderContainer 事件</td>
<td align="left">[<strong>ListViewBase</strong>](https://msdn.microsoft.com/library/windows/apps/br242879) 上的新 [<strong>ChoosingGroupHeaderContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn899082) 事件使你可以在 [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) 或 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) 中针对组标头设置状态。 例如，你可能会处理此事件来设置组标头上的 [<strong>AutomationProperties.NameProperty</strong>](https://msdn.microsoft.com/library/windows/apps/br209099)，从而采用辅助技术来表示组。</td>
</tr>
<tr class="even">
<td align="left">ChoosingItemContainer 事件</td>
<td align="left">[<strong>ListViewBase</strong>](https://msdn.microsoft.com/library/windows/apps/br242879) 上的新 [<strong>ChoosingItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903989) 事件使你可以更好地控制 [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) 或 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) 中的 UI 虚拟化。 结合 [<strong>ContainerContentChanging</strong>](https://msdn.microsoft.com/library/windows/apps/dn298914) 事件来使用此事件，以维持你自己的要根据需要进行利用的回收容器队列。 例如，如果由于筛选而重置了数据源，你可以快速地将一组已创建好的视觉效果 (ItemContainers) 与其数据匹配，以便实现最佳性能。</td>
</tr>
<tr class="odd">
<td align="left">列表滚动虚拟化</td>
<td align="left"><p>XAML [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) 和 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) 控件具有新的 [<strong>ListViewBase.ChoosingItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903989) 事件，可在数据收集发生变更时提升控件性能。</p>
<p>现在，系统将维护当前视图中的项目以及焦点和选择状态，而不是完全重置列表，以免重播进入动画；视口中新建和已删除的项目会流畅地以动画方式进入和离开。 一旦未破坏的容器中的数据集合发生变化，应用即可快速将所有&quot;旧&quot;项与其之前的容器匹配，并跳过容器生命周期替代方法的进一步处理。 仅&quot;新&quot;项得到处理并与回收的容器或新容器相关联。</p></td>
</tr>
<tr class="even">
<td align="left">SelectRange 方法和 SelectedRanges 属性</td>
<td align="left">在通用 Windows 应用中，[<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) 和 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) 控件现在使你可以根据项目索引的范围（而不是项目对象引用）选择项目。 使用此方法描述项目选择更有效率，因为不需要为每个所选的项目创建项目对象。 有关详细信息，请参阅 [<strong>ListViewBase.SelectedRanges</strong>](https://msdn.microsoft.com/library/windows/apps/dn904244)、[<strong>ListViewBase.SelectRange</strong>](https://msdn.microsoft.com/library/windows/apps/dn904245) 和 [<strong>ListViewBase.DeselectRange</strong>](https://msdn.microsoft.com/library/windows/apps/dn904241)。</td>
</tr>
<tr class="odd">
<td align="left">新的 ListViewItemPresenter API</td>
<td align="left">[<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) 和 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) 使用项目表示器为选择和焦点提供默认视觉效果。 在 UWP 应用中，[<strong>ListViewItemPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn298500) 和 [<strong>GridViewItemPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn279298) 具有一些新属性，使你可以进一步自定义列表项目的视觉效果。 这些新属性有 [<strong>CheckBoxBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn913905)、[<strong>CheckMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn913923)、[<strong>FocusSecondaryBorderBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn898370)、[<strong>PointerOverForeground</strong>](https://msdn.microsoft.com/library/windows/apps/dn898372)、[<strong>PressedBackground</strong>](https://msdn.microsoft.com/library/windows/apps/dn913931) 和 [<strong>SelectedPressedBackground</strong>](https://msdn.microsoft.com/library/windows/apps/dn913937)。</td>
</tr>
<tr class="even">
<td align="left">SemanticZoom 更新</td>
<td align="left"><p>对于跨所有设备系列的 UWP 应用，[<strong>SemanticZoom</strong>](https://msdn.microsoft.com/library/windows/apps/hh702601) 控件现在具有一致的行为。</p>
<p>在放大视图和缩小视图之间进行切换的默认操作是点击放大视图中的组标头。 这与 Windows Phone 8.1 中的行为相同，但与 Windows 8.1 中的行为不同，对于后者，可使用收缩手势进行缩放。 若要使用捏合缩放更改视图，请在 [<strong>SemanticZoom</strong>](https://msdn.microsoft.com/library/windows/apps/hh702601) 的内部 [<strong>ScrollViewer</strong>](https://msdn.microsoft.com/library/windows/apps/br209527) 上设置 [[<strong>ScrollViewer.ZoomMode</strong>](https://msdn.microsoft.com/library/windows/apps/br209601)=&quot;Enabled&quot;。</p>
<p>对于通用 Windows 应用，缩小视图会替换放大视图，并且大小与其替换的视图相同。 这与 Windows 8.1 中的行为相同，但与 Windows Phone 8.1 中的行为不同，对于后者，缩小视图的大小占据了整个屏幕，并且在所有其他内容之上呈现。</p></td>
</tr>
<tr class="odd">
<td align="left">DatePicker 和 TimePicker 更新</td>
<td align="left">对于跨所有设备系列的通用 Windows 应用，[<strong>DatePicker</strong>] (https://msdn.microsoft.com/library/windows/apps/dn298584) 和 [<strong>TimePicker</strong>] (https://msdn.microsoft.com/library/windows/apps/dn299280) 控件现在具有一致的实现。 它们还具有适用于 Windows 10 的新外观。 现在，弹出窗口部分在所有设备上都使用的是 [<strong>DatePickerFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn625013) 和 [<strong>TimePickerFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn608313) 控件。 这与 Windows Phone 8.1 中的行为相同，但与 Windows 8.1 中的行为不同，对于后者，使用的是 [<strong>ComboBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209348) 控件。 使用浮出控件使你可以更轻松地创建自定义日期和时间选取器。</td>
</tr>
<tr class="even">
<td align="left">新的 ScrollViewer API</td>
<td align="left">[<strong>ScrollViewer</strong>] (https://msdn.microsoft.com/library/windows/apps/br209527) 具有新的 [<strong>DirectManipulationStarted</strong>] (https://msdn.microsoft.com/library/windows/apps/dn858544) 和 [<strong>DirectManipulationCompleted</strong>] (https://msdn.microsoft.com/library/windows/apps/dn858543) 事件，以便通知你的应用触摸平移何时开始和停止。 你可以处理这些事件以使你的 UI 与这些用户操作相协调。</td>
</tr>
<tr class="odd">
<td align="left">MenuFlyout 更新</td>
<td align="left">在通用 Windows 应用中，有一些新的 API 使你可以更轻松地生成更好的上下文菜单。 新的 [<strong>MenuFlyout.ShowAt</strong>] (https://msdn.microsoft.com/library/windows/apps/dn913174) 方法使你可以指定你希望浮出控件出现的位置（相对于其他元素）。 （而且，你的 [<strong>MenuFlyout</strong>] (https://msdn.microsoft.com/library/windows/apps/dn299030) 甚至可以重叠应用窗口的边界。）使用新的 [<strong>MenuFlyoutSubItem</strong>] (https://msdn.microsoft.com/library/windows/apps/dn913170) 类来创建级联菜单。</td>
</tr>
<tr class="even">
<td align="left">ContentPresenter、Grid 和 StackPanel 的新 Border 属性</td>
<td align="left">常见容器控件具有新的边框属性，可用于在其周围绘制边框，而无需向你的 XAML 添加其他 Border 元素。 [<strong>ContentPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/br209378)、[<strong>Grid</strong>](https://msdn.microsoft.com/library/windows/apps/br242704) 和 [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635) 具有以下新属性：[<strong>BorderBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn960179)、[<strong>BorderThickness</strong>](https://msdn.microsoft.com/library/windows/apps/dn960181)、[<strong>CornerRadius</strong>](https://msdn.microsoft.com/library/windows/apps/dn960183) 和 [<strong>Padding</strong>](https://msdn.microsoft.com/library/windows/apps/dn960187)。</td>
</tr>
<tr class="odd">
<td align="left">ContentPresenter 上的新文本 API</td>
<td align="left">[<strong>ContentPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/br209378) 具有以下新 API，使你可以更好地控制文本显示：[<strong>LineHeight</strong>](https://msdn.microsoft.com/library/windows/apps/dn903982)、[<strong>LineStackingStrategy</strong>](https://msdn.microsoft.com/library/windows/apps/dn831933)、[<strong>MaxLines</strong>](https://msdn.microsoft.com/library/windows/apps/dn831935) 和 [<strong>TextWrapping</strong>](https://msdn.microsoft.com/library/windows/apps/dn831937)。</td>
</tr>
<tr class="even">
<td align="left">系统焦点视觉效果</td>
<td align="left">XAML 控件的焦点视觉效果现在由系统创建，而不是作为 XAML 元素在控件模板中声明。 在移动设备上，通常不需要焦点视觉效果，让系统根据需要创建并管理它们可提升性能。 如果你需要更好地控制焦点视觉效果，可以替代系统行为并提供定义焦点视觉效果的自定义控件模板。 有关详细信息，请参阅 [<strong>UseSystemFocusVisuals</strong>](https://msdn.microsoft.com/library/windows/apps/dn899076) 和 [<strong>IsTemplateFocusTarget</strong>](https://msdn.microsoft.com/library/windows/apps/dn899074)。</td>
</tr>
<tr class="odd">
<td align="left">PasswordBox.PasswordRevealMode</td>
<td align="left"><p>在通用 Windows 应用中，[<strong>PasswordRevealMode</strong>] (https://msdn.microsoft.com/library/windows/apps/dn890867) 属性会替换 [<strong>IsPasswordRevealButtonEnabled</strong>] (https://msdn.microsoft.com/library/windows/apps/hh702579) 属性，以便在各设备系列中提供一致的行为。</p>
<p></p>
<div class="alert">
<strong>注意</strong> 在 Windows 10 之前，密码显示按钮默认情况下不显示；在通用 Windows 应用中，将在默认情况下显示。 如果应用安全要求始终掩盖密码，请务必将 [<strong>PasswordRevealMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn890867) 设置为 Hidden。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">Control.IsTextScaleFactorEnabled</td>
<td align="left">Windows Phone 8.1 中提供的 [<strong>IsTextScaleFactorEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn624997) 属性现在可用于跨所有设备系列的通用 Windows 应用。</td>
</tr>
<tr class="odd">
<td align="left">AutoSuggestBox</td>
<td align="left">Windows Phone 8.1 中的 [<strong>AutoSuggestBox</strong>](https://msdn.microsoft.com/library/windows/apps/dn633874) 控件现在可用于跨所有设备系列的通用 Windows 应用，并且你应当使用它来取代 [<strong>SearchBox</strong>](https://msdn.microsoft.com/library/windows/apps/dn252771)。 <strong>AutoSuggestBox</strong> 会在用户键入内容时提供建议，并且适用于各种输入类型（如触摸、键盘和输入法编辑器）。 它还有一些新成员，使其可以作为搜索框更好地工作：[<strong>QueryIcon</strong>](https://msdn.microsoft.com/library/windows/apps/dn996494) 属性和 [<strong>QuerySubmitted</strong>] (https://msdn.microsoft.com/library/windows/apps/dn996497) 事件。</td>
</tr>
<tr class="even">
<td align="left">ContentDialog</td>
<td align="left">Windows Phone 8.1 中的 [<strong>ContentDialog</strong>](https://msdn.microsoft.com/library/windows/apps/dn633972) 控件现在可用于跨所有设备系列的通用 Windows 应用。 <strong>ContentDialog</strong> 允许你显示可自定义的模式对话框，该对话框可以在所有类型的设备中很好地运行。</td>
</tr>
<tr class="odd">
<td align="left">Pivot</td>
<td align="left">Windows Phone 8.1 中的 [<strong>Pivot</strong>](https://msdn.microsoft.com/library/windows/apps/dn608241) 控件现在可用于跨所有设备系列的通用 Windows 应用。 你现在可以对移动设备应用和桌面设备应用使用相同的 <strong>Pivot</strong> 控件。 <strong>Pivot</strong> 会根据屏幕大小和输入类型提供自适应行为。 你可以设置 <strong>Pivot</strong> 控件的样式来提供类似选项卡的行为，同时在每个透视项目中具有不同的信息视图。</td>
</tr>
</tbody>
</table>

 

## 文本


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Windows 核心文本 API</td>
<td align="left"><p>新的 [<strong>Windows.UI.Text.Core</strong>] (https://msdn.microsoft.com/library/windows/apps/dn958238) 命名空间是客户端服务器系统的一项特色功能，可以将键盘输入的处理集中到单个服务器中。</p>
<p>你可以使用它来操作自定义文本输入控件的编辑缓冲区。 文本输入服务器通过应用和服务器之间的异步信道确保文本输入控件的内容及其编辑缓冲区的内容始终处于同步状态。</p></td>
</tr>
<tr class="even">
<td align="left">矢量图标</td>
<td align="left">[<strong>Glyphs</strong>](https://msdn.microsoft.com/library/windows/apps/br209921) 元素具有新的 [<strong>IsColorFontEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn900468) 和 [<strong>ColorFontPalleteIndex</strong>](https://msdn.microsoft.com/library/windows/apps/dn900466) 属性，用于支持颜色字体；现在，你可以使用字体文件来呈现基于字体的图标。 当你使用 <strong>ColorFontPalleteIndex</strong> 进行调色板切换时，可通过不同的颜色集呈现单个图标；例如，显示图标的启用和禁用版本。</td>
</tr>
<tr class="odd">
<td align="left">输入法编辑器窗口事件</td>
<td align="left">用户有时会通过在文本输入框正下方的窗口中显示的输入法编辑器输入文字（通常用于东亚语言）。 你可以使用 [<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683) 和 [<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548) 上的 [<strong>CandidateWindowBoundsChanged</strong>](https://msdn.microsoft.com/library/windows/apps/dn955144) 事件和 [<strong>DesiredCandidateWindowAlignment</strong>](https://msdn.microsoft.com/library/windows/apps/dn955145) 属性来确保你的应用 UI 可以更好地与 IME 窗口配合工作。</td>
</tr>
<tr class="even">
<td align="left">文本排版事件</td>
<td align="left">[<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683) 和 [<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548) 的新事件会通知你的应用何时使用输入法编辑器对文本进行排版，这些事件有：[<strong>TextCompositionStarted</strong>](https://msdn.microsoft.com/library/windows/apps/dn891458)、[<strong>TextCompositionEnded</strong>](https://msdn.microsoft.com/library/windows/apps/dn891457) 和 [<strong>TextCompositionChanged</strong>](https://msdn.microsoft.com/library/windows/apps/dn891456)。 你可以处理这些事件以使你的应用代码与 IME 文本排版过程相协调。 例如，你可以针对东亚语言实现内联式自动完成功能。</td>
</tr>
<tr class="odd">
<td align="left">改进对双向文本的处理</td>
<td align="left"><p>XAML 文本控件的新 API 可改进对双向文本的处理，从而在各种输入语言中更好地实现文本对齐和段落方向性。</p>
<p>[<strong>TextReadingOrder</strong>](https://msdn.microsoft.com/library/windows/apps/dn949133) 属性的默认值已更改为 <strong>DetectFromContent</strong>，因此默认情况下，对检测阅读顺序的支持处于启用状态。 <strong>TextReadingOrder</strong> 属性也已添加到 [<strong>PasswordBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227519)、[<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548) 和 [<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683) 中。</p>
<p>你可以将文本控件上的 [<strong>TextAlignment</strong>](https://msdn.microsoft.com/library/windows/apps/br209703) 属性设置为新的 <strong>DetectFromContent</strong> 值，以选择在内容中自动检测对齐。</p></td>
</tr>
<tr class="even">
<td align="left">文本呈现</td>
<td align="left">在 Windows 10 中，XAML 应用中的文本呈现速度现在大多比 Windows 8.1 将近快两倍。 在大多数情况下，你的应用将受益于这一改进，而无需进行任何更改。 除了更快地呈现之外，这些改进还降低了 5% 的 XAML 应用典型内存消耗。</td>
</tr>
</tbody>
</table>

 

## 应用程序模型


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Cortana</td>
<td align="left"><p>通过语音命令扩展 <strong>Cortana</strong> 的基本功能，这些命令用于在外部应用程序中启动并执行一个单独操作。</p>
<p>通过集成应用的基本功能，并为用户提供中心入口点以便在不必直接打开应用的情况下即可完成大多数任务，<strong>Cortana</strong> 可以充当应用和用户之间的联络人。 在大多数情况下，这可以为用户节省大量时间和精力。</p>
<p>了解如何[将应用集成到 Cortana 画布](https://msdn.microsoft.com/library/windows/apps/xaml/dn974230)。 如果你需要创意，可以参考[通用 Windows 应用设计基础知识](https://dev.windows.com/design/design-basics)中特定于 <strong>Cortana</strong> 的设计建议和 UX 指南。</p></td>
</tr>
<tr class="even">
<td align="left">文件资源管理器</td>
<td align="left">新的 [<strong>Windows.System.Launcher.LaunchFolderAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn889616) 方法允许你启动文件资源管理器并显示你指定的文件夹中的内容。</td>
</tr>
<tr class="odd">
<td align="left">共享存储</td>
<td align="left">新的 [<strong>Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager</strong>](https://msdn.microsoft.com/library/windows/apps/dn889985) 类及其方法允许你在使用 URI 激活启动其他应用时，通过传递共享令牌与其他应用共享文件。 目标应用会兑换令牌以获取源应用共享的文件。</td>
</tr>
<tr class="even">
<td align="left">设置</td>
<td align="left"><p>通过结合使用 ms-settings 协议和 [<strong>LaunchUriAsync</strong>](https://msdn.microsoft.com/library/windows/apps/hh701476) 方法来显示内置设置页面。 例如，以下代码显示 WLAN 设置的页面。</p>
<p><code>bool result = await Launcher.LaunchUriAsync(new Uri(&quot;ms-settings://network/wifi&quot;));</code></p>
<p>有关可以显示的设置页面列表，请参阅[如何使用 ms-settings 协议显示内置设置页面](https://msdn.microsoft.com/library/windows/apps/jj207014.aspx)。</p></td>
</tr>
<tr class="odd">
<td align="left">应用到应用的通信</td>
<td align="left"><p>通过 Windows 10 中新增的[应用到应用的通信](https://msdn.microsoft.com/library/windows/apps/xaml/dn997827) API，Windows 应用程序（以及 Windows Web 应用程序）可以相互启动并交换数据和文件。</p>
<p>利用这些新 API，使得原本需要用户使用多个应用程序才能完成的复杂任务现在可以无缝地进行处理。 例如，你的应用可启动社交网络应用来选择联系人，或启动结算应用程序来完成支付流程。</p></td>
</tr>
<tr class="even">
<td align="left">应用服务</td>
<td align="left">应用服务是应用在 Windows 10 中向其他应用提供服务的方法。 应用服务的表现形式为后台任务。 前台应用可以调用其他应用中的应用服务以在后台执行任务。 有关应用服务 API 的参考信息，请参阅 [<strong>Windows.ApplicationModel.AppService</strong>](https://msdn.microsoft.com/library/windows/apps/dn921731)。</td>
</tr>
<tr class="odd">
<td align="left">应用包清单</td>
<td align="left"><p>对 Windows 10 的[程序包清单架构](https://msdn.microsoft.com/library/windows/apps/br211474)参考的更新包括已添加、已删除和已更改的元素。</p>
<p>有关该架构中所有元素、属性和类型的参考信息，请参阅[元素层次结构](https://msdn.microsoft.com/library/windows/apps/dn934819)。</p></td>
</tr>
</tbody>
</table>

 

## 设备


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Microsoft Surface Hub</td>
<td align="left"><p>Microsoft Surface Hub 是一款强大的团队协作设备和大屏幕平台，适用于在 Surface Hub 或连接的设备中以本机方式运行的通用 Windows 应用。</p>
<p>针对自己的业务生成可以利用大屏幕、触摸和墨迹输入以及各种板载硬件（如相机和传感器）的应用。</p>
<p>请参阅[通用 Windows 应用设计基础知识](https://dev.windows.com/design/design-basics)中特定于 Surface Hub 的设计建议和 UX 指南。 这些文档介绍了通用 Windows 应用的响应式设计技术。</p>
<p>有关支持公用共享应用的详细信息，请参阅 [<strong>SharedModeSettings</strong>](https://msdn.microsoft.com/library/windows/apps/dn949019)。</p>
<p>有关墨迹输入以及对新 [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535) 控件上多点墨迹书写的支持的详细信息，请参阅 [<strong>Windows.UI.Input.Inking</strong>](https://msdn.microsoft.com/library/windows/apps/br208524) 和 [<strong>Windows.UI.Input.Inking.Core</strong>](https://msdn.microsoft.com/library/windows/apps/dn958452)。</p>
<p>有关处理传感器输入的信息，请参阅[集成设备、打印机和传感器](https://msdn.microsoft.com/library/windows/apps/br229563)。</p></td>
</tr>
<tr class="even">
<td align="left">位置</td>
<td align="left"><p>Windows 10 引入了一种新方法来提示用户以获取访问其位置 [<strong>RequestAccessAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn859152) 的访问权限。</p>
<p>用户可通过<strong>“设置”</strong>应用中的<strong>“位置隐私设置”</strong>来设置其位置数据隐私。 仅当出现以下情况时，你的应用才可以访问用户的位置：</p>
<ul>
<li><strong>此设备的位置</strong>已打开（不适用于 Windows 10 手机版）。</li>
<li>位置服务设置“<strong>位置</strong>”已<strong>打开</strong>。</li>
<li>在<strong>“选择可以使用你的位置的应用”</strong>下，你的应用已设置为<strong>“打开”</strong>。</li>
</ul>
<p>在访问用户的位置之前，请务必调用 [<strong>RequestAccessAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn859152)。 此时，你的应用必须位于前台，并且必须从 UI 线程中调用 <strong>RequestAccessAsync</strong>。 除非用户向你的应用授予访问其位置的权限，否则你的应用将无法访问位置数据。</p></td>
</tr>
<tr class="odd">
<td align="left">AllJoyn</td>
<td align="left"><p>[<strong>Windows.Devices.AllJoyn</strong>](https://msdn.microsoft.com/library/windows/apps/dn894971) Windows 运行时命名空间引入了 Microsoft 对 AllJoyn 开源软件框架和服务的实现。 这些 API 使你的通用 Windows 设备应用可以加入 AllJoyn 支持的物联网 (IoT) 方案中的其他设备。 有关 AllJoyn C API 的详细信息，请在 [AllSeen Alliance](https://allseenalliance.org/) 处下载相关文档。</p>
<p>使用该版本中所包含的 [AllJoynCodeGen 工具](https://msdn.microsoft.com/library/windows/apps/dn913809)，生成可用于在设备应用中启用 AllJoyn 方案的 Windows 组件。</p>
<div class="alert">
<strong>注意</strong> Windows 10 IoT 核心版现在已可用于新类型的小型设备，从而允许你利用 Windows 和 Visual Studio 创建“物联网”(IoT) 设备。 了解有关 Windows IoT 的详细信息，网址为 [WindowsOnDevices.com](http://www.windowsondevices.com/)。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">在移动设备 (XAML) 上打印 API</td>
<td align="left">有一组单独且统一的 API 使你可以在各设备系列（包括移动设备）中基于 XAML 的 UWP 应用内进行打印。 你现在可以通过使用 [<strong>Windows.Graphics.Printing</strong>](https://msdn.microsoft.com/library/windows/apps/br226489) 和 [<strong>Windows.UI.Xaml.Printing</strong>](https://msdn.microsoft.com/library/windows/apps/br243325) 命名空间中你所熟悉且与打印相关的 API 向移动应用添加打印功能。</td>
</tr>
<tr class="odd">
<td align="left">电池</td>
<td align="left"><p>[<strong>Windows.Devices.Power</strong>](https://msdn.microsoft.com/library/windows/apps/dn895017) 命名空间中的电池 API 使你的应用可以了解连接到设备（正在运行你的应用）的任何电池的详细信息。</p>
<p>创建 [<strong>Battery</strong>](https://msdn.microsoft.com/library/windows/apps/dn895004) 对象来表示单个电池控制器或所有电池控制器的总计（如果通过 [<strong>FromIdAsync</strong>] (https://msdn.microsoft.com/library/windows/apps/dn895013) 或 [<strong>AggregateBattery</strong>] (https://msdn.microsoft.com/library/windows/apps/dn895011) 分别创建）。</p>
<p>使用 [<strong>GetReport</strong>](https://msdn.microsoft.com/library/windows/apps/dn895015) 方法返回可指示相应电池充电、容量和状态的 [<strong>BatteryReport</strong>](https://msdn.microsoft.com/library/windows/apps/dn895005) 对象。</p></td>
</tr>
<tr class="even">
<td align="left">MIDI 设备</td>
<td align="left"><p>新的 [<strong>Windows.Devices.Midi</strong>] (https://msdn.microsoft.com/library/windows/apps/dn895002) 命名空间允许你创建：</p>
<ul>
<li>可以与外部 MIDI 设备进行通信的应用。</li>
<li>直接与 Microsoft GS MIDI 软件合成器进行通信的应用和外部设备。</li>
<li>多个客户端同时访问单个 MIDI 端口的方案。</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">自定义传感器支持</td>
<td align="left">[<strong>Windows.Devices.Sensors.Custom</strong>] (https://msdn.microsoft.com/library/windows/apps/dn895032) 命名空间允许硬件开发人员定义新的自定义传感器类型，如 CO2 传感器。</td>
</tr>
<tr class="even">
<td align="left">基于主机的卡仿真 (HCE)</td>
<td align="left"><p>主机卡仿真使你能够实现操作系统中托管的 NFC 卡仿真服务，并且仍可以通过 NFC 射频与外部读取器终端进行通信。</p>
<p>实现后台任务以通过 NFC 进行智能卡仿真。 若要触发该后台任务，请使用 [<strong>SmartCardTrigger</strong>](https://msdn.microsoft.com/library/windows/apps/dn624853) 类。</p>
<p>[<strong>SmartCardTriggerType</strong>] (https://msdn.microsoft.com/library/windows/apps/dn608017) 枚举中的 <strong>EmulatorHostApplicationActivated</strong> 值使你的应用可以了解是否已发生 HCE 事件。</p></td>
</tr>
</tbody>
</table>

 

## 图形


|                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DirectX              | Windows 10 中的 DirectX 12 引入了下一版本的 Microsoft Direct3D，它是 DirectX 的核心 3D 图形 API。 [Direct3D 12 图形](https://msdn.microsoft.com/library/windows/desktop/dn903821)可实现类似控制台的低级别 API 的效率和性能。 Direct3D 12 比以往更快且更有效率。 它可以实现更丰富的场景、更多的对象、更复杂的效果，并且可以更好地利用现代图形硬件。                                                                                                                                                                                                                                                                                     |
| SoftwareBitmapSource | 在通用 Windows 应用中，你可以使用新的 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) 类型作为 XAML 图像源。 这样，你可以将未编码图像传递到 XAML 框架，以便立即在屏幕上显示，从而通过 XAML 框架绕过图像解码。 你可以实现更快的图像呈现，如直接从相机呈现低延迟的照片、使用自定义图像解码器、从 DirectX 表面捕获帧或从头开始创建内存中的图像，并通过低延迟和低内存开销的方式直接在 XAML 中将其全部呈现。                                                                                                     |
| 透视相机   | 在通用 Windows 应用中，XAML 的新 Transform3D API 允许你向 XAML 树（或场景）应用透视转换，它将根据该单个场景范围转换（或相机）来转换所有 XAML 子元素。 你之前可以通过 MatrixTransform 和复杂的数学运算执行此操作，但 Transform3D 极大地简化了此效果，并且还支持对此效果进行动画处理。 有关详细信息，请参阅 [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/dn906919) 属性、[**Transform3D**](https://msdn.microsoft.com/library/windows/apps/dn914748)、[**CompositeTransform3D**](https://msdn.microsoft.com/library/windows/apps/dn914714) 和 [**PerspectiveTransform3D**](https://msdn.microsoft.com/library/windows/apps/dn914740)。 |

 

## 媒体


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">HTTP 实时传送视频流</td>
<td align="left">你可以使用新的 [<strong>AdaptiveMediaSource</strong>] (https://msdn.microsoft.com/library/windows/apps/dn946912) 类向应用添加自适应视频流功能。 通过将对象指向流清单文件可初始化该对象。 支持的清单格式包括 Http 实时传送视频流 (HLS) 和 HTTP 动态自适应流式处理 (DASH)。 对象绑定到 XAML 媒体元素后，即开始自适应播放。 在适当的时候可以查询和设置流的属性，如可用的比特率、最小比特率和最大比特率。</td>
</tr>
<tr class="even">
<td align="left">对媒体基础转换 (MFTs) 的媒体基础转换代码视频处理器 (XVP) 支持</td>
<td align="left"><p>使用媒体基础转换 (MFT) 的 Windows 应用现在可以使用<strong>媒体基础转换代码视频处理器</strong> (XVP) 来改变、缩放和转换原始视频数据：</p>
<p>新 [MF_XVP_CALLER_ALLOCATES_OUTPUT](https://msdn.microsoft.com/library/windows/desktop/dn803919) 属性支持输出到调用方分配的纹理，即使在 Microsoft DirectX 视频加速 (DXVA) 模式下也是如此。</p>
<p>新的 [<strong>IMFVideoProcessorControl2</strong>] (https://msdn.microsoft.com/library/windows/desktop/dn800741) 接口使应用可以启用硬件效果、查询受支持的硬件效果以及替代视频处理器执行的旋转操作。</p></td>
</tr>
<tr class="odd">
<td align="left">转码</td>
<td align="left">新的 [<strong>MediaProcessingTrigger</strong>] (https://msdn.microsoft.com/library/windows/apps/dn806005) API 允许你的应用在后台任务中执行媒体转码，这样即使在前台应用终止后也可以继续进行转码操作。</td>
</tr>
<tr class="even">
<td align="left">MediaElement 媒体故障事件</td>
<td align="left">在通用 Windows 应用中，[<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) 将播放包含多个流的内容，即使存在解码错误的流，只要该媒体内容包含至少一个有效流即可。 例如包含了一个音频流和一个视频流的内容，如果其中的视频流无法播放，<strong>MediaElement</strong> 仍会播放音频流。 [<strong>PartialMediaFailureDetected</strong>] (https://msdn.microsoft.com/library/windows/apps/dn889635) 事件将通知你媒体流中是否存在无法解码的流。 它还会告知你哪些类型的流无法播放，以便你可以在 UI 中反映该信息。 如果媒体流中的所有流都无法播放，将引发 [<strong>MediaFailed</strong>](https://msdn.microsoft.com/library/windows/apps/br227393) 事件。</td>
</tr>
<tr class="odd">
<td align="left">对使用 MediaElement 的自适应视频流的支持</td>
<td align="left">[<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) 具有新的 [<strong>SetPlaybackSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn899085) 方法，可支持自适应视频流。 使用此方法将媒体源设置为 [<strong>AdaptiveMediaSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn946912)。</td>
</tr>
<tr class="even">
<td align="left">通过 MediaElement 和图像转换</td>
<td align="left">[<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) 和 [<strong>Image</strong>](https://msdn.microsoft.com/library/windows/apps/br242752) 控件具有新的 [<strong>GetAsCastingSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn920012) 方法。 你可以使用此方法以编程方式将内容从任何媒体或图像元素发送到更多类型的远程设备，如 Miracast、蓝牙和 DLNA。如果你将 <strong>MediaElement</strong> 上的 [<strong>AreTransportControlsEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn298977) 设置为 true，此功能将自动启用。</td>
</tr>
<tr class="odd">
<td align="left">适用于桌面应用的媒体传输控件</td>
<td align="left">[<strong>ISystemMediaTransportControls</strong>](https://msdn.microsoft.com/library/windows/desktop/dn892299) 接口和相关的 API 允许桌面应用与内置的系统媒体传输控件交互。 其中包括：响应用户与传输控件按钮的交互，以及更新传输控件显示内容以显示有关当前正在播放的媒体内容的元数据。</td>
</tr>
<tr class="even">
<td align="left">随机访问 JPEG 编码和解码</td>
<td align="left">新的 WIC 方法 [<strong>IWICJpegFrameEncode</strong>](https://msdn.microsoft.com/library/windows/desktop/dn903864) 和 [<strong>IWICJpegFrameDecode</strong>] (https://msdn.microsoft.com/library/windows/desktop/dn903834) 支持对 JPEG 图像进行编码和解码。 你现在还可以启用为图像数据编制的索引，从而可以有效地随机访问大型图像，但代价是会产生较大的内存占用量。</td>
</tr>
<tr class="odd">
<td align="left">用于媒体合成的覆盖</td>
<td align="left">新的 [<strong>MediaOverlay</strong>] (https://msdn.microsoft.com/library/windows/apps/dn764793) 和 [<strong>MediaOverlayLayer</strong>] (https://msdn.microsoft.com/library/windows/apps/dn764795) API 使你可以轻松地向媒体合成添加多个图层的静态或动态媒体内容。 可以针对每个图层调整不透明度、位置和计时，并且甚至可以为输入层实现你自己的自定义合成器。</td>
</tr>
<tr class="even">
<td align="left">新的特效框架</td>
<td align="left">[<strong>Windows.Media.Effects</strong>] (https://msdn.microsoft.com/library/windows/apps/dn278802) 命名空间提供了简单直观的框架，可用于向音频和视频流添加特效。 该框架包含你可以实现的基本接口，用于创建自定义音频和视频特效，并将它们插入媒体管道中。</td>
</tr>
</tbody>
</table>

 

## 网络


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">套接字</td>
<td align="left"><p>套接字更新包括：</p>
<ul>
<li><strong>套接字代理。</strong> 套接字代理可以在应用生命周期的任何状态下，代表应用建立和关闭套接字连接。 这使得应用及其提供的服务更容易被发现。 例如，通过套接字代理，Win32 服务即使不在运行中，也仍然可以接受传入的套接字连接。</li>
<li><strong>吞吐量改进。</strong> 已针对使用 [<strong>Windows.Networking.Sockets</strong>](https://msdn.microsoft.com/library/windows/apps/br226960) 命名空间的应用优化了套接字吞吐量。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">后台传输后处理任务</td>
<td align="left">[<strong>Windows.Networking.BackgroundTransfer</strong>](https://msdn.microsoft.com/library/windows/apps/br207242) 命名空间中的新 API 允许你注册后处理任务组。 无论后台传输成功还是失败，应用都可以立即采取措施（即使应用不在前台），而不是等待用户下一次恢复该应用。</td>
</tr>
<tr class="odd">
<td align="left">对广告的蓝牙支持</td>
<td align="left">借助 [<strong>Windows.Devices.Bluetooth.Advertisement</strong>](https://msdn.microsoft.com/library/windows/apps/dn894325) 命名空间，应用可以发送、接收和筛选蓝牙 LE 广告。</td>
</tr>
<tr class="even">
<td align="left">WLAN Direct API 更新</td>
<td align="left"><p>设备代理已更新，目的是在不离开应用的情况下启用与设备配对。 [<strong>Windows.Devices.WiFiDirect</strong>] (https://msdn.microsoft.com/library/windows/apps/dn297687) 命名空间中的添加项还可以使设备能够被其他设备发现，并允许它侦听传入的连接通知。</p>
<div class="alert">
<strong>注意</strong> 此版本中的 WLAN Direct 功能改进并未内置于用户体验中，并且它们仅支持一键配对。 另外，此版本仅支持一个活动连接。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left">JSON 支持改进</td>
<td align="left">[<strong>Windows.Data.Json</strong>](https://msdn.microsoft.com/library/windows/apps/br240639) 命名空间现在可以在调试会话期间转换 JSON 对象时更好地支持现有的标准定义和开发人员体验。</td>
</tr>
</tbody>
</table>

 

## 安全


|                             |                                                                      |
|-----------------------------|----------------------------------------------------------------------|
| ECC 加密              | [
            **Windows.Security.Cryptography**](https://msdn.microsoft.com/library/windows/apps/br241404) 命名空间中的新 API 提供对椭圆曲线加密法的支持，该加密法是一种基于有限字段上的椭圆曲线的公钥加密实现。 ECC 在算术方面比 RSA 更为复杂，但提供了较小的密钥大小、降低了内存消耗，从而提升了性能。 它为 Microsoft 服务和客户提供了 RSA 密钥和 NIST 批准曲线参数的替代方法。 |
| Microsoft Passport          | Microsoft Passport 是身份验证的一种替代方法，使用非对称加密和手势来替换密码。 [
            **Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089) 命名空间中的类（如 [**KeyCredentialManager**](https://msdn.microsoft.com/library/windows/apps/dn973043)），使开发人员可以轻松使用 Microsoft Passport 而非利用加密或生物识别的复杂性来创建应用程序。  |
| Microsoft Passport for Work | Microsoft Passport for Work 是使用 Azure Active Directory 帐户登录 Windows 的替代方法，不需要使用密码、智能卡和虚拟智能卡。 你可以选择是禁用还是启用此策略设置。 |
| 令牌代理                | 令牌代理是一种新的身份验证框架，使应用更易于连接到联机标识提供者（如 Facebook）。 帐户用户名和密码管理等功能和简化的 UI 为用户极大改善了身份验证体验。 |

 

## 系统服务


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">电源</td>
<td align="left"><p>当启用或未启用节点模式时，你的 Windows 桌面应用程序现在都可以收到通知。 通过响应电源条件更改，你的应用程序可以帮助延长电池使用时间。</p>
<p>[<strong>GUID_POWER_SAVING_STATUS</strong>](https://msdn.microsoft.com/library/windows/desktop/hh448380)：将这一新的 GUID 与 [<strong>PowerSettingRegisterNotification</strong>](https://msdn.microsoft.com/library/windows/desktop/hh769082) 函数结合使用，以便在使用或未使用节电模式时都收到通知。</p>
<p>[<strong>SYSTEM_POWER_STATUS</strong>](https://msdn.microsoft.com/library/windows/desktop/aa373232)：已更新此结构以支持节电模式。 第四个成员 <strong>SystemStatusFlag</strong>（以前名为 Reserved1），现在用于指示节电模式是否已启用。 使用 [<strong>GetSystemPowerStatus</strong>](https://msdn.microsoft.com/library/windows/desktop/aa372693) 函数以检索此结构的指针。</p></td>
</tr>
<tr class="even">
<td align="left">Version</td>
<td align="left"><p>你可以使用 [Version 帮助程序函数](https://msdn.microsoft.com/library/windows/desktop/dn424972)确定操作系统的版本。 对于 Windows 10，这些帮助程序函数都包含一个新函数，即 [<strong>IsWindows10OrGreater</strong>] (https://msdn.microsoft.com/library/windows/desktop/dn905474)。 如果你想要确定系统版本，应使用这些帮助程序函数，而非已弃用的 [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451) 和 [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439) 函数。 有关如何获取系统版本的详细信息，请参阅[获取系统版本](https://msdn.microsoft.com/library/windows/desktop/ms724429)。</p>
<p>如果你确实要使用已弃用的 [<strong>GetVersionEx</strong>] (https://msdn.microsoft.com/library/windows/desktop/ms724451) 或 [<strong>GetVersion</strong>] (https://msdn.microsoft.com/library/windows/desktop/ms724439) 函数来获取 [<strong>OSVERSIONINFOEX</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724833) 或 [<strong>OSVERSIONINFO</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724834) 结构中的版本信息，请注意，这些结构包含的版本号已从针对 Windows 8.1 和 Windows Server 2012 R2 的 6.3 上升到了针对 Windows 10 的 10.0。 有关操作系统版本号的详细信息，请参阅[操作系统版本](https://msdn.microsoft.com/library/windows/desktop/ms724832)。</p>
<p>你还需要在应用程序中专门针对 Windows 8.1 或 Windows 10，使用 [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451) 或 [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439) 函数来获取这些版本的正确版本信息。 有关如何针对这些版本的 Windows 定向你的应用程序的信息，请参阅[针对 Windows 定向你的应用程序](https://msdn.microsoft.com/library/windows/desktop/dn481241)。</p></td>
</tr>
<tr class="odd">
<td align="left">用户信息</td>
<td align="left">[<strong>Windows.System</strong>](https://msdn.microsoft.com/library/windows/apps/br241814) 命名空间中的新 API 使你能够轻松访问用户的相关信息（如用户名和用户头像）。 它还提供了响应用户事件（如登录和注销）的功能。</td>
</tr>
<tr class="even">
<td align="left">内存管理和分析</td>
<td align="left">对 [<strong>Windows.System</strong>](https://msdn.microsoft.com/library/windows/apps/br241814) 中内存分析 API 的支持已扩展到所有平台，并且它们的整体功能因为新的类和函数而得到了增强。</td>
</tr>
</tbody>
</table>

 

## 存储


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">适用于 Windows Phone 的文件搜索 API</td>
<td align="left"><p>作为应用发布者，你可以通过向应用清单添加扩展来注册应用以便与你发布的其他应用共享存储文件夹。 然后，调用 [<strong>Windows.Storage.ApplicationData.GetPublisherCacheFolder</strong>] (https://msdn.microsoft.com/library/windows/apps/dn889607) 方法获取共享的存储位置。</p>
<p>Windows 运行时应用强大的安全模型通常会防止应用在自身之间共享数据。 但是，它可以帮助来自同一发布者的应用共享基于每个用户的文件和设置。</p></td>
</tr>
</tbody>
</table>

 

## 工具


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Visual Studio 中的动态可视化树</td>
<td align="left">Visual Studio 具有新的动态可视化树功能。 你可以在调试时使用该功能来快速了解应用的可视化树状态，并了解元素属性的设置方式。 该功能还允许你在应用运行时更改属性值，这样你无需重新启动即可进行调整和实验。</td>
</tr>
<tr class="even">
<td align="left">跟踪日志记录</td>
<td align="left"><p>[跟踪日志记录](https://msdn.microsoft.com/library/windows/desktop/dn904636)是适用于用户模式应用和内核模式驱动程序的全新事件跟踪 API；它基于 [Windows 事件跟踪](https://msdn.microsoft.com/library/windows/desktop/bb968803) (ETW) 生成。 此 API 提供了一种简化方式来检测代码和在结构数据中包括事件，而无需要求单独的检测清单 XML 文件。</p>
<p>WinRT、.NET 和 C/C++ TraceLogging API 都可服务于不同的开发人员受众。</p></td>
</tr>
</tbody>
</table>

 

## 用户体验


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">语音识别</td>
<td align="left">通用 Windows 平台现在支持针对长篇听写方案的连续语音识别。 了解如何在语音交互文档中启用连续听写。</td>
</tr>
<tr class="even">
<td align="left">不同应用程序平台之间的拖放功能</td>
<td align="left"><p>新的 [<strong>Windows.ApplicationModel.DataTransfer.DragDrop</strong>](https://msdn.microsoft.com/library/windows/apps/dn894216) 命名空间可将拖放功能移植到通用 Windows 应用。 以前，桌面程序的常见拖放方案（例如，将文档从文件夹拖动到 Outlook 电子邮件中以作为附件）无法在通用 Windows 应用中实现。 使用这些新的 API，你的应用可以让用户轻松地在不同的通用 Windows 应用和桌面之间移动数据。</p>
<p>为了支持应用之间的拖放，这些新的 API 已添加到XAML：</p>
<ul>
<li>[<strong>ListViewBase.DragItemsCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn831959)</li>
<li>[<strong>UIElement</strong>](https://msdn.microsoft.com/library/windows/apps/br208911)：[<strong>CanDrag</strong>](https://msdn.microsoft.com/library/windows/apps/dn903558)、[<strong>DragStarting</strong>](https://msdn.microsoft.com/library/windows/apps/dn903560)、[<strong>StartDragAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn903562)、[<strong>DropCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn903561)</li>
<li>[<strong>DragOperationDeferral</strong>](https://msdn.microsoft.com/library/windows/apps/dn831917)、[<strong>DragUI</strong>](https://msdn.microsoft.com/library/windows/apps/dn985714)、[<strong>DragUIOverride</strong>](https://msdn.microsoft.com/library/windows/apps/dn985715)</li>
<li>[<strong>DragEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/br242372)：[<strong>AcceptedOperation</strong>](https://msdn.microsoft.com/library/windows/apps/dn831912)、[<strong>DataView</strong>](https://msdn.microsoft.com/library/windows/apps/dn831913)、[<strong>DragUIOverride</strong>](https://msdn.microsoft.com/library/windows/apps/dn985710)、[<strong>GetDeferral</strong>](https://msdn.microsoft.com/library/windows/apps/dn831914)、[<strong>Modifiers</strong>](https://msdn.microsoft.com/library/windows/apps/dn831915)</li>
<li>[<strong>DragItemsCompletedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn831953)、[<strong>DropCompletedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn903549)、[<strong>DragStartingEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn903540)</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">自定义窗口标题栏</td>
<td align="left">对于适用于桌面设备系列的 UWP 应用，你现在可以结合使用 [<strong>ApplicationViewTitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn906115) 类与 the [<strong>ApplicationView.TitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn906131) 属性以及 [<strong>Window.SetTitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn965560) 方法，将默认 Windows 标题栏内容替换为自己自定义的 XAML 内容。 你的 XAML 将被视为&quot;系统镶边&quot;，因此 Windows（而不是你的应用）将处理输入事件。 这意味着用户甚至可以在单击你的自定义标题栏内容时拖动窗口和调整窗口的大小。</td>
</tr>
</tbody>
</table>

 

## Web


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Internet Explorer</td>
<td align="left"><p>Internet Explorer 中引入了边缘模式，这是一种新的&quot;动态&quot;文档模式，旨在实现与其他现代浏览器和现代 Web 内容的最大互操作性。 此实验模式正逐步向随机选择的一组 Windows 10 用户推行。 你可以通过新的 IE <strong>about:flags</strong> 机制手动启用或禁用 Edge 模式。 有关详细信息，请参阅：</p>
<ul>
<li>[边缘模式实际应用 - 下一步是帮助 Web 正常工作](http://blogs.msdn.com/b/ie/archive/2014/11/11/living-on-the-edge-our-next-step-in-interoperability.aspx)。</li>
<li>[Internet Explorer for Windows 10 开发人员指南](https://dev.windows.com/microsoft-edge/)。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">WebView Edge 模式浏览</td>
<td align="left">[<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) 控件使用与新 Edge 浏览器相同的呈现引擎。 这将提供最准确、最符合标准的 HTML 呈现模式。</td>
</tr>
<tr class="odd">
<td align="left">线程外 WebView</td>
<td align="left">你可以指定 [<strong>WebView.ExecutionMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn932034) 以支持对单独后台线程上的 Web 内容进行处理和显示。 在某些特定情况下，这可以提高性能。</td>
</tr>
<tr class="even">
<td align="left">WebView.UnsupportedUriSchemeIdentified 事件</td>
<td align="left"><p>新的 [<strong>WebView.UnsupportedUriSchemeIdentified</strong>] (https://msdn.microsoft.com/library/windows/apps/dn974400) 事件使你可以确定应用应当以何种方式处理不受支持的 URI 方案。 你可以处理此事件以使你的应用能够针对不受支持的 URI 方案提供自定义处理。</p>
<p>有关 HTML WebView 控件，请参阅 [<strong>MSWebViewUnsupportedUriSchemeIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn803906.aspx) 事件。</p></td>
</tr>
<tr class="odd">
<td align="left">WebView.NewWindowRequested 事件</td>
<td align="left"><p>新的 [<strong>WebView.NewWindowRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn974397) 事件使你可以在 WebView 中的脚本请求新的浏览器窗口时进行响应。</p>
<p>有关 HTML WebView 控件，请参阅 [<strong>MSWebViewNewWindowRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn806030) 事件。</p></td>
</tr>
<tr class="even">
<td align="left">WebView.PermissionRequested 事件</td>
<td align="left"><p>新的 [<strong>WebView.PermissionRequested</strong>] (https://msdn.microsoft.com/library/windows/apps/dn974398) 事件使 WebView 内容可以利用丰富的新 HTML5 API（需向用户请求特殊权限），如地理位置。</p>
<p>有关 HTML WebView 控件，请参阅 [<strong>MSWebViewPermissionRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn806030.aspx) 事件。</p></td>
</tr>
<tr class="odd">
<td align="left">WebView.UnviewableContentIdentified 事件</td>
<td align="left"><p>新的 [<strong>WebView.UnviewableContentIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn299351) 事件使你可以在 [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) 导航到非 Web 内容（如 PDF 文件或 Office 文档）时进行响应。</p>
<p>有关 HTML WebView 控件，请参阅 [<strong>MSWebViewUnviewableContentIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn609716) 事件。</p></td>
</tr>
<tr class="even">
<td align="left">WebView.AddWebAllowedObject 方法</td>
<td align="left"><p>你可以调用新的 [<strong>WebView.AddWebAllowedObject</strong>] (https://msdn.microsoft.com/library/windows/apps/dn903993) 方法将 WinRT 对象插入到 XAML [<strong>WebView</strong>] (https://msdn.microsoft.com/library/windows/apps/br227702) 中，然后从受信任的 JavaScript（在该 <strong>WebView</strong> 中托管）调用该对象的函数。 例如，Web 内容可以通过请求其父应用调用 [<strong>ToastNotificationManager</strong>](https://msdn.microsoft.com/library/windows/apps/br208642) WinRT API 来显示系统通知。</p>
<p>有关 HTML [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) 控件，请参阅 [<strong>addWebAllowedObject</strong>](https://msdn.microsoft.com/library/windows/apps/dn926632) 方法。</p></td>
</tr>
<tr class="odd">
<td align="left">WebView.ClearTemporaryWebDataAync 方法</td>
<td align="left">当用户与 XAML [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) 内的 Web 内容交互时，<strong>WebView</strong> 控件将缓存基于该用户会话的数据。 你可以调用新的 [<strong>ClearTemporaryWebDataAsync</strong>] (https://msdn.microsoft.com/library/windows/apps/dn974394) 方法来清除此缓存。 例如，你可以在一位用户退出应用后清除缓存，这样其他用户就无法访问前一会话的任何数据。</td>
</tr>
</tbody>
</table>



<!--HONumber=May16_HO2-->


