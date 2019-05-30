---
title: 准备你的应用程序以进行日本纪元更改
description: 了解 2019 年 5 月日本纪元更改以及如何使你的应用程序准备就绪。
ms.assetid: 5A945F9A-8632-4038-ADD6-C0568091EF27
ms.date: 4/26/2019
ms.topic: article
keywords: windows 10, uwp, 可本地化性, 本地化, 日本, 纪元
ms.localizationpriority: high
ms.openlocfilehash: 54d66d0426e5f0c41d48b93ba96781786d6fab92
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363780"
---
# <a name="prepare-your-application-for-the-japanese-era-change"></a>准备你的应用程序以进行日本纪元更改

> [!NOTE]
> 2019 年 4 月 1 日，已宣布新时代名称：Reiwa （令和）。 年 4 月 25 日，Microsoft 发布了包含与新纪元名称的更新的注册表密钥不同 Windows 操作系统的包。 更新你的设备和检查注册表以查看其是否具有新密钥，然后测试你的应用程序。 检查[支持本文](https://support.microsoft.com/help/4469068/summary-of-new-japanese-era-updates-kb4469068)以确保你的操作系统应会收到更新的注册表项。

日本历划分为纪元，并且对于大多数的计算的现代化时代，我们已经在平成时代;但是，在上，2019 年 5 月 1 日的新时代将开始。 因为这是几十年来第一次更改纪元，因此需要测试支持日本历的软件，以确保在新纪元开始时该软件能够正常运行。

在以下部分中，你将了解可以执行哪些操作来准备和测试你的应用程序以使用即将开始的新纪元。

> [!NOTE]
> 我们建议你使用测试计算机进行相关操作，因为你做出的更改将影响整个计算机的行为。

## <a name="add-a-registry-key-for-the-new-era"></a>为新纪元添加注册表项

> [!NOTE]
> 以下说明是为了使用新的注册表项尚未更新的设备。 首先检查你的设备是否包含新的注册表项，如果没有，请使用和测试的以下说明进行操作。

请务必时代已更改，并可以现在使用新时代名称执行操作之前测试兼容性问题。 若要执行此操作，请使用**注册表编辑器**为新纪元添加注册表项：

1. 导航到 **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Nls\Calendars\Japanese\Eras**。
2. 依次选择**编辑 > 新建 > 字符串值**，并将其命名为 **2019 年 5 月 1 日**。
3. 右键单击该项并选择**修改**。
4. 在中**数值数据**字段中，输入**令和_令_Reiwa_R** （可以复制并粘贴在此处轻松）。

有关这些注册表项的格式的详细信息，请参阅[日本历的纪元处理](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar)。

2019 年 4 月 1 日，已宣布新时代名称。 年 4 月 25 日，将更新为支持的 Windows 版本包含名称的新注册表项与已发布，从而可以验证你的应用程序正确处理它。 此更新将被传播到受支持的早期版本的 Windows 10 和 Windows 8 和 7。

完成应用程序测试后，你可以删除占位符注册表项。 这将确保它不会干扰在 Windows 更新时添加的新注册表项。

## <a name="change-your-devices-calendar-format"></a>更改设备的日历格式

为新纪元添加注册表项后，你需要将设备配置为使用日本历。 无需拥有使用日语的设备，即可执行此操作。 若要进行全面测试，你可能还需要安装日语语言包，但此操作对于基本测试不是必需的。

若要将设备配置为使用日本历，请执行以下操作：

1. 打开 **intl.cpl**（从 Windows 搜索栏中搜索它）。
2. 从**格式**下拉列表中，选择**日语(日本)** 。
3. 选择**其他设置**。
4. 选择**数据**选项卡。
5. 从**日历类型**下拉列表中，选择**和暦**（*wareki*，日本历）。 它应该是第二个选项。
6. 单击 **“确定”** 。
7. 在**地区**窗口中单击**确定**。

你的设备现在应该已配置为使用日本历，它将在注册表中反映所处的纪元。 下面是你现在可能在屏幕右下角中看到的内容示例：

![采用日本历格式的日期和时间](images/japanese-calendar-format.png)

## <a name="adjust-your-devices-clock"></a>调整设备的时钟

若要测试你的应用程序是否适用于新纪元，必须预先将计算机的时钟设置为 2019 年 5 月 1 日或更晚时间。 以下说明适用于 Windows 10，但也应适用于 Windows 8 和 7：

1. 右键单击屏幕右下角中的日期和时间区域。
2. 选择**调整日期/时间**。
3. 在“设置”应用中，在**更改日期和时间**下，选择**更改**。
4. 将日期更改为 2019 年 5 月 1 日或更晚时间。

> [!NOTE]
> 您可能不能更改的日期根据组织设置;如果这种情况，，请与管理员联系。或者，您可以编辑你占位符注册表项以具有日期已过。

## <a name="test-your-application"></a>测试你的应用程序

现在，测试你的应用程序如何处理新纪元。 检查日期的显示位置，例如时间戳和日期选取器。 请确保的时代 （平成，平成） 2019 年 5 月 1 日之前和之后 (Reiwa，令和) 正确。

### <a name="gannen-"></a>*Gannen* (元年)

日本历的格式通常是 **&lt;纪元名称&gt; &lt;纪元的年份&gt;** 。 例如，2018 年是 **Heisei 30** (平成30年)。  但是，纪元的第一年很特殊；不是 **&lt;纪元名称&gt; 1**，而是 **&lt;纪元名称&gt; 元年** (*gannen*)。 因此，平成纪元的第一年将为平成元年 (*Heisei gannen*)。 请确保你的应用程序正确处理第一年的新时代中，并正确输出令和元年。

## <a name="related-apis"></a>相关 API

有几个 WinRT、.NET 和 Win32 API，将更新这些 API 以处理纪元更改，如果你使用它们，无需过多担心。 但是，即使你完全依赖这些 API，测试应用程序并确保获取所需行为仍是一个好方法，在使用它们执行任何特殊操作（如解析）时尤其如此。

你可以遵循以及更新 OS 和 Sdk 处[的更新可能 2019年日本纪元更改](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)。

以下 API 将受到影响：

### <a name="winrt"></a>WinRT

* [Windows.Globalization Namespace](https://docs.microsoft.com/uwp/api/windows.globalization)
  * [日历类](https://docs.microsoft.com/uwp/api/windows.globalization.calendar)
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
    * [纪元属性](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.era)
    * [EraAsString 方法](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.eraasstring)
    * [FirstYearInThisEra 属性](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.firstyearinthisera)
    * [LastEra 属性](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastera)
    * [LastYearInThisEra 属性](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastyearinthisera)
    * [NumberOfYearsInThisEra 属性](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.numberofyearsinthisera)
* [Windows.Globalization.DateTimeFormatting Namespace](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting)
  * [DateTimeFormatter 类](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter)
    * [Format 方法](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter.format)

### <a name="net"></a>.NET

* [System Namespace](https://docs.microsoft.com/dotnet/api/system)
  * [DateTime 结构](https://docs.microsoft.com/dotnet/api/system.datetime)
  * [DateTimeOffset 结构](https://docs.microsoft.com/dotnet/api/system.datetimeoffset)
* [System.Globalization Namespace](https://docs.microsoft.com/dotnet/api/system.globalization)
  * [日历类](https://docs.microsoft.com/dotnet/api/system.globalization.calendar)
  * [DateTimeFormatInfo 类](https://docs.microsoft.com/dotnet/api/system.globalization.datetimeformatinfo)
  * [JapaneseCalendar 类](https://docs.microsoft.com/dotnet/api/system.globalization.japanesecalendar)
  * [JapaneseLunisolarCalendar 类](https://docs.microsoft.com/dotnet/api/system.globalization.japaneselunisolarcalendar)

### <a name="win32"></a>Win32

* [datetimeapi.h header](https://docs.microsoft.com/windows/desktop/api/datetimeapi/)
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

## <a name="see-also"></a>请参阅

* [处理日本历的时代](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar)
* [日本历 Y2K 时刻](https://blogs.msdn.microsoft.com/shawnste/2018/04/12/the-japanese-calendars-y2k-moment/)
* [使用注册表来在 Windows 上测试新的日语时代](https://blogs.msdn.microsoft.com/shawnste/2018/08/07/using-the-registry-to-test-the-new-japanese-era-on-windows/)
* [元年 vs Ichinen](https://blogs.msdn.microsoft.com/shawnste/2018/11/12/gannen-vs-ichinen/)
* [更新可能会 2019年日本纪元更改](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)
* [处理在.NET 中的日语日历中的新时代](https://devblogs.microsoft.com/dotnet/handling-a-new-era-in-the-japanese-calendar-in-net/)
