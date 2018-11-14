---
author: laurenhughes
title: 在 AWS 上为 Web 安装托管 UWP 应用包
description: 设置 AWS web 服务器验证通过应用安装程序应用的应用安装教程
ms.author: cdon
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10，Windows 10，UWP，应用安装程序，AppInstaller 旁, 加载，相关集，可选包，AWS
ms.localizationpriority: medium
ms.openlocfilehash: f24abac93e2444a3c9f454c8883902e5db4d31be
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6209475"
---
# <a name="hosting-uwp-app-packages-on-aws-for-web-install"></a>在 AWS 上为 Web 安装托管 UWP 应用包

通过应用安装程序，开发人员和 IT 专业人员可以通过在各自的内容分发网络 (CDN) 上托管应用的方式来分发 Windows 10 应用。 这种方式适用于不希望或不需要将应用发布到 Microsoft Store，但仍希望利用 Windows 10 打包和部署平台的企业。

本主题概述了配置 Amazon Web 服务 (AWS) 网站托管 UWP 应用包，以及如何使用应用安装程序来安装应用包的步骤。

## <a name="setup"></a>设置

要成功学习此教程，你将需要以下内容：
 
1. AWS 订阅 
2. 网页
3. UWP 应用包 - 要分发的应用包

可选：GitHub 上的[初学者项目](https://github.com/AppInstaller/MySampleWebApp)。 如果没有应用包或网页可以使用，但仍想学习如何使用此功能，这个初学者项目很有用。

本教程将介绍如何在网页上和在 AWS 上的托管包安装。 这将需要在 AWS 订阅。 根据您的操作的比例，你可以使用其免费会员资格获得遵循本教程。 

## <a name="step-1---aws-membership"></a>步骤 1-AWS 成员身份
若要获取 AWS 成员身份，请访问[AWS 帐户详细信息页面](https://aws.amazon.com/free/)。 可以使用免费会员资格获得此教程。

## <a name="step-2---create-an-amazon-s3-bucket"></a>步骤 2-创建 Amazon S3 桶

Amazon 简单存储服务 (S3) 是提供用于收集、 存储和分析数据 AWS。 S3 桶是一种便捷方式对托管 UWP 应用包和分发的网页。 

登录后 AWS 到与你的凭据，在`Services`查找`S3`。 

选择**创建桶**，并输入你的网站**存储段名称**。 按照对话框提示，用于设置属性和权限。 若要确保你的 UWP 应用，可以从你的网站分配，启用**读取**和**写入**你存储段的权限，并选择**公共读访问此存储桶**。

![设置 Amazon S3 桶权限](images/aws-permissions.png) 

查看摘要并确认所选的选项将会反映出来。 单击**创建桶**完成此步骤。 

## <a name="step-3---upload-uwp-app-package-and-web-pages-to-an-s3-bucket"></a>步骤 3-上载到 S3 桶的 UWP 应用包和网页

你已创建一个 Amazon S3 桶，你将能够在 Amazon S3 视图中看到它。 下面是我们演示桶如下所示的示例：

![Amazon S3 桶视图](images/aws-post-create.png)

现在，我们将准备要上传应用包和网页我们想要托管在我们 Amazon S3 桶。 

单击新建存储段中上, 传内容。 存储段是当前为空，因为已尚未上载执行任何操作。 单击**上载**按钮并选择的应用包和要上传的 web 页面文件。

> [!NOTE]
> 如果没有可用的应用包，可以使用 GitHub 上提供的[初学者项目](https://github.com/AppInstaller/MySampleWebApp)库中包含的应用包。 该应用包签名所用的证书 (MySampleApp.cer) 也随 GitHub 上的示例提供。 在安装应用之前，必须在设备上安装该证书。

![上传应用包](images/aws-upload-package.png)

类似于创建 Amazon S3 桶的权限，存储段中的内容还必须具有**读取**、**写入**和**授予对此对象的公共读取访问**权限。

如果你想要测试上载网页，但没有学校帐户，你可以使用从[初学者项目](https://github.com/AppInstaller/MySampleWebApp/blob/master/MySampleWebApp/default.html)的示例 html 页面 (default.html)。

> [!IMPORTANT]
> 上传 web 页面之前，请确认网页中的应用程序包引用正确无误。 

若要获取的应用程序包引用，请首先上载应用包并复制的应用包 URL。 编辑 html web 页面，以反映正确的应用程序包路径。 查看更多详细信息的代码示例。 

选择要获取的引用链接到应用包的已上传的应用包文件，它应该类似于以下示例：

![已上传的程序包路径](images/aws-package-path.png)

**复制**到应用链接打包和 web 页面中添加引用。 

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
Html 文件上传到你的 Amazon S3 存储段中。 请记得设置允许**读取**和**写入**访问权限。

## <a name="step-4---test"></a>第 4 步-测试

网页上载到 Amazon S3 桶后, 通过选择已上传的 html 文件中获取到网页的链接。

使用以下链接以打开的网页。 由于我们设置权限授予对应用包和网页的公共访问权限，附带指向网页的链接的任何人都将能够访问它，并且安装 UWP 应用包使用应用安装程序。 请注意，应用安装程序是 Windows 10 平台的一部分。 作为一名开发人员，你不需要将任何其他代码或功能添加到你的应用的应用安装程序使用。 

## <a name="troubleshooting"></a>疑难解答

### <a name="app-installer-fails-to-install"></a>应用安装程序无法安装 

如果在设备上未安装应用包签名所用的证书，应用安装将失败。 若要解决此问题，需要在安装应用之前安装该证书。 如果你托管应用包出于公开分发，建议使用从证书颁发机构证书对应用包进行签名。 


