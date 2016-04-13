---
Description: 进度控件将为用户提供关于正在处理运行时间较长的操作的反馈。
title: 进度控件指南
ms.assetid: FD53B716-C43D-408D-8B07-522BC1F3DF9D
label: 进度控件
template: detail.hbs
---
# 进度控件

进度控件将为用户提供关于正在处理运行时间较长的操作的反馈。 *确定*进度栏可显示操作已完成部分的百分比。 *不确定*进度栏（或进度环）可显示正在进行操作。

进度控件为只读；它不可交互。

<span class="sidebar_heading" style="font-weight: bold;">重要的 API</span>

-   [**ProgressBar 类**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.aspx)
-   [**IsIndeterminate 属性**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.isindeterminate.aspx)
-   [**ProgressRing 类**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.aspx)
-   [**IsActive 属性**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.isactive.aspx)

![Windows 应用：不确定进度栏、进度环以及确定进度栏](images/ProgressBar.png)

Windows 应用：不确定进度栏、进度环以及确定进度栏

![Windows Phone 应用：状态栏进度指示器和进度栏](images/wp_progress_bar.png)

Windows Phone 应用：状态栏进度指示器和进度栏

## 示例

下面是初始屏幕上的进度环控件的示例。

![说明标准进度环控件的屏幕截图](images/ProgressBar_Standard.png)

进度栏同样是状态或位置的良好指示器。 用于音乐跟踪的进度栏将对应歌曲的时间线：该栏的值是歌曲的播放位置；暂停状态表示播放已暂停。

![Xbox Music 应用在播放歌曲时显示进度栏](images/ProgressBar_MusicTimeline.png)

## 这是正确的控件吗？

比不是始终需要显示进度控件。 有时候，任务的进度就已经足以表明完成状态，或者任务完成得太快，导致显示进度条可能会引起误解。 下面是确定是否显示进度控件时需要考虑的一些注意事项。

-   **操作是否需要两秒以上才能完成？**

    如果是，请在操作开始后立即显示一个进度控件。 如果操作大多数情况下都需要两秒以上才能完成，有时不到两秒就能完成，请在显示控件之前等待 500ms，以避免闪烁。

-   **操作要等待用户完成某个任务吗？**

    如果是这样，请不要使用进度条。 进度条是用于显示电脑进度的，不用于显示用户进度。

-   **用户需要知道发生了什么情况吗？**

    例如，如果应用正在后台进行下载，但是该下载不是由用户启动的，则用户不需要知道有关该下载的信息。

-   **操作是否属于不会阻止用户活动且用户基本不感兴趣（稍有有趣）的背景活动？**

    当应用执行的任务不必始终可见，但仍然需要显示状态时，请使用文本和省略号。

    ![将文本作为进度指示器的示例](images/textprogress.png)

    使用省略号指示任务正在进行。 如果有多个任务或项目，可以指示其余任务的数量。 当所有任务完成时，取消该指示器。

-   **是否可以使用操作中的内容可视化进度？**

    如果是这样，请不要显示进度控件。 例如，在显示从磁盘加载的 /src/assets 时，/src/assets 在加载后逐个显示在屏幕上。 显示进度控件可能只会导致 UI 混乱，毫无益处。

-   **在处理操作时，你是否可以比较确定已完成多少百分比的总工作量？**

    如果可以，请使用确定进度栏，尤其是对于会阻止用户的操作。 否则，使用不确定进度栏或进度环。 即使用户只知道正在发生某件事，这仍会有帮助。

## 创建确定进度控件

确定进度栏显示应用执行的进度状况。 随着工作进行，进度条逐渐填满。 如果你可以估计剩余工作量的时间、字节数、文件数或其他某些可量化的衡量单位，请使用确定进度栏。

进度栏提供多个用来设置和确定进度的属性：
- [
            **IsIndeterminate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.isindeterminate.aspx)：指定进度栏是否不确定。 设置为 **false** 可创建确定的进度栏。
- [
            **Minimum**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.rangebase.minimum.aspx)：值范围的开始值。 默认值为 0.0。
- [
            **Maximum**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.rangebase.maximum.aspx)：值范围的结束值。 默认值为 1.0。 
- [
            **Value**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.rangebase.value.aspx)：指定当前进度的数字。 如果要显示文件下载进度，此值可以是已经下载的字节数（你随后可以将 Maximum 设置为要下载的字节总数）。
 
