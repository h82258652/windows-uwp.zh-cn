---
Description: 下文介绍了磁贴内容中的所有元素和属性。
title: 磁贴内容架构
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Tile content schema
template: detail.hbs
ms.date: 07/28/2017
ms.topic: article
keywords: windows 10, uwp, 磁贴, 磁贴通知, 磁贴内容, 架构, 磁贴负载
ms.localizationpriority: medium
ms.openlocfilehash: 26dcfc992dfc7557b1a9d835111346588cd2431a
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320768"
---
# <a name="tile-content-schema"></a>磁贴内容架构

 

以下内容介绍了磁贴内容中的所有属性和元素。

如果希望使用原始 XML 而非[通知库](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，请参阅 [XML 架构](../tiles-and-notifications/adaptive-tiles-schema.md)。

[TileContent](#tilecontent)
* [TileVisual](#tilevisual)
  * [TileBinding](#tilebinding)
    * [TileBindingContentAdaptive](#TileBindingContentAdaptive)
    * [TileBindingContentIconic](#TileBindingContentIconic)
    * [TileBindingContentContact](#TileBindingContentContact)
    * [TileBindingContentPeople](#TileBindingContentPeople)
    * [TileBindingContentPhotos](#TileBindingContentPhotos)


## <a name="tilecontent"></a>TileContent
TileContent 是描述磁贴通知内容（包括视觉）的顶级对象。

| 属性 | 在任务栏的搜索框中键入 | 必需 | 描述 |
|---|---|---|---|
| **视觉对象** | [ToastVisual](#tilevisual) | true | 描述磁贴通知的视觉部分。 |


## <a name="tilevisual"></a>TileVisual
磁贴的视觉部分包含所有磁贴大小的视觉规格，以及更多视觉相关的属性。

| 属性 | 在任务栏的搜索框中键入 | 必需 | 描述 |
|---|---|---|---|
| **TileSmall** | [TileBinding](#tilebinding) | false | 提供可选的小绑定，以指定小磁贴大小的内容。 |
| **TileMedium** | [TileBinding](#tilebinding) | false | 提供可选的中等绑定，以指定中等磁贴大小的内容。 |
| **TileWide** | [TileBinding](#tilebinding) | false | 提供可选的宽绑定，以指定宽磁贴大小的内容。 |
| **TileLarge** | [TileBinding](#tilebinding) | false | 提供可选的大绑定，以指定大磁贴大小的内容。 |
| **品牌** | [TileBranding](#tilebranding) | false | 磁贴应该用来显示应用品牌的形式。 默认情况下，将从默认磁贴继承品牌。 |
| **DisplayName** | string | false | 在显示此通知时用来覆盖磁贴显示名称的可选字符串。 |
| **自变量** | string | false | 周年更新： 中的新增功能被传递回您的应用程序通过 TileActivatedInfo 属性 LaunchActivatedEventArgs 用户启动你的应用动态磁贴时的应用程序定义的数据。 可以通过这些数据了解用户在点击动态磁贴时看到了哪些磁贴通知。 在没有周年更新的设备上，将忽略此功能。 |
| **LockDetailedStatus1** | string | false | 如果指定此属性，还必须提供 TileWide 绑定。 如果用户已选择磁贴作为其详细的状态应用，这是将显示在锁屏界面上的第一行文本。 |
| **LockDetailedStatus2** | string | false | 如果指定此属性，还必须提供 TileWide 绑定。 如果用户已选择磁贴作为其详细的状态应用，这是将显示在锁屏界面上的第二行文本。 |
| **LockDetailedStatus3** | string | false | 如果指定此属性，还必须提供 TileWide 绑定。 如果用户已选择磁贴作为其详细的状态应用，这是将显示在锁屏界面上的第三行文本。 |
| **BaseUri** | Uri | false | 与图像源属性中的相对 URL 结合的默认基本 URL。 |
| **AddImageQuery** | bool? | false | 设置为“true”将使 Windows 能够将查询字符串附加到在 toast 通知中提供的图像 URL。 如果你的服务器托管图像并且可以处理查询字符串，则可通过基于查询字符串搜索图像变量，或通过忽略查询字符串并且不带查询字符串按指定方式返回图像，来使用此特性。 此查询字符串指定缩放倍数、对比度设置和语言；例如，通知中提供的“www.website.com/images/hello.png”值将变为“www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us” |
| **语言**| string | false | 使用本地化资源时视觉负载的目标区域设置，用 BCP-47 语言标签指定，如“en-US”或“fr-FR”。 此区域设置将用绑定或文本中指定的任何区域设置覆盖。 如果未提供，则将使用系统区域设置。 |


## <a name="tilebinding"></a>TileBinding
绑定对象包含特定磁贴大小的视觉内容。

| 属性 | 在任务栏的搜索框中键入 | 必需 | 描述 |
|---|---|---|---|
| **Content** | [ITileBindingContent](#itilebindingcontent) | false | 要在磁贴上显示的视觉内容。 [TileBindingContentAdaptive](#tilebindingcontentadaptive)、[TileBindingContentIconic](#TileBindingContentIconic)、[TileBindingContentContact](#TileBindingContentContact)、[TileBindingContentPeople](#TileBindingContentPeople) 或 [TileBindingContentPhotos](#TileBindingContentPhotos) 中的一个。 |
| **品牌** | TileBranding | false | 磁贴应该用来显示应用品牌的形式。 默认情况下，将从默认磁贴继承品牌。 |
| **DisplayName** | string | false | 用来覆盖此磁贴大小的磁贴显示名称的可选字符串。 |
| **自变量** | string | false | 周年更新： 中的新增功能被传递回您的应用程序通过 TileActivatedInfo 属性 LaunchActivatedEventArgs 用户启动你的应用动态磁贴时的应用程序定义的数据。 可以通过这些数据了解用户在点击动态磁贴时看到了哪些磁贴通知。 在没有周年更新的设备上，将忽略此功能。 |
| **BaseUri** | Uri | false | 与图像源属性中的相对 URL 结合的默认基本 URL。 |
| **AddImageQuery** | bool? | false | 设置为“true”将使 Windows 能够将查询字符串附加到在 toast 通知中提供的图像 URL。 如果你的服务器托管图像并且可以处理查询字符串，则可通过基于查询字符串搜索图像变量，或通过忽略查询字符串并且不带查询字符串按指定方式返回图像，来使用此特性。 此查询字符串指定缩放倍数、对比度设置和语言；例如，通知中提供的“www.website.com/images/hello.png”值将变为“www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us” |
| **语言**| string | false | 使用本地化资源时视觉负载的目标区域设置，用 BCP-47 语言标签指定，如“en-US”或“fr-FR”。 此区域设置将用绑定或文本中指定的任何区域设置覆盖。 如果未提供，则将使用系统区域设置。 |


## <a name="itilebindingcontent"></a>ITileBindingContent
磁贴绑定内容的标记接口。 这些接口用来选择是将磁贴视觉内容置于自适应磁贴中还是某种特殊模板中。

| 实现 |
| --- |
| [TileBindingContentAdaptive](#TileBindingContentAdaptive) |
| [TileBindingContentIconic](#TileBindingContentIconic) |
| [TileBindingContentContact](#TileBindingContentContact) |
| [TileBindingContentPeople](#TileBindingContentPeople) |
| [TileBindingContentPhotos](#TileBindingContentPhotos) |


## <a name="tilebindingcontentadaptive"></a>TileBindingContentAdaptive
所有大小都支持。 这是推荐用于指定磁贴内容的方式。 Windows 10 中新增了自适应磁贴模板，你可以创建各种自适应自定义磁贴。

| 属性 | 在任务栏的搜索框中键入 | 必需 | 描述 |
|---|---|---|---|
| **Children** | IList<ITileBindingContentAdaptiveChild> | false | 内联视觉元素。 可添加 [AdaptiveText](#adaptivetext)、[AdaptiveImage](#adaptiveimage) 和 [AdaptiveGroup](#adaptivegroup) 对象。 子级以垂直 StackPanel 方式显示。 |
| **BackgroundImage** | [TileBackgroundImage](#tilebackgroundimage) | false | 一个可选的背景图像，显示在所有磁贴内容之后，全出血。 |
| **PeekImage** | [TilePeekImage](#tilepeekimage) | false | 从磁贴顶部以动画形式进入的可选速览图像。 |
| **TextStacking** | [TileTextStacking](#tiletextstacking) | false | 整体性控制子级内容文本堆叠（垂直对齐）。 |


## <a name="adaptivetext"></a>AdaptiveText
自适应文本元素。

| 属性 | 在任务栏的搜索框中键入 | 必需 |描述 |
|---|---|---|---|
| **文本** | string | false | 要显示的文本。 |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | false | 样式控制文本的字体大小、粗细和不透明度。 |
| **HintWrap** | bool? | false | 将其设置为 true 可启用文本换行。 默认为“false”。 |
| **HintMaxLines** | int? | false | 对文本元素允许显示的最大行数。 |
| **HintMinLines** | int? | false | 文本元素必须显示的最小行数。 |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | false | 水平方向上文本的对齐方式。 |
| **语言** | string | false | XML 负载的目标区域设置，以 BCP-47 语言标记指定，如“en-US”或“fr-FR”。 此处指定的区域设置会覆盖其他指定的任何区域设置，如绑定或视觉中指定的设置。 如果此值为文字字符串，则此属性默认为用户的用户界面语言。 如果此值为字符串参考，则此属性默认为解析字符串时由 Windows 运行时选择的区域设置。 |


### <a name="adaptivetextstyle"></a>AdaptiveTextStyle
文本样式控制字体大小、粗细和不透明度。 精细不透明度是 60% 不透明。

| 值 | 含义 |
|---|---|
| **默认** | 默认值。 样式由呈现器确定。 |
| **标题** | 小于段落字体大小。 |
| **CaptionSubtle** | 与 Caption 相同，但具有精细不透明度。 |
| **Body** | 段落字体大小。 |
| **BodySubtle** | 与 Body 相同，但具有精细不透明度。 |
| **基本** | 段落字体大小、粗体。 本质上是正文的粗体版本。 |
| **BaseSubtle** | 与 Base 相同，但具有精细不透明度。 |
| **Subtitle** | H4 字体大小。 |
| **SubtitleSubtle** | 与 Subtitle 相同，但具有精细的不透明度。 |
| **Title** | H3 字体大小。 |
| **TitleSubtle** | 与 Title 相同，但具有精细不透明度。 |
| **TitleNumeral** | 与 Title 相同，但去掉了上/下边距。 |
| **Subheader** | H2 字体大小。 |
| **SubheaderSubtle** | 与 Subheader 相同，但具有精细不透明度。 |
| **SubheaderNumeral** | 与 Subheader 相同，但去掉了上/下边距。 |
| **标头** | H1 字体大小。 |
| **HeaderSubtle** | 与 Header 相同，但具有精细不透明度。 |
| **HeaderNumeral** | 与 Header 相同，但去掉了上/下边距。 |


### <a name="adaptivetextalign"></a>AdaptiveTextAlign
控制文本的水平对齐。

| ReplTest1 | 含义 |
|---|---|
| **默认** | 默认值。 对齐方式由呈现器自动确定。 |
| **自动** | 对齐方式由当前的语言和文化确定。 |
| **Left** | 在水平方向将文本向左对齐。 |
| **Center** | 在水平方向将文本中心对齐。 |
| **Right** | 在水平方向将文本向右对齐。 |


## <a name="adaptiveimage"></a>AdaptiveImage
内联图像。

| 属性 | 在任务栏的搜索框中键入 | 必需 |描述 |
|---|---|---|---|
| **源** | string | true | 图像的 URL。 支持 ms-appx、ms-appdata 和 http。 自 Fall Creators Update 起，正常连接上的 Web 图像的大小限制提升至 3 MB，按流量计费的连接上的限制提升至 1 MB。 在尚未运行 Fall Creators Update 的设备上，Web 图像的大小不得超过 200 KB。 |
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | false | 控制所需的图像裁剪。 |
| **HintRemoveMargin** | bool? | false | 默认情况下，组/子组内的图像周围有 8 像素的边距。 你可以将此属性设置为 true 去掉此边距。 |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | false | 水平方向上图像的对齐方式。 |
| **AlternateText** | string | false | 描述图像的替换文本，用于辅助功能。 |
| **AddImageQuery** | bool? | false | 设置为“true”将使 Windows 能够将查询字符串附加到在磁贴通知中提供的图像 URL。 如果你的服务器托管图像并且可以处理查询字符串，则可通过基于查询字符串搜索图像变量，或通过忽略查询字符串并且不带查询字符串按指定方式返回图像，来使用此特性。 此查询字符串指定缩放倍数、对比度设置和语言；例如，通知中提供的“www.website.com/images/hello.png”值将变为“www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us” |


### <a name="adaptiveimagecrop"></a>AdaptiveImageCrop
指定所需的图像裁剪。

| ReplTest1 | 含义 |
|---|---|
| **默认** | 默认值。 由呈现器确定的裁剪行为。 |
| **无** | 不裁剪图像。 |
| **Circle** | 将图像裁剪为圆形。 |


### <a name="adaptiveimagealign"></a>AdaptiveImageAlign
指定图像在水平方向上的对齐方式。

| 值 | 含义 |
|---|---|
| **默认** | 默认值。 由渲染器确定的对齐行为。 |
| **Stretch** | 图像拉伸以填充可用宽度（根据图像的位置，可能还要填充可用高度）。 |
| **Left** | 向左对齐图像，并用其原始分辨率显示图像。 |
| **Center** | 在水平方向上中心对齐图像，并用其原始分辨率显示图像。 |
| **Right** | 向右对齐图像，并用其原始分辨率显示图像。 |


## <a name="adaptivegroup"></a>AdaptiveGroup
组从语义上确定组中内容是必须作为整体显示，还是无法容纳就不显示。 组还允许创建多个列。

| 属性 | 在任务栏的搜索框中键入 | 必需 |描述 |
|---|---|---|---|
| **Children** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | false | 子组显示为垂直列。 你必须使用子组来提供 AdaptiveGroup 内的任何内容。 |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
子组是可能包含文本和图像的垂直列。

| 属性 | 在任务栏的搜索框中键入 | 必需 |描述 |
|---|---|---|---|
| **Children** | IList<[IAdaptiveSubgroupChild](#iadaptivesubgroupchild)> | false | [AdaptiveText](#adaptivetext) 和 [AdaptiveImage](#adaptiveimage) 是子组的有效子级。 |
| **HintWeight** | int? | false | 通过指定粗细控制此子组列相对于其他子组的宽度。 |
| **HintTextStacking** | [AdaptiveSubgroupTextStacking](#adaptivesubgrouptextstacking) | false | 控制此子组内容在垂直方向上的对齐方式。 |


### <a name="iadaptivesubgroupchild"></a>IAdaptiveSubgroupChild
子组子级的标记界面。

| 实现 |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |


### <a name="adaptivesubgrouptextstacking"></a>AdaptiveSubgroupTextStacking
TextStacking 指定内容在垂直方向上的对齐方式。

| ReplTest1 | 含义 |
|---|---|
| **默认** | 默认值。 渲染器将自动选择默认的垂直对齐方式。 |
| **Top** | 垂直方向上向顶部对齐。 |
| **Center** | 垂直方向上向中心对齐。 |
| **底部** | 垂直方向上向底部对齐。 |


## <a name="tilebackgroundimage"></a>TileBackgroundImage
背景图像在磁贴上全出血显示。

| 属性 | 在任务栏的搜索框中键入 | 必需 |描述 |
|---|---|---|---|
| **源** | string | true | 图像的 URL。 ms-appx、ms-appdata 和 http(s) 都受支持。 Http 图像大小必须为 200 KB 或更小。 |
| **HintOverlay** | int? | false | 在背景图像上的黑色覆盖层 此值控制黑色覆盖层的不透明度，为 0 时无覆盖层，为 100 时为全黑。 默认为“20”。 |
| **HintCrop** | [TileBackgroundImageCrop](#tilebackgroundimagecrop) | false | 1511： 中的新增功能指定你希望如何裁剪图像。 在 1511 之前的版本中，忽略此功能，背景图像将完全显示，无裁剪。 |
| **AlternateText** | string | false | 描述图像的替换文本，用于辅助功能。 |
| **AddImageQuery** | bool? | false | 设置为“true”将使 Windows 能够将查询字符串附加到在磁贴通知中提供的图像 URL。 如果你的服务器托管图像并且可以处理查询字符串，则可通过基于查询字符串搜索图像变量，或通过忽略查询字符串并且不带查询字符串按指定方式返回图像，来使用此特性。 此查询字符串指定缩放倍数、对比度设置和语言；例如，通知中提供的“www.website.com/images/hello.png”值将变为“www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us” |


### <a name="tilebackgroundimagecrop"></a>TileBackgroundImageCrop
控制背景图像裁剪。

| ReplTest1 | 含义 |
|---|---|
| **默认** | 裁剪使用渲染器的默认行为。 |
| **无** | 图像不裁剪，显示为方形。 |
| **Circle** | 将图像裁剪为圆形。 |


## <a name="tilepeekimage"></a>TilePeekImage
从磁贴顶部以动画形式进入的速览图像。

| 属性 | 在任务栏的搜索框中键入 | 必需 |描述 |
|---|---|---|---|
| **源** | string | true | 图像的 URL。 ms-appx、ms-appdata 和 http(s) 都受支持。 Http 图像大小必须为 200 KB 或更小。 |
| **HintOverlay** | int? | false | 1511： 中的新增功能查看映像上黑色覆盖。 此值控制黑色覆盖层的不透明度，为 0 时无覆盖层，为 100 时为全黑。 默认为“20”。 在以前的版本中，将忽略此值，速览图像以 0 覆盖层显示。 |
| **HintCrop** | [TilePeekImageCrop](#tilepeekimagecrop) | false | 1511： 中的新增功能指定你希望如何裁剪图像。 在 1511 之前的版本中，忽略此功能，速览图像将完全显示，无裁剪。 |
| **AlternateText** | string | false | 描述图像的替换文本，用于辅助功能。 |
| **AddImageQuery** | bool? | false | 设置为“true”将使 Windows 能够将查询字符串附加到在磁贴通知中提供的图像 URL。 如果你的服务器托管图像并且可以处理查询字符串，则可通过基于查询字符串搜索图像变量，或通过忽略查询字符串并且不带查询字符串按指定方式返回图像，来使用此特性。 此查询字符串指定缩放倍数、对比度设置和语言；例如，通知中提供的“www.website.com/images/hello.png”值将变为“www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us” |


### <a name="tilepeekimagecrop"></a>TilePeekImageCrop
控制速览图像裁剪。

| ReplTest1 | 含义 |
|---|---|
| **默认** | 裁剪使用渲染器的默认行为。 |
| **无** | 图像不裁剪，显示为方形。 |
| **Circle** | 将图像裁剪为圆形。 |


### <a name="tiletextstacking"></a>TileTextStacking
Textstacking 指定内容在垂直方向上的对齐方式。

| 值 | 含义 |
|---|---|
| **默认** | 默认值。 渲染器将自动选择默认的垂直对齐方式。 |
| **Top** | 垂直方向上向顶部对齐。 |
| **Center** | 垂直方向上向中心对齐。 |
| **底部** | 垂直方向上向底部对齐。 |


## <a name="tilebindingcontenticonic"></a>TileBindingContentIconic
在小磁贴和中等磁贴上受支持。 启用图标磁贴模板，可以在其中将一个图标和徽章以经典 Windows Phone 样式相邻显示。 图标旁边的编号通过单独的徽章通知获得。

| 属性 | 在任务栏的搜索框中键入 | 必需 |描述 |
|---|---|---|---|
| **Icon** | [TileBasicImage](#tilebasicimage) | true | 至少，要支持桌面和移动设备，小磁贴和中等磁贴，请提供分辨率 200x200、PNG 格式、透明且无白色之外其他颜色的方纵横比图像。 有关详细信息，请参阅：[特殊的磁贴模板](../tiles-and-notifications/special-tile-templates-catalog.md)。 |


## <a name="tilebindingcontentcontact"></a>TileBindingContentContact
仅限移动设备。 在小磁贴、中等磁贴和宽磁贴上受支持。

| 属性 | 在任务栏的搜索框中键入 | 必需 |描述 |
|---|---|---|---|
| **Image** | [TileBasicImage](#tilebasicimage) | true | 要显示的图像。 |
| **文本** | [TileBasicText](#tilebasictext) | false | 显示的文本行。 在小磁贴上不显示。 |


## <a name="tilebindingcontentpeople"></a>TileBindingContentPeople
1511： 中的新增功能支持中、 宽，和大 （桌面和移动版）。 以前，这仅限移动设备，并且仅限中等和宽磁贴。

| 属性 | 在任务栏的搜索框中键入 | 必需 |描述 |
|---|---|---|---|
| **图像** | IList<[TileBasicImage](#tilebasicimage)> | true | 将以圆圈形式滚动的图像。 |


## <a name="tilebindingcontentphotos"></a>TileBindingContentPhotos
通过以播放幻灯片的形式显示照片的动画。 所有大小都支持。

| 属性 | 在任务栏的搜索框中键入 | 必需 |描述 |
|---|---|---|---|
| **图像** | IList<[TileBasicImage](#tilebasicimage)> | true | 最多可提供 12 个图像（移动设备最多显示 9 个），这些图像将用于幻灯片放映。 如果添加的图像超过 12 个，将引发异常。 |


### <a name="tilebasicimage"></a>TileBasicImage
在不同特殊模板上使用的图像。

| 属性 | 在任务栏的搜索框中键入 | 必需 |描述 |
|---|---|---|---|
| **源** | string | true | 图像的 URL。 ms-appx、ms-appdata 和 http(s) 都受支持。 Http 图像大小必须为 200 KB 或更小。 |
| **AlternateText** | string | false | 描述图像的替换文本，用于辅助功能。 |
| **AddImageQuery** | bool? | false | 设置为“true”将使 Windows 能够将查询字符串附加到在磁贴通知中提供的图像 URL。 如果你的服务器托管图像并且可以处理查询字符串，则可通过基于查询字符串搜索图像变量，或通过忽略查询字符串并且不带查询字符串按指定方式返回图像，来使用此特性。 此查询字符串指定缩放倍数、对比度设置和语言；例如，通知中提供的“www.website.com/images/hello.png”值将变为“www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us” |


### <a name="tilebasictext"></a>TileBasicText
在各种特殊模板上使用的基本文本元素。

| 属性 | 在任务栏的搜索框中键入 | 必需 |描述 |
|---|---|---|---|
| **文本** | string | false | 要显示的文本。 |
| **语言** | string | false | XML 负载的目标区域设置，以 BCP-47 语言标记指定，如“en-US”或“fr-FR”。 此处指定的区域设置会覆盖其他指定的任何区域设置，如绑定或视觉中指定的设置。 如果此值为文字字符串，则此属性默认为用户的用户界面语言。 如果此值为字符串参考，则此属性默认为解析字符串时由 Windows 运行时选择的区域设置。 |


## <a name="related-topics"></a>相关主题

* [快速入门：发送本地磁贴通知](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [GitHub 上的通知库](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/dev/Notifications)