---
ms.assetid: A37ADD4A-2187-4767-9C7D-EDE8A90AA215
规划性能
用户希望他们的应用保持响应性、感觉自然，并且不会耗尽电池。
---
# 规划性能

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


用户希望他们的应用保持响应性、感觉自然，并且不会耗尽电池。 从技术上讲，性能是非功能要求，但将性能视为一项功能将有助于你满足用户的期望。 指定目标与衡量是关键因素。 确定性能关键型方案是什么；定义良好的性能意味着什么。 然后及早衡量，并在项目的整个生命周期中频繁衡量，以确保达到你的目标。

## 指定目标

用户体验是定义良好性能的一个基本方法。 应用的启动时间可能会影响用户对应用性能的感知。 用户可能会认为应用启动时间少于 1 秒为优秀，少于 5 秒为良好，而超过 5 秒则为较差。

其他指标对用户体验的影响并不明显，例如内存。 应用在暂停或不活动时被终止的几率将会随着活动应用所使用的内存量上升。 较高的内存使用量会降低系统上所有应用的体验是一种普遍的规则，因此对内存消耗制定目标合乎情理。 请考虑用户将认为你的应用大小约为多大：较小、中等还是较大。 对性能的期望将与这种感知相关联。 例如，你可能希望不使用大量媒体的小型应用占用少于 100MB 的内存。

最好设置初始目标，后续再加以修订，而不要根本就没有目标。 应用的性能目标应为具体且可衡量的，并且应分为以下三类：用户或应用完成任务需要花费多长时间（时间）；应用本身为响应用户交互进行重绘的速率和连续性（流畅性）；应用能够在多大程度上节约系统资源，包括电池电源（效率）。

## 时间

考虑用户若要在应用中完成其任务，（*交互类*）所用时间的可接受范围。 为每个交互类分配一个标签、感知的用户情绪以及理想和最长持续时间。 下面是一些建议。

| 交互类标签 | 用户感知                 | 理想时间            | 最长时间          | 示例                                                                     |
|-------------------------|---------------------------------|------------------|------------------|------------------------------------------------------------------------------|
| 迅速                    | 可察觉的最低延迟      | 100 毫秒 | 200 毫秒 | 显示应用栏；按一个按钮（第一个响应）                        |
| 一般                 | 快，但不够迅速             | 300 毫秒 | 500 毫秒 | 调整大小；语义缩放                                                        |
| 响应              | 不够快，但是感觉到响应 | 500 毫秒 | 1 秒         | 导航到不同页面；从已暂停状态恢复应用          |
| 启动                  | 有竞争优势的体验          | 1 秒         | 3 秒        | 首次启动应用，或者在应用先前已终止之后 |
| 连续的              | 不再感觉到有响应      | 500 毫秒 | 5 秒        | 从 Internet 下载文件                                            |
| 卡住                 | 时间长；用户可能切换    | 500 毫秒 | 10 秒       | 从应用商店安装多个应用                                         |

 

你现在可以向应用的性能方案分配交互类。 你可以向每种方案分配应用的时间点引用、部分用户体验以及一个交互类。 以下是针对餐饮应用示例的一些建议。


<!-- DHALE: used HTML table here b/c WDCML src used rowspans -->
<table>
<tr><th>方案</th><th>时间点</th><th>用户体验</th><th>交互类</th></tr>
<tr><td rowspan="3">导航到食谱页面 </td><td>第一个响应</td><td>页面过渡动画启动</td><td>快（100 至 200 毫秒）</td></tr>
<tr><td>响应</td><td>加载配方列表；无图像</td><td>响应（500 毫秒至 1 秒）</td></tr>
<tr><td>可视部分完成</td><td>加载所有内容；显示图像</td><td>连续（500 毫秒至 5 秒）</td></tr>
<tr><td rowspan="2">搜索食谱</td><td>第一个响应</td><td>单击搜索菜单</td><td>快（100 至 200 毫秒）</td></tr>
<tr><td>可视部分完成</td><td>显示本地食谱标题的列表</td><td>一般（300 至 500 毫秒）</td></tr>
</table>

