---
author: Xansky
Description: Describes the concept of automation peers for Microsoft UI Automation, and how you can provide automation support for your own custom UI class.
ms.assetid: AA8DA53B-FE6E-40AC-9F0A-CB09637C87B4
title: 自定义自动化对等
label: Custom automation peers
template: detail.hbs
ms.author: mhopkins
ms.date: 07/13/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a2f9caf8519aa76ef9487e5318a238a6e1d53fe2
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2018
ms.locfileid: "2918878"
---
# <a name="custom-automation-peers"></a>自定义自动化对等  

介绍 Microsoft UI 自动化的自动化对等概念以及如何为自己的自定义 UI 类提供自动化支持。

UI 自动化提供了一个框架，可供自动化客户端用来检查或操作各种 UI 平台和框架的用户界面。 如果要编写通用 Windows 平台 (UWP) 应用，则你用于 UI 的类已经提供了 UI 自动化支持。 你可以从现有的非封装类通过派生方式定义一种新的 UI 控件或支持类。 在执行此操作的过程中，你的类可能会添加应当具有辅助功能支持，但默认的 UI 自动化支持未涵盖的行为。 在这种情况下，你应当通过以下方法扩展现有的 UI 自动化支持：从基实现使用的 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 类进行派生，为你的对等实现添加任何必需的支持，并通知通用 Windows 平台 (UWP) 控件基础结构应当创建新的对等。

