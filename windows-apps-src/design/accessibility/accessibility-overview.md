---
Description: This article is an overview of the concepts and technologies related to accessibility scenarios for Universal Windows Platform (UWP) apps.
ms.assetid: AA053196-F331-4CBE-B032-4E9CBEAC699C
title: 辅助功能概述
label: Accessibility overview
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 50e6c68841440120b783713ef0a591e39a7c7eec
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8886259"
---
# <a name="accessibility-overview"></a>辅助功能概述  




本文概述了与通用 Windows 平台 (UWP) 应用的辅助功能方案相关的概念和技术。

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility/player]

<span id="Accessibility_and_your_app"/>
<span id="accessibility_and_your_app"/>
<span id="ACCESSIBILITY_AND_YOUR_APP"/>

## <a name="accessibility-and-your-app"></a>辅助功能和应用  
有许多可能的残障人士，包括在以下方面受限的人士：移动、视觉、颜色识别、听觉、说话能力、认知能力以及读写能力。 不过，通过遵循此处提供的指南可以满足大部分的要求。 这意味着：

* 为键盘交互和屏幕阅读器提供支持。
* 为用户自定义（如字体、缩放设置（放大）、颜色和高对比度设置）提供支持。
* 为 UI 的某些部分提供替换选项或补充选项。

用于 XAML 的控件提供了内置的键盘支持和对辅助技术（如屏幕阅读器）的支持，从而利用了已支持 UWP 应用、HTML 和其他 UI 技术的辅助功能框架。 此内置支持启用了基本级别的辅助功能，只需执行极少量的操作（设置少量属性）即可完成自定义。 如果你要创建自己的自定义 XAML 组件和控件，则还可以通过使用*自动化对等*这一概念来添加对于这些控件的类似支持。

另外，使用数据绑定、样式以及模板功能可以轻松实现对显示设置和替换 UI 文本的动态更改的支持。

<span id="UI_Automation"/>
<span id="ui_automation"/>
<span id="UI_AUTOMATION"/>

## <a name="ui-automation"></a>UI 自动化  
辅助功能支持主要来自 Microsoft UI 自动化框架的集成支持。 该支持通过控件类型类实现的基类和内置行为以及 UI 自动化提供程序 API 的接口表示形式提供。 每个控件类都使用自动化对等和自动化模式的 UI 自动化概念，以便向 UI 自动化客户端报告控件的角色和内容。 UI 自动化将该应用视为顶级窗口，通过 UI 自动化框架，该应用窗口内的所有辅助功能相关内容均可供 UI 自动化客户端使用。 有关 UI 自动化的详细信息，请参阅 [UI 自动化概述](https://msdn.microsoft.com/library/windows/desktop/Ee684076)。

<span id="Assistive_technology"/>
<span id="assistive_technology"/>
<span id="ASSISTIVE_TECHNOLOGY"/>

## <a name="assistive-technology"></a>辅助技术  
许多用户辅助功能需求由用户安装的辅助技术产品或由操作系统提供的工具和设置来完成。 这包括诸如屏幕阅读器、屏幕放大以及高对比度设置等功能。

辅助技术产品包括大量软件和硬件。 这些产品通过标准的键盘界面和辅助功能框架工作，这些框架向屏幕阅读器和其他辅助技术报告有关 UI 的内容和结构的信息。 辅助技术产品的示例包括：

* 屏幕键盘（可让用户使用指针代替键盘来键入文本）。
* 声音识别软件（可将说词转换为键入的文本）。
* 屏幕阅读器（可将文本转换为说词或其他形式（如盲文））。
* 讲述人屏幕阅读器（专门属于 Windows）。 讲述人具有触摸模式，当没有提供键盘时，该模式可以通过处理触控笔势执行屏幕阅读任务。
* 用于调整显示器或其区域的程序或设置，例如高对比度主题、显示器的每英寸点数 (dpi) 设置或“放大镜”工具。

具有良好键盘和屏幕阅读器支持的应用通常可成功用于大量的辅助技术产品。 在很多情况下，UWP 应用可用于这些产品，而无需对信息或结构进行任何额外的修改。 但是，你可能需要修改某些设置以获得最佳辅助功能体验或实现其他支持。

[辅助功能测试](accessibility-testing.md)中列出了你可以用于通过辅助技术测试基本辅助功能方案的一些选项。

<span id="Screen_reader_support_and_basic_accessibility_information"/>
<span id="screen_reader_support_and_basic_accessibility_information"/>
<span id="SCREEN_READER_SUPPORT_AND_BASIC_ACCESSIBILITY_INFORMATION"/>

## <a name="screen-reader-support-and-basic-accessibility-information"></a>屏幕阅读器支持和基本辅助功能信息  
可通过屏幕阅读器来访问应用中的文本，方法是以某个其他格式呈现该文本，例如口头语言或盲文输出内容。 屏幕阅读器的确切行为取决于软件和软件的用户配置。

例如，当用户启动或切换至要查看的应用时，某些屏幕阅读器会读取整个应用 UI，从而允许用户在接收所有可用信息内容后再尝试进行导航。 某些屏幕阅读器在 Tab 键导航过程中接收焦点时还会读取与单个控件关联的文本。 从而允许用户在应用程序的输入控件之间导航时确定自己的位置。 “讲述人”即为提供两种行为的屏幕阅读器示例，具体取决于用户选择。

屏幕阅读器或任何其他辅助技术在帮助用户了解或导航应用时需要的最重要信息就是应用的元素部分的*辅助名称*。 在许多情况下，控件或元素已经有一个辅助名称，该名称是从你已经以其他方式提供的其他属性值计算的。 对于可支持和显示内部文本的元素，最常使用已经计算的名称。 对于其他元素，有时需要考虑用其他方法来通过遵循元素结构的最佳做法提供辅助名称。 而且有时候，你需要提供一个名称来明确作为应用辅助功能的辅助名称。 有关这些计算值中有多少值可在常见的 UI 元素中使用的列表，以及有关常见辅助名称的详细信息，请参阅[基本辅助功能信息](basic-accessibility-information.md)。

可以使用一些其他自动化属性（包括下一部分中所述的键盘属性）。 但是，并非所有屏幕阅读器都支持所有的自动化属性。 通常，应设置所有适当的自动化属性和测试，以便为屏幕阅读器提供尽可能多的支持。

<span id="Keyboard_support"/>
<span id="keyboard_support"/>
<span id="KEYBOARD_SUPPORT"/>

## <a name="keyboard-support"></a>键盘支持  
为了提供良好的键盘支持，必须确保应用程序的每个部分都可与键盘结合使用。 如果你的应用主要使用标准控件而不使用任何自定义控件，则说明你已经做到了这一点。 基本 XAML 控件模型提供了内置的键盘支持，包括 Tab 导航、文本输入以及控件特定的支持。 充当布局容器（如面板）的元素使用布局顺序来建立默认的 Tab 键顺序。 该顺序通常是适合 UI 的辅助表示形式的 Tab 键顺序。 如果使用 [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) 和 [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) 控件显示数据，则它们会提供内置的箭头键导航。 或者，如果使用 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 控件，则它已经为按钮激活而处理空格键或 Enter 键。

有关键盘支持的各个方面的详细信息（包括 Tab 键顺序和基于键的激活或导航），请参阅[键盘辅助功能](keyboard-accessibility.md)。

<span id="Media_and_captioning"/>
<span id="media_and_captioning"/>
<span id="MEDIA_AND_CAPTIONING"/>

## <a name="media-and-captioning"></a>媒体和字幕  
通常通过 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926) 对象显示视听媒体。 你可以使用 **MediaElement** API 控制媒体播放。 为实现辅助功能，所提供的控件应能使用户根据需要播放、暂停和停止媒体。 有时，媒体包括面向辅助功能的额外组件，如字幕或包括叙述性描述的可选音轨。

