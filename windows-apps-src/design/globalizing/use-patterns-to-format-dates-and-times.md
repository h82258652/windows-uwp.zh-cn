---
author: stevewhims
Description: Use the Windows.Globalization.DateTimeFormatting API with custom templates and patterns to display dates and times in exactly the format you wish.
title: 使用模式设置日期和时间的格式
ms.assetid: 012028B3-9DA2-4E72-8C0E-3E06BEC3B3FE
label: Use patterns to format dates and times
template: detail.hbs
ms.author: stwhi
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 全球化, 可本地化性, 本地化
ms.localizationpriority: medium
ms.openlocfilehash: 485d16cb9c40769c123719f8f55e81d804f220a3
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
ms.locfileid: "1393986"
---
# <a name="use-templates-and-patterns-to-format-dates-and-times"></a>使用模板和模式设置日期和时间格式

结合使用 [**Windows.Globalization.DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 命名空间中的类及自定义模板和模式，以严格按照所需格式显示日期和时间。

## <a name="introduction"></a>简介

[**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 类为全球的语言和区域提供了各种正确设置日期和时间格式的方法。 你可以为年、月、日等使用标准格式。 或者，可以将格式模板传递到 **DateTimeFormatter** 构造函数中的 *formatTemplate*参数，例如“longdate”或“month day”。

但当你希望更好地控制要显示的 [**DateTime**](/uwp/api/windows.foundation.datetime?branch=live) 对象组件的顺序和格式时，可以将格式模式传递到该构造函数的 *formatTemplate* 参数。 格式模式使用特殊语法，该语法让你可以获得 **DateTime** 对象的个别组件&mdash;例如仅获取月份名称或年份值，以便在所选择的任何自定义格式中显示它们。 此外，模式也可以进行本地化以适应其他语言和区域。

**注意**  这只是格式模式的概述。 有关格式模板和格式模式的更完整讨论，请参阅 [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 类的“备注”部分。

## <a name="the-difference-between-format-templates-and-format-patterns"></a>格式模板和格式模式之间的差异

格式模板是区域性不可知的格式字符串。 因此，如果使用格式模板构建 **DateTimeFormatter**，则格式化程序会以当前语言的正确顺序显示格式组件。 相反，格式模式是区域性特定的。 如果使用格式模式构建 **DateTimeFormatter**，则格式化程序将完全按照给定的格式使用模式。 因此，模式在不同区域性中并非一定有效。

让我们用示例来进一步说明这种区别。 我们会将简单格式模板（不是模式）传递到 **DateTimeFormatter** 构造函数。 这是格式模板“month day”。

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```

此模板会根据当前上下文的语言和区域值创建一个格式化程序。 格式模板中组件的顺序并不重要；格式化程序会以当前语言的正确顺序显示它们。 因此，对于英语（美国），它将显示“January 1”，对于法语（法国）则显示“1 janvier”，而日语则显示“1 月 1 日”。

另一方面，格式模式是区域性特定的。 让我们来访问格式模板的格式模式。

```csharp
IReadOnlyList<string> monthDayPatterns = dateFormatter.Patterns;
```

这将产生不同的结果，具体取决于运行时语言和区域。 不同的区域可能使用不同的组件、不同的顺序、带有或者不带其他字符和间隔。

```syntax
En-US: "{month.full} {day.integer}"
Fr-FR: "{day.integer} {month.full}"
Ja-JP: "{month.integer}月{day.integer}日"
```

在上面的示例中，我们输入了区域性不可知格式字符串，并且返回了区域性特定格式字符串（这是我们调用 `dateFormatter.Patterns` 时碰巧有效的语言和区域函数）。 因此，如果从区域性特定格式模式构造 **DateTimeFormatter**，则它仅对特定语言/区域有效。

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```

以上格式化程序对花括号 {} 中的个别组件返回区域性特定值。 但格式模式中的组件顺序是不变的。 你得到的内容完全符合你的要求，但可能适合或不适合相关区域性。 此格式化程序对于英语（美国）有效，但对于法语（法国）或日语无效。

``` syntax
En-US: January 1
Fr-FR: janvier 1 (inappropriate for France; non-standard order)
Ja-JP: 1月1 (inappropriate for Japan; the day symbol 日 is missing)
```

此外，目前正确的模式可能在未来不正确。 国家或地区可能会更改其日历系统，这将改变格式模板。 Windows 会根据格式模板更新格式化程序的输出以适应此类更改。 因此，应该仅在一个或多个这些条件下使用该模式语法。

-   你没有依赖于某个格式的特定输出。
-   你不需要该格式来遵循某些特定文化标准。
-   你专门打算让模式在各种区域性中保持不变。
-   你打算本地化实际的格式模式字符串本身。

下面综述格式模板和格式模式之间的区别。

**格式模板，例如“month day”**

-   以任意顺序包含月、日等值的 [DateTime](/uwp/api/windows.foundation.datetime?branch=live) 格式的抽象表示形式。
-   保证跨 Windows 支持的所有语言-区域值返回有效的标准格式。
-   保证为给定的语言-区域提供符合区域性的格式化字符串。
-   并非所有组件组合都有效。 例如，“dayofweek day”无效。

**格式模式，例如“{month.full} {day.integer}”**

-   字符串具有明显顺序，以该顺序或指定的任何特定格式模式表达完整的月名称，后跟一个空格，然后跟整数日期。
-   可能与任何语言-区域对的有效标准格式不对应。
-   不保证符合区域性。
-   可能以任意顺序指定组件的任何组合。

## <a name="examples"></a>示例

假设你希望将当前月份和日期与当前时间以特定的格式一起显示。 例如，你希望英语（美国）用户看到如下内容：

``` syntax
June 25 | 1:38 PM
```

日期部分对应于“month day”格式模板，而时间部分对应于“hour minute”格式模板。 因此，可以为相关的日期和时间格式模板构造格式化程序，然后使用可本地化的格式字符串将它们的输出连接起来。

```csharp
var dateToFormat = System.DateTime.Now;
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

var date = dateFormatter.Format(dateToFormat);
var time = timeFormatter.Format(dateToFormat);

string output = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), date, time);
```

`CustomDateTimeFormatString` 是关于资源文件 (.resw) 中的可本地化资源的资源标识符。 对于默认语言是英语（美国）的情况，这将设置为值“{0} | {1}”，以及注释说明“{0}”表示日期，“{1}”表示时间。 这样，翻译人员可以按需调整格式项。 例如，如果在某个语言或区域中将时间放在日期前面显得更自然，则他们可以更改项的顺序。 或者，他们也可以将“|”替换为其他分隔符。

实现此示例的另一种方法是查询这两个格式化程序的格式模式，将它们连接起来，然后从结果格式模式构建第三个格式化程序。

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

string dateFormatterPattern = dateFormatter.Patterns[0];
string timeFormatterPattern = timeFormatter.Patterns[0];

string pattern = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), dateFormatterPattern, timeFormatterPattern);

var patternFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(pattern);

string output = patternFormatter.Format(System.DateTime.Now);
```

## <a name="important-apis"></a>重要的 API

* [Windows.Globalization.DateTimeFormatting](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [DateTime](/uwp/api/windows.foundation.datetime?branch=live)

## <a name="related-topics"></a>相关主题

* [设置日期和时间格式示例](http://go.microsoft.com/fwlink/p/?LinkId=231618)