以下示例展示了一个基于值的确定进度栏。 

```xaml
<ProgressBar IsIndeterminate="False" Maximum="100" Width="200"/>
```

```csharp
ProgressBar progressBar1 = new ProgressBar();
progressBar1.IsIndeterminate = false;
progressBar1.Maximum = 100;
progressBar1.Width = 200;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(progressBar1);
```

通常，不要在标记中指定进度栏的值。 不过你可以使用过程代码和数据绑定来更新进度栏的值，以作为某些进度指示器的响应。 例如，如果你的进度栏指示已下载的文件数量，那么每次下载完一个文件就更新此值。

## 创建不确定进度控件

如果无法估计完成任务还剩余的工作量，而且任务不阻止用户交互，使用不确定进度栏或进度环。 不确定进度栏不显示随着进度的完成逐渐填满的进度栏，而是显示从左到右移动的点状动画。 不确定进度环以旋转的点状动画序列显示。 

若要使进度栏不确定，请将其 [**IsIndeterminate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.isindeterminate.aspx) 属性设置为 **true**。

```xaml
<ProgressBar IsIndeterminate="True" Width="200"/>
```

```csharp
ProgressBar progressBar1 = new ProgressBar();
progressBar1.IsIndeterminate = true;
progressBar1.Width = 200;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(progressBar1);
```

若要在应用中显示进度环，请将其 [**IsActive**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.isactive.aspx) 属性设置为 **true**。

```xaml
<ProgressRing IsActive="True"/>
```

```csharp
ProgressRing progressRing1 = new ProgressRing();
progressRing1.IsActive = true;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(progressRing1);
```

## 建议

-   当任务确定时（即任务有具有明确定义的持续时间或可预测的结束时间），使用确定进度栏。 例如，如果能以时间、字节数、文件数或其他某些可量化的测量单位估计其余的工作量，请使用确定进度栏。 下面是确定任务的一些示例：

    -   应用正在下载一张 500k 的照片，到现在为止收到了 100k。
    -   应用正在显示一个为时 15 秒的广告，已经过去了 2 秒。

    ![确定进度条示例](images/progress_determinate_bar.png)

-   为不确定的模式（阻止用户交互）任务使用不确定进度环。

    ![进度环示例](images/progress_ring.png)

-   为不确定的非模式（不阻止用户交互）任务使用不确定进度栏。

    ![不确定进度条示例](images/progress_indeterminate_bar.png)

-   如果模式状态不超过 2 秒，则将部分的模式任务视为非模式。 某些任务会阻止交互，直到某些进度完成后，用户才能开始重新与应用交互。 例如，当用户执行搜索查询时，将阻止交互，直到显示第一个结果为止。 将类似的任务视为非模式任务，如果模式状态持续的时间小于 2 秒，则使用不确定进度条。 如果模型状态可持续 2 秒钟以上，请为该任务的模型阶段使用不确定进度环，对非模型阶段使用不确定进度条。
-   请考虑提供一种取消或暂停正在进行的操作的方法，尤其是在用户必须等待操作完成才能继续工作并且知道还要运行多少百分比的操作的情况下。
-   不要使用“等待光标”指示活动，因为通过触屏与系统交互的用户不会看到光标，而使用鼠标的用户则不需要两种方法可视化活动（光标和进度控件）。
-   为多个活动的相关任务显示单个进度控件。 如果在屏幕上有多个同时执行某种活动的相关项目，不要显示多个进度控件。 相反，应显示一个进度控件，在最后一个任务完成后结束。 例如，如果应用下载多张照片，则显示单个进度控件，而不是为每一张照片显示一个。
-   在任务运行时不更改进度控件的位置或尺寸。

### 确定任务指南

