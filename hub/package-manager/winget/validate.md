---
title: winget validate 命令
description: 验证一个用于向 GitHub 上的 Microsoft 社区程序包清单存储库提交软件的清单文件。
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ec1f15ef9086c1083430c9bbe55ea52827ae4bfb
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334464"
---
# <a name="validate-command-winget"></a>validate 命令 (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) 工具的 validate 命令可验证一个用于向 GitHub 上的 Microsoft 社区程序包清单存储库提交软件的[清单文件](../package/manifest.md)。 清单必须是遵循[规范](https://github.com/microsoft/winget-pkgs/YamlSpec.md)的 YAML 文件。

## <a name="usage"></a>用法

`winget validate [--manifest] \<manifest>`

## <a name="arguments"></a>参数

可使用以下参数。

| 参数  | 说明 |
|--------------|-------------|
| **--manifest** |  要验证的清单的路径。 |
| **-?、--help** |  获取有关此命令的更多帮助 |

## <a name="related-topics"></a>相关主题

* [使用 winget 工具安装和管理应用程序](index.md)
* [将程序包提交到 Windows 程序包管理器](../package/index.md)
