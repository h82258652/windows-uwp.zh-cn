---
Description: 本主题说明限定符的常规概念、如何使用它们以及每个限定符名称的用途。
title: 定制语言、比例、高对比度和其他限定符的资源
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 资源, 图像, 资产, MRT, 限定符
ms.localizationpriority: medium
ms.openlocfilehash: db5ba7fb6e13d2a5aab4b74014458073aa88e7f4
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67820216"
---
# <a name="tailor-your-resources-for-language-scale-high-contrast-and-other-qualifiers"></a>定制语言、比例、高对比度和其他限定符的资源

本主题说明资源限定符的常规概念、如何使用它们以及每个限定符名称的用途。 有关所有可能的限定符值的参考表，请参阅 [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)。

你的应用可加载按运行时环境（例如显示语言、高对比度、[显示比例系数](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)以及许多其他项）定制的资产和资源。 执行此操作的方法是命名资源的文件夹或文件，以匹配与这些环境相对应的限定符名称和限定符值。 例如，你可能想让应用在高对比度模式下加载另一组图像资产。

有关对应用进行本地化的价值主张的详细信息，请参阅[全球化和本地化](../design/globalizing/globalizing-portal.md)。

## <a name="qualifier-name-qualifier-value-and-qualifier"></a>限定符名称、限定符值和限定符

限定符名称是映射到一组限定符值的键。 下面是对比度的限定符名称和限定符值。

| 上下文 | 限定符名称 | 限定符值 |
| :--------------- | :--------------- | :--------------- |
| 高对比度设置 | contrast | standard、high、black、white |

你可以将限定符名称和限定符值结合起来形成限定符。 `<qualifier name>-<qualifier value>` 是一个限定符的格式。 `contrast-standard` 是一个限定符的示例。

因此，对于高对比度，限定符集是 `contrast-standard`、`contrast-high`、`contrast-black` 和 `contrast-white`。 限定符名称和限定符值不区分大小写。 例如，`contrast-standard`和`Contrast-Standard` 是相同的限定符。

## <a name="use-qualifiers-in-folder-names"></a>在文件夹名称中使用限定符

下面是一个使用限定符命名包含资产文件的文件夹的示例。 如果每个限定符都有一些资产文件，请在文件夹名称中使用限定符。 这样，你可以在文件夹级别设置一次限定符，限定符便可应用于文件夹内的所有内容。

```console
\Assets\Images\contrast-standard\<logo.png, and other image files>
\Assets\Images\contrast-high\<logo.png, and other image files>
\Assets\Images\contrast-black\<logo.png, and other image files>
\Assets\Images\contrast-white\<logo.png, and other image files>
```

如果你按照上述示例命名文件夹，那么你的应用会使用高对比度设置从针对适当限定符命名的文件夹中加载资源文件。 因此，如果该设置为“高对比度黑色”，则会加载 `\Assets\Images\contrast-black` 文件夹中的资源文件。 如果该设置为“无”（即，计算机未处于高对比度模式），则会加载 `\Assets\Images\contrast-standard` 文件夹中的资源文件。

## <a name="use-qualifiers-in-file-names"></a>在文件名称中使用限定符

你可以使用限定符来命名资源文件本身，而不是创建和命名文件夹。 如果每个限定符只有一个资源文件，你可能更喜欢执行此操作。 以下提供了一个示例。

```console
\Assets\Images\logo.contrast-standard.png
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.contrast-black.png
\Assets\Images\logo.contrast-white.png
```

加载的文件是名称中包含最适合该设置的限定符的文件。 此匹配逻辑的工作原理对文件名和文件夹名称都相同。

## <a name="reference-a-string-or-image-resource-by-name"></a>按名称引用字符串或图像资源

请参阅[引用 XAML 标记中的字符串资源标识符](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-xaml)、[引用代码中的字符串资源标识符](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-code)以及[引用 XAML 标记和代码中的图像或其他资产](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)。

## <a name="actual-and-neutral-qualifier-matches"></a>实际与中性限定符匹配项
你无需为*每个*限定符值都提供资源文件。 例如，如果你发现高对比度和标准对比度都只需要一个视觉资产，则可以按如下所示命名这些资产。

```console
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.png
```

第一个文件名包含 `contrast-high` 限定符。 当高对比度处于*打开*状态时，该限定符是任何高对比度设置的*实际*匹配项。 换言之，它是一个接近的匹配项，因此是首选。 仅当限定符包含*实际*值时，才存在*实际*匹配项，就像上述示例中一样。 在该示例中，`high` 是 `contrast` 的*实际*值。

