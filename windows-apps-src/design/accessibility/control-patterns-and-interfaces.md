---
Description: 列出 Microsoft UI 自动化控件模式、客户端用于访问这些模式的类以及提供程序用于实现这些模式的接口。
ms.assetid: 2091883C-5D0C-44ED-936A-709022926A42
title: 控件模式和接口
label: Control patterns and interfaces
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fc8018a4ccf53c106a1e10410593e214d9dcead7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359568"
---
# <a name="control-patterns-and-interfaces"></a>控件模式和接口  



列出 Microsoft UI 自动化控件模式、客户端用于访问这些模式的类以及提供程序用于实现这些模式的接口。

本主题中的表描述了 Microsoft UI 自动化控件模式。 此表还列出了 UI 自动化客户端用于访问控件模式的类和 UI 自动化提供程序用来实现这些模式的接口。 从 UI 自动化客户端的角度来看，“控件模式”  列显示模式名称，[**Control Pattern Availability Property Identifiers**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-control-pattern-availability-propids) 中将该名称列为常数值。 从 UI 自动化提供程序的角度来看，其中每个模式都是一个 [**PatternInterface**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) 常量名称。 “类提供程序接口”  列显示提供程序为向自定义 XAML 控件提供此模式而实现的接口名称。

有关如何实现用于公开控件模式和实现接口的自定义自动化对等的详细信息，请参阅[自定义自动化对等](custom-automation-peers.md)。

在实现控件模式时，还应查阅 UI 自动化提供程序文档，该文档说明了客户端在控件模式下将达到的部分预期，无论使用哪个 UI 框架实现控件模式，均不会影响这些预期。 常规 UI 自动化提供程序文档中列出的部分信息将影响对等实现的方式及正确为该模式提供支持的方式。 请参阅[实现 UI 自动化控件模式](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns)，并查看记录了你计划实现的模式的页面。

| 控件模式 | 类提供程序接口 | 描述 |
|-----------------|--------------------------|-------------|
| **批注** | [**IAnnotationProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IAnnotationProvider) | 用于公开文档注释的属性。 |
| **Dock** | [**IDockProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IDockProvider) | 用于可停靠在停靠容器中的控件。 例如，工具栏或工具调色板。 |
| **Drag** | [**IDragProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IDragProvider) | 用于支持可拖动控件或包含可拖动项目的控件。 |
| **DropTarget** | [**IDropTargetProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IDropTargetProvider) | 用于支持可作为拖放操作目标的控件。 |
| **ExpandCollapse** | [**IExpandCollapseProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IExpandCollapseProvider) | 用于支持通过直观展开来显示更多内容并通过折叠来隐藏内容的控件。 |
| **网格** | [**IGridProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridProvider) | 用于支持网格功能（例如调整指定单元格的大小或移动到指定单元格）的控件。 请注意，网格本身并不实现此模式，因为它虽然提供布局，却不是控件。 |
| **GridItem** | [**IGridItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridItemProvider) | 用于在网格中具有单元格的控件。 |
| **Invoke** | [**IInvokeProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) | 用于可调用的控件，例如 [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)。 |
| **ItemContainer** | [**IItemContainerProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IItemContainerProvider) | 支持应用程序查找容器（例如虚拟化列表）中的元素。 |
| **MultipleView** | [**IMultipleViewProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IMultipleViewProvider) | 用于可在同一组信息、数据或子项的多个表示形式之间进行切换的控件。 |
| **ObjectModel** | [**IObjectModelProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IObjectModelProvider) | 用于公开指向文档基础对象模型的指针。 |
| **RangeValue** | [**IRangeValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) | 用于具有一系列可应用于控件的值的控件。 例如，包含年份的微调框控件的数据范围可能介于 1900 至当前年份之间，而另一个呈现月份的微调框控件的数据范围介于 1 至 12 之间。 |
| **滚动** | [**IScrollProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollProvider) | 用于可滚动的控件。 例如，具有滚动条的控件，当信息太多而无法在控件可视区域中全部显示出来时会出现滚动条。 |
| **ScrollItem** | [**IScrollItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollItemProvider) | 用于在可滚动的列表中包含多个独立项的控件。 例如，在滚动列表（如组合框控件）中包含多个独立项的列表控件。 |
| **所选内容** | [**ISelectionProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionProvider) | 用于选择容器控件。 例如，[**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) 和 [**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox)。 |
| **SelectionItem** | [**ISelectionItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionItemProvider) | 用于选择容器控件（如列表框和组合框）中的各个项。 |
| **Spreadsheet** | [**ISpreadsheetProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetProvider) | 用于公开电子表格或其他基于网格的文档的内容。 |
| **SpreadsheetItem** | [**ISpreadsheetItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetItemProvider) | 用于公开电子表格或其他基于网格的文档的单元格属性。 |
| **样式** | [**IStylesProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IStylesProvider) | 用于描述具有特定样式、填充颜色、填充图案或形状的 UI 元素。 |
| **SynchronizedInput** | [**ISynchronizedInputProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISynchronizedInputProvider) | 启用 UI 自动化客户端应用，将鼠标或键盘输入定向到特定的 UI 元素。 |
| **Table** | [**ITableProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableProvider) | 用于具有网格以及标题信息的控件。 例如，表格日历控件。 |
| **TableItem** | [**ITableItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableItemProvider) | 用于表中的项。 |
| **文本** | [**ITextProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITextProvider) | 用于公开文本信息的编辑控件和文档。 另请参阅 [**ITextRangeProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) 和 [**ITextProvider2**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextprovider2)。 |
| **TextChild** | [**ITextChildProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextchildprovider) | 用于访问距元素最近的支持 **Text** 控件模式的上级元素。 |
| **TextEdit** | 无可用的托管类 | 访问用于修改文本的控件，例如通过输入法编辑器 (IME) 执行自动更正或启用输入组合的控件。 |
| **TextRange** | [**ITextRangeProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) | 访问实现 [**ITextProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextprovider) 的文本容器中的一段连续文本。 另请参阅 [**ITextRangeProvider2**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider2)。 |
| **切换** | [**IToggleProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IToggleProvider) | 用于可切换状态的控件。 例如，可选中的 [**CheckBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox) 和菜单项。 |
| **转换** | [**ITransformProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITransformProvider) | 用于可调整大小、移动和旋转的控件。 Transform 控件模式通常用于设计器、窗体、图形编辑器和绘图应用程序。 |
| **ReplTest1** | [**IValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IValueProvider) | 允许客户端在不支持一系列值的控件上获取或设置某个值。 |
| **VirtualizedItem** | [**IVirtualizedItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IVirtualizedItemProvider) | 公开容器中已虚拟化并需要作为 UI 自动化元素可完全进行访问的项目。 |
| **Window** | [**IWindowProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IWindowProvider) | 公开信息特定于 windows、 Microsoft Windows 操作系统的基本概念。 用作窗口的控件示例包括子窗口和对话框。 |

> [!NOTE]
> 现有的 XAML 控件中可能并不会包含所有这些模式的实现。 其中部分模式具有接口，目的仅在于通过模式的常规 UI 自动化框架定义来支持奇偶校验，以及支持需要使用纯粹自定义实现来支持该模式的自动化对等方案。

> [!NOTE]
> Windows Phone 应用商店应用不支持此处列出的所有 UI 自动化控件模式。 **Annotation**、**Dock**、**Drag**、**DropTarget**、**ObjectModel** 是一些不受支持的模式。

<span id="related_topics"/>

## <a name="related-topics"></a>相关主题  
* [自定义自动化对等](custom-automation-peers.md)
* [辅助功能](accessibility.md) 
