---
author: laurenhughes
title: 从网页中安装 UWP 应用
description: 在此部分中，我们将查看允许用户直接从网页中安装应用所需执行的步骤。
ms.author: lahugh
ms.date: 11/16/2017
ms.topic: article
keywords: windows 10, uwp, 应用安装程序, AppInstaller, 旁加载, 相关集, 可选包
ms.localizationpriority: medium
ms.openlocfilehash: 98a761bf04b56d13745f2505b8d0806fc4fdf3e1
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2018
ms.locfileid: "7422437"
---
# <a name="installing-uwp-apps-from-a-web-page"></a>从网页中安装 UWP 应用

通常，在使用应用安装程序安装应用之前，应用需要在设备上本地可用。 对于 Web 方案，这意味着用户必须从 Web 服务器中下载应用包，然后才能使用应用安装程序安装该应用包。 这样做效率低下并且浪费磁盘空间，这就是应用安装程序现在具有用于简化过程的内置功能的原因。

应用安装程序可以直接从 Web 服务器安装应用。 当用户单击应用包托管 Web 链接时，将会自动调用应用安装程序。 之后，用户会转到应用安装程序中的应用信息视图，然后只需一次单击即可直接使用应用。 

只能在 Windows 10 Fall Creators Update 和更高版本中直接安装应用。 [以前版本的 Windows 10 的 Web 安装体验](#web-install-experience)将支持以前版本的 Windows（回退到 Windows 10 周年更新）。 此体验并不像直接安装应用那样顺畅，但它对现有的应用安装过程进行了重大改进。
  
> [!NOTE]
> 若要支持新功能，需要版本大于 1.0.12271.0 的应用安装程序。

## <a name="protocol-activation-scheme"></a>协议激活方案
在此机制中，应用安装程序会针对协议激活方案向操作系统注册。 当用户单击 Web 链接时，浏览器会向操作系统核实是否有注册到该 Web 链接的应用。 如果该方案与应用安装程序指定的协议激活方案匹配，则调用应用安装程序。 请务必注意，此机制独立于浏览器。 这对站点管理员有好处，例如，对将此机制合并到网页时不需要考虑 Web 浏览器差异的站点管理员有好处。 

### <a name="requirements-for-protocol-activation-scheme"></a>协议激活方案的要求

1. Web 服务器需要支持字节范围请求 (HTTP/1.1)
    - 支持 HTTP/1.1 协议的服务器都应具有支持字节范围请求 
2. Web 服务器将需要知道的有关 Windows 10 应用包的内容类型
    - 下面介绍了如何将新的内容类型声明为[web 配置文件](web-install-IIS.md#step-7---configure-the-web-app-for-app-package-mime-types)的一部分

### <a name="how-to-enable-this-on-a-webpage"></a>如何在网页上启用它 
想要在其网站上托管应用包的应用开发人员需要执行以下步骤：

在应用包 URL 前面加上激活方案 `'ms-appinstaller:?source='`，在网页上引用这些 URI 时,应用安装程序会注册到此前缀。 有关详细信息，请参阅 **MyApp 网页**示例。 
``` html
<html>
    <body>
        <h1> MyApp Web Page </h1>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubApp.appx"> Install app package </a>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubAppBundle.appxbundle"> Install app bundle  </a>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubAppSet.appinstaller"> Install related set </a>
    </body>
</html>
```

## <a name="signing-the-app-package"></a>对应用包进行签名
为了让用户安装你的应用，你需要用受信任的证书对应用包签名。 你可以使用来自受信任的证书颁发机构的第三方付费证书来对应用包进行签名。 如果使用第三方证书，用户需要将其设备设为旁加载或开发人员模式，才能安装和运行你的应用。

如果要将应用部署到企业内的员工，可以使用企业颁发的证书对应用签名。 请务必注意，必须将企业证书部署到要安装该应用的所有设备。 有关部署企业应用的详细信息，请参阅[企业应用管理](https://docs.microsoft.com/windows/client-management/mdm/enterprise-app-management)。

## 在 Windows 10 早期版本中的 Web 安装体验<a name="web-install-experience"></a>

在应用安装程序可用的所有版本的 Windows 10（从周年更新开始）上支持从浏览器调用应用安装程序。 但是，只有 Windows 10 Fall Creators Update 中才有无需先下载程序包即可直接从 Web 安装的功能。  

以前版本的 Windows 10（具有可用的应用安装程序）的用户也可以通过应用安装程序来利用 UWP 应用的 Web 安装功能，但将具有不同的用户体验。 当这些用户单击 Web 链接时，应用安装程序将提示**下载**程序包，而不是提示**安装**。 下载后，应用安装程序将自动发起已下载程序包的启动。 由于应用包是从 Web 中下载的，因此这些文件将通过 Microsoft SmartScreen 进行安全检查。 一旦用户提供继续操作的权限，然后再一次单击**安装**，该应用即可使用！

虽然此流程并不像在 Windows 10 Fall Creators Update 上直接安装那么完美，但用户仍然可以快速使用该应用。 另外，使用此流程，用户不必担心应用包文件会不必要地占用驱动器空间。 应用安装程序通过将程序包下载到其应用数据文件夹并在不再需要时清除程序包，来有效地管理空间。 

以下是 Windows 10 Fall Creators Update 版本的应用安装程序与以前版本的应用安装程序之间的快速比较：

| 最新版本的应用安装程序 | 以前版本的应用安装程序 |
|------------------------------|----------------------------------|
| 应用安装程序在开始下载之前显示应用信息 | 浏览器提示用户选择下载  |
| 应用安装程序执行下载 | 用户必须手动发起应用包的启动 |
| 下载程序包后，应用安装程序会自动启动应用包 | 用户必须单击**安装**，然后手动启动应用包 |
| 应用安装程序将处理已下载的程序包 | 用户必须手动删除已下载的文件 |

## <a name="web-install-experience-on-previous-versions-of-windows-10"></a>以前版本的 Windows 10 的 Web 安装体验 
在 Windows 10 Fall Creators Update 之前的版本中，应用安装程序无法直接从 Web 中安装应用。 在这些版本中，应用安装程序只能安装本地可用的应用包。 相反，应用安装程序将下载程序包，并且要求用户双击下载的程序包以进行安装。


## <a name="microsoft-smartscreen-integration"></a>Microsoft SmartScreen 集成

Microsoft SmartScreen 始终是通过应用安装程序安装应用的安装过程的一部分。 SmartScreen 可以确保保护用户，使其不接触可能会进入用户设备的恶意内容。 借助应用安装程序的最新更新，SmartScreen 集成更加完美和可靠，可在安装未知应用时提供警告，并且会保护设备免受损害。 
