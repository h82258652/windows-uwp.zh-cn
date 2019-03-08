---
title: 从 IIS 服务器安装 UWP 应用
description: 本教程介绍了如何设置 IIS 服务器，验证你的 web 应用可以托管应用程序包，并调用和有效地使用应用安装程序。
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10、 uwp、 应用程序安装程序中，应用安装旁, 加载，相关设置此选项，可选包，IIS 服务器
ms.localizationpriority: medium
ms.openlocfilehash: 6a4512229a29a7adc59d6b61edd596eaeb56a5a8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623562"
---
# <a name="install-a-uwp-app-from-an-iis-server"></a>从 IIS 服务器安装 UWP 应用

本教程介绍了如何设置 IIS 服务器，验证你的 web 应用可以托管应用程序包，并调用和有效地使用应用安装程序。

通过应用安装程序，开发人员和 IT 专业人员可以通过在各自的内容分发网络 (CDN) 上托管应用的方式来分发 Windows 10 应用。 这种方式适用于不希望或不需要将应用发布到 Microsoft Store，但仍希望利用 Windows 10 打包和部署平台的企业。 

## <a name="setup"></a>安装

若要成功转学习本教程，您需要：

1. Visual Studio 2017  
2. Web 开发工具和 IIS 
3. UWP 应用包 - 要分发的应用包

