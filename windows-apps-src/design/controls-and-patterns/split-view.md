---
author: QuinnRadich
title: 拆分视图
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: 拆分视图控件具有一个可展开/可折叠的窗格和一个内容区域。
label: Split view
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: tpaine
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: cde4b5d95a0c978faa647fcc108d74874ff52c40
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "4471763"
---
# <a name="split-view-control"></a>拆分视图控件

拆分视图控件具有一个可展开/可折叠的窗格和一个内容区域。

> **重要 API**：[SplitView 类](https://msdn.microsoft.com/library/windows/apps/dn864360)

下面是使用 SplitView 显示其中心的 Microsoft Edge 应用的示例。

![Microsoft Edge 拆分视图示例](images/split_view_Edge.png)


 拆分视图的内容区域始终可见。 窗格可以展开和折叠或停留在打开状态，而且可以从应用窗口的左侧或右侧显示其自身。 窗格中有四种模式：

-   **Overlay**

    在打开之前隐藏窗格。 在打开时，窗格覆盖内容区域。

-   **内联**

    窗格始终可见，并且不会覆盖内容区域。 窗格和内容区域划分可用的屏幕空间。

-   **CompactOverlay**

    在此模式下始终可以看见狭窄的部分窗格，宽度恰好足以显示图标。 默认关闭窗格宽度为 48px，可以使用 `CompactPaneLength` 进行修改。 如果打开窗格，将覆盖内容区域。

-   **CompactInline**

    在此模式下始终可以看见狭窄的部分窗格，宽度恰好足以显示图标。 默认关闭窗格宽度为 48px，可以使用 `CompactPaneLength` 进行修改。 如果打开窗格，将减少用于内容的空间，从而会将内容挤出去。

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

拆分视图控件可用于创建任何“抽屉”体验，其中用户可以打开和关闭补充窗格。 例如，你可以使用 SplitView 构建[大纲/细节](master-details.md)模式。

若要生成带有展开/折叠按钮和导航项列表的导航菜单，可使用 [NavigationView](navigationview.md) 控件。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/SplitView">打开此应用，了解 SplitView 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-split-view"></a>创建拆分视图

下面是 SplitView 控件，有一个打开的窗格内联显示在“内容”旁边。
```xaml
<SplitView IsPaneOpen="True"
           DisplayMode="Inline"
           OpenPaneLength="296">
    <SplitView.Pane>
        <TextBlock Text="Pane"
                   FontSize="24"
                   VerticalAlignment="Center"
                   HorizontalAlignment="Center"/>
    </SplitView.Pane>

    <Grid>
        <TextBlock Text="Content"
                   FontSize="24"
                   VerticalAlignment="Center"
                   HorizontalAlignment="Center"/>
    </Grid>
</SplitView>
```

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-topics"></a>相关主题
- [导航窗格模式](navigationview.md)
- [列表视图](lists.md)
- [大纲/细节](master-details.md)