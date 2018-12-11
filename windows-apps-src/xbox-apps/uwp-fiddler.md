---
title: 在针对 UWP 进行开发时如何将 Fiddler 用于 Xbox One
description: 介绍如何使用免费 Fiddler 工具查看 UWP Xbox One 开发工具包上的网络流量。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 9c133c77-fe9d-4b81-b4b3-462936333aa3
ms.localizationpriority: medium
ms.openlocfilehash: c27891b47bb9f7774799c912cc6f4cae3cea92bc
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2018
ms.locfileid: "8900637"
---
# <a name="how-to-use-fiddler-with-xbox-one-when-developing-for-uwp"></a>在针对 UWP 进行开发时如何将 Fiddler 用于 Xbox One

Fiddler 是一种 Web 调试代理，用于记录 Xbox One 开发工具包和 Internet 之间的所有 HTTP 和 HTTPS 流量。 你将使用它记录和检查与 Xbox 服务和依赖方 Web 服务之间的流量，以便了解和调试 Web 服务调用。 

在正常操作中，通过代理通信的主机面临通信被代理修改的风险，从而可能使玩家有机会作弊。 因此，主机设计为不允许通过代理通信。 将 Fiddler 用于 Xbox One 开发工具包需要你在开发工具包上执行某些特殊配置步骤，以便允许它使用 Fiddler 代理。 

Fiddler 是一款免费软件，可从 [Fiddler 网站](http://www.fiddler2.com/fiddler2/)下载。 

Fiddler 可能影响主机所报告的网络状态。 如果从运行 Fiddler 的计算机禁用上游连接，则在主机的身份验证过期前，主机可能无法检测到此断开连接。 如果你使用的是 Fiddler，请确保断开主机与运行 Fiddler 的计算机之间的连接，而不是使用 Fiddler 模拟断开连接。

### <a name="to-install-and-enable-fiddler-on-your-development-pc"></a>在开发电脑上安装并启用 Fiddler
请按照以下步骤安装并启用 Fiddler，以便从开发工具包监视流量：

1. 按照 [Fiddler 网站](http://www.fiddler2.com/fiddler2/)上的指示在开发电脑上安装 Fiddler。 
2. 启动 Fiddler，然后从“工具”**** 菜单中选择“Fiddler 选项”****。 
3. 选择“连接”**** 选项卡，并确保“允许远程计算机连接”**** 处于选中状态。 
4. 单击“确定”**** 来接受对设置的更改。 你将看到一个显示必须重启 Fiddler 才能使更改生效以及你可能需要手动配置防火墙的对话框。 单击此对话框上的“确定”****，但*不要重启 Fiddler*。
5. 配置必要的防火墙规则来允许远程计算机连接。 启动 Windows 防火墙控制面板小程序。 单击“高级设置”****，然后单击“入站规则”****。 找到名为“FiddlerProxy”的规则并滚动到右侧，从而验证是否为该规则显示下表中的每项设置。
  
  | 设置           | 首选值                |
  | ----              | ----                           |
  | 名称              | FiddlerProxy                   |
  | 组             | *没有值* |
  | 配置文件           | 全部                            |
  | 已启用           | 是                            |
  | 操作            | 允许                          |
  | 替代          | 否                             |
  | 程序           | *fiddler.exe 的路径*          |
  | LocalAddress      | 任意                            |
  | RemoteAddress     | 任意                            |
  | 协议          | TCP                            |
  | LocalPort         | 任意                            |
  | RemotePort        | 任意                            |
  | AllowedUsers      | 任意                            |
  | AllowedComputers  | 任意                            |


6. 通过执行以下操作将 Fiddler 配置为捕获和解密 HTTPS 流量：
  1. 若要实现最佳性能，请通过单击按钮栏上的“流”**** 按钮来将 Fiddler设置为使用流模式。
  2. 在 Fiddler“工具”**** 菜单中，选择“Fiddler 选项”****，然后单击“HTTPS”****。
  3. 选中“解密 HTTPS 流量”**** 复选框。 如果某个对话框询问是否将 Windows 配置为信任 CA 证书，请单击“否”****。
  4. 单击“将根证书导出到桌面”****。
7. 退出并重启 Fiddler。

### <a name="to-configure-a-dev-kit-to-use-fiddler-as-its-proxy-to-the-internet"></a>将开发工具包配置为使用 Fiddler 作为其 Internet 代理

1. 导航到 Xbox Device Portal UI 中的“网络”**** 工具。
2. 浏览你导出到桌面的 Fiddler 根证书。 
3. 键入运行 Fiddler 的开发电脑的 IP 地址或主机名。
4. 键入 Fiddler 正在侦听的端口号（默认情况下，Fiddler 使用端口 8888）。 
5. 单击“启用”****。 这将重启你的开发工具包。

### <a name="to-stop-using-fiddler"></a>停止使用 Fiddler
若要停止使用 Fiddler 作为 Internet 代理（并使 Fiddler 停止跟踪所有开发工具包的网络流量），请执行以下操作：

1. 导航到 Xbox Device Portal UI 中的“网络”**** 工具。
2. 单击“禁用”****。

> [!NOTE]
> 每台安装了 Fiddler 的电脑都使用不同的 Fiddler 根证书。 如果你有多台电脑可用于为开发工具包提供 Fiddler 代理，你将需要在它们之间进行切换时选择新的根证书。 如果你只使用一台电脑，则只需在首次启用 Fiddler 时选择根证书。 你仍然必须指定 IP 地址和端口。

## <a name="see-also"></a>另请参阅
- [Fiddler 设置 API 参考](wdp-fiddler-api.md)
- [常见问题](frequently-asked-questions.md)
- [Xbox One 上的 UWP](index.md)



