---
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: 数据绑定和 MVVM
description: 数据绑定是模型-视图-视图模型 (MVVM) UI 体系结构设计模式的核心，可在 UI 代码与非 UI 代码之间实现松散耦合。
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 931f2fcbcdbf58b9dc2ca40403d7466b620a8991
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63798103"
---
# <a name="data-binding-and-mvvm"></a>数据绑定和 MVVM

模型-视图-视图模型 (MVVM) 是用于解耦 UI 代码和非 UI 代码的 UI 体系结构设计模式。 借助 MVVM，可以在 XAML 中以声明方式定义 UI，并使用数据绑定标记将 UI 链接到包含数据和命令的其他层。 数据绑定基础结构提供松散耦合，使 UI 和链接的数据保持同步，并将用户输入路由到相应的命令。 

由于数据绑定提供松散耦合，因此使用它可以减少不同类型的代码之间的硬性依赖关系。 这样就可以更轻松地更改各个代码单元（方法、类、控件等），且不会导致其他单元中出现意外的负面影响。 这种解耦是“关注点分离”（许多设计模式中的一个重要概念）的一个例子。  

## <a name="benefits-of-mvvm"></a>MVVM 的优势

解耦代码能够带来许多优势，包括：

* 启用迭代的探索性代码样式。 在隔离位置做出的更改风险更低，且更易于试验。
* 简化单元测试。 相互隔离的代码单元可以单独进行测试，也可以在生产环境外部进行测试。
* 支持团队协作。 与妥善设计的接口相符的解耦代码可由不同的个人或团队开发，以后还可以集成。
* 改善可维护性。 修复解耦代码中的 bug 不太可能会导致其他代码中出现回归。

与 MVVM 相比，采用更传统“代码隐藏”结构的应用通常对仅供显示的数据使用数据绑定，并通过直接处理控件公开的事件来响应用户输入。 事件处理程序在代码隐藏文件（例如 MainPage.xaml.cs）中实现，并且往往与控件（通常包含用于直接操作 UI 的代码）紧密耦合。 这样，在不更新事件处理代码的情况下，很难甚至根本无法替换控件。 采用此体系结构时，代码隐藏文件通常会累积与 UI 不直接相关的代码（例如数据库访问代码），最终在与其他页面配合使用时会出现重复或者被修改。

## <a name="app-layers"></a>应用层

使用 MVVM 模式时，应用划分为以下层：

* 模型层定义了代表业务数据的类型。  这包括对核心应用域建模所需的全部内容，通常包括核心应用逻辑。 此层完全独立于视图层和视图模型层，并且往往是部分性地驻留在云中。 获得完全实现的模型层后，如果需要，可以创建多个不同的客户端应用（例如 UWP 和 Web 应用）来处理相同的底层数据。
* 视图层使用 XAML 标记来定义 UI。  标记包含用于定义特定 UI 组件与各个视图模型和模型成员之间的连接的数据绑定表达式（例如 [x:Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)）。 代码隐藏文件有时用作视图层的一部分，以包含自定义或操作 UI，或者在调用执行工作的视图模型方法之前从事件处理程序参数中提取数据所需的附加代码。 
* 视图模型层提供视图的数据绑定目标。  在许多情况下，视图模型直接公开模型，或者提供用于包装特定模型成员的成员。 视图模型还可以定义成员用于跟踪与 UI 相关但与模型不相关的数据，例如项列表的显示顺序。 视图模型还充当与其他服务（例如数据库访问代码）之间的集成点。 对于简单的项目，可能不需要单独的模型层，只需通过一个视图模型来封装所需的全部数据即可。 

## <a name="basic-and-advanced-mvvm"></a>基本和高级 MVVM

与任何设计模式一样，可通过多种方式实现 MVVM，许多不同的技术都被视为 MVVM 的一部分。 出于此原因，有多个不同的第三方 MVVM 框架支持各种基于 XAML 的平台，其中包括 UWP。 不过，这些框架通常包含多个用于实现解耦体系结构的服务，在一定程度上使 MVVM 的确切定义变得模糊。 

尽管复杂的 MVVM 框架可能非常有用，尤其是对于企业规模的项目，但采用任何特定模式或技术通常会产生相关的成本，并且根据项目的规模和大小，其带来的优势不总是明朗的。 幸运的是，我们可以仅采用那些能够提供明确且实在优势的技术，对于其他技术，可以先置之不理，有需要时再使用它们。 

具体而言，只需了解并运用数据绑定的完整强大威力，并将应用逻辑划分为前面所述的层，即可获得大量的优势。 仅使用 Windows SDK 提供的功能，而无需使用任何外部框架，就能实现此目的。 具体而言，与在以前的 XAML 平台中相比，[{x:Bind} 标记扩展](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)能使数据绑定变得更简单且更高效，消除了以前所需的大量样板代码。

有关使用现成的基本 MVVM 的更多指导，请查看 GitHub 上的[客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)。 许多其他 [UWP 应用示例](https://github.com/Microsoft?q=windows-appsample
)也使用基本的 MVVM 体系结构；[交通应用示例](https://github.com/Microsoft/Windows-appsample-trafficapp)包含代码隐藏和 MVVM 版本，其注释中描述了 [MVVM 转换](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md)。 

## <a name="see-also"></a>另请参阅

### <a name="topics"></a>主题

[深入了解数据绑定](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[{x:Bind} 标记扩展](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>示例

[客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[VanArsdel 库存示例](https://github.com/Microsoft/InventorySample)  
[交通应用示例](https://github.com/Microsoft/Windows-appsample-trafficapp)  
