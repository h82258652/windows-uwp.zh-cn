---
title: 使用 MakeAppx.exe 工具创建应用包
description: MakeAppx.exe 创建、加密、解密应用程序包和捆绑包，并从中提取文件。
ms.date: 06/21/2018
ms.topic: article
keywords: windows 10, uwp, 打包, windows 10, uwp, packaging
ms.assetid: 7c1c3355-8bf7-4c9f-b13b-2b9874b7c63c
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: dc109fe2e684dd3bc1fef62cece5cac3ab50d246
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "7831632"
---
# <a name="create-an-app-package-with-the-makeappxexe-tool"></a>使用 MakeAppx.exe 工具创建应用包


**MakeAppx.exe** 同时创建应用包和应用捆绑包。 **MakeAppx.exe** 还从应用包或捆绑包中提取文件，并加密或解密应用程序包和捆绑包。 此工具包含在 Windows 10 SDK 中，并且可以从命令提示符或脚本文件中使用。

> [!IMPORTANT] 
> 如果你使用 Visual Studio 开发你的应用，建议使用 Visual Studio 向导创建应用包。 有关详细信息，请参阅[使用 Visual Studio 打包 UWP 应用](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)。

请注意，**MakeAppx.exe** 不会创建 .appxupload 文件。 .Appxupload 文件作为 Visual Studio 打包过程的一部分进行创建，并包含其他两个文件：.msix 或.appx 和.appxsym。 .Appxsym 文件是压缩的.pdb 文件，其中包含用于在合作伙伴中心中的[故障分析](../publish/health-report.md)应用的公共符号。 也可以提交常规 .appx 文件，但崩溃分析或调试信息将不可用。 有关将程序包提交到应用商店的详细信息，请参阅[上载应用包](../publish/upload-app-packages.md)。 

 更新到最新版本的 Windows 10 中的此工具不会影响.appx 包使用情况。 你可以继续使用此工具与.appx 程序包，或支持针对.msix 包使用该工具，如下所述。

手动创建一个 .appxupload 文件：
- 将.msix 和.appxsym 放在一个文件夹
- 压缩该文件夹
- 将压缩的文件夹扩展名从 .zip 更改为 .appxupload

## <a name="using-makeappxexe"></a>使用 MakeAppx.exe

根据 SDK 的安装路径，以下是 **MakeAppx.exe** 在 Windows 10 电脑上的位置：
- x86: C:\Program Files (x86) \Windows Kits\10\bin\\&lt;内部版本号&gt;\x86\makeappx.exe
- x64: C:\Program Files (x86) \Windows Kits\10\bin\\&lt;内部版本号&gt;\x64\makeappx.exe

没有此工具的 ARM 版本。

### <a name="makeappxexe-syntax-and-options"></a>MakeAppx.exe 语法和选项

常规 **MakeAppx.exe** 语法：

``` Usage
MakeAppx <command> [options]      
```

下表介绍了 **MakeAppx.exe** 的命令。

| **命令**   | **描述**                       |
|---------------|---------------------------------------|
| pack          | 创建程序包。                    |
| unpack        | 将指定程序包中的所有文件提取到指定的输出目录。 |
| bundle        | 创建捆绑包。                     |
| unbundle      | 将所有程序包解压到以捆绑包完整名称命名的指定输出路径下的子目录中。 |
| encrypt       | 在指定的输出程序包/捆绑包中，从输入程序包/捆绑包中创建加密的应用包或捆绑包。 |
| decrypt       | 在指定的输出程序包/捆绑包中，从输入应用包/捆绑包中创建解密的应用包或捆绑包。 |


此选项列表适用于所有命令：

| **选项**    | **描述**                       |
|---------------|---------------------------------------|
| /d            | 指定输入、输出或内容目录。 |
| /l            | 用于本地化的程序包。 本地化程序包上的默认验证过程。 此选项仅禁用特定验证，无需禁用所有验证。 |
| /kf           | 使用指定密钥文件中的键加密或解密程序包或捆绑包。 它不能与 /kt 一起使用。 |
| /kt           | 使用全局测试密钥加密或解密程序包或捆绑包。 它不能与 /kf 一起使用。 |
| /no           | 如果存在，会阻止输出文件的覆盖。 如果不指定此选项或 /o 选项，将询问用户是否要覆盖文件。 |
| /nv           | 跳过语义验证。 如果不指定此选项，该工具将执行程序包的完整验证。 |
| /o            | 覆盖输出文件（如果存在）。 如果不指定此选项或 /no 选项，将询问用户是否要覆盖文件。 |
| /p            | 指定应用包或捆绑包。  |
| /v            | 使详细日志记录输出到控制台。 |
| /?            | 显示帮助文本。                   |


