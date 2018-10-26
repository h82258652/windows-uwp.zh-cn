---
author: muhsinking
Description: A calendar view lets a user view and interact with a calendar that they can navigate by month, year, or decade.
title: 日历视图
ms.assetid: d8ec5ba8-7a9d-405d-a1a5-5a1b502b9e64
label: Calendar view
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: fba6d8ee56e4d9a3d187721b4b2f1c5daa9b1b1f
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5596984"
---
# <a name="calendar-view"></a>日历视图

日历视图让用户查看可按月份、年份或十年期浏览的日历，并与之交互。 用户可选择单个日期或一组日期。 它没有选取器图面，并且日历始终可见。 

> **重要 API**：[CalendarView 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx)、[SelectedDatesChanged 事件](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selecteddateschanged.aspx)


## <a name="is-this-the-right-control"></a>这是正确的控件吗？
使用日历视图，让用户从始终可见的日历中选取单个日期或一组日期。

如果你需要让用户同时选择多个日期，则必须使用日历视图。 如果你需要让用户仅选取单个日期并且不需要日历始终可见，请考虑使用[日历日期选取器](calendar-date-picker.md)或[日期选取器](date-picker.md)控件。

有关选择正确控件的详细信息，请参阅[日期和时间控件](date-and-time.md)文章。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/CalendarView">打开此应用，了解 CalendarView 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

日历视图由 3 种单独视图构成：月份视图、年份视图和十年期视图。 默认情况下，它先打开月份视图。 可通过设置 [DisplayMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.displaymode.aspx) 属性来指定开始视图。

![日历视图的 3 种视图](images/calendar-view-3-views.png)

用户单击月份视图标题可打开年份视图，单击年份视图标题可打开十年期视图。 用户在十年期视图中选取某个年份可返回年份视图，在年份视图中选取某个月份可返回月份视图。 标题两侧的箭头可按月份、年份或十年期来回浏览。 

## <a name="create-a-calendar-view"></a>创建日历视图

本示例显示如何创建简单的日历视图。

```xaml
<CalendarView/>
```

生成的日历视图如下所示：

![日历视图示例](images/controls_calendar_monthview.png)

### <a name="selecting-dates"></a>选择日期

默认情况下，[SelectionMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectionmode.aspx) 属性设置为 **Single**。 这让用户能够在日历中选取单个日期。 将 SelectionMode 设置为 **None** 以禁用日期选择。 

将 SelectionMode 设置为 **Multiple** 以让用户选择多个日期。 可通过将 [DateTime](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetime.aspx)/[DateTimeOffset](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx) 对象添加到 [SelectedDates](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selecteddates.aspx) 集合来以编程方式选择多个日期，如下所示：

```csharp
calendarView1.SelectedDates.Add(DateTimeOffset.Now);
calendarView1.SelectedDates.Add(new DateTime(1977, 1, 5));
```

用户可通过在日历网格中单击或点击已选择的日期来取消选择。

可处理 [SelectedDatesChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selecteddateschanged.aspx) 事件，以在 [SelectedDates](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selecteddates.aspx) 集合更改时收到通知。

