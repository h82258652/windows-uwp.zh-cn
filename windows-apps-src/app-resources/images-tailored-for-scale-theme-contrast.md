---
Description: 应用可以加载在显示比例系数、主题、高对比度和其他运行时上下文方面进行了定制的图像所在的图像资源文件。
title: 加载为比例、主题、高对比度和其他定制的图像和资产
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 资源, 图像, 资产, MRT, 限定符
ms.localizationpriority: medium
ms.openlocfilehash: 2aadcb8dc3d414db7951dc571855e01bddb03a99
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683640"
---
# <a name="load-images-and-assets-tailored-for-scale-theme-high-contrast-and-others"></a>加载为比例、主题、高对比度和其他定制的图像和资产
你的应用可以加载为[显示比例系数](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)、主题、高对比度和其他运行时上下文定制的图像资源文件（或其他资产文件）。 这些图像可以从强制性代码或 XAML 标记引用，如作为 **Image** 的 **Source** 属性。 它们还可以显示在应用包清单源文件（`Package.appxmanifest` 文件）中 &mdash; 例如，作为 Visual Studio 清单设计器的“视觉资源”选项卡上应用图标的值 &mdash; 或显示在磁贴和 toast 上。 通过在图像的文件名称中使用限定符，并选择性地在 [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) 的帮助下动态加载它们，你可以加载与用户的显示比例、主题、高对比度、语言和其他上下文的运行时设置最匹配的最合适图像文件。

图像资源包含在图像资源文件中。 你也可以将图像视为资产，将包含该图像的文件视为资产文件；而且你可以在你的项目的 \Assets 文件夹中找到这些类型的资源文件。 有关如何在图像资源文件的名称中使用限定符的背景，请参阅[定制语言、比例和其他限定符的资源](tailor-resources-lang-scale-contrast.md)。

