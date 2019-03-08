---
ms.assetid: 08C8F359-E8B6-4A45-8F4B-8A1962F0CE38
description: Microsoft Visual Studio 与 Windows 的关系就像 Xcode 与 iOS 和 Mac OS 的关系一样。 在本演练中，我们将帮助你熟悉如何使用 Visual Studio。
title: 在 Visual Studio 中创建项目
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1b6ea9fdf2e504e1ceee71658eab308751e1745c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601442"
---
# <a name="getting-started-creating-a-project"></a>入门：创建项目

## <a name="creating-a-project"></a>创建项目

Microsoft Visual Studio 与 Windows 的关系就像 Xcode 与 iOS 和 Mac OS 的关系一样。 在本演练中，我们将帮助你熟悉如何使用 Visual Studio。 本演练将向你展示入门所需了解的全部基础知识。 每次创建应用时，你都需要遵循以下类似步骤。

下面的视频针对 Xcode 和 Visual Studio 进行了比较。

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Comparing-Xcode-to-Visual-Studio/player]

你还会发现此[生成适用于 Windows 的应用博客文章](https://blogs.windows.com/buildingapps/2016/01/27/visual-studio-walkthrough-for-ios-developers/)非常有用。

创建应用程序 （更准确地讲称为通用 Windows 平台 (UWP) 应用程序） 的 Windows 10 是相当类似于创建 iOS 应用程序使用演示图板。 Windows 10 应用通常构造对页面上，每个页面包含用户界面，例如网站的不同部分。 通常每个页面具有两个关联的源文件：一个源文件存储用户界面（以 [XAML 概述](https://msdn.microsoft.com/library/windows/apps/mt185595)格式存储），另一个源文件包含源代码（通常为 C#）。 当用户与应用交互时，它们会在这些页面之间导航。 在本演练中，你将创建带有两个页面的应用。

**请注意**  Windows 10 应用一项重要功能是相同的源代码和相同的 API 集，是无论平台可用的事实。 正如你所知，在编写适用于 iPhone 和 iPad 的通用 iOS 应用时，可在运行时决定应用运行的平台，并采取相应操作。 类似地，在 Windows 10 应用中可以看到，在运行时运行的设备。 使用 UWP 应用，则无需使用\#ifdef 创建与桌面电话在源代码中的生成。 为方便起见，Windows 10 应用也可以智能地使用其具体取决于设备的用户界面控件： 例如，您的应用程序可以引用日期选取器控件，并且控件将自动查找并运行方式不同，具体取决于是否具有桌面或手机屏幕上运行。 但源代码不会改变。

我们来看，我们可以如何创建 Windows 10 应用。 首先运行 Visual Studio。 首次运行时，Visual Studio 将要求你获取开发人员许可证。 开发人员许可证使你可以先在本地计算机上安装和测试 UWP 应用，然后再将其提交到 Microsoft Store。 要获得许可证，请按照屏幕说明，使用 Microsoft 帐户登录。 如果没有帐户，请单击“开发人员许可证”对话框中的“注册”链接，并按照屏幕上的说明操作。

为了进行比较，在启动 Xcode 时，你首先看到“欢迎使用 Xcode”屏幕，如下图所示。

![Xcode 欢迎屏幕](images/ios-to-uwp/ios-to-uwp-xcode-welcome.png)

Visual Studio 与此非常相似。 你将看到“起始页”，如下图所示。

![Visual Studio 开始屏幕](images/ios-to-uwp/ios-to-uwp-vs-welcome.png)

若要创建新应用，请首先通过执行以下操作之一来创建项目：

-   在“开始”区域中，点击“新建项目”。
-   点击“文件”菜单，然后点击“新建项目”。

为了进行比较，在 Xcode 中创建新项目时，你将看到一个与下图中类似的项目模板列表。

![Xcode“新建项目”对话框](images/ios-to-uwp/ios-to-uwp-xcode-choose-template.png)

在 Visual Studio 中，还可以从若干项目模板中选择，如下图中所示。

![visual studio 新建项目对话框](images/ios-to-uwp/ios-to-uwp-vs-choose-template.png)对于本演练中，点击**Visual C#** ，然后点击**Windows**， **Windows 通用**并**空白应用 （Windows 通用）**。 在“名称”框中，键入“MyApp”，然后点击“确定”。 Visual Studio 将创建并显示你的第一个项目。 现在你可以开始设计你的应用并向其添加代码。

## <a name="next-step"></a>下一步

[入门：选择编程语言](getting-started-choosing-a-programming-language.md)
