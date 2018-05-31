---
author: stevewhims
Description: If your app doesn't have resources that match the particular settings of a customer device, then the app's default resources are used. This topic explains how to specify what those default resources are.
title: 指定应用使用的默认资源
template: detail.hbs
ms.author: stwhi
ms.date: 11/14/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 资源, 图像, 资产, MRT, 限定符
ms.localizationpriority: medium
ms.openlocfilehash: 6f88a6d6be6bf938564f0a31eac118983c256a7e
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
ms.locfileid: "1395986"
---
# <a name="specify-the-default-resources-that-your-app-uses"></a>指定应用使用的默认资源

如果应用不具有与客户设备的特定设置相匹配的资源，则使用应用的默认资源。 本主题介绍如何指定这些默认资源是什么。

当客户从 Microsoft Store 安装应用时，客户设备上的设置与应用的可用资源相匹配。 完成此匹配后，只需为该用户下载并安装适当的资源。 例如，使用最合适的用户语言首选项的字符串和图像，以及设备的分辨率和 DPI 设置。 例如，`scale` 的默认值是 `200`，但可根据自己的意愿使用其他值替代该默认值。

即使对于不进入自己资源包的资源（如为高对比度设置定制的图像），如果无法找到与用户设置匹配的资源，则可以指定应用在运行时应该使用的默认资源。 例如，`contrast` 的默认值是 `standard`，但可根据自己的意愿使用其他值替代该默认值。

这些默认值以默认资源限定符值的形式指定。 有关什么是资源限定符及其用法和目的的说明，请参阅[定制语言、比例、高对比度和其他限定符的资源](tailor-resources-lang-scale-contrast.md)。

可以采用两种方式中的其中一种来配置这些默认值。 可以将配置文件添加到项目，也可以直接编辑项目文件。 使用这些选项中最熟悉或最适用于生成系统的选项。

## <a name="option-1-use-priconfigdefaultxml-to-specify-default-qualifier-values"></a>选项 1 使用 priconfig.default.xml 指定限定符的默认值

