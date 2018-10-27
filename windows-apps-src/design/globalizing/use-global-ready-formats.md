---
author: stevewhims
Description: Design your app to be global-ready by appropriately formatting dates, times, numbers, phone numbers, and currencies. You'll then be able later to adapt your app for additional cultures, regions, and languages in the global market.
title: 全球化日期/时间/数字格式
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
template: detail.hbs
ms.author: stwhi
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, 全球化, 可本地化性, 本地化
ms.localizationpriority: medium
ms.openlocfilehash: 173198c2c61530704dad02e2e92e6a7e47aae420
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5700686"
---
# <a name="globalize-your-datetimenumber-formats"></a>全球化日期/时间/数字格式

通过适当设置日期、时间、数字、电话号码和货币的格式，设计全球通用的应用。 稍后即可调整应用，以适应全球市场中更多的文化、区域和语言。

## <a name="introduction"></a>简介

创建应用时，如果考虑到多种语言和文化，则在应用向新市场发展时出现的意外问题将更少（如果有）。 例如，日期、时间、数字、日历、货币、电话号码、度量单位和纸张大小的显示都会因文化或语言的不同而有所不同。

不同地区和文化使用不同的日期和时间格式。 它们包含在以下方面的使用惯例：日期中的日和月顺序、时间中的小时和分钟的分隔，甚至用作分隔符的标点符号。 此外，可以采用各种长格式（“星期三，2012 年 3 月 28 日”）或短格式（“12/3/28”）显示日期，这些格式因文化而异。 当然，星期中某天和年份中的某月的名称和缩写也因语言而异。

可以预览用于不同语言的格式。 转到**设置** > **时间和语言** > **区域和语言**，单击**其他日期、时间和区域设置** > **更改日期、时间或数字格式**。 在**格式**选项卡上，从**格式**下拉菜单中选择一种语言，并在**示例**中预览此格式。

本主题使用术语“用户配置文件语言列表”、“应用清单语言列表”和“应用运行时语言列表”。 有关这些术语到底款意味着什么，以及如何访问其值的详细信息，请参阅[了解用户配置文件语言和应用清单语言](manage-language-and-region.md)。

## <a name="format-dates-and-times-for-the-app-runtime-language-list"></a>设置应用运行时语言列表的日期和时间格式

如果你需要允许用户选择日期或选择时间，请使用标准的[日历、日期和时间控件](../controls-and-patterns/date-and-time.md)。 它们将自动为应用运行时语言列表使用最佳日期和时间格式。