如果要显示实时内容，则还需考虑内容新鲜程度的目标。 目标是否为每隔几秒便刷新内容？ 或者说，每隔几分钟、每隔几小时甚至每天刷新一次内容是否是可接受的用户体验？

指定目标后，现在可以更好地测试、分析并优化应用。

## 流畅性应用的具体可衡量的流畅性目标可能包括：无任何屏幕重绘停止后又启动（故障）。
- 动画以 60 帧/秒 (FPS) 呈现。
- 当用户平移/滚动时，应用每秒显示 3 到 6 页的内容。

## 效率应用的具体可衡量的效率目标可能包括：对于应用进程，CPU 百分比始终等于或低于 *N*，而内存使用量（以 MB 为单位）始终等于或低于 *M*。
- 当应用处于非活动状态时，对于应用进程而言，*N* 和 *M* 均为零。
- 在使用电池电源时，应用可以有效使用 *X* 小时；当应用处于非活动状态时，设备将保留 *Y* 小时的电量。

## 针对性能设计应用：你现在可以使用性能目标来影响你的应用设计。 以餐饮应用为例，在用户导航到食谱页面后，你可以选择 [update items incrementally](optimize-gridview-and-listview.md#update-items-incrementally)，以便首先呈现食谱名称、延后显示原料，然后再进一步延后显示图像。 这样在平移/滚动时可保持响应速度和流畅的 UI，在交互后高保真的呈现速度将会减慢以使 UI 线程跟上步伐。 下面是要考虑的某些其他方面。

**UI** 

