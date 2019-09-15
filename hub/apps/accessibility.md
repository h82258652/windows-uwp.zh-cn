---
title: Windows 10 中的辅助功能
description: 本页提供有关如何开始开发可访问的 Windows 应用的信息。
ms.topic: article
ms.date: 09/12/2019
keywords: Windows 10 中的辅助功能，可访问性，构建可访问的 win32 应用，构建可访问的 UWP 应用，构建可访问的 WPF 应用，构建可访问的 WinForms
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: 76bbd3f0e04bbb2f729ad0950bae190b2fffb6ac
ms.sourcegitcommit: 6e7665b457ec4585db19b70acfa2554791ad6e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/14/2019
ms.locfileid: "70987189"
---
# <a name="accessibility-in-windows-10"></a>Windows 10 中的辅助功能

![hero-accessibility-bar-smaller .png](images/hero-accessibility-bar-smaller.png)

## <a name="build-accessibility-into-your-applications-to-empower-people-of-all-abilities"></a>在应用程序中构建可访问性，以使所有功能的人员能够

当产品和服务（包括电子介质）设计为尽可能多的人提供完全和成功体验时，它们可供访问。

为残障人士（临时和永久）、个人首选项、特定工作样式或形势约束（如共享工作空间）生成可访问的和包含的 Windows 应用程序，并改进了功能和可用性。驱动、烹饪、防眩等）。 一些常见的解决方案包括以替代格式（如视频标题）提供信息或启用辅助技术（如屏幕阅读器）。

**无论是需要使用楼梯还是电梯，每个人都应该可以访问大楼中的相同房间。**

此页提供有关各种 Windows 开发框架如何为生成 Windows 应用程序的开发人员提供辅助功能支持的信息、辅助技术开发人员（如屏幕阅读器和放大器），以及软件测试工程师创建用于测试应用程序的自动化脚本。

## <a name="platform-specific-documentation"></a>特定于平台的文档

