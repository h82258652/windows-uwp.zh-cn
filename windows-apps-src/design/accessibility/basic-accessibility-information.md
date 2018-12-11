---
Description: Basic accessibility info is often categorized into name, role, and value. This topic describes code to help your app expose the basic information that assistive technologies need.
ms.assetid: 9641C926-68C9-4842-8B55-C38C39A9E5C5
title: 公开基本的辅助功能信息
label: Expose basic accessibility information
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8e7526ec4f32f641f152709e6968f3dc442c2a06
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2018
ms.locfileid: "8900757"
---
# <a name="expose-basic-accessibility-information"></a>公开基本的辅助功能信息  



基本的辅助功能信息通常按照名称、角色和值进行分类。 本主题介绍可帮助应用公开辅助技术所需的基本信息的代码。

<span id="accessible_name"/>
<span id="ACCESSIBLE_NAME"/>

## <a name="accessible-name"></a>辅助名称  
辅助名称是屏幕阅读器用于述说 UI 元素的简短且具有描述性的文本字符串。 请为这样的 UI 元素设置辅助名称，以使其意义对于了解内容或与 UI 交互而言非常重要。 这类元素通常包括图像、输入域、按钮、控件和区域。

下表描述了如何为 XAML UI 中各种类型的元素定义或获取辅助名称。

