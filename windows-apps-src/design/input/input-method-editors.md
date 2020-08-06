---
Description: 输入法编辑器 (IME) 是一个软件组件，它使用户能够以一种标准键盘上无法轻松表示的语言输入文本。
title: '输入法编辑器 (IME) '
label: Input Method Editors (IME)
template: detail.hbs
keywords: ime、输入法编辑器、输入、交互
ms.date: 07/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 438c53a0f3fbec1fdac0206bde3584c738759de4
ms.sourcegitcommit: 86ce67a03e87fa1282849b2fcb4f89d1cf23a091
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/05/2020
ms.locfileid: "87839996"
---
# <a name="input-method-editors-ime"></a>输入法编辑器 (IME) 

输入法编辑器 (IME) 是一个软件组件，它使用户能够以一种标准键盘上无法轻松表示的语言输入文本。 这通常是由于用户的书面语言中的字符数（如各种东亚语言）引起的。

用户键入 IME 解释的键组合，而不是每个单个键盘键上显示的单个字符。 IME 会生成与键击集匹配的字符或要从中进行选择的候选字符列表。 然后，所选字符将插入用户与之交互的编辑控件中。

> [!NOTE]
> Ime 可支持硬件键盘和屏幕或触摸键盘。

您的应用程序不需要与 IME 直接交互。 IME 内置于系统中，就像触摸键盘一样。 如果你的应用程序具有文本输入，并且你打算支持需要 IME 的语言提供文本输入，则应测试输入的端到端客户体验。 这样就可以解决任何问题，例如调整 UI，使触摸键盘或 IME 候选窗口不会封闭像素。

## <a name="creating-an-ime"></a>创建 IME

为了使所有用户都能获得极佳的输入体验，Microsoft 生成了用于各种语言的现成的 Ime。

除了内置输入法外，你还可以构建你自己的自定义 Ime，用户可以安装并使用它，就像内置输入法一样。

所有 Ime 都在 Windows 系统中运行，这将被强制停止恶意 Ime 并改善所有 Ime 的安全性和用户体验。

自定义 Ime 可以链接到默认的触摸键盘并使用其布局，以便最终用户可以通过触摸键盘使用其 IME。 但是，您不能提供自己的独立触摸键盘，触摸键盘的内置 Ime 功能不能用于自定义输入法。

## <a name="requirements-for-imes"></a>Ime 要求

第三方 IME 必须满足以下要求：

- 必须进行数字签名
- 必须是[文本服务框架 (TSF) ](/windows/win32/tsf/text-services-framework)知道，并正确设置适当的 IME 标记
- 必须遵循输入法编辑器中所述的准则[ (IME) 要求](input-method-editor-requirements.md)和[设计和代码 Windows 应用](/windows/uwp/design/)

不满足这些要求的第三方 IME 被阻止运行。

> [!NOTE]
> 旧的自定义 Ime 可以在桌面应用中运行，但会在 Windows 应用中被阻止。

此外，Windows Defender 还会从系统中删除恶意的 Ime。 因此，请务必熟悉 IME 编码要求。 有关详细信息，请参阅[输入法编辑器 (IME) 要求](input-method-editor-requirements.md)。

## <a name="design-guidelines-for-imes"></a>Ime 设计准则

阅读[输入法编辑器 (ime) 要求](input-method-editor-requirements.md)，详细了解有关输入法的最佳实践和设计准则的详细信息。 通常，所有 IME Ui 都需要：

- 遵循 Windows 运行时应用的 UX 指导原则
- 避免出现模式体验，并在需要时仅显示 IME 窗口
- 包括仅黑白的图标

## <a name="related-topics"></a>相关主题

- [输入法编辑器 (IME) 要求](input-method-editor-requirements.md)
- [ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout)
- [ITfCompartmentEventSink](/windows/win32/api/msctf/nn-msctf-itfcompartmenteventsink)
- [ITfThreadMgrEx::GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags)
- [ITfContextView::GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd)
- [TF_INPUTPROCESSORPROFILE](/windows/win32/api/msctf/ns-msctf-tf_inputprocessorprofile)
- [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput)
