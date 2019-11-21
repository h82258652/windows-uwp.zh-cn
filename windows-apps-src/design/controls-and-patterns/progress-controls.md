---
Description: 进度控件将为用户提供关于正在处理运行时间较长的操作的反馈。
title: 进度控件指南
ms.assetid: FD53B716-C43D-408D-8B07-522BC1F3DF9D
label: Progress controls
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: jeffarn
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 67315518238bda1359862f36acd398e25e8481e3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258157"
---
# <a name="progress-controls"></a>进度控件

 

进度控件将为用户提供关于正在处理运行时间较长的操作的反馈。 这意味着，在进度指示器可见，并且还可以根据所使用的指示器指示等待时长时，用户无法与该应用交互。

> **重要的 API**：[ProgressBar 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar)、[IsIndeterminate 属性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressbar.isindeterminate)、[ProgressRing 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)、[IsActive 属性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressring.isactive)

## <a name="types-of-progress"></a>进度类型

向用户显示操作正在进行的控件有两个：ProgressBar 或 ProgressRing。

-   ProgressBar *determinate* 状态显示任务完成的百分比。 此控件应该在持续时间已知的操作中使用，但进度不应阻止用户与应用交互。
-   ProgressBar *indeterminate* 状态显示操作正在进行、没有阻止用户与应用交互，并且完成时间未知。
-   ProgressRing 仅具有 *indeterminate* 状态，并且应该仅在任何其他用户交互均已阻止时使用，直到操作完成为止。

另外，进度控件为只读，不具有交互性。 这意味着用户无法直接调用或使用这些控件。

![ProgressBar 状态](images/ProgressBar_TwoStates.png)

*自顶到底 - 不确定 ProgressBar 和确定 ProgressBar*

![ProgressRing 状态](images/ProgressRing_SingleState.png)

*不确定 ProgressRing*

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处打开该应用，了解 <a href="xamlcontrolsgallery:/item/ProgressBar">ProgressBar</a> 或 <a href="xamlcontrolsgallery:/item/ProgressRing">ProgressRing</a> 的实际应用。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="when-to-use-each-control"></a>使用每个控件的时间

在尝试显示正在执行的操作时，通常不清楚要使用的控件和所处的状态（确定和不确定）。 有时，任务明显不需要进度控件，但有时即使使用了进度控件，仍需要使用一行文本才能向用户解释正在进行的操作。

### <a name="progressbar"></a>进度栏
-   **该控件是否具有已定义的持续时间，或者结束时间是否已预测？**

    使用确定 ProgressBar，并且相应地更新百分比或值。

-   **用户是否可以在不监视操作进度的情况下继续操作？**

    当 ProgressBar 处在使用状态下时，交互为非模式交互，这通常意味着用户没有因操作未完成而受到阻止，并且可以继续以其他方式使用应用，直到这个方面完成。

-   **关键字**

    如果操作属于这些关键字范围内，或者显示进度操作旁边与这些关键字匹配的文本，请考虑使用 ProgressBar：

    - 正在加载... 
    - 正在检索 
    - 正在处理... 

### <a name="progressring"></a>ProgressRing

-   **该操作会使用户等候继续操作吗？**

    如果操作需要全部（或大部分）应用交互等到它完成后才能进行，则最好选择 ProgressRing。 ProgressRing 控件用于模式交互，这意味着用户受到阻止，直到 ProgressRing 消失。

-   **应用是否需要等待用户完成某个任务？**

    如果是，请使用 ProgressRing，因为它们为用户指示未知的等待时间。

-   **关键字**

    如果操作属于这些关键字范围内，或者显示进度操作旁边与这些关键字匹配的文本，请考虑使用 ProgressRing：

    - *正在刷新*
    - *正在登录...*
    - *正在连接...*

