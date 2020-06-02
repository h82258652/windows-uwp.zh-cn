---
title: 创建程序包清单
description: ''
author: denelon
ms.author: denelon
ms.date: 04/29/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8eceb29abbdc7f765628dbd8dbd6f6d0be21f132
ms.sourcegitcommit: e2689c72d5b381eafdb1075090d1961f4c1cb37a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/27/2020
ms.locfileid: "84055151"
---
# <a name="create-your-package-manifest"></a>创建程序包清单

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

如果要将软件包提交到 [Windows 程序包管理器存储库](repository.md)，请首先创建程序包清单。 清单是描述要安装的应用程序的 YAML 文件。

本文介绍了 Windows 程序包管理器的程序包清单的内容。

## <a name="yaml-basics"></a>YAML 基础知识

之所以选择 YAML 格式作为程序包清单格式，是因为人类可以相当容易地阅读它，并且这样可与其他 Microsoft 开发工具保持一致。 如果不熟悉 YAML 语法，可以通过[在 Y 分钟内学会 YAML](https://learnxinyminutes.com/docs/yaml/) 来学习基础知识。

> [!NOTE]
> Windows 程序包管理器的清单当前并非支持所有 YAML 功能。 不支持的 YAML 功能包括定位点、复杂密钥和集。

## <a name="conventions"></a>约定

本文使用以下约定：

* `:` 的左侧是清单定义中使用的文字关键字。
* `:` 的右侧为数据类型。 数据类型可以是基元类型（例如**字符串**），也可以是对本文中其他地方定义的丰富结构的引用。
* 表示法 `[` *datatype* `]` 表示所提到的数据类型的数组。 例如，`[ string ]` 是一个字符串数组。
* 表示法 `{` *datatype* `:` *datatype* `}` 表示一种数据类型到另一种数据类型的映射。 例如，`{ string: string }` 是字符串到字符串的映射。

## <a name="manifest-contents"></a>清单内容

程序包清单必须包含一组必需的项，还可以进一步包含可选项以帮助改善客户在安装软件时的体验。 本部分简要汇总了必需的清单架构和完整的清单架构，以及每种架构的示例。

清单文件中的每个字段都必须采用 Pascal 大小写形式，并且不能重复。

有关清单中的项的完整列表和说明，请参阅 [https://github.com/microsoft/winget-cli](https://github.com/microsoft/winget-cli) 存储库中的[清单规范](https://github.com/microsoft/winget-cli/blob/master/doc/ManifestSpecv0.1.md)。

### <a name="minimal-required-schema"></a>必需的最小架构

#### <a name="minimal-required-schema"></a>[必需的最小架构](#tab/minschema/)

```yaml
Id: string # Publisher.package format.
Publisher: string # The name of the publisher.
Name: string # The name of the application.
Version: string # Version numbering format.
License: string # The open source license or copyright.
InstallerType: string # Enumeration of supported installer types (exe, msi, msix, inno, wix, nullsoft, appx).
Installers:
  - Arch: string # Enumeration of supported architectures.
  - URL: string # Path to download installation file.
  - Sha256: string # SHA256 calculated from installer.
ManifestVersion: 0.1.0
```

#### <a name="example"></a>[示例](#tab/minexample/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
License: Copyright (c) Microsoft Corporation. All rights reserved.
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

* * *

### <a name="complete-schema"></a>完整架构

#### <a name="complete-schema"></a>[完整架构](#tab/compschema/)

```yaml
Id: string # Publisher.package format.
Publisher: string # The name of the publisher.
Name: string # The name of the application.
AppMoniker: string # The common name someone may use to search for the package.
Version: string # Version numbering format for package version.
Channel: string # A string representing the flight ring.
License: string # The open source license or copyright.
LicenseUrl: string # Valid secure URL to license.
MinOSVersion: string # Version numbering format for minimum version of Windows supported.
Description: string # Description of the package.
Homepage: string # Valid secure URL for the package.
Tags: list # Additional strings a user would use to search for the package.
FileExtensions: list # List of file extensions the package could support.
Protocols: list # List of protocols the package provides a handler for.
Commands: list # List of commands or aliases the user would use to run the package.
InstallerType: string # Enumeration of supported installer types (exe, msi, msix, inno, wix, nullsoft, appx).
Custom: string # Custom switches passed to the installer.
Silent: string # Switches passed to the installer for silent installation.
SilentWithProgress: string # Switches passed to the installer for non-interactive install.
Interactive: string # Experimental.
Language: string # Experimental.
Log: string # Specifies log redirection switches and path.
InstallLocation: string # Specifies alternate location to install package.
Installers: # Nested map of keys for specific installer.
  - Arch: string # Enumeration of supported architectures.
  - URL: string # Path to download installation file.
  - Sha256: string # SHA256 calculated from installer.
  - SignatureSha256: string # SHA256 calculated from signature file's hash of MSIX file.
  - Switches: # Collection of entries to override root keys. The primary supported values are: Custom, Silent, SilentWithProgress, Interactive. For a complete list see the specification at https://github.com/microsoft/winget-cli/blob/master/doc/ManifestSpecv0.1.md.
  - Scope: string # Experimental.
  - SystemAppId: string # Experimental.
Localization: # Nested map of keys for localization.
  - Language: string # Locale for display fields and localized URLs.
ManifestVersion: string # Version number format for manifest version.
```

#### <a name="good-example"></a>[良好的示例](#tab/good/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
License: Copyright (c) Microsoft Corporation. All rights reserved.
LicenseUrl: https://docs.microsoft.com/en-us/MicrosoftTeams/assign-teams-licenses
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

#### <a name="better-example"></a>[更好的示例](#tab/better/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
AppMoniker: teams
MinOSVersion: 10.0.0.0
Description: The hub for teamwork in Microsoft 365
Homepage: https://www.microsoft.com/microsoft/teams
License: Copyright (c) Microsoft Corporation. All rights reserved.
LicenseUrl: https://docs.microsoft.com/en-us/MicrosoftTeams/assign-teams-licenses
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

* * *

> [!NOTE]
> 如果安装程序是 .exe，且它是使用 Nullsoft 或 Inno 构建的，则你可改为指定这些值。 指定 Nullsoft 或 Inno 后，客户端将为安装程序设置“无提示”和“无提示但有安装进度”行为。

## <a name="tips-and-best-practices"></a>技巧与最佳做法

* 为了在查找和安装软件时获得最佳的客户体验，建议你尽可能多地包含必需架构以外的可选项。 例如，`AppMoniker` 字段是可选的。 但是，如果你包含此字段，则客户在执行[搜索](../winget/search.md)命令（例如，用 **vscode** 来表示 **Visual Studio Code**）时会看到与 `AppMoniker` 值关联的结果。 如果只有一个应用具有指定的 `AppMoniker` 值，则客户可以通过指定名字对象（而不是完全限定的 ID）来安装应用程序。
* `Id` 必须独一无二。 不能有多个具有相同程序包标识符的提交。 请避免使用空格，因为这会要求用户在使用 [winget](../index.md) 客户端时在 `Id` 两侧加上引号。
* 请避免创建多个发布者文件夹。 例如，如果已有“Contoso”文件夹，请不要再创建“Contoso Ltd”文件夹。 创建文件夹时，也要避免使用空格。
* 如果可能，应使用无提示安装来提交所有程序包。 如果你有一个不支持无提示安装的可执行文件，用户体验会变差。
* 将清单中字符串的换行长度限制为 100 个字符。
* 当程序包的指定版本存在多个安装程序类型时，可以在每个 `Installers` 下放置 `InstallerType` 的一个实例。