<span id="Accessible_text"/>
<span id="accessible_text"/>
<span id="ACCESSIBLE_TEXT"/>

## <a name="accessible-text"></a>辅助文本  
文本的以下三个主要方面与辅助功能相关：

* 工具必须确定文本是在 Tab 序列遍历过程中读取，还是仅作为整个文档表示形式的一部分进行读取。 你还可以通过选择要用来显示文本的相应元素或者通过调整这些文本元素的属性来帮助控件确定上述内容。 每个文本元素都有一种特定的用途，而且该用途通常具有相应的 UI 自动化角色。 如果使用错误的元素，可能会导致向 UI 自动化报告错误的角色，而且可能会为辅助技术用户创建令人混淆的体验。
* 除非文本与背景的对比度很大，否则，许多用户都会因视觉上的局限而很难阅读文本。 对于没有该视觉局限的应用设计人员来说，这对用户造成的影响不太直观。 例如，对于色盲用户，如果设计中的颜色选项不好，可能会使某些用户无法阅读文本。 最初为 Web 内容提出的辅助功能建议还定义了可能会避免这些应用问题的对比度标准。 有关详细信息，请参阅[辅助文本要求](accessible-text-requirements.md)。
* 对于太小的文本，许多用户都有阅读困难。 你可以通过使应用 UI 中的文本在首次出现时合理变大来避免发生此问题。 但是，对于显示大量文本的应用或者对于与其他视觉元素穿插在一起的文本，则具有一定的挑战性。 在这些情况下，请确保应用与能够放大显示器的系统功能正确交互，以便应用中的任何文本都能够随控件一起放大。 （某些用户可采用辅助功能选项方式更改 DPI 值。 该选项可从“轻松使用”**** 的“放大屏幕上显示的内容”**** 中获得，可重定向到“外观和个性化”**** / “屏幕”**** 的“控制面板”**** UI。）

<span id="Supporting_high-contrast_themes"/>
<span id="supporting_high-contrast_themes"/>
<span id="SUPPORTING_HIGH-CONTRAST_THEMES"/>