可选：[初学者项目](https://github.com/AppInstaller/MySampleWebApp)GitHub 上。 如果没有应用程序包，才能使用，但仍想要了解如何使用此功能，这非常有用。

## <a name="step-1---install-iis-and-aspnet"></a>第 1 步-安装 IIS 和 ASP.NET 

[Internet Information Services](https://www.iis.net/)是一项 Windows 功能，可以通过开始菜单安装。 在中**开始菜单**搜索**打开或关闭打开的 Windows 功能**。

找到并选择**Internet Information Services**安装 IIS。

> [!NOTE]
> 不需要选择在 Internet 信息服务下的所有复选框。 只有选择时，检查**Internet Information Services**已足够。

此外需要安装 ASP.NET 4.5 或更高版本。 若要安装它，请找到**Internet 信息服务-> 万维网服务-> 应用程序开发功能**。 选择大于或等于 ASP.NET 4.5 的 ASP.NET 版本。

![安装 ASP.NET](images/install-asp.png)

## <a name="step-2---install-visual-studio-2017-and-web-development-tools"></a>步骤 2-安装 Visual Studio 2017 和 Web 开发工具 

[安装 Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio)如果尚未安装它。 如果已有 Visual Studio 2017，请确保安装了以下工作负荷。 如果工作负荷上不存在您的安装，请按照此过程使用 Visual Studio 安装程序 （从开始菜单中找到）。  

安装过程中，选择**ASP.NET 和 Web 开发**和你感兴趣的任何其他工作负荷。 

安装完成后，启动 Visual Studio 并创建一个新项目 (**文件** -> **新项目**)。

## <a name="step-3---build-a-web-app"></a>步骤 3-生成 Web 应用

启动作为 Visual Studio 2017**管理员**并创建一个新**Visual C# Web 应用程序**具有项目**空**项目模板。 

![新建项目](images/sample-web-app.png)

## <a name="step-4---configure-iis-with-our-web-app"></a>第 4 步-配置 IIS 和 Web 应用 

从解决方案资源管理器，右键单击根项目，然后选择**属性**。

在 web 应用程序属性中，选择**Web**选项卡。在中**服务器**部分中，选择**本地 IIS**从下拉列表菜单，然后单击**创建虚拟目录**。 

![web 选项卡](images/web-tab.png)

## <a name="step-5---add-an-app-package-to-a-web-application"></a>步骤 5-将应用程序包添加到 web 应用程序 

添加想要分发到 web 应用程序的应用程序包。 可以使用的是所提供的一部分的应用程序包[初学者项目包](https://github.com/AppInstaller/MySampleWebApp/tree/master/MySampleWebApp/packages)如果没有可用的应用程序包的 GitHub 上。 该应用包签名所用的证书 (MySampleApp.cer) 也随 GitHub 上的示例提供。 必须具有到你的设备应用程序 (步骤 9) 在安装之前安装的证书。

在初学者项目的 web 应用程序中，一个新的文件夹已添加到 web 应用名为`packages`，其中包含要分发的应用包。 若要在 Visual Studio 中创建文件夹，右键单击解决方案资源管理器，选择根**外** -> **新文件夹**并将其命名`packages`。 若要将应用程序包添加到的文件夹，右键单击`packages`文件夹，然后选择**添加** -> **现有项...** 并浏览到应用程序包的位置。 

![添加包](images/add-package.png)

## <a name="step-6---create-a-web-page"></a>步骤 6-创建 Web 页

此示例 web 应用程序使用简单的 HTML。 你可以随意生成 web 应用程序根据需要根据您的需要。 

右键单击解决方案资源管理器中，选择根项目**外** -> **新项**，并添加一个新**HTML 页**从**Web**部分。

创建 HTML 页后，右键单击解决方案资源管理器中的 HTML 页上，然后选择**设为起始页**。  

双击 HTML 文件以在代码编辑器窗口中打开它。 在本教程中，将使用仅在需要在 web 页后，可以调用应用安装程序应用程序已成功安装 Windows 10 应用中的元素。 

在网页中包括以下 HTML 代码。 已成功调用应用安装程序的关键是要使用的应用安装程序向 OS 注册的自定义方案： `ms-appinstaller:?source=`。 请参阅下面的代码示例的更多详细信息。

> [!NOTE]
> 请确保指定后的自定义方案与 VS 解决方案的 web 选项卡中的项目 Url 匹配的 URL 路径。
 
```HTML
<html>
<head>
    <meta charset="utf-8" />
    <title> Install Page </title>
</head>
<body>
    <a href="ms-appinstaller:?source=http://localhost/SampleWebApp/packages/MySampleApp.appxbundle"> Install My Sample App</a>
</body>
</html>
```

## <a name="step-7---configure-the-web-app-for-app-package-mime-types"></a>步骤 7-将 web 应用的应用包 MIME 类型配置

打开**Web.config**文件从解决方案资源管理器，并添加以下行中的`<configuration>`元素。 

```xml
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
```

## <a name="step-8---add-loopback-exemption-for-app-installer"></a>步骤 8-添加应用程序安装工具的环回例外

由于网络隔离，而 UWP 应用，如应用安装程序会受到限制，以便使用 IP 环回地址，如 http://localhost/。 使用本地 IIS 服务器时，必须将应用安装程序添加到环回免除列表。 

若要执行此操作，打开**命令提示符**作为**管理员**并输入以下命令: ' 命令行 CheckNetIsolation.exe LoopbackExempt-a-n="microsoft.desktopappinstaller_8wekyb3d8bbwe"
```

To verify that the app is added to the exempt list, use the following command to display the apps in the loopback exempt list: 
```Command Line
CheckNetIsolation.exe LoopbackExempt -s
```

应查找`microsoft.desktopappinstaller_8wekyb3d8bbwe`列表中。

通过应用程序的安装程序的应用安装本地验证完成后，可以删除通过此步骤中添加环回例外：

```Command Line CheckNetIsolation.exe LoopbackExempt -d -n="microsoft.desktopappinstaller_8wekyb3d8bbwe"
```

## Step 9 - Run the Web App 

Build and run the web application by clicking on the run button on the VS Ribbon as shown in the image below:

![run](images/run.png)

A web page will open in your browser:

![web page](images/web-page.png)

Click on the link in the web page to launch the App Installer app and install your Windows 10 app package.


## Troubleshooting issues

### Not sufficient privilege 

If running the web app in Visual Studio displays an error such as "You do not have sufficient privilege to access IIS web sites on your machine", you will need to run Visual Studio as an administrator. Close the current instance of Visual Studio and reopen it as an admin.

### Set start page 

If running the web app causes the browser to load with an HTTP 403.14 - Forbidden error, it's because the web app doesn't have a defined start page. Refer to Step 6 in this tutorial to learn how to define a start page.
