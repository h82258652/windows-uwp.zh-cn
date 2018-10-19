---
author: QuinnRadich
title: 2017 年 9 月 Windows 文档中的新增功能 - 开发 UWP 应用
description: 新的功能、视频和开发人员指南已添加到 2017 年 9 月 Windows 10 开发人员文档
keywords: 新增功能, 更新, 功能, 开发人员指南, Windows 10, 1709
ms.author: quradic
ms.date: 09/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 33091903ebf1a7ff1150dcaa9bd83a3e76417926
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "5156989"
---
# <a name="whats-new-in-the-windows-developer-docs-in-september-2017"></a>2017 年 9 月 Windows 开发人员文档中的新增功能

Windows 开发人员文档持续更新对整个 Windows 平台的开发人员提供的新功能的信息。 最近提供了以下功能概述、开发人员指南和示例，包含面向 Windows 开发人员的新的和更新的信息。

当然，Fall Creators Update 即将推出，敬请继续关注即将在下个月发布的大量文档。

只需在 Windows10 上[安装工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/your-first-app.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="features"></a>功能

### <a name="xbox-live-creators-program"></a>Xbox Live 创意者计划

Xbox Live 创意者计划现已推出，你可以通过该计划轻松构建和发布可以在 Windows 10 电脑和 Xbox One 主机上运行的 UWP 游戏。 有关更多信息，请参阅 [Xbox Live 创意者计划入门](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)。

## <a name="developer-guidance"></a>开发人员指南

### <a name="xaml-basics-tutorials"></a>XAML 基础知识教程

我们已编写了四个 [XAML 基础知识教程](https://docs.microsoft.com/en-us/windows/uwp/get-started/xaml-basics-intro) 以支持新的 [PhotoLab 示例](https://github.com/Microsoft/Windows-appsample-photo-lab)，包括 XAML 编程的四个核心方面：用户界面、数据绑定、自定义样式和自适应布局。 每个教程均从部分完整版的 PhotoLab 示例开始，并且分步构建最终应用的一个缺少组件。 

![显示照片库页面的 PhotoLab 示例的屏幕截图](images/PhotoLab-gallery-page.png)  

以下是新文章概览：

+ [**创建用户界面**](https://docs.microsoft.com/en-us/windows/uwp/get-started/xaml-basics-ui)介绍了如何创建基本的照片库界面。
+ [**创建数据绑定**](https://docs.microsoft.com/en-us/windows/uwp/get-started/xaml-basics-data-binding)介绍了如何为照片库添加数据绑定，以使用实时图像数据对其进行填充。
+ [**创建自定义样式**](https://docs.microsoft.com/en-us/windows/uwp/get-started/xaml-basics-style)介绍了如何为照片编辑菜单添加奇特的自定义样式。
+ [**创建自适应布局**](https://docs.microsoft.com/en-us/windows/uwp/get-started/xaml-basics-adaptive-layout)介绍了如何让照片库不会实现自适应，从而使其在每个设备和屏幕大小上均适用。

### <a name="get-started-tutorials"></a>入门教程

UWP 文档的“入门”部分已更新为[新的教程登录页面部分](https://docs.microsoft.com/windows/uwp/get-started/create-uwp-apps)。 本部分提供了经过改进的全新“入门”结构体验，可以帮助用户轻松查找和使用最适合于他们的教程，包括上面提到的 XAML 基础知识教程。

### <a name="voice-and-tone"></a>语音和声调

我们添加了新的 [UWP 应用中的语音和声调指南](https://docs.microsoft.com/windows/uwp/in-app-help/voice-and-tone)，为你提供了有关在应用中编写文本的建议。 无论你创建什么内容，所使用语言的可行性、友好性和信息性都非常重要。

## <a name="samples"></a>示例

### <a name="photolab-sample"></a>PhotoLab 示例

[PhotoLab 示例](https://github.com/Microsoft/windows-appsample-photo-lab)提供基本的照片库和照片编辑体验。

![显示照片编辑页面的 PhotoLab 示例的屏幕截图](images/PhotoLab-editing-page.png)  

### <a name="customer-orders"></a>客户订单

[客户订单数据库](https://github.com/Microsoft/Windows-appsample-customers-orders-database)示例已更新为使用新的 .NET Core 2.0 和实体框架。