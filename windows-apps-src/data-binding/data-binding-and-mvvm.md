---
author: KarlErickson
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: 数据绑定和 MVVM
description: 数据绑定的模型-视图-模型 (MVVM) UI 体系结构设计模式的核心是，使 UI 和非 UI 代码之间的松散耦合。
ms.author: karler
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8a70603c26c7123af50fc920d327ccef332b7ed6
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "7441115"
---
# <a name="data-binding-and-mvvm"></a>数据绑定和 MVVM

模型-视图-模型 (MVVM) 是适用于分离 UI 和非 UI 代码的 UI 体系结构设计模式。 借助 MVVM，你在 XAML 中以声明方式定义 UI，并使用数据绑定标记将其链接到其他包含数据和命令的图层。 数据绑定基础结构提供松散耦合的会使 UI 和链接的数据同步并将路由到相应的命令的用户输入。 

因为它提供松散耦合，使用数据绑定减少了不同类型的代码之间的硬依赖项。 这使得更轻松地更改而不会导致意外的负面影响其他单元中的单个代码单位 （方法、 类、 控件等）。 此分离是一种*分离出关注内容*，这是许多设计模式中的一个重要概念。 

## <a name="benefits-of-mvvm"></a>MVVM 的优势

将你的代码分离方面有许多优势，包括：

* 启用迭代、 探索的编码风格。 在隔离的更改是不太危险且更易于试验。
* 简化单元测试。 可测试代码单元均彼此相互隔离，个别应用和生产环境之外。
* 支持团队协作。 可以由单独的个人或小组，开发和集成更高版本分离的代码，并依附于设计良好的接口。
* 改进可维护性。 修复 bug 分离的代码中不太可能导致其他代码回归。

MVVM，与更传统的"代码隐藏"结构的应用通常使用仅显示数据的数据绑定，通过直接处理公开控件的事件来响应用户输入。 事件处理程序 （例如 MainPage.xaml.cs) 中，代码隐藏文件中实现，并且通常紧密耦合到控件，通常包含直接操纵 UI 的代码。 这使得难或无法替换控件，而无需更新的事件处理代码。 使用此体系结构，代码隐藏累积文件，通常并不直接相关的 UI，如数据库访问代码，结束被复制和修改用于其他页面的代码。

## <a name="app-layers"></a>应用层

在使用 MVVM 模式时，将应用分为以下层：

* **模型**层定义表示你的业务数据的类型。 这包括模型的核心应用域，所需的所有内容，并且通常包括核心应用逻辑。 这一层完全独立于视图和视图模型的图层，并经常在云中驻留一部分。 鉴于完全实现的模型层，你可以创建多个不同的客户端应用如果您这样选择，例如使用相同的基础数据的 UWP 和 web 应用。
* **视图**层定义 UI 使用 XAML 标记。 在标记包含定义特定 UI 组件和不同的视图模型和模型成员之间的连接的数据绑定表达式 （如[x: Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)）。 代码隐藏文件是有时用于视图层的一部分包含自定义或操作 UI，或者在调用执行的工作的视图模型方法之前，从事件处理程序参数中提取数据所需的其他代码。 
* 在**视图模型**层提供数据绑定目标视图。 在许多情况下，视图模型直接公开的模型，或提供换行特定模型成员的成员。 视图模型还可以用于跟踪相关的数据定义成员到 UI，但不是属于模型，如的项目列表的显示顺序。 视图模型还可用作与其他服务例如数据库访问代码的集成点。 对于简单的项目，你可能不需要单独的模型图层，但仅对视图模型的封装所需的所有数据。 

## <a name="basic-and-advanced-mvvm"></a>基本和高级 MVVM

与任何设计模式中，没有超过一种方法来实现 MVVM，并且许多不同的技术被视为 MVVM 的一部分。 出于此原因，有不同的多个第三方 MVVM 框架支持各种基于 XAML 的平台，包括 UWP。 但是，这些框架通常包括实现分离的体系结构，从而 MVVM 的确切定义某种程度上不明确的多个服务。 

尽管复杂的 MVVM 框架可以是非常有用，特别是对于企业级项目，通常是采用任何特定模式或技术，与相关联的成本和权益并不总是清除，具体取决于的比例和大小你的项目。 幸运的是，你可以采用仅提供清晰、 切实好处，这些方法并忽略其他人，直到需要它们。 

特别是，你可以获取大量只需通过了解并应用数据绑定和将你的应用逻辑分离到之前所述的图层的强大的优势。 这可以实现使用仅 Windows SDK 和无需使用任何外部框架提供的功能。 特别是， [{x: Bind} 标记扩展](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)简化数据绑定和更高版本比在之前的 XAML 平台，不需要进行大量样板代码中执行所需更早版本。

有关使用基本的现成的 MVVM 的其他指南，请查看 GitHub 上的[客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)。 很多其他[UWP 应用示例](https://github.com/Microsoft?q=windows-appsample
)还使用了一个基本 MVVM 体系结构和[通信应用示例](https://github.com/Microsoft/Windows-appsample-trafficapp)包括代码隐藏和 MVVM 版本中，描述[MVVM 转换](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md)的说明。 

## <a name="see-also"></a>另请参阅

### <a name="topics"></a>主题

[深入了解数据绑定](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[{x:Bind} 标记扩展](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>示例

[客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[VanArsdel 清单示例](https://github.com/Microsoft/InventorySample)  
[路况应用示例](https://github.com/Microsoft/Windows-appsample-trafficapp)  
