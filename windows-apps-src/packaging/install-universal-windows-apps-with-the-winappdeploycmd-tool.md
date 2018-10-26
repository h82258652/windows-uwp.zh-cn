---
author: laurenhughes
ms.assetid: 6AA037C0-35ED-4B9C-80A3-5E144D7EE94B
title: 使用 WinAppDeployCmd.exe 工具安装应用
description: Windows 应用程序部署 (WinAppDeployCmd.exe) 是可用于将部署到任意 windows 10 设备从 windows 10 电脑的通用 Windows 平台 (UWP) 应用的命令行工具。
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 13468ce3b74992c026d94223b5e67aea99d79991
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5569728"
---
# <a name="install-apps-with-the-winappdeploycmdexe-tool"></a>使用 WinAppDeployCmd.exe 工具安装应用


Windows 应用程序部署 (WinAppDeployCmd.exe) 是可用于将部署到任意 windows 10 设备从 windows 10 电脑的通用 Windows 平台 (UWP) 应用的命令行工具。 此工具可用于在 windows 10 设备而无需为该应用的 Microsoft Visual Studio 或解决方案是通过 USB 连接还是位于同一子网的情况下部署应用包。 你还可以将事先未打包的应用部署到远程电脑或 Xbox One。 本文介绍如何使用此工具安装 UWP 应用。

你只需在 windows 10 SDK 安装为从命令提示符或脚本文件中运行 WinAppDeployCmd 工具。 当你使用 WinAppDeployCmd.exe 安装应用时，这用于.appx/.msix 文件或 AppxManifest （适用于松散文件） 旁加载你的应用到 windows 10 设备。 此命令不会安装应用所需的证书。 若要运行应用时，windows 10 设备必须处于开发人员模式下或已经安装了证书。

若要部署到移动设备，必须首先创建程序包。 有关详细信息，请查看[此处](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)。

**WinAppDeployCmd.exe**工具位于以下 windows 10 电脑上： **C:\\Program Files (x86) \\Windows Kits\\10\\bin\\<SDK Version>\\x86\\WinAppDeployCmd.exe** （基于 sdk 的安装路径表示）。 
> [!NOTE]
> 在 15063 版本及更高版本的 SDK 中，SDK 并行安装到版本特定的文件夹中。  以前的 SDK（14393 及以前）直接写入父文件夹。

首先，将 windows 10 设备连接到同一子网，或将其连接到 windows 10 计算机使用 USB 连接直接。 然后使用以下语法和本文后面的此命令的示例部署 UWP 应用：

## <a name="winappdeploycmd-syntax-and-options"></a>WinAppDeployCmd 语法和选项

这是用于 **WinAppDeployCmd.exe** 的常规语法：
```syntax
WinAppDeployCmd command -option <argument>
```

下面是关于使用各种命令的一些其他语法示例：
```syntax
WinAppDeployCmd devices
WinAppDeployCmd devices <x>
WinAppDeployCmd install -file <path> -ip <address>
WinAppDeployCmd install -file <path> -guid <address> -pin <p>
WinAppDeployCmd install -file <path> -ip <address> -dependency <a> <b> 
WinAppDeployCmd install -file <path> -guid <address> -dependency <a> <b>
WinAppDeployCmd uninstall -file <path>
WinAppDeployCmd uninstall -package <name>
WinAppDeployCmd update -file <path>
WinAppDeployCmd list -ip <address>
WinAppDeployCmd list -guid <address>
WinAppDeployCmd deployfiles -file <path> -remotedeploydir <remoterelativepath> -ip <address>
WinAppDeployCmd registerfiles -remotedeploydir <remoterelativepath> -ip <address>
WinAppDeployCmd addcreds -credserver <server> -credusername <username> -credpassword <password> -ip <address>
WinAppDeployCmd getcreds -credserver <server> -ip <address>
WinAppDeployCmd deletecreds -credserver <server> -ip <address>
```

你可以在目标设备上安装或卸载应用，也可以更新已经安装的应用。 若要保留由已经安装的应用保存的数据或设置，请使用 **update** 选项而非 **install** 选项。

下表介绍了 **WinAppDeployCmd.exe** 命令。


| **命令**  | **说明**                                                     |
|--------------|---------------------------------------------------------------------|
| 设备      | 显示可用网络设备列表。                         |
| 安装      | 将 UWP 应用包安装到目标设备。                     |
| 更新       | 更新已安装在目标设备上的 UWP 应用。    |
| 列表         | 显示已安装在指定目标设备上的 UWP 应用列表。 |
| 卸载    | 从目标设备卸载指定的应用包。         |
| deployfiles  | 将目标路径处的松散文件应用复制到设备上的远程相对路径。|
| registerfiles| 将松散文件应用注册到远程部署目录。         |
| addcreds     | 将凭据添加到 Xbox 以允许它访问应用注册的网络位置。|
| getcreds     | 从网络共享运行应用程序时，获取目标使用的网络凭据。|
| deletecreds  | 从网络共享运行应用程序时，删除目标使用的网络凭据。|


下表介绍了用于 **WinAppDeployCmd.exe** 的选项。


