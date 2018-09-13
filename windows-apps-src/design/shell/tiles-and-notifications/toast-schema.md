---
author: andrewleader
Description: The following article describes all of the properties and elements within toast content.
title: Toast 内容架构
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Toast content schema
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ad1700d58ab3568aa3aefa46b5950d0a8bf3c320
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3962785"
---
# <a name="toast-content-schema"></a>Toast 内容架构

 

以下内容将介绍了 toast 内容中的所有元素和属性。

如果你希望使用原始 XML 而非[通知库](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，请参阅 [XML 架构](toast-xml-schema.md)。

[ToastContent](#toastcontent)
* [ToastVisual](#toastvisual)
  * [ToastBindingGeneric](#toastbindinggeneric)
    * [IToastBindingGenericChild](#itoastbindinggenericchild)
    * [ToastGenericAppLogo](#toastgenericapplogo)
    * [ToastGenericHeroImage](#toastgenericheroimage)
    * [ToastGenericAttributionText](#toastgenericattributiontext)
* [IToastActions](#itoastactions)
* [ToastAudio](#toastaudio)
* [ToastHeader](#toastheader)


## <a name="toastcontent"></a>ToastContent
ToastContent 是描述通知内容的顶级对象，包括视觉、操作和音频。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **Launch**| 字符串 | 否 | 当应用程序由 toast 激活时向其传递的字符串。 此字符串的格式和内容由应用根据其自身用途定义。 当用户点击或单击 Toast 来启动其关联应用时，启动字符串会向应用提供上下文，以允许该应用向用户显示与 Toast 内容相关的视图，而不是以其默认方式启动。 |
| **Visual** | [ToastVisual](#toastvisual) | 是 | 描述 toast 通知的可视部分。 |
| **Actions** | [IToastActions](#itoastactions) | 否 | （可选）使用按钮和输入创建自定义操作。 |
| **Audio** | [ToastAudio](#toastaudio) | 否 | 描述 toast 通知的音频部分。 |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | 否 | 指定用户单击此 Toast 的正文时将使用的激活类型。 |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | 否 | 创意者更新中的新增功能：与 toast 通知有关的更多选项。 |
| **Scenario** | [ToastScenario](#toastscenario) | 否 | 声明将使用你的 toast 的场景，如闹钟或提醒。 |
| **DisplayTimestamp** | DateTimeOffset? | 否 | 创意者更新中的新增功能：使用自定义时间戳覆盖默认时间戳，该自定义时间戳表示你的通知内容的实际提交时间，而不是 Windows 平台上收到通知的时间。 |
| **Header** | [ToastHeader](#toastheader) | 否 | 创意者更新中的新增功能：将自定义标头添加到你的通知以便在操作中心内将多个通知分到一组。 |


### <a name="toastscenario"></a>ToastScenario
指定 toast 表示什么场景。

| 值 | 含义 |
|---|---|
| **Default** | 正常 toast 行为。 |
| **Reminder** | 提醒通知。 这类通知将以预先展开的状态显示在用户屏幕上并一直显示，直至消失。 |
| **Alarm** | 警报通知。 这类通知将以预先展开的状态显示在用户屏幕上并一直显示，直至消失。 音频在默认情况下将循环播放，并将使用警报音频。 |
| **IncomingCall** | 来电通知。 这将以特殊通话格式以预先展开的形式显示在用户屏幕上并一直显示，直至消失。 音频在默认情况下将循环播放，并将使用铃声音频。 |


## <a name="toastvisual"></a>ToastVisual
Toast 的可视部分包含绑定，其中包含文本、图像和自适应内容等。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **BindingGeneric** | [ToastBindingGeneric](#toastbindinggeneric) | 是 | 可在所有设备上呈现的通用 toast 绑定。 此绑定是必需的，不能为 null。 |
| **BaseUri** | Uri | 否 | 与图像源属性中的相对 URL 结合的默认基本 URL。 |
| **AddImageQuery** | bool? | 否 | 设置为“true”将使 Windows 能够将查询字符串附加到在 toast 通知中提供的图像 URL。 如果你的服务器托管图像并且可以处理查询字符串，则可通过基于查询字符串搜索图像变量，或通过忽略查询字符串并且不带查询字符串按指定方式返回图像，来使用此特性。 此查询字符串指定缩放倍数、对比度设置和语言；例如，通知中提供的“www.website.com/images/hello.png”值将变为“www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us” |
| **Language**| 字符串 | 否 | 使用本地化资源时视觉负载的目标区域设置，用 BCP-47 语言标签指定，如“en-US”或“fr-FR”。 此区域设置将用绑定或文本中指定的任何区域设置覆盖。 如果未提供，则将使用系统区域设置。 |


## <a name="toastbindinggeneric"></a>ToastBindingGeneric
通用绑定是 toast 的默认绑定，并且你就在通用绑定中指定文本、图像和自适应内容等。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **Children** | IList<[IToastBindingGenericChild](#itoastbindinggenericchild)> | 否 | Toast 正文的内容，可能包括文本、图像和组（这是在周年更新中增加的功能）。 文本元素必须位于其他任何元素之前，并且仅支持 3 个文本元素。 如果文本元素位于任何其他元素之后，则它将会被拉到顶部，或被删除。 最后，对于根子级文本元素，某些文本属性（如 HintStyle）不受支持，只能在 AdaptiveSubgroup 内部使用。 如果你在没有周年更新的设备上使用 AdaptiveGroup，则将直接删除组内容。 |
| **AppLogoOverride** | [ToastGenericAppLogo](#toastgenericapplogo) | 否 | 用于覆盖应用徽标的可选徽标。 |
| **HeroImage** | [ToastGenericHeroImage](#toastgenericheroimage) | 否 | 在 toast 上和在操作中心内显示的可选主题 Hero 图像 |
| **Attribution** | [ToastGenericAttributionText](#toastgenericattributiontext) | 否 | 将显示在 toast 通知底部的可选特性文本。 |
| **BaseUri** | Uri | 否 | 与图像源属性中的相对 URL 结合的默认基本 URL。 |
| **AddImageQuery** | bool? | 否 | 设置为“true”将使 Windows 能够将查询字符串附加到在 toast 通知中提供的图像 URL。 如果你的服务器托管图像并且可以处理查询字符串，则可通过基于查询字符串搜索图像变量，或通过忽略查询字符串并且不带查询字符串按指定方式返回图像，来使用此特性。 此查询字符串指定缩放倍数、对比度设置和语言；例如，通知中提供的“www.website.com/images/hello.png”值将变为“www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us” |
| **Language**| 字符串 | 否 | 使用本地化资源时视觉负载的目标区域设置，用 BCP-47 语言标签指定，如“en-US”或“fr-FR”。 此区域设置将用绑定或文本中指定的任何区域设置覆盖。 如果未提供，则将使用系统区域设置。 |


## <a name="itoastbindinggenericchild"></a>IToastBindingGenericChild
Toast 子元素的标记界面，包括文本、图像、组等。

| 实现 |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |
| [AdaptiveGroup](#adaptivegroup) |
| [AdaptiveProgressBar](#adaptiveprogressbar) |


## <a name="adaptivetext"></a>AdaptiveText
自适应文本元素。 如果放置在顶级 ToastBindingGeneric.Children 中，则将仅应用 HintMaxLines。 但是如果放置为组/子组的子级，则支持全文本样式。

| 属性 | 类型 | 必需 |描述 |
|---|---|---|---|
| **Text** | 字符串或 [BindableString](#bindablestring) | 否 | 要显示的文本。 在创意者更新中添加了数据绑定支持，但只适用于顶级文本元素。 |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | 否 | 样式控制文本的字体大小、粗细和不透明度。 仅适用于组/子组内部的文本元素。 |
| **HintWrap** | bool? | 否 | 将其设置为 true 可启用文本换行。 顶级文本元素将忽略此属性，并且始终换行（你可以使用 HintMaxLines = 1 禁用顶级文本元素换行）。 组/子组内部的文本元素的换行值默认为 false。 |
| **HintMaxLines** | int? | 否 | 对文本元素允许显示的最大行数。 |
| **HintMinLines** | int? | 否 | 文本元素必须显示的最小行数。 仅适用于组/子组内部的文本元素。 |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | 否 | 水平方向上文本的对齐方式。 仅适用于组/子组内部的文本元素。 |
| **Language** | 字符串 | 否 | XML 负载的目标区域设置，以 BCP-47 语言标记指定，如“en-US”或“fr-FR”。 此处指定的区域设置会覆盖其他指定的任何区域设置，如绑定或视觉中指定的设置。 如果此值为文字字符串，则此属性默认为用户的用户界面语言。 如果此值为字符串参考，则此属性默认为解析字符串时由 Windows 运行时选择的区域设置。 |


### <a name="bindablestring"></a>BindableString
字符串绑定值。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **BindingName** | 字符串 | true | 获取或设置映射到绑定数据值的名称。 |


### <a name="adaptivetextstyle"></a>AdaptiveTextStyle
文本样式控制字体大小、粗细和不透明度。 精细不透明度是 60% 不透明。

| 值 | 含义 |
|---|---|
| **Default** | 默认值。 样式由呈现器确定。 |
| **Caption** | 小于段落字体大小。 |
| **CaptionSubtle** | 与 Caption 相同，但具有精细不透明度。 |
| **正文** | 段落字体大小。 |
| **BodySubtle** | 与 Body 相同，但具有精细不透明度。 |
| **Base** | 段落字体大小、粗体。 本质上是正文的粗体版本。 |
| **BaseSubtle** | 与 Base 相同，但具有精细不透明度。 |
| **Subtitle** | H4 字体大小。 |
| **SubtitleSubtle** | 与 Subtitle 相同，但具有精细的不透明度。 |
| **Title** | H3 字体大小。 |
| **TitleSubtle** | 与 Title 相同，但具有精细不透明度。 |
| **TitleNumeral** | 与 Title 相同，但去掉了上/下边距。 |
| **副标题** | H2 字体大小。 |
| **SubheaderSubtle** | 与 Subheader 相同，但具有精细不透明度。 |
| **SubheaderNumeral** | 与 Subheader 相同，但去掉了上/下边距。 |
| **Header** | H1 字体大小。 |
| **HeaderSubtle** | 与 Header 相同，但具有精细不透明度。 |
| **HeaderNumeral** | 与 Header 相同，但去掉了上/下边距。 |


### <a name="adaptivetextalign"></a>AdaptiveTextAlign
控制文本的水平对齐。

| 值 | 含义 |
|---|---|
| **Default** | 默认值。 对齐方式由呈现器自动确定。 |
| **Auto** | 对齐方式由当前的语言和文化确定。 |
| **Left** | 在水平方向将文本向左对齐。 |
| **Center** | 在水平方向将文本中心对齐。 |
| **Right** | 在水平方向将文本向右对齐。 |


## <a name="adaptiveimage"></a>AdaptiveImage
内联图像。

| 属性 | 类型 | 必需 |描述 |
|---|---|---|---|
| **Source** | 字符串 | 是 | 图像的 URL。 支持 ms-appx、ms-appdata 和 http。 自 Fall Creators Update 起，正常连接上的 Web 图像的大小限制提升至 3 MB，按流量计费的连接上的限制提升至 1 MB。 在尚未运行 Fall Creators Update 的设备上，Web 图像的大小不得超过 200 KB。 |
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | 否 | 周年更新中的新增功能：控制图像的所需裁剪。 |
| **HintRemoveMargin** | bool? | 否 | 默认情况下，组/子组内的图像周围有 8 像素的边距。 你可以将此属性设置为 true 去掉此边距。 |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | 否 | 水平方向上图像的对齐方式。 仅适用于组/子组内的图像。 |
| **AlternateText** | 字符串 | 否 | 描述图像的替换文本，用于辅助功能。 |
| **AddImageQuery** | bool? | 否 | 设置为“true”将使 Windows 能够将查询字符串附加到在 toast 通知中提供的图像 URL。 如果你的服务器托管图像并且可以处理查询字符串，则可通过基于查询字符串搜索图像变量，或通过忽略查询字符串并且不带查询字符串按指定方式返回图像，来使用此特性。 此查询字符串指定缩放倍数、对比度设置和语言；例如，通知中提供的“www.website.com/images/hello.png”值将变为“www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us” |


### <a name="adaptiveimagecrop"></a>AdaptiveImageCrop
指定所需的图像裁剪。

| 值 | 含义 |
|---|---|
| **Default** | 默认值。 由呈现器确定的裁剪行为。 |
| **None** | 不裁剪图像。 |
| **Circle** | 将图像裁剪为圆形。 |


### <a name="adaptiveimagealign"></a>AdaptiveImageAlign
指定图像在水平方向上的对齐方式。

| 值 | 含义 |
|---|---|
| **Default** | 默认值。 由渲染器确定的对齐行为。 |
| **Stretch** | 图像拉伸以填充可用宽度（根据图像的位置，可能还要填充可用高度）。 |
| **Left** | 向左对齐图像，并用其原始分辨率显示图像。 |
| **Center** | 在水平方向上中心对齐图像，并用其原始分辨率显示图像。 |
| **Right** | 向右对齐图像，并用其原始分辨率显示图像。 |


## <a name="adaptivegroup"></a>AdaptiveGroup
周年更新中的新增功能：组从语义上确定组中内容是必须作为整体显示，还是无法容纳就不显示。 组还允许创建多个列。

| 属性 | 类型 | 必需 |描述 |
|---|---|---|---|
| **Children** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | 否 | 子组显示为垂直列。 你必须使用子组来提供 AdaptiveGroup 内的任何内容。 |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
周年更新中的新增功能：子组是可能包含文本和图像的垂直列。

| 属性 | 类型 | 必需 |描述 |
|---|---|---|---|
| **Children** | IList<[IAdaptiveSubgroupChild](#iadaptivesubgroupchild)> | 否 | [AdaptiveText](#adaptivetext) 和 [AdaptiveImage](#adaptiveimage) 是子组的有效子级。 |
| **HintWeight** | int? | 否 | 通过指定粗细控制此子组列相对于其他子组的宽度。 |
| **HintTextStacking** | [AdaptiveSubgroupTextStacking](#adaptivesubgrouptextstacking) | 否 | 控制此子组内容在垂直方向上的对齐方式。 |


### <a name="iadaptivesubgroupchild"></a>IAdaptiveSubgroupChild
子组子级的标记界面。

| Implementations |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |


### <a name="adaptivesubgrouptextstacking"></a>AdaptiveSubgroupTextStacking
TextStacking 指定内容在垂直方向上的对齐方式。

| 值 | 含义 |
|---|---|
| **Default** | 默认值。 渲染器将自动选择默认的垂直对齐方式。 |
| **Top** | 垂直方向上向顶部对齐。 |
| **Center** | 垂直方向上向中心对齐。 |
| **Bottom** | 垂直方向上向底部对齐。 |


## <a name="adaptiveprogressbar"></a>AdaptiveProgressBar
创意者更新中的新增功能：进度栏。 仅支持桌面 Toast，版本 15063 或更高版本。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **Title** | 字符串或 [BindableString](#bindablestring) | false | 获取或设置可选标题字符串。 支持数据绑定。 |
| **Value** | Double 或 [AdaptiveProgressBarValue](#adaptiveprogressbarvalue) 或 [BindableProgressBarValue](#bindableprogressbarvalue) | false | 获取或设置进度栏的值。 支持数据绑定。 默认为 0。 |
| **ValueStringOverride** | 字符串或 [BindableString](#bindablestring) | false | 获取或设置要显示的可选字符串，而不是默认百分比字符串。 如果未提供，会显示诸如“70%”的内容。 |
| **Status** | 字符串或 [BindableString](#bindablestring) | true | 获取或设置状态字符串（必需），它显示在左侧进度栏下方。 此字符串应反映操作的状态，如“正在下载...”或“正在安装...” |


### <a name="adaptiveprogressbarvalue"></a>AdaptiveProgressBarValue
代表进度栏值的类。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **Value** | Double | false | 获取或设置表示完成百分比得值 (0.0 - 1.0)。 |
| **IsIndeterminate** | Bool | false | 获取或设置值，用于指示进度栏是否为不确定。 如果为 true，将忽略 **Value**。 |


### <a name="bindableprogressbarvalue"></a>BindableProgressBarValue
可绑定的进度栏值。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **BindingName** | 字符串 | true | 获取或设置映射到绑定数据值的名称。 |


## <a name="toastgenericapplogo"></a>ToastGenericAppLogo
要显示的徽标（而不是应用徽标）。

| 属性 | 类型 | 必需 |描述 |
|---|---|---|---|
| **Source** | 字符串 | 是 | 图像的 URL。 支持 ms-appx、ms-appdata 和 http。 Http 图像大小必须为 200 KB 或更小。 |
| **HintCrop** | [ToastGenericAppLogoCrop](#toastgenericapplogocrop) | 否 | 指定你希望如何裁剪图像。 |
| **AlternateText** | 字符串 | 否 | 描述图像的替换文本，用于辅助功能。 |
| **AddImageQuery** | bool? | 否 | 设置为“true”将使 Windows 能够将查询字符串附加到在 toast 通知中提供的图像 URL。 如果你的服务器托管图像并且可以处理查询字符串，则可通过基于查询字符串搜索图像变量，或通过忽略查询字符串并且不带查询字符串按指定方式返回图像，来使用此特性。 此查询字符串指定缩放倍数、对比度设置和语言；例如，通知中提供的“www.website.com/images/hello.png”值将变为“www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us” |


### <a name="toastgenericapplogocrop"></a>ToastGenericAppLogoCrop
控制应用徽标图像裁剪。

| 值 | 含义 |
|---|---|
| **Default** | 裁剪使用渲染器的默认行为。 |
| **None** | 图像不裁剪，显示为方形。 |
| **Circle** | 将图像裁剪为圆形。 |


## <a name="toastgenericheroimage"></a>ToastGenericHeroImage
在 toast 上和在操作中心内显示的主题 Hero 图像

| 属性 | 类型 | 必需 |描述 |
|---|---|---|---|
| **Source** | 字符串 | 是 | 图像的 URL。 支持 ms-appx、ms-appdata 和 http。 Http 图像大小必须为 200 KB 或更小。 |
| **AlternateText** | 字符串 | 否 | 描述图像的替换文本，用于辅助功能。 |
| **AddImageQuery** | bool? | 否 | 设置为“true”将使 Windows 能够将查询字符串附加到在 toast 通知中提供的图像 URL。 如果你的服务器托管图像并且可以处理查询字符串，则可通过基于查询字符串搜索图像变量，或通过忽略查询字符串并且不带查询字符串按指定方式返回图像，来使用此特性。 此查询字符串指定缩放倍数、对比度设置和语言；例如，通知中提供的“www.website.com/images/hello.png”值将变为“www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us” |


## <a name="toastgenericattributiontext"></a>ToastGenericAttributionText
显示在 toast 通知底部的属性文本。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **Text** | 字符串 | 是 | 要显示的文本。 |
| **Language** | 字符串 | 否 | 使用本地化资源时视觉负载的目标区域设置，用 BCP-47 语言标签指定，如“en-US”或“fr-FR”。 如果未提供，则将使用系统区域设置。 |


## <a name="itoastactions"></a>IToastActions
toast 操作/输入的标记界面。

| Implementations |
| --- |
| [ToastActionsCustom](#toastactionscustom) |
| [ToastActionsSnoozeAndDismiss](#toastactionssnoozeanddismiss) |


## <a name="toastactionscustom"></a>ToastActionsCustom
*实现 [IToastActions](#itoastactions)*

使用按钮、文本框和选择输入等控件创建你自己的自定义操作和输入。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **Inputs** | IList<[IToastInput](#itoastinput)> | 否 | 如文本框和选择输入等输入。 最多仅允许 5 个输入。 |
| **Buttons** | IList<[IToastButton](#itoastbutton)> | 否 | 显示在所有输入之后（或者，如果按钮用作快速回复按钮，则在输入旁边）的按钮。 最多仅允许 5 个按钮（或者，如果你还有上下文菜单项，则更少）。 |
| **ContextMenuItems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | 否 | 周年更新中的新增功能：自定义上下文菜单项，如果用户右键单击通知，则会提供更多操作。 最多只能有 5 个按钮和上下文菜单项*组合*。 |


## <a name="itoastinput"></a>IToastInput
toast 输入的标记界面。

| 实现 |
| --- |
| [ToastTextBox](#toasttextbox) |
| [ToastSelectionBox](#toastselectionbox) |


## <a name="toasttextbox"></a>ToastTextBox
*实现 [IToastInput](#itoastinput)*

用户可以在其中键入文本的文本框控件。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **Id** | 字符串 | 是 | Id 是必需的，并用于将用户输入的文本映射到你的应用以后使用的 id/值的键值对。 |
| **Title** | 字符串 | 否 | 显示在文本框上方的标题文本。 |
| **PlaceholderContent** | 字符串 | 否 | 用户尚未输入任何文本时显示在文本框中的占位符文本。 |
| **DefaultInput** | 字符串 | 否 | 要放置在文本框中的初始文本。 对于空白文本框将此保留为 null。 |


## <a name="toastselectionbox"></a>ToastSelectionBox
*实现 [IToastInput](#itoastinput)*

选择框控件，让用户能够从选项的下拉列表中进行选择。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **Id** | 字符串 | 是 | 此 Id 是必需的。 如果用户选择此项，则此 Id 将传递回应用的代码，表示他们所做的选择。 |
| **Content** | 字符串 | 是 | Content 是必需的，是显示在选择项上的字符串。 |


### <a name="toastselectionboxitem"></a>ToastSelectionBoxItem
选择框项（用户可从下拉列表选择的项）。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **Id** | 字符串 | 是 | Id 是必需的，并用于将用户输入的文本映射到你的应用以后使用的 id/值的键值对。 |
| **Title** | 字符串 | 否 | 显示在选择框上方的标题文本。 |
| **DefaultSelectionBoxItemId** | 字符串 | 否 | 它控制默认情况下选择哪一项，并引用 [ToastSelectionBoxItem](#toastselectionboxitem) 的 Id 属性。 如果不提供此属性值，则默认选项将为空（用户看不到任何内容）。 |
| **Items** | IList<[ToastSelectionBoxItem](#toastselectionboxitem)> | 否 | 用户可从此 SelectionBox 中进行选择的选择项。 仅可添加 5 个项。 |


## <a name="itoastbutton"></a>IToastButton
toast 按钮的标记界面。

| Implementations |
| --- |
| [ToastButton](#toastbutton) |
| [ToastButtonSnooze](#toastbuttonsnooze) |
| [ToastButtonDismiss](#toastbuttondismiss) |


## <a name="toastbutton"></a>ToastButton
*实现 [IToastButton](#itoastbutton)*

用户可单击的按钮。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **Content** | 字符串 | 是 | 必需。 要显示在按钮上的文本。 |
| **Arguments** | 字符串 | 是 | 必需。 如果用户单击此按钮，应用将在稍后接收到的应用定义的参数字符串。 |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | 否 | 控制当用户单击此按钮时，此按钮将使用何种类型的激活。 默认为前台激活。 |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | 创意者更新中的新增功能：获取或设置与激活 Toast 按钮相关的更多选项。 |


### <a name="toastactivationtype"></a>ToastActivationType
决定用户与特定操作交互时将使用的激活的类型。

| 值 | 含义 |
|---|---|
| **Foreground** | 默认值。 启动前台应用。 |
| **Background** | 将触发你相应的后台任务（假设你已完成一切设置），并且你可以在后台执行代码（如发送用户的快速回复消息）而不会打扰用户。 |
| **Protocol** | 使用协议激活启动其他应用。 |


### <a name="toastactivationoptions"></a>ToastActivationOptions
创意者更新中的新增功能：与激活相关的其他选项。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **AfterActivationBehavior** | [ToastAfterActivationBehavior](#toastafteractivationbehavior) | false | 秋季创意者更新中的新增功能：获取或设置在用户调用某操作时 Toast 应使用的行为。 仅适用于桌面上的 [ToastButton](#toastbutton) 和 [ToastContextMenuItem](#toastcontextmenuitem)。 |
| **ProtocolActivationTargetApplicationPfn** | 字符串 | 否 | 如果你使用*ToastActivationType.Protocol*，则可以选择指定目标 PFN，这样，无论是否有多个应用注册以处理同一协议 uri，始终都会启动你所需的应用。 |


### <a name="toastafteractivationbehavior"></a>ToastAfterActivationBehavior
指定当用户在 Toast 上执行操作时 Toast 应使用的行为。

| Value | 含义 |
|---|---|
| **Default** | 默认行为。 当用户在 Toast 上执行操作时将消除 Toast。 |
| **PendingUpdate** | 用户单击 Toast 上的按钮后，通知仍存在，且处于“挂起的更新”视觉状态。 应立即从后台任务更新 Toast，以便用户不会长时间看到此“挂起的更新”视觉状态。 |


## <a name="toastbuttonsnooze"></a>ToastButtonSnooze
*实现 [IToastButton](#itoastbutton)*

自动处理通知推迟的一个系统处理推迟按钮。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **CustomContent** | 字符串 | 否 | 在按钮上显示的可选自定义文本，会覆盖默认的本地化的“推迟”文本。 |


## <a name="toastbuttondismiss"></a>ToastButtonDismiss
*实现 [IToastButton](#itoastbutton)*

一个系统处理的消除按钮，单击时将消除通知。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **CustomContent** | 字符串 | 否 | 在按钮上显示的可选自定义文本，会覆盖默认的本地化的“消除”文本。 |


## <a name="toastactionssnoozeanddismiss"></a>ToastActionsSnoozeAndDismiss
*实现 [IToastActions](#itoastactions)

自动为暂停时间间隔构造选择框、推迟/消除按钮，这些都全部自动执行本地化，且推迟逻辑由系统自动处理。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **ContextMenuItems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | 否 | 周年更新中的新增功能：自定义上下文菜单项，如果用户右键单击通知，则会提供更多操作。 你最多只能有 5 个项。 |


## <a name="toastcontextmenuitem"></a>ToastContextMenuItem
上下文菜单项条目。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **Content** | 字符串 | 是 | 必需。 要显示的文本。 |
| **Arguments** | 字符串 | 是 | 必需。 用户单击菜单项后，应用一旦激活便可以在稍后检索的应用定义的参数字符串。 |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | 否 | 控制当用户单击此按钮时，此菜单项将使用何种类型的激活。 默认为前台激活。 |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | 否 | 创意者更新中的新增功能：与激活 toast 上下文菜单项相关的其他选项。 |


## <a name="toastaudio"></a>ToastAudio
指定收到 Toast 通知时要播放的音频。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **Src** | uri | 否 | 播放来代替默认声音的媒体文件。 仅支持 ms-appx 和 ms-appdata。 |
| **Loop** | 布尔 | 否 | 如果声音需在 Toast 显示时不断重复，则设置为 true；如果仅播放一次，则设置为 false（默认设置）。 |
| **Silent** | 布尔 | 否 | 设置为 true 将声音静音；设置为 false 允许播放 toast 声音（默认设置）。 |


## <a name="toastheader"></a>ToastHeader
创意者更新中的新增功能：在操作中心内将多个通知归组在一起的自定义标头。

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **Id** | 字符串 | 是 | 开发人员创建的标识符，用以唯一标识此标头。 如果两个通知具有相同的标头 id，它们将显示在操作中心中的同一标头下。 |
| **Title** | 字符串 | 是 | 标头的标题。 |
| **Arguments**| 字符串 | true | 获取或设置当用户单击此标头时返回给应用的、开发人员定义的参数字符串。 不能为 null。 |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | 获取或设置当用户单击时此标头将使用的激活类型。 默认为 Foreground。 请注意，仅支持 Foreground 和 Protocol。 |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | 获取或设置与 Toast 标头有关的更多选项。 |


## <a name="related-topics"></a>相关主题

* [快速入门：发送本地 toast 和句柄激活](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/07/08/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10.aspx)
* [GitHub 上的通知库](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)