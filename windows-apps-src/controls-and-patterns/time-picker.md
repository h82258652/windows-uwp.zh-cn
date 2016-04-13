---
时间选取器提供了一种标准化途径，可使用户使用触摸、鼠标或键盘输入选取时间值。
时间选取器
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
时间选取器
template: detail.hbs
---

# 时间选取器

时间选取器提供了一种标准化途径，可使用户使用触摸、鼠标或键盘输入选取时间值。 

<span class="sidebar_heading" style="font-weight: bold;">重要的 API</span>

-   [**TimePicker 类**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx)
-   [**Time 属性**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.time.aspx)

## 这是正确的控件吗？
使用时间选取器使用户可以选取单个时间值。

有关选择正确控件的详细信息，请参阅[日期和时间控件](date-and-time.md)文章。

## 示例

入口点显示所选的时间，当用户选择该入口点时，会从中间垂直展开一个选取器图面以供用户进行选择。 时间选取器会覆盖其他 UI；它不会将其他 UI 推开。

![时间选取器展开示例](images/controls_timepicker_expand.png)

## 创建时间选取器

本示例显示如何创建带有标题的简单时间选取器。

```xaml
<TimePicker x:Name=arrivalTimePicker Header="Arrival time"/>
```

```csharp
TimePicker arrivalTimePicker = new TimePicker();
arrivalTimePicker.Header = "Arrival time";
```

生成的时间选取器如下所示：

![时间选取器示例](images/time-picker-closed.png)

> **注意**：有关日期和时间值的重要信息，请参阅*日期和时间控件*文章中的 [DateTime 和 Calendar 值](date-and-time.md#datetime-and-calendar-values)。

\[本文包含特定于通用 Windows 平台 (UWP) 应用和 Windows 10 的信息。 有关 Windows 8.1 指南，请下载 [Windows 8.1 指南 PDF](https://go.microsoft.com/fwlink/p/?linkid=258743)。\]

## 相关主题

- [日期和时间控件](date-and-time.md)
- [日历日期选取器](calendar-date-picker.md)
- [日历视图](calendar-view.md)
- [日期选取器](date-picker.md)


<!--HONumber=Mar16_HO1-->


