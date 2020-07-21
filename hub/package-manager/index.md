---
title: Windows 程序包管理器
description: Windows 程序包管理器是一个综合的程序包管理器解决方案，由一个命令行工具和一组用于在 Windows 10 上安装应用程序的服务组成。
ms.date: 05/03/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 5b25f2c651e11a5ff97a630bb802b236771f5441
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334624"
---
# <a name="windows-package-manager-preview"></a>Windows 程序包管理器（预览）

[!INCLUDE [preview-note](../includes/package-manager-preview.md)]

Windows 程序包管理器是一个综合的[程序包管理器解决方案](#understanding-package-managers)，由一个命令行工具和一组用于在 Windows 10 上安装应用程序的服务组成。

## <a name="windows-package-manager-for-developers"></a>面向开发人员的 Windows 程序包管理器

开发人员可以使用 winget 命令行工具发现、安装、升级、删除和配置特选应用程序集。 安装后，开发人员可以通过 Windows 终端、PowerShell 或命令提示符访问 winget。

有关详细信息，请参阅[使用 winget 工具安装和管理应用程序](winget/index.md)。

## <a name="windows-package-manager-for-isvs"></a>面向 ISV 的 Windows 程序包管理器

独立软件供应商 (ISV) 可以将 Windows 程序包管理器用作包含工具和应用程序的软件包的分发渠道。 我们在 GitHub 上提供了开源 Microsoft 社区程序包清单存储库，方便用户将软件包（包含 .msix、.msi 或 .exe 安装程序）提交到 Windows 程序包管理器。ISV 可以在 GitHub 中上传[程序包清单](package/manifest.md)，我们在考虑后会决定是否将其软件包包括在 Windows 程序包管理器中。 清单会自动进行验证，你也可以手动查看清单。

有关详细信息，请参阅[将程序包提交到 Windows 程序包管理器](package/repository.md)。

## <a name="understanding-package-managers"></a>了解程序包管理器

程序包管理器是一个用于自动安装、升级、配置和使用软件的系统或工具集。 大多数程序包管理器都是设计用于发现和安装开发人员工具。

理想情况下，开发人员使用程序包管理器来指定先决条件，这些先决条件适用于为给定项目开发解决方案所需的工具。 然后，程序包管理器就会按照声明性说明来安装和配置这些工具。 程序包管理器可减少准备环境所需的时间，并有助于确保在计算机上安装相同版本的程序包。

第三方程序包管理器可以利用 [Microsoft 社区程序包清单存储库](package/repository.md)增加其软件目录的大小。

## <a name="related-topics"></a>相关主题

* [使用 winget 工具安装和管理软件包](winget/index.md)
* [将程序包提交到 Windows 程序包管理器](package/index.md)