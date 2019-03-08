---
title: 为已转换的桌面应用和游戏使用 MRT
description: 通过将你的 .NET 或 Win32 应用或游戏打包为 AppX 包，你可以利用资源管理系统加载为运行时上下文定制的应用资源。 本主题对方法进行了深入描述。
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp, mrt, pri. 资源, 游戏, centennial, desktop app converter, mui, 卫星程序集
ms.localizationpriority: medium
ms.openlocfilehash: 287c22cbd50f1b69f505bbddd445740fe9422c31
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597012"
---
# <a name="use-the-windows-10-resource-management-system-in-a-legacy-app-or-game"></a>在旧应用或游戏中使用 Windows 10 资源管理系统

## <a name="overview"></a>概述

.NET 和 Win32 应用和游戏通常本地化为不同语言，从而扩展总目标市场。 有关对应用进行本地化的价值主张的详细信息，请参阅[全球化和本地化](../design/globalizing/globalizing-portal.md)。 通过将你的 .NET 或 Win32 应用或游戏打包为 AppX 包，你可以利用资源管理系统加载为运行时上下文定制的应用资源。 本主题对方法进行了深入描述。

有多种方法可本地化传统的 Win32 应用程序，但 Windows 8 引入了[新资源管理系统](https://msdn.microsoft.com/en-us/library/windows/apps/jj552947.aspx)，它可以跨各种编程语言和应用程序类型进行工作，并提供超越简单本地化的功能。 本主题中，该系统将被称为“MRT”。 过去，这代表“现代资源技术”，但“现代”一词已停止使用。 资源管理器也可以被称为 MRM（现代资源管理器）或 PRI（包资源索引）。

与基于 AppX 的部署（例如，来自 Microsoft Store）相结合，MRT 可以自动为指定用户/设备提供最适用的资源，这可以最大程度地减小应用程序下载和安装的大小。 减小大小对于具有大量本地化内容的应用程序来说非常有意义，或许近似于 AAA 游戏的几*千兆字节*。 MRT 的其他好处包括 Windows Shell 和 Microsoft Store 的本地化列表，用户的首选语言与可用资源不匹配时的自动回退逻辑。

本文介绍 MRT 的高级体系结构，并提供用于帮助在进行最少量代码更改的情况下将传统 Win32 应用程序移至 MRT 的移植指南。 移至 MRT 后，开发人员还可以获得更多好处（如按比例系数或系统主题分类资源）。 请注意，基于 MRT 的本地化同时适用于桌面桥（又称“Centennial”）处理的 UWP 应用程序和 Win32 应用程序。

在许多情况下，你可以继续使用现有的本地化格式和源代码，同时与 MRT 集成以在运行时解析资源并最小化下载大小 - 这不是“全有或全无”的方法。 下表汇总了每个阶段的工作和估计成本/好处。 此表不包含非本地化任务，如提供高分辨率或高对比度的应用程序图标。 有关为磁贴、图标等提供多个资产的详细信息，请参阅[定制语言、比例、高对比度和其他限定符的资源](tailor-resources-lang-scale-contrast.md)。

<table>
<tr>
<th>Work</th>
<th>益处</th>
<th>估计成本</th>
</tr>
<tr>
<td>本地化 AppX 清单</td>
<td>让本地化内容显示在 Windows Shell 和 Microsoft Store 中需要完成的最基本工作</td>
<td>小</td>
</tr>
<tr>
<td>使用 MRT 确定并找到资源</td>
<td>最小化下载和安装大小的先决条件；自动语言回退</td>
<td>中等</td>
</tr>
<tr>
<td>生成资源包</td>
<td>最小化下载和安装大小的最后一步</td>
<td>小</td>
</tr>
<tr>
<td>迁移到 MRT 资源格式和 API</td>
<td>显著减小的文件大小（具体取决于现有资源技术）</td>
<td>大</td>
</tr>
</table>

## <a name="introduction"></a>简介

最非比寻常的应用程序包含称为*资源*的用户界面元素，其脱离自应用程序代码（与在源代码本身编写的*硬编码值*不同）。 有几个原因将资源的优先级放在硬编码值前面 - 例如，非开发人员编辑更轻松 - 但主要原因之一是支持应用程序在运行时选取相同逻辑资源的不同表示形式。 例如，按钮上显示的文本（或图标中显示的图像）可能因用户了解的语言、显示设备的特性，或者用户是否启用了任何辅助技术而不同。

因此，任何资源管理技术的主要用途是在运行时，将逻辑或符号*资源名称*（如 `SAVE_BUTTON_LABEL`）的请求，从一组可能的*候选项*（例如，“保存”、“存放”或“저장”）转换为可能的最佳实际*值*（例如，“保存”）。 MRT 提供此类功能，并使用各种属性（称为*限定符*）让应用程序能够确定资源候选项，如用户的语言、屏幕比例系数、用户所选的主题和其他环境因素。 MRT 甚至还支持应用程序（需要这些自定义限定符）的自定义限定符（例如，应用程序可能会为使用帐户登录的用户或来宾用户提供不同的图形资源，而不将此检查明确添加到其应用程序的每个部分）。 MRT 既处理字符串资源也处理基于文件的资源，其中基于文件的资源作为对外部数据（文本本身）的引用实现。 

### <a name="example"></a>示例

下面是一个在两个按钮（`openButton` 和 `saveButton`）上有文本标签并有用于徽标 (`logoImage`) 的 PNG 文件的应用程序示例。 文本标签被本地化为英语和德语，徽标针对正常桌面显示（100% 比例系数）和高分辨率手机（300% 比例系数）进行了优化。 请注意，此图表呈现的是模型的高级概念视图；它不会准确映射到实现。

<p><img src="images\conceptual-resource-model.png"/></p>

在图中，应用程序代码引用三个逻辑资源名称。 在运行时，`GetResource` 伪函数使用 MRT 在资源表（也称为 PRI 文件）中查找这些资源名称，并根据环境条件（用户的语言和显示的比例系数）查找最适合的候选项。 如果是标签，则直接使用字符串。 如果是徽标图像，字符串解释为文件名，并从磁盘读取文件。 

如果用户使用英语或德语以外的语言，或者显示比例系数不是 100% 或 300%，MRT 将基于一组回退规则选取“最接近”的匹配候选项（请参阅 [MSDN 中的**资源管理系统**主题](https://msdn.microsoft.com/en-us/library/windows/apps/jj552947.aspx)了解更多背景）。 

请注意，MRT 支持专门针对于多个限定符的资源-例如，如果徽标图像包含嵌入的文本，也需要进行本地化，徽标将有四个候选项：EN/规模-100、 DE/规模-100、 EN/规模-300 和 DE/规模-300。

### <a name="sections-in-this-document"></a>本文档的各个部分

以下部分概述了将 MRT 与你的应用程序集成所需执行的高级任务。

**阶段 0:生成应用程序包**

此部分概述了如何将现有的桌面应用程序生成为一个应用程序包。 在此阶段不使用任何 MRT 功能。

**第 1 阶段：本地化应用程序清单**

此部分概述了如何本地化应用程序的清单（以使其在 Windows Shell 中正确显示），同时仍使用旧的源格式和 API 打包并找到资源。 

**阶段 2:使用 MRT 来识别和查找资源**

此部分概述了如何修改应用程序代码（以及可能的资源布局）来使用 MRT 找到资源，同时仍使用现有资源格式和 API 加载和使用资源。 

**阶段 3:生成资源包**

此部分概述了将你的资源分成单独的*资源包*所需的最终更改，这可以最小化应用的下载（和安装）大小。

### <a name="not-covered-in-this-document"></a>本文档不包括

完成上述阶段 0-3 之后，你将有一个可以提交到 Microsoft Store 的应用程序“捆绑包”，它们还可以让用户通过忽略不需要的资源（例如，他们不使用的语言）最小化下载和安装大小。 可以采用一个最终步骤对应用程序的大小和功能进行进一步的改进。 

**阶段 4:迁移到 MRT 的资源格式和 Api**

此阶段超出了本文档的范围；它要求将资源（特别是字符串）从旧格式（如 MUI DLL 或 .NET 资源程序集）移入 PRI 文件。 这能够为下载和安装进一步节省空间。 它还允许使用其他 MRT 功能，例如根据比例系数、辅助功能设置等最小化图像文件的下载和安装。

- - -

## <a name="phase-0-build-an-application-package"></a>阶段 0:生成应用程序包

在对应用程序资源进行任何更改之前，必须先将当前的打包和安装技术替换为标准 UWP 打包和部署技术。 有三种方法可执行此操作：

0. 如果你有安装程序复杂的较大的桌面应用程序，或利用大量操作系统扩展点，你可以使用 Desktop App Converter 工具从现有的应用安装程序（例如，MSI）生成 UWP 文件布局和清单信息
0. 如果你有文件相对较少或安装程序简单的较小的桌面应用程序，且不连接扩展点，你可以手动创建文件布局和清单信息
0. 如果你在从源重建，并且想要将应用更新为“纯”UWP 应用程序，你可以在 Visual Studio 中创建新项目，并依靠 IDE 为你完成大量工作

如果你想要使用[Desktop App Converter](https://aka.ms/converter)，请参阅[**桌面到 UWP 桥：Desktop App Converter** MSDN 上的主题](https://aka.ms/converterdocs)的转换过程的详细信息。 可以在[**桌面桥到 UWP 示例** GitHub 存储库](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)中找到一整套桌面转换器示例。

如果你想要手动创建程序包，你将需要创建包含所有应用程序文件（可执行文件和内容，但不包括源代码）和 `AppXManifest.xml` 文件的目录结构。 示例可以在 [**Hello, World** GitHub 示例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/blob/master/Samples/HelloWorldSample/CentennialPackage/AppxManifest.xml)中找到，但运行名为 `ContosoDemo.exe` 的桌面可执行文件的基本 `AppXManifest.xml` 文件如下所示，其中的<span style="background-color: yellow">突出显示文本</span>会被替换为你自己的值：

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8" ?&gt;
&lt;Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         IgnorableNamespaces="uap mp rescap"&gt;
    &lt;Identity Name="<span style="background-color: yellow">Contoso.Demo</span>"
              Publisher="<span style="background-color: yellow">CN=Contoso.Demo</span>"
              Version="<span style="background-color: yellow">1.0.0.0</span>" /&gt;
    &lt;Properties&gt;
    &lt;DisplayName&gt;<span style="background-color: yellow">Contoso App</span>&lt;/DisplayName&gt;
    &lt;PublisherDisplayName&gt;<span style="background-color: yellow">Contoso, Inc</span>&lt;/PublisherDisplayName&gt;
    &lt;Logo&gt;Assets\StoreLogo.png&lt;/Logo&gt;
  &lt;/Properties&gt;
    &lt;Dependencies&gt;
    &lt;TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" /&gt;
  &lt;/Dependencies&gt;
    &lt;Resources&gt;
    &lt;Resource Language="<span style="background-color: yellow">en-US</span>" /&gt;
  &lt;/Resources&gt;
    &lt;Applications&gt;
    &lt;Application Id="<span style="background-color: yellow">ContosoDemo</span>" Executable="<span style="background-color: yellow">ContosoDemo.exe</span>" 
                 EntryPoint="Windows.FullTrustApplication"&gt;
    &lt;uap:VisualElements DisplayName="<span style="background-color: yellow">Contoso Demo</span>" BackgroundColor="#777777" 
                        Square150x150Logo="Assets\Square150x150Logo.png" 
                        Square44x44Logo="Assets\Square44x44Logo.png" 
        Description="<span style="background-color: yellow">Contoso Demo</span>"&gt;
      &lt;/uap:VisualElements&gt;
    &lt;/Application&gt;
  &lt;/Applications&gt;
    &lt;Capabilities&gt;
    &lt;rescap:Capability Name="runFullTrust" /&gt;
  &lt;/Capabilities&gt;
&lt;/Package&gt;
</pre>
</blockquote>

有关 `AppXManifest.xml` 文件和程序包布局的详细信息，请参阅 [MSDN 中的**应用程序包清单**主题](https://msdn.microsoft.com/en-us/library/windows/apps/br211474.aspx)。

最后，如果你使用 Visual Studio 创建新项目并横向迁移现有代码，请参阅 [MSDN 文档了解如何生成新 UWP 项目](https://msdn.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)。 你可以将现有代码包含到新项目中，但你可能必须进行大范围的代码更改（尤其是在用户界面）以便作为“纯”UWP 运行。 这些更改不是本文档讨论的范围。

---

## <a name="phase-1-localize-the-application-manifest"></a>第 1 阶段：本地化应用程序清单

### <a name="step-11-update-strings--assets-in-the-appxmanifest"></a>步骤 1.1:更新字符串和 AppXManifest 中的资产

在阶段 0 中，你为应用程序创建了一个基本 `AppXManifest.xml` 文件（基于提供给转换器、从 MSI 提取或手动输入到清单的值），但它既不包含本地化的信息，也不会支持其他功能，如高分辨率“开始”磁贴资源等。 

若要确保你的应用程序的名称和描述能够正确本地化，必须在一组资源文件中定义一些资源，并更新 AppX 清单来引用它们。

**创建默认的资源文件**

第一步是使用你的默认语言（例如，美国英语）创建默认资源文件。 你可以使用文本编辑器手动执行此操作，或通过 Visual Studio 中的资源设计器进行操作。

如果你想要手动创建资源：

0. 创建名为 `resources.resw` 的 XML 文件，并将其放在你的项目的 `Strings\en-us` 子文件夹中。 
 * 如果你的默认语言不是美国英语，应使用合适的 BCP-47 代码 
0. 在 XML 文件中，添加以下内容，其中<span style="background-color: yellow">突出显示文本</span>替换为你的应用的相应文本（使用你的默认语言）。

[!Note] 有一些这些字符串的长度限制。 有关详细信息，请参阅 [VisualElements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements?branch=live)。

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;root&gt;
  &lt;data name="ApplicationDescription"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo app with localized resources (English)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="ApplicationDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Sample (English)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PackageDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Package (English)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PublisherDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Samples, USA</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="TileShortName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso (EN)</span>&lt;/value&gt;
  &lt;/data&gt;
&lt;/root&gt;
</pre>
</blockquote>

如果你想要使用 Visual Studio 中的设计器：

0. 创建`Strings\en-us`在你的项目文件夹 （或根据需要其他语言），并添加**新项**到你的项目的根文件夹，使用的默认名称 `resources.resw`
 * 请务必选择**资源文件 (.resw)** 而非**资源字典** -“资源字典”是 XAML 应用程序使用的文件
0. 使用设计器，输入以下字符串（使用同一个 `Names`，但将 `Values` 替换为你的应用程序的相应文本）：

<img src="images\editing-resources-resw.png"/>

注意：如果你从 Visual Studio 设计器，你始终可以通过按 `F7` 直接编辑 XML。 但是，如果你从最小的 XML 文件，*设计器将不识别该文件*，因为它缺少大量其他元数据；你可以通过将样本 XSD 信息从设计器生成的文件复制到你手动编辑的 XML 来解决此问题。 

**更新清单以引用资源**

在 `.resw` 文件中定义值后，下一步是更新清单以引用资源字符串。 同样，你可以直接编辑 XML 文件，或依靠 Visual Studio 清单设计器。

如果你直接编辑 XML，打开 `AppxManifest.xml` 文件，对<span style="background-color: lightgreen">突出显示值</span>进行以下更改 - 使用此*确切*文本，而不是特定于应用程序的文本。 对于使用这些具体的资源名称没有要求（你可以选择自己的名称），但不论你如何选择，所选名称都必须与 `.resw` 文件中的名称完全一致。 这些名称应与你在 `.resw` 文件中创建的 `Names` 一致，带有前缀 `ms-resource:` 架构和 `Resources/` 命名空间。 

*注意： 许多的清单元素中省略了此代码段-不删除任何内容 ！*

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;Package&gt;
  &lt;Properties&gt;
    &lt;DisplayName&gt;<span style="background-color: lightgreen">ms-resource:Resources/PackageDisplayName</span>&lt;/DisplayName&gt;
    &lt;PublisherDisplayName&gt;<span style="background-color: lightgreen">ms-resource:Resources/PublisherDisplayName</span>&lt;/PublisherDisplayName&gt;
  &lt;/Properties&gt;
  &lt;Applications&gt;
    &lt;Application&gt;
      &lt;uap:VisualElements DisplayName="<span style="background-color: lightgreen">ms-resource:Resources/ApplicationDisplayName</span>"
        Description="<span style="background-color: lightgreen">ms-resource:Resources/ApplicationDescription</span>"&gt;
        &lt;uap:DefaultTile ShortName="<span style="background-color: lightgreen">ms-resource:Resources/TileShortName</span>"&gt;
          &lt;uap:ShowNameOnTiles&gt;
            &lt;uap:ShowOn Tile="square150x150Logo" /&gt;
          &lt;/uap:ShowNameOnTiles&gt;
        &lt;/uap:DefaultTile&gt;
      &lt;/uap:VisualElements&gt;
    &lt;/Application&gt;
  &lt;/Applications&gt;
&lt;/Package&gt;
</pre>
</blockquote>

如果你使用 Visual Studio 清单设计器，打开 `Package.appxmanifest` 文件，更改 `Application` 选项卡和 `Packaging` 选项卡中的<span style="background-color: lightgreen">突出显示值</span>：

<img src="images\editing-application-info.png"/>
<img src="images\editing-packaging-info.png"/>

### <a name="step-12-build-pri-file-make-an-appx-package-and-verify-its-working"></a>步骤 1.2:生成 PRI 文件，使一个 AppX 包，并验证它是否正常工作

你现在应该能够生成 `.pri` 文件，并部署应用程序，确认是否在“开始”菜单中显示正确的信息（使用你的默认语言）。 

如果你在 Visual Studio 中生成，只需按 `Ctrl+Shift+B` 来生成项目，然后右键单击项目并从 `Deploy` 上下文菜单中选择。 

如果你手动生成，请按照以下步骤创建 `MakePRI` 工具的配置文件，并生成 `.pri` 文件本身（可以在 [MSDN 中的**手动应用打包**主题](https://docs.microsoft.com/en-us/windows/uwp/packaging/manual-packaging-root)中找到更多信息）：

0. 从“开始”菜单中的 `Visual Studio 2015` 文件夹打开开发人员命令提示符
0. 切换到项目根目录（包含 `AppxManifest.xml` 文件和 `Strings` 文件夹的目录）
0. 键入以下命令，将“contoso_demo.xml”替换为适合你的项目的名称，并将“en-US”替换为你的应用的默认语言（或如果适用，保留为 en-US）。 请注意，xml 文件在父目录中创建（**不是**在项目目录中），因为它不是应用程序的一部分（你可以选择你需要的任何其他目录，但请务必在将来的命令中替换它）。

```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
```

0. 你可以键入 `makepri createconfig /?` 查看每个参数的作用，但概括起来：
 * `/cf` 设置配置文件名 （此命令的输出）
 * `/dq` 设置在此情况下的默认限定符的语言 `en-US`
 * `/pv` 在此事例的 Windows 10 设置平台版本中，
 * `/o` 将其覆盖输出文件，如果存在设置
0. 现在你有了配置文件，再次运行 `MakePRI` 以实际上搜索磁盘查找资源，并将它们打包为一个 PRI 文件。 将“Contoso_demop.xml”替换为你在上一个步骤中使用的 XML 文件名，请务必指定输入和输出的父目录： 

    `makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o`
0. 你可以键入 `makepri new /?` 查看每个参数的作用，但概括起来：
 * `/pr` 设置项目根目录 （在本例中为当前目录）
 * `/cf` 设置配置文件名，在上一步中创建
 * `/of` 设置输出文件 
 * `/mf` （因此，我们可以排除在更高版本的步骤中的包中的文件） 创建映射文件
 * `/o` 将其覆盖输出文件，如果存在设置
0. 现在你有一个拥有默认语言资源（例如，en-US）的 `.pri` 文件。 若要验证能否正常使用，可以运行以下命令：

    `makepri dump /if ..\resources.pri /of ..\resources /o`
0. 你可以键入 `makepri dump /?` 查看每个参数的作用，但概括起来：
 * `/if` 设置输入文件名 
 * `/of` 设置输出文件名 (`.xml`将会自动追加)
 * `/o` 将其覆盖输出文件，如果存在设置
0. 最后，你可以在文本编辑器中打开 `..\resources.xml`，确认其中列出了你的 `<NamedResource>` 值（如 `ApplicationDescription` 和 `PublisherDisplayName`），以及你选择的默认语言的 `<Candidate>` 值（文件开头将为其他内容；暂时忽略）。

如果你愿意，你可以打开映射文件 `..\resources.map.txt` 以确认其中含有项目所需的文件（包括 PRI 文件，它不是项目目录的一部分）。 重要的是，映射文件将*不*包括对你的 `resources.resw` 文件的引用，因为该文件的内容已嵌入到 PRI 文件中。 但它将包含其他资源，如你的映像的文件名。

**生成和对程序包进行签名**

现在 PRI 文件已生成，你可以生成程序包，并进行签名：

0. 若要创建应用包，运行以下命令，将 `contoso_demo.appx` 替换为你要创建的 AppX 文件的名称，并确保为 `.AppX` 文件选择不同目录（本示例使用父目录；它可以在任何位置，但**不**应该是项目目录）：

    `makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o`
0. 你可以键入 `makeappx pack /?` 查看每个参数的作用，但概括起来：
 * `/m` 设置要使用的清单文件
 * `/f` 设置映射要使用的文件 （在上一步中创建） 
 * `/p` 将输出设置包名称
 * `/o` 将其覆盖输出文件，如果存在设置
0. 创建程序包后，必须对它进行签名。 若要获取签名证书的最简单方法是通过在 Visual Studio 中创建一个空的通用 Windows 项目，并将复制`.pfx`它创建的文件，但你可以创建一个使用手动`MakeCert`和`Pvk2Pfx`中所述的实用程序<a href="https://msdn.microsoft.com/en-us/library/windows/desktop/jj835832.aspx">如何创建应用程序包签名证书</a>，MSDN 上的主题。 
 * **重要说明：** 如果手动创建一个签名证书，请确保你在不同的目录将文件放比源项目或你的包源，否则为可能会收到包含包，包括私钥 ！
0. 若要对包签名，请使用以下命令。 请注意，`AppxManifest.xml` 的 `Identity` 元素中指定的 `Publisher` 必须与证书的 `Subject` 匹配（这**不**是 `<PublisherDisplayName>` 元素，是向用户显示的本地化显示名称）。 像往常一样，将 `contoso_demo...` 文件名替换为适合你的项目的名称，并（**非常重要**）确保 `.pfx` 文件不在当前目录中（否则它可能被作为你的程序包的一部分创建，包括签名私钥！）：

    `signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appx`
0. 你可以键入 `signtool sign /?` 查看每个参数的作用，但概括起来：
 * `/fd` 设置文件摘要算法 （SHA256 为 AppX 的默认值）
 * `/a` 将自动选择最佳的证书
 * `/f` 指定包含签名证书的输入的文件

最后，你现在可以双击 `.appx` 文件以安装它，或如果你更喜欢命令行，你可以打开 PowerShell 提示，更改为包含程序包的目录，然后键入以下内容（将 `contoso_demo.appx` 替换为你的程序包名称）：

```CMD
    add-appxpackage contoso_demo.appx
```

如果你收到有关证书不受信任的错误，请确保将它添加到计算机存储（**不**是用户存储）。 若要将证书添加到计算机存储，可以使用命令行或 Windows 资源管理器。

如何使用命令行：

0. 以管理员身份运行 Visual Studio 2015 命令提示
0. 切换到包含 `.cer` 文件的目录（切记确保在你的源目录或项目目录之外！）
0. 键入以下命令，使用你的文件命替换 `contoso_demo.cer`：

    `certutil -addstore TrustedPeople contoso_demo.cer`
0. 你可以运行 `certutil -addstore /?` 来查看每个参数的作用，但概括起来：
 * `-addstore` 将证书添加到证书存储
 * `TrustedPeople` 指示在其中放置证书存储区

如何 Windows 资源管理器：

0. 导航到包含 `.pfx` 文件的文件夹
0. 双击 `.pfx` 文件，**证书导入向导**应该会显示
0. 选择`Local Machine`单击 `Next`
0. 接受用户帐户控制管理员的提升提示，如果它出现，然后单击 `Next`
0. 如果有的话，私钥，输入密码，然后单击 `Next`
0. 选择 `Place all certificates in the following store`
0. 单击 `Browse`，然后选择 `Trusted People` 文件夹（**不是**”受信任的发布者“）
0. 单击`Next`，然后 `Finish`

将证书添加到 `Trusted People` 存储后，尝试再次安装程序包。

现在，你应该看到你的应用显示在”开始“菜单的”所有应用“列表中，并具有来自 `.resw` / `.pri` 文件的正确信息。 如果你看到空白字符串或字符串 `ms-resource:...`，表示出现了错误 - 仔细检查你的编辑，确保它们正确无误。 如果右键单击”开始“菜单中的应用，你可以将它固定为磁贴，并确认同时显示正确的信息。

### <a name="step-13-add-more-supported-languages"></a>步骤 1.3:添加更多受支持的语言

对 AppX 清单进行更改并且 `resources.resw` 文件已创建后，添加其他语言很简单。

**创建其他的本地化的资源**

首先，创建其他本地化的资源值。 

在 `Strings` 文件夹内，使用相应的 BCP-47 代码为你支持的每种语言创建其他文件夹（例如，`Strings\de-DE`）。 在每个文件夹内，创建包括翻译的资源值的 `resources.resw` 文件（使用 XML 编辑器或 Visual Studio 设计器）。 假设你在某处已经有可用的本地化的字符串，你只需将其插入到 `.resw` 文件；本文档不包括翻译步骤本身。 

例如，`Strings\de-DE\resources.resw` 文件可能如下所示，包含从 `en-US` 翻译过来的<span style="background-color: yellow">突出显示文本</span>：

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;root&gt;
  &lt;data name="ApplicationDescription"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo app with localized resources (German)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="ApplicationDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Sample (German)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PackageDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Package (German)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PublisherDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Samples, DE</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="TileShortName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso (DE)</span>&lt;/value&gt;
  &lt;/data&gt;
&lt;/root&gt;
</pre>
</blockquote>

下面的步骤假定你为 `de-DE` 和 `fr-FR` 添加了资源，不过任何语言都可以按照相同方式操作。

**AppX 清单更新为列出支持的语言更新**

AppX 清单必须更新，以列出应用支持的语言。 Desktop App Converter 添加默认语言，但必须明确添加其他语言。 如果直接编辑 `AppxManifest.xml` 文件，按照如下方法更新 `Resources` 节点，根据需要多添加一些元素，替换<span style="background-color: yellow">你支持的适当语言</span>，并确保列表中的第一个条目为默认（回退）语言。 在此示例中，默认语言为英语（美国），另外还支持德语（德国）和法语（法国）：

<blockquote>
<pre>
&lt;Resources&gt;
  &lt;Resource Language="<span style="background-color: yellow">EN-US</span>" /&gt;
  &lt;Resource Language="<span style="background-color: yellow">DE-DE</span>" /&gt;
  &lt;Resource Language="<span style="background-color: yellow">FR-FR</span>" /&gt;
&lt;/Resources&gt;
</pre>
</blockquote>

如果你使用的 Visual Studio，你应该不需要执行任何操作；如果你看一下 `Package.appxmanifest`，你应该看到特殊的 <span style="background-color: yellow">x-generate</span> 值，这会让生成过程将找到的语言插入到你的项目中（基于使用 BCP-47 代码命名的文件夹）。 请注意，这不是真正的 Appx 清单的有效值；仅适用于 Visual Studio 项目：

<blockquote>
<pre>
&lt;Resources&gt;
  &lt;Resource Language="<span style="background-color: yellow">x-generate</span>" /&gt;
&lt;/Resources&gt;
</pre>
</blockquote>

**重新生成的本地化值**

现在你可以生成和部署应用程序，同样，如果你更改 Windows 中的语言首选项，你应该看到重新本地化的值显示在“开始”菜单中（下面是有关如何更改语言的说明）。

对于 Visual studio，同样，你使用 `Ctrl+Shift+B` 即可生成，然后右键单击项目进行 `Deploy`。

如果你手动生成项目，请按照上方的相同步骤，但在创建配置文件时将其他语言（使用下划线分开）添加到限定符列表 (`/dq`)。 例如，若要支持在上一步中添加的英语、德语和法语资源：

```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_fr-FR /pv 10.0 /o
```

这将创建包含你可以轻松用来测试的所有指定语言的 PRI 文件。 如果资源的总大小较小，或仅支持少数语言，这可能适用于应用发货；仅当你需要利用最小化资源（你执行生成单独语言包的其他工作所需的资源）的安装/下载大小的好处时。

**使用本地化的值进行测试**

若要测试新的本地化的更改，你只需将新的 UI 首选语言添加到 Windows。 无需下载语言包、重新启动系统，或让整个 Windows UI 使用外语显示。 

0. 运行 `Settings` 应用 (`Windows + I`)
0. 转到 `Time & language`
0. 转到 `Region & language`
0. 单击 `Add a language`
0. 键入（或者选择）所需的语言（例如 `Deutsch` 或 `German`）
 * 如果有子语言，选择所需的那个（例如，`Deutsch / Deutschland`）
0. 在语言列表中选择新的语言
0. 单击 `Set as default`

现在，打开“开始”菜单并搜索应用程序，你应该可以看到所选语言的本地化的值（其他应用也可能显示为本地化值）。 如果你未看到本地化的名称，请立即，请等待几分钟，直到刷新开始菜单的缓存。 若要返回到你的本地语言，只需在语言列表中将其设置为默认语言。 

### <a name="step-14-localizing-more-parts-of-the-appx-manifest-optional"></a>步骤 1.4:本地化 AppX 清单 （可选） 的多个的部件

可以本地化 AppX 清单的其他部分。 例如，如果你的应用程序处理文件扩展名，那么它在清单中应有 `windows.fileTypeAssociation` 扩展名，使用与显示的文本完全相同的<span style="background-color: lightgreen">绿色突出显示文本</span>（因为它将参考资源），将<span style="background-color: yellow">黄色突出显示文本</span>替换为特定于你的应用程序的信息：

<blockquote>
<pre>
&lt;Extensions&gt;
  &lt;uap:Extension Category="windows.fileTypeAssociation"&gt;
    &lt;uap:FileTypeAssociation Name="default"&gt;
      &lt;uap:DisplayName&gt;<span style="background-color: lightgreen">ms-resource:Resources/FileTypeDisplayName</span>&lt;/uap:DisplayName&gt;
      &lt;uap:Logo&gt;<span style="background-color: yellow">Assets\StoreLogo.png</span>&lt;/uap:Logo&gt;
      &lt;uap:InfoTip&gt;<span style="background-color: lightgreen">ms-resource:Resources/FileTypeInfoTip</span>&lt;/uap:InfoTip&gt;
      &lt;uap:SupportedFileTypes&gt;
        &lt;uap:FileType ContentType="<span style="background-color: yellow">application/x-contoso</span>"&gt;<span style="background-color: yellow">.contoso</span>&lt;/uap:FileType&gt;
      &lt;/uap:SupportedFileTypes&gt;
    &lt;/uap:FileTypeAssociation&gt;
  &lt;/uap:Extension&gt;
&lt;/Extensions&gt;
</pre>
</blockquote>

你还可以使用 Visual Studio 清单设计器添加此信息，使用 `Declarations` 选项卡，并记录<span style="background-color: lightgreen">突出显示值</span>：

<p><img src="images\editing-declarations-info.png"/></p>

现在将相应的资源名称添加到你的各个 `.resw` 文件中，并将<span style="background-color: yellow">突出显示文本</span>替换为适合你的应用的文本（请记住为*每个受支持语言*执行此操作！）：

<blockquote>
<pre>
... existing content...

&lt;data name="FileTypeDisplayName"&gt;
  &lt;value&gt;<span style="background-color: yellow">Contoso Demo File</span>&lt;/value&gt;
&lt;/data&gt;
&lt;data name="FileTypeInfoTip"&gt;
  &lt;value&gt;<span style="background-color: yellow">Files used by Contoso Demo App</span>&lt;/value&gt;
&lt;/data&gt;
</pre>
</blockquote>

这随后将显示在 Windows shell 的多个部分中，如文件资源管理器：

<p><img src="images\file-type-tool-tip.png"/></p>

与之前一样生成并测试程序包，使用应显示新 UI 字符串的所有新方案。

- - -

## <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>阶段 2:使用 MRT 确定并找到资源

上一部分介绍了如何使用 MRT 本地化应用的清单文件，以使 Windows Shell 可以正确显示应用的名称和其他元数据。 此任务不需要更改代码；它只需要使用 `.resw` 文件和其他一些工具。 此部分将介绍如何使用 MRT 找到你现有资源格式的资源，以及如何在最小改动的情况下使用现有的资源处理代码。

### <a name="assumptions-about-existing-file-layout--application-code"></a>有关现有文件布局和应用程序代码的假设

因为有多种方法可以本地化 Win32 桌面应用，所以本文作出一些有关你将需要映射到特定环境的现有应用程序结构的一些简化假设。 你可能需要对你现有的基本代码或资源布局进行一些更改，使其符合 MRT 的要求，这些内容大部分超出了本文档的范围。

**资源文件布局**

此白皮书假设你的本地化的资源均具有相同的文件名（例如，`contoso_demo.exe.mui` 或 `contoso_strings.dll` 或 `contoso.strings.xml`），但放在具有 BCP-47 名称（`en-US`、`de-DE` 等）的不同文件夹内。 它有多少个资源文件，则其名称是，哪些其文件格式并不重要 / 关联 Api，等等。重要的是，每个唯一*逻辑*资源具有相同文件名 (但位于不同*物理*目录)。 

作为一个反例，如果你的应用程序使用平面文件结构（具有包含文件 `english_strings.dll` 和 `french_strings.dll` 的单个 `Resources` 目录），它不会很好地映射到 MRT。 更好的结构是 `Resources` 目录，有子目录和文件 `en\strings.dll` 和 `fr\strings.dll`。 也可以使用相同的基本文件名，但具有嵌入限定符，如 `strings.lang-en.dll` 和 `strings.lang-fr.dll`，不过使用具有语言代码的目录在概念上更简单，所以我们将重点关注这一点。

**注意**即使你不能遵循此文件命名约定，也仍然有可能可以使用 MRT 和 AppX 打包的好处；只是需要更多工作。

例如，应用程序可能在名为 <span style="background-color: yellow">ui.txt</span> 的简单文本文件中有一组自定义的 UI 命令（用于按钮标签等），放在 <span style="background-color: yellow">UICommands</span> 文件夹下：

<blockquote>
<pre>
+ ProjectRoot
|--+ Strings
|  |--+ en-US
|  |  \--- resources.resw
|  \--+ de-DE
|     \--- resources.resw
|--+ <span style="background-color: yellow">UICommands</span>
|  |--+ en-US
|  |  \--- <span style="background-color: yellow">ui.txt</span>
|  \--+ de-DE
|     \--- <span style="background-color: yellow">ui.txt</span>
|--- AppxManifest.xml
|--- ...rest of project...
</pre>
</blockquote>

**资源加载代码**

此白皮书假定在某一时刻，在你的代码中，你想要查找包含本地化的资源的文件，加载它，然后使用它。 用于加载资源的 API，用来解提取资源的 API，等等，都不重要。 在伪代码中，基本上有三个步骤：

```
set userLanguage = GetUsersPreferredLanguage()
set resourceFile = FindResourceFileForLanguage(MY_RESOURCE_NAME, userLanguage)
set resource = LoadResource(resourceFile) 
    
// now use 'resource' however you want
```

MRT 只需要更改此流程的前两个步骤 - 如何确定最佳候选资源和如何找到它们。 它不需要你更改加载或使用资源的方式（尽管在你想要利用它们时，它会提供所需的工具）。
 
例如，应用程序可能会使用 Win32 API `GetUserPreferredUILanguages`、CRT 函数 `sprintf` 和 Win32 API `CreateFile` 来替换上述三个伪代码函数，然后手动分析查找 `name=value` 对的文本文件。 （细节并不重要；这只是为了说明 MRT 对找到资源后对用于处理资源的技巧没有影响）。

### <a name="step-21-code-changes-to-use-mrt-to-locate-files"></a>步骤 2.1:若要使用 MRT 来查找文件的代码更改

将代码切换为使用 MRT 查找资源并不困难。 这需要使用一些 WinRT 类型和几行代码。 你将使用的主要类型如下所示：

* [ResourceContext](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext)，封装当前处于活动状态的一组限定符值（语言、比例系数等）
* [ResourceManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemanager)（WinRT 版本，而不是 .NET 版本），支持访问来自 PRI 文件的所有资源
* [ResourceMap](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemap)，表示 PRI 文件中的一组特定资源子集（在本示例中，为基于文件的资源与字符串资源）
* [NamedResource](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource)，表示逻辑资源及其所有可能的候选项
* [ResourceCandidate](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcecandidate)，表示一个具体的候选资源 

在伪代码中，你解决给定资源文件名（如上方示例中的 `UICommands\ui.txt`）的方式如下所示：

```
// Get the ResourceContext that applies to this app
set resourceContext = ResourceContext.GetForViewIndependentUse()
    
// Get the current ResourceManager (there's one per app)
set resourceManager = ResourceManager.Current
    
// Get the "Files" ResourceMap from the ResourceManager
set fileResources = resourceManager.MainResourceMap.GetSubtree("Files")
    
// Find the NamedResource with the logical filename we're looking for,
// by indexing into the ResourceMap
set desiredResource = fileResources["UICommands\ui.txt"]
    
// Get the ResourceCandidate that best matches our ResourceContext
set bestCandidate = desiredResource.Resolve(resourceContext)
   
// Get the string value (the filename) from the ResourceCandidate
set absoluteFileName = bestCandidate.ValueAsString
```

尤其要注意的是，此代码**不**请求特定的语言文件夹 - 如 `UICommands\en-US\ui.txt` - 即使这指示了文件在磁盘中的位置。 相反，它要求提供*逻辑*文件名 `UICommands\ui.txt`，并依靠 MRT 在其中一个语言目录中查找相应的磁盘上文件。

在这里，示例应用可以和以前一样继续使用 `CreateFile` 加载 `absoluteFileName`，并分析 `name=value` 对；不需要在应用中对该逻辑进行更改。 如果你在 C# 或 C++/CX 中编写，实际代码不会比此代码复杂多少（实际上，许多中间变量可以删去）- 请参见下方**加载 .NET 资源**的内容。 由于用于激活和调用 WinRT API 的基于 COM 的低级别 API，基于 C++/WRL 的应用程序将更加复杂，但所采取的基本步骤是相同的 - 请参见下方**加载 Win32 MUI 资源**的内容。

**正在加载.NET 资源**

因为 .NET 具有查找和加载资源的内置机制（称为“卫星集”），因此没有上方人为示例中要替换的明确代码 - 在 .NET 中，只需在相应的目录中有资源 DLL，系统将自动为你定位。 当应用使用资源包打包为 AppX 时，目录结构会稍有不同 - 不会将资源目录作为应用程序主目录的子目录，它们是对等的（或者如果用户没有在首选项中列出语言，它们根本不会显示）。 

例如，假设 .NET 应用程序具有以下布局，其中所有文件均位于 `MainApp` 文件夹下：

<blockquote>
<pre>
+ MainApp
|--+ en-us
|  \--- MainApp.resources.dll
|--+ de-de
|  \--- MainApp.resources.dll
|--+ fr-fr
|  \--- MainApp.resources.dll
\--- MainApp.exe
</pre>
</blockquote>

在转换为 AppX 后，布局外观如下所示，假设 `en-US` 是默认语言，并且用户语言列表中同时列出了德语和法语：

<blockquote>
<pre>
+ WindowsAppsRoot
|--+ MainApp_neutral
|  |--+ en-us
|  |  \--- <span style="background-color: yellow">MainApp.resources.dll</span>
|  \--- MainApp.exe
|--+ MainApp_neutral_resources.language_de
|  \--+ de-de
|     \--- <span style="background-color: yellow">MainApp.resources.dll</span>
\--+ MainApp_neutral_resources.language_fr
   \--+ fr-fr
      \--- <span style="background-color: yellow">MainApp.resources.dll</span>
</pre>
</blockquote>

由于本地化的资源不再位于可执行文件安装主位置下的子目录中，所以内置 .NET 资源解决失败。 所幸，.NET 在处理失败的程序集加载尝试方面具有明确定义的机制 - `AssemblyResolve`事件。 使用 MRT 的 .NET 应用必须注册此事件，并提供 .NET 资源子系统缺少的程序集。 

如何使用 WinRT API 查找 .NET 使用的卫星集的简明示例如下所示；所显示的代码被有意压缩以显示最基本的实现，虽然你可以看到它紧密映射到上方的伪代码，其中使用传入的 `ResolveEventArgs` 提供我们需要查找的程序集的名称。 此代码的可运行版本（包含详细注释和错误处理）可以在 [GitHub 中的 **.NET 程序集解析器**示例](https://aka.ms/fvgqt4)的 `PriResourceRsolver.cs` 文件中找到。

```C#
static class PriResourceResolver
{
  internal static Assembly ResolveResourceDll(object sender, ResolveEventArgs args)
  {
    var fullAssemblyName = new AssemblyName(args.Name);
    var fileName = string.Format(@"{0}.dll", fullAssemblyName.Name);

    var resourceContext = ResourceContext.GetForViewIndependentUse();
    resourceContext.Languages = new[] { fullAssemblyName.CultureName };

    var resource = ResourceManager.Current.MainResourceMap.GetSubtree("Files")[fileName];

    // Note use of 'UnsafeLoadFrom' - this is required for apps installed with AppX, but
    // in general is discouraged. The full sample provides a safer wrapper of this method
    return Assembly.UnsafeLoadFrom(resource.Resolve(resourceContext).ValueAsString);
  }
}
```

鉴于上述类，你会在早期在应用程序启动代码中添加以下内容（在需要加载所有本地化资源之前）：

```C#
void EnableMrtResourceLookup()
{
  AppDomain.CurrentDomain.AssemblyResolve += PriResourceResolver.ResolveResourceDll;
}
```

每当 .NET 运行时无法找到资源 DLL 时，它都将引发 `AssemblyResolve` 事件，此时，所提供的事件处理程序将通过 MRT 找到所需文件，并返回程序集。

**注意**如果你的应用已有用于其他用途的 `AssemblyResolve` 处理程序，你需要将资源解决代码与现有代码结合。

**Win32 MUI 资源加载**

加载 Win32 MUI 资源本质上与加载 .NET 卫星集相同，但使用 C++/CX 或 C++/WRL 代码。 使用 C++/CX 允许使用与上方的 C# 代码非常接近但简单得多的代码，不过它使用 C++ 语言扩展、编译器开关，且会产生你可能想要避免的其他运行时开销。 如果是这种情况，使用 C++/WRL 提供影响更小的解决方案，代价是使用更冗长的代码。 不过，如果你熟悉 ATL 编程（或一般是 COM），那么 WRL 应该感觉熟悉。 

下面的示例函数展示了如何使用 C++/WRL 加载特定资源 DLL，并返回可用于使用常用的 Win32 资源 API 加载其他资源的 `HINSTANCE`。 请注意，与使用 .NET 运行时请求的语言明确初始化 `ResourceContext` 的 C# 示例不同，此代码依赖用户的当前语言。

```CPP
#include <roapi.h>
#include <wrl\client.h>
#include <wrl\wrappers\corewrappers.h>
#include <Windows.ApplicationModel.resources.core.h>
#include <Windows.Foundation.h>
   
#define IF_FAIL_RETURN(hr) if (FAILED((hr))) return hr;
    
HRESULT GetMrtResourceHandle(LPCWSTR resourceFilePath,  HINSTANCE* resourceHandle)
{
  using namespace Microsoft::WRL;
  using namespace Microsoft::WRL::Wrappers;
  using namespace ABI::Windows::ApplicationModel::Resources::Core;
  using namespace ABI::Windows::Foundation;
    
  *resourceHandle = nullptr;
  HRESULT hr{ S_OK };
  RoInitializeWrapper roInit{ RO_INIT_SINGLETHREADED };
  IF_FAIL_RETURN(roInit);
    
  // Get Windows.ApplicationModel.Resources.Core.ResourceManager statics
  ComPtr<IResourceManagerStatics> resourceManagerStatics;
  IF_FAIL_RETURN(GetActivationFactory(
    HStringReference(
    RuntimeClass_Windows_ApplicationModel_Resources_Core_ResourceManager).Get(),
    &resourceManagerStatics));
    
  // Get .Current property
  ComPtr<IResourceManager> resourceManager;
  IF_FAIL_RETURN(resourceManagerStatics->get_Current(&resourceManager));
    
  // get .MainResourceMap property
  ComPtr<IResourceMap> resourceMap;
  IF_FAIL_RETURN(resourceManager->get_MainResourceMap(&resourceMap));
    
  // Call .GetValue with supplied filename
  ComPtr<IResourceCandidate> resourceCandidate;
  IF_FAIL_RETURN(resourceMap->GetValue(HStringReference(resourceFilePath).Get(),
    &resourceCandidate));
    
  // Get .ValueAsString property
  HString resolvedResourceFilePath;
  IF_FAIL_RETURN(resourceCandidate->get_ValueAsString(
    resolvedResourceFilePath.GetAddressOf()));
    
  // Finally, load the DLL and return the hInst.
  *resourceHandle = LoadLibraryEx(resolvedResourceFilePath.GetRawBuffer(nullptr),
    nullptr, LOAD_LIBRARY_AS_DATAFILE | LOAD_LIBRARY_AS_IMAGE_RESOURCE);
    
  return S_OK;
}
```

## <a name="phase-3-building-resource-packs"></a>阶段 3:构建资源包

既然你有了一个包含所有资源的“大满包”，有两条途径可以分别生成主包和资源包，以最小化下载和安装大小：

0. 获取现有的大满包，并通过[捆绑包生成器工具](https://aka.ms/bundlegen)运行，来自动创建资源包。 如果你拥有已生成大满包的生成系统，并且你想要后处理它来生成资源包，这是首选方法。
0. 直接生成单个资源包，并将它们生成为一个捆绑包。 如果你对生成系统有更多控制，并且可以直接生成包，这是首选方法。

### <a name="step-31-creating-the-bundle"></a>步骤 3.1:创建捆绑

**使用绑定生成器工具**

要使用捆绑包生成器工具，为包创建的 PRI 配置文件需要手动更新，以删除 `<packaging>` 部分。

如果你使用 Visual Studio，请参阅 [MSDN 中的**确保已安装资源…** 主题](https://msdn.microsoft.com/en-us/library/dn482043.aspx)，了解有关如何通过创建文件 `priconfig.packaging.xml` 和 `priconfig.default.xml` 将所有语言构建到主包中的信息。

如果你手动编辑文件，请按照下列步骤操作： 

0. 使用之前的方法创建配置文件，替换正确的路径、文件名和语言：

    `makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_es-MX /pv 10.0 /o`
0. 手动打开创建的 `.xml` 文件，并删除整个 `&lt;packaging&rt;` 部分（但保持所有内容完整无缺）：

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="UTF-8" standalone="yes" ?&gt; 
&lt;resources targetOsVersion="10.0.0" majorVersion="1"&gt;
  &lt;!-- Packaging section has been deleted... --&gt;
  &lt;index root="\" startIndexAt="\"&gt;
    &lt;default&gt;
    ...
    ...
</pre>
</blockquote>

0. 与以前一样生成 `.pri` 文件和 `.appx` 程序包，使用更新的配置文件和相应的目录和文件名（请参阅上方了解这些命令的详细信息）：

```CMD
makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
```

0. 创建程序包后，使用以下命令创建捆绑包，使用相应的目录和文件名：

    `BundleGenerator.exe -Package ..\contoso_demo.appx -Destination ..\bundle -BundleName contoso_demo`

现在，你可以进入到最后一步，即签名（见下方）。

**手动创建的资源包**

手动创建资源包需要运行一组略有不同的命令来生成单独的 `.pri` 和 `.appx` 文件 - 这些命令均与上方用于创建大满包的命令类似，因此只给出最基本的说明。 注意：所有命令都假设当前目录是该目录包含`AppXManifest.xml`文件，但所有文件放入 （您可以使用不同的目录，如果有必要，但您不应影响任何的项目目录的父目录这些文件）。 像往常一样，将“Contoso”文件名替换为你自己的文件名。

0. 使用以下命令创建**仅**将默认语言指定为默认限定符的配置文件 - 在此为 `en-US`：

    `makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o`
0. 使用以下命令为主程序包创建默认的 `.pri` 和 `.map.txt` 文件，并为在项目中找到的每种语言创建其他各个文件：

    `makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o`
0. 使用以下命令创建主程序包（其中包含可执行代码和默认语言资源）。 像往常一样，根据你的需求更改名称，尽管你应将程序包放在单独的目录中，以便以后可以更加轻松地创建捆绑包（本例使用 `..\bundle` 目录）：

    `makeappx pack /m .\AppXManifest.xml /f ..\resources.map.txt /p ..\bundle\contoso_demo.main.appx /o`
0. 创建主程序包后，为其他每种语言使用以下命令，每种语言使用一次（即对上一步中生成的每个语言映射文件重复此命令）。 同样，输出应位于单独的目录（与主程序包相同）。 请注意，语言应**同时**在 `/f` 选项和 `/p` 选项中指定，并使用新的 `/r` 参数（这表示需要资源包）：

    `makeappx pack /r /m .\AppXManifest.xml /f ..\resources.language-de.map.txt /p ..\bundle\contoso_demo.de.appx /o`
0. 将捆绑包目录中的所有程序包合并为单个 `.appxbundle` 文件。 新 `/d` 选项指定用于捆绑包中所有文件的目录（这就是为什么上一步中要将 `.appx` 文件放入一个单独的目录）：

    `makeappx bundle /d ..\bundle /p ..\contoso_demo.appxbundle /o`

生成程序包的最后一步是签名。

### <a name="step-32-signing-the-bundle"></a>步骤 3.2:对捆绑进行签名

创建了 `.appxbundle` 文件后（通过捆绑包生成器工具或手动创建），你将有一个包含主程序包以及所有资源包的单个文件。 最后一步是为此文件签名，以便 Windows 进行安装：

    signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appxbundle

这会生成一个签名的 `.appxbundle` 文件，包含主程序包以及所有语言特定的资源包。 你可以双击它（就像一个程序包文件）来安装应用，以及任何相应的语言（基于用户的 Windows 语言首选项）。

## <a name="related-topics"></a>相关主题

* [定制您的语言、 缩放、 高对比度和其他限定符的资源](tailor-resources-lang-scale-contrast.md)