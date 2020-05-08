---
Description: 提供了一个核对清单，可帮助你确保 Windows 应用程序可访问。
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: 辅助功能清单
label: Accessibility checklist
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c7775c2d6c9e579e14c9f607fa0a09b665dbb24b
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82969758"
---
# <a name="accessibility-checklist"></a>辅助功能清单

提供了一个核对清单，可帮助你确保 Windows 应用程序可访问。

我们在此提供了一个清单，你可以用它来确保你的应用是辅助应用。

1. 为应用中的内容和交互式 UI 元素设置辅助名称（必选）和描述（可选）。

    辅助名称是屏幕阅读器用于述说 UI 元素的简短且具有描述性的文本字符串。 一些 UI 元素（如 [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 和 [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)）将其文本内容提升为默认辅助名称；请参阅[基本辅助功能信息](basic-accessibility-information.md#name_from_inner_text)。

    你应当为图像或没有将内部文本内容提升为隐式辅助名称的其他控件明确设置辅助名称。 你应当针对窗体元素使用标签，以便标签文本可以用作 Microsoft UI 自动化模型中的 [**LabeledBy**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) 目标来将标签与输入相关联。 如果你想要为用户提供的 UI 指南比辅助名称中通常包括的指南多，则可以使用辅助说明和工具提示来帮助用户了解 UI。

    有关详细信息，请参阅[辅助名称](basic-accessibility-information.md#accessible_name)和[辅助说明](basic-accessibility-information.md)。

2. 实现键盘辅助功能：

    * 测试 UI 的默认 Tab 键索引顺序。 必要时调整 Tab 键索引顺序，这可能需要启用或禁用某些控件或者更改某些 UI 元素上 [**TabIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.tabindex) 的默认值。
    * 为复合元素使用支持箭头键导航的控件。 对于默认控件，通常已经实现了箭头键导航。
    * 使用支持键盘激活的控件。 对于默认控件，特别是那些支持 UI 自动化 [**Invoke**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) 模式的控件，通常提供键盘激活功能；请检查该控件的文档。
    * 为 UI 中支持交互的特定部分设置访问键或实现加速键。
    * 对于 UI 中所使用的任何自定义控件，请验证你已经用正确的 [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) 激活支持实现了这些控件，而且已经根据需要定义了键处理替换选项以支持激活、遍历、访问键或加速键。

    有关详细信息，请参阅[键盘交互](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)。

3. 确保文本为可读的大小

    * Windows 包括各种辅助功能和设置，用户可利用这些工具和设置，并调整其自身需求和阅读文本的首选项。 其中包括：
        * "放大镜" 工具，用于放大 UI 的选定区域。 你应确保应用中的文本布局不会使使用放大镜进行读取变得困难。
        * 设置中的全局缩放和分辨率设置 **->系统 >显示->比例和布局**。 确切的可用大小调整选项会有所不同，具体取决于显示设备的功能。
        * 设置中的文本大小设置 **->轻松访问->显示**。 调整 "放大**文本**" 设置，以便仅在所有应用程序和屏幕上指定支持控件中的文本大小（所有 UWP 文本控件都支持没有任何自定义项或模板化的文本缩放体验）。
        > [!NOTE]
        > "**使所有内容变得更大**" 设置允许用户仅在主屏幕上指定其首选的文本和应用大小。

4. 以直观方式验证你的 UI 以确保文本对比度足够大、元素以高对比度主题正确呈现以及使用了正确的颜色。

    * 使用颜色分析器工具验证视觉文本对比度至少为 4.5:1。
    * 切换到高对比度主题并验证应用的 UI 可读且可用。
    * 确保你的 UI 未将颜色用作传递信息的唯一方式。

    有关详细信息，请参阅[高对比度主题](high-contrast-themes.md)和[辅助文本要求](accessible-text-requirements.md)。

5. 运行辅助功能工具、解决报告的问题并验证屏幕阅读体验。

    使用 [**Inspect**](https://docs.microsoft.com/windows/desktop/WinAuto/inspect-objects) 之类的工具验证编程访问，运行诊断工具（如 [**AccChecker**](https://docs.microsoft.com/windows/desktop/WinAuto/ui-accessibility-checker)）发现常见错误以及使用讲述人验证屏幕读取体验。

    有关详细信息，请参阅[辅助功能测试](accessibility-testing.md)。

6. 确保你的应用清单设置遵循辅助功能指南。

7. 在 Microsoft Store 中将你的应用声明为辅助应用。

    如果已实现基线辅助功能支持，则在 Microsoft Store 中将应用声明为辅助应用有助于获得更多客户并获取一些额外好评。

    有关详细信息，请参阅 [Microsoft Store 中的辅助功能](accessibility-in-the-store.md)。

## <a name="related-topics"></a>相关主题  

* [辅助文本要求](accessible-text-requirements.md)
* [文本缩放](../input/text-scaling.md)
* [辅助功能](accessibility.md)
* [辅助功能设计](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-overview)
* [要避免的做法](practices-to-avoid.md)