:::row:::
   :::column:::
      ![通用 Windows 平台 (UWP)](images/platform-uwp.png)

      **通用 Windows 平台 (UWP)**

      在适用于任何 Windows 设备（包括电脑、手机、Xbox One、HoloLens 等）的现代平台上开发可访问的应用程序和工具，并将其发布到 Microsoft Store。

      [设计非独占软件](https://docs.microsoft.com/windows/uwp/accessibility/designing-inclusive-software)

      [开发非独占 Windows 应用](https://docs.microsoft.com/windows/uwp/accessibility/developing-inclusive-windows-apps)

      [辅助功能测试](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-testing)

      [应用商店中的辅助功能](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-in-the-store)
   :::column-end:::
   :::column:::
      ![Win32 平台应用](images/platform-win32.png)

      **Win32 平台**

      在适用于 C/C++ Windows 应用程序的原始平台上开发可访问的应用程序和工具。

      [Windows 辅助功能和自动化中的新增功能](https://docs.microsoft.com/windows/desktop/accessibility-whatsnew)

      [开发可访问 Windows 应用程序](https://docs.microsoft.com/windows/desktop/accessibility-appdev)

      [开发可访问的适用于 Windows 的 UI 框架](https://docs.microsoft.com/windows/desktop/accessibility-uiframeworkdev)

      [开发适用于 Windows 的辅助技术](https://docs.microsoft.com/windows/desktop/accessibility-atdev)

      [测试辅助功能](https://docs.microsoft.com/windows/desktop/accessibility-testwithuia)

      [旧版辅助功能和自动化技术-通过 MSAA 实现 UI 自动化](https://docs.microsoft.com/windows/desktop/accessibility-legacy)

      [Windows 辅助功能](https://docs.microsoft.com/windows/desktop/winauto/about-windows-accessibility-features)

      [设计辅助应用指南](https://docs.microsoft.com/windows/desktop/uxguide/inter-accessibility)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![WPF 平台](images/platform-wpf2-small.png)

      **Windows Presentation Foundation （WPF）**

      在使用 XAML UI 模型和 .NET Framework 的托管 Windows 应用程序的已建立平台上开发可访问的应用程序和工具。

      [辅助功能最佳做法](https://docs.microsoft.com/dotnet/framework/ui-automation/accessibility-best-practices)

      [UI 自动化基础知识](https://docs.microsoft.com/dotnet/framework/ui-automation/index)

      [托管代码的 UI 自动化提供程序](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-providers-for-managed-code)

      [托管代码的 UI 自动化客户端](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-clients-for-managed-code)

      [UI 自动化控件模式](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-patterns)

      [UI 自动化文本模式](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-text-pattern)

      [UI 自动化控件类型](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-types)

      [UI 自动化规范和社区承诺](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-specification-and-community-promise)
   :::column-end:::
   :::column:::
      ![Windows 窗体平台应用](images/platform-winforms.png)

      **Windows 窗体（WinForms）**

      使用 XAML UI 模型和 .NET Framework 为托管 Windows 应用程序开发可访问的应用程序和工具。

      [Windows 窗体辅助功能](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)

      [创建可访问的 Windows 应用程序](https://docs.microsoft.com/dotnet/framework/winforms/advanced/walkthrough-creating-an-accessible-windows-based-application)

      [支持辅助功能准则 Windows 窗体控件上的属性](https://docs.microsoft.com/dotnet/framework/winforms/advanced/properties-on-windows-forms-controls-that-support-accessibility-guidelines)

      [为 Windows 窗体上的控件提供辅助功能信息](https://docs.microsoft.com/dotnet/framework/winforms/controls/providing-accessibility-information-for-controls-on-a-windows-form)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **Web 辅助功能**

      在 Microsoft Edge 中设计、构建和测试可访问的网站。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Web 辅助功能简介](https://docs.microsoft.com/microsoft-edge/accessibility)

      [设计辅助网站](https://docs.microsoft.com/microsoft-edge/accessibility/design)
   :::column-end:::
   :::column:::
      [生成辅助网站](https://docs.microsoft.com/microsoft-edge/accessibility/build)

      [测试可访问的网站](https://docs.microsoft.com/microsoft-edge/accessibility/test)
   :::column-end:::
:::row-end:::

## <a name="samples"></a>示例

下载并运行完整的 Windows 示例，这些示例演示了各种辅助功能。

:::row:::
   :::column:::
      [代码示例浏览器](https://docs.microsoft.com/en-us/samples/browse/)

      新示例浏览器取代了 MSDN 代码库。
   :::column-end:::
   :::column:::
      [MSDN 代码库（已停用）](https://code.msdn.microsoft.com/site/search?query=accessibility&f%5B0%5D.Value=accessibility&f%5B0%5D.Type=SearchText&ac=2)

      下载适用于 Windows、Windows Phone、Microsoft Azure、Office、SharePoint、Silverlight 和其他产品的示例。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [GitHub 上的 Windows 经典示例](https://github.com/microsoft/Windows-classic-samples/search?q=accessibility&unscoped_q=accessibility)

      这些示例演示了 Windows 和 Windows Server 的功能和编程模型。 
   :::column-end:::
   :::column:::
      [GitHub 上的通用 Windows 平台（UWP）示例](https://github.com/microsoft/Windows-universal-samples/search?q=accessibility&unscoped_q=accessibility)

      这些示例演示适用于 Windows 10 的 Windows 软件开发工具包（SDK）中的通用 Windows 平台（UWP）的 API 使用模式。
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [XAML 控件库](https://github.com/microsoft/Xaml-Controls-Gallery)

      此应用演示了在熟知设计系统中支持的各种 Xaml 控件。
   :::column-end:::
:::row-end:::

## <a name="videos"></a>视频

各种视频涵盖了如何构建可访问的 Windows 应用程序，以了解常见的辅助功能，以及 Microsoft 如何解决这些问题。

:::row:::
   :::column:::
      **如何在 Windows 应用程序中开始利用辅助功能**
   :::column-end:::
   :::column:::
      **一分钟的开发人员：开发辅助功能的应用**
   :::column-end:::
   :::column:::
      **残疾和辅助功能简介**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2017/P4072/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/Kl4CT4DaypM]
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      **从黑客到产品，适用于 Windows 10 的目视控制**
   :::column-end:::
   :::column:::
      **Windows 10 上的辅助功能**
   :::column-end:::
   :::column:::
      **构建可访问的 UWP 应用简介**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/AShNPfmAkvY]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2016/P541/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2016/P497/player]
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      **设计 Windows 辅助功能**
   :::column-end:::
   :::column:::
      **Windows 10 辅助功能提供每个人的功能**
   :::column-end:::
   :::column:::
      **使鼠标指针更易于查看**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/Y_NJbE7wxlk]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/BseTf-4q9GA]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/4UzaF7_T3bw]
   :::column-end:::
:::row-end:::

## <a name="other-resources"></a>其他资源

:::row:::
   :::column span="3":::
      **博客和新闻**

      Microsoft 辅助功能领域的最新功能。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [新闻](https://news.microsoft.com/presskits/accessibility/)
   :::column-end:::
   :::column:::
      [辅助功能博客](https://blogs.microsoft.com/accessibility/)
   :::column-end:::
   :::column:::
      [Windows UI 自动化博客](https://blogs.msdn.microsoft.com/winuiautomation/)
   :::column-end:::
:::row-end:::

:::row:::
   :::column span="3":::
      **社区和支持**

      Windows 开发人员和用户共同见面和学习的地方。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Windows 社区-辅助功能](https://community.windows.com/search?q=accessibility)
   :::column-end:::
   :::column:::
      [Windows 辅助功能和自动化开发论坛](https://social.msdn.microsoft.com/Forums/windows/home?forum=windowsaccessibilityandautomation)
   :::column-end:::
   :::column:::
      [伤残咨询台](https://www.microsoft.com/Accessibility/disability-answer-desk)
   :::column-end:::
:::row-end:::
