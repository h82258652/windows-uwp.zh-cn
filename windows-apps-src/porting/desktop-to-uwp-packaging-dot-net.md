---
author: normesta
Description: This guide explains how to configure your Visual Studio Solution to edit, debug, and package desktop application.
Search.Product: eADQiWindows 10XVcnh
title: 使用 Visual Studio 打包的桌面应用程序
ms.author: normesta
ms.date: 08/30/2017
ms.topic: article
keywords: windows 10，uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: 1fb6a552686e35f319d9624ba4c4663dad966f23
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5640874"
---
# <a name="package-a-desktop-application-by-using-visual-studio"></a>使用 Visual Studio 打包的桌面应用程序

可以使用 Visual Studio 为你的桌面应用生成一个包。 然后，可将该包发布到 Microsoft Store 或将其旁加载到一台或多台电脑中。

Visual Studio 的最新版本提供了新版本的打包项目，能够消除在打包应用时必需的所有手动步骤。 只需添加打包项目，参考桌面项目，再按 F5 进行应用调试。 无需手动调整。 相比于使用以往版本的 Visual Studio 的体验，新的简洁体验是一个巨大的改进。

>[!IMPORTANT]
>创建 Windows 应用包的桌面应用程序的功能 （Windows 10 版本 1607年中引入了称为桌面桥，否则，它仅用于在项目中面向 Windows 10 周年更新 (10.0;内部版本 14393） 或更高版本的 Visual Studio。

## <a name="first-prepare-your-application"></a>首先，准备应用程序

在开始为你的应用程序创建程序包之前查看本指南：[准备打包的桌面应用程序](desktop-to-uwp-prepare.md)。

<a id="new-packaging-project"/>

## <a name="create-a-package"></a>创建程序包

1. 在 Visual Studio 中，打开包含你的桌面应用程序项目的解决方案。

2. 向解决方案中添加一个 **Windows 应用程序包项目**项目。

   你无需向该项目中添加任何代码。 该项目只用于为你生成包。 我们将此项目称为“打包项目”。

   ![打包项目](images/desktop-to-uwp/packaging-project.png)

   >[!NOTE]
   >此项目仅出现在 15.5 版或更高版本的 Visual Studio 2017 中。

3. 将此项目的**目标版本**设置为你需要的任何版本，但请务必将**最低版本**设置为 **Windows 10 周年更新**。

   ![打包版本选择器对话框](images/desktop-to-uwp/packaging-version.png)

4. 在打包项目中，右键单击**应用程序**文件夹，然后选择**添加引用**。

   ![添加项目引用](images/desktop-to-uwp/add-project-reference.png)

5. 选择你的桌面应用程序项目，然后选择**确定**按钮。

   ![桌面项目](images/desktop-to-uwp/reference-project.png)

   你可以在程序包中包括多个桌面应用程序，但在用户选择应用磁贴时只能启动其中一个应用程序。 在**应用程序**节点中，右键单击你希望用户在选择应用磁贴时启动的应用程序，然后选择**设置为入口点**。

   ![设置入口点](images/desktop-to-uwp/entry-point-set.png)

6. 生成打包项目，以确保未显示任何错误。  如果你收到错误，打开**配置管理器**，并确保你的项目面向相同的平台。

   ![配置管理器](images/desktop-to-uwp/config-manager.png)

7. 使用[创建应用包](../packaging/packaging-uwp-apps.md)向导生成 appxupload 文件。

   你可以直接将该文件上传到 Microsoft Store 中。

**视频**

&nbsp;
> [!VIDEO https://www.youtube.com/embed/fJkbYPyd08w]

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。

**运行、 调试或测试的桌面应用程序**

请参阅[运行、 调试和测试打包的桌面应用程序](desktop-to-uwp-debug.md)

**通过添加 UWP Api 来增强桌面应用程序**

请参阅[增强用于 Windows 10 的桌面应用程序](desktop-to-uwp-enhance.md)

**通过添加 UWP 项目和 Windows 运行时组件来扩展桌面应用程序**

请参阅[使用新式 UWP 组件扩展桌面应用程序](desktop-to-uwp-extend.md)。

**分发应用**

请参阅[分布的已打包的桌面应用程序](desktop-to-uwp-distribute.md)