名为 `logo.png` 的文件自身根本没有对比度限定符。 限定符缺少一个*中性*值。 如果找不到首选匹配项，则中性值可用作回退匹配项。 在此示例中，如果高对比度处于*关闭*状态，则没有实际匹配项。 *中性*匹配项是可以找到的最佳匹配项，因此会加载资产 `logo.png`。

如果你要将 `logo.png` 的名称更改为 `logo.contrast-standard.png`，则文件名将包含实际限定符值。 关闭高对比度后，将存在 `logo.contrast-standard.png` 的实际匹配项，这是将加载的资产文件。 因此，将在相同条件下加载相同文件，但却是因为不同的匹配项而加载。

如果只需要一组资产用于高对比度，一组资产用于标准对比度，则可以使用文件夹名称而不是文件名。 在此情况下，彻底忽略文件夹名称可为你提供中性匹配项。

```console
\Assets\Images\contrast-high\<logo.png, and other images to load when high contrast theme is not None>
\Assets\Images\<logo.png, and other images to load when high contrast theme is None>
```

有关限定符匹配的工作原理的更多详细信息，请参阅[资源管理系统](resource-management-system.md)。

## <a name="multiple-qualifiers"></a>多个限定符

你可以在文件夹名称和文件名中合并限定符。 例如，你可能希望应用在高对比度模式处于打开状态*并且*显示比例系数为 400 时加载图像资产。 执行此操作的一种方法是使用嵌套文件夹。

```console
\Assets\Images\contrast-high\scale-400\<logo.png, and other image files>
```

对于 `logo.png` 以及要加载的其他文件，设置必须与*两个*限定符都匹配。

另一个选项是在一个文件夹名称中合并多个限定符。

```console
\Assets\Images\contrast-high_scale-400\<logo.png, and other image files>
```

在文件夹名称中，你可以合并用下划线分隔的多个限定符。 `<qualifier1>[_<qualifier2>...]` 是的格式。

你可以用相同格式在文件名中合并多个限定符。

```console
\Assets\Images\logo.contrast-high_scale-400.png
```

根据用于创建资产的工具和工作流，或者根据你所发现的最容易阅读和/或管理的内容，你可以为所有限定符选择单个命名策略，也可以针对不同限定符合并它们。

## <a name="alternateform"></a>AlternateForm

