---
Description: 提供了一个可帮助你确保你的通用 Windows 平台 (UWP) 应用是辅助应用的清单。
title: 辅助功能清单
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
label: Checklist
template: detail.hbs
---

辅助功能清单
===================================================================================

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

提供了一个可帮助你确保你的通用 Windows 平台 (UWP) 应用是辅助应用的清单。

我们在此提供了一个清单，你可以用它来确保你的应用是辅助应用。

1.  为应用中的内容和交互 UI 元素设置辅助名称（必选）和辅助说明（可选）。

    辅助名称是屏幕阅读器用于述说 UI 元素的简短且具有描述性的文本字符串。 一些 UI 元素（如 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 和 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)）将其文本内容提升为默认辅助名称；请参阅[基本辅助功能信息](basic-accessibility-information.md#name_from_inner_text)。

    你应当为图像或没有将内部文本内容提升为隐式辅助名称的其他控件明确设置辅助名称。 你应当针对窗体元素使用标签，以便标签文本可以用作 Microsoft UI 自动化模型中的 [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) 目标来将标签与输入相关联。 如果你想要为用户提供的 UI 指南比辅助名称中通常包括的指南多，则可以使用辅助说明和工具提示来帮助用户了解 UI。

    有关详细信息，请参阅[辅助名称](basic-accessibility-information.md#accessible_name)和[辅助说明](basic-accessibility-information.md)。

2.  实现键盘辅助功能：


    -   Test the default tab index order for a UI. Adjust the tab index order if necessary, which may require enabling or disabling certain controls, or changing the default values of [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) on some of the UI elements.
    -   Use controls that support arrow-key navigation for composite elements. For default controls, the arrow-key navigation is typically already implemented.
    -   Use controls that support keyboard activation. For default controls, particularly those that support the UI Automation [**Invoke**](https://msdn.microsoft.com/library/windows/apps/BR242582) pattern, keyboard activation is typically available; check the documentation for that control.
    -   Set access keys or implement accelerator keys for specific parts of the UI that support interaction.
    -   For any custom controls that you use in your UI, verify that you have implemented these controls with correct [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) support for activation, and defined overrides for key handling as needed to support activation, traversal and access or accelerator keys.

    For more info, see [Keyboard interactions](https://msdn.microsoft.com/library/windows/apps/Mt185607).

3.  以直观方式验证你的 UI 以确保文本对比度足够大，元素以高对比度主题正确呈现以及使用了正确的颜色。

    -   使用可调节屏幕的每英寸点数 (dpi) 值的系统屏幕选项，并确保当该 dpi 值发生更改时，你的应用 UI 可正确缩放。 （某些用户可使用辅助功能选项更改 dpi 值，该选项可从**“轻松访问”**中获得。）
    -   使用颜色分析器工具验证视觉文本对比度至少为 4.5:1。
    -   切换到高对比度主题并验证应用的 UI 可读且可用。
    -   确保你的 UI 未将颜色用作传递信息的唯一方式。

    有关详细信息，请参阅[高对比度主题](high-contrast-themes.md)和[辅助文本要求](accessible-text-requirements.md)。

4.  运行辅助功能工具，解决报告的问题以及验证屏幕读取体验。

    使用 [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) 之类的工具验证编程访问，运行诊断工具（如 [**AccChecker**](https://msdn.microsoft.com/library/windows/desktop/Hh920985)）发现常见错误以及使用讲述人验证屏幕读取体验。

    有关详细信息，请参阅[辅助功能测试](accessibility-testing.md)。

5.  确保你的应用清单设置遵循辅助功能指南。

6.  在 Windows 应用商店中将你的应用声明为辅助应用。

    如果已实现基线辅助功能支持，则在 Windows 应用商店中将应用声明为辅助应用有助于获得更多客户并获取一些额外好评。

    有关详细信息，请参阅[应用商店中的辅助功能](accessibility-in-the-store.md)。

<span id="related_topics"> </span>相关主题
-----------------------------------------------

* [辅助功能](accessibility.md)
* [辅助功能设计](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [要避免的做法](practices-to-avoid.md)
 

 





<!--HONumber=Mar16_HO3-->


