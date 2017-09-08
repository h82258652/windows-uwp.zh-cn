---
title: "使用 Visual Studio 开发创意者主题作品"
author: KevinAsgari
description: "使用 Visual Studio 开发 Xbox Live 创意者计划主题作品入门"
ms.assetid: 6952dac0-66ff-4717-b3c7-8b3792e834e3
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "xbox live, xbox, 游戏, uwp, windows 10, xbox one, xbox live 创意者, visual studio"
ms.openlocfilehash: 51bdb1cc3e6f06fef9a842c52a10a31b44dcbec9
ms.sourcegitcommit: 90fbdc0e25e0dff40c571d6687143dd7e16ab8a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2017
---
# <a name="get-started-developing-an-xbox-live-creators-program-title-with-visual-studio"></a>使用 Visual Studio 开发 Xbox Live 创意者计划主题作品入门

## <a name="setup"></a>安装

### <a name="software-requirements"></a>软件要求

- Windows 操作系统
    - Windows 10（作为开发和/或测试计算机）
- Visual Studio
    - Visual Studio 2015。  请参阅 [https://www.visualstudio.com/en-us/downloads/visual-studio-2015-downloads-vs.aspx](https://www.visualstudio.com/en-us/downloads/visual-studio-2015-downloads-vs.aspx)
    - Windows 10 SDK v10.0.14393.0 或更高版本 [https://developer.microsoft.com/zh-CN/windows/downloads/windows-10-sdk](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk)

### <a name="install-the-windows-10-sdk"></a>安装 Windows 10 SDK

Windows 10 SDK 提供了用于生成 Windows 10 应用的最新标头、库、元数据和工具。 通过安装此 SDK、最新版 Visual Studio 2015 和 IDE 环境，你将能够访问新的 Windows 10 API。 Windows 10 SDK 允许你生成通用 Windows 应用以及适用于 Windows 10 的桌面应用。

Windows 10 SDK 通常由 Visual Studio 2015 安装和更新。  如果已安装 Visual Studio 2015，请转到“工具”菜单的“扩展和更新”，然后勾选“更新”选项卡安装最新的 Windows SDK。

你还可以访问 [https://developer.microsoft.com/zh-CN/windows/downloads/windows-10-sdk](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk) 安装独立版本的 Windows 10 SDK。

### <a name="create-a-new-title-on-windows-dev-center"></a>在 Windows 开发人员中心上创建新的主题作品

你需要在开发人员中心上创建新的 UWP 主题作品，然后才能登录到 Xbox Live。  你的服务配置（如“成就”定义）是与主题作品相关联的。

请参阅[创建新的创意者主题作品](create-and-test-a-new-creators-title.md)，了解如何创建新的主题作品。

### <a name="configuring-your-development-device"></a>配置开发设备

需要在你的设备上完成以下初步设置步骤，以便成功登录 Xbox Live 并调用各种 Xbox Live 服务。

#### <a name="set-your-sandbox"></a>设置沙盒

有关沙盒的更多详细信息，请参阅文章 [Xbox Live 沙盒](../xbox-live-sandboxes.md)。  你可以根据情况先自行阅读本文章，然后再回来，但下面将会提供对本文章的简短概述。

沙盒可以提供一种方法，使 [Xbox Live 服务配置](xbox-live-service-configuration-creators.md)与零售版本隔离，直到你准备发布主题作品为止。  你累积的一些数据是特定于沙盒的。  例如，假设你的主题作品定义了一个名为*头像*的统计数据，并且在测试主题作品时，你在用户帐户中累积了一定数量的头像。  该值将特定于你主题作品的开发沙盒。如果你玩的是零售版本的主题作品，则不会保留这些头像。

有关如何设置沙盒的信息，请参阅 [Xbox Live 沙盒](../xbox-live-sandboxes.md)。

#### <a name="sign-in-with-an-xbox-live-account-that-has-been-authorized-for-testing"></a>使用已获得测试授权的 Xbox Live 帐户登录

要登录到开发沙盒，你必须预配一个常规的 Microsoft 帐户 (MSA) 来访问你的沙盒。  这样有助于提高开发中主题作品的安全性，而且还能带来其他好处。

要了解有关测试帐户的详细信息，请参阅[授权 Xbox Live 帐户在你的环境中进行测试](authorize-xbox-live-accounts.md)

## <a name="sdk-samples"></a>SDK 示例

通过 SDK 示例可以很好地了解如何使用 Xbox Live API。

在 [https://github.com/Microsoft/xbox-live-samples](https://github.com/Microsoft/xbox-live-samples) 中的 /Samples/CreatorsSDK/ 下找到的示例展示了开发人员可在 Xbox Live 创意者计划中使用的 API。  

若要使用这些示例，你需要将沙盒更改为 XDKS.1。有关如何设置沙盒的信息，请参阅 [Xbox Live 沙盒](../xbox-live-sandboxes.md)。

## <a name="visual-studio-project-setup"></a>设置 Visual Studio 项目

### <a name="1-create-a-blank-project"></a>1. 创建空白项目

如果你已经有 UWP 应用，则可以跳过此步骤。

从“开始”菜单或任务栏启动 Visual Studio 2015。  
如未安装 Visual Studio 2015，可访问 [https://www.visualstudio.com/](https://www.visualstudio.com/) 进行安装

![启动 Visual Studio](../images/VisualStudioStart.png)

启动 Visual Studio 后，依次选择**文件** -> **新建** -> **项目**，如下所示。

![](../images/VS_new_project.png)

当**新建项目**对话框出现后，在左侧窗格中依次选择 **Visual C#**、**Windows**、**通用**节点，然后在右侧窗格中单击**空白应用（通用 Windows）**。  在该对话框的下半部分中，为项目命名。

### <a name="2-add-references-to-the-xbox-live-api-xsapi-in-your-project"></a>2. 在项目中添加对 Xbox Live API (XSAPI) 的引用

要使用项目中的 Xbox Live API，请：

* 将对 Xbox Live API 二进制文件的引用添加到采用 NuGet 程序包格式的项目中。
<p/>
或者
<p/>
* 将对 Xbox Live 服务 API 源的引用添加到项目中

引用源使调试变得更容易。
引用二进制文件可更快地进行编译。

本文假设你使用的是 NuGet 程序包。  如果要使用源，请参阅[在 UWP 项目中编译 Xbox Live API 源](../get-started-with-partner/add-xbox-live-apis-source-to-a-uwp-project.md)

#### <a name="add-references-to-xbox-live-api-binaries-to-your-project"></a>将对 Xbox Live API 二进制文件的引用添加到项目中
若要在项目中添加对 Xbox Live API NuGet 程序包的引用，请在 Visual Studio 中转到“管理 Nuget 程序包”

![](../images/getting_started/first_vstitle_nuget.png)

#### <a name="locate-xbox-live"></a>查找 Xbox Live
在 NuGet 的搜索字段中输入“Xbox Live”（不含引号），你会看到 Xbox Live SDK 的四种变体，如下所示。

![](../images/getting_started/first_vstitle_nuget_xbox.png)

Xbox 服务 API 同时支持 UWP 和 XDK 以及 C++ 和 WinRT。  

在 `Microsoft.Xbox.Live.SDK.*.UWP` 和 `Microsoft.Xbox.Live.SDK.*.XboxOneXDK` 之间选择。  `XboxOneXDK`  适用于 ID@Xbox 和使用 Xbox One XDK 的托管开发人员。  `UWP`  适用于可在电脑、Xbox One 或 Windows Phone 上运行的 UWP 游戏。  有关在 Xbox One 上运行 UWP 的更多详细信息，请访问 [https://docs.microsoft.com/zh-cn/windows/uwp/xbox-apps/getting-started](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started)

在 `Microsoft.Xbox.Live.SDK.Cpp.*` 和 `Microsoft.Xbox.Live.SDK.WinRT.*` 之间选择。 `Cpp`  适用于使用 Xbox Live API 的 C++ 游戏引擎。  `WinRT`  适用于使用 Xbox Live API 通过 C++、C# 或 Javascript 语言编写的游戏引擎。  

如果你是 Xbox Live 创意者计划的一员，则你可以使用以下任一选项：
* 适用于 C++ UWP 游戏引擎的 Microsoft.Xbox.Live.SDK.Cpp.UWP。
* 适用于 C# 或 JavaScript UWP 游戏引擎的 Microsoft.Xbox.Live.SDK.WinRT.UWP。 当结合使用 WinRT 与 C++ 引擎时，你可以使用运用乘幂号 (^) 的 C++/CX。  Microsoft.Xbox.Live.SDK.Cpp.UWP 是用于 C++ 游戏引擎的推荐 API。   
* Unity。  请参阅[使用 Unity 开发创意者主题作品](develop-creators-title-with-unity.md)文章获取详细信息。

### <a name="optional-install-the-xbox-live-platform-extensions-sdk"></a>（可选）安装 Xbox Live Platform Extensions SDK

如果你要使用连接存储，需要从 [http://aka.ms/xblextsdk](http://aka.ms/xblextsdk) 下载 Xbox Live 平台扩展 SDK zip 文件。

> **注意** 如果你参加了 Xbox Live 创意者计划，则无法访问安全套接字。  但是，你可以使用连接存储。

下载 zip 文件后，将其中的内容解压缩到你选择的文件夹中，然后安装 MSI。  
该程序包包含与 UWP平台的安全网络和连接存储功能相关的 winmd 文件和文档。

在安装了 Xbox Live Platform Extensions SDK 后，你可以使用 Visual Studio 添加对 Extensions SDK 的引用，并在你的通用 Windows 平台 (UWP) 游戏中使用以下命名空间：

- Windows.Gaming.XboxLive.Storage

### <a name="3-associate-your-visual-studio-project-with-your-uwp-app"></a>3. 将 Visual Studio 项目与 UWP 应用关联

为了使你的应用支持登录，必须将其关联到你之前在开发人员中心上创建的 UWP 应用。  我们将向你介绍如何完成此次关联。

如果可以使用 Visual Studio 中的**应用商店** -> **将应用与应用商店关联…**选项， 则执行以下步骤：

1.  在 Visual Studio 2015 中打开你的项目
1.  右键单击主项目（启动项目），然后单击**应用商店** -> **将应用程序与应用商店关联...**
1.  如果需要，使用用于创建应用的 **Windows 开发人员帐户**登录
1.  在下一页上，选择你刚创建的应用，确认相关信息，然后单击**关联**

如果如果无法使用 Visual Studio 中的**应用商店** -> **将应用与应用商店关联…**选项， 则可以通过以下操作手动更新应用的程序包清单，以使用应用的应用商店程序包标识：

1.  在文本编辑器中打开 Package.appxmanifest 文件，并将**保留应用名称**部分中的“标识”节点更新为**应用程序标识**。
1.  从项目中删除 .pfx 文件（从项目中排除该文件还是不够的）

### <a name="4-associate-your-visual-studio-project-with-your-xbox-live-enabled-title"></a>4. 将 Visual Studio 项目与支持 Xbox Live 的主题作品关联

若要执行此操作，你需要添加可供 Xbox Live SDK 在运行时读取的配置文件。

1.  创建一个文本文件，并将其命名为 **xboxservices.config**。 注意，该文件的扩展名必须是 .config
1.  将该文本文件添加到你的主项目（启动项目）中
1.  右键单击该文件，选择“属性”，并确保**内容**已设置为**是**，或者**生成操作**已设置为**内容**，然后将**复制到输出目录**设置为**始终复制**。  这样可以确保在 AppX 文件夹中正确复制该文件。
1.  你可以将“项目类型”保留为**不参与生成**
1.  使用以下模板编辑文本文件，并使用从 Windows 开发人员中心获取的值替换 TitleId 和 PrimaryServiceConfigId。  PrimaryServiceConfigId 在 Windows 开发人员中心中显示为“SCID”。  
1.  对于 Xbox Live 创意者计划中的主题作品，必须将 XboxLiveCreatorsTitle 设置为 true，因为它会更改 Xbox Live 创意者计划中的主题作品使用的登录方法。

```
    {
       "TitleId" : your title ID (JSON number in decimal),
       "PrimaryServiceConfigId" : "{your primary service config ID}",
       "XboxLiveCreatorsTitle" : true
    }
```

例如：

```
    {
        "TitleId" : 1563044810,
        "PrimaryServiceConfigId" : "12200100-88da-4d8b-af88-e38f5d2a2bca",
        "XboxLiveCreatorsTitle" : true
    }
```

### <a name="5-add-internet-capabilities-to-your-visual-studio-project"></a>5. 将 Internet 功能添加到 Visual Studio 项目

1. 在 Visual Studio 2015 中双击 **package.appxmanifest** 文件，以打开清单设计器。
2. 单击**功能**选项卡
3. 单击 **Internet（客户端）**
4. 关闭文件并保存更改。

### <a name="6-optionally-include-xsapi-header-in-your-project"></a>6.（可选）在项目中包括 XSAPI 标头

对于基于 Microsoft.Xbox.Live.SDK.WinRT.* 的项目，无需包括任何标头。

对于基于 Microsoft.Xbox.Live.SDK.Cpp.* 的项目，请在 C++ 项目中包括“xsapi\\services.h”，以引入 Xbox Live 服务 API (XSAPI) NuGet 程序包的标头。 包括 XSAPI 标头之前，你必须定义 XBOX_LIVE_CREATORS_SDK。 这会将 API 图面区域限制为仅可由 Xbox Live 创意者计划中的开发人员使用的 API。 例如：

```c++
#define XBOX_LIVE_CREATORS_SDK
#include "xsapi\services.h"
```

### <a name="7-change-your-sandbox-on-the-target-device"></a>7. 更改目标设备上的沙盒

有关如何设置沙盒的信息，请参阅 [Xbox Live 沙盒](../xbox-live-sandboxes.md)。

### <a name="8-learn-more"></a>8. 了解更多

在 [https://github.com/Microsoft/xbox-live-samples](https://github.com/Microsoft/xbox-live-samples) 中的 /Samples/CreatorsSDK/ 下找到的示例展示了开发人员可在 Xbox Live 创意者计划中使用的 API。  若要使用这些示例，你需要将沙盒更改为 XDKS.1