- 通过 [optimizing your XAML markup](optimize-xaml-loading.md) 最大程度地为应用每个页面（尤其初始页面）的 UI 提高分析和加载时间效率以及内存效率。 简而言之，在需要 UI 和代码之前先将其延迟加载。
- 对于 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 和 [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705)，使所有项目都保持相同大小并尽可能多地使用 [ListView and GridView optimization techniques](optimize-gridview-and-listview.md)。
- 采用框架可以在区块中加载并重用的标记形式声明 UI，而不是在代码中以命令方式构建。
- 在用户需要 UI 元素之前先将其折叠。 请参阅 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) 属性。
- 首选主题过渡和动画而不是情节提要动画。 有关详细信息，请参阅 [Animations overview](https://msdn.microsoft.com/library/windows/apps/Mt187350)。 请记住，情节提要动画需要持续更新屏幕，并保持 CPU 和图形管道处于活动状态。 若要维持电池电量，当用户未与应用交互时，请不要运行动画。
- 你加载的图像的大小应适合将使用 [**GetThumbnailAsync**](https://msdn.microsoft.com/library/windows/apps/BR227210) 方法显示它的视图。

**CPU、内存和电源** 

- 将优先级较低的工作安排在优先级较低的线程和/或内核上运行。 请参阅 [Asynchronous programming](https://msdn.microsoft.com/library/windows/apps/Mt187335)、[**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR209054) 属性和 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211) 类。
- 通过在应用暂停时释放开销较大的资源（例如媒体）来最大程度地减少应用的内存占用量。
- 最大程度地减少代码的工作集。
- 通过尽可能取消注册事件处理程序以及取消引用 UI 元素来避免内存泄露。
- 出于对电池的考虑，请谨慎处理轮询数据、查询传感器的频率，或在 CPU 空闲时对其安排工作。

**数据访问** 

- 预取内容（如果可能）。 有关自动预取，请参阅 [**ContentPrefetcher**](https://msdn.microsoft.com/library/windows/apps/Dn279042) 类。 有关手动预取，请参阅 [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/BR224847) 命名空间和 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/Hh700517) 类。
- 缓存访问开销较大的内容（如果可能）。 请参阅 [**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/BR241621) 和 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/BR241622) 属性。
- 对于缓存失误，尽快显示占位符 UI 以指示应用仍在加载内容。 以一种不会让用户感到不协调的方式从占位符过渡到实时内容。 例如，在应用加载实时内容时，不要更改用户手指下或鼠标指针下的内容的位置。

**应用启动和恢复** 

- 延迟应用的初始屏幕，除非有必要，否则不要延长应用的初始屏幕。 有关详细信息，请参阅 [Creating a fast and fluid app launch experience](http://go.microsoft.com/fwlink/p/?LinkId=317595) 和 [Display a splash screen for more time](https://msdn.microsoft.com/library/windows/apps/Mt187309)。
- 禁用在欢迎屏幕消除后立即发生的动画，因为这些动画只会带来应用启动时间延迟的感觉。

**自适应 UI 和方向** 

- 使用 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021) 类。
- 仅完成马上需要的工作，将密集的应用工作延迟到以后（在用户看到应用 UI 的裁剪状态之前，应用有 200 到 800 毫秒的时间完成工作）。

准备好性能相关的设计后，你便可以开始为应用编写代码。

## 性能检测：在编写代码时，添加用于在应用运行的某些时刻记录消息和事件的代码。 之后，在测试应用时，你可以使用 Windows Performance Recorder 和 Windows Performance Analyzer（均包含在 [Windows Performance Toolkit](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh162945.aspx) 中）等分析工具，以创建和查看有关你的应用性能的报告。 在此报告中，可以查找这些消息和事件以帮助你更加轻松地分析报告的结果。

通用 Windows 平台 (UWP) 提供了日志记录 API，在 [Event Tracing for Windows (ETW)](https://msdn.microsoft.com/library/windows/desktop/Bb968803) 的支持下，它们共同提供丰富的事件日志和跟踪解决方案。 这些 API（属于 [**Windows.Foundation.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/BR206677) 命名空间）包括 [**FileLoggingSession**](https://msdn.microsoft.com/library/windows/apps/Dn264138)、[**LoggingActivity**](https://msdn.microsoft.com/library/windows/apps/Dn264195)、[**LoggingChannel**](https://msdn.microsoft.com/library/windows/apps/Dn264202) 和 [**LoggingSession**](https://msdn.microsoft.com/library/windows/apps/Dn264217) 类。

若要在应用运行到某个特定时刻时在报告中记录一条消息，请创建一个 **LoggingChannel** 对象，然后调用此对象的 [**LogMessage**](https://msdn.microsoft.com/library/windows/apps/Dn264202-logmessage) 方法，如下所示。

'```csharp // using Windows.Foundation.Diagnostics; // ... LoggingChannel myLoggingChannel = new LoggingChannel(&quot;MyLoggingChannel&quot;); myLoggingChannel.LogMessage(LoggingLevel.Information, &quot;Here' s my logged message.&quot;); // ... ``` 若要在报告中记录应用运行期间内的启动和停止事件，请创建一个 **LoggingActivity** 对象，然后调用此对象的 [**LoggingActivity**](https://msdn.microsoft.com/library/windows/apps/Dn264195-loggingactivity) 构造函数，如下所示。

```csharp // using Windows.Foundation.Diagnostics; // ... LoggingActivity myLoggingActivity; // myLoggingChannel 已在上面的代码示例中经过定义和初始化。
using (myLoggingActivity = new LoggingActivity(&quot;MyLoggingActivity&quot;), myLoggingChannel)) { // 在此日志记录活动启动后，将记录启动事件。
    
    // 在此处添加代码，执行一些有趣的操作。
    
} // 此日志记录活动结束后，将记录结束事件。

// ... ``` 另请参阅 [Logging sample](http://go.microsoft.com/fwlink/p/?LinkId=529576)。

检测应用之后，你可以测试和衡量应用的性能。

## 针对性能目标进行测试和衡量：你的性能计划的一部分是在开发过程中定义要进行性能衡量的点。 这些目标将会不同，具体取决于是在原型设计期间、开发期间还是在部署期间进行衡量。 在原型设计的早期阶段中衡量性能会非常有价值，因此我们建议你在编写完具有工作意义的代码后便立即衡量性能。 早期衡量可以让你很好地了解在你的应用中重要的开销在哪些位置，并可为设计决策提供信息。 这会产生扩展性良好的高性能应用。 通常，较早更改设计的成本与较晚时候相比成本更低。 在产品周期的后期才测量性能会导致在最后关头做出匆忙修改而因此带来较差的性能。

使用以下技术和工具测试你的应用与原始性能目标相比如何。

- 针对各种不同硬件配置进行测试，包括一体机和台式电脑、笔记本电脑、超极本和平板电脑以及其他移动设备。
- 针对各种不同屏幕大小进行测试。 尽管更大的屏幕大小可以显示更多的内容，但呈现所有额外内容可能会对性能产生负面影响。
- 尽量消除测试因素。
    - 在测试设备上关闭后台应用。 若要执行此操作，请从 Windows 的“开始”菜单中依次选择**“设置”**>**“个性化”**>**“锁屏界面”**。 选择每个活动应用，然后选择**“无”**。
    - 在将应用部署到测试设备之前，通过使用发布配置生成应用将其编译为本机代码。
    - 若要确保自动维护不会影响测试设备的性能，请手动将其触发并等待完成。 在 Windows 的“开始”菜单中，搜索**“安全和维护”**。 在**“维护”**区域的**“自动维护”**下，选择**“开始维护”**并等待状态从**“正在进行维护”**发生变化。
    - 多次运行应用有助于消除随机测试变量，并且有助于确保一致的测量结果。
- 针对降低的功能可用性进行测试。 用户设备的功率可能明显低于你的开发计算机。 Windows 设计时考虑到了低功率设备，例如移动电脑。 在平台上运行的应用应确保在这些设备上也可以良好地执行。 提示：预期低功率设备的运行速度大约是台式机的四分之一，请相应地设置你的目标。
- 使用 Microsoft Visual Studio 和 Windows Performance Analyzer 等工具的组合衡量应用性能。 Visual Studio 可以提供侧重于应用的分析，如源代码链接。 Windows Performance Analyzer 可以提供侧重于系统的分析，如提供系统信息、关于触摸操作事件以及关于磁盘输入/输出 (I/O) 和图形处理单元 (GPU) 开销的信息。 这两个工具都会跟踪捕获和导出，并且都可以重新打开共享跟踪和事后跟踪。
- 在将应用提交到应用商店以进行认证之前，请确保将测试计划合并到性能相关测试案例中，如 [Windows App Certification Kit tests](windows-app-certification-kit-tests.md) 的“性能测试”部分以及 [Windows Store app test cases](https://msdn.microsoft.com/library/windows/apps/Dn275879) 的“性能和稳定性”部分中所述。

有关详细信息，请参阅以下资源和分析工具。

-   [
            Windows Performance Analyzer](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh448170.aspx) 
-   [Windows Performance Toolkit](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh162945.aspx) 
-   [Analyze performance using Visual Studio diagnostic tools](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh696636.aspx) 
- //build/ 会话 [XAML Performance](https://channel9.msdn.com/Events/Build/2015/3-698)
- //build/ 会话 [New XAML Tools in Visual Studio 2015](https://channel9.msdn.com/Events/Build/2015/2-697)

## 响应性能测试结果：在分析性能测试结果后，确定是否需要进行任何更改，例如：是否应更改任何应用设计决策，或优化代码？
- 是否应在代码中添加、删除或更改任何检测？
- 是否应修改任何性能目标？

如果需要任何更改，请进行更改，然后重新检测或测试并重复。

## 优化：仅优化代码中性能关键型代码路径，即花费时间最多的地方。 分析将会告知你是哪些部分。 通常，在创建遵循良好设计实践的软件与编写以最高优化程度执行的代码之间，存在一个权衡取舍。 通常，在不关注性能的方面，优先考虑开发人员生产效率和良好的软件设计会更好。



<!--HONumber=Mar16_HO1-->


