---
title: winget hash 命令
description: 为安装程序生成 SHA256 哈希。
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2f7b2074a9d6f2a01fe5a74f7f081ceeedcccec9
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/19/2020
ms.locfileid: "83824948"
---
# <a name="hash-command-winget"></a>hash 命令 (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) 工具的 hash 命令可为安装程序生成 SHA256 哈希。 如果需要创建[清单文件](../package/manifest.md)，以便将软件提交到 GitHub 上的 Microsoft 社区程序包清单存储库，请使用此命令。 此外，hash 命令还支持为 MSIX 文件生成 SHA256 证书哈希。

## <a name="usage"></a>用法

`winget hash [-f] \<file> [\<options>]`

hash 子命令只能对本地文件运行。 若要使用 hash 子命令，请将安装程序下载到已知位置。 然后，将文件路径作为参数传递到 hash 子命令。

## <a name="arguments"></a>参数

可使用以下参数：

| 参数  | 说明 |
|--------------|-------------|
| **-f、--file** |  要进行哈希运算的文件的路径。 |
| **-m、--msix**  | 指定 hash 命令还会创建可以与 MSIX 安装程序配合使用的 SHA 256 SignatureSha256。 |
| **-?、--help** |  获取有关此命令的更多帮助。 |

## <a name="related-topics"></a>相关主题

* [使用 winget 工具安装和管理应用程序](index.md)
* [将程序包提交到 Windows 程序包管理器](../package/index.md)
