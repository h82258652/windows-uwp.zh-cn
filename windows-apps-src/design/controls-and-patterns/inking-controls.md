---
Description: Ink tools described
title: 墨迹书写控件
label: Inking Controls
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 97eae5f3-c16b-4aa5-b4a1-dd892cf32ead
ms.localizationpriority: medium
ms.openlocfilehash: a76a03eaff3d831b48e7b86c0b70ff2cbae7360a
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "7845134"
---
# <a name="inking-controls"></a>墨迹书写控件



有两种不同的控件可促进通用 Windows 平台 (UWP) 应用中的墨迹书写：[InkCanvas](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) 和 [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)。

InkCanvas 控件将笔输入呈现为笔划墨迹（使用颜色和粗细的默认设置）或擦除笔划。 此控件是一个透明的覆盖层，该覆盖层不包含任何用于更改默认笔划墨迹属性的内置 UI。

> [!NOTE]
> InkCanvas 可以配置为针对鼠标和触控输入支持类似功能。

由于 InkCanvas 控件不包括对更改默认笔划墨迹设置的支持，因此它可以与 InkToolbar 控件配对。 InkToolbar 包含一组可自定义和可扩展的按钮，用于在关联的 InkCanvas 中激活墨迹相关的功能。

默认情况下，InkToolbar 包括用于绘制、擦除、突出显示和显示标尺的按钮。 根据功能，在浮出控件中提供其他设置和命令，如墨迹颜色、笔划粗细、擦除所有墨迹。

> [!NOTE]
> InkToolbar 支持笔和鼠标输入，并且可配置为识别触控输入。

<img src="images/ink-tools-invoked-toolbar.png" width="300" alt="InkToolbar palette flyout">

