---
Description: 了解如何在已打包在 Windows 应用包中的桌面应用程序中添加对 Windows 10 用户的新式体验。
title: 实现打包桌面应用程序的现代化
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6d71233bc7b96af9d9b261406d6b149f36f65f29
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985287"
---
# <a name="features-that-require-package-identity"></a>需要包标识的功能。

如果你想要更新使用对桌面应用程序[现代 Windows 10 体验](index.md)，许多功能都仅在桌面应用程序打包在 MSIX 包中可用。

MSIX 是为所有 Windows 应用程序、 WPF、 Windows 窗体和 Win32 应用提供了通用的打包体验的现代 Windows 应用包格式。 打包桌面 Windows 应用程序，可将动态磁贴和通知等现代 Windows 10 体验集成到您的应用程序。 它还获取访问权限的可靠的安装和更新体验、 具有灵活功能系统，支持 Microsoft Store、 企业管理和自定义分布的多个模型的受管理的安全模型。 有关详细信息，请参阅[桌面应用程序打包](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)MSIX 文档中。

如果您打包桌面应用程序，然后可以使用需要的包标识、 包扩展和 UWP 组件打包应用程序中的 UWP Api。 有关详细信息，请参阅以下文章。

## <a name="use-uwp-apis-that-require-package-identity"></a>使用 UWP Api 要求包标识

有些 UWP Api 需要[打包标识](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)桌面应用程序中使用。 MSIX 程序包 （包括包清单） 提供了此标识。

有关详细信息，请参阅[这一系列 Api](desktop-to-uwp-supported-api.md#list-of-apis)。

## <a name="integrate-with-package-extensions"></a>集成包扩展

如果你的应用程序需要与系统集成 (例如： 建立防火墙规则)、 描述您的应用程序在包清单中的这些事物和系统将完成其余部分。 对于其中大多数任务，你根本不必编写任何代码。 使用少量的 XML 清单中，您可以执行操作像在用户登录时启动的进程，将你的应用程序集成到文件资源管理器，并添加你的应用程序出现在其他应用中的打印目标的列表。

有关详细信息，请参阅[将对桌面应用程序集成包扩展](desktop-to-uwp-extensions.md)。

## <a name="extend-with-uwp-components"></a>使用 UWP 组件扩展

一些 Windows 10 体验（例如：启用触摸功能的 UI 页面）必须在现代应用容器内运行。 一般情况下，应首先确定是否可以添加你的体验[增强](desktop-to-uwp-enhance.md)UWP Api 与您现有桌面应用程序。 如果您必须使用 UWP 组件，将获得的体验，你可以将 UWP 项目添加到你的解决方案和应用服务用于桌面应用程序和 UWP 组件之间进行通信。

有关详细信息，请参阅[扩展桌面应用程序与 UWP 组件](desktop-to-uwp-extend.md)。

## <a name="distribute"></a>分配

您可以将你的应用程序通过 Microsoft Store 发布或通过旁加载到其他系统。

请参阅[分发打包桌面应用程序](desktop-to-uwp-distribute.md)。
