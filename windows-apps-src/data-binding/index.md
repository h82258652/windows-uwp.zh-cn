---
ms.assetid: 83b4be37-6613-4d00-a48a-0451a24a30fb
title: 数据绑定
description: 数据绑定是你的应用 UI 用来显示数据的一种方法，可以选择与该数据保持同步。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 96bb0ca90ee3a55f43837fa92bad750b95b248eb
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362440"
---
# <a name="data-binding"></a>数据绑定

数据绑定是你的应用 UI 用来显示数据的一种方法，可以选择与该数据保持同步。 借助数据绑定，你可以将关注的数据从关注的 UI 中分离开来，从而可形成一个更简易的概念模型，并且使你的应用拥有更好的可读性、可测试性和可维护性。 在标记中，你既可以选用 [{x:Bind} 标记扩展](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)，也可以选用 [{Binding} 标记扩展](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)。 还可以在同一应用中（甚至是同一 UI 元素上）混合使用这两个标记扩展。 {x:Bind} 是 Windows 10 的新增内容，并且具备更好的性能。

| 主题 | 描述 |
|-------|-------------|
| [数据绑定概述](data-binding-quickstart.md) | 本主题介绍了如何在通用 Windows 平台 (UWP) 应用中将控件（或其他 UI 元素）绑定到单个项目，或者将项目控件绑定到项目集合。 此外，我们还介绍了如何控制项的呈现、基于所选内容实现详细信息视图，以及转换数据以供显示。 有关更多详细信息，请参阅[深入了解数据绑定](data-binding-in-depth.md)。 | 
| [深入了解数据绑定](data-binding-in-depth.md) | 本主题详细介绍数据绑定功能。 |
| [设计面图上以及用于原型制作的示例数据](displaying-data-in-the-designer.md) | 为了使你的控件填充 Visual Studio 设计器中的数据（以便你可以处理应用的布局、模板和其他视觉属性），你可以通过各种方式使用设计时示例数据。 如果你正要生成一个草图（或原型）应用，则示例数据可能真的非常有用而且节省时间。 你可以在运行时在草图或原型中使用示例数据来阐明你的想法，而无需连接到真实且实时的数据。 |
| [绑定分层数据和创建大纲/细节视图](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md) | 你可以通过将项目控件绑定到 [<strong>CollectionViewSource</strong>](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) 实例（它们绑定在同一个链中），从而生成分层数据的多级主视图/详细信息视图（也称为列表详细信息视图）。 |
| [数据绑定和 MVVM](data-binding-and-mvvm.md) | 本主题介绍了模型-视图-视图模型 (MVVM) UI 体系结构设计模式。 数据绑定是 MVVM 的核心，并且在 UI 与非 UI 代码之间实现了松散耦合。 |
