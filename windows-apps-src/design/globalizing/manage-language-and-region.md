---
Description: 本主题定义了术语 "用户配置文件语言列表"、"应用程序清单语言列表" 和 "应用运行时语言列表"。 我们将在此主题和此功能区域中的其他主题中使用以上术语，因此了解它们的含义非常重要。
title: 了解用户配置文件语言和应用清单语言
ms.assetid: 22D3A937-736A-4121-8285-A55DED56E594
template: detail.hbs
ms.date: 11/08/2017
ms.topic: article
keywords: windows 10, uwp, 全球化, 可本地化性, 本地化
ms.localizationpriority: medium
ms.openlocfilehash: 46ce97ecc11d7aa04013d77ad8cbfe3e50a31f91
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970462"
---
# <a name="understand-user-profile-languages-and-app-manifest-languages"></a>了解用户配置文件语言和应用清单语言
Windows 用户可以使用**设置** > **时间 & 语言** > **区域 & 语言**来配置首选显示语言的排序列表，或只使用单个首选显示语言。 某种语言可能会具有区域变体。 例如，可以选择西班牙使用的西班牙语、墨西哥使用的西班牙语、美国使用的西班牙语等。

此外，在**设置** > **时间 & 语言** > **区域 & 语言**，但不同于语言，用户可以在世界上指定其位置（称为区域）。 请注意，显示语言（以及区域变体）设置不是地区设置的决定因素，反之亦然。 例如，尽管某位用户可能目前居住在法国，但选择的却是西班牙语（墨西哥）作为首选 Windows 显示语言。

