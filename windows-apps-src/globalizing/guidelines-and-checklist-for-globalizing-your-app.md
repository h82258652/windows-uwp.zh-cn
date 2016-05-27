---
author: DelfCo
Description: 在将你的应用全球化使其适用于更广泛的用户以及将你的应用本地化使其适用于特定市场时，请遵循这些最佳做法。
Search.Refinement.TopicID: 180
title: 全球化和本地化指南
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
label: Do's and don'ts
template: detail.hbs
---

# 全球化和本地化的应做事项和禁止事项





**重要的 API**

-   [**全球化**](https://msdn.microsoft.com/library/windows/apps/br206813)
-   [**Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)
-   [**Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
-   [**资源**](https://msdn.microsoft.com/library/windows/apps/br206022)
-   [**Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039)

在将你的应用全球化使其适用于更广泛的用户以及将你的应用本地化使其适用于特定市场时，请遵循这些最佳做法。



## <span id="guidelines_for_internationalization"></span><span id="GUIDELINES_FOR_INTERNATIONALIZATION"></span>全球化

准备好你的应用，使其轻松适应不同的市场，方法包括：为你的 UI 选择在全球范围都适合的字词和图像、使用 [**Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) API 格式化应用数据，以及避免基于位置或语言的假设。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">建议</th>
<th align="left">说明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>使用正确的数字、日期、时间、地址和电话号码格式。</p></td>
<td align="left"><p>数字、日期、时间和其他形式数据采用的格式因文化、地区、语言和市场的不同而不同。 如果要显示数字、日期、时间或其他数据，请使用 [<strong>Globalization</strong>](https://msdn.microsoft.com/library/windows/apps/br206813) API 获取适用于特定受众的格式。</p></td>
</tr>
<tr class="even">
<td align="left"><p>支持国际纸张尺寸。</p></td>
<td align="left"><p>最常用的纸张尺寸因国家/地区的不同而不同，因此如果包含依赖纸张尺寸的功能，例如打印，则请确保支持和测试常用国际尺寸。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>支持国际度量单位和货币单位。</p></td>
<td align="left"><p>尽管最流行的度量单位为公制和英制，但不同的国家/地区会采用不同的度量单位和尺度。 如果你需要处理度量问题（例如长度、温度或面积），请使用 [<strong>CurrenciesInUse</strong>](https://msdn.microsoft.com/library/windows/apps/br206793) 属性校正系统度量。</p></td>
</tr>
<tr class="even">
<td align="left"><p>正确地显示文本和字体。</p></td>
<td align="left"><p>理想的字体、字体大小和文本方向因市场的不同而不同。</p>
<p>有关详细信息，请参阅 [<strong>调整布局和字体并支持 RTL</strong>] (adjust-layout-and-fonts--and-support-rtl.md)。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>使用 Unicode 进行字符编码。</p></td>
<td align="left"><p>默认情况下，最新版本的 Microsoft Visual Studio 针对所有文档使用 Unicode 字符编码。 如果使用的是不同的编辑器，则请确保在适当的 Unicode 字符编码中保存源文件。 所有 Windows 运行时 API 均返回 UTF-16 编码字符串。</p></td>
</tr>
<tr class="even">
<td align="left"><p>记录输入的语言。</p></td>
<td align="left"><p>当应用询问用户有关文本输入事宜时，会记录输入的语言。 这将确保在稍后显示输入时，将为用户采用正确的格式进行显示。 使用 [<strong>CurrentInputMethodLanguage</strong>](https://msdn.microsoft.com/library/windows/apps/hh700658) 属性获取当前输入语言。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>请勿通过语言假定用户的位置，并且请勿通过位置假定用户的语言。</p></td>
<td align="left"><p>在 Windows 中，用户的语言和位置是单独的概念。 用户可以使用某种语言的特定区域变体，例如在大不列颠使用的 en-gb 形式的英语，而用户却可以位于完全不同的国家或地区。 请考虑应用是否需要有关用户语言的知识，例如针对 UI 文本、或位置，例如用于许可问题。</p>
<p>有关详细信息，请参阅 [<strong>管理语言和区域</strong>](manage-language-and-region.md)。</p></td>
</tr>
<tr class="even">
<td align="left"><p>请勿使用俗语和比喻。</p></td>
<td align="left"><p>特定于某种人群的语言（如文化和年龄）很难理解或翻译，因为只有属于该类人群的人才会使用该语言。 同样，比喻可能对于一个人而言有意义，而对其他人而言没有意义。 例如，&quot;蓝鸟&quot;对于了解滑雪文化的人而言表示特定意义，但对于不了解该文化的人而言则没有意义。 如果计划本地化你的应用并且采用的是口语化的语言或语气，则确保你向本地化者充分解释要翻译的意义和语言。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>请勿使用技术行话、缩写或缩略语。</p></td>
<td align="left"><p>技术语言很有可能不会被来自其他文化或区域的非技术用户或人员所理解，并且也很难翻译。 人们在日常对话中不会采用这样的说法。 技术语言常常显示在错误消息中，用于标识硬件和软件问题。 有时，这是必要的，但应将字符串重写为非技术性的。</p></td>
</tr>
<tr class="even">
<td align="left"><p>不要使用可能存在冒犯性的图像。</p></td>
<td align="left"><p>适合你自己的文化的图像可能在其他文化中具有冒犯性或会被曲解。 避免使用宗教符号、动物或与国旗或政治运动关联的颜色的组合。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>在地图中或在涉及地理区域时避免政治冒犯。</p></td>
<td align="left"><p>地图可能包含有争议的地理区域或国界，并且这是引起政治冲突的常见原因。 请务必小心用于选择国家/地区的任何 UI，应将其称为&quot;国家/地区&quot;。 将有争议的领土置于标记为&quot;国家/地区&quot;的列表中（例如地址表单中）可能会导致纠纷。</p></td>
</tr>
<tr class="even">
<td align="left"><p>请勿使用与自身比较的字符串来比较语言标记。</p></td>
<td align="left"><p>BCP-47 语言标记非常复杂。 在比较语言标记时会存在许多问题，包括匹配脚本信息、旧标记以及多个地区变体的问题。 Windows 中的资源管理系统负责进行匹配。 你可以指定一组资源（以任何语言），而系统将为用户和应用选择合适的资源。</p>
<p>有关资源管理的详细信息，请参阅 [<strong>定义应用资源</strong>](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321)。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>请勿假定始终按字母顺序进行排序。</p></td>
<td align="left"><p>对于不使用拉丁语脚本的语言，排序基于诸如发音、笔划数和其他因素的因素。 即使使用拉丁语脚本的语言也不是始终使用字母顺序排序。 例如，在某些文化中，电话簿可能不是按照字母顺序排序。 系统可以为你处理排序问题，但如果你创建自己的排序算法，请确保要将你的目标市场使用的排序方法考虑在内。</p></td>
</tr>
</tbody>
</table>

 

## <span id="guidelines_for_localization"></span><span id="GUIDELINES_FOR_LOCALIZATION"></span>本地化

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">建议</th>
<th align="left">说明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>将资源（例如 UI 字符串和图像）与代码分开。</p></td>
<td align="left"><p>对应用进行设计，以便将资源（例如，字符串和图像）与代码分开。 这样，你就可以分别对这些资源进行维护、本地化，并针对不同的比例因子、辅助功能选项以及大量其他用户和计算机上下文进行自定义。</p>
<p>将字符串与应用的代码分离，以创建独立于语言的单独的代码库。 始终将字符串与应用代码和标记分开，并将其放入资源文件，如 ResW 或 ResJSON 文件。</p>
<p>在 Windows 中使用资源基础结构来处理选择最适合的资源以匹配用户的运行时环境。</p></td>
</tr>
<tr class="even">
<td align="left"><p>隔离其他可本地化的资源文件。</p></td>
<td align="left"><p>获取需要本地化的其他文件，例如包含要翻译的文本或由于文化敏感性而需要进行更改的图像，并将这些文件置于用语言名称标记的文件夹中。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>设置默认语言，并标记所有资源，甚至包括默认语言中的资源。</p></td>
<td align="left"><p>始终在应用清单 (package.appxmanifest) 中为应用适当地设置默认语言。 默认语言确定当用户不使用应用的所有支持语言时要使用的语言。 使用语言标记默认语言资源（例如 zh-cn/Logo.png），以便系统可以通知语言哪些资源使用该语言以及在特定情况下如何使用该语言。</p></td>
</tr>
<tr class="even">
<td align="left"><p>确定需要本地化的应用的资源。</p></td>
<td align="left"><p>如果应用要针对其他市场进行本地化，则需要作出哪些更改？ 文本字符串需要翻译为其他语言。 图像可能需要根据其他文化作出调整。 请考虑本地化将如何影响应用使用的其他资源，例如音频或视频。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>在代码和标记中使用资源标识符来引用资源。</p></td>
<td align="left"><p>在标记中不针对图像使用字符串文字或特定的文件名称，而应使用对资源的引用。 请确保针对每个资源使用唯一标识符。 有关详细信息，请参阅 [<strong>如何使用限定符命名资源</strong>](https://msdn.microsoft.com/library/windows/apps/xaml/Hh965324)。</p>
<p>侦听在系统更改以及它开始使用一组不同的限定符时触发的事件。 重新处理文档，加载正确的资源。</p></td>
</tr>
<tr class="even">
<td align="left"><p>使文本大小可以增加。</p></td>
<td align="left"><p>动态分配文本缓冲区，因为在翻译后文本大小可能会扩展。 如果必须使用静态缓冲区，则必须为资源保留超大空间（可能是英文字符串的两倍长度）以便在字符串翻译后可以为潜在的扩展提供足够位置。 对于用户界面可能也存在受限的可用空间问题。 若要为本地化语言提供足够位置，则应确保字符串长度要长于英语语言所需长度大约 40%。 对于实际较短的字符串，例如单个词，则可能需要 300% 的更多空间。 此外，在控件中启用多行支持和文本换行可提供更多空间来显示每个字符串。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>支持镜像。</p></td>
<td align="left"><p>文本对齐和读取顺序可以采用从左到右的方式（按照英语采用的方式）或者从右到左的方式 (RTL)，通常在阿拉伯语或希伯来语中会采用后面一种方式。 如果要将你的产品本地化为使用与你本身语言不同读取顺序的语言，请确保你的 UI 元素的布局支持镜像。 甚至诸如后退按钮、UI 过渡效果以及图像之类的项都可能需要镜像。</p>
<p>有关详细信息，请参阅 [<strong>调整布局和字体并支持 RTL</strong>] (adjust-layout-and-fonts--and-support-rtl.md)。</p></td>
</tr>
<tr class="even">
<td align="left"><p>注释字符串。</p></td>
<td align="left"><p>请确保字符串正确注释，并且只为本地化人员提供需要翻译的字符串。 过度本地化是问题的常见根源。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>使用短字符串。</p></td>
<td align="left"><p>较短的字符串更易于翻译，并使翻译结果可循环使用。 由于无需将相同的字符串再次发送给本地化人员，因此翻译结果的循环使用节约了成本。</p>
<p>某些本地化工具可能不支持长于 8192 个字符的字符串，因此请保持字符串长度为 4000 或更少。</p></td>
</tr>
<tr class="even">
<td align="left"><p>提供包含完整句子的字符串。</p></td>
<td align="left"><p>提供包含完整句子的字符串，而不要将句子分割成单独的词，因为词语的翻译依赖于其在句子中的位置。 此外，请勿假定具有多个参数的短语在每种语言中这些参数都保持相同的顺序。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>针对本地化优化图像和音频文件。</p></td>
<td align="left"><p>通过避免在图像中使用文本或在音频文件中使用语音来降低本地化成本。 如果要本地化为与你本身语言的读取方向不同的语言，请使用对称图像和效果，以便可以更容易地支持镜像。</p></td>
</tr>
<tr class="even">
<td align="left"><p>不要在不同的上下文中重复使用字符串。</p></td>
<td align="left"><p>不要在不同的上下文中重复使用字符串，因为即使是简单的词（例如 &quot;on&quot; 和 &quot;off&quot;）都可能具有不同的翻译，翻译结果依赖于上下文。</p></td>
</tr>
</tbody>
</table>

 

## <span id="related_topics"></span>相关文章


**示例**
* [应用程序资源和本地化示例](http://go.microsoft.com/fwlink/p/?linkid=254478)
* [全球化首选项示例](http://go.microsoft.com/fwlink/p/?linkid=231608)
 

 





<!--HONumber=May16_HO2-->