图像的一些常见限定符是[比例](tailor-resources-lang-scale-contrast.md#scale)、[主题](tailor-resources-lang-scale-contrast.md#theme)、[对比度](tailor-resources-lang-scale-contrast.md#contrast)和[目标大小](tailor-resources-lang-scale-contrast.md#targetsize)。

## <a name="qualify-an-image-resource-for-scale-theme-and-contrast"></a>限定图像资源的比例、主题和对比度
`scale` 限定符的默认值是 `scale-100`。 因此，这两种变体是等效的（都提供比例为 100 或比例系数为 1 的图像）。

<blockquote>
<pre>
\Assets\Images\logo.png
\Assets\Images\logo.scale-100.png
</pre>
</blockquote>


你可以在文件夹名称，而不是文件名称中使用限定符。 如果每个限定符有多个资产文件，则将是一个更好的策略。 为了便于说明，这两种变体同等于上面两种。

<blockquote>
<pre>
\Assets\Images\logo.png
\Assets\Images\scale-100\logo.png
</pre>
</blockquote>

接下来举例说明针对不同的显示比例、主题和高对比度设置提供一个图像资源名称为 `/Assets/Images/logo.png` 的变体。 此示例使用文件夹命名。

<blockquote>
<pre>
\Assets\Images\contrast-standard\theme-dark
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-standard\theme-light
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-high
    \scale-100\logo.png
    \scale-200\logo.png
</pre>
</blockquote>

## <a name="reference-an-image-or-other-asset-from-xaml-markup-and-code"></a>引用 XAML 标记和代码中的图像或其他资产
图像资源的名称或限定符为其路径及删除了所有限定符的文件名称。 如果你按照上一部分的任何示例为文件夹和/或文件命名，则你有一个图像资源，且其名称（作为绝对路径）为 `/Assets/Images/logo.png`。 下面介绍如何在 XAML 标记中使用该名称。

```xaml
<Image x:Name="myXAMLImageElement" Source="ms-appx:///Assets/Images/logo.png"/>
```

请注意，你使用的是 `ms-appx` URI 方案，因为你引用的是来自应用包的文件。 请参阅 [URI 方案](uri-schemes.md)。 下面介绍如何在强制性代码中引用该相同图像资源。

```csharp
this.myXAMLImageElement.Source = new BitmapImage(new Uri("ms-appx:///Assets/Images/logo.png"));
```

你可以使用 `ms-appx` 从应用包加载任何任意文件。

```csharp
var uri = new System.Uri("ms-appx:///Assets/anyAsset.ext");
var storagefile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(uri);
```

`ms-appx-web` 方案与 `ms-appx` 访问相同的文件，但是前者在 Web 隔离舱中访问。

```xaml
<WebView x:Name="myXAMLWebViewElement" Source="ms-appx-web:///Pages/default.html"/>
```

```csharp
this.myXAMLWebViewElement.Source = new Uri("ms-appx-web:///Pages/default.html");
```

对于这些示例中所示的任何方案，请使用推断 [UriKind](https://docs.microsoft.com/dotnet/api/system.urikind) 的 [Uri 构造函数](https://docs.microsoft.com/dotnet/api/system.uri.-ctor?view=netcore-2.0#System_Uri__ctor_System_String_)重载。 指定一个包括方案和颁发机构的有效的绝对 URI，或者如上述示例所示让颁发机构默认为应用包。

注意在这些示例 URI 中，方案（“`ms-appx`”或“`ms-appx-web`”）后依次跟随“`://`”和绝对路径。 在绝对路径中，前导“`/`”导致从包的根解释路径。

> [!NOTE]
> `ms-resource` （对于[字符串资源](localize-strings-ui-manifest.md)）和 `ms-appx(-web)` （对于图像和其他资产），URI 方案会执行自动限定符匹配，以查找最适合当前上下文的资源。 `ms-appdata` URI 方案（用于加载应用数据）不执行任何此类自动匹配，但你可以响应 [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) 的内容，并使用它们在 URI 中的完整物理文件名从应用数据明确加载合适的资产。 有关应用数据的信息，请参阅[存储和检索设置以及其他应用数据](../design/app-settings/store-and-retrieve-app-data.md)。 Web URI 方案（如 `http`、`https`和 `ftp`）也无法执行自动匹配。 有关如何处理这种情况的信息，请参阅[在云中托管和加载图像](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md#hosting-and-loading-images-in-the-cloud)。

如果你的图像文件保留它们在项目结构中所在的位置，绝对路径是一个不错的选择。 如果你希望能够移动图像文件，但又注意让它们与其引用 XAML 标记文件的相对位置保持不变，则你可能不想要使用绝对路径，而是改为使用相对于其所在的标记文件的路径。 如果这样做，你不必使用 URI 方案。 在这种情况下，你仍会获得自动限定符匹配的好处，但这只是因为你使用的是 XAML 标记中的相对路径。

```xaml
<Image Source="Assets/Images/logo.png"/>
```

另请参阅[磁贴和 toast 的语言、比例和高对比度支持](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)。

## <a name="qualify-an-image-resource-for-targetsize"></a>限定目标大小的图像资源
你可以对相同图像资源的不同变体使用 `scale` 和 `targetsize` 限定符，但不能在一个资源的一个变体上同时使用它们。 另外，你需要定义至少一个不具有 `TargetSize` 限定符的变体。 该变体必须定义 `scale` 的值或使其默认为 `scale-100`。 因此，`/Assets/Square44x44Logo.png` 资源的这两种变体均有效。

<blockquote>
<pre>
\Assets\Square44x44Logo.scale-200.png
\Assets\Square44x44Logo.targetsize-24.png
</pre>
</blockquote>

且以下两种变体有效。 

<blockquote>
<pre>
\Assets\Square44x44Logo.png // defaults to scale-100
\Assets\Square44x44Logo.targetsize-24.png
</pre>
</blockquote>

但此变体无效。

<blockquote>
<pre>
\Assets\Square44x44Logo.scale-200_targetsize-24.png
</pre>
</blockquote>

## <a name="refer-to-an-image-file-from-your-app-package-manifest"></a>引用来自应用包清单的图像文件
如果你按照上一部分的两个有效示例的其中任何一个示例为文件夹和/或文件命名，则你有一个应用图标图像资源，且其名称（作为相对路径）为 `Assets\Square44x44Logo.png`。 在你的应用包清单中，只需按名称引用资源。 无需使用任何 URI 方案。

![添加资源，英语](images/app-icon.png)

这就是你需要执行的所有操作，操作系统会执行自动限定符匹配，以查找最适合当前上下文的资源。 有关你可以本地化或以这种方式进行限定的应用包清单中的所有项目的列表，请参阅[可本地化清单项目](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)。

## <a name="qualify-an-image-resource-for-layoutdirection"></a>限定布局方向的图像资源
请参阅[镜像图像](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images)。

## <a name="load-an-image-for-a-specific-language-or-other-context"></a>为特定语言或其他上下文加载图像
有关对应用进行本地化的价值主张的详细信息，请参阅[全球化和本地化](../design/globalizing/globalizing-portal.md)。

默认 [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)（从 [**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView) 获取）包含每个限定符名称的限定符值，表示默认运行时上下文（换言之，即当前用户和计算机的设置）。 根据该运行时上下文中的限定符值匹配图像文件 - 基于其名称中的限定符。

但在有些时候，你可能想要你的应用覆盖系统设置，并明确当查找要加载的匹配图像时要使用的语言、比例或其他限定符值。 例如，你可能想要精确控制何时加载哪个高对比度图像。

为此，你可以构建一个新的 **ResourceContext**（而不使用默认值），覆盖它的值，然后在你的图像查找中使用该上下文对象。

```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView 
resourceContext.QualifierValues["Contrast"] = "high";
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
var resourceCandidate = namedResource.Resolve(resourceContext);
var imageFileStream = resourceCandidate.GetValueAsStreamAsync().GetResults();
var bitmapImage = new Windows.UI.Xaml.Media.Imaging.BitmapImage();
bitmapImage.SetSourceAsync(imageFileStream);
this.myXAMLImageElement.Source = bitmapImage;
```

为了在全局级别达到相同的效果，*可以* 覆盖默认 **ResourceContext** 中的限定符值。 但我们建议你调用 [**ResourceContext.SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)。 你通过调用 **SetGlobalQualifierValue** 一次性设置值，则每次当你使用默认 **ResourceContext** 进行查找时，这些值都会对它产生影响。 默认情况下，[**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) 类使用默认 **ResourceContext**。

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Contrast", "high");
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
this.myXAMLImageElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
```

## <a name="updating-images-in-response-to-qualifier-value-change-events"></a>响应限定符值更改事件更新图像
你运行的应用可以响应影响默认资源上下文中的限定符值的系统设置更改。 其中任何系统设置在 [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) 上调用 [**MapChanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) 事件。

为了响应此事件，可以在 [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) 默认使用的默认 **ResourceContext** 的帮助下重新加载图像。

```csharp
public MainPage()
{
    this.InitializeComponent();

    ...

    // Subscribe to the event that's raised when a qualifier value changes.
    var qualifierValues = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
    qualifierValues.MapChanged += new Windows.Foundation.Collections.MapChangedEventHandler<string, string>(QualifierValues_MapChanged);
}

private async void QualifierValues_MapChanged(IObservableMap<string, string> sender, IMapChangedEventArgs<string> @event)
{
    var dispatcher = this.myImageXAMLElement.Dispatcher;
    if (dispatcher.HasThreadAccess)
    {
        this.RefreshUIImages();
    }
    else
    {
        await dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => this.RefreshUIImages());
    }
}

private void RefreshUIImages()
{
    var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
    this.myImageXAMLElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
}
```

## <a name="important-apis"></a>重要的 API
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)
* [ResourceContext. SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>相关主题
* [为语言、缩放和其他限定符定制资源](tailor-resources-lang-scale-contrast.md)
* [对 UI 和应用包清单中的字符串进行本地化](localize-strings-ui-manifest.md)
* [存储和检索设置以及其他应用数据](../design/app-settings/store-and-retrieve-app-data.md)
* [磁贴和 toast 支持语言、缩放和高对比度](tile-toast-language-scale-contrast.md)
* [可本地化清单项](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [镜像图像](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images)
* [全球化和本地化](../design/globalizing/globalizing-portal.md)
