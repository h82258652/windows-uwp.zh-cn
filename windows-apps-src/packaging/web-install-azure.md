---
author: c-don
title: 从 Azure Web 服务器安装 UWP 应用
description: 阅读此教程，了解如何设置 Azure Web 服务器、如何验证 Web 应用可以托管应用包，以及如何有效调用和使用应用安装程序。
ms.author: cdon
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 应用安装程序, AppInstaller, 旁加载, 相关集, 可选包, Azure web 服务器
ms.localizationpriority: medium
ms.openlocfilehash: b98ca6316f733210dbdbc5201178b3a89a2b5982
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2018
ms.locfileid: "4537501"
---
# <a name="install-a-uwp-app-from-an-azure-web-app"></a>从 Azure Web 应用安装 UWP 应用

通过应用安装程序，开发人员和 IT 专业人员可以通过在各自的内容分发网络 (CDN) 上托管应用的方式来分发 Windows 10 应用。 这种方式适用于不希望或不需要将应用发布到 Microsoft Store，但仍希望利用 Windows 10 打包和部署平台的企业。

本主题概述了配置 Azure Web 服务器以托管 UWP 应用包的步骤，并介绍如何使用应用安装程序来安装应用包。

在此教程中，将介绍如何设置 IIS 服务器以在本地验证 Web 应用程序能否正常托管应用包，并有效地调用和使用应用安装程序。 我们还另外提供了教程，介绍如何在外部的常用云 Web 服务（Azure 和 AWS）中正确托管 Web 应用程序，以确保其满足应用安装程序的 Web 安装要求。 此分步教程不需要任何专家知识，很容易学习。 

## <a name="setup"></a>设置

要成功学习此教程，你将需要以下内容：
 
1. Microsoft Azure 订阅 
2. UWP 应用包 - 要分发的应用包

可选：GitHub 上的[初学者项目](https://github.com/AppInstaller/MySampleWebApp)。 如果没有应用包或网页可以使用，但仍想学习如何使用此功能，这个初学者项目很有用。

### <a name="step-1---get-an-azure-subscription"></a>步骤 1 - 获得 Azure 订阅
若要获得 Azure 订阅，请访问 [Azure 帐户页面](https://azure.microsoft.com/free/)。 可以使用免费会员资格获得此教程。

### <a name="step-2---create-an-azure-web-app"></a>步骤 2 - 创建 Azure Web 应用 
在 Azure 门户页面中，单击 **+ 创建资源**按钮，然后选择 **Web 应用**

![创建](images/azure-create-app.png)

创建一个唯一的**应用名称**，并将其余字段保留为默认值。 单击**创建**完成 Web 应用创建向导。 

![创建 Web 应用](images/azure-create-app-2.png)

### <a name="step-3---hosting-the-app-package-and-the-web-page"></a>步骤 3 - 托管应用包和网页 
创建 Web 应用之后，可从 Azure 门户上的仪表板访问该应用。 在此步骤中，将使用 Azure 门户的 GUI 创建一个简单的网页。

从仪表板中选择新建的 Web 应用之后，使用搜索字段查找并打开**应用服务编辑器**。 

编辑器中有一个默认的 `hostingstart.html` 文件。 在文件资源管理器面板的空白区域右键单击，并选择**上传文件**开始上传应用包。

> [!NOTE]
> 如果没有可用的应用包，可以使用 GitHub 上提供的[初学者项目](https://github.com/AppInstaller/MySampleWebApp)库中包含的应用包。 该应用包签名所用的证书 (MySampleApp.cer) 也随 GitHub 上的示例提供。 在安装应用之前，必须在设备上安装该证书。

![上传](images/azure-upload-file.png)

在文件资源管理器面板的空白区域右键单击，并选择**新建文件**创建一个新文件。 为文件命名：`default.html`。

如果使用的是[初学者项目](https://github.com/AppInstaller/MySampleWebApp)中提供的应用包，请将以下 HTML 代码复制到新建的网页 `default.html`。 如果使用的是自己的应用包，请修改应用服务 URL（`source=` 后面的 URL）。 可以从 Azure 门户中的应用概述页面获取应用服务 URL。

```html
<html>
<head>
    <meta charset="utf-8" />
    <title> Install My Sample App</title>
</head>
<body>
    <a href="ms-appinstaller:?source=https://appinstaller-azure-demo.azurewebsites.net/MySampleApp.appxbundle"> Install My Sample App</a>
</body>
</html>
```

### <a name="step-4---configure-the-web-app-for-app-package-mime-types"></a>步骤 4 - 针对应用包 MIME 类型配置 Web 应用

向 Web 应用添加一个名为 `Web.config` 的新文件。 从资源管理器打开 `Web.config` 文件并在其中添加下面几行。 

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <!--This is to allow the web server to serve resources with the appropriate file extension-->
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/appx" />
      <mimeMap fileExtension=".msix" mimeType="application/msix" />
      <mimeMap fileExtension=".appxbundle" mimeType="application/appxbundle" />
      <mimeMap fileExtension=".msixbundle" mimeType="application/msixbundle" />
      <mimeMap fileExtension=".appinstaller" mimeType="application/appinstaller" />
    </staticContent>
  </system.webServer>
</configuration>
```

### <a name="step-5---run-and-test"></a>步骤 5 - 运行和测试

若要启动你创建的网页，请在浏览器中输入步骤 3 中的 URL 并在后面附加 `/default.html`。 

![边缘](images/edge.png)

单击“安装我的示例应用”启动应用安装程序并安装应用包。 

## <a name="troubleshooting-issues"></a>解决问题

### <a name="app-installer-app-fails-to-install"></a>应用安装程序无法安装 
如果未在设备上安装为应用包签名所用的证书，应用安装将失败。 若要解决此问题，需要在安装应用之前安装该证书。 如果出于公开分发目的托管应用包，建议使用证书颁发机构的证书对应用包签名。 

![应用证书](images/aws-app-cert.png)

### <a name="nothing-happens-when-you-click-the-link"></a>点击链接后无反应 
确保应用安装程序已安装。 转到**设置** -> **应用和功能**，在已安装应用列表中找到应用安装程序。 

