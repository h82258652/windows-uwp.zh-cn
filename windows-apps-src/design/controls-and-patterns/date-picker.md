---
Description: 日期选取器向你提供了一种标准化方式，可使用户通过触摸、鼠标或键盘输入选取本地化格式的日期值。
title: 日期选取器
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: true
label: Date picker
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4f0215cd9d3f9a105b6a7ff0ec3f07ef50ff1e44
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "80080911"
---
# <a name="date-picker"></a>日期选取器

日期选取器向你提供了一种标准化方式，可使用户通过触摸、鼠标或键盘输入选取本地化格式的日期值。

![日期选取器示例](images/date-picker-closed.png)

**获取 Windows UI 库**

|  |  |
| - | - |
| ![WinUI 徽标](images/winui-logo-64x64.png) | Windows UI 库 2.2 或更高版本包含此控件的使用圆角的新模板。 有关详细信息，请参阅[圆角半径](/windows/uwp/design/style/rounded-corner)。 WinUI 是一种 NuGet 包，其中包含用于 UWP 应用的新控件和 UI 功能。 有关详细信息（包括安装说明），请参阅 [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **平台 API：** [DatePicker class](/uwp/api/Windows.UI.Xaml.Controls.DatePicker)（DatePicker 类），[Date 属性](/uwp/api/windows.ui.xaml.controls.datepicker.date)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用日期选取器以使用户选取日历上下文不重要的已知日期，例如生日。

有关选择正确日期控件的详细信息，请参阅[日期和时间控件](date-and-time.md)文章。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 XAML 控件库应用，请单击此处<a href="xamlcontrolsgallery:/item/DatePicker">打开此应用，了解 DatePicker 的实际应用</a><strong style="font-weight: semi-bold"></strong>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

入口点显示选定的日期，当用户选择该入口点时，会从中间垂直展开一个选取器图面以供用户进行选择。 日期选取器会覆盖其他 UI；它不会将其他 UI 推开。

![日期选取器展开示例](images/controls_datepicker_expand.png)

## <a name="create-a-date-picker"></a>创建日期选取器

本示例演示如何创建附带标头的简单日期选取器。

```xaml
<DatePicker x:Name="birthDatePicker" Header="Date of birth"/>
```

```csharp
DatePicker birthDatePicker = new DatePicker();
birthDatePicker.Header = "Date of birth";
```

生成的日期选取器如下所示：

![日期选取器示例](images/date-picker-closed.png)

> 注意：有关日期值的重要信息，请参阅日期和时间控件文章中的 [DateTime&nbsp;和&nbsp;Calendar 值](date-and-time.md#datetime-and-calendar-values)  。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

- [日期和时间控件](date-and-time.md)
- [日历日期选取器](calendar-date-picker.md)
- [日历视图](calendar-view.md)
- [时间选取器](time-picker.md)
