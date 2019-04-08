---
Description: 你可以使用几种 URI（统一资源标识符）方案引用来自应用包、应用的数据文件夹或云的文件。 还可以使用 URI 方案引用从应用的资源文件 (.resw) 加载的字符串。
title: URI 方案
template: detail.hbs
ms.date: 10/16/2017
ms.topic: article
keywords: windows 10, uwp, 资源, 图像, 资产, MRT, 限定符
ms.localizationpriority: medium
ms.openlocfilehash: b449179468d26c357e69ad1d8868004cadd6e2fa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632052"
---
# <a name="uri-schemes"></a>URI 方案

你可以使用几种 URI（统一资源标识符）方案引用来自应用包、应用的数据文件夹或云的文件。 还可以使用 URI 方案引用从应用的资源文件 (.resw) 加载的字符串。 你可以在代码中、在 XAML 标记中、在应用包清单中或在磁贴和 toast 通知模板中使用这些 URI 方案。

## <a name="common-features-of-the-uri-schemes"></a>URI 方案的常见功能

本主题中所述的所有方案均遵循适用于标准化和资源检索的典型的 URI 方案规则。 请参阅 [RFC 3986](https://go.microsoft.com/fwlink/p/?LinkId=263444) 获取 URI 的常规语法。

所有 URI 方案均按照 [RFC 3986](https://go.microsoft.com/fwlink/p/?LinkId=263444) 定义作为 URI 颁发机构和路径组件的层次结构部分。

```syntax
URI         = scheme ":" hier-part [ "?" query ] [ "#" fragment ]
hier-part   = "//" authority path-abempty
            / path-absolute
            / path-rootless
            / path-empty
```

这意味着 URI 基本上有三个组件。 URI *方案*的两个正斜杠后紧跟着名称为*颁发机构*的组件（可以为空）。 之后紧跟*路径*。 以 URI `http://www.contoso.com/welcome.png` 为例，方案为“`http://`”，颁发机构为“`www.contoso.com`”，路径为“`/welcome.png`”。 另一个示例是 URI `ms-appx:///logo.png`，其中颁发机构组件为空并采用默认值。

本主题中所述的具体方案的 URI 处理忽略了片段组件。 在资源检索和比较过程中，片段组件没有任何影响。 但是，特定实现上方的层可以解释片段以检索辅助资源。

对所有 IRI 组件进行标准化后按字节进行比较。

## <a name="case-insensitivity-and-normalization"></a>不区分大小写和标准化

本主题中所述的所有 URI 方案均遵循适用于方案标准化和资源检索的典型的 URI 规则 (RFC 3986)。 这些 URI 的标准化形式保持大小写，并对 RFC 3986 未保留的字符进行百分比解码。

对于本主题所述的所有 URI 方案，*方案*、*颁发机构*和*路径*按照标准不区分大小写，或者由系统以不区分大小写的方式进行处理。 **注意**该规则的唯一例外是 `ms-resource` 的*颁发机构*区分大小写。

## <a name="ms-appx-and-ms-appx-web"></a>ms-appx 和 ms-appx-web

使用 `ms-appx` 或 `ms-appx-web` URI 方案引用来自应用包的文件（请参阅[打包应用](../packaging/index.md)）。 应用包中的文件通常为静态图像、数据、代码和布局文件。 `ms-appx-web` 方案与 `ms-appx` 访问相同的文件，但是前者在 Web 隔离舱中访问。 有关示例和详细信息，请参阅[引用 XAML 标记和代码中的图像或其他资产](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)。

### <a name="scheme-name-ms-appx-and-ms-appx-web"></a>方案名称（ms-appx 和 ms-appx-web）

URI 方案名称为字符串“ms-appx”或“ms-appx-web”。

```xml
ms-appx://
```

```xml
ms-appx-web://
```

### <a name="authority-ms-appx-and-ms-appx-web"></a>颁发机构（ms-appx 和 ms-appx-web）

颁发机构为在程序包清单中定义的程序包标识名称。 因此在 URI 和 IRI（国际化资源标识符）形式中都限制为在程序包标识名称中允许的字符组。 程序包名称必须是在当前运行的应用的程序包依赖关系图中的其中一个包的名称。

```xml
ms-appx://Contoso.MyApp/
ms-appx-web://Contoso.MyApp/
```

如果颁发机构中出现任何其他字符，则检索和比较会失败。 颁发机构的默认值是当前运行的应用的程序包。

```xml
ms-appx:///
ms-appx-web:///
```

### <a name="user-info-and-port-ms-appx-and-ms-appx-web"></a>用户信息和端口（ms-appx 和 ms-appx-web）

`ms-appx` 方案与其他常用的方案不同，它不定义用户信息或端口组件。 由于“@" and ":”不允许作为有效的颁发机构值使用，因此如果将其包括在其中，查找将失败。 以下各项均无法正常工作。

```xml
ms-appx://john@contoso.myapp/default.html
ms-appx://john:password@contoso.myapp/default.html
ms-appx://contoso.myapp:8080/default.html
ms-appx://john:password@contoso.myapp:8080/default.html
```

### <a name="path-ms-appx-and-ms-appx-web"></a>路径（ms-appx 和 ms-appx-web）

路径组件匹配通用 RFC 3986 语法，并支持 IRI 中的非 ASCII 字符。 路径组件定义文件的逻辑或物理文件路径。 该文件位于与应用包的安装位置关联的文件夹中，适用于颁发机构指定的应用。

如果路径引用物理路径和文件名，则检索该物理文件资产。 但如果找不到任何此类物理文件，则在运行时使用内容协商确定在检索过程中返回的实际资源。 这确定基于应用、操作系统和用户设置（如语言、显示比例系数、主题、高对比度和其他运行时上下文）。 例如，确定要检索的实际资源值时，可能会综合考虑应用的语言、系统的显示设置以及用户的高对比度设置。

```xml
ms-appx:///images/logo.png
```

上述 URI 实际上可能使用以下物理文件名检索当前应用包中的文件。

```
\Images\fr-FR\logo.scale-100_contrast-white.png
```

当然，也可以通过直接引用全名的方式检索该相同的物理文件。

```xaml
<Image Source="ms-appx:///images/fr-FR/logo.scale-100_contrast-white.png"/>
```

`ms-appx(-web)` 的路径组件同通用 URI 一样区分大小写。 但是，当访问资源所用的基本文件系统不区分大小写时，例如对于 NTFS，检索资源时将不区分大小写。

URI 的标准化形式保持大小写，并对 RFC 3986 未保留的字符进行百分比解码（“%”符号后紧跟两位十六进制表示形式）。 字符“？”、“#”、“/”、“*”和‘”’（双引号）在路径中必须为百分比编码以表示文件或文件名等数据。 所有百分比编码字符在检索前解码。 因此，要检索名称为 Hello#World.html 的文件，请使用此 URI。

```xml
ms-appx:///Hello%23World.html
```

### <a name="query-ms-appx-and-ms-appx-web"></a>查询（ms-appx 和 ms-appx-web）

在资源检索过程中忽略查询参数。 query 参数的标准化形式保持大小写。 在比较过程中不忽略查询参数。

## <a name="ms-appdata"></a>ms-appdata

使用 `ms-appdata` URI 方案引用来自应用的本地、漫游和临时数据文件夹的文件。 有关这些应用数据文件夹的详细信息，请参阅[存储和检索设置以及其他应用数据](../design/app-settings/store-and-retrieve-app-data.md)。

`ms-appdata` URI 方案不执行 [ms appx 和 ms appx web](#ms-appx-and-ms-appx-web) 所执行的运行时内容协商。 但你可以响应 [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) 的内容并使用它们在 URI 中的完整物理文件名加载相应资产。

### <a name="scheme-name-ms-appdata"></a>方案名称 (ms-appdata)

URI 方案名称为字符串“ms-appdata”。

```xml
ms-appdata://
```

### <a name="authority-ms-appdata"></a>颁发机构 (ms-appdata)

颁发机构为在程序包清单中定义的程序包标识名称。 因此在 URI 和 IRI（国际化资源标识符）形式中都限制为在程序包标识名称中允许的字符组。 程序包名称必须是当前运行的应用包的名称。

```xml
ms-appdata://Contoso.MyApp/
```

如果颁发机构中出现任何其他字符，则检索和比较会失败。 颁发机构的默认值是当前运行的应用的程序包。

```xml
ms-appdata:///
```

### <a name="user-info-and-port-ms-appdata"></a>用户信息和端口 (ms-appdata)

`ms-appdata` 方案与其他常用的方案不同，它不定义用户信息或端口组件。 由于“@" and ":”不允许作为有效的颁发机构值使用，因此如果将其包括在其中，查找将失败。 以下各项均无法正常工作。

```xml
ms-appdata://john@contoso.myapp/local/data.xml
ms-appdata://john:password@contoso.myapp/local/data.xml
ms-appdata://contoso.myapp:8080/local/data.xml
ms-appdata://john:password@contoso.myapp:8080/local/data.xml
```

### <a name="path-ms-appdata"></a>路径 (ms-appdata)

路径组件匹配通用 RFC 3986 语法，并支持 IRI 中的非 ASCII 字符。 在 [Windows.Storage.ApplicationData](/uwp/api/Windows.Storage.ApplicationData?branch=live) 位置中有三个保留的文件夹用于本地、漫游和临时状态存储。 `ms-appdata` 方案允许访问这些位置的文件和文件夹。 路径组件的第一段必须按以下方式指定特定文件夹。 因此“hier-part”的“path-empty”形式不合法。

本地文件夹。

```xml
ms-appdata:///local/
```

临时文件夹。

```xml
ms-appdata:///temp/
```

漫游文件夹。

```xml
ms-appdata:///roaming/
```

`ms-appdata` 的路径组件同通用 URI 一样区分大小写。 但是，当访问资源所用的基本文件系统不区分大小写时，例如对于 NTFS，检索资源时将不区分大小写。

URI 的标准化形式保持大小写，并对 RFC 3986 未保留的字符进行百分比解码（“%”符号后紧跟两位十六进制表示形式）。 字符“？”、“#”、“/”、“*”和‘”’（双引号）在路径中必须为百分比编码以表示文件或文件名等数据。 所有百分比编码字符在检索前解码。 因此，要检索名称为 Hello#World.html 的本地文件，请使用此 URI。

```xml
ms-appdata://local/Hello%23World.html
```

资源的检索以及顶级路径段的标识在点标准化（“.././b/c”）后进行处理。 因此，URI 不能在其中一个保留的文件夹对它们自身使用点。 因此不允许使用以下 URI。

```xml
ms-appdata:///local/../hello/logo.png
```

但允许此 URI（尽管冗余）。

```xml
ms-appdata:///local/../roaming/logo.png
```

### <a name="query-ms-appdata"></a>查询 (ms-appdata)

在资源检索过程中忽略查询参数。 query 参数的标准化形式保持大小写。 在比较过程中不忽略查询参数。

## <a name="ms-resource"></a>ms-resource

使用 `ms-resource` URI 方案引用从应用的资源文件 (.resw) 加载的字符串。 有关资源文件的示例和详细信息，请参阅[本地化 UI 和应用包清单中的字符串](localize-strings-ui-manifest.md)。

### <a name="scheme-name-ms-resource"></a>方案名称 (ms-resource)

URI 方案名称为字符串“ms-resource”。

```xml
ms-resource://
```

### <a name="authority-ms-resource"></a>颁发机构 (ms-resource)

颁发机构是在包资源索引 (PRI) 中定义的顶级资源映射，它通常对应在程序包清单中定义的程序包标识名称。 请参阅[打包应用](../packaging/index.md)。 因此在 URI 和 IRI（国际化资源标识符）形式中都限制为在程序包标识名称中允许的字符组。 程序包名称必须是在当前运行的应用的程序包依赖关系图中的其中一个包的名称。

```xml
ms-resource://Contoso.MyApp/
ms-resource://Microsoft.WinJS.1.0/
```

如果颁发机构中出现任何其他字符，则检索和比较会失败。 颁发机构的默认值是当前运行的应用的区分大小写的包名称。

```xml
ms-resource:///
```

颁发机构区分大小写，并且其标准化形式保持其大小写。 但查找资源时不区分大小写。

### <a name="user-info-and-port-ms-resource"></a>用户信息和端口 (ms-resource)

`ms-resource` 方案与其他常用的方案不同，它不定义用户信息或端口组件。 由于“@" and ":”不允许作为有效的颁发机构值使用，因此如果将其包括在其中，查找将失败。 以下各项均无法正常工作。

```xml
ms-resource://john@contoso.myapp/Resources/String1
ms-resource://john:password@contoso.myapp/Resources/String1
ms-resource://contoso.myapp:8080/Resources/String1
ms-resource://john:password@contoso.myapp:8080/Resources/String1
```

### <a name="path-ms-resource"></a>路径 (ms-resource)

路径标识 [ResourceMap](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap?branch=live) 子树（请参阅[资源管理系统](https://msdn.microsoft.com/library/windows/apps/jj552947)）和其中的 [NamedResource](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource?branch=live) 的分层位置。 通常情况下，它对应资源文件 (.resw) 的文件名（不包括扩展）和其中的字符串资源的标识符。

有关示例和详细信息，请参阅[本地化 UI 和应用包清单中的字符串](localize-strings-ui-manifest.md)和[磁贴和 toast 通知的语言、比例和高对比度支持](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)。

`ms-resource` 的路径组件同通用 URI 一样区分大小写。 但却提供了基础检索[CompareStringOrdinal](https://msdn.microsoft.com/library/windows/apps/br224628)与*ignoreCase*设置为`true`。

URI 的标准化形式保持大小写，并对 RFC 3986 未保留的字符进行百分比解码（“%”符号后紧跟两位十六进制表示形式）。 字符“？”、“#”、“/”、“*”和‘”’（双引号）在路径中必须为百分比编码以表示文件或文件名等数据。 所有百分比编码字符在检索前解码。 因此，若要从资源文件中检索字符串资源名为`Hello#World.resw`，使用此 URI。

```xml
ms-resource:///Hello%23World/String1
```

### <a name="query-ms-resource"></a>查询 (ms-resource)

在资源检索过程中忽略查询参数。 query 参数的标准化形式保持大小写。 在比较过程中不忽略查询参数。 查询参数在比较时区分大小写。

在此 URI 分析上分层的特定组件的开发人员在合适的情况下可以选择使用查询参数。

## <a name="related-topics"></a>相关主题

* [统一资源标识符 (URI):常规语法](https://go.microsoft.com/fwlink/p/?LinkId=263444)
* [打包应用](../packaging/index.md)
* [从 XAML 标记和代码中引用图像或其他资产](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)
* [存储和检索设置和其他应用程序数据](../design/app-settings/store-and-retrieve-app-data.md)
* [对 UI 和应用包清单中的字符串进行本地化](localize-strings-ui-manifest.md)
* [资源管理系统](https://msdn.microsoft.com/library/windows/apps/jj552947)
* [磁贴和 toast 通知支持的语言、 缩放性和高对比度](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)