### <a name="no-progress-indication-necessary"></a>不需要进度指示
-   **用户需要知道发生了什么情况吗？**

    例如，如果应用正在后台下载文件，但是用户没有启动下载，则用户不一定需要知道有关该下载的信息。

-   **操作是否属于不会阻止用户活动且用户基本不感兴趣（但略有趣）的后台活动？**

    如果应用执行的任务不必始终可见，但仍然需要显示状态时，请使用文本。

-   **用户是否仅仅关注操作完成与否？**

    有时最好仅在操作完成后显示一条通知，或者提供操作刚才已完成的视觉效果，最后在后台进行处理。

## <a name="progress-controls-best-practices"></a>进度控件最佳做法

有时最好可以看到何时在何处使用这些不同进度控件的视觉表示形式：

**ProgressBar - 确定**

![ProgressBar 确定示例](images/PB_DeterminateExample.png)

第一个示例是确定 ProgressBar。 如果已知操作持续时间，则最好使用确定 ProgressBar 显示何时安装、下载、设置等。

**ProgressBar - 不确定**

![ProgressBar 不确定示例](images/PB_IndeterminateExample.png)

如果不知道操作持续时间，请使用不确定 ProgressBar。 在填充虚拟化列表和在不确定和确定 ProgressBar 之间创建流畅的视觉过渡时，也可以使用不确定 ProgressBar。

-   **操作是否在虚拟化集合中？**

    如果是，则不要在显示列表项目时在这些项目上放置进度指示器。 相反，使用 ProgressBar，并将其放置在所加载的项目集合顶部，以显示正在提取项目。

**ProgressRing - 不确定**

![ProgressRing 不确定示例](images/PR_IndeterminateExample.png)

如果暂停任何其他用户应用交互，或者应用在等待用户输入内容才能继续操作，则使用不确定 ProgressRing。 上述“正在登录…” 示例是 ProgressRing 的完美方案，在登录完成前，用户无法继续使用该应用。

## <a name="customizing-a-progress-control"></a>自定义进度控件

两个进度控件都非常简单，但控件的某些视觉功能看起来不能自定义。

**设置 ProgressRing 的大小**

ProgressRing 的大小可以调整到所需大小，但最小只能为 20x20epx。 为了调整 ProgressRing 大小，必须设置其高度和宽度。 如果仅设置高度或宽度，控件将假定最小大小 (20x20epx)；相反，如果高度和宽度设为两个不同大小，将假定两个大小中较小的一个。
若要确保 ProgressRing 符合需要，请将高度和宽度设为相同值：

```XAML
<ProgressRing Height="100" Width="100"/>
```

为了使 ProgressRing 可见并且可以形成动画，必须将 IsActive 属性设为 true：

```XAML
<ProgressRing IsActive="True" Height="100" Width="100"/>
```

```C#
progressRing.IsActive = true;
```

**为进度控件着色**

默认情况下，进度控件的主色设置为系统的主题色。 若要替代此画笔，只需更改任一控件的前景属性。

```XAML
<ProgressRing IsActive="True" Height="100" Width="100" Foreground="Blue"/>
<ProgressBar Width="100" Foreground="Green"/>
```

更改 ProgressRing 的前景色将更改点颜色。 进度条的前景属性可以更改进度条的填充颜色；若要更改进度条的未填充部分，只需重写背景属性。

**显示等待光标**

有时，如果应用或操作需要时间考虑，并且你需要向用户指示在等待光标消失前不应与等待光标可见的应用或区域交互，最好只简短地显示等待光标。

```C#
Window.Current.CoreWindow.PointerCursor = new Windows.UI.Core.CoreCursor(Windows.UI.Core.CoreCursorType.Wait, 10);
```

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

- [ProgressBar 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar)
- [ProgressRing 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)

**面向开发人员 (XAML)**
- [添加进度控件](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10))
- [如何为 Windows Phone 创建自定义不确定进度栏](https://msdn.microsoft.com/library/windows/apps/gg442303.aspx)