-   如果操作属于模式操作（阻止用户交互），并且所需时间长于 10 秒，请提供取消该操作的方式。 在操作开始时，应该提供用于取消的选项。
-   空间进度均匀更新。 避免出现进度前进到 80% 以上后停止很长时间的情况。 你需要加速进度走向结束，而不是放慢速度。 避免剧烈跳动，例如从 0% 到 90%。
-   进度达到 100% 后，等待确定进度条完成动画，然后再隐藏进度条。
-   如果你的任务已停止（由用户或外部条件），但用户可以使它继续，请在视觉上指示已暂停进度。 在 JavaScript 应用中，你可使用 win-paused CSS 样式完成此操作。 在 C\#/C++/VB 应用中，你可以通过将 ShowPaused 属性设置为 true 来执行此操作。 在进度栏下提供状态文本以告知用户将发生的情况。
-   如果任务已停止且不能继续，或者必须从头开始，请在视觉上指示存在错误。 在 JavaScript 应用中，你可使用 win-error CSS 样式完成此操作。 在 C\#/C++/VB 应用中，你可以通过将 ShowError 属性设置为 true 来执行此操作。 使用消息替换状态文本（位于进度栏下方）以告知用户发生的情况以及如何修复该问题（如果可能）。
-   如果需要一定的时间（或操作）才能开始提供确定进度，请首先使用不确定进度栏，然后切换到确定进度栏。 例如，如果下载任务的第一步是连接到服务器，则无法估计需要多长时间。 在建立连接后，切换到确定进度条以显示下载进度。 在切换后，将进度条完全保持在同一个位置并显示相同的尺寸。

    ![将不确定进度条更改为确定进度条](images/progress_changing.png)

-   如果你有一个项目列表（例如打印机列表），并且某些操作可以对该列表中的项目启动某项操作（例如为其中一个打印机安装驱动程序），请在该项目旁显示确定进度栏。

    在进度条的上方显示任务的标题（标签），并在下方显示状态。 如果发生的情况很明显，则不要提供状态文本。 任务完成后，隐藏进度条。 使用状态文本传达项目的新状态。

    ![显示嵌入式进度和状态](images/progress_multiplebars.png)

-   若要显示一个任务列表，在网格中对齐内容，以便用户可以一眼看到状态。 为所有项目显示进度栏，即使是处于挂起状态的项目。

    由于此列表的用途是显示正在进行的操作，因此请在操作完成后从列表中删除这些操作。

    ![显示多个进度条](images/progress_bar_multiple.png)

-   如果用户从应用栏启动了一个阻止用户交互的任务，则在应用栏中显示进度控件。

    如果在进度条很容易看到进度所针对的对象，则可以将进度条与应用栏的顶部对齐，并省略标签和状态；否则，提供一个标签和状态文本。

    通过禁用应用栏中的控件并忽略内容区域中的输入，在任务期间禁用交互。

-   不要让进度递减。 始终使进度值递增。 如果需要反转某个操作，请显示反向进度，正如你显示任何其他操作的进度。
-   不要重新开始计算进度（从 100% 到 0%），除非用户可明显看出当前步骤或任务不是最后一个。 例如，假定任务具有两个部分：下载一些数据，然后处理和显示数据。 在下载完成后，将进度栏重置为 0%，然后开始显示数据处理进度。 如果用户不清楚任务中有多个步骤，请将这些任务折叠到单个 0-100% 范围内，并在从一个任务前进到下一个任务时更新状态文本。

### 使用进度环的模式不确定任务指南

-   在操作的上下文中显示进度环：在用户启动操作的位置或者将显示结果数据的位置附近显示进度环。
-   在进度环右侧提供状态文本。
-   为进度环及其状态文本设置相同的颜色。
-   禁用在任务正在运行时不应当与其交互的控件。
-   如果任务导致错误，请隐藏进度指示器和状态文本，并在其位置显示错误消息。
-   在对话框中，如果某个操作必须完成，然后你才能继续到下一个屏幕，请在按钮区域正上方放置进度环，使其与对话框的内容左对齐。

    ![对话框中的进度](images/prog_ring_dialog.png)

-   在具有左对齐控件的应用窗口中，将进度环放在导致该操作的控件左侧或正上方。 将进度环与相关内容左对齐。

    ![在具有右对齐控件的应用窗口中显示进度](images/prog_right_aligned_controls.png)

-   在具有左对齐控件的应用窗口中，将进度环放在导致该操作的控件右侧或正下方。

    ![具有左对齐控件的进度环](images/prog_left_aligned_1.png)

    ![左对齐控件下的进度环](images/prog_left_aligned_2.png)

-   如果你显示多个项目，将进度环和状态文本放在项目标题下方。 如果出错，则将进度环和状态替换为错误文本。

    ![多个项目列表中的进度环](images/prog_ring_multiple.png)

