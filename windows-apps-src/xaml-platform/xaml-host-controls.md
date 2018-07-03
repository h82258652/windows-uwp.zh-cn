---
author: normesta
description: 本指南将帮助您直接在 WPF 和 Windows 窗体应用程序中创建基于 Fluent 的 UWP UI
title: 在 WPF 和 Windows 窗体应用程序中托管 UWP 控件
ms.author: normesta
ms.date: 05/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp, windows forms, wpf
keywords: windows 10, uwp, windows 窗体, wpf
ms.localizationpriority: medium
ms.openlocfilehash: 4823654bce3373ace5b04ced8ec14c4b6c1b6f1d
ms.sourcegitcommit: 3500825bc2e5698394a8b1d2efece7f071f296c1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2018
ms.locfileid: "1862066"
---
# <a name="host-uwp-controls-in-wpf-and-windows-forms-applications"></a>在 WPF 和 Windows 窗体应用程序中托管 UWP 控件

> [!NOTE]
> 与在商业发行之前可能会进行实质性修改的预发布产品相关的一些信息。 Microsoft 对于此处提供的信息不作任何明示或默示的担保。

我们正在将 UWP 控件引入桌面，以帮助您通过 Fluent Design 功能增强现有 WPF 或 Windows 应用程序的外观、使用体验和功能。 可通过两种方式实现此功能。

首先，你可以在 WPF 或 Windows 窗体项目的设计图面中直接添加控件，然后在设计器中像使用任何其他控件那样使用它们。  立即试用新的 **WebView** 控件。 此控件使用 Microsoft Edge 呈现引擎，目前仅适用于 UWP 应用程序。 你可以在最新版本的 [Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)中找到 **WebView**。

你很快可以获得更多的 Fluent Design 功能：我们将提供允许您托管各种 UWP 控件的控件。 请在 Windows 社区工具包未来版本中查找此控件及许多其他控件。

下面是这些控件的组织结构速览。 此图表中使用的名称可能发生更改。  

![托管控件体系结构](images/host-controls.png)

此图表底部所示的 API 随 Windows SDK 提供。  将添加到设计器中的控件在 Windows 社区工具包中作为 Nuget 程序包提供。

这些新控件存在一些限制，因此在使用它们之前，请花一点时间了解尚不支持什么功能，以及什么功能仅仅作为临时的变通方法。

### <a name="whats-supported"></a>支持的功能

大多数情况下，除非下表中明确排除，否则支持所有功能。

### <a name="whats-supported-only-with-workarounds"></a>仅作为变通方法支持的功能

:heavy_check_mark：在多个窗口中托管多个收件箱控件。 你必须将每个窗口放置在自己的线程中。

:heavy_check_mark：将 ``x:Bind`` 用于托管控件。 你必须在 .NET 标准库中声明数据模型。

:heavy_check_mark: 基于 C# 的第三方控件。 如果有第三方控件的源代码，可以编译该代码。

### <a name="whats-not-yet-supported"></a>尚不支持的功能

:no_entry_sign：跨应用程序和托管控件无缝工作的辅助功能工具。

:no_entry_sign：添加到应用程序中的控件中不包含 Windows 应用包的本地化内容。

:no_entry_sign：应用程序中以 XAML 作出的不包含 Windows 应用包的资产引用。

:no_entry_sign：正确响应 DPI 和缩放更改的控件。

:no_entry_sign：将 **WebView** 控件添加到自定义用户控件（无论是线程上、线程下还是进程外）。

:no_entry_sign: [突出显示](https://docs.microsoft.com/windows/uwp/design/style/reveal) Fluent 效果。

:no_entry_sign：输入控件的内联墨迹书写、@Places 和 @People。

:no_entry_sign：分配加速键。

:no_entry_sign：基于 C++ 的第三方控件。

:no_entry_sign：托管自定义用户控件。

随着我们继续改进在桌面引入 Fluent 的体验，此列表中的项目可能发生更改。  
