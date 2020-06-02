---
title: search 命令
description: 查询可用于安装的应用程序的源
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 5a176c1138ebfe3f3a9eb2cbef02dad745cfe170
ms.sourcegitcommit: 8193aef04deb3514eb2d34bfe5cb9424ba12cd76
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/26/2020
ms.locfileid: "83865014"
---
# <a name="search-command-winget"></a>search 命令 (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) 工具的 search 命令查询可用于安装的应用程序的源。  

search 命令可以显示所有可用的应用程序，也可以向下筛选到特定应用程序。 search 命令通常用于确定某个用于安装特定应用程序的字符串。

## <a name="usage"></a>用法

`winget search [[-q] \<query>] [\<options>]`

![搜索](images\search.png)

## <a name="arguments"></a>参数

可使用以下参数。

| 参数  | 说明 |
 --------------|-------------|
| **-q、--query** |  用于搜索应用的查询。 |
| **-?、--help** |  获取有关此命令的更多帮助。 |

## <a name="show-all"></a>全部显示

如果 search 命令不包含筛选器或选项，它会显示默认源中所有可用的应用程序。 如果只传入“source”选项，则还可搜索另一个源中的所有应用程序。

## <a name="search-strings"></a>搜索字符串

可以使用以下选项筛选搜索字符串。

| 选项  | 说明 |
 --------------|-------------|
| **--id**        |   将搜索限制为应用程序的 ID。 该 ID 包含发布者和应用程序名称。 |
| **--name**      |  将搜索限制为应用程序的名称。 |
| **--moniker**  |    将搜索限制为指定的名字对象。 |
| **--tag**    |  将搜索限制为针对应用程序列出的标记。 |
| **--command**   |   将搜索限制为应用程序的名称。 |

系统会将该字符串视为子字符串。 默认情况下，搜索也不区分大小写。 例如，`winget search micro` 可返回以下内容：

* Microsoft
* microscope
* MyMicro

## <a name="search-options"></a>搜索选项

search 命令支持使用多个选项或筛选器来限制结果。

| 选项  | 说明 |
 --------------|-------------|
| **-e、--exact**  |     在查询中使用确切的字符串，包括检查是否区分大小写。 它不会使用子字符串的默认行为。  |  
| **-n、--count**      |  将显示的输出限制为指定的计数。 |
| **-s、--source**     |  将搜索限制为指定的[源](source.md)名称。  |

## <a name="related-topics"></a>相关主题

* [使用 winget 工具安装和管理应用程序](index.md)
