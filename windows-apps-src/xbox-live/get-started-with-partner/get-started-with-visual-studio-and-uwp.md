---
title: "适用于 UWP 游戏的 Visual Studio 入门"
author: KevinAsgari
description: "了解如何设置 Visual Studio 项目以启用适用于 UWP 游戏的 Xbox Live"
ms.assetid: b53bc91f-79db-4d8f-8919-b9144e2d609b
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "xbox live, xbox, 游戏, uwp, windows 10, xbox one"
ms.openlocfilehash: 9dc93bc0adf4709f586366145b5a978aa3fc2e77
ms.sourcegitcommit: 90fbdc0e25e0dff40c571d6687143dd7e16ab8a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2017
---
# <a name="get-started-using-visual-studio-for-uwp-games"></a>适用于 UWP 游戏的 Visual Studio 的入门

## <a name="setup"></a>安装

### <a name="software-requirements"></a>软件要求

- Windows 操作系统
    - Windows 8.1（作为开发计算机）
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

请参阅[创建新的主题作品](create-a-new-title.md)，了解如何创建新的主题作品。

### <a name="configuring-your-development-device"></a>配置开发设备

需要在你的设备上完成以下初步设置步骤，以便成功登录 Xbox Live 并调用各种 Xbox Live 服务。

#### <a name="set-your-sandbox"></a>设置沙盒

有关沙盒的更多详细信息，请参阅文章 [Xbox Live 沙盒](../xbox-live-sandboxes.md)。  你可以根据情况先自行阅读本文章，然后再回来，但下面将会提供对本文章的简短概述。

在准备好发布你的主题作品前，可以使用沙盒将 [Xbox Live 服务配置](../xbox-live-service-configuration.md)与零售版本隔离开来。  你累积的一些数据是特定于沙盒的。  例如，假设你的主题作品定义了一个名为*头像*的统计数据，并且在测试主题作品时，你在用户帐户中累积了一定数量的头像。  该值将特定于你主题作品的开发沙盒。如果你玩的是零售版本的主题作品，则不会保留这些头像。

请参阅文章 [Xbox Live 沙盒](../xbox-live-sandboxes.md)，以了解详细信息并查看如何设置沙盒。

#### <a name="sign-in-with-a-test-account"></a>使用测试帐户登录

要登录到开发沙盒，则必须创建一个测试帐户，或者预配一个常规的 Microsoft 帐户 (MSA) 来访问沙盒。  这样有助于提高开发中主题作品的安全性，而且还能带来其他好处。

若要了解有关测试帐户以及如何创建测试帐户的详细信息，请参阅 [Xbox Live 测试帐户](../xbox-live-test-accounts.md)

#### <a name="sdk-samples"></a>SDK 示例

通过 SDK 示例可以很好地了解如何使用 Xbox Live API。