如果你需要自行显示日期或时间，则可以使用 [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 类。 默认情况下，**DateTimeFormatter** 将自动为应用运行时语言列表使用最佳日期和时间格式。 因此，以下代码针对该列表以最佳方式设置给定**日期时间**的格式。 例如，假设你的应用清单语言列表包括英语(美国)（默认语言）和德语(德国)。 如果当前日期为 2017 年 11 月 6 日，并且用户配置文件语言列表首先包含德语(德国)，则格式化程序将呈现“06.11.2017”。 如果用户配置文件语言列表首先包含英语(美国)（或既不包含英语，也不包含德语），则格式化程序将呈现“11/6/2017”（因为“en-US”匹配，或用作默认语言）。

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate");
    var shortTimeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shorttime");

    var dateTimeToFormat = DateTime.Now;

    var shortDate = shortDateFormatter.Format(dateTimeToFormat);
    var shortTime = shortTimeFormatter.Format(dateTimeToFormat);

    var results = "Short Date: " + shortDate + "\n" +
                  "Short Time: " + shortTime;
```

可在你自己的电脑上按此操作测试以上代码。

- 请确保同时针对“en-US”和“de-DE”限定了项目中的资源文件（请参阅[针对语言、缩放、高对比度和其他限定符定制资源](../../app-resources/tailor-resources-lang-scale-contrast.md)）。
- 在**设置** > **时间和语言** > **区域和语言** > **语言**中更改你的用户配置文件语言列表。 添加德语(德国)，使其成为默认语言，并再次运行代码。

## <a name="format-dates-and-times-for-the-user-profile-language-list"></a>设置用户配置文件语言列表的日期和时间格式

请记住，默认情况下，**DateTimeFormatter** 匹配应用运行时语言列表。 这样，如果显示“日期为 &lt;date&gt;”等字符串，语言将匹配日期格式。

如果无论出于何种原因想要仅根据用户配置文件语言列表设置日期和/或时间的格式，则可以使用类似于以下示例的代码执行此操作。 但如果执行了此操作，则将了解到，用户可以选择你的应用没有将字符串翻译为某种语言的语言。 例如，如果你的应用未本地化为德语(德国)，但用户将该语言作为其首选语言，则可能导致显示有争议的奇怪字符串，例如“日期为 06.11.2017”。

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate", userLanguages);

    var results = "Short Date: " + shortDateFormatter.Format(DateTime.Now);
```

## <a name="format-numbers-and-currencies-appropriately"></a>设置适当的数字和货币格式

不同的文化设置数字格式的方式也不相同。 格式差异可能包括显示多少小数位数、什么字符用作小数分隔符以及要使用什么货币符号。 使用 [**NumberFormatting**](/uwp/api/windows.globalization.numberformatting?branch=live) 命名空间中的类来显示小数、分数、千分比和货币。 大多数情况下，建议使用这些格式化程序类，将最佳格式用于用户配置文件。 但是你可以使用格式化程序显示任何区域或格式的货币。

此示例展示如何按照用户配置文件和针对特定给定货币系统来显示货币。

```csharp
    // This scenario uses the CurrencyFormatter class to format a number as a currency.

    var userCurrency = Windows.System.UserProfile.GlobalizationPreferences.Currencies[0];

    var valueToBeFormatted = 12345.67;

    var userCurrencyFormatter = new Windows.Globalization.NumberFormatting.CurrencyFormatter(userCurrency);
    var userCurrencyValue = userCurrencyFormatter.Format(valueToBeFormatted);

    // Create a formatter initialized to a specific currency,
    // in this case US Dollar (specified as an ISO 4217 code) 
    // but with the default number formatting for the current user.
    var currencyFormatUSD = new Windows.Globalization.NumberFormatting.CurrencyFormatter("USD");
    var currencyValueUSD = currencyFormatUSD.Format(valueToBeFormatted);

    // Create a formatter initialized to a specific currency.
    // In this case it's the Euro with the default number formatting for France.
    var currencyFormatEuroFR = new Windows.Globalization.NumberFormatting.CurrencyFormatter("EUR", new[] { "fr-FR" }, "FR");
    var currencyValueEuroFR = currencyFormatEuroFR.Format(valueToBeFormatted);

    // Results for display.
    var results = "Fixed number (" + valueToBeFormatted + ")\n" +
                    "With user's default currency: " + userCurrencyValue + "\n" +
                    "Formatted US Dollar: " + currencyValueUSD + "\n" +
                    "Formatted Euro (fr-FR defaults): " + currencyValueEuroFR;
```

通过更改**设置** > **时间和语言** > **区域和语言** > **国家或地区**中的国家或地区，可在自己的电脑上测试以上代码。 选择一个国家或地区（如冰岛），并再次运行代码。

## <a name="use-a-culturally-appropriate-calendar"></a>使用与文化相对应的日历

日历因区域和语言而异。 并非每一个区域都使用公历作为默认日历。 某些区域的用户可能选择其他日历，例如日本历或伊斯兰历。 日历上的日期和时间还对不同的时区和夏令时敏感。

为确保使用首选的日历格式，可使用标准的[日历、日期和时间控件](../controls-and-patterns/date-and-time.md)。 对于可能需要对日历日期直接操作的更加复杂的方案，**Windows.Globalization** 提供了一个 [**Calendar**](/uwp/api/windows.globalization.calendar?branch=live) 类，该类为给定的文化、区域和日历类型提供适当的日历表示。

## <a name="format-phone-numbers-appropriately"></a>设置相应的电话号码

设置电话号码格式的方式因区域而异。 电话号码的数字位数、数字组合方式和某些部分的重要性因国家/地区而异。 从 Windows 10 版本 1607 开始，可以使用 [**PhoneNumberFormatting**](/uwp/api/windows.globalization.phonenumberformatting?branch=live) 命名空间中的类为当前区域设置适当的电话号码格式。

[**PhoneNumberInfo**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberinfo?branch=live) 可解析一串数字，并允许你：确定这些数字在当前区域中是否是有效的电话号码；比较两组数字是否相等；提取电话号码的不同功能部分，例如国家/地区代码或地理区域代码。

[**PhoneNumberFormatter**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberformatter?branch=live) 可设置一串数字或 **PhoneNumberInfo** 的格式以供显示，即使该串数字表示部分电话号码。 你可以使用此部分号码格式设置，将号码设置为用户输入该号码时所示的格式。

下面的示例显示了如何使用 **PhoneNumberFormatter** 将电话号码设置为输入时所示的格式。 每当名为 phoneNumberInputTextBox 的 **TextBox** 中的文本发生更改时，文本框内容都会使用当前默认区域进行格式设置并显示在名为 phoneNumberOutputTextBlock 的 **TextBlock** 中。 出于演示目的，该字符串也使用新西兰区域进行格式设置，并显示在名为 phoneNumberOutputTextBlockNZ 的 TextBlock 中。
  
```csharp
    using Windows.Globalization.PhoneNumberFormatting;

    PhoneNumberFormatter currentFormatter, NZFormatter;

    public MainPage()
    {
        this.InitializeComponent();

        // Use the default formatter for the current region
        this.currentFormatter = new PhoneNumberFormatter();

        // Create an explicit formatter for New Zealand. 
        PhoneNumberFormatter.TryCreate("NZ", out this.NZFormatter);
    }

    private void phoneNumberInputTextBox_TextChanged(object sender, TextChangedEventArgs e)
    {
        // Format for the default region.
        this.phoneNumberOutputTextBlock.Text = currentFormatter.FormatPartialString(this.phoneNumberInputTextBox.Text);

        // If the NZFormatter was created successfully, format the partial string for the NZ TextBlock.
        if(this.NZFormatter != null)
        {
            this.phoneNumberOutputTextBlockNZ.Text = this.NZFormatter.FormatPartialString(this.phoneNumberInputTextBox.Text);
        }
    }
```    

通过更改**设置** > **时间和语言** > **区域和语言** > **国家或地区**中的国家或地区，可在自己的电脑上测试以上代码。 选择一个国家或地区（可能是新西兰，以便确认格式匹配），并再次运行代码。 对于测试数据，可以针对新西兰的商业电话号码进行 Web 搜索。

## <a name="the-users-language-and-cultural-preferences"></a>用户的语言和文化首选项

对于想要仅基于用户的语言、区域或文化首选项提供不同功能的方案，Windows 为你提供了一种访问这些首选项的方法：通过 [**Windows.System.UserProfile.GlobalizationPreferences**](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live)。 需要时，使用 **GlobalizationPreferences** 类获取用户当前地理区域、首选语言、首选货币等项目的值。 但请记住，如果应用的字符串/图像未针对用户的首选语言进行本地化，则针对该首选语言进行格式设置的日期、时间和其他数据将与你显示的字符串不匹配。

## <a name="important-apis"></a>重要的 API

* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [NumberFormatting](/uwp/api/windows.globalization.numberformatting?branch=live)
* [日历](/uwp/api/windows.globalization.calendar?branch=live)
* [PhoneNumberFormatting](/uwp/api/windows.globalization.phonenumberformatting?branch=live)
* [GlobalizationPreferences](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live)

## <a name="related-topics"></a>相关主题

* [日历、日期和时间控件](../controls-and-patterns/date-and-time.md)
* [了解用户配置文件语言和应用清单语言](manage-language-and-region.md)
* [定制语言、比例、高对比度和其他限定符的资源](../../app-resources/tailor-resources-lang-scale-contrast.md)

## <a name="samples"></a>示例

* [日历详细信息和数学示例](http://go.microsoft.com/fwlink/p/?linkid=231636)
* [设置日期和时间格式示例](http://go.microsoft.com/fwlink/p/?linkid=231618)
* [全球化首选项示例](http://go.microsoft.com/fwlink/p/?linkid=231608)
* [数字格式和分析示例](http://go.microsoft.com/fwlink/p/?linkid=231620)