| **命令**  | **说明**  |
|--------------|------------------|
| -h (-help)       | 显示命令、选项和参数。 |
| -ip              | 目标设备的 IP 地址。 |
| -g (-guid)       | 目标设备的唯一标识符。|
| -d (-dependency) | （可选）指定每个程序包依赖项的依赖项路径。 如果未指定任何路径，该工具会在应用包的根目录和 SDK 目录中搜索依赖项。|
| -f (-file)       | 要安装、更新或卸载的应用包的文件路径。|
| -p (-package)    | 要卸载的应用包的完整程序包名称。 （你可以使用列表命令查找已经安装在设备上的程序包的完整名称） |
| -pin             | 与目标设备建立连接所需的引脚。 （如果需要身份验证，将提示你使用 -pin 选项重试） |
| -credserver      | 供目标使用的网络凭据的服务器名称。 |
| -credusername    | 供目标使用的网络凭据的用户名。 |
| -credpassword    | 供目标使用的网络凭据的密码。 |
| -connecttimeout  | 连接到设备时所使用的超时（以秒为单位）。 |
| -remotedeploydir | 要将文件复制到的远程设备上的相对目录路径/名称；这将是一个自动确定的已知远程部署文件夹。 |
| -deleteextrafile | 用于指示是否应清除远程目录中的现有文件以匹配源目录的开关。 |


下表介绍了 **WinAppDeployCmd.exe** 的选项。

| **参数**           | **描述**                                                              |
|------------------------|------------------------------------------------------------------------------|
| &lt;x&gt;              | 超时（以秒为单位）。 （默认值为 10 秒）                                          |
| &lt;地址&gt;        | 目标设备的 IP 地址或唯一标识符。                        |
| &lt;a&gt;&lt;b&gt; ... | 每个应用包依赖项的依赖项路径。                    |
| &lt;p&gt;              | 在设备设置中显示的用于建立连接的字母数字引脚。 |
| &lt;路径&gt;           | 文件系统路径。                                                            |
| &lt;名称&gt;           | 要卸载的应用包的完整程序包名称。                          |
| &lt;server&gt;         | 文件网络上的服务器。                                                  |
| &lt;username&gt;       | 有权访问文件网络上的服务器的凭据的用户。      |
| &lt;password&gt;       | 有权访问文件网络上的服务器的凭据的密码。 |
| &lt;remotedeploydir&gt;| 设备上相对于部署位置的目录                      |

 
## <a name="winappdeploycmdexe-examples"></a>WinAppDeployCmd.exe 示例

以下是一些有关如何使用 **WinAppDeployCmd.exe** 语法在命令行进行部署的示例。

显示可用于部署的设备。 命令超时 3 秒。

``` syntax
WinAppDeployCmd devices 3
```

从 windows 10 设备的 IP 地址为 192.168.0.1、 PIN 为 A1B2C3 以与设备建立连接到电脑的下载目录中的 MyApp.appx 程序包安装应用

``` syntax
WinAppDeployCmd install -file "Downloads\MyApp.appx" -ip 192.168.0.1 -pin A1B2C3
```

从 IP 地址为 192.168.0.1 的 Windows 10 设备中卸载指定的程序包（基于其完整名称）。 你可以使用列表命令查看安装在设备上的任意程序包的完整名称。

``` syntax
WinAppDeployCmd uninstall -package Company.MyApp_1.0.0.1_x64__qwertyuiop -ip 192.168.0.1
```

更新已经安装在 IP 地址为 192.168.0.1 使用指定的应用包的 windows 10 设备的应用。

``` syntax
WinAppDeployCmd update -file "Downloads\MyApp.appx" -ip 192.168.0.1
```

将应用的文件部署到 IP 地址为 192.168.0.1 的电脑或 Xbox 中，这些文件与该设备的部署路径下的 app1_F5 目录中 AppxManifest 位于同一文件夹中。

``` syntax
WinAppDeployCmd deployfiles -file "C:\apps\App1\AppxManifest.xml" -remotedeploydir app1_F5 -ip 192.168.0.1
```

将应用注册在 IP 地址为 192.168.0.1 的电脑或 Xbox 的部署路径下的 app1_F5 目录。

``` syntax
WinAppDeployCmd registerfiles -file app1_F5 -ip 192.168.0.1
```

## <a name="using-winappdeploycmd-to-set-up-run-from-pc-deployment-on-xbox-one"></a>使用 WinAppDeployCmd 设置 Xbox One 上的从电脑运行部署。

从电脑运行使你无需复制二进制文件就可以将 UWP 应用程序部署到 Xbox One，而不是将这些二进制文件托管在 Xbox 所在同一网络的网络共享上。  为此，需要开发人员已解锁的 Xbox One 和 Xbox 可以访问的网络驱动器上的松散文件（即 UWP 应用程序）。

运行此命令注册应用：
``` syntax
WinAppDeployCmd registerfiles -ip <Xbox One IP> -remotedeploydir <location of app> -username <user for network> -password <password for user>

ex. WinAppDeployCmd register files -ip 192.168.0.1 -remotedeploydir \\driveA\myAppLocation -username admin -password A1B2C3
```
