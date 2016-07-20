---
Description: "下面是创建可在任何基于 Windows 10 的设备（包括手机、平板电脑和 PC）上运行的通用 Windows 应用所需的信息。"
title: "Windows 10 应用的操作方法指南 - Windows 应用开发"
ms.assetid: 2A39F3D8-85AD-4315-A69B-2B79242780E3
redirect_url: https://developer.microsoft.com/windows/develop
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 654eac5df1bd3309714348dcfc511850234bb0ae

---


# Windows 10 应用的操作方法指南

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

下面是创建可在任何基于 Windows 10 的设备（包括手机、平板电脑和 PC）上运行的通用 Windows 应用所需的信息。 本部分中提供的说明和代码示例均按你需完成的任务类别进行组织。

如果你希望大致了解通用 Windows 平台 (UWP) 以及它如何让你能够使用相同的代码正确地将定制体验传递给多种 Windows 设备类型，请参阅以下文章：

-   创建你的第一个通用 Windows 应用
-   通用 Windows 平台 (UWP) 应用指南
-   什么是通用 Windows 应用？

| 主题 | 说明 |
|-------|-------------|
| [应用到应用的通信](app-to-app/index.md) | 了解通用 Windows 应用（包括 Windows Web 应用）如何启动其他应用，以及如何交换数据和文件。 往常需要用户管理多个应用的复杂任务现在可无缝进行处理。 |
| [音频、视频和相机](audio-video-camera/index.md) | 用于在应用中从捕获设备（例如摄像头）捕获照片和视频，以及呈现音频流。 |
| [联系人和日历](contacts-and-calendar/index.md) | 允许用户从你的应用访问其 Windows 联系人和日历约会，以便他们可以共享内容、电子邮件和日历信息或者发送消息，而无需在 Windows 应用之间切换。|
| [数据访问](data-access/index.md) | 了解在专用数据库中存储设备上的数据，并在通用 Windows 平台 (UWP) 应用中使用对象关系映射。 |
| [数据绑定](data-binding/index.md) | 用于将通用 Windows 应用的 UI 元素与不同的数据源进行同步，其中包括数据库、文件以及内部对象，以提供数据驱动的用户体验。 |
| [调试、测试和性能](debug-test-perf/index.md) | 了解测试和调试周期，以及如何使用 Microsoft Visual Studio 随附的或单独下载的相关工具。 请确保你的通用 Windows 应用提供了你所期望的体验，并且已准备好发布到 Windows 应用商店。 |
| [设备、传感器和电源](devices-sensors\index.md) | 用于将不同设备（如打印机、相机和传感器）集成到你的通用 Windows 应用中，以便为用户提供既可靠又灵活的连接设备体验。 | 
| [企业版](enterprise/index.md) | 了解 Windows 10 通用 Windows 平台 (UWP) 应用的关键企业功能。 |
| [文件、文件夹和库](files/index.md) | 了解如何在文件中读取和写入文本及其他数据格式，并管理文件和文件夹。 还需了解有关应用设置的读取和写入、文件和文件夹选取器，以及诸如视频/音乐库等特殊“沙盒式”位置的信息。 |
| [游戏和 DirectX](https://msdn.microsoft.com/library/windows/apps/mt228375.aspx) | 了解在新的通用 Windows 平台 (UWP) 上创建游戏的基础知识。 |
| [图形和动画](graphics/index.md) | 通过使用 UI 图形和动画增强你的通用 Windows 应用，从而以视觉方式让用户参与到用户体验中并产生兴趣。 |
| [启动、恢复和后台任务](launch-resume/index.md) | 通过创建后台任务并注册系统生成的事件来提供相关功能，即使通用 Windows 应用已暂停或不在运行也是如此。 |
| [地图与位置](maps-and-location/index.md) | 了解通用 Windows 应用如何点击进入必应地图服务，并生成精确的地图视觉效果，其上现在还提供 3D 鸟瞰图和街景视图。 |
| [获取应用收益](monetize\index.md) | 通过创建免费应用、试用应用（基于时间和基于功能均可）、付费应用和应用内产品，让你的客户在应用体验期内既能免费试用你的应用，又能进行购买。 |
| [网络和 Web 服务](networking\index.md) | 通过创建可使用可用网络连接的已连接的或网络感知的通用 Windows 应用，执行诸如获取 RSS 源、加入多人游戏或与附近设备交互等操作。 |
| [打包应用](packaging\index.md) | 了解包含组成你的通用 Windows 应用的文件的应用包，以及如何通过 Windows 应用商店将其用于部署、管理和更新你的应用。 此外，了解有关必须在应用包清单中声明才可以访问特定资源的应用功能。 |
| [将应用移植到 Windows 10](porting\index.md) | 将现有应用引入 UWP，你可以在其上创建这样一种程序包：既面向你选择的基于 Windows 的设备，又集各种功能和每种设备类型所独有的用户体验于一身。 |
| [安全性](security/index.md) | 管理敏感型用户信息并帮助保护应用数据和资源的安全，同时保持完整的用户体验。 诸如基本密码保护、漫游凭据、单一登录、Microsoft 帐户身份验证和加密等功能垂手可得。 |
| [线程和异步编程](threading-async/index.md) | 使用异步编程可帮助你的应用保持响应，即当它完成可能花费较长时间的其他工作时允许它继续运行并响应 UI。 |
| [Windows 运行时组件](winrt-components/index.md) | 了解有关自包含对象的详细信息，可将这些对象实例化，并且可采用包括 C#、Visual Basic、JavaScript 和 C++ 在内的任一语言使用它们。 例如，你可以使用 C++ 创建 Windows 运行时组件，以便可使用第三方库来执行很耗计算的操作，也可以只在你的通用 Windows 应用中重新使用部分 Visual Basic 或 C# 代码。 
| [XAML 平台](xaml-platform/index.md) | 了解 XAML 编程语言的基本概念。 或者，如果你已熟悉 XAML，可跳至后面部分，了解如何通过使用 Visual Studio 实现采用 XAML 的 Windows 运行时功能，从而创建出色的通用 Windows 应用。 |



<!--HONumber=Jul16_HO2-->


