---
title: source 命令
description: 管理 Windows 程序包管理器访问的存储库。
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 5d383dfc4e66c75c993210d382b674508ad3cef4
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334489"
---
# <a name="source-command-winget"></a>source 命令 (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

> [!NOTE]
> source 命令目前仅供内部使用。 目前不支持其他源。

[winget](index.md) 工具的 source 命令可管理 Windows 程序包管理器访问的存储库。 使用 source 命令，可以添加、删除、列出以及更新存储库。

源为你提供用于发现和安装应用程序的数据。 仅当你信任新源作为安全位置时才会添加该源。

## <a name="usage"></a>用法

`winget source \<sub command> \<options>`

![源图像](images\source.png)

## <a name="arguments"></a>参数

可使用以下参数。

| 参数  | 说明 |
|--------------|-------------|
| **-?、--help** |  获取有关此命令的更多帮助。 |

## <a name="sub-commands"></a>子命令

源支持以下用于操作源的子命令。

| 子命令  | 说明 |
|--------------|-------------|
|  **add** |  添加一个新源。 |
|  **list** | 枚举已启用源的列表。 |
|  **update** | 更新源。 |
|  **删除** | 删除源。 |
|  **reset** | 将 winget 重置回初始配置。  |

## <a name="options"></a>选项

source 命令支持以下选项。

| 选项  | 说明 |
|--------------|-------------|
|  **-n、--name** | 用于标识源的名称。 |
|  **-a、--arg** | 源的 URL 或 UNC。 |
|  **-t、--type** | 源的类型。 |
| **-?、--help** |  获取有关此命令的更多帮助。 |

## <a name="add"></a>添加

add 子命令可添加新源。 此子命令需要“--name”选项和“name”参数。

用法：`winget source add [-n, --name] \<name> [-a] \<url> [[-t] \<type>]`

示例：`winget source add --name Contoso  https://www.contoso.com/cache`

add 子命令还支持可选的“type”参数。 “type”参数向客户端传递它所连接的存储库类型。 支持以下类型。

| 类型  | 说明 |
|--------------|-------------|
| **Microsoft.PreIndexed.Package** | 源 \<default> 的类型。 |

## <a name="list"></a>列表

list 子命令枚举当前已启用的源。 此子命令还提供特定源的详细信息。

用法：`winget source list [-n, --name] \<name>`

### <a name="list-all"></a>全部列出

list 子命令本身会显示受支持源的完整列表。 例如：

```CMD
> C:\winget source list
> Name   Arg
> -----------------------------------------
> winget https://winget.azureedge.net/cache

```

### <a name="list-source-details"></a>列出源详细信息

若要获取源的完整详细信息，请传入用于标识源的名称。 例如：

```CMD
> C:\winget source list --name contoso  
> Name   : contoso  
> Type   : Microsoft.PreIndexed.Package  
> Arg    : https://pkgmgr-int.azureedge.net/cache  
> Data   : AppInstallerSQLiteIndex-int_g4ype1skzj3jy  
> Updated: 2020-4-14 17:45:32.000
```

Name 显示用于标识源的名称。
Type 显示存储库的类型。
Arg 显示源使用的 URL 或路径。
Data 显示所使用的可选包名称（如果适用）。
Updated 显示上次更新源的日期和时间。

## <a name="update"></a>更新

update 子命令强制更新单个源或所有源。

用法：`winget source update [-n, --name] \<name>`

### <a name="update-all"></a>全部更新

update 子命令本身会请求并更新每个存储库。 例如：`C:\winget update`

### <a name="update-source"></a>更新源

update 子命令与“--name”选项结合使用可定向到单个源并对其进行更新。 例如：`C:\winget source update --name contoso`

## <a name="remove"></a>删除

remove 子命令可删除源。 此子命令需要“--name”选项和“name”参数才能标识源。

用法：`winget source add [-n, --name] \<name>`

例如：`winget source remove --name Contoso`

## <a name="reset"></a>reset

reset 子命令可将客户端重置回其原始配置。 reset 子命令可删除所有源并将源设置为默认值。 仅在极少数情况下才使用此子命令。

用法：`winget source reset`

例如：`winget source reset`

## <a name="default-repository"></a>默认存储库

Windows 程序包管理器会指定默认存储库。 可使用 list 命令标识该存储库。 例如：`winget source list`

## <a name="related-topics"></a>相关主题

* [使用 winget 工具安装和管理应用程序](index.md)
