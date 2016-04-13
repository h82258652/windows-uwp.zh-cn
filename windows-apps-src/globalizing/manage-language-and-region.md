---
通过使用 Windows 提供的各种语言和区域设置，控制 Windows 如何选择 UI 资源和设置应用的 UI 元素的格式。
管理语言和区域
ms.assetid: 22D3A937-736A-4121-8285-A55DED56E594
管理语言和区域
template: detail.hbs
---

# 管理语言和区域


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813)
-   [**Windows.ApplicationModel.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022)
-   [**WinJS.Resources Namespace**](https://msdn.microsoft.com/library/windows/apps/br229779)

通过使用 Windows 提供的各种语言和区域设置，控制 Windows 如何选择 UI 资源和设置应用的 UI 元素的格式。

## <span id="Introduction"> </span> <span id="introduction"> </span> <span id="INTRODUCTION"> </span>简介


对于演示如何管理语言和区域设置的示例应用，请参阅[应用程序资源和本地化示例](http://go.microsoft.com/fwlink/p/?linkid=231501)。

Windows 用户无需从一组有限的语言中只选择一种语言。 相反，用户可以告诉 Windows 他们使用全球的任意语言，即使 Windows 本身未翻译为该语言也是如此。 用户甚至可以指定他们可以使用多种语言。

Windows 用户可以指定其所在位置，该位置可以是全球任意位置。 此外，用户可以指定在任意位置使用任意语言。 位置和语言彼此不会相互制约。 仅仅由于用户使用法语不能推断他们住在法国，或者仅仅由于用户住在法国不能推断他们更喜欢使用法语。

Windows 用户可以采用完全不同于 Windows 的其他语言来运行应用。 例如，用户可以使用西班牙语运行应用，同时 Windows 使用英语运行。

对于 Windows 应用商店应用，语言以 [BCP-47 语言标记](http://go.microsoft.com/fwlink/p/?linkid=227302)表示。 Windows 运行时、HTML 和 XAML 中的大多数 API 可返回或接受这些 BCP-47 语言标记的字符串表示形式。 另请参阅[语言的 IANA 列表](http://go.microsoft.com/fwlink/p/?linkid=227303)。

有关 Windows 应用商店专门支持的语言标记列表，请参阅[支持的语言](https://msdn.microsoft.com/library/windows/apps/jj657969)。

## <span id="Tasks"> </span> <span id="tasks"> </span> <span id="TASKS"> </span>任务


### <span id="Users_can_set_their_language_preferences."> </span> <span id="users_can_set_their_language_preferences."> </span> <span id="USERS_CAN_SET_THEIR_LANGUAGE_PREFERENCES."> </span>用户可以设置他们的语言首选项。

用户语言首选项列表是按顺序排列的语言列表，按用户希望使用的语言顺序描述了用户的语言。

用户在**“设置”**>**“时间和语言”**>**“区域和语言”**中设置该列表。 或者，他们可以使用**“控制面板”**>**“时钟、语言和区域”**。

用户的语言首选项列表可以包含多种语言和区域性或其他形式的特定变体。 例如，用户可能更喜欢 fr-CA，但也可以理解 en-GB。

### <span id="Specify_the_supported_languages_in_the_app_s_manifest."> </span> <span id="specify_the_supported_languages_in_the_app_s_manifest."> </span> <span id="SPECIFY_THE_SUPPORTED_LANGUAGES_IN_THE_APP_S_MANIFEST."> </span>在应用的清单中指定支持的语言。

在应用的清单文件（通常为 Package.appxmanifest）的 [**Resources element**](https://msdn.microsoft.com/library/windows/apps/dn934770) 中指定应用的支持的语言列表，或者 Visual Studio 基于在项目中找到的语言在清单文件中自动生成语言列表。 清单应以相应的精度级别准确描述支持的语言。 在清单中列出的语言是在 Windows 应用商店中显示给用户的语言。

### <span id="Specify_the_default_language."> </span> <span id="specify_the_default_language."> </span> <span id="SPECIFY_THE_DEFAULT_LANGUAGE."> </span>指定默认语言。

在 Visual Studio 中打开 package.appxmanifest、转到**“应用程序”**选项卡，然后将默认语言设置为要用于创作你的应用程序的语言。

当应用不支持用户选择的任何语言时，应用使用默认语言。 Visual Studio 使用默认语言将元数据添加到采用该语言标记的资源中，以便在运行时选择适当的资源。

还必须在清单中将默认语言属性设置为第一语言，以便正确地设置应用程序语言（如下面的步骤“创建应用程序语言列表”中所述）。 默认语言中的资源仍必须使用其语言进行限定（例如，en-US/logo.png）。 默认语言不指定非限定资源的隐式语言。 若要了解详细信息，请参阅[如何使用限定符命名资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)。

### <span id="Qualify_resources_with_their_language."> </span> <span id="qualify_resources_with_their_language."> </span> <span id="QUALIFY_RESOURCES_WITH_THEIR_LANGUAGE."> </span>使用资源的语言限定资源。

仔细考虑你的受众以及要面向的用户的语言和位置。 许多生活在某个地区的人们并不偏好该地区的主要语言。 例如，美国有数百万家庭的主要语言是西班牙语。

当你使用语言限定资源时：

-   当没有为语言定义禁止脚本值时，请包含脚本。 有关语言标记详细信息，请参阅 [IANA 子标记注册表](http://go.microsoft.com/fwlink/p/?linkid=227303)。 例如，请使用 zh-Hant、zh-Hant-TW 或 zh-Hans，而不是 zh-CN 或 zh-TW。
-   使用语言标记所有语言内容。 默认语言项目属性不是未标记资源的语言（即，中性语言）；它指定在没有任何其他标记的语言与用户匹配时，应选择哪些标记语言资源。

使用准确的内容表示形式标记资源。

-   Windows 执行复杂的匹配（包括跨区域变体（如 en-US 到 en-GB）），因此应用程序可以使用准确的内容表示形式自由标记资源，并让 Windows 为每个用户进行相应的匹配。
-   Windows 应用商店向查看应用程序的用户显示清单中的内容。
-   请注意，某些工具和其他组件（如机器翻译程序）可能会发现特定的语言标记（如区域方言信息）在理解数据方面很有帮助。
-   请确保使用完整的详细信息标记资产，特别是在提供多个变量时。 例如，请标记 en-GB 和 en-US（如果这两者都是特定于该地区的）。
-   对于只有一种标准方言的语言，无需添加地区。 在某些情况下，通用标记是合理的，如使用 ja 而不是 ja-JP 来标记资产。

有时会存在并非所有资源都需要进行本地化的情形。

-   对于在所有语言中都提供的资源（如 UI 字符串），请使用它们所使用的相应语言标记它们，并确保让所有这些资源都使用默认语言。 不需要指定中性资源（不使用语言标记的资源）。
-   对于在整个应用程序的语言集的某个子集中提供的资源（部分本地化），请指定提供资源时所使用的语言集，并确保所有这些资源都使用默认语言。 Windows 通过按用户的首选顺序查看用户所讲的所有语言，挑选可能最适合该用户的语言。 例如，如果应用的整个资源集都使用西班牙语，那么并非应用的全部 UI 都可以本地化为加泰罗尼亚语。 对于将泰罗尼亚语排在西班牙语前面的用户来说，无法用加泰罗尼亚语提供的资源将以西班牙语显示。
-   对于资源在某些语言中存在特定例外，而其他所有语言都映射到常见资源的情况，应该使用未确定语言标记“und”来标记应该用于所有语言的资源。 Windows 以类似于“\*”的方式解释“und”语言标记，因为它可以匹配其他任何特定匹配后面的首个应用程序语言。 例如，如果某些资源（如元素的宽度）对于芬兰语是不同的，但是资源的剩余部分对于所有语言是相同的，那么芬兰语资源应该使用芬兰语语言标记进行标记，而剩余部分应该使用“und”进行标记。
-   对于基于语言的脚本而不是基于语言的资源（如文本的字体或高度），请将未确定的语言标记与某个指定的脚本一起使用：“und-<script>”。 例如，对于拉丁文字体，请使用 und-Latn\\fonts.css，而对于西里尔文字体，请使用 und-Cryl\\fonts.css。

### <span id="Create_the_application_language_list."> </span> <span id="create_the_application_language_list."> </span> <span id="CREATE_THE_APPLICATION_LANGUAGE_LIST."> </span>创建应用程序语言列表。

在运行时，系统将确定应用在其清单中声明支持的用户语言首选项，然后创建一个*应用程序语言列表*。 它使用此列表来确定应用程序应该使用的语言。 该列表确定用于应用和系统资源、日期、时间和数字以及其他组件的语言。 例如，资源管理系统（[**Windows.ApplicationModel.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022)、[**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) 和 [**WinJS.Resources 命名空间**](https://msdn.microsoft.com/library/windows/apps/br229779)）根据应用程序语言加载 UI 资源。 [
            **Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) 也根据应用程序语言列表选择格式。 通过使用 [**Windows.Globalization.ApplicationLanguages.Languages**](https://msdn.microsoft.com/library/windows/apps/hh972396) 提供应用程序语言列表。

语言与资源的匹配十分复杂。 我们建议让 Windows 处理匹配，因为语言标记存在多种可能影响匹配优先级的可选组件，在实践中可能会遇到这些组件。

使用某个语言标记的可选组件的示例有：

-   用于禁止脚本语言的脚本。 例如，en-Latn-US 与 en-US 匹配。
-   区域。 例如，en-US 与 en 匹配。
-   变体。 例如，de-DE-1996 与 de-DE 匹配。
-   -x 和其他扩展名。 例如，en-US-x-Pirate 与 en-US 匹配。

对于不采用 xx 或 xx-yy 形式的语言标记，也存在许多组件，且并非全部匹配。

-   zh-Hant 与 zh-Hans 不匹配。

Windows 以一个标准的易于理解的方式排定语言匹配的优先顺序。 例如，按优先顺序，en-US 依次与 en-US、en、en-GB 等等匹配。

-   Windows 执行跨区域匹配。 例如，en-US 与 en-US 匹配，然后依次与 en、en-\* 匹配。
-   Windows 提供了一些额外数据，它们可用于区域内（如某种语言的主要区域）的相关性匹配。 例如，fr-FR 比 fr-CA 更匹配 fr-BE。
-   如果你使用 Windows API，则可以免费获取日后 Windows 在语言匹配方面的任何改进。

在与列表中的首个语言匹配之后才会与列表中第二个语言匹配，对于其他区域变体也是如此。 例如，如果应用程序语言为 en-US，则会先于 fr-CA 资源选择用于 en-GB 的资源。 仅当没有用于 en 形式的资源时才选择用于 fr-CA 的资源。

应用程序语言列表设置为用户的区域变体，尽管该变体不同于应用提供的区域变体。 例如，如果用户使用 en-GB，但应用支持 en-US，则应用程序语言列表将包含 en-GB。 这将确保日期、时间和数字的格式更接近用户的期望 (en-GB)，但仍然使用应用支持的语言 (en-US) 加载 UI 资源（由于语言匹配）。

应用程序语言列表由以下项目组成：

1.  **（可选）主要语言替代** [**PrimaryLanguageOverride**](https://msdn.microsoft.com/library/windows/apps/hh972398)是一个简单的替代设置，它用于让用户独立选择语言的应用，或者有充分理由替代默认语言选择的应用。 若要了解详细信息，请参阅[应用程序资源和本地化示例](http://go.microsoft.com/fwlink/p/?linkid=231501)。
2.  **受应用支持的用户语言。** 这是一个用户的语言首选项列表，它以语言首选项顺序排列。 它通过应用清单中的受支持语言列表进行筛选。 通过受应用支持的语言筛选用户的语言可以保持以下对象之间的一致性：软件开发工具包 (SDK)、类库、从属框架包以及应用。
3.  **如果 1 和 2 为空，则使用默认语言或第一个受应用支持的语言。** 如果用户不会说应用支持的任何语言，则选择应用支持的第一个语言作为应用程序语言。

有关示例，请参阅下面的“备注”部分。

### <span id="Set_the_HTTP_Accept_Language_header."> </span> <span id="set_the_http_accept_language_header."> </span> <span id="SET_THE_HTTP_ACCEPT_LANGUAGE_HEADER."> </span>设置 HTTP 接受的语言标头。

从 Windows 应用商店应用和桌面应用发出的典型 Web 请求和 XMLHttpRequest (XHR) 中的 HTTP 请求使用标准的 HTTP Accept-Language 标头。 默认情况下，HTTP 标头设置为在**“设置”**>**“时间和语言”**>**“区域和语言”**中指定的用户语言首选项（按用户的首选顺序排列）。 列表中的每种语言进一步扩展为包含中性语言和权重 (q)。 例如，fr-FR 和 en-US 的用户语言列表会产生 fr-FR、fr、en-US、en 的 HTTP Accept-Language 标头（“fr-FR,fr;q=0.8,en-US;q=0.5,en;q=0.3”）。

### <span id="Use_the_APIs_in_the_Windows.Globalization_namespace."> </span> <span id="use_the_apis_in_the_windows.globalization_namespace."> </span> <span id="USE_THE_APIS_IN_THE_WINDOWS.GLOBALIZATION_NAMESPACE."> </span>使用 Windows.Globalization 命名空间中的 API。

通常，[**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) 命名空间中的 API 元素使用应用程序语言列表确定语言。 如果没有任何一种语言有匹配的格式，则使用用户区域设置。 该区域设置即系统时钟所使用的区域设置。 **“设置”**>**“时间和语言”**>**“区域和语言”**>**“其他日期、时间和区域设置”**>**“区域：更改日期、时间或数字格式”**中提供了用户区域设置。 **Windows.Globalization** API 还接受替代来指定要使用的语言列表，而不使用应用程序语言列表。

[
            **Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) 还具有作为帮助程序对象提供的 [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) 对象。 它允许应用检查有关语言的详细信息，例如语言的脚本、显示名称和本地名称。

### <span id="Use_geographic_region_when_appropriate."> </span> <span id="use_geographic_region_when_appropriate."> </span> <span id="USE_GEOGRAPHIC_REGION_WHEN_APPROPRIATE."> </span>在适当的时候使用地理区域。

你可以使用用户的主地理区域设置来选择要显示给用户的内容，而不是语言。 例如，资讯应用可能会默认显示来自用户主位置的内容，该位置在 Windows 安装时设置并在**“区域：更改日期、时间或数字格式”**下的 Windows UI 中提供，如上一任务所述。 你可以使用 [**Windows.System.UserProfile.GlobalizationPreferences.HomeGeographicRegion**](https://msdn.microsoft.com/library/windows/apps/br241829) 检索当前用户的主区域设置。

[
            **Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) 还具有作为帮助程序对象提供的 [**GeographicRegion**](https://msdn.microsoft.com/library/windows/apps/br206795) 对象。 它允许应用检查有关特定区域的详细信息，例如其显示名称、本地名称以及使用的货币。

## <span id="Remarks"> </span> <span id="remarks"> </span> <span id="REMARKS"> </span>备注


下表包含用户针对各种语言和区域设置在应用的 UI 中看到的内容的示例。

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
<th align="left">应用支持的语言（在清单中定义）</th>
<th align="left">用户语言首选项（在控制面板中设置）</th>
<th align="left">应用的主要语言替代（可选）</th>
<th align="left">应用语言</th>
<th align="left">用户在应用中看到的内容</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">英语(大不列颠) (默认)； 德语(德国)</td>
<td align="left">英语(大不列颠)</td>
<td align="left">无</td>
<td align="left">英语(英国)</td>
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
<td align="left">英语(加拿大)；法语(加拿大)</td>
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
<td align="left">加泰罗尼亚语；法语(法国)</td>
<td align="left">UI：大部分使用加泰罗尼亚语，部分使用法语(法国)，因为不是所有的字符串都使用加泰罗尼亚语<br>日期/时间/数字：加泰罗尼亚语</td>
</tr>
<tr>
<td align="left">英语(英国)（默认）；法语(法国)；德语(德国)</td>
<td align="left">德语(德国)； 英语(英国)</td>
<td align="left">英语(英国)（用户在应用的 UI 中选择）</td>
<td align="left">英语(英国)；德语(德国)</td>
<td align="left">UI：英语(英国)（语言替代）<br>日期/时间/数字英语(英国)</td>
</tr>
</tbody>
</table>

 

## <span id="related_topics"> </span>相关主题


* [BCP-47 语言标记](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [语言的 IANA 列表](http://go.microsoft.com/fwlink/p/?linkid=227303)
* [应用程序资源和本地化示例](http://go.microsoft.com/fwlink/p/?linkid=231501)
* [支持的语言](https://msdn.microsoft.com/library/windows/apps/jj657969)
 

 





<!--HONumber=Mar16_HO1-->


