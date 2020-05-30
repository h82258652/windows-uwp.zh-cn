---
title: show 命令
description: 显示指定应用程序的详细信息，包括有关该应用程序的源以及与该应用程序关联的元数据的详细信息。
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 1df5a5287b6c7a1321025182f7b3f24ed896e76d
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/19/2020
ms.locfileid: "83824978"
---
# <a name="show-command-winget"></a>show 命令 (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) 工具的 show 命令显示指定应用程序的详细信息，包括有关该应用程序的源以及与该应用程序关联的元数据的详细信息。

show 命令仅显示随应用程序提交的元数据。 如果提交的应用程序排除某些元数据，则不会显示这些数据。

## <a name="usage"></a>用法

`winget show [[-q] \<query>] [\<options>]`

![show 命令](images\show.png)

## <a name="arguments"></a>参数

可使用以下参数。

| 参数  | 说明 |
|--------------|-------------|
| **-q、--query** |  用于搜索应用程序的查询。 |
| **-?、--help** |  获取有关此命令的更多帮助。 |

## <a name="options"></a>选项

可以使用以下选项。

| 选项  | 说明 |
|--------------|-------------|
| **-m、--manifest** | 要安装的应用程序的清单的路径。 |
| **--id**         |  按 ID 筛选结果。 |
| **--name**   |      按名称筛选结果。 |
| **--moniker**   |  按应用程序名字对象筛选结果。 |
| **-v、--version** |  使用指定的版本。 默认值为最新版本。 |
| **-s、--source** |   使用指定的[源](source.md)来查找应用程序。 |
| **-e、--exact**     | 使用完全匹配的方式查找应用程序。 |
| **--versions**    | 显示应用程序的可用版本。 |

## <a name="multiple-selections"></a>多选

如果提供给 winget 的查询未生成单个应用程序，则 winget 会显示搜索结果。 这将为你提供优化搜索所需的其他数据。

## <a name="results-of-show"></a>show 的结果

如果检测到单个应用程序，则会显示以下数据。

### <a name="metadata"></a>元数据

| 值  | 说明 |
|--------------|-------------|
| **Id**   | 应用程序的 ID。 |
| **Name**  | 应用程序的名称。 |
| **Publisher** | 应用程序的发布者。 |
| **Version** | 应用程序的版本。 |
| **Author**  | 应用程序的作者。 |
| **AppMoniker** | 应用程序的 AppMoniker。 |
| **Description** | 应用程序的说明。 |
| **License**  | 应用程序的许可证。 |
| **LicenseUrl** | 应用程序的许可证文件的 URL。 |
| **Homepage**  | 应用程序的主页。 |
| **Tags** | 为协助搜索提供的标记。  |
| **Command** | 应用程序支持的命令。 |
| **Channel**  | 有关应用程序是预览版还是发行版的详细信息。  |
| **Minimum OS Version** | 应用程序支持的最低 OS 版本。 |

### <a name="installer-details"></a>安装程序详细信息

| 值  | 说明 |
|--------------|-------------|
| **Arch**   | 安装程序的体系结构。 |
| **Language**  | 安装程序的语言。 |
| **Installer Type**  | 安装程序的类型。 |
| **Download Url** | 安装程序的 URL。 |
| **Hash** | 安装程序的 Sha-256。  |
| **Scope** | 显示安装程序是针对每台计算机还是针对每个用户。 |

## <a name="related-topics"></a>相关主题

* [使用 winget 工具安装和管理应用程序](index.md)