下面的列表包含可能的参数：

| **参数**                          | **描述**                       |
|---------------------------------------|---------------------------------------|
| &lt;output package name&gt;           | 创建的程序包的名称。 这是附有.msix 或.appx 的文件名。 |
| &lt;encrypted output package name&gt; | 创建的加密包的名称。 这是附有.emsix 或.eappx 的文件名。 |
| &lt;input package name&gt;            | 程序包的名称。 这是附有.msix 或.appx 的文件名。 |
| &lt;encrypted input package name&gt;  | 加密包的名称。 这是附有.emsix 或.eappx 的文件名。 |
| &lt;output bundle name&gt;            | 创建的捆绑包的名称。 这是附有.msixbundle 或.appxbundle 文件名称。 |
| &lt;encrypted output bundle name&gt;  | 创建的加密捆绑包的名称。 这是附有.emsixbundle 或.eappxbundle 的文件名。 |
| &lt;input bundle name&gt;             | 捆绑包的名称。 这是附有.msixbundle 或.appxbundle 文件名称。 |
| &lt;encrypted input bundle name&gt;   | 加密捆绑包的名称。 这是附有.emsixbundle 或.eappxbundle 的文件名。 |
| &lt;content directory&gt;             | 应用包或捆绑包内容的路径。 |
| &lt;mapping file&gt;                  | 指定程序包源和目标的文件名。 |
| &lt;output directory&gt;              | 输出程序包和捆绑包的目录路径。 |
| &lt;密钥文件&gt;                      | 含有加密或解密的密钥的文件名。 |
| &lt;algorithm ID&gt;                  | 创建块映射时使用的算法。 有效算法包括：SHA256（默认值）、SHA384、SHA512。 |


### <a name="create-an-app-package"></a>创建应用包

