---
title: 客户数据库应用程序结构
description: 查看客户数据库教程，该结构和构造它的原因。
keywords: 企业版、 教程、 客户、 数据、 crud
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: b1f8f8c8a2fd1522d8c304a45514d5257543f222
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656612"
---
# <a name="customer-database-app-structure"></a>客户数据库应用程序结构

复杂的业务线应用程序通常具有许多页面和功能和出色将多行代码。 因此，它是非常重要的设计您的应用程序根据可预测的结构问题。 这适用于企业应用的多个应用程序设计模式，但它们所有围绕构建，旨在让大型应用程序更轻松地了解和使用。

虽然[客户数据库教程](customer-database-tutorial.md)提供了一个单页面应用程序为简单起见，它可实现模型-视图-视图模型 (MVVM) 应用程序设计模式来展示在操作中的这些概念。 顾名思义，MVVM 设计模式可将核心应用逻辑分离为三个类别：

* 模型是包含应用程序的数据的类。
* 视图是任何给定页面的 UI。
* Viewmodel 提供应用程序逻辑。 这可以包括处理从视图的用户操作和/或管理与模型交互。

虽然此应用不是 MVVM 的完美和 architypical 示例，它显示在操作中的主要关注点分离原则。 [请查看此处的应用。](https://github.com/Microsoft/windows-tutorials-customer-database)

## <a name="application-structure"></a>应用程序结构

打开应用后，开始通过检查**解决方案资源管理器。** 应该有类似的 UWP 应用之前，使用过，但您还会看到一系列文件夹看到的内容的一些用于保存应用程序的组件部分。

![在解决方案资源管理器中起始点的应用](images/customer-database-tutorial/solution-explorer.png)

### <a name="views"></a>视图

在 Views 文件夹中定义应用的 UI。 由于在本教程中稍后再试是单页面应用，这意味着只有一个视图中的**CustomerListPage**。 它具有 XAML 用户界面标记和 xaml.cs 代码隐藏-这两个文件构成了一个视图。 您稍后将添加到 UI 元素**CustomerListPage.xaml**。

> [!NOTE]
> 您可能注意到此应用没有 MainPage。 这是因为我们在中指定**App.xaml.cs**的应用程序应启动**CustomerListPage**后启动。

### <a name="viewmodels"></a>ViewModels

尽管此应用程序只有一个视图，具有两个 Viewmodel。 这是为什么？

**CustomerListPageViewModel.cs**是 MVVM 模式中的标准 ViewModel。 也是页面的应用的基本逻辑所在和页面您将使用在本教程中大多数的位置。 通过在视图中每个用户所执行的 UI 操作传递到处理此 ViewModel 时。

**CustomerViewModel.cs**，但是，未与任何特定的视图相关联。 相反，它将与单个客户的模型中包含的数据关联 （已编辑的属性） 的编程概念。

### <a name="models"></a>模型

此应用包含三个模型，它存储应用程序的数据并提供了与存储库进行交互的接口。 虽然这些都是应用的关键部分，他们不是你将直接编辑本教程中的内容。

最重要的是**Customer.cs**，它描述了你将在本教程中使用它的客户数据结构。

> [!NOTE]
> 本教程会忽略*电子邮件*并*Phone*客户对象的属性。 如果你想要超越这里介绍的内容，将这两个属性添加到应用的 UI 是很好的第一步。

### <a name="repository"></a>存储库

存储库文件夹包含构造和使用本地 SQLite 数据库进行交互的类。 本教程中，SQLite 数据库显示为的是。 虽然您将在代码中添加**CustomerListPageViewModel.cs**调用通过这些类定义的方法，无需进行任何更改以便设置它们。

有关详细信息中 UWP，SQLite[请参阅此文章](../data-access/sqlite-databases.md)。

如果您尝试在本教程的"继续"部分，这是你创建一个类来连接到远程的其余部分数据库。 它还将实现**ICustomerRepository**接口定义在模型部分中，但它看起来与对应的 SQLite 大不相同。

### <a name="other-elements"></a>其他元素

由于是常用于 UWP 应用，在中定义的应用程序启动行为**App.xaml.cs**类。 下面大部分是代码的任何 UWP 应用的默认代码。 但我们已做出了几个小更改：

* 我们已指定应用程序应显示**CustomerListPage**上启动。
* 我们创建了一个存储库对象，它将保存我们要使用的数据源。
* 我们添加了**SQLiteDatabase**方法，用于初始化本地数据库，并将其设置为指定的存储库。

如果您尝试"继续"部分，将添加一个类似的方法来初始化 REST 存储库对象。 因为我们已分隔我们主要关注，并且 SQLite 和 REST 操作使用同一定义的接口，这将是唯一的现有代码将需要更改应用程序中使用而不是 SQLite 的其余部分。

## <a name="next-steps"></a>后续步骤

如果你已完成本教程中，则您可以签出[完整的示例应用](https://github.com/Microsoft/Windows-appsample-customers-orders-database)若要查看这些功能在规模较大的实现方式。

否则，既然您知道为什么所有内容就在这里，您应该[返回到本教程](customer-database-tutorial.md)以及使用我们刚刚已经提到的结构。