---
Description: 提供了一个可帮助你确保你的通用 Windows 平台 (UWP) 应用是辅助应用的清单。
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: 辅助功能清单
label: Accessibility checklist
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e8e9395517511a40c215e31816962c186968c9f3
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362103"
---
# <a name="accessibility-checklist"></a>辅助功能清单

提供了一个帮助你确保你的通用 Windows 平台 (UWP) 应用是辅助应用的清单。

我们在此提供了一个清单，你可以用它来确保你的应用是辅助应用。

1. 为应用中的内容和交互 UI 元素设置辅助名称（必选）和辅助说明（可选）。

    辅助名称是屏幕阅读器用于述说 UI 元素的简短且具有描述性的文本字符串。 一些 UI 元素（如 [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 和 [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)）将其文本内容提升为默认辅助名称；请参阅[基本辅助功能信息](basic-accessibility-information.md#name_from_inner_text)。

    你应当为图像或没有将内部文本内容提升为隐式辅助名称的其他控件明确设置辅助名称。 你应当针对窗体元素使用标签，以便标签文本可以用作 Microsoft UI 自动化模型中的 [**LabeledBy**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v%3Dvs.95)) 目标来将标签与输入相关联。 如果你想要为用户提供的 UI 指南比辅助名称中通常包括的指南多，则可以使用辅助说明和工具提示来帮助用户了解 UI。

    有关详细信息，请参阅[辅助名称](basic-accessibility-information.md#accessible_name)和[辅助说明](basic-accessibility-information.md)。

2. 实现键盘辅助功能：

    * 测试 UI 的默认 Tab 键索引顺序。 必要时调整 Tab 键索引顺序，这可能需要启用或禁用某些控件或者更改某些 UI 元素上 [**TabIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.tabindex) 的默认值。
    * 为复合元素使用支持箭头键导航的控件。 对于默认控件，通常已经实现了箭头键导航。
    * 使用支持键盘激活的控件。 对于默认控件，特别是那些支持 UI 自动化 [**Invoke**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) 模式的控件，通常提供键盘激活功能；请检查该控件的文档。
    * 为 UI 中支持交互的特定部分设置访问键或实现加速键。
    * 对于 UI 中所使用的任何自定义控件，请验证你已经用正确的 [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) 激活支持实现了这些控件，而且已经根据需要定义了键处理替换选项以支持激活、遍历、访问键或加速键。

    有关详细信息，请参阅[键盘交互](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)。

3. 请确保文本是可读大小

    * Windows 包括各种可访问性工具和设置，用户可以充分利用并调整其自己的需求和用于读取文本的首选项。 这些问题包括：
        * 放大镜工具，它将放大选定的区域的用户界面。 您应确保应用程序中的文本的布局不会使其难以使用放大镜以进行读取。
        * 中的全局规模和解决方法设置**设置-> 系统-> 显示-> 扩展和布局**。 完全有哪些大小调整选项可能有所不同，因为这取决于显示设备的功能。
        * 中的文本大小设置**设置-> 轻松访问-> 显示**。 调整**使文本更大**设置支持跨所有应用程序和屏幕 （所有 UWP 文本控件都支持缩放体验，而无任何自定义或模板化的文本） 的控件中指定的文本大小。
        > [!NOTE]
        > **保持所有对象的更大**设置可使用户在一般情况下在其主屏幕上指定的文本和应用程序的首选的大小。

4. 以直观的方式验证你的 UI 以确保文本对比度足够大，元素以高对比度主题正确呈现以及使用了正确颜色。

    * 使用颜色分析器工具验证视觉文本对比度至少为 4.5:1。
    * 切换到高对比度主题并验证应用的 UI 可读且可用。
    * 确保你的 UI 未将颜色用作传递信息的唯一方式。

    有关详细信息，请参阅[高对比度主题](high-contrast-themes.md)和[辅助文本要求](accessible-text-requirements.md)。

5. 运行辅助功能工具，解决报告的问题以及验证屏幕读取体验。

    使用 [**Inspect**](https://docs.microsoft.com/windows/desktop/WinAuto/inspect-objects) 之类的工具验证编程访问，运行诊断工具（如 [**AccChecker**](https://docs.microsoft.com/windows/desktop/WinAuto/ui-accessibility-checker)）发现常见错误以及使用讲述人验证屏幕读取体验。

    有关详细信息，请参阅[辅助功能测试](accessibility-testing.md)。

6. 确保你的应用清单设置遵循辅助功能指南。

7. 在 Microsoft Store 中将你的应用声明为辅助应用。

    如果已实现基线辅助功能支持，则在 Microsoft Store 中将应用声明为辅助应用有助于获得更多客户并获取一些额外好评。

    有关详细信息，请参阅[应用商店中的辅助功能](accessibility-in-the-store.md)。

## <a name="related-topics"></a>相关主题  

* [辅助文本要求](accessible-text-requirements.md)
* [文本缩放](../input/text-scaling.md)
* [辅助功能](accessibility.md)
* [可访问性的设计](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-overview)
* [要避免的做法](practices-to-avoid.md)
