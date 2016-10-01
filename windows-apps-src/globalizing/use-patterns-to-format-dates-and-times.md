---
author: DelfCo
Description: "使用 Windows.Globalization.DateTimeFormatting API 和自定义模式严格按照所需模式显示日期和时间。"
title: "使用模式设置日期和时间的格式"
ms.assetid: 012028B3-9DA2-4E72-8C0E-3E06BEC3B3FE
label: Use patterns to format dates and times
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: f49af17ada36ceb2e5898d80047c2d616b1d0c6e

---

# 使用模式设置日期和时间的格式





**重要的 API**

-   [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
-   [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828)
-   [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)

结合使用 [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859) API 和自定义模式以严格按照所需模式显示日期和时间。

## <span id="Introduction"></span><span id="introduction"></span><span id="INTRODUCTION"></span>简介


[**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859) 为全球的语言和区域提供了各种正确设置日期和时间格式的方法。 你可以为年、月、日等使用标准格式，或使用标准字符串模板，例如“longdate”或“month day”。

但当你希望更好地控制要显示的 [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) 字符串要素的顺序和格式时，你可以为字符串模板参数使用名为“模式”的特殊语法。 使用模式语法可以获得 **DateTime** 对象的个别要素（例如获取月名称或仅获取年值），以便在你选择的任何自定义格式中显示它们。 此外，模式也可以进行本地化以适应其他语言和区域。

**注意** 这是格式模式的概述。 有关格式模板和格式模式的更完整讨论，请参阅 [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828) 类的“备注”部分。

 

## <span id="What_you_need_to_know"></span><span id="what_you_need_to_know"></span><span id="WHAT_YOU_NEED_TO_KNOW"></span>你需要了解的内容


需要指出的是在使用模式时，你生成的是一种自定义格式，而该格式不能保证满足各种区域性。 例如，看一下“month day”模板：

**C#**
```CSharp
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```
**JavaScript**
```JavaScript
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```

此模板会根据当前上下文的语言和区域值创建一个格式化程序。 因此，它始终以适当的全局格式同时显示月和日。 例如，对于英语(美国)，它将显示“January 1”，但对于法语(法国) 则显示“1 janvier”，而日语则显示“1月1日”。 这是因为模板是基于特定文化的模式字符串，可以通过模式属性进行访问：

**C#**
```CSharp
var monthdaypattern = datefmt.Patterns;
```
**JavaScript**
```JavaScript
var monthdaypattern = datefmt.patterns;
```

这将产生不同的结果，具体取决于格式化程序的语言和区域。 注意，不同的区域可能使用不同的要素、不同的顺序、带有或者不带其他字符和间隔：

``` syntax
En-US: "{month.full} {day.integer}"
Fr-FR: "{day.integer} {month.full}"
Ja-JP: "{month.integer}月{day.integer}日"
```

你可以使用模式构建一个自定义 [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828)，例如此格式化程序基于美国英语模式：

**C#**
```CSharp
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```
**JavaScript**
```JavaScript
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```

Windows 对花括号 {} 中的个别要素返回特定文化的值。 但若使用模式语法，则要素顺序将是固定的。 你得到的内容完全符合你的要求，但可能不适合相关文化：

``` syntax
En-US: January 1
Fr-FR: janvier 1 (inappropriate for France; non-standard order)
Ja-JP: 1月1 (inappropriate for Japan; the day symbol is missing)
```

而且，模式不能保证随着时间的推移而保持一致性。 国家或地区可能会更改其日历系统，这将改变格式模板。 Windows 会更新格式化程序的输出以适应此类更改。 因此，在以下情况下，仅应使用模式语法设置 [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) 的格式：

-   你没有依赖于某个格式的特定输出。
-   你不需要该格式来遵循某些特定文化标准。
-   你专门打算让模式在各种文化中保持不变。
-   你要对模式进行本地化。

标准字符串模板和非标准字符串模式之间的区别总结：

**像“month day”（月日）这样的字符串模板：**

-   以某种顺序抽象 [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) 格式的表示形式，包括月和日的值。
-   保证跨 Windows 支持的所有语言-区域值返回有效的标准格式。
-   保证为给定的语言-区域提供符合文化的格式化字符串。
-   并非所有要素组合都有效。 例如，“dayofweek day”就没有字符串模板。

**像“{month.full} {day.integer}”这样的字符串模式：**

-   字符串具有明显顺序，以该顺序表达完整的月名称、后跟一个空格，然后跟整数日期。
-   可能与任何语言-区域对的有效标准格式不对应。
-   不保证文化上的符合性。
-   能够以任何顺序指定要素的任何组合。

## <span id="Tasks"></span><span id="tasks"></span><span id="TASKS"></span>任务


假设你希望将当前月份和日期与当前时间以特定的格式一起显示。 例如，你希望“英语(美国)”用户看到如下内容：

``` syntax
June 25 | 1:38 PM
```

日期部分对应于“month day”模板，而时间部分对应于“hour minute”模板。 因此你可以创建自定义格式，用于连接组成这些模板的模式。

首先，获取相应日期和时间模板的格式化程序，然后获取这些模板的模式：

**C#**
```CSharp
// Get formatters for the date part and the time part.
var mydate = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var mytime = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

// Get the patterns from these formatters.
var mydatepattern = mydate.Patterns[0];
var mytimepattern = mytime.Patterns[0];
```
**JavaScript**
```JavaScript
// Get formatters for the date part and the time part.
var dtf = Windows.Globalization.DateTimeFormatting;
var mydate = dtf.DateTimeFormatter("month day");
var mytime = dtf.DateTimeFormatter("hour minute");

// Get the patterns from these formatters.
var mydatepattern = mydate.patterns[0];
var mytimepattern = mytime.patterns[0];
```

你应该将你自定义的格式存储为可本地化的资源字符串。 例如，“英语(美国)”的字符串应是“{date} | {time}”。 本地化人员可以根据需要调整此字符串。 例如，如果在某个语言或区域中将时间放在日期前面显得更自然，则他们可以更改要素的顺序。 或者，他们也可以将“|”替换为其他分隔符。 在运行时，使用相应的模式替换该字符串的 {date} 和 {time} 部分：

**C#**
```CSharp
// Assemble the custom pattern. This string comes from a resource, and should be localizable. 
var resourceLoader = new Windows.ApplicationModel.Resources.ResourceLoader();
var mydateplustime = resourceLoader.GetString("date_plus_time");
mydateplustime = mydateplustime.replace("{date}", mydatepattern);
mydateplustime = mydateplustime.replace("{time}", mytimepattern);
```
**JavaScript**
```JavaScript
// Assemble the custom pattern. This string comes from a resource, and should be localizable. 
var mydateplustime = WinJS.Resources.getString("date_plus_time");
mydateplustime = mydateplustime.replace("{date}", mydatepattern);
mydateplustime = mydateplustime.replace("{time}", mytimepattern);
```

然后你就可以根据自定义模式构造一个新的格式化程序：

**C#**
```CSharp
// Get the custom formatter.
var mydateplustimefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(mydateplustime);
```
**JavaScript**
```JavaScript
// Get the custom formatter.
var mydateplustimefmt = new dtf.DateTimeFormatter(mydateplustime);
```

## <span id="related_topics"></span>相关主题


* [设置日期和时间格式示例](http://go.microsoft.com/fwlink/p/?LinkId=231618)
* [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
* [**Windows.Foundation.DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)
 

 






<!--HONumber=Aug16_HO3-->


