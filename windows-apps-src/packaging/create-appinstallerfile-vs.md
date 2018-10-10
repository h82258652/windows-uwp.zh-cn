---
author: ridomin
title: 使用 Visual Studio 创建应用安装程序文件
description: 了解如何使用 Visual Studio 通过 .appinstaller 文件启用自动更新。
ms.author: rmpablos
ms.date: 5/2/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 应用安装程序, AppInstaller, 旁加载
ms.localizationpriority: medium
ms.openlocfilehash: 6158b804e1d4ece3c76099a3f8d33d5fa562078d
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "4472580"
---
# <a name="create-an-app-installer-file-with-visual-studio"></a>使用 Visual Studio 创建应用安装程序文件

从 Windows 10 版本 1804 和 Visual Studio 2017 更新 15.7 开始，可以将旁加载应用配置为使用 `.appinstaller` 文件接收自动更新。 Visual Studio 支持启用这些更新。

## <a name="app-installer-file-location"></a>应用安装程序文件位置
`.appinstaller` 文件可以托管在一个共享位置，如 HTTP 终结点或 UNC 共享文件夹，并包含用于查找要安装的应用包的路径。 用户从共享位置安装应用并启用对新更新的定期检查。 


### <a name="configure-the-project-to-target-the-correct-windows-version"></a>配置项目 - 以正确的 Windows 版本为目标

你可以在创建项目时配置 `TargetPlatformMinVersion` 属性，也可以稍后从项目属性中进行更改。 

>[!IMPORTANT]
> 只在 `TargetPlatformMinVersion` 为 Windows 10 版本 1804 或更高版本时，才生成应用安装程序文件。


### <a name="create-packages"></a>创建程序包

若要分发通过旁加载应用，必须创建应用包 (.appx/.msix) 或应用程序包 (.appxbundle/.msixbundle)，并将其发布在共享位置。

要执行该操作，请使用 Visual Studio 中的**创建应用序包**向导，并执行以下步骤。

1. 右键单击该项目，然后依次选择 **Microsoft Store** -> **创建应用包**。  

![上下文菜单，可导航到“创建应用包”](images/packaging-screen2.jpg)   

将显示**创建应用包**向导。

2. 选择 **I want to create packages for sideloading.** 和**启用自动更新**  

![显示的“创建应用包”对话框窗口](images/select-sideloading.png)  

只有在项目的 `TargetPlatformMinVersion` 设置为正确的 Windows 10 版本时，才启用**启用自动更新**。

3. 在**选择和配置包**对话框中可以选择支持的体系结构配置。 如果选择捆绑包，将生成一个安装程序；如果不想要捆绑包，而是希望为每个体系结构生成一个程序包，它还会为每个体系结构生成一个安装程序文件。  如果不确定该选择哪种体系结构，或者想了解有关各种设备使用哪种体系结构的详细信息，请参阅[应用包体系结构](device-architecture.md)。

4. 配置任何其他详细信息，例如版本编号或包输出位置。

![显示的创建应用包窗口及包配置](images/packaging-screen5.jpg)  

5. 如果在步骤 2 中选中了**启用自动更新**，此时会显示**配置更新设置**对话框。 在该页面中，可以指定**安装 URL** 和更新检查频率。

![显示的配置更新设置窗口及发布位置配置](images/sideloading-screen.png)  

6. 当你的应用成功打包后，将出现一个对话框，显示包含你的应用包的输出文件夹位置。 输出文件夹包含旁加载应用所需的所有文件，包括可用于宣传你的应用的 HTML 页面。

### <a name="publish-packages"></a>发布程序包

要使应用程序可用，必须将生成的文件发布到指定位置：

#### <a name="publish-to-shared-folders-unc"></a>发布到共享文件夹 (UNC)

如果要通过通用命名约定 (UNC) 共享文件夹发布程序包，请将应用包输出文件夹和安装 URL（请参阅步骤 6 了解详细信息）配置为相同的路径。 向导将在正确的位置生成文件，用户将从同一路径获取应用和后续更新。

#### <a name="publish-to-a-web-location-http"></a>发布到 Web 位置 (HTTP)

要发布到 Web 位置，需要将内容发布到 Web 服务器的访问权限，确保最终 URL 与向导中定义的安装 URL 相匹配（请参阅步骤 6 了解详细信息）。 一般情况下，使用文件传输协议 (FTP) 或 SSH 文件传输协议 (SFTP) 上传文件，但还有一些其他的发布方法，如 MSDeploy、SSH 或 Blob 存储，具体视你的 Web 提供商而定。

要配置 Web 服务器，你必须验证用于所使用的文件类型的 MIME 类型。 下面的示例为 Internet Information Services (IIS) 的 `web.config`：

```xml
<configuration>
  <system.webServer>
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/vns.ms-appx" />
      <mimeMap fileExtension=".appxbundle" mimeType="application/vns.ms-appx" />
      <mimeMap fileExtension=".appinstaller" mimeType="application/xml" />
    </staticContent>  
  </system.webServer>  
</configuration>
```




