应用包是整套.msix 或.appx 包文件中打包的应用的文件。 若要使用 **pack** 命令创建应用包，必须提供程序包位置的内容目录或映射文件。 你还可以在创建它时加密程序包。 如果你想要加密程序包，必须使用 /ep 并指定是否使用密钥文件 (/kf) 或全局测试密钥 (/kt)。 有关创建加密程序包的详细信息，请参阅[加密或解密程序包或捆绑包](#encrypt-or-decrypt-a-package-or-bundle)。

特定于 **pack** 命令的选项：

| **选项**    | **描述**                       |
|---------------|---------------------------------------|
| /f            | 指定映射文件。           |
| /h            | 指定创建块映射时使用的哈希算法。 它仅可以与 pack 命令一起使用。 有效算法包括：SHA256（默认值）、SHA384、SHA512。 |
| /m            | 指定输入应用清单的路径，该清单将用于生成输出应用包或资源包清单的基础。  当使用此选项时，还必须使用 /f 并在映射文件中包含 [ResourceMetadata] 部分，来指定要包含在生成的清单中的资源维度。|
| /nc           | 阻止程序包文件的压缩。 默认情况下，将基于检测到的文件类型压缩文件。 |
| /r            | 生成资源包。 它必须与 /m 一起使用，并表示使用 /l 选项。 |  


下面的用法示例演示了一些可能的 **pack** 命令语法选项：

``` syntax 
MakeAppx pack [options] /d <content directory> /p <output package name>
MakeAppx pack [options] /f <mapping file> /p <output package name>
MakeAppx pack [options] /m <app package manifest> /f <mapping file> /p <output package name>
MakeAppx pack [options] /r /m <app package manifest> /f <mapping file> /p <output package name>
MakeAppx pack [options] /d <content directory> /ep <encrypted output package name> /kf <key file>
MakeAppx pack [options] /d <content directory> /ep <encrypted output package name> /kt

```
下面显示了 **pack** 命令的命令行示例：

``` examples
MakeAppx pack /v /h SHA256 /d "C:\My Files" /p MyPackage.msix
MakeAppx pack /v /o /f MyMapping.txt /p MyPackage.msix
MakeAppx pack /m "MyApp\AppxManifest.xml" /f MyMapping.txt /p AppPackage.msix
MakeAppx pack /r /m "MyApp\AppxManifest.xml" /f MyMapping.txt /p ResourcePackage.msix
MakeAppx pack /v /h SHA256 /d "C:\My Files" /ep MyPackage.emsix /kf MyKeyFile.txt
MakeAppx pack /v /h SHA256 /d "C:\My Files" /ep MyPackage.emsix /kt
```

### <a name="create-an-app-bundle"></a>创建应用程序包

应用程序包类似于应用包，但应用程序包可以减少用户下载的应用大小。 例如，对于特定于语言的资源、各种图像缩放资源或适用于特定版本的 Microsoft DirectX 的资源，应用程序包很有用。 类似于创建加密应用包，还可以在绑定它时加密应用程序包。 要加密应用程序包，请使用 /ep 选项并指定是否使用密钥文件 (/kf) 或全局测试密钥 (/kt)。 有关创建加密捆绑包的详细信息，请参阅[加密或解密程序包或捆绑包](#encrypt-or-decrypt-a-package-or-bundle)。

特定于 **bundle** 命令的选项：

| **选项**    | **描述**                       |
|---------------|---------------------------------------|
| /bv           | 指定捆绑包的版本号。 版本号必须以句号分为四部分，格式为：&lt;主要&gt;.&lt;次要&gt;.&lt;内部版&gt;.&lt;修订版&gt;。 |
| /f            | 指定映射文件。           |

请注意，如果未指定捆绑包版本，或如果已设置为“0.0.0.0”，则使用当前日期时间创建捆绑包。

下面的用法示例演示了一些可能的 **bundle** 命令语法选项：

``` syntax
MakeAppx bundle [options] /d <content directory> /p <output bundle name>
MakeAppx bundle [options] /f <mapping file> /p <output bundle name>
MakeAppx bundle [options] /d <content directory> /ep <encrypted output bundle name> /kf MyKeyFile.txt
MakeAppx bundle [options] /f <mapping file> /ep <encrypted output bundle name> /kt
```
下面的块包含 **bundle** 命令的示例：

``` examples
MakeAppx bundle /v /d "C:\My Files" /p MyBundle.msixbundle
MakeAppx bundle /v /o /bv 1.0.1.2096 /f MyMapping.txt /p MyBundle.msixbundle
MakeAppx bundle /v /o /bv 1.0.1.2096 /f MyMapping.txt /ep MyBundle.emsixbundle /kf MyKeyFile.txt
MakeAppx bundle /v /o /bv 1.0.1.2096 /f MyMapping.txt /ep MyBundle.emsixbundle /kt
```

### <a name="extract-files-from-a-package-or-bundle"></a>从程序包或捆绑包中解压缩文件

除了打包和捆绑应用，**MakeAppx.exe** 还可以解压缩或解包现有程序包。 必须提供内容目录作为解压缩文件的目标。 如果要从加密的程序包或捆绑包中解压缩文件，可以使用 /ep 选项并指定它是否应使用密钥文件 (/kf) 或全局测试密钥 (/kt) 进行解密，同时解密和解压缩文件。 有关解密程序包或捆绑包的详细信息，请参阅[加密或解密程序包或捆绑包](#encrypt-or-decrypt-a-package-or-bundle)。

特定于 **unpack** 和 **unbundle** 命令的选项：

| **选项**    | **描述**                       |
|---------------|---------------------------------------|
| /nd           | 解压缩或解包软件包/捆绑包时，不执行解密。 |
| /pfn          | 将所有文件解压缩/解包到指定输出路径下的子目录中，以程序包或捆绑包的完整名称命名 |

下面的用法示例演示了一些可能的 **unpack** 和 **unbundle** 命令语法选项：

``` syntax
MakeAppx unpack [options] /p <input package name> /d <output directory>
MakeAppx unpack [options] /ep <encrypted input package name> /d <output directory> /kf <key file>
MakeAppx unpack [options] /ep <encrypted input package name> /d <output directory> /kt

MakeAppx unbundle [options] /p <input bundle name> /d <output directory>
MakeAppx unbundle [options] /ep <encrypted input bundle name> /d <output directory> /kf <key file>
MakeAppx unbundle [options] /ep <encrypted input bundle name> /d <output directory> /kt
```

下面的块包含 **unpack** 和 **unbundle** 命令的使用示例：

``` examples
MakeAppx unpack /v /p MyPackage.msix /d "C:\My Files"
MakeAppx unpack /v /ep MyPackage.emsix /d "C:\My Files" /kf MyKeyFile.txt
MakeAppx unpack /v /ep MyPackage.emsix /d "C:\My Files" /kt

MakeAppx unbundle /v /p MyBundle.msixbundle /d "C:\My Files"
MakeAppx unbundle /v /ep MyBundle.emsixbundle /d "C:\My Files" /kf MyKeyFile.txt
MakeAppx unbundle /v /ep MyBundle.emsixbundle /d "C:\My Files" /kt
```

### <a name="encrypt-or-decrypt-a-package-or-bundle"></a>加密或解密程序包或捆绑包

**MakeAppx.exe** 工具也可以加密或解密现有程序包或捆绑包。 只需提供程序包名称、输出程序包名称，以及加密或解密是否应使用密钥文件 (/kf) 或全局测试密钥 (/kt)。

Visual Studio 打包向导不提供加密和解密。 

特定于 **encrypt** 和 **decrypt** 命令的选项：

| **选项**    | **描述**                       |
|---------------|---------------------------------------|
| /ep           | 指定加密的应用包或捆绑包。 |

下面的用法示例演示了一些可能的 **encrypt** 和 **decrypt** 命令语法选项：

``` syntax
MakeAppx encrypt [options] /p <package name> /ep <output package name> /kf <key file>
MakeAppx encrypt [options] /p <package name> /ep <output package name> /kt

MakeAppx decrypt [options] /ep <package name> /p <output package name> /kf <key file>
MakeAppx decrypt [options] /ep <package name> /p <output package name> /kt
```

下面的块包含 **encrypt** 和 **decrypt** 命令的使用示例：

``` examples
MakeAppx.exe encrypt /p MyPackage.msix /ep MyEncryptedPackage.emsix /kt
MakeAppx.exe encrypt /p MyPackage.msix /ep MyEncryptedPackage.emsix /kf MyKeyFile.txt

MakeAppx.exe decrypt /p MyPackage.msix /ep MyEncryptedPackage.emsix /kt
MakeAppx.exe decrypt p MyPackage.msix /ep MyEncryptedPackage.emsix /kf MyKeyFile.txt
```

## <a name="key-files"></a>密钥文件

关键文件必须以包含字符串“[密钥]”的行开始，后跟描述要用于加密每个程序包的密钥的行。 每个密钥由一对引号内的字符串表示，以空格或制表符分隔。 第一个字符串表示 base64 编码的 32 字节键 ID，第二个表示 base64 编码的 32 字节加密密钥。 密钥文件应该是一个简单的文本文件。

密钥文件的示例：

``` Example
[Keys]
"OWVwSzliRGY1VWt1ODk4N1Q4R2Vqc04zMzIzNnlUREU="    "MjNFTlFhZGRGZEY2YnVxMTBocjd6THdOdk9pZkpvelc="
```

## <a name="mapping-files"></a>映射文件
映射文件必须以包含字符串“[文件]”的行开始，后跟描述要添加到程序包的文件的行。 每个文件由一对引号内的路径表示，以空格或制表符分隔。 每个文件表示其源（在磁盘上）和目标（在程序包中）。 映射文件应该是一个简单的文本文件。

映射文件（无 /m 选项）的示例：

``` Example
[Files]
"C:\MyApp\StartPage.html"               "default.html"
"C:\Program Files (x86)\example.txt"    "misc\example.txt"
"\\MyServer\path\icon.png"              "icon.png"
"my app files\readme.txt"               "my app files\readme.txt"
"CustomManifest.xml"                    "AppxManifest.xml"
``` 

使用映射文件时，可以选择是否想要使用 /m 选项。 /M 选项允许用户指定映射文件中的资源元数据，该文件将包含在生成的清单中。 如果使用 /m 选项，映射文件必须包含以“[ResourceMetadata]”行开始的部分，后跟指定“ResourceDimensions”和“ResourceId”的行。 应用程序包可以包含多个“ResourceDimensions”，但永远只会存在一个“ResourceId”。

映射文件（带 /m 选项）的示例：

``` Example
[ResourceMetadata]
"ResourceDimensions"                    "language-en-us"
"ResourceId"                            "English"

[Files]
"images\en-us\logo.png"                 "en-us\logo.png"
"en-us.pri"                             "resources.pri"
```

## <a name="semantic-validation-performed-by-makeappxexe"></a>由 MakeAppx.exe 执行的语义式验证

**MakeAppx.exe** 执行有限的语义式验证，旨在捕获最常见的部署错误并帮助确保应用包有效。 如果想要在使用 **MakeAppx.exe** 时跳过验证，请参阅 /nv 选项。 

此验证确保：
- 在程序包清单中引用的所有文件都包含在应用包中。
- 应用程序没有两个相同的密钥。
- 应用程序不会注册此列表中的禁用协议：SMB、FILE、MS-WWA-WEB、MS-WWA。 

这不是完整的语义式验证，因为它仅用于捕获常见错误。 不能保证通过 **MakeAppx.exe** 生成的程序包均可安装。
