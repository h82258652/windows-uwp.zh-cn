---
author: stevewhims
Description: Your tiles and toasts can load strings and images tailored for display language, display scale factor, high contrast, and other runtime contexts.
title: 磁贴和 toast 通知的语言、比例和高对比度支持
template: detail.hbs
ms.author: stwhi
ms.date: 10/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 资源, 图像, 资产, MRT, 限定符
ms.localizationpriority: medium
ms.openlocfilehash: 87aafe36d05298a8fa157426e39c530190f98908
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2018
ms.locfileid: "4966910"
---
# <a name="tile-and-toast-notification-support-for-language-scale-and-high-contrast"></a>磁贴和 toast 通知的语言、比例和高对比度支持

你的磁贴和 toast 可以加载为显示语言、[显示比例系数](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md)、高对比度和其他运行时上下文定制的字符串和图像。 有关如何在资源文件的名称中使用限定符的背景，请参阅[定制语言、 比例和其他限定符的资源](../../../app-resources/tailor-resources-lang-scale-contrast.md)和[应用图标和徽标](/windows/uwp/design/style/app-icons-and-logos)。

有关对应用进行本地化的价值主张的详细信息，请参阅[全球化和本地化](../../globalizing/globalizing-portal.md)。

## <a name="refer-to-a-string-resource-from-a-template"></a>从模板引用字符串资源

在你的磁贴或 toast 模板中，你可以使用后跟简单字符串资源标识符的 `ms-resource` URI（统一资源标识符）方案来引用字符串资源。 例如，如果你有一个包含名称为“Farewell”的资源条目的 Resources.resx 文件，则必须具有一个标识符为“Farewell”的字符串资源。 有关字符串资源标识符和资源文件 (.resw) 的详细信息，请参阅[本地化 UI 和应用包清单中的字符串](../../../app-resources/localize-strings-ui-manifest.md)。

这是“Farewell”字符串资源标识符引用在使用 `ms-resource` 的模板内容的[文本](/uwp/schemas/tiles/tilesschema/element-text?branch=live)正文中的外观。

```xml
<text id="1">ms-resource:Farewell</text>
```

如果你省略 `ms-resource` URI 方案，则文本正文只是一个字符串参数，*不是*标识符引用。

```xml
<text id="1">Farewell</text>
```

## <a name="refer-to-an-image-resource-from-a-template"></a>引用模板中的图像资源

在你的磁贴或 toast 模板中，你可以使用后跟图像资源名称的 `ms-appx` URI（统一资源标识符）方案来引用图像资源。 方法与你引用 XAML 标记中的图像资源的方法相同（有关详细信息，请参阅[引用 XAML 标记和代码中的图像或其他资产](../../../app-resources/images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)）。

例如，你可以这样给文件夹命名。

```
\Assets\Images\contrast-standard\welcome.png
\Assets\Images\contrast-high\welcome.png
```

在本例中，你有一个图像资源且其名称（作为绝对路径）为 `/Assets/Images/welcome.png`。 下面介绍如何在模板中使用该名称。

```xml
<image id="1" src="ms-appx:///Assets/Images/welcome.png"/>
```

注意在此示例 URI 中，方案（“`ms-appx`”）后依次跟随“`://`”和绝对路径（以“`/`”开头的绝对路径）。

## <a name="hosting-and-loading-images-in-the-cloud"></a>在云中托管和加载图像

`ms-resource` 和 `ms-appx` URI 方案执行自动限定符匹配，以查找最适合当前上下文的资源。 Web URI 方案（如 `http`、`https` 和 `ftp`）无法执行任何此类自动匹配。

改为在你的图像的 URI 上附加一个描述请求的限定符值的查询字符串。

```xml
<image id="1" src="http://www.contoso.com/Assets/Images/welcome.png?ms-lang=en-US"/>
```

然后，在提供图像的应用服务中，实施检查和使用查询字符串确定要返回的图像的 HTTP 处理程序。

你还需在[磁贴](/uwp/schemas/tiles/tilesschema/schema-root?branch=live)或 [toast](/uwp/schemas/tiles/toastschema/schema-root?branch=live) 通知 XML 负载中将 [**addImageQuery**](/uwp/schemas/tiles/tilesschema/element-visual?branch=live) 属性设置为 `true`。 **addImageQuery** 属性显示在磁贴和 toast 架构的 `visual`、`binding` 和 `image` 元素中。 在元素上显式设置 **addImageQuery** 将覆盖在上级元素上设置的任何值。 例如，在 `image` 元素中为 `true` 的 **addImageQuery** 值覆盖在其父级 `binding` 元素中为 `false` 的 **addImageQuery**。

以下是你可以使用的查询字符串。

| 限定符 | 查询字符串 | 示例 |
| --------- | ------------ | ------- |
| 比例 | ms-scale | ?ms-scale=400 |
| 语言 | ms-lang | ?ms-lang=en-US |
| 对比度 | ms-contrast | ?ms-contrast=high |

有关你在查询字符串中可以使用的所有可能的限定符值的参考表，请参阅 [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)。

## <a name="important-apis"></a>重要的 API

* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)

## <a name="related-topics"></a>相关主题

* [响应式设计的屏幕大小和断点](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [定制语言、比例和其他限定符的资源](../../../app-resources/tailor-resources-lang-scale-contrast.md)
* [磁贴和图标资产指南](app-assets.md)。
* [全球化和本地化](../../globalizing/globalizing-portal.md)
* [对 UI 和应用包清单中的字符串实施本地化](../../../app-resources/localize-strings-ui-manifest.md)
* [引用 XAML 标记和代码中的图像或其他资产](../../../app-resources/images-tailored-for-scale-theme-contrast.md)
* [addImageQuery](/uwp/schemas/tiles/tilesschema/element-visual?branch=live)
* [磁贴架构](/uwp/schemas/tiles/tilesschema/schema-root?branch=live)
* [Toast 架构](/uwp/schemas/tiles/toastschema/schema-root?branch=live)
