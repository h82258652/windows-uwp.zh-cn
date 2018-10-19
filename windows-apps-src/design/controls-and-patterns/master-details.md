---
author: Jwmsft
Description: The master/detail pattern displays a master list and the details for the currently selected item. This pattern is frequently used for email and contact lists/address books.
title: 大纲/细节
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: Master/details
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f1236c2ade0423f6e092024e786741f3f3bf6d11
ms.sourcegitcommit: 310a4555fedd4246188a98b31f6c094abb33ec60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "5131295"
---
# <a name="masterdetails-pattern"></a>大纲/细节模式

 

大纲/细节模式具有一个大纲窗格（通常带有[列表视图](lists.md)）和一个用于内容的细节窗格。 当选择大纲列表中的项时，将更新细节窗格。 此模式通常用于电子邮件和通讯簿。

> **重要 API**：[ListView 类](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ListView)，[SplitView 类](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.splitview)

![大纲细节模式的示例](images/HIGSecOne_MasterDetail.png)

## <a name="is-this-the-right-pattern"></a>这是正确的模式吗？

如果你希望进行以下操作，则适用大纲/细节模式：

-   构建一个电子邮件应用、通讯簿或任何基于列表细节布局的应用。
-   定位并设置大型内容集合的优先级。
-   允许从列表中快速添加和删除项，同时在上下文之间来回工作。

## <a name="choose-the-right-style"></a>选择正确的样式

在实现大纲/细节模式时，我们建议你根据可用屏幕空间量使用堆叠样式或并排样式。

| 可用窗口宽度 | 建议样式 |
|------------------------|-------------------|
| 320 epx-640 epx        | 堆叠           |
| 641 epx 或更宽       | 并排对齐      |

 
## <a name="stacked-style"></a>堆叠样式

在堆叠样式中，一次只有一个窗格可见：大纲窗格或细节窗格。

![堆叠模式下的大纲细节](images/patterns-md-stacked.png)

用户从大纲窗格开始，然后通过在大纲列表中选择一个项来“深入”到细节窗格。 对用户来说，它表现为大纲和细节视图存在于两个单独的页面上。

### <a name="create-a-stacked-masterdetails-pattern"></a>创建堆叠大纲/细节模式

创建堆叠大纲/细节模式的一种方法是为大纲窗格和细节窗格使用单独的页面。 将大纲视图放在一个页面上，细节窗格放在一个单独的页面上。

![堆叠样式大纲细节的各个部分](images/patterns-md-stacked-parts.png)

对于大纲视图页面，[列表视图](lists.md)控件适用于显示可包含图像和文本的列表。 

对于细节视图页面，使用[内容元素](../layout/layout-panels.md)最为合理。 如果你有大量的单独字段，请考虑使用**网格**布局将元素排列为一个表单。

有关页面间的导航，请参阅 [UWP 应用的导航历史记录和向后导航](../basics/navigation-history-and-backwards-navigation.md)。

## <a name="side-by-side-style"></a>并排样式

在并排样式中，大纲窗格和细节窗格同时可见。

![大纲/细节模式](images/patterns-masterdetail-400x227.png)

大纲模式中的列表具有一个选择可视，用于指示当前选定的项。 在大纲列表中选择一个新项将更新细节窗格。

### <a name="create-a-side-by-side-masterdetails-pattern"></a>创建并排大纲/细节模式

创建并排大纲/细节模式的一种方法是使用[拆分视图](split-view.md)控件。 将大纲视图放在拆分视图窗格中，细节视图放在拆分视图内容中。

![大纲细节拆分视图的各个部分](images/patterns_md_splitview_parts.png)

对于大纲窗格，[列表视图](lists.md)控件适用于显示可包含图像和文本的列表。

对于细节内容，使用[内容元素](../layout/layout-panels.md)最为合理。 如果你有大量的单独字段，请考虑使用**网格**布局将元素排列为一个表单。

## <a name="adaptive-layout"></a>自适应布局

要实现适用于任何屏幕大小的大纲/细节模式，请使用[自适应布局](../layout/layouts-with-xaml.md)创建响应式 UI。

![自适应大纲细节布局](images/patterns_masterdetail.png)

### <a name="create-an-adaptive-masterdetails-pattern"></a>创建自适应大纲/细节模式
要创建自适应布局，请为 UI 定义不同的 [**VisualStates**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.visualstate)，然后使用 [**AdaptiveTriggers**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) 为不同的状态声明断点。

## <a name="get-the-sample-code"></a>获取示例代码

以下示例使用自适应布局实现大纲/细节模式并演示到静态数据、数据库和在线资源的数据绑定。 
- [大纲/细节示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlMasterDetail) 
- [大纲/细节与选择示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Windows Template Studio 大纲/细节示例](https://github.com/Microsoft/WindowsTemplateStudio/tree/master/templates/Uwp/Pages/MasterDetail)
- [客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
- [RSS 阅读器示例](https://github.com/Microsoft/Windows-appsample-rssreader)

## <a name="related-articles"></a>相关文章

- [列表](lists.md)
- [搜索](search.md)
- [应用和命令栏](app-bars.md)
- [ListView 类](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [SplitView 类](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.splitview)