## <a name="supporting-high-contrast-themes"></a>支持高对比度主题  
UI 控件使用一种定义为 XAML 资源主题字典一部分的视觉表示形式。 这些主题中的一个或多个专门用于为系统设置了高对比度的情况。 当用户通过从资源字典中动态查找相应主题来切换到高对比度时，所有 UI 控件也将使用相应的高对比度主题。 你只需确保尚未禁用这些主题即可，方法是指定明确的样式，或者使用其他可防止高对比度主题加载和替代样式更改的样式技术。 有关详细信息，请参阅[高对比度主题](high-contrast-themes.md)。

<span id="Design_for_alternative_UI"/>
<span id="design_for_alternative_ui"/>
<span id="DESIGN_FOR_ALTERNATIVE_UI"/>

## <a name="design-for-alternative-ui"></a>替换 UI 设计  
在设计应用时，应考虑在行动、视觉和听觉方面受限的用户可以如何使用这些应用。 由于辅助技术产品大量使用标准 UI，因此提供良好的键盘和屏幕阅读器支持尤为重要，即便未对辅助功能做任何其他调整，也是如此。

很多情况下，可以使用多种技术传递重要信息以便扩宽受众范围。 例如，可以同时使用图标和颜色信息来突出显示信息以帮助色盲用户，并可显示视觉警报和声音效果以帮助听力受损的用户。

如有必要，可以提供可完全删除非必要元素和动画的辅助性替换用户界面元素，并提供其他简化措施以简化用户体验。 以下代码示例演示了如何根据用户设置显示一个 [**UserControl**](https://msdn.microsoft.com/library/windows/apps/BR227647) 实例以代替另一实例。

XAML
```xml
<StackPanel x:Name="LayoutRoot" Background="White">

  <CheckBox x:Name="ShowAccessibleUICheckBox" Click="ShowAccessibleUICheckBox_Click">
    Show Accessible UI
  </CheckBox>

  <UserControl x:Name="ContentBlock">
    <local:ContentPage/>
  </UserControl>

</StackPanel>
```

Visual Basic
```vb
Private Sub ShowAccessibleUICheckBox_Click(ByVal sender As Object,
    ByVal e As RoutedEventArgs)

    If (ShowAccessibleUICheckBox.IsChecked.Value) Then
        ContentBlock.Content = New AccessibleContentPage()
    Else
        ContentBlock.Content = New ContentPage()
    End If
End Sub
```

C#
```csharp
private void ShowAccessibleUICheckBox_Click(object sender, RoutedEventArgs e)
{
    if ((sender as CheckBox).IsChecked.Value)
    {
        ContentBlock.Content = new AccessibleContentPage();
    }
    else
    {
        ContentBlock.Content = new ContentPage();
    }
}
```

<span id="Verification_and_publishing"/>
<span id="verification_and_publishing"/>
<span id="VERIFICATION_AND_PUBLISHING"/>

## <a name="verification-and-publishing"></a>验证和发布  
有关辅助功能声明和发布应用的详细信息，请参阅 [Microsoft Store 中的辅助功能](accessibility-in-the-store.md)。

> [!NOTE]
> 将应用声明为辅助应用仅与 Microsoft Store 有关。

<span id="Assistive_technology_support_in_custom_controls"/>
<span id="assistive_technology_support_in_custom_controls"/>
<span id="ASSISTIVE_TECHNOLOGY_SUPPORT_IN_CUSTOM_CONTROLS"/>

## <a name="assistive-technology-support-in-custom-controls"></a>自定义控件中的辅助技术支持  
创建自定义控件时，建议你同时实现或扩展一个或多个 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 子类以提供辅助功能支持。 在某些情况下，只要你使用与基本控件类所用相同的对等类，对你的派生类的自动化支持在基本级别上便足够了。 但是，你应该对此进行测试，并且作为最佳做法，仍然建议你实现一个对等，以便对等可以正确地报告你的新控件类的类名称。 实现自定义自动化对等涉及多个步骤。 有关详细信息，请参阅[自定义自动化对等](custom-automation-peers.md)。

<span id="Assistive_technology_support_in_apps_that_support_XAML___Microsoft_DirectX_interop"/>
<span id="assistive_technology_support_in_apps_that_support_xaml___microsoft_directx_interop"/>
<span id="ASSISTIVE_TECHNOLOGY_SUPPORT_IN_APPS_THAT_SUPPORT_XAML___MICROSOFT_DIRECTX_INTEROP"/>

## <a name="assistive-technology-support-in-apps-that-support-xaml--microsoft-directx-interop"></a>支持 XAML / Microsoft DirectX 互操作的应用中的辅助技术支持  
默认情况下，无法访问 XAML UI 中托管的 Microsoft DirectX 内容（使用 [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/Dn252834) 或 [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/Hh702041)）。 [XAML SwapChainPanel DirectX 互操作示例](http://go.microsoft.com/fwlink/p/?LinkID=309155)显示如何为托管的 DirectX 内容创建 UI 自动化对等。 这种技术让托管的内容可通过 UI 自动化进行访问。

## <a name="related-topics"></a>相关主题  
* [**Windows.UI.Xaml.Automation**](https://msdn.microsoft.com/library/windows/apps/BR209179)
* [辅助功能设计](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [XAML 辅助功能示例](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [辅助功能](accessibility.md)
* [讲述人入门](https://support.microsoft.com/help/22798/windows-10-narrator-get-started)
