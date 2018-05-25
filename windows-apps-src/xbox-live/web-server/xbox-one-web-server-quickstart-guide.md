---
title: Xbox One Web 服务器快速入门指南
author: KevinAsgari
description: 了解如何设置 Xbox One Web 服务器。
ms.assetid: 2f6831ab-2dea-4f21-bb32-39cb4de4799c
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one, web 服务器
ms.localizationpriority: low
ms.openlocfilehash: e979767f127ad146c413a26266443273fe74cee4
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="xbox-one-web-server-quickstart-guide"></a>Xbox One Web 服务器快速入门指南

> [!NOTE] 
> 本主题是一个高级主题，需要访问权限受限制的站点，它们仅向托管的合作伙伴和 ID@Xbox 成员开放。

为 Xbox One 开发设置简单的 Web 服务器的分步指南。

-   [简介](#introduction)
-   [服务器设置](#server-setup)
-   [为部署构建 SimpleAuthService 示例网站](#build-sample-website)
-   [配置 IIS 与运行示例网站](#configure-sample-website)
-   [验证信赖方和 XSTS 令牌设置](#verify-tokens)
-   [为 Web 服务启用 HTTPS](#enable-https)
-   [常见问题和疑难解答](#faqs-and-troubleshooting)


## <a name="introduction"></a>简介

Xbox One 和 Xbox Live 平台非常依赖 RESTful HTTPS 通信的使用。 而且在 Xbox One 上，开发人员可以通过 HTTPS 而不是 Xbox Live 服务器平台 (XLSP) 连接设置其游戏服务器，这是 Xbox 360 的最初要求。 这可以扩展游戏提供快速可靠的灵活游戏服务的能力。

在本快速入门指南中，你将了解如何使用游戏开发人员网络 (GDN) 提供的简单 Web 服务器示例 ("SimpleAuthService")，为你的 Xbox One 游戏设置工作 HTTP/HTTPS Web 服务。 本指南将指导你设置运行示例 Web 服务的 Windows 服务器，以便你可以开始探索为 Xbox One 游戏与服务器之间的通信使用 Xbox 安全令牌服务 (XSTS) 和 HTTPS。 另外还提供通过 Microsoft Azure 快速轻松地设置为服务器设置新虚拟机的步骤。


## <a name="server-setup"></a>服务器设置

GDN 上为运行 Xbox One Web 服务提供的信赖方 SDK 和示例可用于 Windows Server 2008 R2 或更高版本。 后面的说明专门针对设置运行 Windows Server 2012 R2 的服务器进行了调整。 所需设置在各版本中本质上相同，但从何处访问某些服务器设置功能可能略有不同。 你可以使用 Windows Server 软件配备的物理计算机作为服务器，或者通过 Azure 设置虚拟机。

-   [Azure 虚拟机设置](#azure-virtual-machine-setup)
-   [服务器角色和功能设置](#server-roles-and-features-setup)
-   [证书安装](#certificate-installation)


### <a name="azure-virtual-machine-setup"></a>Azure 虚拟机设置

如果你有权访问 Azure，设置用于开发和测试的虚拟服务器很简单，也很快。 只需按照以下步骤进行操作：

1. 使用你的帐户登录到 [Azure 门户](https://manage.windowsazure.com/)。

1. 选择**虚拟机**并单击 **+新建**。

1. 选择**快速创建**。

1. 为你的服务输入 DNS 名称。 附加上 ".cloudapp.net" 后，这将成为你用于对服务器进行 HTTP 调用的域名。

1. 选择 Windows Server 2012 R2 和“大小设置 A1”（你也可以选择其他设置，但 A1 比较适合此示例）。

1. 在虚拟机中输入你将用于远程桌面连接 (RDC) 的用户名和密码；并选择你希望服务器运行的地区（最好靠近你的物理位置）。

1. 要完成设置，单击**创建虚拟机**。

1. 等待预配稍后完成，然后单击计算机名称打开 Azure 配置设置。

1. 选择**终结点**选项卡。

1. 单击 **+添加**选择**添加独立终结点**。

1. 从**名称**下拉列表中选择 **HTTP (TCP 端口 80)**，然后单击**确定**（或选中标记按钮）。

| 注意                                                                                                                |
|----------------------------------------------------------------------------------------------------------------------------------|
| 请勿选择“启用直接服务器返回”选项，这样做实际上会阻止传入流量进入服务器。 |

1. 添加了 HTTP 终结点后，为 HTTPS 添加另一个终结点：
 1.  选择**终结点**选项卡。
 1.  单击 **+添加**选择**添加独立终结点**。
 1.  从**名称**下拉列表中选择 **HTTPS (TCP 端口 443)**，然后单击**确定**（或选中标记按钮）。

1. 转到**仪表板**选项卡，请确保已启动虚拟机，并选择**连接**打开 .rdp 文件，然后使用 RDC 连接到计算机。
1. 使用你在步骤 6 中设置的用户名和密码登录计算机。

| 注意                                                                                                     |
|-----------------------------------------------------------------------------------------------------------------------|
| 如果你使用的是加入域的计算机，你可能需要使用 "\\UserNameYouChose" 直接登录到计算机。 |


### <a name="server-roles-and-features-setup"></a>服务器角色和功能设置

运行 Azure 虚拟机或在物理计算机上安装了 Windows Server 后，你需要将计算机变为运行 Internet 信息服务 (IIS) 的 Web 服务器。 为此，你需要向服务器添加角色：

1.  从**开始**菜单打开“服务器管理器”。
2.  单击**管理**，然后选择**添加角色和功能**。
3.  从选项中选择**基于角色**或**基于功能**安装，然后单击**下一步**。
4.  在“服务器池”列表中突出显示你的服务器，然后单击**下一步**。
5.  从“服务器角色”列表中，选择“应用程序服务器”和“Web 服务器(IIS)”，然后单击“下一步”。
6.  在**功能**列表中，在同一组中，选择 **.NET Framework 4.5 功能**下的 **ASP.NET 4.5**，以及 **WCF 服务**下的 **HTTP 激活**。
7.  单击**下一步**直到到达“应用程序服务器角色服务”屏幕。
8.  在弹出的窗口中选择 **Web 服务器(IIS)支持**和**添加功能**。
9.  单击**下一步**直到到达 **Web 服务器角色(IIS)服务**屏幕。
10. 保留选中的所有默认值，单击**下一步**。
11. 检查将要安装的项目列表，然后单击**安装**。
12. 等待所有软件和服务安装完成。


### <a name="certificate-installation"></a>证书安装

你需要使用 Xbox 安全令牌服务 (XSTS) 令牌安装以下证书，以让 Xbox One 主机对你的服务器进行 Web 服务调用：

-   [XSTS 签名证书](https://developer.xboxlive.com/_layouts/xna/download.ashx?file=xsts.auth.xboxlive.com.cer.zip&folder=platform/RelyingParty)
    -   下载证书。
    -   将证书安装到“本地计算机/个人”和“本地计算机/受信任人”证书存储中。

| 重要提示                                                                                                                                                                                                                                                                                                                                                   |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 从 MMC 将证书指纹直接粘贴到 web.config 文件可能会向指纹添加不可见的额外 Unicode 字符。 有关此问题及其解决方法的详细信息，请参阅 [MMC 证书管理单元中显示的证书指纹包含不可见的额外 Unicode 字符](http://support.microsoft.com/kb/2023835?wa=wsignin1.0)。 |

-   你的服务的信赖方证书
    -   信赖方证书的公钥通过 XDP 上载。 在 XDP 中，导航到你的沙盒，单击**管理**，然后单击 **Web 服务**。 在 **Web 服务**页上，选择现有 Web 服务或创建新 Web 服务。 为 Web 服务创建了终结点后，必须添加新的令牌定义，然后上载密钥。 有关在 XDP 中设置 Web 服务的详细信息，请参阅 [XDP 文档](https://developer.xboxlive.com/en-us/xdp/documentation/xdphelp/Pages/AboutWebServiceManagement_SS_xdpdocs.aspx)。
    -   将证书安装到“本地计算机/个人”证书存储中。

此外，如果你的 Web 服务将代表用户（委派的身份验证）直接与 Xbox Live 服务通信，你需要以下证书：

### <a name="business-partner-certificate"></a>业务合作伙伴证书

你或你的发布者按照 Xbox One 服务配置工作簿的“业务合作伙伴证书”选项卡上列出的步骤创建该证书。 工作簿还包括了创建证书后必须向 Microsoft 提供的证书信息的相关指南。
-   将证书安装到“本地计算机/个人”证书存储中。

| 注意                                                                                                                                                                                                                                |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 如果你还没有信赖方或业务合作伙伴证书，请按照服务配置工作簿中的说明进行操作，并与你的开发人员客户经理联系，以安排向 Microsoft 提交证书。 |

确保证书安装在正确位置并向私钥提供适当权限的最简单方法是通过 Microsoft 管理控制台 (mmc.exe)：

1.  从“开始”菜单打开 mmc.exe。
2.  从**文件**菜单中，单击**添加/删除管理单元**。
3.  选择**证书**，并单击**添加**。
4.  选择**计算机帐户**，然后单击**下一步**。
5.  确保选中了本地计算机，并单击**完成**。
6.  单击**确定**。
7.  展开**证书(本地计算机)** 列表。
8.  打开你想要将证书导入其中的文件夹或证书存储。
9.  选择**操作**-&gt;**所有任务**-&gt;**导入**。
10. 通过为证书指定 .pfx 或 .cer 文件按照证书导入向导操作。

| 注意 |
|------|
| .cer 是文件选取器中的默认文件类型。 若要安装信赖方或业务合作伙伴证书，你可能需要将视图更改为**所有文件** (\*.\*)。 |

此外，你的信赖方和业务合作伙伴证书将包含 IIS 服务需要读取以处理 XSTS 令牌的私钥。 若要给予 IIS 服务访问私钥的权限，请从 mmc.exe 内执行以下操作：

1.  右键单击证书名称。
2.  选择**所有任务**-&gt;**管理私钥**。
3.  单击**添加**。
4.  添加 \[Local Computer Name\]\\SERVICE 帐户，然后单击**确定**。
5.  确保“服务”帐户具有读取访问私钥的权限。
6.  单击**确定**。


## <a name="building-the-simpleauthservice-sample-website-for-deployment-a-namebuild-sample-website"></a>为部署构建 SimpleAuthService 示例网站<a name="build-sample-website">

配置服务器并验证 IIS 已正确设置后，你可以设置示例，并进行编译以在你的 Web 服务器上运行。

1.  请确保已安装 Visual Studio 2012。
2.  从 GDN 上的 [XDK 软件下载](https://developer.xboxlive.com/en-us/platform/development/downloads/Pages/home.aspx)页面下载最新的 Xbox One 简单 Web 服务器示例 ("SimpleAuthService")。
3.  从 GDN 上的软件下载页面下载 Xbox One 信赖方 SDK。
4.  在 Visual Studio 中打开 SimpleAuthService 项目。
5.  安装[用于处理 JSON Web 令牌 (JWT) 的 IdentityModel](https://www.nuget.org/packages/Microsoft.IdentityModel.Tokens.JWT)。
6.  右键单击 SimpleAuthService 项目，然后选择**添加引用**。
7.  选择**浏览**并定位到从信赖方 SDK 解包的 Microsoft.XboxLive.Auth.dll。
8.  选择**确定**。
9.  打开 Web.config 文件，找到 audienceUris 节点。
10. 更改 <http://Your_Relying_Party.com/> 以体现你的发布者或服务的信赖方名称。
11. 为进行测试以确保网站处于活动状态，通过将 directoryBrowse 设置为 true 在 Web.config 文件中暂时启用目录浏览。 此设置的默认值为 false：
12. 编译解决方案以验证它是否成功。 如果成功，你现在可以发布服务了。
13. 右键单击 **SimpleAuthService** 项目，然后选择**发布**。
14. 从下拉列表中选择&lt;新建配置文件...&gt;。
15. 将你的发布配置文件命名为 **SimpleAuthService**，单击**下一步**。
16. 在“连接”页上，从下拉列表中选择**文件系统**。
17. 在开发计算机上将目标位置设置为新文件夹，然后单击**下一步**。
18. 为配置选择**调试**，以便你以后可以根据需要调试站点，然后单击**发布**。
19. 将步骤 16 中输出文件夹中的生成文件复制到服务器上的 C:\\SampleService\\ 文件夹。


## <a name="configuring-iis-and-running-the-sample-website-a-nameconfigure-sample-website"></a>配置 IIS 与运行示例网站<a name="configure-sample-website">

你必须先在 IIS 中配置服务，然后才能够让控制台与你的 Web 服务器通信。 为简单起见，首先从运行 SimpleAuthService 示例开始。 以下步骤将帮助你通过 IIS 为该示例设置 HTTP/HTTPS Web 服务。

1.  打开“Internet 信息服务 (IIS) 管理器”。
2.  展开服务器列表，右键单击**应用程序池**；然后选择**添加新池**。
3.  将池命名为 "SampleServicePool"，并设置为 .NET Framework 4.0。
4.  展开**站点**列表，选择**默认网站**。
5.  在**管理网站**下，选择**停止**。
6.  现在右键单击**站点**并选择**添加网站…**
7.  将站点命名为 "SampleService"，将应用程序池设置为 SampleServicePool，将物理路径设置为 C:\\SampleService\\。
8.  保留默认绑定为端口 80，单击**确定**，然后在绑定警告上单击**是**。
9.  从管理员命令提示符运行以下命令：

           %windir%\Microsoft.Net\Framework64\v4.0.30319\aspnet_regiis.exe -ir

10. 在“站点”下，确保默认网站已停止，并重启你的 Web 服务。

此时，你的服务器应设置为 HTTP 流量。 在继续启用 HTTPS 之前 - 这是任何 Xbox One 零售环境中所有基于 Web 的通信的要求 - 确保系统仅使用你在 IIS 中配置的站点上启用的 HTTP 非常重要。 这将帮助你更好地解决启用 HTTPS 时可能会出现的问题，本文稍后有述。

若要验证目前是否一切正常，请尝试访问 `http://localhost/`，确认你能够看到所部署的 SampleService 网站的目录信息。 如果你的服务器此时可通过公用 Internet 访问，你可能需要关闭目录浏览，方法是在 Web.config 文件中将 directoryBrowse 设置重新更新为 false。 这将阻止未经授权的访问和发现你的服务：

      <directoryBrowse enabled=?false?>

现在，你知道 IIS 已经设置完成，请尝试从服务器调用一项服务，并验证你可以收到 .json 响应：

      http://localhost/RESTService.svc/messageoftheday


## <a name="verifying-relying-party-and-xsts-token-setup-a-nameverify-tokens"></a>验证信赖方和 XSTS 令牌设置<a name="verify-tokens">

既然你已经能够通过服务器上的 HTTP 运行所部署的服务了，下一步是使用 XSTS 令牌从 Xbox One 主机进行调用，并验证证书是否已正确安装。

从 [GDN 上的示例页面](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx)为 Xbox One 下载 Web 服务示例代码，并将它提取到安装了 Xbox One XDK 的开发计算机上。
打开 \\live\\WebServices\\WebServices110.sln 文件。
打开 WebServices.cpp 文件。
将 MYSERVERNAME 定义更新为测试服务器的域（例如，yourserver.cloudapp.net）。
打开 Package.Appxmanifest。
更新 TitleID 和 PrimaryServiceConfigID 值，使其与开发游戏的值一致。
确保你的 Xbox One 开发控制台 (devkit) 位于设置信赖方证书的沙盒内。
在你的 devkit 上使用开发者帐户之一登录。
打开 NSAL.json 文件，将其更新为以测试服务器主机和信赖方为目标（如果你还没有为信赖方证书设置网络安全授权列表 (NSAL) 设置）。

有关配置 NSAL.json 文件的详细信息，请参阅娱乐开发人员论坛上的这篇帖子：如何配置 NSAL.json 文件。

1.  运行 SetNSAL.bat 命令将所需的 NSAL 文件复制到你的 devkit 中。

编译和运行解决方案。
按 **Y** 按钮对“人脉服务”进行调用，并验证客户端示例在正确运行。
按 **A** 和 **X** 按钮对你的测试 Web 服务进行调用。


## <a name="enabling-https-for-your-web-service-a-nameenable-https"></a>为 Web 服务启用 HTTPS<a name="enable-https">

-   [导出自签名证书的公钥](#export-key)
-   [在 IIS 中为 Web 服务创建 HTTPS 绑定](#create-https-binding)
-   [在你的 Xbox One 主机上安装自签名证书](#install-certificate)

在验证了服务器已设置并通过 HTTP 端到端使用 Xbox One 主机后，更新服务器设置以接受 HTTPS 流量。 如之前说明的，从主机到 Xbox One 上的任何零售环境的基于 Web 的所有流量均需要 HTTPS。

如果你已经通过 Azure 设置了服务器，服务器已准备好了证书。 若要对此进行验证，请执行以下操作：

1.  从“开始”菜单打开“Internet 信息服务 (IIS) 管理器”。
2.  从“连接”页面选择服务器名称。
3.  打开**服务器证书**。
4.  确认列表中存在包含你的服务器域（例如，server.cloudapp.net）的证书。
5.  选择该证书，并单击**查看...**
6.  选择证书路径，并确认其中包含你的服务器的 DNS 名称（例如，server.cloudapp.net）。

如果你未使用 Azure 或你需要为 SSL 通信创建证书，你可以按照本文中的说明创建自签名证书：

[操作方法：创建在部署期间使用的临时证书](http://msdn.microsoft.com/en-us/library/ms733813(v=vs.110).aspx)

请确保在创建证书时为证书名称使用完整的服务器 DNS 名称（如 server.contoso.com）。 否则证书名称和服务器名称不完全相同，主机将不信任该证书。 证书准备就绪后，与安装服务器设置中的其他证书一样，将其安装在计算机的个人证书存储中。


### <a name="export-the-self-signed-certificates-public-key-a-nameexport-key"></a>导出自签名证书的公钥<a name="export-key">

1.  从“开始”菜单打开 mmc.exe。
2.  从**文件**菜单中，单击**添加/删除管理单元**。
3.  选择**证书**，并单击**添加**。
4.  选择**计算机帐户**，然后单击**下一步**。
5.  确保选中了本地计算机，并单击**完成**。
6.  单击**确定**。
7.  展开“证书(本地计算机)\\个人\\证书”列表。
8.  右键单击你之前创建（或已在你的计算机上创建）的自签名 SSL 证书的名称。
9.  选择**所有任务**和**导出…**
10. 在证书导出向导中，单击**下一步**。
11. 选择**不，不要导出私钥**，然后单击**下一步**。
12. 选择 **Base-64 编码的 X.509 (.CER)** 并单击**下一步**。
13. 使用服务器可识别的名称为证书文件命名，单击**下一步**，然后单击**完成**。
14. 将生成的 .cer 文件复制到你的开发计算机。


### <a name="create-an-https-binding-for-your-web-service-in-iis-a-namecreate-https-binding"></a>在 IIS 中为 Web 服务创建 HTTPS 绑定<a name="create-https-binding">

1.  在 IIS 管理器中打开你的服务网站。
2.  单击**绑定**。
3.  选择**添加**。
4.  从下拉列表中选择 **https**。
5.  从 SSL 证书下拉列表中选择你的自签名证书。
6.  单击**确定**。

你现在应该可以试运行 https://localhost 了，你将在 Internet Explorer 中收到一个警告，提示网站证书有问题。 这只是因为证书是自签名的，没有链接到受信任的证书颁发机构 (CA)。 在发布游戏之前，你必须为通过受信任的 CA 请求的域设置具有真实证书的 Web 服务。

如果你尝试转到 https://localhost/RESTService.svc/messageoftheday，你将收到一条错误消息。 原因是你还必须更新该服务的 Web.config 文件以支持 HTTPS 请求。 若要执行此操作，取消以下设置的注释、保存 Web.config 文件，然后重试此链接：

      <!-- Uncomment this block to enable HTTPS

      <endpoint behaviorConfiguration="webHttpBehavior" binding="webHttpBinding"
      bindingConfiguration="webHttpsBindingConfig" contract="SimpleAuthService.IRESTService" /> -->

既然你已经可以使用 Internet 浏览器通过 HTTPS 访问你的服务了，最后一步就是获得 devkit 以通过 HTTPS 进行服务调用。 如果你的服务器使用链接到某个受信任 CA 的 SSL 证书，那么你现在就应该能够调用 Web 服务的 HTTPS 版本了，无需任何其他步骤。 或者，如果你为 SSL 通信创建了自签名证书，如上所述，你将需要获取证书并将它安装在主机上；否则在尝试通过 HTTPS 与服务器通信时，主机不会信任该服务器。 由于主机在关闭时会擦除所有临时证书，你将需要在每次重启主机时重新安装证书。


### <a name="install-the-self-signed-certificate-on-your-xbox-one-console-a-nameinstall-certificate"></a>在你的 Xbox One 主机上安装自签名证书<a name="install-certificate">

1.  打开 Xbox One XDK 命令提示符，导航到你保存服务器的 SSL 证书的位置。
2.  运行以下命令（使用 .bat 文件是快速重新安装证书的简单方法）：

        xbcp "%DurangoXDK%\bin\setproxy.exe" xd:\
        xbcp "%DurangoXDK%\bin\certmgr.exe" xd:\
        xbcp [YourSSLCert].cer xd:\
        xbrun /o d:\certmgr.exe -add -all d:\[YourSSLCert].cer -s -r localmachine root

最后一步是更新 NSAL.json 文件以包服务的 HTTPS 定义，方法很简单，只需复制当前的 HTTP 定义，并将协议值更新为 *HTTPS*。


## <a name="faqs-and-troubleshooting"></a>常见问题和疑难解答

 **当客户端示例尝试调用我们的服务时，收到来自 GetTokenAndSignatureAsync() 的错误。 我如何解决这些错误？**   
大部分来自主机的 NSAL 或令牌相关错误都是以 0x87DD\* 开头。 [XboxLive.com 上的这篇论坛文章](https://forums.xboxlive.com/AnswerPage.aspx?qid=10ef67c1-db3a-4f29-b4bf-3eacd77313cb&tgt=1)提供了最常见错误的列表及其解决方法。

 **在尝试访问 Web 服务 URL 时，我收到了 HTTP 错误 404.3。 这是什么原因导致的？应该如何解决？**   
在最初尝试使用示例设置服务器时，404 错误通常指示缺少 .NET 3.5、4.5 或 ASP.NET 组件。 请确保在“添加角色和功能向导”中启用了以下服务角色（功能）：

-   .NET 3.5 下的 HTTP 激活 (Windows Server 2008)
-   .NET 4.5 下的 ASP.NET 4.5
-   .NET 4.5 / WCF 服务下的 HTTP 激活

如果你运行的是 Windows Server 2008 R2，请从管理员命令提示符运行以下命令：

      %windir%\"Microsoft.NET\Framework\v3.0\Windows Communication Foundation\ servicemodelreg" –i

 **Windows Server 2008 R2 - 配置错误：无法读取配置节 "system.serviceModel"，因为它缺少节声明。**   
安装 .NET 3.5。 有关详细信息，请参阅 [MSDN 上的这篇文章](http://blogs.msdn.com/b/sqlblog/archive/2010/01/08/how-to-install-net-framework-3-5-sp1-on-windows-server-2008-r2-environments.aspx)。

 **Windows Server 2008 R2 - HTTP 错误 500.21：处理程序 "svc-Integrated" 的模块列表中存在错误模块 "ManagedPipelineHandler"。**   
从管理员命令提示符运行：

    %windir%\Microsoft.NET\Framework\v4.0.21006\aspnet_regiis.exe –i

 **HTTP 错误 500.19 和 0x80070021 是什么错误？**   
你服务器上的处理程序部分目前已锁定。 从管理员命令提示符运行以下命令应该可以解决此错误：

    %windir%\system32\inetsrv\appcmd.exe unlock config "Default Web Site/exchange" -section:handlers -commitpath:apphost

 **在尝试从主机中通过 HTTPS 调用我的 Web 服务时，我收到了 HRESULT 错误 0x800c0019。 为什么？**   
这意味着你用于 HTTPS 流量的 SSL 证书不受主机信任，或在服务器上配置错误。 首先，请确保在双击导出的 .cer 文件时，你在服务器上创建的用于 SSL 的证书在“颁发对象”值中包含你的服务器的完整域名。 示例：颁发给：Server.Contoso.com。然后，请确保在导出 .cer 文件时选择了“Base-64”选项。 最后，你可以查看 IIS，确保 HTTPS 端口 (443) 使用的是你导出的证书，而不是其他证书。 此外，请确保在你的 devkit 上重新安装该证书。