### 使用进度栏的非模式不确定任务指南

-   如果在浮出控件中显示进度，请将不确定进度栏放在浮出控件的顶部，并将其宽度设置为跨整个浮出控件。 这样放置可最大程度降低注意力的分散，但仍然可以通知正在进行的活动。 不要为弹出窗口提供标题，因为标题会阻止你将进度条放在弹出窗口的顶部。

    ![弹出窗口中的不确定进度条](images/prog_flyout_indeterminate_bar.png)

-   如果要在应用窗口中显示进度，请将不确定进度栏放在应用窗口的顶部，并使其跨整个窗口。

    ![位于应用窗口顶部的进度条](images/prog_indeterminate_bar_app_window.png)

### 状态文本指南

-   使用确定进度条时，不要在状态文本中显示进度比例。 控件已经提供该信息。
-   如果你使用文本（而不使用进度控件）来指示活动，请使用省略号指示该活动正在进行。
-   如果你使用进度控件，请不要在状态文本中使用省略号，因为进度控件始终指示操作正在进行。

### 外观和布局指南

-   确定进度栏显示为有颜色的栏，它会延长以充满灰色的背景栏。 有颜色部分的总长度百分比可指示操作完成的相对程度。
-   不确定进度栏或进度环由不断移动的带颜色的点构成。
-   根据其重要性选择进度控件的位置和醒目程度。

    重要的进度控件可用来唤起行动，告诉用户在系统完成工作后继续特定操作。 某些内置的 Windows Phone 应用在重要情况的屏幕顶部使用状态栏进度指示器。 你也可以这样做，并将其配置为确定或不确定。

    较不重要的情况（例如在下载期间）显示较小的进度控件，并且限制在一个视图中显示。

-   使用标签显示进度值、说明正在进行的流程，或者指示已中断操作。 标签可选用，但我们强烈推荐它。

    要说明正在进行的流程，请使用动名词 （带有“正在”的动词），例如，“正在连接”、“正在下载”或者“正在发送”。

    要指示已暂停进度或已发生异常，请使用过去分词，例如“已暂停”、“下载已失败”或“已取消”。

-   带标签和状态的确定进度条

    ![带标签和状态信息的确定进度条](images/progress_bar_determinate_redline.png)

-   多个进度条

    ![多个进度条的推荐布局](images/progress_bar_multi_redline.png)

-   带状态文本的不确定进度环

    ![带状态文本的不确定进度环的布局](images/progress_ring_status_text.png)

-   不确定进度条

    ![用于不确定进度条的布局](images/progress_indeterminate_bar_redline.png)

## 其他使用指南

### 选择进度样式的决策树

-   **用户需要知道发生了什么情况吗？**

    如果答案为否定，则不显示进度控件。

-   **是否提供有关还有多长时间可完成任务的信息？**
    -   **是：****任务是否需要两秒以上才能完成？**
        -   **是：**使用确定进度栏。 对于执行时间超过 10 秒的任务，请提供取消该任务的方法。
        -   **否：**不显示进度控件。

    -   **否：****是否阻止用户与 UI 交互直至任务完成？**
        -   **是：****此任务是否属于用户需要了解的操作的特定详情的多步骤流程？**
            -   **是：**使用不确定进度环，并且屏幕中心水平放置状态文本。
            -   **否：**使用不确定进度环，屏幕中心不显示文本。
        -   **否：****这是否是主要活动？**
            -   **是：****进度是否与 UI 中的特定的单个元素相关？**
                -   **是：**使用内联不确定进度环，并在其相关 UI 元素旁边显示状态文本。
                -   **否：****是否会有大量数据会加载到列表？**
                    -   **是：**在顶部使用不确定进度栏，并带有表示传入内容的占位符。
                    -   **否：**在屏幕或图面顶部使用不确定进度条。
            -   **否：**在屏幕上角中使用状态文本。

## 相关文章


- [**ProgressBar 类**](https://msdn.microsoft.com/library/windows/apps/br227529)
- [**ProgressRing 类**](https://msdn.microsoft.com/library/windows/apps/br227538)

**对于开发人员 (XAML)**
- [添加进度控件](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651)
- [如何为 Windows Phone 创建自定义不确定进度栏](http://go.microsoft.com/fwlink/p/?LinkID=392426)


<!--HONumber=Mar16_HO1-->