UI 自动化不仅支持辅助功能应用和辅助技术（如屏幕阅读器），而且支持质量保证（测试）代码。 在任一情况下，UI 自动化客户端都可以检查用户界面元素并从应用外的其他代码模拟用户与你的应用的交互。 有关所有平台及其更广泛含义内的 UI 自动化的信息，请参阅 [UI 自动化概述](https://msdn.microsoft.com/library/windows/desktop/Ee684076)。

使用 UI 自动化框架的受众有以下两种。

* **UI 自动化*客户端*** 调用 UI 自动化 API 来了解当前显示给用户的所有 UI。 例如，屏幕阅读器等辅助技术充当 UI 自动化客户端。 UI 以相关自动化元素的树形式呈现。 UI 自动化客户端一次可能只对一个应用或整个树感兴趣。 UI 自动化客户端可以使用 UI 自动化 API 在树中导航并读取或更改自动化元素中的信息。
* **UI 自动化*提供程序*** 通过实现在作为应用的一部分引入的 UI 中显示元素的 API，向 UI 自动化树提供信息。 在创建新控件时，你现在的角色应该是 UI 自动化提供程序方案中的参与者。 作为提供程序的参与者，你应当确保所有 UI 自动化客户端可以针对辅助功能和测试用途，使用 UI 自动化框架与你的控件交互。

通常，在 UI 自动化框架中存在平行 API：一个 API 用于 UI 自动化客户端，另一个相似命名的 API 用于 UI 自动化提供程序。 本主题主要介绍了用于 UI 自动化提供程序的 API，尤其是用于支持提供程序在该 UI 框架中实现可扩展性的类和接口。 有时，我们会提到 UI 自动化客户端使用的 UI 自动化 API，用于提供某些视角，或提供将客户端和提供程序 API 相关联的查找表。 有关客户端视角的详细信息，请参阅 [UI 自动化客户端程序员指南](https://msdn.microsoft.com/library/windows/desktop/Ee684021)。

> [!NOTE]
> UI 自动化客户端通常不使用托管代码，也不作为 UWP 应用实现（常常是作为桌面应用实现）。 UI 自动化基于一种标准，而不是基于特定的实现或框架。 许多现有的 UI 自动化客户端（包括辅助技术产品，如屏幕阅读器）使用组件对象模型 (COM) 接口与 UI 自动化、系统和在子窗口中运行的应用进行交互。 有关 COM 接口以及如何使用 COM 编写 UI 自动化客户端的详细信息，请参阅 [UI 自动化基础](https://msdn.microsoft.com/library/windows/desktop/Ee684007)。

<span id="Determining_the_existing_state_of_UI_Automation_support_for_your_custom_UI_class"/>
<span id="determining_the_existing_state_of_ui_automation_support_for_your_custom_ui_class"/>
<span id="DETERMINING_THE_EXISTING_STATE_OF_UI_AUTOMATION_SUPPORT_FOR_YOUR_CUSTOM_UI_CLASS"/>

## <a name="determining-the-existing-state-of-ui-automation-support-for-your-custom-ui-class"></a>确定对于自定义 UI 类的现有 UI 自动化支持状态  
在尝试为自定义控件实现自动化对等之前，应测试基类及其自动化对等是否已经提供你所需的辅助功能或自动化支持。 在许多情况下，将 [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) 实现、特定的对等以及它们所实现的模式组合在一起可以提供一个基本但令人满意的辅助功能体验。 是否如此取决于你对暴露给控件的对象模型相对于其基类进行的更改量。 而且，这取决于你为基类功能添加的功能是否与模板合约中的新 UI 元素或者控件的视觉外观相关联。 在某些情况下，你的更改可能会在用户体验中引入新的、要求提供其他辅助功能支持的方面。

即使使用现有的基本对等类可以提供基本的辅助功能支持，定义对等仍然是最佳做法，以便你可以针对自动测试方案向 UI 自动化报告精确的 **ClassName** 信息。 如果你正在编写旨在供第三方使用的控件，则此注意事项尤为重要。

<span id="Automation_peer_classes__"/>
<span id="automation_peer_classes__"/>
<span id="AUTOMATION_PEER_CLASSES__"/>

## <a name="automation-peer-classes"></a>自动化对等类  
UWP 基于先前的托管代码 UI 框架（如 Windows 窗体、Windows Presentation Foundation (WPF) 和 Microsoft Silverlight）所使用的现有 UI 自动化技术与约定而构建。 许多控件类及其功能和用途也是源自先前的 UI 框架。

按照合约，对等类名称以控件类名称开头，以“AutomationPeer”结尾。 例如，[**ButtonAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242458) 是 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 控件类的对等类。

> [!NOTE]
> 在本主题中，当你实现控件对等时，我们将认为与辅助功能相关的属性更为重要。 但对于更普遍的 UI 自动化支持概念，你应该根据 [UI 自动化提供程序程序员指南](https://msdn.microsoft.com/library/windows/desktop/Ee671596)和 [UI 自动化基础](https://msdn.microsoft.com/library/windows/desktop/Ee684007)中描述的建议实现对等。 这些主题未涵盖你在 UWP 框架中用于为 UI 自动化提供信息的具体 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) API，但是它们介绍了用于标识你的类或者提供其他信息或交互操作的属性。

<span id="Peers__patterns_and_control_types"/>
<span id="peers__patterns_and_control_types"/>
<span id="PEERS__PATTERNS_AND_CONTROL_TYPES"/>

## <a name="peers-patterns-and-control-types"></a>对等、模式和控件类型  
*控件模式*是一种控件实现，用来向 UI 自动化客户端暴露控件功能的特定方面。 UI 自动化客户端使用通过控件模式暴露的属性和方法，检索有关控件功能的信息或者在运行时操作控件的行为。

控件模式提供一种分类和暴露控件功能的方式，与控件类型或控件外观无关。 例如，显示表格界面的控件使用 **Grid** 控件模式暴露表格中的行数和列数并允许 UI 自动化客户端从表格中检索项目。 以其他示例相同，UI 自动化客户端可以针对可调用的控件（如按钮）使用 **Invoke** 控件模式，针对具有滚动条的控件（如列表框、列表视图或组合框）使用 **Scroll** 控件模式。 每个控件模式代表一个单独的功能类型，并且可以结合使用多个控件模式来描述特定控件支持的全套功能。

控件模式与 UI 相关，而接口与 COM 对象相关。 在 COM 中，你可以查询对象以询问它支持什么接口，然后使用这些接口访问功能。 在 UI 自动化中，UI 自动化客户端可以查询 UI 自动化元素，确定它支持哪些控件模式，然后通过受支持的控件模式公开的属性、方法、事件和结构与该元素及其对等控件进行交互。

自动化对等的一个主要用途就是向 UI 自动化客户端报告该 UI 元素可以通过其对等元素支持哪些控件模式。 为此，UI 自动化提供程序通过替代 [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 方法来实现用于更改 [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) 方法行为的新对等。 UI 自动化客户端发出将 UI 自动化提供程序映射到调用 **GetPattern** 的调用。 UI 自动化客户端查询它们要与之交互的每个特定模式。 如果该对等支持所请求的模式，它将返回一个指向其本身的对象引用，否则它将返回 **null**。 如果返回的不是 **null**，则该 UI 自动化客户端会预期它能够将相应模式接口的 API 作为客户端来调用，以便与该控件模式进行交互。

*控件类型*是广泛定义该对等表示的控件功能的一种方式。 此概念与控件模式的概念不同，因为当某种模式通知 UI 自动化，它可以得到哪些信息或通过特定接口执行哪些操作时，控件类型比该模式高一个级别。 每个控件类型都有 UI 自动化的这些方面的指南：

* UI 自动化控件模式：一个控件类型可能支持多个模式，每个模式表示信息或交互的不同类别。 每个控件类型都具有一组该控件必须支持的控件模式（此组模式可选）和一组该控件不得支持的控件模式。
* UI 自动化属性值：每个控件类型都具有一组该控件必须支持的属性。 这些属性是常规属性（如 [UI 自动化属性概述](https://msdn.microsoft.com/library/windows/desktop/Ee671594)中所述），而不是特定于模式的属性。
* UI 自动化事件：每个控件类型都具有一组该控件必须支持的事件。 同样地，这些事件是常规事件，而不是特定于模式的事件，如 [UI 自动化事件概述](https://msdn.microsoft.com/library/windows/desktop/Ee671221)中所述。
* UI 自动化树结构：每个控件类型都将定义该控件必须显示在 UI 自动化树结构中的方式。

无论是否为框架实现了自动化对等，UI 自动化客户端功能都不绑定到 UWP，实际上，现有的 UI 自动化客户端（如辅助技术）将使用其他编程模型（如 COM）。 在 COM 中，客户端可以针对用来实现属性、事件或树检查的所请求模式或常规 UI 自动化框架的 COM 控件模式接口执行 **QueryInterface**。 对于这些模式，UI 自动化框架会将该接口代码封送到针对应用的 UI 自动化提供程序以及相关对等运行的 UWP 代码中。

当你为托管代码框架（例如使用 C\# 或 Microsoft Visual Basic 的 UWP 应用）实现控件模式时，你可以使用 .NET Framework 接口来表示这些模式，而不是使用 COM 接口表示。 例如，由 Microsoft .NET 实现的 **Invoke** 模式的 UI 自动化模式界面是 [**IInvokeProvider**](https://msdn.microsoft.com/library/windows/apps/BR242582)。

有关控件模式、提供程序接口及其用途的列表，请参阅[控件模式和接口](control-patterns-and-interfaces.md)。 有关控件类型的列表，请参阅 [UI 自动化控件类型概述](https://msdn.microsoft.com/library/windows/desktop/Ee671197)。

<span id="Guidance_for_how_to_implement_control_patterns"/>
<span id="guidance_for_how_to_implement_control_patterns"/>
<span id="GUIDANCE_FOR_HOW_TO_IMPLEMENT_CONTROL_PATTERNS"/>

### <a name="guidance-for-how-to-implement-control-patterns"></a>如何实现控件模式指南  
控件模式及其用途包含在 UI 自动化框架的较大定义中，不只适用于 UWP 应用的辅助功能支持。 实现控件模式时，应确保采用与 MSDN 上记录的指南匹配并位于 UI 自动化规范中的方式来实现它。 如果你正在查找指南，通常可以使用 MSDN 主题，无需参考该规范。 此处记录了有关每个模式的指南：[实现 UI 自动化控制模式](https://msdn.microsoft.com/library/windows/desktop/Ee671292)。 你会注意到此区域下方的每个主题都具有“实现指南和约定”部分以及“所需成员”部分。 本指南通常参考[适用于提供程序的控件模式接口](https://msdn.microsoft.com/library/windows/desktop/Ee671201)参考中的相关控件模式接口的特定 API。 这些接口是本机/COM 接口（其 API 使用 COM 样式的语法）。 你也可以在 [**Windows.UI.Xaml.Automation.Provider**](https://msdn.microsoft.com/library/windows/apps/BR209225) 命名空间中看到此处显示的所有内容。

如果你使用的是默认自动化对等并针对其行为进行扩展，则这些对等已遵照 UI 自动化指南进行编写。 如果它们支持控件模式，则你可以依赖符合[实现 UI 自动化控件模式](https://msdn.microsoft.com/library/windows/desktop/Ee671292)中的指南的这一模式支持。 如果控件对等报告它代表的控件类型由 UI 自动化定义，则该控件遵循的是在[支持 UI 自动化控件类型](https://msdn.microsoft.com/library/windows/desktop/Ee671633)上记录的指南。

但是，你可能还需要有关控件模式或控件类型的其他指南，以便在你的对等实现操作中遵循 UI 自动化建议。 这尤其适用于以下情况：你实现的模式或控件类型支持尚未作为 UWP 控件中的默认实现而存在。 例如，适用于批注的模式未在任何默认 XAML 控件中实现。 但是你可能具有一个广泛使用批注的应用，因此你可能需要可访问其功能的图面。 对于此应用场景，你的对等应该实现 [**IAnnotationProvider**](https://msdn.microsoft.com/library/windows/apps/Hh738493)，并且应该将自身报告为具有相应属性的 **Document** 控件类型，以指示你的文档支持批注。

我们建议你将在[实现 UI 自动化控件模式](https://msdn.microsoft.com/library/windows/desktop/Ee671292)下找到的有关模式或在[支持 UI 自动化控件类型](https://msdn.microsoft.com/library/windows/desktop/Ee671633)下找到的有关控件类型的指南用作入门和一般指南。 你甚至可以尝试通过某些 API 链接来获取有关 API 用途的说明和备注。 但是对于 UWP 应用编程所需的特定语法，请在 [**Windows.UI.Xaml.Automation.Provider**](https://msdn.microsoft.com/library/windows/apps/BR209225) 命名空间中查找等效 API，然后使用这些参考页面获取详细信息。

<span id="Built-in_automation_peer_classes"/>
<span id="built-in_automation_peer_classes"/>
<span id="BUILT-IN_AUTOMATION_PEER_CLASSES"/>

## <a name="built-in-automation-peer-classes"></a>内置的自动化对等类  
通常，如果元素接受来自用户的 UI 活动，或者元素中包含辅助技术（表示应用的交互或有意义 UI）的用户所需的信息，则元素将实现自动化对等类。 并非所有的 UWP 视觉元素都有自动化对等。 实现自动化对等的类的示例包括 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 和 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)。 不实现自动化对等的类的示例包括 [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209250) 和基于 [**Panel**](https://msdn.microsoft.com/library/windows/apps/BR227511) 的类（如 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 和 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/BR209267)）。 **Panel** 没有对等，因为它提供仅可见的布局行为。 用户无法通过任何辅助功能相关方法与 **Panel** 交互。 **Panel** 包含的任何子元素会转而作为树中下一个具有对等或元素表示形式的可用父元素的子元素，报告给 UI 自动化树。

<span id="UI_Automation_and_UWP_process_boundaries"/>
<span id="ui_automation_and_uwp_process_boundaries"/>
<span id="UI_AUTOMATION_AND_UWP_PROCESS_BOUNDARIES"/>

## <a name="ui-automation-and-uwp-process-boundaries"></a>UI 自动化和 UWP 进程边界  
通常，访问 UWP 应用的 UI 自动化客户端代码在进程外运行。 UI 自动化框架基础结构使信息能够跨进程边界。 此概念在 [UI 自动化基础](https://msdn.microsoft.com/library/windows/desktop/Ee684007)中进行详细介绍。

<span id="OnCreateAutomationPeer"/>
<span id="oncreateautomationpeer"/>
<span id="ONCREATEAUTOMATIONPEER"/>

## <a name="oncreateautomationpeer"></a>OnCreateAutomationPeer  
所有派生自 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) 的类都包含受保护的虚拟方法 [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer)。 自动化对等的对象初始化序列调用 **OnCreateAutomationPeer** 以获取每个控件的自动化对等对象，从而构造一个 UI 自动化树以供运行时使用。 UI 自动化代码可以使用对等来获取有关控件的特征和功能的信息并借助控件的控件模式来模拟交互用法。 支持自动化的自定义控件必须替代 **OnCreateAutomationPeer** 并返回从 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 派生的类的实例。 例如，如果自定义控件派生自 [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/BR227736) 类，则 **OnCreateAutomationPeer** 返回的对象应当派生自 [**ButtonBaseAutomationPeer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.automation.peers.buttonbaseautomationpeer)。

如果要编写自定义控件类而且打算还提供一个新的自动化对等，则应当替代自定义控件的 [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) 方法，以便它返回对等的新实例。 你的对等类必须从 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 直接或间接派生。

例如，下面的代码声明自定义控件 `NumericUpDown` 应当使用对等 `NumericUpDownPeer` 来实现 UI 自动化用途。

```csharp
using Windows.UI.Xaml.Automation.Peers;
...
public class NumericUpDown : RangeBase {
    public NumericUpDown() {
    // other initialization; DefaultStyleKey etc.
    }
    ...
    protected override AutomationPeer OnCreateAutomationPeer()
    {
        return new NumericUpDownAutomationPeer(this);
    }
}
```

```vb
Public Class NumericUpDown
    Inherits RangeBase
    ' other initialization; DefaultStyleKey etc.
       Public Sub New()
       End Sub
       Protected Overrides Function OnCreateAutomationPeer() As AutomationPeer
              Return New NumericUpDownAutomationPeer(Me)
       End Function
End Class
```

```cppwinrt
// NumericUpDown.idl
namespace MyNamespace
{
    runtimeclass NumericUpDown : Windows.UI.Xaml.Controls.Primitives.RangeBase
    {
        NumericUpDown();
        Int32 MyProperty;
    }
}

// NumericUpDown.h
...
struct NumericUpDown : NumericUpDownT<NumericUpDown>
{
    ...
    Windows::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer()
    {
        return winrt::make<MyNamespace::implementation::NumericUpDownAutomationPeer>(*this);
    }
};
```

```cpp
//.h
public ref class NumericUpDown sealed : Windows::UI::Xaml::Controls::Primitives::RangeBase
{
// other initialization not shown
protected:
    virtual AutomationPeer^ OnCreateAutomationPeer() override
    {
         return ref new NumericUpDownAutomationPeer(this);
    }
};
```

> [!NOTE]
> [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) 实现应指示初始化自定义自动化对等的新实例、以所有者身份传递调用控件并返回该实例，而不是执行任何其他操作。 请勿尝试此方法中的其他逻辑。 特别是，可能会导致损坏同一调用中 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 的任何逻辑可能会产生意外的运行时行为。

在典型的 [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) 实现中，*owner* 指定为 **this** 或 **Me**，因为此方法替代与控件类定义的其余部分具有相同的作用域。

实际的对等类定义可以在与控件相同的代码文件中完成，也可以在与控件不同的代码文件中完成。 对等定义都存在于 [**Windows.UI.Xaml.Automation.Peers**](https://msdn.microsoft.com/library/windows/apps/BR242563) 命名空间中，该命名空间独立于这些定义为其提供对等的控件。 你也可以选择在另外的命名空间中声明对等，但前提是你为 [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) 方法调用引用必需的命名空间。

<span id="Choosing_the_correct_peer_base_class"/>
<span id="choosing_the_correct_peer_base_class"/>
<span id="CHOOSING_THE_CORRECT_PEER_BASE_CLASS"/>

### <a name="choosing-the-correct-peer-base-class"></a>选择正确的对等基类  
请确保你的 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 派生自一个基类，该基类为要作为派生来源的控件类的现有对等逻辑提供最佳匹配。 在上例情况下，由于 `NumericUpDown` 派生自 [**RangeBase**](https://msdn.microsoft.com/library/windows/apps/BR227863)，所以有一个 [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506) 类可用，应将对等基于该类。 通过使用与你派生控件本身的方式最接近匹配的对等类，你可以至少避免替代某些 [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) 功能，因为基本对等类已经实现它。

基本 [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) 类没有相应的对等类。 如果你需要使对等类与派生自 **Control** 的自定义控件相对应，请从 [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) 派生自定义对等类。

如果直接派生自 [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365)，则该类将没有默认的自动化对等行为，因为没有引用对等类的 [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) 实现。 因此，请确保要么实现 **OnCreateAutomationPeer** 以使用自己的对等，要么使用 [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) 作为对等（前提是该级别的辅助功能支持对于你的控件是足够的）。

> [!NOTE]
> 通常应从 [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) 进行派生，而不是 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185)。 如果直接从 **AutomationPeer** 进行派生，你将需要复制本可以由 **FrameworkElementAutomationPeer** 提供的大量辅助功能基本支持。

<span id="Initialization_of_a_custom_peer_class"/>
<span id="initialization_of_a_custom_peer_class"/>
<span id="INITIALIZATION_OF_A_CUSTOM_PEER_CLASS"/>

## <a name="initialization-of-a-custom-peer-class"></a>自定义对等类的初始化  
自定义对等应当定义一个类型安全构造函数，该构造函数使用所有者控件的实例来进行基本初始化。 在下一个示例中，该实现将 *owner* 值传递给 [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506) 基类，但最终将是 [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) 实际使用 *owner* 设置 [**FrameworkElementAutomationPeer.Owner**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner)。

```csharp
public NumericUpDownAutomationPeer(NumericUpDown owner): base(owner)
{}
```

```vb
Public Sub New(owner As NumericUpDown)
    MyBase.New(owner)
End Sub
```

```cppwinrt
// NumericUpDownAutomationPeer.idl
import "NumericUpDown.idl";
namespace MyNamespace
{
    runtimeclass NumericUpDownAutomationPeer : Windows.UI.Xaml.Automation.Peers.AutomationPeer
    {
        NumericUpDownAutomationPeer(NumericUpDown owner);
        Int32 MyProperty;
    }
}

// NumericUpDownAutomationPeer.h
...
struct NumericUpDownAutomationPeer : NumericUpDownAutomationPeerT<NumericUpDownAutomationPeer>
{
    ...
    NumericUpDownAutomationPeer(MyNamespace::NumericUpDown const& owner);
};
```

```cpp
//.h
public ref class NumericUpDownAutomationPeer sealed :  Windows::UI::Xaml::Automation::Peers::RangeBaseAutomationPeer
//.cpp
public:    NumericUpDownAutomationPeer(NumericUpDown^ owner);
```

<span id="Core_methods_of_AutomationPeer"/>
<span id="core_methods_of_automationpeer"/>
<span id="CORE_METHODS_OF_AUTOMATIONPEER"/>

## <a name="core-methods-of-automationpeer"></a>AutomationPeer 的核心方法  
出于 UWP 基础结构的原因，自动化对等的可替代方法属于下面的两个方法：UI 自动化提供程序用作 UI 自动化客户端转发点的公共访问方法；UWP 类可以替代以影响行为的受保护的“Core”自定义方法。 这两个方法在默认情况下绑定在一起，方法是在调用访问方法时始终调用并行的、具有提供程序实现的“Core”方法，或者从基类调用默认实现来进行回退。

在为自定义控件实现对等时，替代自动化对等基类中你希望展示你的自定义控件所独有行为的所有“Core”方法。 UI 自动化代码通过调用对等类的公共方法来获取有关你的控件的信息。 要提供有关你的控件的信息，请在你的控件实现和设计创建不同于自动化对等基类所支持方案的辅助功能方案或其他 UI 自动化方案时，替代名称结尾为“Core”的每个方法。

至少，只要你定义新的对等类，均实现 [**GetClassNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore) 方法，如下一个示例所示。

```csharp
protected override string GetClassNameCore()
{
    return "NumericUpDown";
}
```

> [!NOTE]
> 你可能希望将字符串存储为常量，而不是直接存储在方法正文中，但具体如何存储取决于你。 对于 [**GetClassNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore)，你将不希望对该字符串进行本地化。 只要 UI 自动化客户端需要使用本地化的字符串，就会使用 **LocalizedControlType** 属性，而不是 **ClassName**。

### <span id="GetAutomationControlType"/>
<span id="getautomationcontroltype"/>
<span id="GETAUTOMATIONCONTROLTYPE"/>GetAutomationControlType

一些辅助技术在将 UI 自动化树中的项目特征报告为 UI 自动化 **Name** 之外的额外信息时，直接使用 [**GetAutomationControlType**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltype) 值。 如果你的控件明显不同于你正派生自的控件，并且你希望报告与控件使用的基本对等类所报告不同的控件类型，则必须实现对等并在你的对等实现中替代 [**GetAutomationControlTypeCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore)。 如果你派生自诸如 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) 或 [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365) 等一般基类（基本对等不提供有关控件类型的精确信息），这尤为重要。

你的 [**GetAutomationControlTypeCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore) 实现通过返回 [**AutomationControlType**](https://msdn.microsoft.com/library/windows/apps/BR209182) 值来描述控件。 尽管你可以返回 **AutomationControlType.Custom**，但是你应当返回一个更具体的控件类型，但前提是该类型能够准确地描述控件的主要情形。 下面提供了一个示例。

```csharp
protected override AutomationControlType GetAutomationControlTypeCore()
{
    return AutomationControlType.Spinner;
}
```

> [!NOTE]
> 除非你指定了 [**AutomationControlType.Custom**](https://msdn.microsoft.com/library/windows/apps/BR209182)，否则不必为了向客户端提供 **LocalizedControlType** 属性值而实现 [**GetLocalizedControlTypeCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlocalizedcontroltypecore)。 UI 自动化公共基础结构为每个可能的 **AutomationControlType** 值提供经过翻译的字符串，而不是 **AutomationControlType.Custom**。

<span id="GetPattern_and_GetPatternCore"/>
<span id="getpattern_and_getpatterncore"/>
<span id="GETPATTERN_AND_GETPATTERNCORE"/>

### <a name="getpattern-and-getpatterncore"></a>GetPattern 和 GetPatternCore  
对等中实现的 [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 所返回的对象支持在输入参数中请求的模式。 具体来说，UI 自动化客户端调用一个转发到提供程序的 [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) 方法的方法，并指定用来对所请求的模式进行命名的 [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496) 枚举值。 你替代的 **GetPatternCore** 应当返回一个实现特定模式的对象。 该对象就是对等本身，因为对等在任何时候报告它支持某个模式时，应实现相应的模式接口。 如果你的对等没有对某个模式的自定义实现，但是你知道对等的基类一定会实现该模式，则你可以从 **GetPatternCore** 调用基本类型的 **GetPatternCore** 实现。 如果对等不支持某个模式，则该对等的 **GetPatternCore** 应返回 **null**。 但是，这并不是直接从你的实现返回 **null**，而通常是通过调用基本实现来为任何不受支持的模式返回 **null**。

如果某个模式受支持，则 [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 实现会返回 **this** 或 **Me**。 预期结果是 UI 自动化客户端将任何不为 **null** 的 [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) 返回值转换为所请求的模式接口。

如果某个对等类继承自另一个对等，而且所有必需的支持和模式报告已经由基类处理，则不必实现 [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore)。 例如，如果要实现一个派生自 [**RangeBase**](https://msdn.microsoft.com/library/windows/apps/BR227863) 的范围控件，而且你的对等派生自 [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506)，则该对等将针对 [**PatternInterface.RangeValue**](https://msdn.microsoft.com/library/windows/apps/BR242496) 返回其本身，而且有一个能够正常工作的 [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) 接口实现支持此模式。

尽管不是逐字代码，但是此示例与 [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506) 中已经存在的 [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 的实现近似。


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    return base.GetPattern(patternInterface);
}
```

如果要实现的对等中不具备对等基类中所需的全部支持，或者你希望更改从基类继承而且受你的对等支持的模式集，则应当替代 [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 以允许 UI 自动化客户端使用这些模式。

有关 UI 自动化支持的 UWP 实现中可用提供程序模式的列表，请参阅 [**Windows.UI.Xaml.Automation.Provider**](https://msdn.microsoft.com/library/windows/apps/BR209225)。 每个类似的模式都有一个相应的 [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496) 枚举值，该值说明 UI 自动化客户端如何在 [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) 调用中请求模式。

一个对等可以报告它支持多个模式。 如果是这样，该替代应当包括每个受支持的 [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496) 值的返回路径逻辑，而且应当在每个匹配情况下返回相应的对等。 预期结果是，调用方一次仅请求一个接口，而且由调用方负责转换为预期的接口。

下面是自定义对等的 [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 替代示例。 它报告它支持两个模式：[**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) 和 [**IToggleProvider**](https://msdn.microsoft.com/library/windows/apps/BR242653)。 此处的控件是一个媒体显示控件，它可以显示为全屏（切换模式），而且具有一个进度条（范围控件），用户可以在该进度条中选择位置。 此代码源自 [XAML 辅助功能示例](http://go.microsoft.com/fwlink/p/?linkid=238570)。


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    else if (patternInterface == PatternInterface.Toggle)
    {
        return this;
    }
    return null;
}
```

<span id="Forwarding_patterns_from_sub-elements"/>
<span id="forwarding_patterns_from_sub-elements"/>
<span id="FORWARDING_PATTERNS_FROM_sub-elementS"/>

### <a name="forwarding-patterns-from-sub-elements"></a>从子元素转发模式  
[**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 方法实现还可以将某个子元素或部分指定为其主机的模式提供程序。 此示例模拟说明 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) 如何将滚动模式处理转移到其内部 [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/BR209527) 控件的对等。 若要为模式处理指定子元素，此代码将获取子元素对象、使用 [**FrameworkElementAutomationPeer.CreatePeerForElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.createpeerforelement) 方法为该子元素创建一个对等，然后返回新对等。


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.Scroll)
    {
        ItemsControl owner = (ItemsControl) base.Owner;
        UIElement itemsHost = owner.ItemsHost;
        ScrollViewer element = null;
        while (itemsHost != owner)
        {
            itemsHost = VisualTreeHelper.GetParent(itemsHost) as UIElement;
            element = itemsHost as ScrollViewer;
            if (element != null)
            {
                break;
            }
        }
        if (element != null)
        {
            AutomationPeer peer = FrameworkElementAutomationPeer.CreatePeerForElement(element);
            if ((peer != null) && (peer is IScrollProvider))
            {
                return (IScrollProvider) peer;
            }
        }
    }
    return base.GetPatternCore(patternInterface);
}
```

<span id="Other_Core_methods"/>
<span id="other_core_methods"/>
<span id="OTHER_CORE_METHODS"/>

### <a name="other-core-methods"></a>其他核心方法  
你的控件可能需要为主要方案支持键盘等效功能；有关为什么可能必须支持键盘等效功能的详细信息，请参阅[键盘辅助功能](keyboard-accessibility.md)。 实现键盘支持必定属于控件代码而不是对等代码，因为这是控件逻辑的一部分，但是你的对等类应替代 [**GetAcceleratorKeyCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getacceleratorkeycore) 和 [**GetAccessKeyCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getaccesskeycore) 方法以便向使用键盘的 UI 自动化客户端报告。 请考虑，报告键盘信息的字符串可能需要进行本地化，因此应当来自资源（而非硬编码字符串）。

如果你为支持集合的类提供对等，最好从已经支持这类集合的函数类和对等类派生。 如果无法这样做，则用来维护子集合的控件的对等可能必须替代与父集合相关的对等方法 [**GetChildrenCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getchildrencore) 才能向 UI 自动化树正确地报告父-子关系。

实现 [**IsContentElementCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.iscontentelementcore) 和 [**IsControlElementCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.iscontrolelementcore) 方法以指示你的控件是否包含数据内容或者/而且是否满足用户界面中的交互角色。 默认情况下，这两种方法都返回 **true**。 这些设置可改进辅助技术（如屏幕阅读器）的可用性，辅助技术可能会使用这些方法筛选自动化树。 如果你的 [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 方法将模式处理转移到子元素对等，则子元素对等的 **IsControlElementCore** 方法可能返回 **false**，以便在自动化树中隐藏子元素对等。

某些控件可能支持标签方案，其中文本标签部分为非文本部分提供信息，或者控件旨在与 UI 中的另一个控件保持一种已知的标签关系。 如果可以提供一种基于类的有用行为，你可以替代 [**GetLabeledByCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) 以提供此行为。

[**GetBoundingRectangleCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore) 和 [**GetClickablePointCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore) 主要用于自动化测试方案。 如果你希望支持控件的自动化测试，则可能希望替代这些方法。 范围类型控件可能需要这样做，在这样的控件中，你不能仅建议单个点，因为用户在坐标空间中的单击位置会对范围产生一个不同的影响。 例如，默认的 [**ScrollBar**](https://msdn.microsoft.com/library/windows/apps/BR209745) 自动化对等会替代 **GetClickablePointCore**，以返回“非数字”[**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 值。

[**GetLiveSettingCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlivesettingcore) 影响控件的 UI 自动化的 **LiveSetting** 默认值。 如果你希望你的控件返回 [**AutomationLiveSetting.Off**](https://msdn.microsoft.com/library/windows/apps/JJ191519) 以外的值，则可能希望替代此值。 有关 **LiveSetting** 所表示的含义的详细信息，请参阅 [**AutomationProperties.LiveSetting**](https://msdn.microsoft.com/library/windows/apps/JJ191516)。

如果你的控件具有一个可设置的而且可以映射到 [**AutomationOrientation**](https://msdn.microsoft.com/library/windows/apps/BR209184) 的方向属性，则可以替代 [**GetOrientationCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getorientationcore)。 [**ScrollBarAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242522) 和 [**SliderAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242546) 类执行此类操作。

<span id="Base_implementation_in_FrameworkElementAutomationPeer"/>
<span id="base_implementation_in_frameworkelementautomationpeer"/>
<span id="BASE_IMPLEMENTATION_IN_FRAMEWORKELEMENTAUTOMATIONPEER"/>

### <a name="base-implementation-in-frameworkelementautomationpeer"></a>FrameworkElementAutomationPeer 中的基本实现  
[**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) 的基本实现提供一些 UI 自动化信息，可从在框架级别定义的各种布局和行为属性中解释这些信息。

* [**GetBoundingRectangleCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore)：根据已知的布局特征返回 [**Rect**](https://msdn.microsoft.com/library/windows/apps/BR225994) 结构。 如果 [**IsOffscreen**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isoffscreen) 为 **true**，则返回 0 值 **Rect**。
* [**GetClickablePointCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore)：根据已知的布局特征返回 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 结构，但前提是 **BoundingRectangle** 非零。
* [**GetNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getnamecore)：此处可以汇总更广泛的行为；请参阅 [**GetNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getnamecore)。 基本上，它会尝试对 [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365) 的任何已知内容或具有内容的相关类进行字符串转换。 另外，如果 [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) 有一个值，则会将该项的 **Name** 值用作 **Name**。
* [**HasKeyboardFocusCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.haskeyboardfocuscore)：根据所有者的 [**FocusState**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.focusstate) 和 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled) 属性求值。 不是控件的元素始终返回 **false**。
* [**IsEnabledCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isenabledcore)：根据所有者的 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled) 属性求值（如果是一个 [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390)）。 不是控件的元素始终返回 **true**。 这并不意味着已在传统的交互意义上启用所有者；它表示已启用对等，即使所有者不具有 **IsEnabled** 属性。
* [**IsKeyboardFocusableCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.iskeyboardfocusablecore)：在所有者是 [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) 时返回 **true**，否则返回 **false**。
* [**IsOffscreenCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isoffscreencore)：所有者元素或其任何父项上 [**Collapsed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visibility) 的 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility) 针对 [**IsOffscreen**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isoffscreen) 等于 **true** 值。 例外：即使 [**Popup**](https://msdn.microsoft.com/library/windows/apps/BR227842) 对象所有者的父项不可见，该对象也可见。
* [**SetFocusCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.setfocuscore)：调用 [**Focus**](https://msdn.microsoft.com/library/windows/apps/hh702161)。
* [**GetParent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getparent)：从所有者调用 [**FrameworkElement.Parent**](https://msdn.microsoft.com/library/windows/apps/BR208739)，并查找相应的对等。 这并不是对“Core”方法的替代配对，因此你无法更改此行为。

> [!NOTE]
> 默认 UWP 对等通过使用实现 UWP 所需的内部本机代码来实现某个行为，而不必使用实际的 UWP 代码。 你不能通过常见语言运行时 (CLR) 反射或其他技术查看实现的代码或逻辑。 你也无法查看不同引用页来了解基本对等行为的子类特定替代。 例如，[**TextBoxAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242550) 的 [**GetNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getnamecore) 可能有其他行为，这些行为没有在 **AutomationPeer.GetNameCore** 引用页中进行描述，并且没有 **TextBoxAutomationPeer.GetNameCore** 的引用页。 甚至没有 **TextBoxAutomationPeer.GetNameCore** 引用页。 因此，请阅读最接近的对等类的参考主题，并在“备注”部分查找实现说明。

<span id="Peers_and_AutomationProperties"/>
<span id="peers_and_automationproperties"/>
<span id="PEERS_AND_AUTOMATIONPROPERTIES"/>

## <a name="peers-and-automationproperties"></a>对等和 AutomationProperties  
自动化对等应当为控件的辅助功能相关信息提供合适的默认值。 请注意，使用控件的任何应用代码都可以通过在控件实例上包括 [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081) 附加属性值来替代该行为的一部分。 调用方既可以针对默认控件也可以针对自定义控件执行此操作。 例如，下面的 XAML 创建一个按钮，该按钮具有两个自定义的 UI 自动化属性： `<Button AutomationProperties.Name="Special"      AutomationProperties.HelpText="This is a special button."/>`

有关 [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081) 附加属性的详细信息，请参阅[基本辅助功能信息](basic-accessibility-information.md)。

由于制定了有关 UI 自动化提供程序应如何报告信息的一般合约，因此会存在一些 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 方法，但这些方法通常不在控件对等中实现。 这是因为该信息应当由 [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081) 值提供，而且这些值应用到在特定 UI 中使用控件的应用代码。 例如，大多数应用会通过应用 [**AutomationProperties.LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) 值在 UI 中定义两个不同控件之间的标签关系。 但是，[**LabeledByCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) 在某些代表控件中数据或项目关系的对等中实现，例如，使用标头部分对数据字段部分进行标记、使用项目容器对项目进行标记或者执行类似的方案。

<span id="Implementing_patterns"/>
<span id="implementing_patterns"/>
<span id="IMPLEMENTING_PATTERNS"/>

## <a name="implementing-patterns"></a>实现模式  
让我们看一下如何通过实现展开折叠的控件模式接口，为用来实现展开折叠行为的控件编写对等。 每当用值 [**PatternInterface.ExpandCollapse**](https://msdn.microsoft.com/library/windows/apps/BR242496) 调用 [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) 时，该对等应当能够通过返回其自身来针对展开折叠行为启用辅助功能。 然后，该对等应当继承此模式的提供程序接口 ([**IExpandCollapseProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider))，并为该提供程序接口的每个成员提供实现。 在这种情况下，该接口有三个需要替代的成员：[**Expand**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expand)、[**Collapse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider.collapse) 和 [**ExpandCollapseState**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expandcollapsestate)。

在该类本身的 API 设计中提前为辅助功能进行规划非常有用。 只要存在一个行为可能会由于与在 UI 中工作的用户进行典型交互或者通过自动化提供程序模式进行请求，就需要提供一个能够由 UI 响应或由自动化模式调用的方法。 例如，如果你的控件有按钮部分而且该部分绑定了可展开或折叠该控件的事件处理程序，并且你的控件中还包含这些操作的键盘等效功能，请让这些事件处理程序调用你从该对等中 [**IExpandCollapseProvider**](https://msdn.microsoft.com/library/windows/desktop/Ee671242) 的 [**Expand**](https://msdn.microsoft.com/library/windows/apps/BR242570) 或 [**Collapse**](https://msdn.microsoft.com/library/windows/apps/BR242569) 实现的主体中调用的相同方法。 为了确保控件的视觉状态进行更新以按照统一的方式显示逻辑状态，而不考虑行为的调用方式，使用常见的逻辑方法也可能非常有用。

典型实现是，提供程序 API 首先调用 [**Owner**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner) 以在运行时访问控件实例。 然后，可以在该对象上调用必要的行为方法。


```csharp
public class IndexCardAutomationPeer : FrameworkElementAutomationPeer, IExpandCollapseProvider {
    private IndexCard ownerIndexCard;
    public IndexCardAutomationPeer(IndexCard owner) : base(owner)
    {
         ownerIndexCard = owner;
    }
}
```

备用实现是指控件本身可以引用其对等。 这在从控件引发自动化事件时是常见的模式，因为 [**RaiseAutomationEvent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent) 方法就是对等方法。

<span id="UI_Automation_events"/>
<span id="ui_automation_events"/>
<span id="UI_AUTOMATION_EVENTS"/>

## <a name="ui-automation-events"></a>UI 自动化事件  

UI 自动化事件属于以下类别。

| 事件 | 说明 |
|-------|-------------|
| 属性更改 | 当 UI 自动化元素或控件模式上的属性发生更改时触发。 例如，如果客户端需要监视应用的复选框控件，它可以注册侦听 [**ToggleState**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itoggleprovider.togglestate) 属性上的属性更改事件。 当选中或取消选中复选框控件时，提供程序将触发该事件，然后客户端可以根据需要进行操作。 |
| 元素操作 | 当 UI 中的更改是由于用户或编程活动引起时触发；例如，当通过 **Invoke** 模式单击或调用按钮时。 |
| 结构更改 | 当 UI 自动化树的结构发生更改时触发。 当桌面上有新 UI 项变得可见、隐藏或删除时，结果便发生更改。 |
| 全局更改 | 当全局关注客户端的操作发生时触发，例如当焦点从一个元素移动到另一个元素时，或者关闭子窗口时。 某些事件不一定表示 UI 的状态已发生更改。 例如，如果用户用 Tab 键切换到文本输入字段，然后单击某个按钮以更新该字段，将触发 [**TextChanged**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.textchanged) 事件，即使用户实际上并未更改文本。 在处理事件时，客户端应用程序在采取操作之前可能有必要检查是否已实际发生任何更改。 |

<span id="AutomationEvents_identifiers"/>
<span id="automationevents_identifiers"/>
<span id="AUTOMATIONEVENTS_IDENTIFIERS"/>

### <a name="automationevents-identifiers"></a>AutomationEvents 标识符  
UI 自动化事件通过 [**AutomationEvents**](https://msdn.microsoft.com/library/windows/apps/BR209183) 值标识。 枚举值将唯一标识事件种类。

<span id="Raising_events"/>
<span id="raising_events"/>
<span id="RAISING_EVENTS"/>

### <a name="raising-events"></a>引发事件  
UI 自动化客户端可以订阅自动化事件。 在自动化对等模型中，自定义控件的对等必须通过调用 [**RaiseAutomationEvent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent) 方法，报告对控件进行的、与辅助功能相关的更改。 同样，当某个键的 UI 自动化属性值发生更改时，自定义控件对等应当调用 [**RaisePropertyChangedEvent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.raisepropertychangedevent) 方法。

接下来的代码示例显示如何从控件定义代码中获取对等对象，以及如何调用用来从该对等触发事件的方法。 最佳方法是由代码确定对于该事件类型是否存在任何侦听器。 仅当存在侦听器时才触发事件和创建对等对象可避免不必要的开销，并帮助使控件保持响应状态。


```csharp
if (AutomationPeer.ListenerExists(AutomationEvents.PropertyChanged))
{
    NumericUpDownAutomationPeer peer =
        FrameworkElementAutomationPeer.FromElement(nudCtrl) as NumericUpDownAutomationPeer;
    if (peer != null)
    {
        peer.RaisePropertyChangedEvent(
            RangeValuePatternIdentifiers.ValueProperty,
            (double)oldValue,
            (double)newValue);
    }
}
```

<span id="Peer_navigation"/>
<span id="peer_navigation"/>
<span id="PEER_NAVIGATION"/>

## <a name="peer-navigation"></a>对等导航  
在找到自动化对等之后，UI 自动化客户端可以通过调用该对等对象的 [**GetChildren**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getchildren) 和 [**GetParent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getparent) 方法来在应用的对等结构中进行导航。 可通过在对等中实现 [**GetChildrenCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getchildrencore) 方法来支持在控件中的各个 UI 元素之间进行导航。 UI 自动化系统调用此方法以生成一个由控件中包含的子元素（如列表框中的列表项）组成的树。 [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) 中的默认 **GetChildrenCore** 方法会遍历元素的视觉树以生成自动化对等树。 自定义控件可以替代此方法以便向自动化客户端暴露子元素的不同表示形式，并返回可传达信息或允许用户交互的元素的自动化对等。

<span id="Native_automation_support_for_text_patterns"/>
<span id="native_automation_support_for_text_patterns"/>
<span id="NATIVE_AUTOMATION_SUPPORT_FOR_TEXT_PATTERNS"/>

## <a name="native-automation-support-for-text-patterns"></a>对文本模式的本机自动支持  
某些默认的 UWP 应用自动化对等提供对文本模式的控件模式支持 ([**PatternInterface.Text**](https://msdn.microsoft.com/library/windows/apps/BR242496))。 但是它们通过本机方法提供这一支持，所涉及的对等不会在（托管）继承中注意到 [**ITextProvider**](https://msdn.microsoft.com/library/windows/apps/BR242627) 接口。 如果托管或非托管 UI 自动化客户端查询对等的模式，则它将报告对文本模式的支持，当调用客户端 API 时，它还提供模式各个部分的行为。

如果你打算从某一个 UWP 应用文本控件派生自定义对等，并创建派生自某一个文本关联对等的自定义对等，请检查对等的“备注”部分，以了解有关模式的任何本机级别支持的详细信息。 如果你从托管提供程序接口实现调用基本实现，则可在自定义对等中访问本机基本行为，但很难修改基本实现的操作，因为对等及其所有者控件上的本机接口没有公开。 通常，你应该按原样使用基本实现（仅调用基本实现），或者使用自己的托管代码来完全替换功能，而不调用基本实现。 后者是一个高级应用场景，你将需要很好地熟悉由你的控件使用的文本服务框架，以便在使用该框架时支持辅助功能要求。

<span id="AutomationProperties.AccessibilityView"/>
<span id="automationproperties.accessibilityview"/>
<span id="AUTOMATIONPROPERTIES.ACCESSIBILITYVIEW"/>

## <a name="automationpropertiesaccessibilityview"></a>AutomationProperties.AccessibilityView  
除提供自定义对等外，你还可以通过在 XAML 中设置 [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.accessibilityview) 来调整任何控件实例的树视图表示。 此过程不会作为对等类的一部分实现，但由于它与自定义控件或自定义模板的辅助功能整体支持密切相关，我们将在此处提及它。

使用 **AutomationProperties.AccessibilityView** 的主要方案有意省略 UI 自动化视图模板中的某些控件，因为它们对整个控件的辅助功能视图并无实质作用。 为避免发生此类情况，请将 **AutomationProperties.AccessibilityView** 设置为“Raw”。

<span id="Throwing_exceptions_from_automation_peers"/>
<span id="throwing_exceptions_from_automation_peers"/>
<span id="THROWING_EXCEPTIONS_FROM_AUTOMATION_PEERS"/>

## <a name="throwing-exceptions-from-automation-peers"></a>从自动化对等中引发异常  
你为自动化对等支持实现的 API 会引发异常。 正常情况下，侦听的任何 UI 自动化客户端都足够强大，能够在引发大多数异常后继续运行。 该侦听器很可能正在查找包含应用而非你自身的全方位自动化树。如果仅仅因为树的某个区域在客户端调用其 API 时引发了基于对等的异常就关闭整个客户端，这样的客户端设计是不可接受的。

对于传递到对等中的参数，可使用它来验证输入，例如，如果传递了 **null**，会引发 [**ArgumentNullException**](https://msdn.microsoft.com/library/windows/apps/system.argumentnullexception)，并且对于你的实现，该参数不是有效值。 但是，如果你的对等执行了后续操作，请记住对等与托管控件的交互将向其传入某些异步字符。 对等执行的任何操作不一定会阻止控件中的 UI 线程（而且它可能不应该执行此操作）。 因此，当创建了对等或首次调用了自动化对等方法时，会出现以下情况：对象可用或具有某些属性，但同时控件状态也会发生更改。 在这些情况下，提供程序会引发两种专门的异常：

* 如果你无法访问对等所有者或相关对等元素，则会引发 [**ElementNotAvailableException**](https://msdn.microsoft.com/library/system.windows.automation.elementnotavailableexception)，具体取决于你的 API 传递的原始信息。 例如，你的对等可能会尝试运行其方法，但是所有者自此从 UI 中删除，已关闭的模式对话框就是这样的例子。 对于非 .NET 客户端，该对等将映射到 [**UIA\_E\_ELEMENTNOTAVAILABLE**](https://msdn.microsoft.com/library/windows/desktop/Ee671218)。
* 如果仍然具有所有者，但该所有者处于诸如 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled)`=`**false** 的模式下，则会引发 [**ElementNotEnabledException**](https://msdn.microsoft.com/library/system.windows.automation.elementnotenabledexception)，该模式会阻止对等正在尝试完成的某些特定编程更改。 对于非 .NET 客户端，该对等将映射到 [**UIA\_E\_ELEMENTNOTENABLED**](https://msdn.microsoft.com/library/windows/desktop/Ee671218)。

除此之外，对等还应该是它们从其对等支持中引发的相对保守的相关异常。 大多数客户端无法处理对等中的异常，也无法将其转换为当与客户端交互时其用户可以选择的可操作选项。 因此，与在每次对等尝试执行的某些操作不起作用时即引发异常相比，有时 no-op 以及捕获异常（不会在对等实现中重新引发异常）是一个较好的策略。 同时还需考虑到，大多数 UI 自动化客户端均不会采用托管代码编写。 大多数客户端将采用 COM 编写，并仅用于在调用最终会访问对等的 UI 自动化客户端方法时，在 **HRESULT** 中检查 **S\_OK**。

<span id="related_topics"/>

## <a name="related-topics"></a>相关主题  
* [辅助功能](accessibility.md)
* [XAML 辅助功能示例](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472)
* [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185)
* [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer)
* [控件模式和接口](control-patterns-and-interfaces.md)
