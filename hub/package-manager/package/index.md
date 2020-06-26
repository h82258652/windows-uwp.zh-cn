---
title: 将程序包提交到 Windows 程序包管理器
description: 可将 Windows 程序包管理器用作包含应用程序的软件包的分发渠道。
ms.date: 04/29/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: ae9c9039154e2a576a691a01d64abcf8c9029c1c
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334603"
---
# <a name="submit-packages-to-windows-package-manager"></a>将程序包提交到 Windows 程序包管理器

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

如果你是独立软件供应商 (ISV)，则可将 Windows 程序包管理器用作包含应用程序的软件包的分发渠道。 Windows 程序包管理器目前支持以下格式的安装程序：MSIX、MSI 和 EXE。

若要将软件包提交到 Windows 程序包管理器，请执行以下步骤：

1. [创建提供应用程序相关信息的程序包清单](manifest.md)。 清单是遵循 Windows 程序包管理器架构的 YAML 文件。
2. [将清单提交到 Windows 程序包管理器存储库](repository.md)。 这是 GitHub 上的开源存储库，其中包含 winget**** 工具可以访问的清单的集合。

## <a name="related-topics"></a>相关主题

* [使用 winget 工具](../winget/index.md)
* [创建程序包清单](manifest.md)
* [将清单提交到存储库](repository.md)