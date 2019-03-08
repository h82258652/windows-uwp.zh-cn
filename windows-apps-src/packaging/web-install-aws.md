---
title: 在 AWS 上为 Web 安装托管 UWP 应用包
description: 设置 AWS web 服务器以验证通过应用安装程序应用程序的应用程序安装教程
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10 中，Windows 10 UWP，应用程序安装程序中，应用安装旁, 加载，相关设置此选项，可选包，AWS
ms.localizationpriority: medium
ms.openlocfilehash: 53fe01a1c1a825377e886e042b4eef3868cbf5eb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628052"
---
# <a name="hosting-uwp-app-packages-on-aws-for-web-install"></a>在 AWS 上为 Web 安装托管 UWP 应用包

通过应用安装程序，开发人员和 IT 专业人员可以通过在各自的内容分发网络 (CDN) 上托管应用的方式来分发 Windows 10 应用。 这种方式适用于不希望或不需要将应用发布到 Microsoft Store，但仍希望利用 Windows 10 打包和部署平台的企业。

本主题概述了配置为主机 UWP 应用包时，将 Amazon Web Services (AWS) 网站以及如何使用应用程序安装程序应用来安装应用程序包的步骤。

## <a name="setup"></a>安装

要成功学习此教程，你将需要以下内容：
 
1. AWS 订阅 
2. 网页
3. UWP 应用包 - 要分发的应用包

可选：[初学者项目](https://github.com/AppInstaller/MySampleWebApp)GitHub 上。 如果没有应用包或网页可以使用，但仍想学习如何使用此功能，这个初学者项目很有用。

本教程将详细阐述如何设置 web 页面和 AWS 上的承载包。 这将需要使用 AWS 订阅。 具体取决于您的操作的小数位数，可以使用其可用的成员资格要遵循本教程。 

## <a name="step-1---aws-membership"></a>步骤 1-AWS 成员身份
若要获取 AWS 成员身份，请访问[AWS 帐户详细信息页](https://aws.amazon.com/free/)。 可以使用免费会员资格获得此教程。

## <a name="step-2---create-an-amazon-s3-bucket"></a>步骤 2-创建 Amazon S3 存储桶

Amazon 简单存储服务 (S3) 是 AWS 提供用于收集、 存储和分析数据。 S3 存储桶是指向承载 UWP 应用包和分发 web 页的简便方法。 

之后在登录到 AWS 与你的凭据下,`Services`查找`S3`。 

选择**创建存储桶**，并输入**存储桶名称**为你的网站。 按照对话框提示用于设置属性和权限。 若要确保你的 UWP 应用，可以分发从你的网站，启用**读**并**编写**权限存储桶并选择**授予对此存储桶的公共读取访问权限**.

![对 Amazon S3 存储桶设置权限](images/aws-permissions.png) 

查看摘要，以确保反映所选的选项。 单击**创建存储桶**来完成此步骤。 

## <a name="step-3---upload-uwp-app-package-and-web-pages-to-an-s3-bucket"></a>步骤 3-将 UWP 应用包和网页上传到 S3 存储桶

一个已创建 Amazon S3 存储桶，你将能够在 Amazon S3 视图中看到它。 下面是我们演示存储桶如下所示的示例：

![Amazon S3 存储桶视图](images/aws-post-create.png)

我们现已准备要上传应用包和我们想要托管在我们的 Amazon S3 存储桶中的网页。 

单击新创建的存储桶上, 传内容。 存储桶是当前为空，因为具有尚未上载任何内容。 单击**上传**按钮，然后选择应用包和网页要上传的文件。

> [!NOTE]
> 如果没有可用的应用包，可以使用 GitHub 上提供的[初学者项目](https://github.com/AppInstaller/MySampleWebApp)库中包含的应用包。 该应用包签名所用的证书 (MySampleApp.cer) 也随 GitHub 上的示例提供。 在安装应用之前，必须在设备上安装该证书。

![上传应用程序包](images/aws-upload-package.png)

类似于用于创建 Amazon S3 存储桶的权限，存储桶中的内容还必须具有**读取**，**编写**，并**授予对此对象的公共读取访问权限**权限。

如果你想要测试上载网页，但还没有，则可以从使用示例 html 页面 (default.html)[初学者项目](https://github.com/AppInstaller/MySampleWebApp/blob/master/MySampleWebApp/default.html)。

> [!IMPORTANT]
> 上传 web 页之前，请确认在网页中的应用包引用不正确。 

若要获取应用程序包引用，请首先上载应用程序包并复制应用包 URL。 编辑 html web 页面以反映正确的应用程序的包路径。 请参阅更多详细信息的代码示例。 

选择要获取应用程序包的引用链接的已上载的应用包文件，它应该类似于以下示例：

![上载的包路径](images/aws-package-path.png)

**复制**指向应用程序的包并在网页中添加引用。 

```html
<html>
    <head>
        <meta charset="utf-8" />
        <title> Install My Sample App</title>
    </head>
    <body>
        <a href="ms-appinstaller:?source=https://s3-us-west-2.amazonaws.com/appinstaller-aws-demo/MySampleApp.appxbundle"> Install My Sample App</a>
    </body>
</html>
```
将 html 文件上载到 Amazon S3 存储桶。 请记住要设置权限以允许**读取**并**编写**访问。

## <a name="step-4---test"></a>步骤 4-测试

一旦 web 页上载到 Amazon S3 存储桶中，选择已上传的 html 文件获取到 web 页的链接。

使用以下链接以打开 web 页。 由于我们设置权限授予公共访问权限的应用包和网页，指向 web 页的链接的任何人都将能够访问它，并且在 UWP 应用使用安装包的应用安装程序。 请注意，应用安装程序的 Windows 10 平台的一部分。 作为开发人员，您不必将任何其他代码或功能添加到你的应用启用的应用安装程序使用。 

## <a name="troubleshooting"></a>疑难解答

### <a name="app-installer-fails-to-install"></a>应用安装程序安装失败 

如果使用的应用程序包进行签名的证书未安装在设备上，应用程序安装将失败。 若要解决此问题，需要在安装应用之前安装该证书。 如果要在托管公开分发的应用包，建议使用来自证书颁发机构的证书在应用包进行签名。 


