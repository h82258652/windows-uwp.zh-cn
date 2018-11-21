---
author: laurenhughes
title: 从 IIS 服务器安装 UWP 应用
description: 本教程中演示了如何设置 IIS 服务器，验证你的 web 应用可以托管应用包，并调用并有效地使用应用安装程序。
ms.author: cdon
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10，uwp，应用安装程序，AppInstaller 旁, 加载，相关集，可选包，IIS 服务器
ms.localizationpriority: medium
ms.openlocfilehash: 2898a3450f75379492bae1ade5c85581cc8e5a4e
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2018
ms.locfileid: "7428933"
---
# <a name="install-a-uwp-app-from-an-iis-server"></a>从 IIS 服务器安装 UWP 应用

本教程中演示了如何设置 IIS 服务器，验证你的 web 应用可以托管应用包，并调用并有效地使用应用安装程序。

通过应用安装程序，开发人员和 IT 专业人员可以通过在各自的内容分发网络 (CDN) 上托管应用的方式来分发 Windows 10 应用。 这种方式适用于不希望或不需要将应用发布到 Microsoft Store，但仍希望利用 Windows 10 打包和部署平台的企业。 

## <a name="setup"></a>安装

若要成功完成本教程，你需要：

1. Visual Studio 2017  
2. Web 开发工具和 IIS 
3. UWP 应用包 - 要分发的应用包

可选：GitHub 上的[初学者项目](https://github.com/AppInstaller/MySampleWebApp)。 如果你没有应用包，以使用，但仍想要了解如何使用此功能，这非常有用。

## <a name="step-1---install-iis-and-aspnet"></a>第 1 步-安装 IIS 和 ASP.NET 

[Internet 信息服务](https://www.iis.net/)是一种 Windows 功能，可以通过开始菜单中安装。 在**开始菜单****打开或关闭 Windows 功能**搜索。

查找并选择要安装 IIS 的**Internet 信息服务**。

> [!NOTE]
> 你无需选择在 Internet 信息服务下的所有复选框。 只有选择检查**Internet 信息服务**时就足够了。

你还将需要安装 ASP.NET 4.5 或更高版本。 若要安装它，找到**Internet 信息服务-> 万维网服务-> 应用程序开发功能**。 选择大于或等于 ASP.NET 4.5 ASP.NET 的一个版本。

![安装 ASP.NET](images/install-asp.png)

## <a name="step-2---install-visual-studio-2017-and-web-development-tools"></a>第 2 步-安装 Visual Studio 2017 和 Web 开发工具 

[安装 Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio)如果尚未安装它。 如果你尚未获得 Visual Studio 2017，请确保已安装了以下工作负荷。 如果工作负荷不存在于你的安装，请按照使用 Visual Studio 安装程序 （从开始菜单中找到）。  

在安装期间，选择**ASP.NET 和 Web 开发**和你感兴趣的任何其他工作负荷。 

安装完成后，启动 Visual Studio 并创建一个新项目 (**文件** -> **新项目**)。

## <a name="step-3---build-a-web-app"></a>步骤 3-生成 Web 应用

启动 Visual Studio 2017 以**管理员身份**并使用**空**项目模板创建新的**Visual C# Web 应用程序**项目。 

![新建项目](images/sample-web-app.png)

## <a name="step-4---configure-iis-with-our-web-app"></a>步骤 4-配置 IIS 与我们的 Web 应用 

从解决方案资源管理器，右键单击该根项目，然后选择**属性**。

在 web 应用属性中，选择**Web**选项卡。在**服务器**部分中，从下拉菜单中选择**本地 IIS** ，然后单击**创建虚拟目录**。 

![web 选项卡](images/web-tab.png)

## <a name="step-5---add-an-app-package-to-a-web-application"></a>步骤 5-将应用包添加到 web 应用程序 

添加要分配到 web 应用程序的应用包。 你可以使用是 GitHub 上提供[初学者项目包](https://github.com/AppInstaller/MySampleWebApp/tree/master/MySampleWebApp/packages)的一部分，如果你没有可用的应用包的应用包。 该应用包签名所用的证书 (MySampleApp.cer) 也随 GitHub 上的示例提供。 你必须安装到你之前安装该应用 (第 9 步) 的设备的证书。

初学者项目的 web 应用程序中，在一个新文件夹添加到名为 web 应用`packages`，其中包含要分发的应用包。 若要在 Visual Studio 中创建文件夹，右键单击解决方案资源管理器的根上，选择**添加** -> **新文件夹**并将其命名`packages`。 若要将应用包添加到文件夹，右键单击`packages`文件夹，然后选择**添加** -> **现有项目...** 和浏览到应用包位置。 

![添加程序包](images/add-package.png)

## <a name="step-6---create-a-web-page"></a>第 6 步-创建网页

此示例 web 应用使用简单的 HTML。 你可以自由地生成 web 应用根据需要根据你的需求。 

右键单击解决方案资源管理器的根项目中，选择**添加** -> **新项目**，并从**Web**部分中添加一个新的**HTML 页面**。

HTML 页面创建后，右键单击解决方案资源管理器中的 HTML 页面上，然后选择**设为开始页**。  

双击 HTML 文件，以在代码编辑器窗口中打开它。 在本教程中，将使用仅在所需的网页来调用应用安装程序应用成功安装的 Windows 10 应用中的元素。 

在你的 web 页面包含以下的 HTML 代码。 成功调用应用安装程序的关键是使用应用安装程序向操作系统注册的自定义方案： `ms-appinstaller:?source=`。 请参阅下面的代码示例，更多详细信息。

> [!NOTE]
> 请确保指定的自定义方案匹配 VS 解决方案的 web 选项卡中的项目 Url 后的 URL 路径。
 
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

## <a name="step-7---configure-the-web-app-for-app-package-mime-types"></a>步骤 7-配置应用包 MIME 类型的 web 应用

从解决方案资源管理器中打开**Web.config**文件并添加以下行中的`<configuration>`元素。 

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

## <a name="step-8---add-loopback-exemption-for-app-installer"></a>步骤 8-添加应用安装程序的环回豁免

由于网络隔离，如应用安装程序的 UWP 应用被限制为使用 IP 环回地址，如http://localhost/。 使用本地 IIS 服务器时，必须将应用安装程序添加到环回豁免列表。 

若要执行此操作，以打开**命令提示符****管理员**和输入以下内容: '' 命令行 CheckNetIsolation.exe LoopbackExempt-a-n=microsoft.desktopappinstaller_8wekyb3d8bbwe
```

To verify that the app is added to the exempt list, use the following command to display the apps in the loopback exempt list: 
```Command Line
CheckNetIsolation.exe LoopbackExempt -s
```

你应查找`microsoft.desktopappinstaller_8wekyb3d8bbwe`列表中。

通过应用安装程序安装应用的本地验证完成后，你可以删除你在此步骤中通过添加环回豁免：

'' 命令行 CheckNetIsolation.exe LoopbackExempt-d-n=microsoft.desktopappinstaller_8wekyb3d8bbwe
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
