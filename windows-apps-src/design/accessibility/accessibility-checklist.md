---
Description: Provides a checklist to help you ensure that your Universal Windows Platform (UWP) app is accessible.
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: 辅助功能清单
label: Accessibility checklist
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c9ff9760b3ae9b852fe1ae1b86d1cc48e49c5dd4
ms.sourcegitcommit: 393180e82e1f6b95b034e99c25053d400e987551
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2019
ms.locfileid: "8990480"
---
# <a name="accessibility-checklist"></a>辅助功能清单

提供了一个帮助你确保你的通用 Windows 平台 (UWP) 应用是辅助应用的清单。

我们在此提供了一个清单，你可以用它来确保你的应用是辅助应用。

1. 为应用中的内容和交互 UI 元素设置辅助名称（必选）和辅助说明（可选）。

    辅助名称是屏幕阅读器用于述说 UI 元素的简短且具有描述性的文本字符串。 一些 UI 元素（如 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 和 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)）将其文本内容提升为默认辅助名称；请参阅[基本辅助功能信息](basic-accessibility-information.md#name_from_inner_text)。

    你应当为图像或没有将内部文本内容提升为隐式辅助名称的其他控件明确设置辅助名称。 你应当针对窗体元素使用标签，以便标签文本可以用作 Microsoft UI 自动化模型中的 [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) 目标来将标签与输入相关联。 如果你想要为用户提供的 UI 指南比辅助名称中通常包括的指南多，则可以使用辅助说明和工具提示来帮助用户了解 UI。

    有关详细信息，请参阅[辅助名称](basic-accessibility-information.md#accessible_name)和[辅助说明](basic-accessibility-information.md)。

2. 实现键盘辅助功能：

    * 测试 UI 的默认 Tab 键索引顺序。 必要时调整 Tab 键索引顺序，这可能需要启用或禁用某些控件或者更改某些 UI 元素上 [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) 的默认值。
    * 为复合元素使用支持箭头键导航的控件。 对于默认控件，通常已经实现了箭头键导航。
    * 使用支持键盘激活的控件。 对于默认控件，特别是那些支持 UI 自动化 [**Invoke**](https://msdn.microsoft.com/library/windows/apps/BR242582) 模式的控件，通常提供键盘激活功能；请检查该控件的文档。
    * 为 UI 中支持交互的特定部分设置访问键或实现加速键。
    * 对于 UI 中所使用的任何自定义控件，请验证你已经用正确的 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 激活支持实现了这些控件，而且已经根据需要定义了键处理替换选项以支持激活、遍历、访问键或加速键。

    有关详细信息，请参阅[键盘交互](https://msdn.microsoft.com/library/windows/apps/Mt185607)。

3. 确保文本是可读的大小

    * Windows 包含各种辅助功能工具和设置，用户可以充分利用和调整到自己需求和偏好阅读文本。 其中包括：
        * 放大 UI 的选定的区域放大镜工具。 你应确保在应用中的文本布局不会将很难使用放大镜进行读取。
        * 中的全局缩放和分辨率设置**设置-> 系统-> 显示-> 缩放和布局**。 究竟有哪些大小设置选项可用可能有所不同因为这取决于显示设备的功能。
        * 在文本大小设置**设置-> 轻松使用-> 显示**。 调整**使更大的文本**设置指定仅文本的大小以支持跨所有应用程序和屏幕 （所有 UWP 文本控件都支持文本缩放体验而无需任何自定义或模板化） 控件。
        > [!NOTE]
        > **使一切变大**设置允许用户仅其主屏幕上一般情况下指定文本和应用的首选的大小。

4. 以直观方式验证你的 UI 以确保文本对比度足够大、元素以高对比度主题正确呈现以及使用了正确的颜色。

    * 使用颜色分析器工具验证视觉文本对比度至少为 4.5:1。
    * 切换到高对比度主题并验证应用的 UI 可读且可用。
    * 确保你的 UI 未将颜色用作传递信息的唯一方式。

    有关详细信息，请参阅[高对比度主题](high-contrast-themes.md)和[辅助文本要求](accessible-text-requirements.md)。

5. 运行辅助功能工具，解决报告的问题以及验证屏幕读取体验。

    使用 [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) 之类的工具验证编程访问，运行诊断工具（如 [**AccChecker**](https://msdn.microsoft.com/library/windows/desktop/Hh920985)）发现常见错误以及使用讲述人验证屏幕读取体验。

    有关详细信息，请参阅[辅助功能测试](accessibility-testing.md)。

6. 确保你的应用清单设置遵循辅助功能指南。

7. 在 Microsoft Store 中将你的应用声明为辅助应用。

    如果已实现基线辅助功能支持，则在 Microsoft Store 中将应用声明为辅助应用有助于获得更多客户并获取一些额外好评。

    有关详细信息，请参阅 [Microsoft Store 中的辅助功能](accessibility-in-the-store.md)。

## <a name="related-topics"></a>相关主题  

* [辅助文本要求](accessible-text-requirements.md)
* [文本缩放](../input/text-scaling.md)
* [辅助功能](accessibility.md)
* [辅助功能设计](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [要避免的做法](practices-to-avoid.md)
