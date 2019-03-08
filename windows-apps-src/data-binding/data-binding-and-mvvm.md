---
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: 数据绑定和 MVVM
description: 数据绑定是模型-视图-视图模型 (MVVM) UI 体系结构设计模式，核心，并且 UI 和非 UI 代码之间实现松散耦合。
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 931f2fcbcdbf58b9dc2ca40403d7466b620a8991
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616732"
---
# <a name="data-binding-and-mvvm"></a>数据绑定和 MVVM

模型-视图-视图模型 (MVVM) 是分离 UI 和非 UI 代码的 UI 体系结构设计模式。 使用 MVVM，你可以在 XAML 中以声明方式定义您的 UI 和数据绑定标记用于将其链接到其他层包含数据和命令。 数据绑定基础结构提供了使 UI 松散耦合和链接的数据同步，并将路由到适当的命令的用户输入。 

因为它提供了松散耦合，使用数据绑定可减少硬依赖项之间不同类型的代码。 这使得更轻松地更改单个代码单元 （方法、 类、 控件等），而其他单元中导致意外的副作用。 这种分离是一种*关注点分离*，这是许多设计模式中的一个重要概念。 

## <a name="benefits-of-mvvm"></a>MVVM 的优势

分离你的代码具有许多好处，包括：

* 启用迭代性的探索性的编码样式。 是独立的更改是较低风险且更轻松地试验。
* 简化单元测试。 单独和非生产环境，可以测试是从另一个隔离的代码单元。
* 支持团队协作。 可以由单独的个人或团队，开发和更高版本集成符合设计良好的接口的分离式的代码。
* 改进可维护性。 分离式代码中修复的 bug 是不太可能导致其他代码中的回归。

与 MVVM，相比具有更常规的"代码隐藏"结构的应用通常使用数据绑定，仅显示数据，并通过直接处理由控件公开的事件来响应用户输入。 事件处理程序实现代码隐藏文件 （例如 MainPage.xaml.cs)，并通常紧密耦合到控件，通常包含直接操作用户界面的代码。 这使得难以或无法替换控件，而无需更新的事件处理代码。 这种体系结构，代码隐藏文件通常会累积并不直接相关的 ui，如数据库访问代码，最终会被复制并修改以用于其他页的代码。

## <a name="app-layers"></a>应用层

使用 MVVM 模式时，将应用程序分为以下层：

* **模型**层定义表示您的业务数据的类型。 这包括核心应用程序域，建模所需的所有内容和通常包括核心应用程序逻辑。 此层是完全独立的视图和视图模型层，并通常驻留在云中部分。 给定完全实现的模型层，您可以创建多个不同的客户端应用程序如果因此选择，例如使用相同的基础数据的 UWP 和 web 应用。
* **视图**层定义 UI 使用 XAML 标记。 标记包含数据绑定表达式 (如[X:bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) 用于定义特定 UI 组件与不同的视图模型和模型成员之间的连接。 代码隐藏文件有时视图层的一部分用于包含自定义或操作 UI，或者调用执行的工作的视图模型方法之前从事件处理程序参数中提取数据所需的其他代码。 
* **视图模型**层的视图提供数据绑定目标。 在许多情况下，视图模型直接公开的模型，或提供包装特定模型成员的成员。 视图模型还可以用于跟踪相关的数据定义成员的 ui，但不是到模型中，如项列表的显示顺序。 视图模型还充当一个集成点，与其他服务，如数据库访问代码。 对于简单的项目，可能不需要单独的模型层，但仅对视图模型，用于封装所有所需的数据。 

## <a name="basic-and-advanced-mvvm"></a>基本和高级 MVVM

与任何设计模式一样，没有多个方法来实现 MVVM，和多种不同的技术被视为 MVVM 的一部分。 出于此原因，有不同的多个第三方 MVVM 框架支持各种基于 XAML 的平台，包括 UWP。 但是，这些框架通常包含多个服务实现分离的体系结构，使 MVVM 的精确定义某种程度上不明确。 

尽管复杂的 MVVM 框架可能非常有用，尤其是对于企业级项目中，没有通常与采用任何特定模式或技术，相关的成本和好处并不总是很清晰，具体取决于的规模和大小你的项目。 幸运的是，你可以使用仅提供明确和有形好处是，这些技术并忽略其他信息，直到需要它们。 

具体而言，就可以只需通过了解和应用数据绑定和将应用程序逻辑分离到前面所述的各层的完整功能的好处很多。 这可以实现使用仅由 Windows SDK，无需使用任何外部框架所提供的功能。 具体而言， [{x： 绑定} 标记扩展](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)使数据绑定，更容易和更高版本比在先前的 XAML 平台，从而无需大量之前所需的样板代码中执行。

有关使用基本的开箱 MVVM 的详细说明，请参阅[客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)GitHub 上。 其他许多[UWP 应用示例](https://github.com/Microsoft?q=windows-appsample
)还使用一个基本的 MVVM 体系结构，并[流量应用示例](https://github.com/Microsoft/Windows-appsample-trafficapp)说明中包含代码隐藏和 MVVM 的版本中， [MVVM 转换](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md). 

## <a name="see-also"></a>另请参阅

### <a name="topics"></a>主题

[深度中的数据绑定](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[{x： 绑定} 标记扩展](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>示例

[客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[VanArsdel 清单示例](https://github.com/Microsoft/InventorySample)  
[流量应用示例](https://github.com/Microsoft/Windows-appsample-trafficapp)  
