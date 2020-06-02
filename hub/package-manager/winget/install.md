---
title: install 命令
description: 安装指定的应用程序。
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 8c460ccd18bb1bb12e5322e0e08a17edbd9692f7
ms.sourcegitcommit: 5a145eda92b5915393e58006867cdd8b98e922f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84166236"
---
# <a name="install-command-winget"></a>install 命令 (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) 工具的 install 命令可安装指定的应用程序。 使用 [search](search.md) 命令找到要安装的应用程序。  

install 命令要求你指定要安装内容的具体字符串。 如果存在任何不明确性，系统会提示你进一步将 install 命令筛选到具体应用程序。

## <a name="usage"></a>用法

`winget install [[-q] \<query>] [\<options>]`

![search 命令](images\install.png)

## <a name="arguments"></a>参数

可使用以下参数。

| 参数      | 说明 |
|-------------|-------------|  
| **-q、--query**  |  用于搜索应用的查询。 |
| **-?、--help** |  获取有关此命令的更多帮助。 |

## <a name="options"></a>选项

这些选项允许你根据自己的需求自定义安装体验。

| 选项      | 说明 |
|-------------|-------------|  
| **-m、--manifest** |   必须后跟清单 (YAML) 文件的路径。 可以使用清单从[本地 YAML 文件](#local-install)运行安装体验。 |
| **--id**    |  将安装限制为应用程序的 ID。   |  
| **--name**   |  将搜索限制为应用程序的名称。 |  
| **--moniker**   | 将搜索限制为针对应用程序列出的名字对象。 |  
| **-v、--version**  |  允许你指定要安装的确切版本。 如果此项未指定，则使用 latest 会安装最高版本的应用程序。 |  
| **-s、--source**   |  将搜索限制为所提供的源名称。 必须后跟源名称。 |  
| **-e、--exact**   |   在查询中使用确切的字符串，包括检查是否区分大小写。 它不会使用子字符串的默认行为。 |  
| **-i、--interactive** |  以交互模式运行安装程序。 默认体验会显示安装程序进度。 |  
| **-h、--silent** |  以静默模式运行安装程序。 此选项禁止显示所有 UI。 默认体验会显示安装程序进度。 |  
| **-o、--log**  |  将日志记录定向到日志文件。 必须提供你具有写入权限的文件的路径。 |
| **--override** | 要直接传递给安装程序的字符串。    |
| **-l、--location** |    要安装到的位置（如果支持）。 |

### <a name="example-queries"></a>示例查询

以下示例安装特定版本的应用程序。

```CMD
winget install powertoys --version 0.15.2
```

以下示例根据应用程序 ID 安装相应应用程序。

```CMD
winget install --id Microsoft.PowerToys
```

以下示例按版本和 ID 安装应用程序。

```CMD
winget install --id Microsoft.PowerToys --version 0.15.2
```

## <a name="multiple-selections"></a>多选

如果提供给 winget 的查询未生成单个应用程序，则 winget 会显示搜索结果。 这将为你提供优化搜索所需的其他数据，以便正确进行安装。

## <a name="local-install"></a>本地安装

使用“manifest”选项，可以通过将 YAML 文件直接传递到客户端来安装应用程序。 “manifest”选项有以下用法。

用法：`winget install --manifest \<file>`

| 选项  | 说明 |
|-------------|-------------|  
|  **-m、--manifest** | 要安装的应用程序的清单的路径。 |

### <a name="log-files"></a>日志文件

除非重定向，否则 winget 的日志文件将位于以下文件夹中：\%temp%\\AICLI\\*.log

## <a name="related-topics"></a>相关主题

* [使用 winget 工具安装和管理应用程序](index.md)