| 元素类型 | 说明 |
|--------------|-------------|
| 静态文本 | 对于 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 和 [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565) 元素，辅助名称是从可见（内部）文本自动确定的。 该元素中所有文本都用作其名称。 请参阅[根据内部文本命名](#name_from_inner_text)。 |
| 图像 | XAML [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) 元素没有对 **img** 和类似元素的 HTML **alt** 属性的直接模拟。 使用 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 提供名称，或者使用描述技术。 请参阅[图像的辅助名称](#images)。 |
| 窗体元素 | 窗体元素的辅助名称应当与针对该元素显示的标签同名。 请参阅[标签和 LabeledBy](#labels)。 |
| 按钮和链接 | 默认情况下，按钮或链接的辅助名称基于可见文本，并使用相同的规则，如[根据内部文本命名](#name_from_inner_text)所述。 如果按钮中仅包含一个图像，请使用 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 提供与按钮的预期操作等效的仅文本操作。 |

大多数容器元素（如面板）不会将其内容提升为辅助名称。 这是由于项目内容应当报告名称及其角色而非其容器。 容器元素可能报告它在 Microsoft UI 自动化表示形式中有子元素，以便它能够由辅助技术逻辑遍历。 但是，辅助技术的用户通常不需要了解容器，因此大多数容器都没有命名。

<span id="role_value"/>
<span id="ROLE_VALUE"/>

## <a name="role-and-value"></a>角色和值  
属于 XAML 词汇的控件和其他 UI 元素都实现了对于 UI 自动化的支持，可以在元素的定义中报告角色和值。 你可以使用 UI 自动化工具检查控件的角色和值信息，也可以读取每个控件的 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 实现文档。 UI 自动化框架中的可用角色在 [**AutomationControlType**](https://msdn.microsoft.com/library/windows/apps/BR209182) 枚举中定义。 通过调用 UI 自动化框架使用控件的 **AutomationPeer** 公开的方法，UI 自动化客户端（例如，辅助技术）可以获取角色信息。

并非所有的控件都有值。 有值的控件将通过该控件支持的对等和模式来向 UI 自动化报告此信息。 例如，[**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 窗体元素有值。 辅助技术可以是 UI 自动化客户端，而且既能发现值的存在，又能发现值是多少。 在此特定情况下，**TextBox** 通过 [**TextBoxAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242550) 定义支持 [**IValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242663) 模式。

> [!NOTE]
> 如果你使用 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 或其他技术明确提供辅助名称，请不要在辅助名称中包括由控件角色或类型信息使用的文本。 例如，不要在名称中包含诸如“button”或“list”的字符串。 角色和类型信息源自对 UI 自动化的默认控件支持所提供的另一个 UI 自动化属性 (**LocalizedControlType**)。 许多辅助技术都在辅助名称后面附加 **LocalizedControlType**，因此，使用辅助名称来复制角色可能会导致不必要的重复词。 例如，如果你为 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 控件提供辅助名称“button”或将“button”包含为该名称的最后一部分，则可能导致屏幕阅读器将其读为“button button”。 你应该使用讲述人功能对辅助功能信息的该方面进行测试。

<span id="Influencing_the_UI_Automation_tree_views"/>
<span id="influencing_the_ui_automation_tree_views"/>
<span id="INFLUENCING_THE_UI_AUTOMATION_TREE_VIEWS"/>

## <a name="influencing-the-ui-automation-tree-views"></a>影响 UI 自动化树视图  
UI 自动化框架包含树视图概念，在这里 UI 自动化客户端可以使用以下三种可能的视图检索 UI 中元素之间的关系：原始视图、控件视图和内容视图。 控件视图是 UI 自动化客户端经常会使用的视图，因为它可以很好地表示和组织 UI 中的交互元素。 在测试工具显示元素的组织结构时，你通常可以选择使用哪种树视图。

默认情况下，当 UI 自动化框架表示通用 Windows 平台 (UWP) 应用的 UI 时，控件视图中将会显示所有由 [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) 派生的类以及少量其他元素。 但有些时候，出于 UI 组合的原因，你可能不希望某个元素显示在控件视图中（因为该元素复制的信息或显示的信息对于辅助功能方案无足轻重）。 使用附加属性 [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/Dn251788) 更改将元素公开给树视图的方式。 如果你将某个元素放置在 **Raw** 树中，则大多数辅助技术都不会将该元素报告为其视图的一部分。 若要查看此操作在现有控件中的工作原理的某些示例，请在文本编辑器中打开 generic.xaml 设计参考 XAML 文件，并在模板中搜索 **AutomationProperties.AccessibilityView**。

<span id="name_from_inner_text"/>
<span id="NAME_FROM_INNER_TEXT"/>

## <a name="name-from-inner-text"></a>根据内部文本命名  
为了更便于将可见 UI 中已经存在的字符串用于辅助名称值，许多控件和其他 UI 元素都支持以下功能：基于元素中的内部文本或者内容属性的字符串值自动确定默认辅助名称。

* [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)、[**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565)、[**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 和 **RichTextBlock** 各自将 **Text** 属性的值提升为默认的辅助名称。
* 任何 [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content) 子类都使用迭代“ToString”技术在其 [**Content**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content) 值中查找字符串，并将这些字符串提升为默认的辅助名称。

> [!NOTE]
> 由于辅助名称通过 UI 自动化强制执行，因此其长度不能超过 2048 个字符。 如果用于自动确定辅助名称的字符串超过该限制，则辅助名称会在与该限制相对应的位置被截断。

<span id="images"/>
<span id="IMAGES"/>

## <a name="accessible-names-for-images"></a>图像的辅助名称
为了支持屏幕阅读器并为 UI 中的每个元素提供基本的标识信息，你有时必须为非文本信息（如图像和图表，不包括纯装饰性或结构化元素）提供文本替换选项。 这些元素没有内部文本，因此辅助名称将不包含计算的值。 可以通过设置 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 附加属性（如下面的示例中所示）来直接设置辅助名称。

XAML
```xml
<!-- Comment -->
<Image Source="product.png"
  AutomationProperties.Name="An image of a customer using the product."/>
```

或者，考虑包括一个文本描述文字，该描述文字在可见 UI 中出现并同时充当图形内容的、与标签相关的辅助功能信息。 下面是一个示例：

XAML
```xml
<Image HorizontalAlignment="Left" Width="480" x:Name="img_MyPix"
  Source="snoqualmie-NF.jpg"
  AutomationProperties.LabeledBy="{Binding ElementName=caption_MyPix}"/>
<TextBlock x:Name="caption_MyPix">Mount Snoqualmie Skiing</TextBlock>
```

<span id="labels"/>
<span id="LABELS"/>

## <a name="labels-and-labeledby"></a>标签和 LabeledBy  
将标签与窗体元素相关联的首选方法是针对标签文本结合使用 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 和 **x:Name**，然后将窗体元素上的 [**AutomationProperties.LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) 附加属性设置为按其 XAML 名称引用标签 **TextBlock**。 如果你使用此模式，当用户单击该标签时，焦点会移动到相关的控件上，辅助技术可以使用标签文本作为窗体字段的辅助名称。 以下示例显示此技术。

XAML
```xml
<StackPanel x:Name="LayoutRoot" Background="White">
   <StackPanel Orientation="Horizontal">
     <TextBlock Name="lbl_FirstName">First name</TextBlock>
     <TextBox
      AutomationProperties.LabeledBy="{Binding ElementName=lbl_FirstName}"
      Name="tbFirstName" Width="100"/>
   </StackPanel>
   <StackPanel Orientation="Horizontal">
     <TextBlock Name="lbl_LastName">Last name</TextBlock>
     <TextBox
      AutomationProperties.LabeledBy="{Binding ElementName=lbl_LastName}"
      Name="tbLastName" Width="100"/>
   </StackPanel>
 </StackPanel>
```

<span id="accessible_description"/>
<span id="ACCESSIBLE_DESCRIPTION"/>

## <a name="accessible-description-optional"></a>辅助说明（可选）  
辅助说明提供了有关特定 UI 元素的其他辅助功能信息。 通常在仅有辅助名称不足以传递元素用途的情况下提供辅助说明。

仅当用户通过按 CapsLock + F 请求有关某个元素的更多信息时，讲述人屏幕阅读器才会读取该元素的辅助说明。

辅助名称旨在标识控件而不是完全记录其行为。 如果简短说明不足以很好地说明控件，则除了 [**AutomationProperties.HelpText**](https://msdn.microsoft.com/library/windows/apps/Hh759765) 外，还可以设置 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 附加属性。

<span id="Testing_accessibility_early_and_often"/>
<span id="testing_accessibility_early_and_often"/>
<span id="TESTING_ACCESSIBILITY_EARLY_AND_OFTEN"/>

## <a name="testing-accessibility-early-and-often"></a>提前并经常测试辅助功能  
最后，支持屏幕阅读器的最佳方法是自己使用屏幕阅读器测试应用。 这将说明屏幕阅读器的行为方式以及应用中可能缺少辅助功能的基本信息。 然后，可以相应调整 UI 或 UI 自动化属性值。 有关详细信息，请参阅[辅助功能测试](accessibility-testing.md)。

可用于测试辅助功能的工具之一是 **AccScope**。 **AccScope** 工具十分有用，因为你可以查看 UI 的视觉表示，它们将会呈现辅助技术如何将你的应用视为自动化树。 特别要说明的是，其中提供了讲述人模式，你可以通过它来了解讲述人如何从你的应用中获取文本以及如何组织 UI 中的元素。 还设计了 AccScope，以供在整个应用开发周期中有效地使用，甚至在初步设计阶段也不例外。 有关详细信息，请参阅 [AccScope](https://msdn.microsoft.com/library/windows/desktop/Dn433239)。

<span id="Accessible_names_from_dynamic_data"/>
<span id="accessible_names_from_dynamic_data"/>
<span id="ACCESSIBLE_NAMES_FROM_DYNAMIC_DATA"/>

## <a name="accessible-names-from-dynamic-data"></a>动态数据中的辅助名称  
Windows 通过一个名为*数据绑定*的功能，支持许多可用来显示相关数据源所提供的值的控件。 当你用数据项填充列表时，可能需要在初始列表填充之后使用一种用来为数据绑定列表项设置辅助名称的技术。 有关详细信息，请参阅 [XAML 辅助功能示例](http://go.microsoft.com/fwlink/p/?linkid=238570)中的“方案 4”。

<span id="Accessible_names_and_localization"/>
<span id="accessible_names_and_localization"/>
<span id="ACCESSIBLE_NAMES_AND_LOCALIZATION"/>

## <a name="accessible-names-and-localization"></a>辅助名称和本地化  
为了确保辅助名称同时还是已本地化的元素，应当使用正确的技术将可本地化字符串作为资源进行存储，然后引用具有 [x:Uid 指令](https://msdn.microsoft.com/library/windows/apps/Mt204791)值的资源连接。 如果辅助名称来自显式设置的 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 用法，请确保该用法中的字符串同样本地化。

请注意，附加属性（如 [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081) 属性）对资源名称使用特定的限定语法，以便资源在应用到特定元素时引用该附加属性。 例如，应用到名为 `MediumButton` 的 UI 元素 时，[**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 的资源名称为 `MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name`。

<span id="related_topics"/>

## <a name="related-topics"></a>相关主题  
* [辅助功能](accessibility.md)
* [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770)
* [XAML 辅助功能示例](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [辅助功能测试](accessibility-testing.md)
