---
title: 准备你的应用程序以进行日本纪元更改
description: 了解 2019 年 5 月日本纪元更改以及如何使你的应用程序准备就绪。
ms.assetid: 5A945F9A-8632-4038-ADD6-C0568091EF27
ms.date: 04/26/2019
ms.topic: article
keywords: windows 10, uwp, 可本地化性, 本地化, 日本, 纪元
ms.localizationpriority: high
ms.openlocfilehash: 7e8250ccae96ed835aba2a2a993fdde9ae31a884
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "67714113"
---
# <a name="prepare-your-application-for-the-japanese-era-change"></a>准备你的应用程序以进行日本纪元更改

> [!NOTE]
> 2019 年 4 月 1 日宣布了新纪元名称：Reiwa (令和)。 4 月 25 日，Microsoft 发布了针对不同 Windows 操作系统的包，其中包含更新的注册表项和新纪元名称。 请更新设备并检查注册表，看其是否有新项，然后测试应用程序。 请查看[此支持文章](https://support.microsoft.com/help/4469068/summary-of-new-japanese-era-updates-kb4469068)，确保操作系统已经收到更新的注册表项。

日本历以纪元划分，并且在现代年代计算的大部分时间里，我们都处于平成时代；但在 2019 年 5 月 1 日后，将开始新的纪元。 因为这是几十年来第一次更改纪元，因此需要测试支持日本历的软件，以确保在新纪元开始时该软件能够正常运行。

在以下部分中，你将了解可以执行哪些操作来准备和测试你的应用程序以使用即将开始的新纪元。

> [!NOTE]
> 我们建议你使用测试计算机进行相关操作，因为你做出的更改将影响整个计算机的行为。

## <a name="add-a-registry-key-for-the-new-era"></a>为新纪元添加注册表项

> [!NOTE]
> 以下说明适用于尚未使用新注册表项更新的设备。 请先检查设备是否包含新注册表项，如果没有，请按以下说明进行测试。

请务必在纪元更改之前测试兼容性问题，你现在可以使用新纪元名称执行此操作。 若要执行此操作，请使用**注册表编辑器**为新纪元添加注册表项：

1. 导航到 **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Nls\Calendars\Japanese\Eras**。
2. 依次选择“编辑”>“新建”>“字符串值”，并将其命名为 **2019 05 01**。 
3. 右键单击该项并选择“修改”  。
4. 在“值数据”中，输入“令和_令_Reiwa_R”（可以从此处复制并粘贴以简化操作）。  

有关这些注册表项的格式的详细信息，请参阅[日本历的纪元处理](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar)。

2019 年 4 月 1 日宣布了新纪元名称。 4 月 25 日，我们发布了包含该名称的适用于受支持的 Windows 版本的新注册表项更新，这样你就可以验证应用程序是否可以正确处理它。 此更新将会传播到受支持的较早版本的 Windows 10 以及 Windows 8 和 7。

完成应用程序测试后，你可以删除占位符注册表项。 这将确保它不会干扰在 Windows 更新时添加的新注册表项。

## <a name="change-your-devices-calendar-format"></a>更改设备的日历格式

为新纪元添加注册表项后，需要将设备配置为使用日本历。 无需拥有使用日语的设备，即可执行此操作。 若要进行全面测试，可能还需要安装日语语言包，但此操作对于基本测试不是必需的。

若要将设备配置为使用日本历，请执行以下操作：

1. 打开 **intl.cpl**（从 Windows 搜索栏中搜索它）。
2. 从“格式”  下拉列表中，选择“日语(日本)”  。
3. 选择“其他设置”  。
4. 选择“日期”  选项卡。
5. 从“日历类型”  下拉列表中，选择“和暦”  （*wareki*，日本历）。 它应该是第二个选项。
6. 单击“确定”  。
7. 在“区域”  窗口中单击“确定”  。

你的设备现在应该已配置为使用日本历，它将在注册表中反映所处的纪元。 下面是你现在可能在屏幕右下角中看到的内容示例：

![采用日本历格式的日期和时间](images/japanese-calendar-format.png)

## <a name="adjust-your-devices-clock"></a>调整设备的时钟

若要测试应用程序是否适用于新纪元，必须预先将计算机的时钟设置为 2019 年 5 月 1 日或更晚时间。 以下说明适用于 Windows 10，但也应适用于 Windows 8 和 7：

1. 右键单击屏幕右下角中的日期和时间区域。
2. 选择“调整日期/时间”  。
3. 在“设置”应用的“更改日期和时间”  下，选择“更改”  。
4. 将日期更改为 2019 年 5 月 1 日或更晚时间。

> [!NOTE]
> 你可能无法根据组织设置更改日期；如果发生此情况，请联系你的管理员。或者，你可以编辑占位符注册表项以设置过去的日期。

## <a name="test-your-application"></a>测试应用程序

现在，测试应用程序如何处理新纪元。 检查日期的显示位置，例如时间戳和日期选取器。 确保纪元在 2019 年 5 月 1 日 (Heisei, 平成) 之前和 (Reiwa, 令和) 之后正确无误。

### <a name="gannen-"></a>*Gannen* (元年)

日本日历的格式通常为 **&lt;纪元名称&gt; &lt;纪元年份&gt;** 。 例如，2018 年是 **Heisei 30** (平成 30 年)。  但是，纪元的第一年很特殊；不是 **&lt;纪元名称&gt; 1**，而是 **&lt;纪元名称&gt; 元年** (*gannen*)。 因此，平成纪元的第一年将为平成元年 (*Heisei gannen*)。 请确保应用程序正常处理新纪元的第一年，并且正确输出令和元年。

## <a name="related-apis"></a>相关 API

有几个 WinRT、.NET 和 Win32 API，将更新这些 API 以处理纪元更改，如果你使用它们，无需过多担心。 但是，即使你完全依赖这些 API，测试应用程序并确保获取所需行为仍是一个好方法，在使用它们执行任何特殊操作（如解析）时尤其如此。

你可以遵循 [2019 年 5 月日本纪元更改的更新](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)中的操作系统和 SDK 的更新。

以下 API 将受到影响：

### <a name="winrt"></a>WinRT

* [Windows.Globalization 命名空间](https://docs.microsoft.com/uwp/api/windows.globalization)
  * [Calendar 类](https://docs.microsoft.com/uwp/api/windows.globalization.calendar)
    * [AddDays 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.adddays)
    * [AddEras 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.adderas)
    * [AddHours 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addhours)
    * [AddMinutes 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addminutes)
    * [AddMonths 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addmonths)
    * [AddNanoseconds 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addnanoseconds)
    * [AddPeriods 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addperiods)
    * [AddSeconds 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addseconds)
    * [AddWeeks 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addweeks)
    * [AddYears 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addyears)
    * [Era 属性](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.era)
    * [EraAsString 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.eraasstring)
    * [FirstYearInThisEra 属性](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.firstyearinthisera)
    * [LastEra 属性](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastera)
    * [LastYearInThisEra 属性](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastyearinthisera)
    * [NumberOfYearsInThisEra 属性](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.numberofyearsinthisera)
* [Windows.Globalization.DateTimeFormatting 命名空间](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting)
  * [DateTimeFormatter 类](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter)
    * [Format 方法](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter.format)

### <a name="net"></a>.NET

* [System 命名空间](https://docs.microsoft.com/dotnet/api/system)
  * [DateTime 结构](https://docs.microsoft.com/dotnet/api/system.datetime)
  * [DateTimeOffset 结构](https://docs.microsoft.com/dotnet/api/system.datetimeoffset)
* [System.Globalization 命名空间](https://docs.microsoft.com/dotnet/api/system.globalization)
  * [Calendar 类](https://docs.microsoft.com/dotnet/api/system.globalization.calendar)
  * [DateTimeFormatInfo 类](https://docs.microsoft.com/dotnet/api/system.globalization.datetimeformatinfo)
  * [JapaneseCalendar 类](https://docs.microsoft.com/dotnet/api/system.globalization.japanesecalendar)
  * [JapaneseLunisolarCalendar 类](https://docs.microsoft.com/dotnet/api/system.globalization.japaneselunisolarcalendar)

### <a name="win32"></a>Win32

* [datetimeapi.h 标头](https://docs.microsoft.com/windows/desktop/api/datetimeapi/)
  * [GetDateFormatA 函数](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformata)
  * [GetDateFormatEx 函数](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatex)
  * [GetDateFormatW 函数](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatw)
* [winnls.h 标头](https://docs.microsoft.com/windows/desktop/api/winnls/)
  * [EnumDateFormatsA 函数](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsa)
  * [EnumDateFormatsExA 函数](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexa)
  * [EnumDateFormatsExEx 函数](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexex)
  * [EnumDateFormatsExW 函数](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexw)
  * [EnumDateFormatsW 函数](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsw)
  * [GetCalendarInfoA 函数](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfoa)
  * [GetCalendarInfoEx 函数](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfoex)
  * [GetCalendarInfoW 函数](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfow)

## <a name="see-also"></a>另请参阅

* [日本历的纪元处理](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar)
* [日本历的 Y2K 时刻](https://blogs.msdn.microsoft.com/shawnste/2018/04/12/the-japanese-calendars-y2k-moment/)
* [使用注册表在 Windows 上测试新的日本纪元](https://blogs.msdn.microsoft.com/shawnste/2018/08/07/using-the-registry-to-test-the-new-japanese-era-on-windows/)
* [Gannen 与 Ichinen](https://blogs.msdn.microsoft.com/shawnste/2018/11/12/gannen-vs-ichinen/)
* [2019 年 5 月日本纪元更改的更新](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)
* [Handling a new era in the Japanese calendar in .NET](https://devblogs.microsoft.com/dotnet/handling-a-new-era-in-the-japanese-calendar-in-net/)（在 .NET 中处理日本历中的新纪元）