1. 在 Visual Studio 中，将新项添加到项目。 选择 XML 文件，并将该文件命名为 `priconfig.default.xml`。
2. 在“解决方案资源管理器”中，选择 `priconfig.default.xml` 并检查“属性”窗口。 文件的“生成操作”应设置为 None，“复制到输出目录”应设置为“不要复制”。
3. 将文件的内容替换为此 XML。
   ```xml
   <default>
      <qualifier name="Language" value="LANGUAGE-TAG(S)" />
      <qualifier name="Contrast" value="standard" />
      <qualifier name="Scale" value="200" />
      <qualifier name="HomeRegion" value="001" />
      <qualifier name="TargetSize" value="256" />
      <qualifier name="LayoutDirection" value="LTR" />
      <qualifier name="DXFeatureLevel" value="DX9" />
      <qualifier name="Configuration" value="" />
      <qualifier name="AlternateForm" value="" />
   </default>
   ```
   
   **注意** 值 `LANGUAGE-TAG(S)` 需要与应用的默认语言保持同步。 如果该值是单一的 [BCP 47 语言标记](http://go.microsoft.com/fwlink/p/?linkid=227302)，则应用的默认语言必须是同一个标记。 如果它是一个以逗号分隔的语言标记列表，则应用的默认语言必须是列表中的第一个标记。 在应用包清单源文件 (`Package.appxmanifest`) 中的**应用程序**选项卡上，在**默认语言**字段中设置应用的默认语言。

4. 每个`<qualifier>`元素指示 Visual Studio 将何值用作每个限定符名称的默认值。 就目前拥有的文件内容来说，实际上尚未更改 Visual Studio 的行为。 换言之，Visual Studio *已表现得如同*该文件已存在这些内容，因为这些都是默认值。 因此若要使用自己的默认值替代默认值，需要更改文件中的值。 以下示例是编辑前三个值后该文件的外观。
   ```xml
   <default>
      <qualifier name="Language" value="de-DE" />
      <qualifier name="Contrast" value="black" />
      <qualifier name="Scale" value="400" />
      <qualifier name="HomeRegion" value="001" />
      <qualifier name="TargetSize" value="256" />
      <qualifier name="LayoutDirection" value="LTR" />
      <qualifier name="DXFeatureLevel" value="DX9" />
      <qualifier name="Configuration" value="" />
      <qualifier name="AlternateForm" value="" />
   </default>
   ```
5. 保存并关闭该文件，然后重新构建项目。

若要确认是否将已替代的默认值考虑在内，请查找文件 `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml` 并确认其内容是否与替代的内容相匹配。 如果匹配，则已成功地配置应用将默认使用的资源的限定符值。 如果未找到用户设置的匹配项，则将使用其文件夹或文件名包含在此处设置的默认限定符值的资源。

### <a name="how-does-this-work"></a>这是如何实现的？

在后台，Visual Studio 启动一个名为 `MakePri.exe` 的工具来生成一个称为包资源索引 (PRI) 的文件，用于描述所有应用的资源，包括指示哪些是默认资源。 有关此工具的详细信息，请参阅[使用 MakePri.exe 手动编译资源](compile-resources-manually-with-makepri.md)。 Visual Studio 将配置文件传递给 `MakePri.exe`。 `priconfig.default.xml` 文件的内容用作该配置文件的 `<default>` 元素，该配置文件是指定一组被认为是默认值的限定符值的部分。 因此，添加和编辑 `priconfig.default.xml` 最终会影响 Visual Studio 为应用生成的并包括在其应用包中的包资源索引文件的内容。

**注意** 每当更改 `<qualifier name="Language" ... />` 元素的值时，都需要将该更改与应用的默认语言同步。 这样，在应用的 PRI 文件中索引的语言资源便可与应用的清单默认语言匹配。 `<qualifier name="Language" ... />` 元素中的值替代清单中与 `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml` 的内容相关的值，但该文件和应用的清单应匹配。

### <a name="using-a-different-file-name-than-priconfigdefaultxml"></a>使用不同的文件名而不是 `priconfig.default.xml`

如果将文件命名为 `priconfig.default.xml`，则 Visual Studio 将自动识别并使用它。 如果为其提供不同的名称，需要让 Visual Studio 知道。 将此 XML 添加到项目文件中第一个 `<PropertyGroup>` 元素的开始和结束标记之间。

```xml
<AppxPriConfigXmlDefaultSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlDefaultSnippetPath>
```

将 `FILE-PATH-AND-NAME` 替换为文件的路径和名称。

## <a name="option-2-use-your-project-file-to-specify-default-qualifier-values"></a>选项 2 使用项目文件指定限定符的默认值

这是选项 1 的替代方法。 了解选项 1 的工作原理后，如果选项 2 更适合开发和/或构建工作流，则可以选择执行选项 2。

将此 XML 添加到项目文件中第一个 `<PropertyGroup>` 元素的开始和结束标记之间。

```xml
<AppxDefaultResourceQualifiers>Language=LANGUAGE-TAG(S)|Contrast=standard|Scale=200|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

以下示例是编辑前三个值后该文件可能的外观。

```xml
<AppxDefaultResourceQualifiers>Language=de-DE|Contrast=black|Scale=400|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

保存并关闭，然后重新生成项目。

**注意** 每当更改 `Language=` 值时，都需要在清单设计器中将该更改与应用的默认语言同步（通过打开 `Package.appxmanifest`）。

## <a name="related-topics"></a>相关主题

* [定制语言、比例、高对比度和其他限定符的资源](tailor-resources-lang-scale-contrast.md)
* [BCP-47 语言标记](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [使用 MakePri.exe 手动编译资源](compile-resources-manually-with-makepri.md)