> **重要 API**：[InkCanvas 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx)，[InkToolbar 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)，[InkPresenter 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx)，[Windows.UI.Input.Inking](https://msdn.microsoft.com/library/windows/apps/br208524)


## <a name="is-this-the-right-control"></a>这是正确的控件吗？

当你需要在应用中支持基本墨迹书写，而不向用户提供任何墨迹设置时，请使用 InkCanvas。

默认情况下，使用笔尖（粗细为 2 个像素的黑色圆珠笔）时笔划呈现为墨迹，使用橡皮擦尖时笔划呈现为橡皮擦。 如果橡皮擦尖不存在，则 InkCanvas 可配置为将来自笔尖的输入作为擦除笔划处理。

将 InkCanvas 与 InkToolbar 配对以提供用于激活墨迹功能和设置基本墨迹属性（如笔划大小、颜色和笔尖形状）的 UI。

> [!NOTE] 
> 若要在 InkCanvas 上对笔划墨迹呈现进行更广泛的自定义，请使用基础的 [InkPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx) 对象。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/InkCanvas">打开此应用，了解 InkCanvas 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

**Microsoft Edge**

Microsoft Edge 针对 **Web 笔记**使用 InkCanvas 和 InkToolbar。  
![InkCanvas 用于在 Microsoft Edge 中进行墨迹书写](images/ink-tools-edge.png)

**Windows Ink 工作区**

InkCanvas 和 InkToolbar 还用于 **Windows Ink 工作区**中的**草图板**和**屏幕草图**。  
![Windows Ink 工作区中的 InkToolbar](images/ink-tools-ink-workspace.png)

## <a name="create-an-inkcanvas-and-inktoolbar"></a>创建 InkCanvas 和 InkToolbar

将 InkCanvas 添加到应用只需一行标记：

```xaml
<InkCanvas x:Name=“myInkCanvas”/>
```

> [!NOTE]
> 有关使用 InkPresenter 进行的详细的 InkCanvas 自定义，请参阅[“UWP 应用中的笔和触笔交互”](http://windowsstyleguide/input/pen-and-stylus-interactions/)。

InkToolbar 控件必须与 InkCanvas 结合使用。 将 InkToolbar（以及所有内置工具）合并到应用需要一行额外的标记：

 ```xaml
<InkToolbar TargetInkCanvas=“{x:Bind myInkCanvas}”/>
 ```

这显示以下 InkToolbar：
<img src="images/ink-tools-uninvoked-toolbar.png" width="250" alt="Basic InkToolbar">

### <a name="built-in-buttons"></a>内置的按钮

InkToolbar 包含以下内置按钮：

**笔**

- 圆珠笔：使用圆形笔尖绘制实心、不透明的笔划。 笔划大小取决于检测到的笔压力。
- 铅笔 - 使用圆形笔尖绘制边缘柔化、带纹理且半透明的笔划（适用于分层的着色效果）。 笔划颜色（暗度）取决于检测到的笔压力。
- 荧光笔：使用矩形笔尖绘制半透明笔划。

你可以为每支笔在浮出控件中自定义调色板和大小属性（最小值、最大值、默认值）。

**工具**

- 橡皮擦：删除接触到的任何墨迹笔划。 请注意，将检测到整个笔划墨迹，而不仅仅是擦除笔划下的部分。

**切换**

- 标尺：显示或隐藏标尺。 在标尺边缘附近绘制会导致墨迹笔划贴靠到标尺上。  
 ![与 InkToolbar 关联的标尺视觉对象](images/inking-tools-ruler.png)

尽管这是默认配置，但对于为你的应用将哪些内置按钮包含在 InkToolbar 中，你具有完全的控制权。

### <a name="custom-buttons"></a>自定义按钮

InkToolbar 由两组不同的按钮类型组成：

1. 一组“工具”按钮，包含内置绘制、擦除和突出显示按钮。 在此处添加自定义的笔和工具。
> [!NOTE]
> 功能选择相互排斥。

2. 一组“切换”按钮，包含内置标尺按钮。 在此处添加自定义切换。
> [!NOTE]
> 功能相互不排斥，并且可以与其他活动工具同时使用。

根据你的应用程序和所需的墨迹书写功能，你可以将以下任意按钮（绑定到你的自定义墨迹功能）添加到 InkToolbar：

- 自定义笔：由主机应用为其定义墨迹调色板和笔尖属性（如形状、旋转和大小）的笔。
- 自定义工具：非笔工具，由主机应用定义。
- 自定义切换：将应用定义的功能状态设置为开或关。 当打开时，功能将与活动工具结合使用。

> [!NOTE]
> 你无法更改内置按钮的显示顺序。 默认的显示顺序为：圆珠笔、铅笔、荧光笔、橡皮擦和标尺。 自定义笔附加到最后一个默认笔，自定义工具按钮添加到最后一个笔按钮和橡皮擦按钮之间，而自定义切换按钮添加到标尺按钮之后。 （自定义按钮按照指定它们的顺序添加。）

尽管 InkToolbar 可以是顶级项目，但它通常通过“墨迹书写”按钮或命令公开。 我们建议使用 Segoe MLD2 Assets 字体中的 EE56 字形作为顶级图标。

## <a name="inktoolbar-interaction"></a>InkToolbar 交互

所有内置的笔和工具按钮都包含一个浮出控件菜单，可以在该菜单中设置墨迹属性和笔尖形状与大小。 一种“扩展字形” ![InkToolbar 字形](images/ink-tools-glyph.png) 显示在按钮上，以指示存在浮出控件。

当再次选择活动工具的按钮时，会显示浮出控件。 当颜色或大小更改时，将自动消除浮出控件，并且可以恢复墨迹书写。 自定义笔和工具可以使用默认的浮出控件或指定自定义布局。

橡皮擦也有提供**擦除所有墨迹**命令的浮出控件。  
![调用了橡皮擦浮出控件的 InkToolbar](images/ink-tools-erase-all-ink.png)

 有关自定义和可扩展性的信息，请查看 [SimpleInk 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)。

## <a name="dos-and-donts"></a>应做事项和禁止事项

- InkCanvas 和通常的墨迹书写可通过主动笔获得最佳体验。 但是，如果应用需要，我们建议支持使用鼠标和触控（包括被动笔）输入的墨迹书写。
- 将 InkToolbar 控件与 InkCanva 结合使用来提供基本墨迹书写功能和设置。 InkCanvas 和 InkToolbar 均可以采用编程方式自定义。
- InkToolbar 和通常的墨迹书写可通过主动笔获得最佳体验。 但是，如果应用需要，可以支持使用鼠标和触控的墨迹书写。
- 如果支持使用触控输入的墨迹书写，我们建议为切换按钮使用 Segoe MLD2 Assets 字体中的 ED5F 图标，并附带“触控书写”工具提示。
- 如果提供笔划选择，我们建议为工具按钮使用 Segoe MLD2 Assets 字体中的 EF20 图标，并附带“选择工具”工具提示。
- 如果使用多个 InkCanvas，我们建议使用单个 InkToolbar 控制跨画布的墨迹书写。
- 为了实现最佳性能，我们建议更改默认的浮出控件，而不是为默认和自定义工具都创建一个自定义浮出控件。

## <a name="get-the-sample-code"></a>获取示例代码

- [SimpleInk 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)演示关于 InkCanvas 和 InkToolbar 控件的自定义和扩展性功能的 8 个方案。 每个方案都提供了有关常见墨迹书写和控件实现的基本指南。
- [ComplexInk 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk) - 演示更高级的墨迹书写方案。
- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

- [UWP 应用中的笔和触笔交互](http://windowsstyleguide/input/pen-and-stylus-interactions/)
- [识别笔划墨迹](http://windowsstyleguide/input/convert-ink-to-text/)
- [存储和检索笔划墨迹](http://windowsstyleguide/input/save-and-load-ink/)
