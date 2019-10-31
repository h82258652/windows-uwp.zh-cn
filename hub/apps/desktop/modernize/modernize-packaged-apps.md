---
Description: 了解如何在已打包到 Windows 应用包的桌面应用程序中为 Windows 10 用户添加新式体验。
title: 现代化打包桌面应用
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ec1c56f205b270262f618ffb46db16b38276975d
ms.sourcegitcommit: d7eccdb27c22bccac65bd014e62b6572a6b44602
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73142506"
---
# <a name="features-that-require-package-identity"></a>需要包标识的功能

如果要使用[新式 Windows 10 体验](index.md)来更新桌面应用，则许多功能仅在具有[包标识](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)的桌面应用中可用。 可以通过多种方式将包标识授予桌面应用：

* 将其打包在[.msix 包](/windows/msix/desktop/desktop-to-uwp-root)中。 .MSIX 是一种新式应用包格式，可为所有 Windows 应用、WPF、Windows 窗体和 Win32 应用提供通用打包体验。 它提供可靠的安装和更新体验、具有灵活功能系统的托管安全模型、对 Microsoft Store、企业管理和许多自定义分发模型的支持。 有关详细信息，请参阅 MSIX 文档中的[打包桌面应用程序](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)。
* 如果无法采用 .MSIX 打包来部署桌面应用，请从 Windows 10 有问必答 Preview Build 10.0.19000.0 开始，可以通过创建只包含包清单的*稀疏 .msix 包*来授予包标识。 有关详细信息，请参阅[向非打包桌面应用授予标识](grant-identity-to-nonpackaged-apps.md)。

如果桌面应用具有包标识，你可以在应用中使用以下功能。

## <a name="use-uwp-apis-that-require-package-identity"></a>使用需要包标识的 UWP Api

以下 UWP Api 列表需要在桌面应用中使用包标识： [api 列表](desktop-to-uwp-supported-api.md#list-of-apis)。

## <a name="integrate-with-package-extensions"></a>集成包扩展

如果你的应用程序需要与系统集成（例如：建立防火墙规则），请在应用程序的包清单中描述这些内容，系统将执行其余操作。 对于其中大多数任务，你根本不必编写任何代码。 对于清单中的 XML，你可以执行一些操作，例如，在用户登录时启动进程，将应用程序集成到文件资源管理器，然后添加你的应用程序显示在其他应用中的打印目标的列表。

有关详细信息，请参阅[将桌面应用与包扩展集成](desktop-to-uwp-extensions.md)。

## <a name="extend-with-uwp-components"></a>使用 UWP 组件进行扩展

一些 Windows 10 体验（例如：启用触摸功能的 UI 页面）必须在现代应用容器内运行。 通常，你应该首先确定是否可以通过使用 UWP Api[增强](desktop-to-uwp-enhance.md)现有的桌面应用程序来添加体验。 如果必须使用 UWP 组件来实现体验，则可以将 UWP 项目添加到解决方案，并使用应用服务在桌面应用程序和 UWP 组件之间进行通信。

有关详细信息，请参阅[用 UWP 组件扩展桌面应用](desktop-to-uwp-extend.md)。

## <a name="distribute"></a>分配

如果你将应用打包在 .MSIX 包中，则可以通过将其发布到 Microsoft Store 或将其旁加载到其他系统来分发应用。

请参阅[分发打包的桌面应用](desktop-to-uwp-distribute.md)。