对于 Windows 应用，语言表示为[BCP-47 语言标记](https://tools.ietf.org/html/bcp47)。 例如，BCP-47 语言标记“en-US”对应**设置**中的英语（美国）。 适当的 Windows 运行时 Api 接受并返回 BCP-47 语言标记的字符串表示形式。

另请参阅 [IANA 语言子标记注册表](https://www.iana.org/assignments/language-subtag-registry)。

以下三部分定义了术语“用户配置文件语言列表”、“应用部件清单语言列表”和“应用运行时语言列表”。 我们将在此主题和此功能区域中的其他主题中使用以上术语，因此了解它们的含义非常重要。

## <a name="user-profile-language-list"></a>用户配置文件语言列表
用户配置文件语言列表是用户在**Settings** > **Time & language** > **Region & language language** > 中配置的列表的**名称。** 在代码中，可以使用 [**GlobalizationPreferences.Languages**](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages) 属性将用户配置文件语言列表作为字符串只读列表进行访问，其中每个字符串均为单个 [BCP-47 语言标记](https://tools.ietf.org/html/bcp47)，例如“en-US”或“ja-JP”。

```csharp
    IReadOnlyList<string> userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;
```

## <a name="app-manifest-language-list"></a>应用部件清单语言列表
应用部件清单语言列表是你的应用声明（或将声明）支持的语言列表。 随着应用从开发生命周期一直到本地化的过程中，此列表会不断扩展。

尽管此列表是在编译时确定的，但可以通过两种方式来精确控制它的确定方式。 一种方式是让 Visual Studio 从项目的文件中确定此列表。 为此，首先在应用包清单源文件 (`Package.appxmanifest`) 中的**应用程序**选项卡上设置应用的**默认语言**。 然后，确认同一文件中包含此配置（默认包含）。

```xml
  <Resources>
    <Resource Language="x-generate" />
  </Resources>
```

每次 Visual Studio 生成构建的应用包清单文件 (`AppxManifest.xml`) 时，它会将源文件中的单个 `Resource` 元素扩展到在项目中所找到的所有语言限定符集中（请参阅[定制语言、比例、高对比度和其他限定符的资源](../../app-resources/tailor-resources-lang-scale-contrast.md)）。 例如，如果已开始本地化，并且具有文件夹名称或文件名称中包含“en-US”、“ja-JP”和“fr-FR”的字符串、图像和/或文件资源，则构建的 `AppxManifest.xml` 文件将包含以下内容（列表中的第一个条目为设置的默认语言）。

```xml
  <Resources>
    <Resource Language="EN-US" />
    <Resource Language="JA-JP" />
    <Resource Language="FR-FR" />
  </Resources>
```

另一种方式是将应用包清单源文件 (`Package.appxmanifest`) 中的单个“x-generate”`<Resource>` 元素替换为 `<Resource>` 元素的扩展列表（注意要首先列出默认语言）。 尽管该方法会涉及更多的维护工作，但如果使用的是自定义构建系统，这可能是合适的方法。

首先，应用部件清单语言列表将仅包含一种语言。 可能是 en-US。 但最后，由于你已手动配置清单或向项目添加已翻译的资源，因此，此列表将扩展。

当应用在 Microsoft Store 中发布后，应用部件清单语言列表中的语言就是向客户显示的语言。 有关 Microsoft Store 专门支持的 BCP-47 语言标记列表，请参阅[支持的语言](../../publish/supported-languages.md)。

在代码中，可以使用 [**ApplicationLanguages.ManifestLanguages**](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages) 属性将应用部件清单语言列表作为字符串只读列表进行访问，其中每个字符串均为单个 BCP-47 语言标记。

```csharp
    IReadOnlyList<string> userLanguages = Windows.Globalization.ApplicationLanguages.ManifestLanguages;
```

## <a name="app-runtime-language-list"></a>应用运行时语言列表
我们所关注的第三个语言列表是刚刚描述的那两个列表之间的交集。 在运行时，应用已声明支持的语言列表（应用部件清单语言列表）会与用户已声明作为首选项的语言列表（用户配置文件语言列表）进行比较。 应用运行时语言列表会设置为此交集（如果此交集不为空），或者仅设置为应用的默认语言（如果此交集为空）。

更具体地说，应用运行时语言列表是由以下项组成的。

1.  **（可选）主要语言替代**。 [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride) 是一个简单的替代设置，适用于让用户独立选择语言的应用，或者有充分理由替代默认语言选择的应用。 若要了解详细信息，请参阅[应用程序资源和本地化示例](https://code.msdn.microsoft.com/windowsapps/Application-resources-and-cd0c6eaa)。
2.  **受应用支持的用户语言**。 这是由应用部件清单语言列表筛选的用户配置文件语言列表。 通过受应用支持的语言筛选用户的语言可以保持以下对象之间的一致性：软件开发工具包 (SDK)、类库、从属框架包以及应用。
3.  **如果 1 和 2 为空，则使用默认语言或第一种受应用支持的语言**。 如果用户配置文件语言列表不包含应用支持的任何语言，应用运行时语言则为应用支持的第一种语言。

在代码中，可以使用 [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) 属性以字符串的形式访问应用运行时语言列表，其中包含以分号分隔的 BCP-47 语言标记列表。

```csharp
    string runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["Language"];
```

也可以将它作为字符串只读列表进行访问，其中每个字符串都包含一个 BCP-47 语言标记。 可以使用 [**ResourceContext.Languages**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages) 属性或 [**ApplicationLanguages.Languages**](/uwp/api/windows.globalization.applicationlanguages.Languages) 属性执行此操作。

```csharp
    IReadOnlyList<string> runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().Languages;

    runtimeLanguages = Windows.Globalization.ApplicationLanguages.Languages;
```

应用运行时语言列表确定 Windows 为应用加载的资源，以及用于设置日期、时间、数字和其他组件格式的语言。 请参阅[全球化日期/时间/数字格式](use-global-ready-formats.md)。

**注意**：如果用户配置文件语言和应用部件清单语言是彼此的区域变体，用户的区域变体则用作应用运行时语言。 例如，如果用户首选 en-GB，而应用支持 en-US，应用运行时语言则为 en-GB。 这将确保日期、时间和数字的格式更接近用户的期望 (en-GB)，但仍然会使用应用支持的语言 (en-US) 加载本地化资源（由于语言匹配）。

## <a name="qualify-resource-files-with-their-language"></a>使用资源文件的语言限定资源文件
使用语言资源限定符命名资源文件或其文件夹。 若要了解有关资源限定符的详细信息，请参阅[定制语言、比例、高对比度和其他限定符的资源](../../app-resources/tailor-resources-lang-scale-contrast.md)。 资源文件可以是图像（或其他资产），也可以是包含文本字符串的 *.resw*等资源容器文件。

**注意**甚至应用的默认语言中的资源都必须指定语言限定符。 例如，如果应用的默认语言为英语（美国），则将资产限定为`\Assets\Images\en-US\logo.png`。

- Windows 执行复杂匹配，其中包括 en-us 和 en 等区域变体。 因此，请根据需要包含区域子标记。 请参阅[资源管理系统匹配语言标记的方式](../../app-resources/how-rms-matches-lang-tags.md)。
- 如果没有为该语言定义的禁止显示脚本值，请在限定符中指定语言脚本子标记。 例如，使用 zh-chs-Zh-hant、zh-chs-Zh-hant 或 zh-chs-Hans （有关更多详细信息，请参阅[IANA 语言](https://www.iana.org/assignments/language-subtag-registry)子标记注册表），而不是 zh-chs 或 zh-chs。
- 对于具有单个标准方言的语言，无需包含区域限定符。 例如，使用 ja 而不是 ja-jp。
- 某些工具和其他组件（如机器翻译程序）可能会发现特定的语言标记（如区域方言信息）在理解数据方面很有帮助。

### <a name="not-all-resources-need-to-be-localized"></a>并非所有资源都需要本地化

并非所有资源都需要进行本地化。

- 至少，确保所有资源都以默认语言存在。
- 某些资源的子集可能足以满足紧密相关的语言（部分本地化）。 例如，如果应用的整个资源集都使用西班牙语，那么你可能不会将应用的全部 UI 本地化为加泰罗尼亚语。 对于说加泰罗尼亚语和西班牙语的用户，加泰罗尼亚语中未提供的资源将以西班牙语显示。
- 某些资源可能需要特定语言的例外，而其他大多数资源则映射到公共资源。 在这种情况下，请将旨在用于所有语言的资源标记为 "und"。 Windows 将 "und" 语言标记解释为通配符（类似于 "\*"），因为它与任何其他特定匹配项后的顶级应用程序语言匹配。 例如，如果某些资源对于芬兰语有所不同，但是这些资源的剩余部分对于所有语言都相同，应该使用芬兰语语言标记对芬兰语资源进行标记，而剩余部分则应该通过“und”进行标记。
- 对于基于语言脚本的资源（如字体或文本的高度），请将不确定的语言标记用于指定的脚本： ' und&lt;&gt;'。 例如，对于拉丁语字体，请使用 `und-Latn\\fonts.css`；对于西里尔文字体，请使用 `und-Cryl\\fonts.css`。

## <a name="set-the-http-accept-language-request-header"></a>设置 HTTP 接受的语言请求标头
请考虑所调用的 Web 服务是否具有与应用相同的本地化程度范围。 从典型 web 请求中的 Windows 应用程序和 XMLHttpRequest （XHR）发出的 HTTP 请求，使用标准的 HTTP Accept-Language 请求标头。 默认情况下，HTTP 标头设置为用户配置文件语言列表。 列表中的每种语言进一步扩展为包含中性语言和权重 (q)。 例如，fr-FR 和 en-US 的用户语言列表会产生 fr-FR、fr、en-US、en 的 HTTP 接受的语言请求标头（“fr-FR,fr;q=0.8,en-US;q=0.5,en;q=0.3”）。 但是，如果天气应用（以此为例）以法语（法国）显示 UI，但用户在其首选项列表中排在最顶端的语言是德语，你则需要从服务中显式请求使用法语（法国），以便在应用内保持一致性。

## <a name="apis-in-the-windowsglobalization-namespace"></a>Windows.Globalization 命名空间中的 API
通常，[**Windows.Globalization**](/uwp/api/windows.globalization?branch=live) 命名空间中的 API 使用应用运行时语言列表确定语言。 如果没有任何一种语言有匹配的格式，则使用用户区域设置。 该区域设置即用于系统时钟的区域设置。 用户区域设置可从**设置** > **时间 & 语言** > **区域 & 语言** > **其他日期、时间、& 区域设置** > **区域中获取：更改日期、时间或数字格式**。 **Windows.Globalization** API 还具有指定要使用的语言列表的替代，而不是应用运行时语言列表。

通过使用 [**Language**](/uwp/api/windows.globalization.language?branch=live) 类，可以检查有关特定语言的详细信息，例如语言的脚本、显示名称和本地名称。

## <a name="use-geographic-region-when-appropriate"></a>在适当的时候使用地理区域
在 "**设置** > **时间 & 语言** > **区域 & 语言** > "**国家或地区**，用户可以指定他们在世界中的位置。 可以使用此设置（而不是语言）来选择要显示给用户的内容。 例如，新应用可能会默认为从此区域显示内容。

在代码中，可以通过使用 [**GlobalizationPreferences.HomeGeographicRegion**](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion) 属性访问此设置。

通过使用 [**GeographicRegion**](/uwp/api/windows.globalization.geographicregion?branch=live) 类，可以检查有关特定区域的详细信息，例如其显示名称、本地名称以及使用的货币。

## <a name="examples"></a>示例
下表包含针对各种语言和区域设置用户将在应用的 UI 中看到的内容的示例。

<table border="1">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">应用部件清单语言列表</th>
<th align="left">用户配置文件语言列表</th>
<th align="left">应用的主要语言替代（可选）</th>
<th align="left">应用运行时语言列表</th>
<th align="left">用户在应用中看到的内容</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">英语(英国) (默认)； 德语(德国)</td>
<td align="left">英语 (英国)</td>
<td align="left">无</td>
<td align="left">英语 (英国)</td>
<td align="left">UI：英语(英国)<br>日期/时间/数字：英语(英国)</td>
</tr>
<tr>
<td align="left">德语(德国)（默认）；法语(法国)；意大利语(意大利)</td>
<td align="left">法语(奥地利)</td>
<td align="left">无</td>
<td align="left">法语(奥地利)</td>
<td align="left">UI：法语(法国)（从“法语(奥地利)”回退）<br>日期/时间/数字：法语(奥地利)</td>
</tr>
<tr>
<td align="left">英语(美国)（默认）；法语(法国)；英语(英国)</td>
<td align="left">英语(加拿大)； 法语(加拿大)</td>
<td align="left">无</td>
<td align="left">英语(加拿大)； 法语(加拿大)</td>
<td align="left">UI：英语(美国)（从“英语(加拿大)”回退）<br>日期/时间/数字：英语(加拿大)</td>
</tr>
<tr>
<td align="left">西班牙语(西班牙)（默认）；西班牙语(墨西哥)；西班牙语(拉丁美洲)；葡萄牙语(巴西)</td>
<td align="left">英语(美国)</td>
<td align="left">无</td>
<td align="left">西班牙语(西班牙)</td>
<td align="left">UI：西班牙语(西班牙)（由于没有可用于英语的回退，因此使用默认值）<br>日期/时间/数字西班牙语(西班牙)</td>
</tr>
<tr>
<td align="left">加泰罗尼亚语（默认）；西班牙语(西班牙)；法语(法国)</td>
<td align="left">加泰罗尼亚语； 法语(法国)</td>
<td align="left">无</td>
<td align="left">加泰罗尼亚语； 法语(法国)</td>
<td align="left">UI：大部分使用加泰罗尼亚语，部分使用法语(法国)，因为不是所有的字符串都使用加泰罗尼亚语<br>日期/时间/数字：加泰罗尼亚语</td>
</tr>
<tr>
<td align="left">英语(英国)（默认）；法语(法国)；德语(德国)</td>
<td align="left">德语(德国)； 英语(英国)</td>
<td align="left">英语(英国)（用户在应用的 UI 中选择）</td>
<td align="left">英语(英国)；德语(德国)</td>
<td align="left">UI：英语(英国)（语言替代）<br>日期/时间/数字 英语(英国)</td>
</tr>
</tbody>
</table>

>[!NOTE]
> 有关 Microsoft 使用的标准国家/地区代码的列表，请参阅[官方国家/地区列表](/windows/uwp/publish/supported-languages)。

## <a name="important-apis"></a>重要的 API
* [GlobalizationPreferences.Languages](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages)
* [ApplicationLanguages.ManifestLanguages](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages)
* [PrimaryLanguageOverride](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride)
* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [ResourceContext.Languages](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages)
* [ApplicationLanguages.Languages](/uwp/api/windows.globalization.applicationlanguages.Languages)
* [Windows](/uwp/api/windows.globalization?branch=live)
* [语言](/uwp/api/windows.globalization.language?branch=live)
* [GlobalizationPreferences.HomeGeographicRegion](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion)
* [GeographicRegion](/uwp/api/windows.globalization.geographicregion?branch=live)

## <a name="related-topics"></a>相关主题
* [BCP-47 语言标记](https://tools.ietf.org/html/bcp47)
* [IANA 语言子标记注册表](https://www.iana.org/assignments/language-subtag-registry)
* [定制语言、比例、高对比度和其他限定符的资源](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [支持的语言](../../publish/supported-languages.md)
* [全球化日期/时间/数字格式](use-global-ready-formats.md)
* [资源管理系统如何匹配语言标记](../../app-resources/how-rms-matches-lang-tags.md)

## <a name="samples"></a>示例
* [应用程序资源和本地化示例](https://code.msdn.microsoft.com/windowsapps/Application-resources-and-cd0c6eaa)
