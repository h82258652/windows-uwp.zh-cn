---
Description: Testing procedures to follow to ensure that your Universal Windows Platform (UWP) app is accessible.
ms.assetid: 272D9C9E-B179-4F5A-8493-926D007A0225
title: 辅助功能测试
label: Accessibility testing
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3a06859d6580720150383f2995505e671c0a31a4
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "8199928"
---
# <a name="accessibility-testing"></a>辅助功能测试  

为确保通用 Windows 平台 (UWP) 应用为辅助应用而要遵循的测试过程。

<span id="run_accessibility_testing_tools"/>
<span id="RUN_ACCESSIBILITY_TESTING_TOOLS"/>

## <a name="run-accessibility-testing-tools"></a>运行辅助功能测试工具  
Windows 软件开发工具包 (SDK) 包括多个辅助功能测试工具，例如 [**AccScope**](https://msdn.microsoft.com/library/windows/desktop/Dn433239)、[**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) 和 [**UI Accessibility Checker**](https://msdn.microsoft.com/library/windows/desktop/Hh920985)。 这些工具可帮助你验证应用的辅助功能。 确保验证所有应用方案和 UI 元素。

可从 Microsoft Visual Studio 命令提示符或从 Windows SDK 工具文件夹（你的开发计算机上安装 Windows SDK 的 bin 子目录）启动辅助功能测试工具。
  
> [!VIDEO https://www.youtube.com/embed/ce0hKQfY9B8]
  
<span id="AccScope"/>
<span id="accscope"/>
<span id="ACCSCOPE"/>

### **<a name="accscope"></a>AccScope**  

[**AccScope**](https://msdn.microsoft.com/library/windows/desktop/Dn433239) 工具允许开发人员和测试人员在其应用的开发和设计期间评估该应用的辅助功能，并且可能在早期原型设计阶段进行，而不是在应用开发周期的后期测试阶段。 它专门用于测试应用程序的讲述人辅助功能方案。

<span id="inspect"/>
<span id="INSPECT"/>

### **<a name="inspect"></a>检查**  

你可以使用 [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) 选择任何 UI 元素并查看其辅助功能数据。 可以查看 Microsoft UI 自动化属性和控件模式并测试 UI 自动化树中自动化元素的导航结构。 当你开发 UI 以验证如何在 UI 自动化中公开辅助功能属性时，请使用 **Inspect**。 在某些情况下，属性来自已经为默认 XAML 控件实现的 UI 自动化支持。 在其他情况下，属性来自已经在 XAML 标记中设置为 [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties) 附加属性的特定值。

下图展示了 [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) 工具，用于查询记事本中 **“编辑”** 菜单元素的 UI 自动化属性。

![“检查”工具的屏幕截图。](./images/inspect.png)

<span id="ui_accessibility_checker"/>
<span id="UI_ACCESSIBILITY_CHECKER"/>

### **<a name="ui-accessibility-checker"></a>UI 辅助功能检查器**  
**UI 辅助功能检查器 (AccChecker)** 有助于发现运行时的辅助功能问题。 如果你的 UI 完整且可使用，则将 **AccChecker** 用于测试不同的方案、验证运行时辅助功能信息的正确性以及发现运行时问题。 可以在 UI 或命令行模式下运行 **AccChecker**。 若要运行 UI 模式工具，在 Windows SDK bin 目录中打开 **AccChecker** 目录、运行 acccheckui.exe，然后单击“帮助”**** 菜单。

<span id="ui_automation_verify"/>
<span id="UI_AUTOMATION_VERIFY"/>

### **<a name="ui-automation-verify"></a>UI 自动化验证**  
**UI 自动化验证（UIA 验证）** 是一种用于实现 UI 自动化的自动化测试和验证框架。 **UIA 验证**可以集成到测试代码中并针对 UI 自动化方案定期执行自动化测试或抽查。 若要运行 **UIA 验证**，请从 UIAVerify 子目录运行 VisualUIAVerifyNative.exe。

<span id="accessible_event_watcher"/>
<span id="ACCESSIBLE_EVENT_WATCHER"/>

### **<a name="accessible-event-watcher"></a>辅助功能事件查看器**  
[**Accessible Event Watcher (AccEvent)**](https://msdn.microsoft.com/library/windows/desktop/Dd317979) 测试在 UI 发生变化时，应用的 UI 元素是否引发正确的 UI 自动化和 Microsoft Active Accessibility 事件。 当焦点改变时，或者当 UI 元素被调用、选择或者其状态或属性发生变化时，UI 会发生变化。

> [!NOTE]
> 文档中提及的大多数辅助功能测试工具在电脑而不是手机上运行。 开发和使用模拟器时可以运行某些工具，但其中大部分工具无法在模拟器中公开 UI 自动化树。

<span id="test_keyboard_accessibility"/>
<span id="TEST_KEYBOARD_ACCESSIBILITY"/>

## <a name="test-keyboard-accessibility"></a>测试键盘辅助功能  
测试键盘辅助功能的最佳方式是拔下鼠标或使用屏幕键盘（如果使用的是平板设备）。 使用 _Tab_ 键测试键盘辅助功能导航。 应可以使用 _Tab_ 键循环选择所有交互 UI 元素。 对于复合 UI 元素，验证你可以使用箭头键在元素的不同部分之间导航。 例如，你应当能够使用键盘键在项目列表中导航。 最后，对于所有的交互 UI 元素，确保你可以在它们拥有焦点之后立即使用键盘（通常使用 Enter 键或空格）调用它们。

<span id="verify_the_contrast_ratio_of_visible_text"/>
<span id="VERIFY_THE_CONTRAST_RATIO_OF_VISIBLE_TEXT"/>

## <a name="verify-the-contrast-ratio-of-visible-text"></a>验证可见文本的对比度  
使用颜色对比工具验证可见文本的对比度是否可接受。 例外情况包括不活动的 UI 元素，以及不传递任何信息且可以在含义不变的情况下重新整理的徽标或装饰文本。 有关对比率和例外情况的详细信息，请参阅[辅助文本要求](accessible-text-requirements.md)。 请参阅 [WCAG 2.0 G18 的技术（“资源”部分）](http://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources)了解可以测试对比率的工具。

> [!NOTE]
> “WCAG 2.0 G18 的技术”列出的某些工具无法与 UWP 应用交互使用。 你可能需要在该工具中手动输入前景和背景颜色值，对应用 UI 进行屏幕捕获，然后对屏幕捕获图像运行对比度工具，或者在图像编辑程序中打开源位图文件时运行该工具，而不是在应用加载该图像时。

<span id="verify_your_app_in_high_contrast"/>
<span id="VERIFY_YOUR_APP_IN_HIGH_CONTRAST"/>

## <a name="verify-your-app-in-high-contrast"></a>在高对比度下验证应用  
当高对比度主题处于活动状态时使用你的应用，以验证所有 UI 元素均可正确显示。 所有文本都应可读，且所有图像都应清晰可见。 调整 XAML 主题-字典资源或控件模板，以更正源自控件的所有主题问题。 如果显著的高对比度问题不是来自主题或控件（如图像文件），请提供在高对比度主题处于活动状态时要使用的单独的版本。

<span id="verify_your_app_with_make_everything_on_your_screen_bigger"/>
<span id="VERIFY_YOUR_APP_WITH_MAKE_EVERYTHING_ON_YOUR_SCREEN_BIGGER"/>

## <a name="verify-your-app-with-display-settings"></a>使用显示设置验证你的应用  

使用可调节屏幕的每英寸点数 (dpi) 值的系统屏幕选项，并确保当该 dpi 值发生更改时，你的应用 UI 可正确缩放。 （某些用户可采用辅助功能选项方式更改 dpi 值，该选项可从“轻松使用”**** 以及屏幕属性中获得。）如果发现任何问题，请遵循[布局缩放指南](https://msdn.microsoft.com/library/windows/apps/Dn611863)，并提供其他资源以用于不同的缩放因素。

<span id="verify_main_app_scenarios_by_using_narrator"/>
<span id="VERIFY_MAIN_APP_SCENARIOS_BY_USING_NARRATOR"/>

## <a name="verify-main-app-scenarios-by-using-narrator"></a>使用“讲述人”验证主应用方案  
使用“讲述人”测试应用的屏幕阅读体验。

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode/player]

**使用以下步骤，配合鼠标和键盘使用“讲述人”测试你的应用：**
1.  按 _Windows 徽标键 + Ctrl + Enter_ 启动“讲述人”。 在 Windows 10 版本 1607 之前的版本中，使用 _Windows 徽标键 + Enter_ 启动“讲述人”。
2.  使用键盘上的 _Tab_ 键、箭头键、_Caps Lock + 箭头键_在你的应用中导航。
3.  在应用中导航时，听“讲述人”读 UI 元素，验证下列项：
    * 对于每个控件，确保“讲述人”读出所有可见内容。 还需要确保“讲述人”读出每个控件的名称、所有适用的状态（已选中、已选择，等等）、控件类型（按钮、复选框、列表项，等等）。
    * 如果相应元素可交互，请验证是否可以使用“讲述人”通过按 _Caps Lock + Enter_ 调用其操作。
    * 对于每个表格，确保“讲述人”正确读出表格名称、表格描述（如果可用）以及行标题和列标题。

4.  按 _Caps Lock + Shift + Enter_ 搜索你的应用，验证所有控件都显示在搜索列表中，并验证控件名称已本地化并且可读。
5.  关闭监视器，尝试只使用键盘和“讲述人”完成主屏方案。 若要获取“讲述人”命令和快捷方式的完整列表，请按 _Caps Lock + F1_。

从 Windows 10 版本 1607 开始，我们在“讲述人”中引入了新的开发人员模式。 当“讲述人”已经在运行时，通过按 _Caps Lock + Shift + F12_ 打开开发人员模式。 当开发人员模式已启用时，屏幕将屏蔽，并且将仅突出显示可访问的对象和以编程方式向“讲述人”公开的关联文本。 针对向“讲述人”公开的信息，这为你提供了良好的视觉表示形式。

**按照以下步骤，使用“讲述人”的触摸模式测试你的应用：**

> [!NOTE]
> 在支持 4 个以上联系人的设备上，“讲述人”自动进入触摸模式。 在主要屏幕上，“讲述人”不支持多监视器方案或多点触控数字化器。

1.  熟悉 UI，了解布局。

    * **使用单根手指轻扫手势在 UI 中导航。** 使用向左或向右轻扫在项目间移动，使用向上或向下轻扫更改要导航的项目的类别。 类别包括所有项目、链接、表格、标题等。 使用单根手指轻扫手势进行导航类似于使用 _Caps Lock + 箭头键_进行导航。
    * **使用 Tab 手势在可聚焦的元素间导航。** 三根手指向右或向左轻扫与使用键盘上的 _Tab_ 和 _Shift + Tab_ 导航相似。
    * **使用单根手指从空间角度调查 UI。** 上下拖动或左右拖动单根手指，以使“讲述人”读出你的手指下的项目。 可以使用鼠标作为替代项，因为鼠标在拖动单根手指时使用相同的点击测试逻辑。
    * **通过三个手指向上轻扫读出整个窗口及其所有内容**。 这等效于使用 _Caps Lock + W_。

    如果存在无法到达的重要 UI，则表明可能存在辅助功能问题。

2.  与控件交互，测试其主要操作、辅助操作以及滚动行为。

    主要操作包括激活按钮、放置文本插入符号、对控件设置焦点等操作。 辅助操作包括选择列表项、展开提供了多个选项的按钮等。

    * 测试主要操作：双击，或用一个手指按住，另一个手指点击。
    * 测试辅助操作：点击三次，或用一个手指按住，另一个手指双击。
    * 测试滚动行为：使用两个手指轻扫，沿所需方向滚动。

    某些控件提供其他操作。 若要显示完整列表，请用四根手指点击。

    如果控件响应鼠标或键盘，但不响应主要触摸交互或辅助触摸交互，则控件可能需要实现其他的 [UI 自动化](https://msdn.microsoft.com/library/windows/desktop/Ee684009)控件模式。

还应考虑使用 [**AccScope**](https://msdn.microsoft.com/library/windows/desktop/Dn433239) 工具测试应用的“讲述人”辅助功能方案。 [**AccScope 工具主题**](https://msdn.microsoft.com/library/windows/desktop/Dn433239)介绍了如何配置 **AccScope** 来测试“讲述人”方案。

<span id="Examine_the_UI_Automation_representation_for_your_app"/>
<span id="examine_the_ui_automation_representation_for_your_app"/>
<span id="EXAMINE_THE_UI_AUTOMATION_REPRESENTATION_FOR_YOUR_APP"/>

## <a name="examine-the-ui-automation-representation-for-your-app"></a>检查应用的 UI 自动化表示  
前面提到的某些 UI 自动化测试工具提供了一种查看应用的方式，这种方式有意不考虑应用的外观，而以 UI 自动化元素结构表示应用。 在辅助功能方案中，UI 自动化客户端（主要是辅助技术）将通过此方式与你的应用进行交互。

[**AccScope**](https://msdn.microsoft.com/library/windows/desktop/Dn433239) 工具提供了一种十分有趣的应用视图，因为你既能以可视化表示的形式查看 UI 自动化元素，也能以列表形式来查看。 如果使用可视化效果，那么你可以深入查看各个部分，从而对应用 UI 的视觉外观有一定的了解。 你甚至可以在为 UI 指定全部逻辑之前对最初 UI 原型的辅助功能进行测试，确保应用的视觉交互和辅助功能方案导航保持平衡。

可供测试的一个方面是，UI 自动化元素视图中是否存在不应出现在其中的元素。 如果发现要从该视图中去除的元素，或者恰好相反，视图中缺少某些元素，则可以使用 [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.accessibilityview) XAML 附加属性调整 XAML 控件在辅助功能视图中的显示方式。 查看基本辅助功能视图后，还可以通过启用箭头键重新检查 Tab 键序列或空间导航，从而确保用户可以查看控件视图中公开的各个交互部分。

<span id="related_topics"/>

## <a name="related-topics"></a>相关主题  
* [辅助功能](accessibility.md)
* [要避免的做法](practices-to-avoid.md)
* [UI 自动化](https://msdn.microsoft.com/library/windows/desktop/Ee684009)
* [Windows 中的辅助功能](http://go.microsoft.com/fwlink/p/?LinkId=320802)
* [讲述人入门](https://support.microsoft.com/help/22798/windows-10-narrator-get-started)
