---
Description: Date and time controls let you view and set the date and time. This article provides design guidelines and helps you pick the right control.
title: 日期和时间控件指南
ms.assetid: 4641FFBB-8D82-4290-94C1-D87617997F61
label: Calendar, date, and time controls
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f65ed68db51ea173dfec3c06a9dc81a7f7735afd
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8944206"
---
# <a name="calendar-date-and-time-controls"></a>日历、日期和时间控件

 

日期和时间控件向你提供了标准的本地化方法，可供用户在你的应用中查看并设置日期和时间值。 本文提供设计指南，并帮助你选取正确的控件。

> **重要 API**：[CalendarView 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx)，[CalendarDatePicker 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.aspx)，[DatePicker 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx)，[TimePicker 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx)

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/category/DataInput">打开此应用，了解这些控件的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="which-date-or-time-control-should-you-use"></a>应使用哪个日期或时间控件？

有四个日期和时间控件可供选择；要使用的控件取决于你的方案。 使用此信息来选取要在你的应用中使用的正确控件。

&nbsp;|&nbsp;|&nbsp;                                                                                                                      
--------------------|-------|-------------------------------------------------------------------------------------------------------------------------------
日历视图       |![日历视图示例](images/controls_calendar_monthview_small.png)|用于从始终可见的日历中选取一个日期或日期范围。                   
日历日期选取器|![日历日期选取器示例](images/calendar-date-picker-closed.png)|用于从上下文日历中选取一个日期。 
日期选取器         |![日期选取器示例](images/date-picker-closed.png)|用于在上下文信息不重要时选取一个已知日期。
时间选取器         |![时间选取器示例](images/time-picker-closed.png)|用于选取一个时间值。                                        

<!-- This table seems redundant, not sure it's needed.-->

### <a name="calendar-view"></a>日历视图

**CalendarView** 让用户查看可按月份、年份或十年期浏览的日历，并与之交互。 用户可选择单个日期或一组日期。 它没有选取器图面，并且日历始终可见。

日历视图中包含 3 个单独的视图：月视图、年视图和十年视图。 默认情况下，它通过打开月视图启动，但你可以将其他视图指定为启动视图。

![日历日期选取器示例](images/calendar-view-3-views.png)

- 如果需要允许用户选择多个日期，则必须使用 **CalendarView**。
- 如果需要仅允许用户选取一个日期并且不需要日历始终可见，请考虑使用 **CalendarDatePicker** 或 **DatePicker** 控件。

### <a name="calendar-date-picker"></a>日历日期选取器

**CalendarDatePicker** 是一个下拉式控件，该控件已针对从日历视图中选取某个日期进行了优化，尤其是能够显示诸如星期几或丰富的日历信息等上下文信息。 可以修改日历以提供其他上下文或限制可用日期。

如果尚未设置日期，入口点将显示占位符文本；否则，它将显示选择的日期。 当用户选择该入口点时，日历视图将进行扩展以供用户选择日期。 日历视图将会覆盖其他 UI；它不会将其他 UI 推开。

![日历日期选取器示例](images/calendar-date-picker-2-views.png)

- 日历日期选取器可用于选择约会或出发日期等事项。 

### <a name="date-picker"></a>日期选取器

**DatePicker** 控件提供了一种用于选择特定日期的标准化方法。 

入口点显示选定的日期，当用户选择该入口点时，会从中间垂直展开一个选取器图面以供用户进行选择。 日期选取器会覆盖其他 UI；它不会将其他 UI 推开。

![日期选取器展开示例](images/controls_datepicker_expand.png)

- 使用日期选取器以使用户选取日历上下文不重要的已知日期，例如生日。

### <a name="time-picker"></a>时间选取器

**TimePicker** 用于为约会或出发时间等事项选择一个时间值。 它是由用户或使用代码设置的静态显示方式，但不会更新以显示当前时间。 

入口点显示选定的时间，当用户选择该入口点时，会从中间垂直展开一个选取器图面以供用户进行选择。 时间选取器会覆盖其他 UI；它不会将其他 UI 推开。

