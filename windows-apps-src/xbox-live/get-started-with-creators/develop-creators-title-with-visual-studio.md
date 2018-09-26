---
title: 使用 Visual Studio 开发创意者主题作品
author: aablackm
description: 使用 Visual Studio 开发 Xbox Live 创意者计划主题作品入门
ms.assetid: 6952dac0-66ff-4717-b3c7-8b3792e834e3
ms.author: aablackm
ms.date: 11/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, xbox live 创意者, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: 19ee781a7143c0da5b1549776646ab5b133a9667
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2018
ms.locfileid: "4211340"
---
# <a name="get-started-developing-an-xbox-live-creators-program-title-with-visual-studio"></a>使用 Visual Studio 开发 Xbox Live 创意者计划主题作品入门

> [!NOTE]
> 有一个插件可用于使用 Unity 开发的游戏。 有关详细信息，请参阅[使用 Unity 开发创意者主题作品](develop-creators-title-with-unity.md)文章。

## <a name="requirements"></a>要求

1. 注册**[开发人员中心开发人员计划](https://developer.microsoft.com/store/register)**。
2. **[Windows 10](https://microsoft.com/windows)**。
3. **[Visual Studio 2015](https://www.visualstudio.com/)**（或更高版本）与**通用 Windows 应用开发工具**。
4. **[Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) v10.0.10586.0** 或更高版本。

> [!IMPORTANT]
> 如果使用的是 Windows 10 SDK 版本 10.0.15063.0（也称为创作者更新）或更高版本，则需要使用 Visual Studio 2017。

## <a name="create-a-new-product-on-microsoft-dev-center"></a>在 Microsoft 开发人员中心创建新产品

每个 Xbox Live 主题作品必须先在[Microsoft 开发人员中心](https://developer.microsoft.com/store)上创建产品，然后才能登录并进行 Xbox Live 服务调用。 有关详细信息，请参阅[创建新创意者主题作品](create-and-test-a-new-creators-title.md)。

## <a name="configuring-your-development-device"></a>配置开发设备

需要在你的设备上完成以下初步设置步骤，以便成功登录 Xbox Live 并调用各种 Xbox Live 服务。

### <a name="set-your-sandbox"></a>设置沙盒

沙盒可以提供一种方法，使 [Xbox Live 服务配置](xbox-live-service-configuration-creators.md)与零售版本隔离，直到你准备发布主题作品为止。 你累积的一些数据是特定于沙盒的。 例如，假设你的主题作品定义了一个名为*头像*的统计数据，并且在测试主题作品时，你在用户帐户中累积了一定数量的头像。 该值将特定于你主题作品的开发沙盒。如果你改玩零售版本的主题作品，则不会保留这些头像。

请参阅文章 [Xbox Live 沙盒](xbox-live-sandboxes-creators.md)，以了解详细信息并查看如何设置沙盒。

### <a name="sign-in-with-an-xbox-live-account-that-has-been-authorized-for-testing"></a>使用已获得测试授权的 Xbox Live 帐户登录

要登录到开发沙盒，你必须预配一个常规的 Microsoft 帐户 (MSA) 来访问你的沙盒。 这样有助于提高开发中主题作品的安全性，而且还能带来其他好处。

要了解有关测试帐户以及如何创建测试帐户的详细信息，请参阅[授权 Xbox Live 帐户在你的环境中进行测试](authorize-xbox-live-accounts.md)

## <a name="visual-studio-project-setup"></a>设置 Visual Studio 项目

### <a name="1-open-a-uwp-project"></a>1. 打开 UWP 项目
如果你还没有现成的 UWP 项目，可以通过以下方法创建一个：

1. 在 Visual Studio 中，单击**文件** > **新建** > **项目**。
2. 在**新建项目**对话框中，在左侧窗格中依次选择 **Visual C#** > **Windows** > **通用**节点，然后在右侧窗格中单击**空白应用（通用 Windows）**。
3. 在对话框下半部分，指定项目名称，然后指定项目的位置。
4. 指定 Windows 10 SDK 的目标版本和最低版本。 有关详细信息，请参阅[选择 UWP 版本](https://docs.microsoft.com/windows/uwp/updates-and-versions/choose-a-uwp-version)。

![在 VS 中创建项目](../images/getting_started/vs-create-project.gif)

> [!NOTE]
> > Xbox Live API (XSAPI) 需要使用最低版本 10.0.10586.0 或更高版本。

### <a name="2-add-references-to-the-xbox-live-api-xsapi-in-your-project"></a>2. 在项目中添加对 Xbox Live API (XSAPI) 的引用
Xbox 服务 API 同时支持 C++ 和 WinRT，其命名结构的形式为 **Microsoft.Xbox.Live.SDK.*.UWP**。 有关在 Xbox One 上运行 UWP 的更多信息，请参阅 [https://docs.microsoft.com/windows/uwp/xbox-apps/getting-started](https://docs.microsoft.com/windows/uwp/xbox-apps/getting-started)。 C++ SDK 可用于 C++ 游戏引擎，而 WinRT SDK 用于以 C++、C# 或 JavaScript 编写的游戏引擎。 在将 WinRT 与 C++ 引擎结合使用时，将使用有乘幂号 (^) 的 C++/CX。 C++ 是建议用于 C++ 游戏引擎的 API。  

要从你的项目使用 Xbox Live API，可以通过使用 NuGet 程序包或添加 API 源添加对二进制文件的引用。 添加 NuGet 程序包可加快编译速度，而添加源可简化调试。 本文将演示如何使用 NuGet 程序包。 如果要使用源，请参阅[在 UWP 项目中编译 Xbox Live API 源](../get-started-with-partner/add-xbox-live-apis-source-to-a-uwp-project.md)。 可通过以下方式添加 Xbox Live SDK NuGet 程序包：

1. 在 Visual Studio 中，转到**工具** > **NuGet 包管理器** > **管理解决方案的 NuGet 程序包...**。
2. 在 NuGet 包管理器中，单击**浏览**并在搜索框中输入 **Xbox.Live.SDK**。
3. 从左侧列表中选择要使用的 Xbox Live SDK 版本。 在本例中，我们将使用 Microsoft.Xbox.Live.SDK.WinRT.UWP 程序包。
3. 在窗口右侧，选中你的项目旁的复选框并单击**安装**。

![通过 NuGet 添加 XBL](../images/getting_started/vs-add-nuget-xbl.gif)

#### <a name="optionally-include-xsapi-header-in-your-project"></a>（可选）在项目中包括 XSAPI 标头

对于基于 Microsoft.Xbox.Live.SDK.Cpp.* 的项目，需要在 C++ 项目中包括 `xsapi\\services.h`，以引入 Xbox Live 服务 API (XSAPI) NuGet 程序包的标头。 包括 XSAPI 标头之前，必须定义 `XBOX_LIVE_CREATORS_SDK`。 这会将 API 图面区域限制为仅可由 Xbox Live 创意者计划中的开发人员使用的 API。 例如：

```c++
#define XBOX_LIVE_CREATORS_SDK
#include "xsapi\services.h"
```
### <a name="3-optional-using-connected-storage"></a>3.（可选）使用连接存储
如果你想要使用[连接存储](../storage-platform/connected-storage/connected-storage-technical-overview.md)服务，你需要访问`Windows.Gaming.XboxLive.Storage`命名空间。 根据你所使用的 Windows SDK 版本，可能需要安装额外的内容，或者手动将引用添加到使用它的项目中。 如果你已经以 Windows 10 SDK 10.0.16299 或更高版本作为目标，那么你将能够访问连接存储命名空间，而无需执行任何其他工作。

#### <a name="windows-10-sdk-version-10015063-or-lower"></a>Windows 10 SDK 版本 10.0.15063 或更低版本
如果你想要使用连接存储，将需要安装 Xbox Live 平台扩展 SDK，然后才能将引用添加到项目中。 你可以通过以下方式来执行此操作：

1. 下载 [Xbox Live 平台扩展 SDK](http://aka.ms/xblextsdk)并解压缩。
2. 解压缩完成后，运行其中与你所使用的 Windows 10 SDK 版本匹配的 MSI 文件。

安装完 Xbox Live 平台扩展 SDK 后，需要在 Visual Studio 中向它添加引用。 你可以通过以下方式来执行此操作：

1. 在**解决方案资源管理器**中，右键单击**引用**节点，然后选择**添加引用...**。
2. 在**引用管理器**对话框的左侧，选择**通用 Windows** > **扩展**。
3. 在显示的列表中，搜索**适用于 UWP 的 Windows 桌面扩展**并选择与你的 Windows 10 SDK 匹配的版本旁边的复选框。
4. 单击**确定**。

![在 VS 中添加新引用](../images/getting_started/get-started-vs-add-ref.png)

### <a name="4-associate-your-visual-studio-project-with-your-uwp-app"></a>4. 将 Visual Studio 项目与 UWP 应用关联

为了使你的游戏支持登录，必须将其与你在 Microsoft 开发人员中心上创建的产品关联。 您可以通过使用 Microsoft Store 关联向导在 Visual Studio 中关联你的游戏。 在 Visual Studio 中，执行以下操作：

1.  右键单击主项目（启动项目），然后单击 **Store** > **Associate App with the Store...**
2.  如果需要，使用用于创建应用的 **Windows 开发人员帐户**登录并按照提示进行操作。

> [!TIP]
> 有关准备 Microsoft Store 游戏的详细信息，请参阅[打包应用](https://docs.microsoft.com/windows/uwp/packaging/)。

### <a name="5-add-internet-capabilities-to-your-visual-studio-project"></a>5. 将 Internet 功能添加到 Visual Studio 项目
UWP 项目需要指定 Internet 功能与 Xbox Live 通信。 你可以通过以下方法来设置这些属性：

1. 在 Visual Studio 中双击 **package.appxmanifest** 文件，以打开**清单设计器**。
2. 单击**功能**选项卡，然后检查 **Internet（客户端）**。

![在 VS 中添加新引用](../images/getting_started/get-started-vs-add-capability.png)

### <a name="6-associate-your-visual-studio-project-with-your-xbox-live-enabled-title"></a>6. 将 Visual Studio 项目与支持 Xbox Live 的主题作品关联

若要与 Xbox Live 服务通信，需要向项目中添加服务配置文件。 可通过以下方法轻松完成此操作：

1. 在启动项目中，单击右键然后依次选择**添加** > **新项目**。
2. 选择**文本文件**类型，并将其命名为 **xboxservices.config**。
3. 右键单击文件，然后选择**属性**并确保：
    1. **生成操作**已设置为**内容**，并且  
    2. **复制到输出目录**已设置为**始终复制**。
5.  使用以下模板编辑配置文件，并使用适用于你的主题作品的值替换 **TitleId** 和 **PrimaryServiceConfigId**。 在 Microsoft 开发人员中心的根 Xbox Live 页面上，可以获取正确值。 **PrimaryServiceConfigId** 在 Microsoft 开发人员中心上显示为 **SCID**。

```json
    {
       "TitleId" : "your title ID (JSON number in decimal)",
       "PrimaryServiceConfigId" : "your primary service config ID",
       "XboxLiveCreatorsTitle" : true
    }
```

例如：

```json
    {
        "TitleId" : 1563044810,
        "PrimaryServiceConfigId" : "12200100-88da-4d8b-af88-e38f5d2a2bca",
        "XboxLiveCreatorsTitle" : true
    }
```

> [!TIP]
> Xboxservices.config 中的所有值都区分大小写。 有关获取 TitleID 和 PrimaryServiceConfigId 的详细信息，请参阅[服务配置](../xbox-live-service-configuration.md)。

## <a name="learn-more"></a>了解详细信息

[Xbox Live SDK 示例](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/CreatorsSDK)下展示了为 Xbox Live 创意者计划中的开发人员提供的 API。 若要使用这些示例，需要将沙盒更改为 XDKS.1。
