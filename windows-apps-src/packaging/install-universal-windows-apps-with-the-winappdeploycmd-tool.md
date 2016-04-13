---
ms.assetid: 6AA037C0-35ED-4B9C-80A3-5E144D7EE94B
使用 WinAppDeployCmd.exe 工具安装应用
Windows 应用程序部署 (WinAppDeployCmd.exe) 是一个命令行工具，可用于将通用 Windows 平台 (UWP) 应用从 Windows 10 计算机部署到任意 Windows 10 移动版设备。
---
# 使用 WinAppDeployCmd.exe 工具安装应用

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Windows 应用程序部署 (WinAppDeployCmd.exe) 是一个命令行工具，可用于将通用 Windows 平台 (UWP) 应用从 Windows 10 计算机部署到任意 Windows 10 移动版设备。 当 Windows 10 移动版设备通过 USB 进行连接或无需 Microsoft Visual Studio 或该应用的解决方案即可连接到同一子网时，可使用此工具部署 .appx 程序包。 本文介绍如何使用此工具安装 UWP 应用。

你只需安装 Windows 10 SDK 即可从命令提示符或脚本文件中运行 WinAppDeployCmd 工具。 在与 WinAppDeployCmd.exe 一起安装应用时，此操作使用 .appx 文件将应用旁加载到 Windows 10 移动版设备。 此命令不会安装应用所需的证书。 若要运行该应用，Windows 10 移动版设备必须处于开发人员模式下或已经安装了证书。

在可以将应用部署到 Windows 10 移动版设备前，必须首先创建程序包。 有关详细信息，请参阅 \[parent link here\]。

**WinAppDeployCmd.exe** 工具在 Windows 10 计算机上位于此处：**C:\\Program Files (x86)\\Windows Kits\\10\\bin\\x86\\WinAppDeployCmd.exe**（基于 SDK 的安装路径）。 首先，将你的 Windows 10 移动版设备连接到同一子网，或通过 USB 直接将其连接到你的 Windows 10 计算机。 然后使用以下语法和本文后面的此命令的示例部署 .appx 程序包：

## WinAppDeployCmd 语法和选项

以下是适用于 **WinAppDeployCmd.exe** 的语法

``` syntax
WinAppDeployCmd command -option <argument> ...
    WinAppDeployCmd devices
    WinAppDeployCmd devices <x>
    WinAppDeployCmd install -file <path> -ip <address>
    WinAppDeployCmd install -file <path> -guid <address> -pin <p>
    WinAppDeployCmd install -file <path> -ip <address> -dependency <a> <b> ...
    WinAppDeployCmd install -file <path> -guid <address> -dependency <a> <b> ...
    WinAppDeployCmd uninstall -file <path>
    WinAppDeployCmd uninstall -package <name>
    WinAppDeployCmd update -file <path>
    WinAppDeployCmd list -ip <address>
    WinAppDeployCmd list -guid <address>
```

你可以在目标设备上安装或卸载应用，也可以更新已经安装的应用。 若要保留由已经安装的应用保存的数据或设置，请使用 **update** 选项而非 **install** 选项。

下表介绍了 **WinAppDeployCmd.exe** 命令。

|             |                                                                     |
|-------------|---------------------------------------------------------------------|
| **命令** | **说明**                                                     |
| 设备     | 显示可用网络设备列表。                         |
| 安装     | 将 UWP 应用包安装到目标设备。                     |
| 更新      | 更新已安装在目标设备上的 UWP 应用。    |
| 列表        | 显示已安装在指定目标设备上的 UWP 应用列表。 |
| 卸载   | 从目标设备卸载指定的应用包。         |

 

下表介绍了 **WinAppDeployCmd.exe** 的选项

|                  |                                                                                                                                                                                                               |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **命令**      | **说明**                                                                                                                                                                                               |
| -h (-help)       | 显示命令、选项和参数。                                                                                                                                                                     |
| -ip              | 目标设备的 IP 地址。                                                                                                                                                                              |
| -g (-guid)       | 目标设备的唯一标识符。                                                                                                                                                                       |
| -d (-dependency) | （可选）指定每个程序包依赖项的依赖项路径。 如果未指定任何路径，该工具会在应用包的根目录和 SDK 目录中搜索依赖项。 |
| -f (-file)       | 要安装、更新或卸载的应用包的文件路径。                                                                                                                                                |
| -p (-package)    | 要卸载的应用包的完整程序包名称。 （你可以使用列表命令查找已经安装在设备上的程序包的完整名称。）                                                   |
| -pin             | 与目标设备建立连接所需的引脚。 （如果需要身份验证，将提示你使用 -pin 选项重试。）                                                 |

 

下表介绍了 **WinAppDeployCmd.exe** 的选项。

|                        |                                                                              |
|------------------------|------------------------------------------------------------------------------|
| **参数**           | **说明**                                                              |
| <x>              | 超时（以秒为单位）。 （默认值为 10 秒）                                          |
| <address>        | 目标设备的 IP 地址或唯一标识符。                        |
| <a><b>... | 每个程序包依赖项的依赖项路径。                    |
| <p>              | 在设备设置中显示的用于建立连接的字母数字引脚。 |
| <path>           | 文件系统路径。                                                            |
| <name>           | 要卸载的应用包的完整程序包名称。                          |

 
## WinAppDeployCmd.exe 示例

以下是一些有关如何使用 **WinAppDeployCmd.exe** 语法在命令行进行部署的示例。

显示可用于部署的设备。 命令超时 3 秒。

``` syntax
WinAppDeployCmd devices 3
```

将应用从电脑“下载”目录中的 MyApp.appx 程序包安装到 IP 地址为 192.168.0.1、PIN 为 A1B2C3 的 Windows 10 移动版设备，来与该设备建立连接

``` syntax
WinAppDeployCmd install -file "Downloads\MyApp.appx" -ip 192.168.0.1 -pin A1B2C3
```

从 IP 地址为 192.168.0.1 的 Windows 10 移动版设备卸载指定的程序包（基于其完整名称）。 你可以使用列表命令查看安装在设备上的任意程序包的完整名称。

``` syntax
WinAppDeployCmd uninstall -package Company.MyApp_1.0.0.1_x64__qwertyuiop -ip 192.168.0.1
```

使用指定的 .appx 程序包更新已经安装在 IP 地址为 192.168.0.1 的 Windows 10 移动版设备上的应用。

``` syntax
WinAppDeployCmd update -file "Downloads\MyApp.appx" -ip 192.168.0.1
```



<!--HONumber=Mar16_HO1-->


