---
Description: 了解如何在打包到 Windows 应用包的桌面应用程序中向 Windows 10 用户提供新式体验。
title: 为打包的桌面应用提供新式体验
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ec1c56f205b270262f618ffb46db16b38276975d
ms.sourcegitcommit: d7eccdb27c22bccac65bd014e62b6572a6b44602
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73142506"
---
# <a name="features-that-require-package-identity"></a>需要包标识的功能

如果要将桌面应用更新为[新式 Windows 10 体验](index.md)，请注意，许多功能仅在具有[程序包标识符](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)的桌面应用中可用。 可通过多种方式向桌面应用授予程序包标识符：

* 将其打包到 [MSIX 包](/windows/msix/desktop/desktop-to-uwp-root)中。 MSIX 是一种新式应用包格式，提供适合所有 Windows 应用、WPF、Windows 窗体和 Win32 应用的通用打包体验。 它提供了可靠的安装和更新体验、功能系统灵活的托管安全模型、对 Microsoft Store 的支持、企业管理以及许多自定义分发模型。 有关详细信息，请参阅 MSIX 文档中的[打包桌面应用程序](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)。
* 如果无法采用 MSIX 打包来部署桌面应用，自 Windows 10 预览体验计划预览版本 10.0.19000.0 起，你可通过创建一个仅包含程序包清单的稀疏 MSIX 包来授予包标识  。 有关详细信息，请参阅[向未打包的桌面应用授予标识](grant-identity-to-nonpackaged-apps.md)。

如果你的桌面应用具有程序包标识符，可以在其中使用以下功能。

## <a name="use-uwp-apis-that-require-package-identity"></a>使用需要程序包标识符的 UWP API

以下 UWP API 要求在桌面应用中使用程序包标识符：[API 列表](desktop-to-uwp-supported-api.md#list-of-apis)。

## <a name="integrate-with-package-extensions"></a>集成包扩展

如果应用程序需要与系统集成（例如，建立防火墙规则），请在应用程序的包清单中描述集成任务，系统将完成其余操作。 对于其中的大多数任务，根本不必编写任何代码。 在清单中添加少量的 XML 后，可以执行一些操作，例如，在用户登录时启动进程、将应用程序集成到文件资源管理器中，以及为应用程序添加显示在其他应用中的打印目标列表。

有关详细信息，请参阅[将桌面应用与包扩展集成](desktop-to-uwp-extensions.md)。

## <a name="extend-with-uwp-components"></a>使用 UWP 组件进行扩展

一些 Windows 10 体验（例如：启用触摸功能的 UI 页面）必须在现代应用容器内运行。 一般情况下，首先应确定是否可以通过 UWP API [增强](desktop-to-uwp-enhance.md)现有桌面应用程序来增加你的体验。 如果必须使用 UWP 组件来实现体验，可将 UWP 项目添加到解决方案，并使用应用服务在桌面应用程序与 UWP 组件之间通信。

有关详细信息，请参阅[使用 UWP 组件扩展桌面应用](desktop-to-uwp-extend.md)。

## <a name="distribute"></a>分发

如果在 MSIX 包中打包应用，可以通过将应用发布到 Microsoft Store 或将其旁加载到其他系统来分发应用。

请参阅[分发打包的桌面应用](desktop-to-uwp-distribute.md)。