> [!NOTE]
> 有关日期值的重要信息，请参阅日期和时间控件一文中的 [Datetime 和日历值](date-and-time.md#datetime-and-calendar-values)。

### <a name="customizing-the-calendar-views-appearance"></a>自定义日历视图的外观

日历视图由在 ControlTemplate 中定义的 XAML 元素和直接由该控件呈现的视觉元素构成。 
- 在控件模板中定义的 XAML 元素包括围绕控件的边框、标题、上一步和下一步按钮以及 DayOfWeek 元素。 你可以像任何 XAML 控件一样设置这些元素的样式并重新为它们制定模板。 
- 日历网格由 [CalendarViewDayItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarviewdayitem.aspx) 对象构成。 你无法设置这些元素的样式或重新为它们制定模板，但可使用提供的各种属性自定义它们的外观。

此图显示了构成日历月份视图的元素。 有关详细信息，请参阅 [CalendarViewDayItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarviewdayitem.aspx) 类上的“备注”。

![日历月份视图的元素](images/calendar-view-month-elements.png)

下表列出了为修改日历元素外观而可更改的属性。

元素 | 属性
--------|-----------
DayOfWeek | [DayOfWeekFormat](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.dayofweekformat.aspx)  
CalendarItem | [CalendarItemBackground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.calendaritembackground.aspx)、[CalendarItemBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.calendaritemborderbrush.aspx)、[CalendarItemBorderThickness](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.calendaritemborderthickness.aspx)、[CalendarItemForeground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.calendaritemforeground.aspx)  
DayItem | [DayItemFontFamily](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.dayitemfontfamily.aspx)、[DayItemFontSize](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.dayitemfontsize.aspx)、[DayItemFontStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.dayitemfontstyle.aspx)、[DayItemFontWeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.dayitemfontweight.aspx)、[HorizontalDayItemAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.horizontaldayitemalignment.aspx)、[VerticalDayItemAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.verticaldayitemalignment.aspx)、[CalendarViewDayItemStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.calendarviewdayitemstyle.aspx)  
MonthYearItem（在年份和十年期视图中相当于 DayItem） | [MonthYearItemFontFamily](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.monthyearitemfontfamily.aspx)、[MonthYearItemFontSize](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.monthyearitemfontsize.aspx)、[MonthYearItemFontStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.monthyearitemfontstyle.aspx)、[MonthYearItemFontWeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.monthyearitemfontweight.aspx)  
FirstOfMonthLabel | [FirstOfMonthLabelFontFamily](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontfamily.aspx)、[FirstOfMonthLabelFontSize](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontsize.aspx)、[FirstOfMonthLabelFontStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontstyle.aspx)、[FirstOfMonthLabelFontWeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontweight.aspx)、[HorizontalFirstOfMonthLabelAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.horizontalfirstofmonthlabelalignment.aspx)、[VerticalFirstOfMonthLabelAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.verticalfirstofmonthlabelalignment.aspx)、[IsGroupLabelVisible](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.isgrouplabelvisible.aspx)  
FirstofYearDecadeLabel（在年份和十年期视图中相当于 FirstOfMonthLabel） | [FirstOfYearDecadeLabelFontFamily](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontfamily.aspx)、[FirstOfYearDecadeLabelFontSize](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontsize.aspx)、[FirstOfYearDecadeLabelFontStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontstyle.aspx)、[FirstOfYearDecadeLabelFontWeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontweight.aspx)  
视觉状态边框 | [FocusBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.focusborderbrush.aspx)、[HoverBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.hoverborderbrush.aspx)、[PressedBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.pressedborderbrush.aspx)、[SelectedBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectedborderbrush.aspx)、[SelectedForeground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectedforeground.aspx)、[SelectedHoverBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectedhoverborderbrush.aspx)、[SelectedPressedBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectedpressedborderbrush.aspx)  
OutofScope | [IsOutOfScopeEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.isoutofscopeenabled.aspx)、[OutOfScopeBackground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.outofscopebackground.aspx)、[OutOfScopeForeground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.outofscopeforeground.aspx)  
今天 | [IsTodayHighlighted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.istodayhighlighted.aspx)、[TodayFontWeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.todayfontweight.aspx)、[TodayForeground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.todayforeground.aspx)  

 默认情况下，月份视图一次显示 6 个星期。 你可通过设置 [NumberOfWeeksInView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.numberofweeksinview.aspx) 属性来更改显示的星期数。 显示的最小星期数是 2 周，最大数是 8 周。

默认情况下，年份和十年期视图按 4x4 网格显示。 若要更改行数或列数，请使用所需的行数和列数调用 [SetYearDecadeDisplayDimensions](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.setyeardecadedisplaydimensions.aspx)。 这将更改年份和十年期视图的网格。

年份和十年视图在此设置为按 3x4 网格显示。

```csharp
calendarView1.SetYearDecadeDisplayDimensions(3, 4);
```

默认情况下，日历视图显示的最小日期比当前日期早 100 年，而显示的最大日期比当前日期晚 100 年。 可以通过设置 [MinDate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.mindate.aspx) 和 [MaxDate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.maxdate.aspx) 属性来更改日历显示的最小日期和最大日期。

```csharp
calendarView1.MinDate = new DateTime(2000, 1, 1);
calendarView1.MaxDate = new DateTime(2099, 12, 31);
```

### <a name="updating-calendar-day-items"></a>更新日历日期项目

日历中的每一天均由 [CalendarViewDayItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarviewdayitem.aspx) 对象表示。 若要访问单独的日期项目并使用其属性和方法，请处理 [CalendarViewDayItemChanging](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.calendarviewdayitemchanging.aspx) 事件并使用事件参数的项目属性来访问 CalendarViewDayItem。

通过将某个日期的 [CalendarViewDayItem.IsBlackout](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarviewdayitem.isblackout.aspx) 属性设置为 **true**，可使该日期无法选择。 

通过调用 [CalendarViewDayItem.SetDensityColors](https://msdn.microsoft.com/library/windows/apps/xaml/dn890067.aspx) 方法，可显示有关一天中事件密度的上下文信息。 你可以为每个日期显示 0 到 10 个密度条，并设置每个密度条的颜色。 

以下是日历中的某些日常项目。 第 1 和 2 天已排除。第 2、3 和 4 天设置了不同的密度条。

![有密度条的日历天数](images/calendar-view-density-bars.png)

### <a name="phased-rendering"></a>分阶段呈现

日历视图可包含大量的 CalendarViewDayItem 对象。 为保持 UI 响应并能够顺利浏览日历，日历视图支持分阶段呈现。 这允许你将日常项目处理分解为多个阶段。 如果某个日期在所有阶段完成前即已移出视图，则没有其他时间可用于尝试处理和呈现该项目。

本示例显示为安排约会而分阶段呈现日历视图。 
- 在第 0 阶段，将呈现默认的日常项目。 
- 在第 1 阶段，你排除不可预订的日期。 这包括过去的日期、星期日和已全部预订的日期。 
- 在第 2 阶段，你查看当天已预订的每个约会。 每个已确定的约会显示绿色密度条，而每个暂定的约会显示蓝色密度条。 

本示例中的 `Bookings` 类来源于虚构的约会预订应用，不会显示。

```xaml
<CalendarView CalendarViewDayItemChanging="CalendarView_CalendarViewDayItemChanging"/>
```

```csharp
private void CalendarView_CalendarViewDayItemChanging(CalendarView sender, 
                                   CalendarViewDayItemChangingEventArgs args)
{
    // Render basic day items.
    if (args.Phase == 0)
    {
        // Register callback for next phase.
        args.RegisterUpdateCallback(CalendarView_CalendarViewDayItemChanging);
    }
    // Set blackout dates.
    else if (args.Phase == 1)
    {   
        // Blackout dates in the past, Sundays, and dates that are fully booked.
        if (args.Item.Date < DateTimeOffset.Now ||
            args.Item.Date.DayOfWeek == DayOfWeek.Sunday ||
            Bookings.HasOpenings(args.Item.Date) == false)
        {
            args.Item.IsBlackout = true;
        }
        // Register callback for next phase.
        args.RegisterUpdateCallback(CalendarView_CalendarViewDayItemChanging);
    }
    // Set density bars.
    else if (args.Phase == 2)
    {
        // Avoid unnecessary processing.
        // You don't need to set bars on past dates or Sundays.
        if (args.Item.Date > DateTimeOffset.Now &&
            args.Item.Date.DayOfWeek != DayOfWeek.Sunday)
        {
            // Get bookings for the date being rendered.
            var currentBookings = Bookings.GetBookings(args.Item.Date);

            List<Color> densityColors = new List<Color>();
            // Set a density bar color for each of the days bookings.
            // It's assumed that there can't be more than 10 bookings in a day. Otherwise,
            // further processing is needed to fit within the max of 10 density bars.
            foreach (booking in currentBookings)
            {
                if (booking.IsConfirmed == true)
                {
                    densityColors.Add(Colors.Green);
                }
                else
                {
                    densityColors.Add(Colors.Blue);
                }
            }
            args.Item.SetDensityColors(densityColors);
        }
    }
}
```

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

- [日期和时间控件](date-and-time.md)
- [日历日期选取器](calendar-date-picker.md)
- [日期选取器](date-picker.md)
- [时间选取器](time-picker.md)