在 [https://github.com/Microsoft/xbox-live-samples](https://github.com/Microsoft/xbox-live-samples) 中的 /Samples/ID@Xbox/ 下找到的示例展示了开发人员可在 ID@Xbox 计划中使用的 API。  

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

若要从项目中使用 Xbox Live API，请执行以下操作之一：

* 将对 Xbox Live API `binaries` 的引用添加到项目中，形式为 NuGet 程序包。  引用 `binaries` 可更快地进行编译。
<p/>
`or`
<p/>
* 将对 Xbox Live API `source` 的引用添加到项目中。  引用 `source` 可使调试变得更容易。

本文假设你使用的是 NuGet 程序包。  若要使用源，请参阅[在 UWP 项目中编译 Xbox Live API 源](add-xbox-live-apis-source-to-a-uwp-project.md)

#### <a name="add-references-to-xbox-live-api-binaries-to-your-project"></a>将对 Xbox Live API 二进制文件的引用添加到项目中
若要在项目中添加对 Xbox Live API NuGet 程序包的引用，请在 Visual Studio 中转到“管理 Nuget 程序包”

![](../images/getting_started/first_vstitle_nuget.png)

#### <a name="locate-xbox-live"></a>查找 Xbox Live
在 NuGet 的搜索字段中输入“Xbox Live”（不含引号），你会看到 Xbox Live SDK 的四种变体，如下所示。

![](../images/getting_started/first_vstitle_nuget_xbox.png)

Xbox 服务 API 同时支持 UWP 和 XDK 以及 C++ 和 WinRT。  

在 `Microsoft.Xbox.Live.SDK.*.UWP` 和 `Microsoft.Xbox.Live.SDK.*.XboxOneXDK` 之间选择。  `XboxOneXDK`  适用于 ID@Xbox 和使用 Xbox One XDK 的托管开发人员。  `UWP`  适用于可在电脑、Xbox One 或 Windows Phone 上运行的 UWP 游戏。  有关在 Xbox One 上运行 UWP 的更多详细信息，请访问 [https://docs.microsoft.com/zh-cn/windows/uwp/xbox-apps/getting-started](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started)

在 `Microsoft.Xbox.Live.SDK.Cpp.*` 和 `Microsoft.Xbox.Live.SDK.WinRT.*` 之间选择。 `Cpp`  适用于使用 Xbox Live API 的 C++ 游戏引擎。  `WinRT`  适用于使用 Xbox Live API 通过 C++、C# 或 Javascript 语言编写的游戏引擎。  在将 WinRT 与 C++ 引擎结合使用时，将使用有乘幂号 (^) 的 C++/CX。  `Cpp`  是建议用于 C++ 游戏引擎的 API。   

### <a name="optional-install-the-xbox-live-platform-extensions-sdk"></a>（可选）安装 Xbox Live Platform Extensions SDK

如果想要使用连接存储或安全套接字，则需要从 [http://aka.ms/xblextsdk](http://aka.ms/xblextsdk) 下载 Xbox Live Platform Extensions SDK zip 文件。

> **注意** 如果你参加了 Xbox Live 创意者计划，则无法访问安全套接字。  但是，你可以使用连接存储。

下载 zip 文件后，将其中的内容解压缩到你选择的文件夹中，然后安装 MSI。  
该程序包包含与 UWP平台的安全网络和连接存储功能相关的 winmd 文件和文档。

在安装了 Xbox Live Platform Extensions SDK 后，你可以使用 Visual Studio 添加对 Extensions SDK 的引用，并在你的通用 Windows 平台 (UWP) 游戏中使用以下命名空间：

- Windows.Gaming.XboxLive.Storage
- Windows.Networking.XboxLive

### <a name="3-associate-your-visual-studio-project-with-your-uwp-app"></a>3. 将 Visual Studio 项目与 UWP 应用关联

为了使你的应用支持登录，必须将其关联到你之前在开发人员中心上创建的 UWP 应用。  我们将向你介绍如何完成此次关联。

如果可以使用 Visual Studio 中的**应用商店** -> **将应用程序与应用商店关联…**选项， 则执行以下步骤：

1.  在 Visual Studio 2015 中打开你的项目
1.  右键单击主项目（启动项目），然后单击**应用商店** -> **将应用程序与应用商店关联...**
1.  如果需要，使用用于创建应用的 **Windows 开发人员帐户**登录
1.  在下一页上，选择你刚创建的应用，确认相关信息，然后单击**关联**

如果无法使用 Visual Studio 中的**应用商店** -> **将应用程序与应用商店关联…**选项， 则可以通过以下操作手动更新应用的程序包清单，以使用应用的应用商店程序包标识：

1.  在文本编辑器中打开 Package.appxmanifest 文件，并将**保留应用名称**部分中的“标识”节点更新为**应用程序标识**。
1.  从项目中删除 .pfx 文件（从项目中排除该文件还是不够的）

### <a name="4-associate-your-visual-studio-project-with-your-xbox-live-enabled-title"></a>4. 将 Visual Studio 项目与支持 Xbox Live 的主题作品关联

若要执行此操作，你需要添加可供 Xbox Live SDK 在运行时读取的配置文件。

1.  创建一个文本文件，并将其命名为 **xboxservices.config**。  注意，该文件的扩展名是 .config
1.  将该文本文件添加到你的主项目（启动项目）中
1.  右键单击该文件，选择“属性”，并确保**内容**已设置为**是**，或者**生成操作**已设置为**内容**，然后将**复制到输出目录**设置为**始终复制**。  这样可以确保在 AppX 文件夹中正确复制该文件。
1.  你可以将“项目类型”保留为**不参与生成**
1.  使用以下模板编辑 JSON 文件，并使用从 Windows 开发人员中心获取的值替换 TitleId、PrimaryServiceConfigId。  PrimaryServiceConfigId 在 Windows 开发人员中心中显示为“SCID”

```
    {
       "TitleId" : your title ID (JSON number in decimal),
       "PrimaryServiceConfigId" : "{your primary service config ID}"
    }
```

例如：

```
    {
        "TitleId" : 1563044810,
        "PrimaryServiceConfigId" : "12200100-88da-4d8b-af88-e38f5d2a2bca"
    }
```

### <a name="5-if-you-plan-to-use-multiplayer-update-your-appxmanifest"></a>5. 如果你打算使用多人游戏，请更新你的 AppXManifest

如果你打算为主题作品添加多人游戏支持，并希望实现玩家邀请其他用户进行多人游戏的功能，则需要在 AppXManifest 文件中添加另一个字段。  请参阅[为多人游戏配置 AppXManifest](../multiplayer/service-configuration/configure-your-appxmanifest-for-multiplayer.md)

### <a name="6-add-internet-capabilities-to-your-visual-studio-project"></a>6. 将 Internet 功能添加到 Visual Studio 项目

1. 在 Visual Studio 2015 中双击 **package.appxmanifest** 文件，以打开清单设计器。
2. 单击**功能**选项卡
3. 单击 **Internet（客户端）**
4. 关闭文件并保存更改。

### <a name="7-change-the-sandbox-of-your-target-device"></a>7. 更改目标设备的沙盒

有关如何设置沙盒的信息，请参阅 [Xbox Live 沙盒](../xbox-live-sandboxes.md)。

### <a name="8-learn-more"></a>8. 了解更多

在 [https://github.com/Microsoft/xbox-live-samples](https://github.com/Microsoft/xbox-live-samples) 中的 /Samples/ID@Xbox/ 下找到的示例展示了开发人员可在 ID@Xbox 计划中使用的 API。  若要使用这些示例，你需要将沙盒更改为 XDKS.1