![时间选取器展开示例](images/controls_timepicker_expand.png)

- 使用时间选取器使用户可以选取单个时间值。

## <a name="create-a-date-or-time-control"></a>创建一个日期或时间控件

有关特定于每个日期和时间控件的信息和示例，请参阅以下这些文章。

- [日历视图](calendar-view.md)
- [日历日期选取器](calendar-date-picker.md)
- [日期选取器](date-picker.md)
- [时间选取器](time-picker.md)

### <a name="globalization"></a>全球化

XAML 日期控件支持 Windows 支持的各种日历系统。 这些日历均在 [Windows.Globalization.CalendarIdentifiers](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.calendaridentifiers.aspx) 类中指定。 每个控件均使用与应用的默认语言对应的正确日历，还可以设置 **CalendarIdentifier** 属性以使用特定的日历系统。

时间选取器控件支持在 [Windows.Globalization.ClockIdentifiers](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.clockidentifiers.aspx) 类中指定的每个时钟系统。 若要使用 12 小时制时钟或 24 小时制时钟，可以设置 [ClockIdentifier](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.clockidentifier.aspx) 属性。 该属性的类型是字符串，但必须使用对应于 ClockIdentifiers 类的静态字符串属性的值。 如下所示：TwelveHour（字符串“12HourClock”）和TwentyFourHour（字符串“24HourClock”）。 “12HourClock”是默认值。


### <a name="datetime-and-calendar-values"></a>DateTime 和日历值

XAML 日期和时间控件中所使用的日期对象具有不同的表示形式，具体取决于你的编程语言。 
- C# 和 Visual Basic 使用 [System.DateTimeOffset](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx) 结构，它是 .NET 的一部分。 
- C++/CX 使用 [Windows::Foundation::DateTime](https://msdn.microsoft.com/library/windows/apps/xaml/br205770.aspx) 结构。 

相关概念是 Calendar 类，它对在上下文中解释日期的方式产生影响。 所有 Windows 运行时应用都可以使用 [Windows.Globalization.Calendar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.calendar.aspx) 类。 C# 和 Visual Basic 应用还可以使用 [System.Globalization.Calendar](https://msdn.microsoft.com/library/windows/apps/xaml/system.globalization.calendar.aspx) 类，该类具有非常类似的功能。 （Windows 运行时应用可使用 .NET 日历基类而非诸如 GregorianCalendar 的特定实现。）

.NET 还支持名为 [DateTime](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetime.aspx) 的类型，该类型可隐式转换为 [DateTimeOffset](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx)。 因此，可能会看到要在 .NET 代码中用于设置实际上是 DateTimeOffset 的值的“DateTime”类型。 有关 DateTime 和 DateTimeOffset 之间的区别的详细信息，请参阅 [DateTimeOffset](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx) 类中的“备注”。

> **注意**&nbsp;&nbsp;获取日期对象的属性不可以设置为 XAML 属性字符串，因为 Windows 运行时 XAML 解析器不具有用于将字符串转换为日期（作为 DateTime/DateTimeOffset 对象）的转换逻辑。 通常使用代码设置这些值。 另一个可行的方法是定义可用作数据对象或在数据上下文中可用的日期，然后将该属性设置为引用 [\{Binding\} 标记扩展](../../xaml-platform/binding-markup-extension.md)表达式的 XAML 属性，以便可以将该日期作为数据访问。

## <a name="get-the-sample-code"></a>获取示例代码
* [XAML UI 基本示例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)


## <a name="related-topics"></a>相关主题

**面向开发人员 (XAML)**
- [CalendarView 类](https://msdn.microsoft.com/library/windows/apps/dn890052)
- [CalendarDatePicker 类](https://msdn.microsoft.com/library/windows/apps/dn950083)
- [DatePicker 类](https://msdn.microsoft.com/library/windows/apps/dn298584)
- [TimePicker 类](https://msdn.microsoft.com/library/windows/apps/dn299280)
