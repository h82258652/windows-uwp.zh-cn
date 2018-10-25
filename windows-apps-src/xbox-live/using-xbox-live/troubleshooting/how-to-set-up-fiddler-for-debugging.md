---
title: 使用 Fiddler 的 Xbox Live 疑难解答
author: KevinAsgari
description: 了解如何使用 Fiddler 解决 Xbox Live 服务调用问题。
ms.assetid: 7d76e444-027b-4659-80d5-5b2bf56d199e
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, fiddler, 服务调用, 疑难解答
ms.localizationpriority: medium
ms.openlocfilehash: 9eaa21ca792ab3071cb06a9e564f7ad1826782b4
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5481414"
---
# <a name="troubleshooting-xbox-live-using-fiddler"></a>使用 Fiddler 的 Xbox Live 疑难解答

Fiddler 是一种 Web 调试代理，用于记录你的设备和 Internet 之间的所有 HTTP 和 HTTPS 流量。 你将使用它记录和检查与 Xbox Live 服务和依赖方 Web 服务之间的流量，以便了解和调试 Web 服务调用。

## <a name="for-windows-uwp-pc-apps"></a>适用于 Windows UWP 电脑应用

1. 确保当前用户位于电脑上的管理员组中。
1. 下载从 Fiddler[http://www.telerik.com/fiddler](http://www.telerik.com/fiddler)
1. 确保你选择“适用于 .NET 4”的版本
1. 安装完成后，转至“工具”->“Fiddler”选项并启用“捕获 HTTPS CONNECT 和解密 HTTPS 流量”。  运行时与 Xbox LIVE 服务之间的所有通信将使用 SSL 加密。  未选中此选项的话，你将无法看到任何有用信息。  接受 Fiddler 弹出的所有对话（应该有 5 个对话，包括 UAC）
1. 转至“WinConfig”、“全部豁免”和“保存更改”。  否则，Fiddler 将不会使用应用商店应用。
1. 现在，如果运行应用，你应该可以看到应用、运行时与 Xbox LIVE 之间的请求/响应。

若要调试 UWP 操作系统级别调用（通常不需要，但对调试登录和游戏内事件有帮助），你需要将 Fiddler 配置为捕获 WinHTTP 流量。
可通过以下方法完成此操作：
```cpp
    netsh winhttp set proxy 127.0.0.1:8888 "<-loopback>"
```
其中，端口 8888 匹配你在 Fiddlers“工具”|“选项”|“连接”选项卡上配置的端口（默认为 8888）。
若要撤销，请执行以下操作：
```cpp
    netsh winhttp reset proxy
```

## <a name="for-xbox-one-uwp-based-projects"></a>对于基于 Xbox One UWP 的项目

请按照此处的步骤[https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/uwp-fiddler](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/uwp-fiddler)

## <a name="for-xbox-one-xdk-based-projects"></a>对于基于 Xbox One XDK 的项目

在正常操作中，通过代理通信的主机面临通信被代理修改的风险，从而可能使玩家有机会作弊。 因此，主机设计为不允许通过代理通信。 将 Fiddler 用于 Xbox One 开发工具包需要你在开发工具包上执行某些特殊配置步骤，以便允许它使用 Fiddler 代理。

Fiddler 是一款免费软件，可从 [Fiddler 网站](http://www.telerik.com/fiddler/)下载。

Fiddler 可能影响主机所报告的网络状态。 如果从运行 Fiddler 的计算机禁用上游连接，则在主机的身份验证过期前，主机可能无法检测到此断开连接。 如果你使用的是 Fiddler，请确保断开主机与运行 Fiddler 的计算机之间的连接，而不是使用 Fiddler 模拟断开连接。 更好的是，使用网络压力工具，模拟断开连接，以便进行测试。

在开发电脑上安装并启用 Fiddler

请按照以下步骤安装并启用 Fiddler，以便从开发工具包监视流量。

1. 按照 [Fiddler 网站](http://www.telerik.com/fiddler/)上的指示在开发电脑上安装 Fiddler。
1. 启动 Fiddler，然后从“工具”菜单中选择“Fiddler 选项”。
1. 选择“连接”选项卡，并确保“允许远程计算机连接”复选框处于选中状态。
1. 单击“确定”来接受对设置的更改。 你将看到一个显示必须重启 Fiddler 才能使更改生效以及你可能需要手动配置防火墙的对话框。 单击此对话框上的“确定”，但不要重启 Fiddler。
1. 配置必要的防火墙规则来允许远程计算机连接。 启动 Windows 防火墙控制面板小程序。 单击“高级设置”，然后单击“入站规则”。 找到名为“FiddlerProxy”的规则并滚动到右侧，从而验证是否为该规则显示以下每项设置。

| 设置          | 首选值                |
|------------------|--------------------------------|
| 名称             | FiddlerProxy                   |
| 组            | （不要为组设置值） |
| 个人资料          | 全部                            |
| 已启用          | 是                            |
| 操作           | 允许                          |
| 替代         | 否                             |
| 程序          | fiddler.exe 的路径            |
| LocalAddress     | 任意                            |
| RemoteAddress    | 任意                            |
| 协议         | TCP                            |
| LocalPort        | 任意                            |
| RemotePort       | 任意                            |
| AllowedUsers     | 任意                            |
| AllowedComputers | 任意                            |


1. 将 Fiddler 配置为捕获和解密 HTTPS 流量。
    1. 若要实现最佳性能，请通过单击按钮栏上的“流”按钮来将 Fiddler 设置为使用流模式。
    1. 在 Fiddler 中，选择“工具”，然后再依次选择“Fiddler 选项”和“HTTPS”。
    1. 选中“解密 HTTPS 流量”复选框。 如果某个消息框询问是否将 Windows 配置为信任 CA 证书，请单击“否”。
    1. 单击“将根证书导出到桌面”按钮。
1. 退出 Fiddler 并重启。

### <a name="to-configure-a-dev-kit-to-use-fiddler-as-its-proxy-to-the-internet"></a>将开发工具包配置为使用 Fiddler 作为其 Internet 代理
已通过上一版本中使用的方法简化了开发工具包中的 Fiddler 配置。

1. 将你导出到桌面的 Fiddler 根证书作为 ``` xs:\Microsoft\Cert\FiddlerRoot.cer``` 复制到开发工具包。  可以使用以下命令：  ```xbcp [local Fiddler Root directory]\FiddlerRoot.cer xs:\Microsoft\Cert\FiddlerRoot.cer```
1. 创建一个名为 ```ProxyAddress.txt``` 的文本文件，使用运行 Fiddler 的开发电脑的 IP 地址或主机名称以及 Fiddler 在其中仅侦听文件中文件的端口号。 将名称/IP 地址和端口设置为以下格式：“主机:端口”。 （默认情况下，Fiddler 使用端口 8888。）例如，“10.124.220.250:8888”或“my_dev_pc.contoso.com:8888”。 将此文件作为 xs:\Microsoft\Fiddler\ProxyAddress.txt 复制到开发工具包。  可以使用以下命令：  ```xbcp [local Proxy Address file directory]\ProxyAddress.txt xs:\Microsoft\Fiddler\ProxyAddress.txt```
1. 通过将 ```xbreboot``` 键入命令提示符中来重启开发工具包

### <a name="to-stop-using-fiddler"></a>停止使用 Fiddler

若要停止将 Fiddler 用作 Internet 代理，以此跟踪 Fiddler 中所有开发工具包的网络流量，只需重命名或删除开发工具包中的 FiddlerRoot.cer 文件，然后重启开发工具包。

### <a name="how-it-works"></a>工作原理

启动时，xs:\Microsoft\Cert\FiddlerRoot.cer file 和 xs:\Microsoft\Fiddler\ProxyAddress.txt 文件的存在将会导致开发工具包将自身配置为使用 ProxyAddress.txt 中指定的代理服务器。 如果任一文件缺失，则开发工具包将不会将自身配置为通过 Fiddler 代理工作。

每台安装了 Fiddler 的电脑都使用不同的 Fiddler 根证书。 如果你拥有多台可用于为开发工具包提供 Fiddler 代理的电脑，则可以将每台电脑的 Fiddler 根证书复制到 xs:\Microsoft\Cert 目录，只要其中一个的名称为 FiddlerRoot.cer。 对 Fiddler 代理进行身份验证时，将会检查 Cert 目录中的所有证书。 电脑地址位于 ProxyAddress.txt 中的 Fiddler 实例将用作代理，前提是其证书位于 Cert 目录中。
