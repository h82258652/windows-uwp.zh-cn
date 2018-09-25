---
title: 管理本地连接存储
author: aablackm
description: 了解如何在开发环境中管理本地连接存储数据。
ms.assetid: 630cb5fc-5d48-4026-8d6c-3aa617d75b2e
ms.author: aablackm
ms.date: 02/27/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 连接存储
ms.localizationpriority: medium
ms.openlocfilehash: 5185ae50b428302c26b7a38389e4b925dcecd552
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2018
ms.locfileid: "4175711"
---
# <a name="managing-local-connected-storage"></a>管理本地连接存储
使用连接存储在云中存储游戏数据时，连接存储服务还有本地存储组件。 无论你在使用电脑还是主机，都存在包含已同步到云的数据的本地连接存储数据缓存。 无论你要创建 XDK 还是 UWP 主题作品，都有工具可用来管理本地连接存储数据。

请参阅下表查找可用于管理本地缓存的连接存储数据的相应工具：

|主题作品分类  |设备  |本地存储工具  |
|---------|---------|---------|
|XDK     |Xbox One 主机     |*xbstorage*         |
|UWP     |电脑         |*gamesaveutil*         |
|UWP     |Xbox One 主机     |Xbox 设备门户 (XDP |)

- *Xbstorage* 是命令行工具，从 XDK 命令提示符下运行，用于管理 Xbox One 主机上本地缓存的连接存储。 *Xbstorage* 工具可以在 Xbox One XDK 的以下文件路径下找到：**/Program 文件 (x86)/Microsoft Durango XDK/bin/xbstorage.exe**
- *Gamesaveutil* 是用于管理电脑上 UWP 本地缓存的连接存储的命令行工具。 [Windows 10 SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk) Fall Creators Update 及更高版本（版本 10.0.16299.15 及更高版本）附带 *Gamesaveutil* 工具。 在安装合适版本的 Windows 10 SDK 后，可以在此文件夹下找到 *gamesaveutil*：**ProgramFiles(x86)/Windows Kits/10/bin/[SDK Version]/x64/gamesaveutil.exe**。
- *Xbox 设备门户 (XDP)* 是可用来管理 Xbox One 主机上本地缓存的连接存储 UWP 数据的在线门户。 本文不介绍 XDP 的使用方法。 若要了解如何使用 XDP，请阅读 [Xbox 设备门户](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-xbox)。

## <a name="xbstorage"></a>Xbstorage

*Xbstorage*允许从硬盘上清除本地缓存的连接存储数据，以及通过使用 XML 文件将用户或计算机的数据导入/导出连接存储空间。

在使用 *xbstorage* 工具对本地数据执行操作时，系统会像应用自己执行操作那样运行，因此，将连接存储空间中的数据读取到本地文件的操作会导致在复制之前与云同步。

同样，将开发电脑上 XML 文件中的数据复制到 Xbox One 开发工具包上的连接存储容器，也会导致主机开始将此数据上传到云。 但是，在一些情况下不会发生此现象：如果开发工具包无法获取锁，或者，如果主机上的容器和云中的容器之间存在数据冲突，则主机会像用户已经决定不解决冲突（通过选取一个要保留的容器版本）那样运行，并且在下次启动主题作品之前，主机会像用户处于脱机状态一样运行。

这些命令有种例外情况，即 **reset** **/force**，它会为所有 SCID 和用户清除已保存数据的本地存储，但不会改变在云中存储的数据。 如果用户漫游到控制台，并在运行一个游戏后从云中下载数据，那么，这对将控制台置于它应所处的状态非常有用。

### <a name="xbstorage-commands"></a>Xbstorage 命令

Xbstorage 具有以下六个开发人员可以在 XDK 命令提示符下用来管理其 Xbox One 开发工具包上的本地数据的命令：

<a id="xbstorage_reset"></a>

|命令  |说明  |
|---------|---------|
|reset    |对连接存储执行出厂重置。         |
|import   |将数据从指定的 XML 文件导入连接存储空间。         |
|export   |将数据从连接存储空间导出到指定的 XML 文件。         |
|delete   |从连接存储空间删除数据。         |
|generate |生成虚拟数据并保存到指定的 XML 文件中。         |
|simulate |模拟存储空间不足的情况。         |

