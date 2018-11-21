---
author: stevewhims
Description: In this scenario, we'll make a new app to represent our custom build system. We'll create a resource indexer and add strings and other kinds of resources to it. Then we'll generate and dump a PRI file.
title: 方案 1 从字符串资源和资产文件生成 PRI 文件
template: detail.hbs
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, 资源, 图像, 资产, MRT, 限定符
ms.localizationpriority: medium
ms.openlocfilehash: 7555f4a61f7798fa32d137928cde8c042a7fcdfc
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "7554718"
---
# <a name="scenario-1-generate-a-pri-file-from-string-resources-and-asset-files"></a>方案 1：从字符串资源和资产文件生成 PRI 文件
在此方案中，我们使用[包资源索引 (PRI) API](https://msdn.microsoft.com/library/windows/desktop/mt845690) 让新应用代表我们的自定义生成系统。 请记住，此自定义生成系统的目的是为目标 UWP 应用创建 PRI 文件。 因此，作为此演练的一部分，我们将创建一些示例资源文件（包含字符串和其他类型的资源）来代表该目标 UWP 应用的资源。

## <a name="new-project"></a>新建项目
首先在 Microsoft Visual Studio 中创建新项目。 创建 **Visual C++ Windows 控制台应用程序**项目，然后将其命名为 *CBSConsoleApp*（“自定义生成系统控制台应用”的简称）。

从**解决方案平台**下拉列表选择 *x64*。

## <a name="headers-static-library-and-dll"></a>标题、静态库和 dll
MrmResourceIndexer.h 标头文件（该文件安装到 `%ProgramFiles(x86)%\Windows Kits\10\Include\<WindowsTargetPlatformVersion>\um\`）中已声明 PRI API。 打开文件 `CBSConsoleApp.cpp` 并将标头与需要的其他标头一同包括在内。

```cppwinrt
#include <string>
#include <windows.h>
#include <MrmResourceIndexer.h>
```

API 是在 MrmSupport.dll 中实现的，可通过链接到静态库 MrmSupport.lib 进行访问。 打开项目的**属性**，单击**链接器** > **输入**，编辑 **AdditionalDependencies** 并添加 `MrmSupport.lib`。

生成解决方案，然后再从 `C:\Program Files (x86)\Windows Kits\10\bin\<WindowsTargetPlatformVersion>\x64\` 将 `MrmSupport.dll` 复制到生成输出文件夹（可能为 `C:\Users\%USERNAME%\source\repos\CBSConsoleApp\x64\Debug\`）。

将下面的 helper 函数添加到 `CBSConsoleApp.cpp`，因为我们需要它。

```cppwinrt
inline void ThrowIfFailed(HRESULT hr)
{
    if (FAILED(hr))
    {
        // Set a breakpoint on this line to catch Win32 API errors.
        throw new std::exception();
    }
}
```

在 `main()` 函数中，添加对初始化和取消初始化 COM 的调用。

```cppwinrt
int main()
{
    ::ThrowIfFailed(::CoInitializeEx(nullptr, COINIT_MULTITHREADED));
    
    // More code will go here.
    
    ::CoUninitialize();
}
```

## <a name="resource-files-belonging-to-the-target-uwp-app"></a>属于目标 UWP 应用的资源文件
现在我们需要一些示例资源文件（包含字符串和其他类型的资源）来代表目标 UWP 应用的资源。 当然，这些内容可以位于文件系统上的任意位置。 但是在本演练中，将它们置于 CBSConsoleApp 的项目文件夹中非常方便，所有内容均在同一位置。 只需将这些资源文件添加到文件系统；不要将其添加到 CBSConsoleApp 项目。

在包含 `CBSConsoleApp.vcxproj` 的同一文件夹内，添加一个名为 `UWPAppProjectRootFolder` 的新建子文件夹。 在该新建子文件夹内创建这些示例资源文件。

### <a name="uwpappprojectrootfoldersample-imagepng"></a>\UWPAppProjectRootFolder\sample-image.png
此文件可能包含任何 PNG 图像。

### <a name="uwpappprojectrootfolderresourcesresw"></a>\UWPAppProjectRootFolder\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString1">
        <value>LocalizedString1-neutral</value>
    </data>
    <data name="LocalizedString2">
        <value>LocalizedString2-neutral</value>
    </data>
    <data name="NeutralOnlyString">
        <value>NeutralOnlyString-neutral</value>
    </data>
</root>
```

### <a name="uwpappprojectrootfolderde-deresourcesresw"></a>\UWPAppProjectRootFolder\de-DE\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString2">
        <value>LocalizedString2-de-DE</value>
    </data>
</root>
```

### <a name="uwpappprojectrootfolderen-usresourcesresw"></a>\UWPAppProjectRootFolder\en-US\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString1">
        <value>LocalizedString1-en-US</value>
    </data>
    <data name="EnOnlyString">
        <value>EnOnlyString-en-US</value>
    </data>
</root>
```

## <a name="index-the-resources-and-create-a-pri-file"></a>为资源编制索引并创建 PRI 文件
在 `main()` 函数中，调用初始化 COM 之前，声明某些我们需要的字符串，并创建将在其中生成 PRI 文件输出文件夹。

```cppwinrt
std::wstring projectRootFolderUWPApp{ L"UWPAppProjectRootFolder" };
std::wstring generatedPRIsFolder{ projectRootFolderUWPApp + L"\\Generated PRIs" };
std::wstring filePathPRI{ generatedPRIsFolder + L"\\resources.pri" };
std::wstring filePathPRIDumpBasic{ generatedPRIsFolder + L"\\resources-pri-dump-basic.xml" };

::CreateDirectory(generatedPRIsFolder.c_str(), nullptr);
```

调用初始化 COM 后立即声明资源索引器图柄，然后调用 [**MrmCreateResourceIndexer**]() 以创建资源索引器。

```cppwinrt
MrmResourceIndexerHandle indexer;
::ThrowIfFailed(::MrmCreateResourceIndexer(
    L"OurUWPApp",
    projectRootFolderUWPApp.c_str(),
    MrmPlatformVersion::MrmPlatformVersion_Windows10_0_0_0,
    L"language-en_scale-100_contrast-standard",
    &indexer));
```

以下是对传递给 **MrmCreateResourceIndexer** 的参数的说明。

- 目标 UWP 应用的包系列名称，以后从此资源索引器生成 PRI 文件时将用作资源映射名称。
- 目标 UWP 应用的项目根。 换言之，即资源文件的路径。 指定此路径，以便可以在后续对同一个资源索引器的 API 调用中指定相对于该根的路径。
- 希望面向的 Windows 版本。
- 默认资源限定符列表。
- 指向资源索引器图柄的指针，以便函数可以对其进行设置。

下一步是将资源添加到刚刚创建的资源索引器中。 `resources.resw` 是一个包含目标 UWP 应用中性字符串的资源文件 (.resw)。 如果想查看其内容，请向上滚动（在本主题中）。 `de-DE\resources.resw` 包含德语字符串，`en-US\resources.resw` 包含英语字符串。 若要将资源文件内的字符串资源添加到资源索引器，请调用 [**MrmIndexResourceContainerAutoQualifiers**]()。 第三步，向包含资源索引器中性图像资源的文件调用 [**MrmIndexFile**]() 函数。

```cppwinrt
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"de-DE\\resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"en-US\\resources.resw"));
::ThrowIfFailed(::MrmIndexFile(indexer, L"ms-resource:///Files/sample-image.png", L"sample-image.png", L""));
```

调用 **MrmIndexFile** 时，值 L"ms-resource:///Files/sample-image.png" 是资源 uri。 第一个路径段是“Files”，之后从这个资源索引器生成 PRI 文件时，它将用作资源映射子树名称。

在简要介绍了有关资源文件的资源索引器之后，是时候让它通过调用 [**MrmCreateResourceFile**]() 函数在磁盘上生成 PRI 文件了。

```cppwinrt
::ThrowIfFailed(::MrmCreateResourceFile(indexer, MrmPackagingModeStandaloneFile, MrmPackagingOptionsNone, generatedPRIsFolder.c_str()));
```

此时，在一个名为 `Generated PRIs` 的文件夹中已创建了名为 `resources.pri` 的 PRI 文件。 现在我们已经完成了资源索引器，调用 [**MrmDestroyIndexerAndMessages**]() 来破坏其图柄并释放其分配的任何计算机资源。

```cppwinrt
::ThrowIfFailed(::MrmDestroyIndexerAndMessages(indexer));
```

由于 PRI 文件是二进制的，如果将二进制 PRI 文件转储为等效 XML，就更易于查看刚才生成的内容。 调用 [**MrmDumpPriFile**]() 即可实现该操作。

```cppwinrt
::ThrowIfFailed(::MrmDumpPriFile(filePathPRI.c_str(), nullptr, MrmDumpType::MrmDumpType_Basic, filePathPRIDumpBasic.c_str()));
```

以下是对传递给 **MrmDumpPriFile** 的参数的说明。

- 要转储的 PRI 文件的路径。 此调用中未使用资源索引器（刚刚将其破坏），所以需要指定完整的文件路径。
- 无架构文件。 稍后将在主题中讨论什么是架构。
- 仅基本信息。
- 要创建的 XML 文件的路径。

下面是转储到此处 XML 的 PRI 文件包含的内容。

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <ResourceMap name="OurUWPApp" version="1.0" primary="true">
        <Qualifiers>
            <Language>en-US,de-DE</Language>
        </Qualifiers>
        <ResourceMapSubtree name="Files">
            <NamedResource name="sample-image.png" uri="ms-resource://OurUWPApp/Files/sample-image.png">
                <Candidate type="Path">
                    <Value>sample-image.png</Value>
                </Candidate>
            </NamedResource>
        </ResourceMapSubtree>
        <ResourceMapSubtree name="resources">
            <NamedResource name="EnOnlyString" uri="ms-resource://OurUWPApp/resources/EnOnlyString">
                <Candidate qualifiers="Language-en-US" isDefault="true" type="String">
                    <Value>EnOnlyString-en-US</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="LocalizedString1" uri="ms-resource://OurUWPApp/resources/LocalizedString1">
                <Candidate qualifiers="Language-en-US" isDefault="true" type="String">
                    <Value>LocalizedString1-en-US</Value>
                </Candidate>
                <Candidate type="String">
                    <Value>LocalizedString1-neutral</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="LocalizedString2" uri="ms-resource://OurUWPApp/resources/LocalizedString2">
                <Candidate qualifiers="Language-de-DE" type="String">
                    <Value>LocalizedString2-de-DE</Value>
                </Candidate>
                <Candidate type="String">
                    <Value>LocalizedString2-neutral</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="NeutralOnlyString" uri="ms-resource://OurUWPApp/resources/NeutralOnlyString">
                <Candidate type="String">
                    <Value>NeutralOnlyString-neutral</Value>
                </Candidate>
            </NamedResource>
        </ResourceMapSubtree>
    </ResourceMap>
</PriInfo>
```

信息以资源映射开始，它是用目标 UWP 应用的包系列名称命名的。 资源映射包含两个资源映射子树：一个用于已编制索引的文件资源，另一个用于字符串资源。 请注意包系列名称是如何插入到所有资源 URI 中的。

第一个字符串资源是来自 `en-US\resources.resw` 的 *EnOnlyString*，它仅有一个候选项（与 *language-en-US* 限定符匹配）。 接下来是来自 `resources.resw` 和 `en-US\resources.resw` 的 *LocalizedString1*。 因此，它有两个候选项：一个与 *language-en-US* 匹配，另一个回退与任何上下文匹配的中性候选项。 同样，*LocalizedString2* 有两个候选项：*language-de-DE* 和中性。 最后，*NeutralOnlyString* 仅以中性形式存在。 将其命名为该名称以明确不应对它进行本地化。

## <a name="summary"></a>小结
在此方案中，我们介绍了如何使用[包资源索引 (PRI) API](https://msdn.microsoft.com/library/windows/desktop/mt845690) 创建资源索引器。 我们将字符串资源和资产文件添加到资源索引器。 然后，我们使用资源索引器生成二进制 PRI 文件。 最后，我们以 XML 形式转储二进制 PRI 文件，以便可以确认其中含有预期的信息。

## <a name="important-apis"></a>重要的 API
* [包资源索引 (PRI) 参考](https://msdn.microsoft.com/library/windows/desktop/mt845690)

## <a name="related-topics"></a>相关主题
* [包资源索引 (PRI) API 和自定义生成系统](pri-apis-custom-build-systems.md)