`alternateform` 限定符用于为某些特殊用途提供资源的替代形式。 这通常仅供日语应用开发人员使用，用于提供保留 `msft-phonetic` 值所针对的假名注音字符串（请参阅[如何为本地化做好准备](https://docs.microsoft.com/previous-versions/windows/apps/hh967762(v=win.10))中的“支持可存储的日语字符串的假名注音”部分。

你的目标系统或应用必须提供匹配 `alternateform` 限定符所依据的值。 请不要将 `msft-` 前缀用于自己的自定义 `alternateform` 限定符值。

## <a name="configuration"></a>配置

需要 `configuration` 限定符名称的可能性较低。 它可用于指定仅适用于给定的创作时环境的资源，例如仅用于测试的资源。

`configuration` 限定符用于加载与 `MS_CONFIGURATION_ATTRIBUTE_VALUE` 环境变量的值最佳匹配的资源。 因此，你可以将变量设置为已分配给相关资源的字符串值，例如 `designer` 或 `test`。

## <a name="contrast"></a>与此示例

`contrast` 限定符用于提供与高对比度设置最佳匹配的资源。

## <a name="custom"></a>自定义

你的应用可以为 `custom` 限定符设置值，以后系统会加载与该值最佳匹配的资源。 例如，你可能希望根据你的应用许可证来加载资源。 当你的应用启动时，它会检查其许可证，并将其用作 `custom` 限定符的值，所采用的方法是调用 [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue)，如代码示例中所示。

```csharp
public void SetLicenseLevel(BrandID brand)
{
    if (brand == BrandID.Premium)
    {
        ResourceContext.SetGlobalQualifierValue("Custom", "Premium", ResourceQualifierPersistence.LocalMachine);
    }
    else if (brand == BrandID.Standard)
    {
        ResourceContext.SetGlobalQualifierValue("Custom", " Standard", ResourceQualifierPersistence.LocalMachine);
    }
    else
    {
        ResourceContext.SetGlobalQualifierValue("Custom", "Trial", ResourceQualifierPersistence.LocalMachine);
    }
}
```

在此方案中，你将为你的资源提供包括 `custom-premium`、`custom-standard` 和 `custom-trial` 限定符的名称。

## <a name="devicefamily"></a>DeviceFamily

需要 `devicefamily` 限定符名称的可能性较低。 你可以并且应该尽可能避免使用它，因为可以改用更加方便和可靠的方法。 [检测正运行应用的平台](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on)和[版本自适应代码](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)中介绍了这些方法。

但是，万不得已时，可以使用 devicefamily 限定符来命名包含 XAML 视图的文件夹（XAML 视图是包含 UI 布局和控件的 XAML 文件）。

```console
\devicefamily-desktop\<MainPage.xaml, and other markup files to load when running on a desktop computer>
\devicefamily-mobile\<MainPage.xaml, and other markup files to load when running on a phone>
```

或者，你可以命名文件。

```console
\MainPage.devicefamily-desktop.xaml
\MainPage.devicefamily-mobile.xaml
```

在任一情况下，`MainPage.[<qualifier>].xaml` 的每个副本都共享一个在你的项目中名称、位置和内容保持不变的通用 `MainPage.xaml.cs`。

你还可以使用 devicefamily 限定符命名资源文件 (`.resw`) 或文件夹。 例如，当你的应用在移动设备系列上运行时，UI 元素 `<TextBlock x:Uid="DeviceFriendlyName"/>` 将使用 `Resources.devicefamily-mobile.resw` 文件中定义的文本和前台资源，条件是该文件包含以下内容

```xml
<data name="DeviceFriendlyName.Foreground">
    <value>Red</value>
</data>
<data name="DeviceFriendlyName.Text">
    <value>Mobile device</value>
</data>
```

有关使用资源文件的详细信息，请参阅[本地化你的 UI 字符串](localize-strings-ui-manifest.md)。

## <a name="dxfeaturelevel"></a>DXFeatureLevel

需要 `dxfeaturelevel` 限定符名称的可能性较低。 它旨在与 Direct3D 游戏资产配合使用，以导致加载下层资源，从而与当时的特定下层硬件配置匹配。 但是，该硬件配置的普及程度现在非常低，因此我们建议你不要使用此限定符。

## <a name="homeregion"></a>HomeRegion

`homeregion` 限定符对应于用户的国家或地区设置。 它表示用户的主位置。 值包括任何有效的 [BCP-47 区域标记](https://go.microsoft.com/fwlink/p/?linkid=227302)。 即，任何 **ISO 3166 1 alpha-2** 双字母区域代码，以及所构成区域的一组 **ISO 3166-1 数字**三位数地理代码（请参阅[联合国统计部门 M49 区域代码构成](https://go.microsoft.com/fwlink/p/?linkid=247929)）。 “选定的经济组织和其他组织”的代码无效。

## <a name="language"></a>语言

`language` 限定符对应于显示语言设置。 值包括任何有效的 [BCP-47 语言标记](https://go.microsoft.com/fwlink/p/?linkid=227302)。 有关语言的列表，请参阅 [IANA 语言子标记注册表](https://go.microsoft.com/fwlink/p/?linkid=227303)。

如果你希望应用支持其他显示语言，并且你的代码或 XAML 标记中有字符串文本，则从代码/标记内将这些字符串移到资源文件 (`.resw`) 中。 然后，你可以针对应用支持的每种语言制作该资源文件的翻译副本。

你通常使用 `language` 限定符来命名包含资源文件 (`.resw`) 的文件夹。

```console
\Strings\language-en\Resources.resw
\Strings\language-ja\Resources.resw
```

你可以忽略 `language` 限定符的 `language-` 部分（即，限定符名称）。 你不能对其他类型的限定符执行此操作；只能在文件夹名称中进行此操作。

```console
\Strings\en\Resources.resw
\Strings\ja\Resources.resw
```

你可以使用 `language` 限定符来命名资源文件本身，而不是命名文件夹。

```console
\Strings\Resources.language-en.resw
\Strings\Resources.language-ja.resw
```

有关使用字符串资源使你的应用可本地化，以及如何在应用中引用字符串资源的详细信息，请参阅[本地化你的 UI 字符串](localize-strings-ui-manifest.md)。

## <a name="layoutdirection"></a>LayoutDirection

`layoutdirection` 限定符对应于显示语言设置的布局方向。 例如，像阿拉伯语或希伯来语这样从右到左阅读的语言，可能需要镜像图像。 如果设置了 [FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) 属性，则 UI 中的布局面板和图像将相应地响应布局方向（请参阅[调整布局和字体并支持 RTL](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)）。 但是，`layoutdirection` 限定符适用于简单翻转无法满足要求的情况，可以采用更常规的方式响应特定阅读顺序和文本对齐的方向性。

## <a name="scale"></a>比例

Windows 会根据其 DPI（每英寸点数）和设备的观看距离自动为每个显示器选择一个缩放比例。 请参阅[有效像素和缩放比例](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)。 你应该创建多个建议大小（至少包括 100、200 和 400）的图像，使 Windows 可以选择理想的大小或使用最近大小并进行缩放。 这样一来，Windows 可以识别哪个物理文件包含适合显示比例系数的正确大小的图像，你使用 `scale` 限定符。 资源的比例与 [DisplayInformation.ResolutionScale](/uwp/api/windows.graphics.display.displayinformation.ResolutionScale) 或下一个最大比例资源的值匹配。

下面是在文件夹级别设置限定符的示例。

```console
\Assets\Images\scale-100\<logo.png, and other image files>
\Assets\Images\scale-200\<logo.png, and other image files>
\Assets\Images\scale-400\<logo.png, and other image files>
```

此示例在文件级别对其进行设置。

```console
\Assets\Images\logo.scale-100.png
\Assets\Images\logo.scale-200.png
\Assets\Images\logo.scale-400.png
```

有关限定资源的 `scale` 和 `targetsize` 的信息，请参阅[限定图像资源的目标大小](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize)。

## <a name="targetsize"></a>TargetSize

`targetsize` 限定符主要用于指定要在文件资源管理器中显示的[文件类型关联图标](https://docs.microsoft.com/windows/desktop/shell/how-to-assign-a-custom-icon-to-a-file-type)或[协议图标](https://docs.microsoft.com/windows/desktop/search/-search-3x-wds-ph-ui-extensions)。 限定符值表示以原始（物理）像素为单位的正方形图像的边长。 系统会加载其值与文件资源管理器中的“视图”设置匹配的资源；或者在缺少完全匹配的情况下加载具有下一个最大值的资源。

你可以在应用程序包清单设计器的“可见资产”选项卡中定义资产，以表示应用图标 (`/Assets/Square44x44Logo.png`) 的 `targetsize` 限定符值的一些大小。

有关限定资源的 `scale` 和 `targetsize` 的信息，请参阅[限定图像资源的目标大小](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize)。

## <a name="theme"></a>主题

`theme` 限定符用于提供与默认应用模式设置最匹配的资源，或使用 [Application.RequestedTheme](/uwp/api/windows.ui.xaml.application.requestedtheme) 的应用替代。


## <a name="shell-light-theme-and-unplated-resources"></a>Shell 浅色主题和未着色的资源
*Windows 10 可能 2019年更新*适用于 Windows 外壳程序引入新的"light"主题。 因此，现在将浅色背景上显示以前在深色背景显示一些应用程序资产。 对于应用该应用程序，提供 altform 未着色资产对于任务栏和窗口切换器 （Alt + Tab、 任务视图等），您应验证它们具有可接受的对比度浅色背景上。

### <a name="providing-light-theme-specific-assets"></a>提供浅色主题特定资产
想要提供量身定制的资源，shell 浅色主题就可以使用新的替代形式的资源限定符的应用： `altform-lightunplated`。 此限定符镜像现有 altform 未着色限定符。 

### <a name="downlevel-considerations"></a>下层注意事项
应用程序不应使用`theme-light`限定符`altform-unplated`限定符。 这将导致不可预知的行为上 RS5 和为任务栏加载早期版本的 Windows 由于方式资源。 在早期版本的 windows 中，可能会错误地使用的主题 light 版本。 `altform-lightunplated`限定符可避免此问题。 

### <a name="compatibility-behavior"></a>兼容性的行为
有关向后兼容性，Windows 包括逻辑来检测单色图标，并检查它是否与预期背景对比。 如果图标不能满足对比度要求，Windows 将查找对比度-白色版的资产。 如果不可用，Windows 将回退到使用镍的版的资产。



## <a name="important-apis"></a>重要的 API

* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue)

## <a name="related-topics"></a>相关主题

* [有效像素和缩放比例](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)
* [资源管理系统](resource-management-system.md)
* [用于本地化的准备工作](https://docs.microsoft.com/previous-versions/windows/apps/hh967762(v=win.10))
* [检测到平台上运行你的应用](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on)
* [设备系列概述](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)
* [本地化 UI 字符串](localize-strings-ui-manifest.md)
* [BCP-47](https://go.microsoft.com/fwlink/p/?linkid=227302)
* [联合国统计信息除法 M49 组成的区域代码](https://go.microsoft.com/fwlink/p/?linkid=247929)
* [IANA 语言子标记注册表](https://go.microsoft.com/fwlink/p/?linkid=227303)
* [调整布局和字体并支持 RTL](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)