### <a name="xbstorage-reset"></a>Xbstorage reset

`xbstorage reset [/force]`

清除本地主机的连接存储中的所有本地数据，将其还原为出厂设置。 已保存到云中的数据不会修改，并会根据需要下载。

|选项  |说明  |
|---------|---------|
|/force   |确认应重置连接存储。 如果运行 reset 命令时不指定 **/force**，将显示以下消息：连接存储工厂重置操作可能具有破坏性，除非存在 **/force** 标记，否则此命令不会执行重置操作。 |

<a id="xbstorage_import"></a>

### <a name="xbstorage-import"></a>Xbstorage import

`xbstorage import *file-name* [/scid:*SCID*] [/machine] [/msa:*account*] [/replace] [/verbose]`

将 *filename* 中的指定数据导入到连接存储空间。
该文件是包含该数据的 XML 文件。 有关示例，请参阅 [xbstorage generate](#xbstorage_generate)。 关于该文件的 XML 格式的详细信息，请参阅本主题后面部分的[导入和导出文件格式](#xbstorage_fileformat)。
有两种指定连接存储空间的方法：

- 如果输入文件包含已正确填充的 **ContextDescription** 小节，则它将用于指定连接存储空间。
- 存储空间还可以通过命令行参数部分或完全指定，这些参数优先于输入文件中指定的存储空间对应元素。

用法示例：

```cmd
  xbstorage import mydata.xml
  xbstorage import mydata.xml /replace
  xbstorage import mydata.xml /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage import mydata.xml /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage import mydata.xml /verbose 
```

> [!NOTE]
> 导入到指定连接存储空间之前，系统将尝试使用与正在运行的应用获取连接存储空间时所用的相同逻辑来与云同步。
>
> 如果具有相同主 SCID 的应用程序正在运行，此操作可能导致争用，并且连接存储空间的内容可能会处于不确定的状态。
>
> 如果未指定 **/replace**，将在写入输入文件中指定的 blob 之前清除输入文件中指定的容器。 输入文件中未指定的连接存储空间中的容器将保持不变。

|选项  |说明  |
|---------|---------|
|*file-name*     |指定包含要导入的数据的 XML 文件。         |
|/scid:*SCID*    |指定服务配置标识符 (SCID)。         |
|/machine        |指定每台计算机的连接存储空间。  此选项不能与 **/msa** 选项一起使用。         |
|/msa:*account*  |为每个用户的连接存储指定要使用的帐户。 用户必须登录到主机才能使用此空间。  此选项不能与 **/machine** 选项一起使用。         |
|/replace        |在导入前删除指定连接存储空间中的所有容器。         |
|/verbose        |显示导入的状态。         |


 <a id="xbstorage_export"></a>

### <a name="xbstorage-export"></a>Xbstorage export

`xbstorage export *outfile* [/context:*input-file*] [/scid:*SCID*] [/machine] [/msa:*account*] [/verbose]`

将数据从连接存储空间导出到 **outfile** 指定的文件。    该文件是包含该数据的 XML 文件。 请参阅 [xbstorage generate](#xbstorage_generate) 了解如何生成示例。 关于该文件的 XML 格式的详细信息，请参阅本主题后面部分的[导入和导出文件格式](#xbstorage_fileformat)。 有两种指定连接存储空间的方法：

- 如果使用了 **/context**参数，并且由 \<infile> 指定文件名的文件中包含正确填充的 **ContextDescription** 小节，则将使用该文件来指定连接存储空间。
- 存储空间还可以通过命令行参数部分或完全指定，这些参数优先于 **/context** 文件中指定的存储空间对应元素。

用法示例：

```cmd
  xbstorage export exporteddata.xml /context:space.xml
  xbstorage export exporteddata.xml /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage export exporteddata.xml /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage export exporteddata.xml /context:space.xml /verbose
```

> [!NOTE]
> 导出指定连接存储空间之前，系统将尝试使用与正在运行的应用获取连接存储空间时所用的相同逻辑来与云同步。
>
> 如果具有相同主 SCID 的应用程序正在运行，此操作可能导致争用，并且连接存储空间的内容可能会处于不确定的状态。

|选项  |说明  |
|---------|---------|
|*outfile*             |数据将导出到其中的 XML 文件。         |
|/context:*input-file* |指定要从中读取空间信息的输入文件。         |
|/scid:*SCID*          |指定服务配置标识符 (SCID)。         |
|/machine              |指定每台计算机的连接存储空间。  此选项不能与 **/msa** 选项一起使用。         |
|/msa:*account*        |为每个用户的连接存储指定要使用的帐户。 用户必须登录到主机才能使用此空间。  此选项不能与 **/machine** 选项一起使用。         |
|/verbose              |显示导出操作的状态。         |

<a id="xbstorage_delete"></a>

### <a name="xbstorage-delete"></a>Xbstorage delete

`xbstorage delete [/context:*input-file*] [/scid:*SCID*] [/machine] [/msa:*account*] [/verbose]`

从连接存储空间中删除所有数据。
有两种指定连接存储空间的方法：

- 如果使用了 **/context**参数，并且由 \<infile> 指定文件名的文件中包含正确填充的 **ContextDescription** 小节，则将使用该文件来指定连接存储空间。
- 存储空间还可以通过命令行参数部分或完全指定，这些参数优先于 **/context** 文件中指定的存储空间对应元素。

用法示例：

```cmd
  xbstorage delete /context:space.xml
  xbstorage delete /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage delete /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage delete /context:space.xml /verbose
```

> [!NOTE]
> 删除指定连接存储空间之前，系统将尝试使用与正在运行的应用获取连接存储空间时所用的相同逻辑来与云同步。
>> 如果具有相同主 SCID 的应用程序正在运行，此操作可能导致争用，并且连接存储空间的内容可能会处于不确定的状态。

|选项  |说明 |
|---------|---------|
|/context:*input-file*     |指定要从中读取空间信息的输入文件。         |
|/scid:*SCID*              |指定服务配置标识符 (SCID)。         |
|/machine                  |指定每台计算机的连接存储空间。  此选项不能与 **/msa** 选项一起使用。         |
|/msa:*account*            |为每个用户的连接存储指定要使用的帐户。 用户必须登录到主机才能使用此空间。  此选项不能与 **/machine** 选项一起使用。         |
|/verbose                  |显示删除操作的状态。         |

 <a id="xbstorage_generate"></a>

### <a name="xbstorage-generate"></a>Xbstorage generate

`xbstorage generate *file-name* [/containers:*n*] [/blobs:*n*] [/blobsize:*n*]`

生成虚拟数据并将它保存到 \<filename> 指定的文件中。 关于该文件的 XML 格式的详细信息，请参阅本主题后面部分的[导入和导出文件格式](#xbstorage_fileformat)。    服务配置标识符 (SCID) 将设置为 00000000-0000-0000-0000-000000000000，并将为每台计算机的连接存储空间设置帐户。 如果想更改这些值，可以在它生成之后直接编辑该文件。

用法示例：

```cmd
  xbstorage generate dummydata.xml
  xbstorage generate dummydata.xml /containers:4
  xbstorage generate dummydata.xml /blobs:10
  xbstorage generate dummydata.xml /containers:4 /blobs:10
  xbstorage generate dummydata.xml /containers:4 /blobs:10 /blobsize:512
```

> [!NOTE]
> 字节数据是一种简单的升序系列；例如，五字节 blob 可能包含以下字节：00 01 02 03 04。 >> 如果要指定每位用户的连接存储空间，请将 XML 文件中的**帐户**节点更改为如下内容：
>  ```
>    <Account msa="user@domain.com"/>
>  ```

|选项  |说明  |
|---------|---------|
|*file-name*     | 数据将写入到其中的 XML 文件。 |
|/containers:*n* | 指定要生成的容器数 *n*。 默认数量为 2。  |
|/blobs:*n*      | 指定要生成的 blob 数 *n*。 默认数量为 3。  |
|/blobsize:*n*   | 指定每个 blob 的字节数 *n*。 默认大小为 1024 字节。  |

 <a id="xbstorage_simulate"></a>

### <a name="xbstorage-simulate"></a>Xbstorage simulate

`xbstorage simulate [/reserveremainingspace] [/forceoutoflocalstorage] [/stop] [/verbose]`

模拟连接存储服务的本地存储用尽情况。

|选项  |说明  |
|---------|---------|
|/reserveremainingspace | 保留连接存储中的所有剩余空间。 从连接存储中删除内容将释放你可以使用的空间。 |

|/forceoutoflocalstorage | 模拟没有可用剩余空间的连接存储服务。 从连接存储中删除内容不会改变连接存储服务的内存用尽报告状态。 |

|/stop | 停止所有模拟。 |

|/verbose | 显示所模拟操作的状态。 |

 <a id="ID4E4MAC"></a>

### <a name="common-options"></a>常用选项

`xbstorage [/?] [/X*:address* [*+accesskey*] ]`

|选项  |说明  |
|---------|---------|
| /?                           |  显示 xbstorage.exe 帮助 |
| /X *:address* [*+accesskey*]  | 指定目标主机的主机名称或地址（显示为主机上的**工具 IP**），但不会更改默认的主机。 如果不使用此选项，则使用默认主机。*Accesskey* 是字符串，可使用它来仅限知道访问密钥的人访问主机。 使用命令 **xbconfig** **accesskey=***your-key* 设置访问密钥；然后重启主机以使访问密钥生效。 若要访问配置了访问密钥的主机，必须在主机的 IP 地址或主机名后使用加号 (+)，再跟访问密钥。
> [!NOTE]
> 如果提供了访问密钥，而默认主机由 xbconnect 设置，则该访问密钥将作为默认主机的一部分地址存储。
|

## <a name="gamesaveutil"></a>Gamesaveutil

*Gamesaveutil* 用于管理应用的本地缓存的连接存储，具有 *xbstorage* 提供的所有功能。 与 xbstorage 工具一样，*gamesaveutil* 提供相同的六个数据管理功能，只是行为上存在一些差异。 import、export、delete 和 reset 命令要求有 Xbox Live 用户登录。 可以在 Windows 10 中使用 Xbox 应用来查看和更改当前用户。

### <a name="commands"></a>命令

|命令  |说明  |
|---------|---------|
|import   |从指定的 XML 文件导入数据         |
|export   |将数据导出到指定的 xml 文件         |
|delete   |从指定应用删除数据        |
|reset    |仅删除指定应用的本地数据         |
|generate |生成虚拟数据并保存到指定的 xml 文件中         |
|simulate |模拟难以测试的特殊情况         |

### <a name="gamesaveutil-import"></a>Gamesaveutil import

`gamesaveutil import <filename> [/pfn:<PFN>] [/scid:<SCID>] [/replace]`

导入 \<filename> 中指定的数据

该文件是包含该数据的 XML 文件。 键入 `gamesaveutil help generate` 了解如何生成示例。

有两种方式可指定保存数据的应用 PFN 和 SCID：

如果输入文件包含已正确填充的 ContextDescription 小节，则它将用于指定目标应用 PFN 和 SCID。

该 PFN 和 SCID 还可以通过命令行参数部分或完全指定，这些参数优先于输入文件中指定的 PFN 和 SCID 对应元素。

|选项  |说明  |
|---------|---------|
|/pfn:\<PFN>       |指定为其执行导入的应用的包系列名称 (PFN)。 该应用必须已经安装。         |
|/scid:\<SCID>     |指定服务配置标识符 (SCID)。 这是来自 Xbox Live 配置的设置。         |
|/replace         |在执行导入前先删除所有容器。         |

用法示例：

```cmd
gamesaveutil import mydata.xml
gamesaveutil import mydata.xml /replace
gamesaveutil import mydata.xml /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> 该应用必须已安装并已同步数据才能导入。
> 
> 如果未指定 /replace，则除非已在输入文件中指定，否则不会对现有容器执行操作。

### <a name="gamesaveutil-export"></a>Gamesaveutil export

`gamesaveutil export <outfile> [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

将数据导出到 \<outfile> 指定的文件

该文件是包含该数据的 XML 文件。 键入 gamesaveutil help generate 了解如何生成示例。

有两种方式可指定要导出的数据的位置：

如果使用了 /context 参数，并且由 \<infile> 指定的文件名包含正确填充的 ContextDescription 小节，则将使用该文件来指定源数据的位置。

该位置还可以通过命令行参数指定，这些参数优先于 /context 文件指定的对应元素。

|选项  |说明  |
|---------|---------|
|/context:\<infile>     |使用指定文件指定应用 PFN 和 SCID。         |
|/pfn:\<PFN>            |指定从中导出数据的应用的包系列名称 (PFN)。 该应用必须已经安装。       |
|/scid:\<SCID>          |指定服务配置标识符 (SCID)。 这是来自 Xbox Live 配置的设置。        |

用法示例：

```cmd
gamesaveutil export exporteddata.xml /context:target.xml
gamesaveutil export exporteddata.xml /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> 该应用必须已安装并已同步数据才能导出。

### <a name="gamesaveutil-delete"></a>Gamesaveutil delete

`gamesaveutil delete [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

针对指定的 PFN 和 SCID 删除所有数据。

有两种方式可指定要删除的数据的位置：

如果使用了 /context 参数，并且由 \<infile> 指定的文件名包含正确填充的 ContextDescription 小节，则将使用该文件来指定源数据的位置。

该位置还可以通过命令行参数指定，这些参数优先于 /context 文件指定的对应元素。

|选项  |说明  |
|---------|---------|
|/context:\<infile>     |使用指定文件指定应用 PFN 和 SCID。         |
|/pfn:\<PFN>            |指定从中删除数据的应用的包系列名称 (PFN)。 该应用必须已经安装。       |
|/scid:\<SCID>          |指定服务配置标识符 (SCID)。 这是来自 Xbox Live 配置的设置。        |

用法示例：

```cmd
gamesaveutil delete /context:target.xml
gamesaveutil delete /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> 该应用必须已安装才能删除容器。

### <a name="gamesaveutil-reset"></a>Gamesaveutil reset

`gamesaveutil reset [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

针对指定的 PFN 和 SCID 删除所有本地数据。

有两种方式可指定要删除的数据的位置：

如果使用了 /context 参数，并且由 \<infile> 指定的文件名包含正确填充的 ContextDescription 小节，则将使用该文件来指定源数据的位置。

该位置还可以通过命令行参数指定，这些参数优先于 /context 文件指定的对应元素。

|选项  |说明  |
|---------|---------|
|/context:\<infile>     |使用指定文件指定应用 PFN 和 SCID。         |
|/pfn:\<PFN>            |指定从中删除数据的应用的包系列名称 (PFN)。 该应用必须已经安装。       |
|/scid:\<SCID>          |指定服务配置标识符 (SCID)。 这是来自 Xbox Live 配置的设置。        |

用法示例：

```cmd
gamesaveutil reset /context:target.xml
gamesaveutil reset /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> 该应用必须已安装才能删除本地数据。

### <a name="gamesaveutil-generate"></a>Gamesaveutil generate

`gamesaveutil generate <filename> [/containers:<n>] [/blobs:<n>] [/blobsize:<n>]`

生成虚拟数据并将它保存到 \<filename> 指定的文件中。
服务配置标识符 (SCID) 将设置为 00000000-0000-0000-0000-000000000000。 生成后，如果需要，可手动编辑该文件以更改这些值。

|选项  |说明  |
|---------|---------|
|/containers:\<n>     |指定要生成多少容器。 默认值为 2。         |
|/blobs:\<n>          |指定每个容器要生成多少个 blob。 默认值为 3。        |
|/blobsize:\<n>       |指定每个 blob 包含多少字节。 默认值为 1024。         |

用法示例：

```cmd
gamesaveutil generate dummydata.xml
gamesaveutil generate dummydata.xml /containers:4
gamesaveutil generate dummydata.xml /blobs:10
gamesaveutil generate dummydata.xml /containers:4 /blobs:10
gamesaveutil generate dummydata.xml /containers:4 /blobs:10 /blobsize:512
```


> [!NOTE]
> 字节数据是一种简单的升序系列；例如，五字节 blob 可能包含以下字节：00 01 02 03 04。

### <a name="gamesaveutil-simulate"></a>Gamesaveutil simulate

`gamesaveutil simulate [/markcontainerschanged] [/stop]`

模拟用于测试某些情况的特殊条件。

|选项  |说明  |
|---------|---------|
|/markcontainerschanged     |当应用从暂停状态恢复并获得新提供程序时，强制使所有容器都看起来像已发生更改。 影响所有应用，直至使用 /stop 清除。      |
|/stop                      |停止所有模拟。         |


 <a id="xbstorage_fileformat"></a>

## <a name="import-and-export-file-format"></a>导入和导出文件格式

与 *xbstorage* 工具的 **import**、**export** 和 **generate** 命令一起使用的 XML 文件具有以下示例所示的格式。

```xml
<?xml version="1.0" encoding="UTF-8"?>
  <XbConnectedStorageSpace>
    <ContextDescription>
      <Account msa="user@domain.com" />
      <Title scid="00000000-0000-0000-0000-000000000000" />
    </ContextDescription>
    <Data>
      <Containers>
        <Container name="Container1" displayName="Optional Display Name">
          <Blobs>
            <Blob name="Blob1">
              <![CDATA[... ] ]>
            </Blob>
            ...
            <Blob name="BlobN">
              <![CDATA[... ] ]>
            </Blob>
          </Blobs>
        </Container>
        ...
        <Container name="ContainerN">
        ...
        </Container>
      </Containers>
    </Data>
  </XbConnectedStorageSpace>
```

在 *gamesaveutil* 中需要针对 **import**、**export** 和 **generate** 对此 xml 文件所做的唯一更改是将 \<ContextDescription> 节点的 \<Account> 成员节点替换为 \<PackageFamilyName> 节点。
这将改变 \<ContextDescription> 节点，从：

```xml
<ContextDescription>
    <Account msa="user@domain.com" />
    <Title scid="00000000-0000-0000-0000-000000000000" />
</ContextDescription>
```

变为：

```xml
<ContextDescription>
    <PackageFamilyName pfn="MyGame_xxxxxx" />
    <Title scid="00000000-0000-0000-0000-000000000000" />
</ContextDescription>
```

> [!NOTE]
> 这些 XML 文件中的数据的格式与平台上的内容不同，它仅用于导入和导出。 这些 XML 文件的数据格式可能在以后发生更改，因此应将它们视为中间格式或实用工具格式，而不是存档格式。

**ContextDescription** 节点是可选的。 如果要为一台计算机设置连接存储空间，可以使用 `<Account machine="true"/>` 而不是 `<Account msa="user@domain.com"/>`。 否则，可以在命令行上为导入指定上下文。
应该按要为其生成文件的游戏或应用程序为 Blob 和容器提供相应的名称。
每个 Blob 的内容都应该是包含在 **CDATA** 标记中的字符串，该字符串使用以原始字节队列的形式为 blob 提供数据的 **CRYPT_STRING_BASE64** 标记调用 **CryptBinaryToStringW** 来生成。 反过来，blob 数据可以也通过调用 **CryptStringToBinary** 并提供之前加密的字符串来重新转换为原来的形式。 Visual Studio 的 MSDN 论坛中的 [CryptBinaryToString 返回无效字节](http://social.msdn.microsoft.com/Forums/vstudio/en-US/9acac904-c226-4ae0-9b7f-261993b9fda2/cryptbinarytostring-returns-invalid-bytes?forum=vcgeneral)提供了使用这两个函数的示例。

<a id="ID4EWBAE"></a>