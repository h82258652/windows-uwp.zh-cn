---
author: mijacobs
Description: Use the ParallaxView control to add depth and movement to your app.
title: 如何使用 ParallaxView 控件向你的应用添加深度和移动效果。
ms.assetid: ''
label: Parallax View
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: conrwi
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: b144c7e790d0462688795d9e1a6c4f076b569eb3
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2018
ms.locfileid: "4309185"
---
# <a name="parallax"></a>视差

视差是一种视觉效果，即靠近观察者的物体比背景中的物体移动得更快。 视差可产生一种深度、透视和移动感。 在 UWP 应用中，你可以使用 ParallaxView 控件来创建视差效果。  

> **重要的 API**：[ParallaxView 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview)、[VerticalShift 属性](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift)、[HorizontalShift 属性](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift)

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果你已安装了 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，单击此处<a href="xamlcontrolsgallery:/item/ParallaxView">打开此应用，了解 ParallaxView 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="parallax-and-the-fluent-design-system"></a>视差和 Fluent 设计系统

 Fluent 设计系统可帮助你创建包含光线、深度、动画、材料和比例的现代、粗体 UI。 视差是 Fluent 设计系统的一个组成部分，它将动画、深度和比例添加到你的应用。 要了解详细信息，请参阅 [UWP 的 Fluent 设计概述](../fluent-design-system/index.md)。

## <a name="how-it-works-in-a-user-interface"></a>它在用户界面中的工作原理

在 UI 中，你可以在 UI 滚动或平移时，通过以不同的速率移动不同的对象来创建视差效果。 <!-- Parallax is an important tool in adding depth to applications along with other techniques like transition animations, perspective tilt, and layering. --> 要进行演示，请让我们查看两层内容，分别是列表和背景图像。  该列表位于该背景图像的顶部，这已经给人以列表可能靠观察者更近的错觉。  现在，为了实现视差效果，我们希望靠我们最近的对象比远处的对象移动得“更快”。  当用户滚动界面时，列表比背景图像移动得更快，从而产生深度错觉。

 ![附带列表和背景图像的一个视差示例](images/_Parallax_v2.gif)

 
## <a name="using-the-parallaxview-control-to-create-a-parallax-effect"></a>使用 ParallaxView 控件创建视差效果

要创建视差效果，你可以使用 [ParallaxView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) 控件。 该控件将前景元素（如列表）的滚动位置与背景元素（如图像）关联到一起。 当你滚动前景元素时，它会为背景元素创建动画以创建视差效果。 

要使用 ParallaxView 控件，你需要提供源元素、背景元素，并将 [VerticalShift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift)（针对垂直滚动）和/或  [HorizontalShift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift)（针对水平滚动）属性设置为大于零的值。 
* 源属性参考前景元素。 为了让视差效果出现，前景应为 [ScrollViewer](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) 或一个包含 ScrollViewer 的元素，例如 [ListView](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.listview) 或 [RichTextBox](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)。 

* 要设置背景元素，你需要将该元素添加为 ParallaxView 控件的子元素。 背景元素可以是任何 [UIElement](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement)，如[图像](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Image)或包含其他 UI 元素的面板。 

要创建视差效果，ParallaxView 必须位于前景元素之后。 [网格](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.grid)和[画布](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.canvas)面板让你能够以相互叠加的形式放置项目，以便它们与 ParallaxView 控件很好地配合使用。  

该示例创建了列表的视差效果：
 
```xaml
<Grid>
    <ParallaxView Source="{x:Bind ForegroundElement}" VerticalShift="50"> 
    
        <!-- Background element --> 
        <Image x:Name="BackgroundImage" Source="Assets/turntable.png"
               Stretch="UniformToFill"/>
    </ParallaxView>
    
    <!-- Foreground element -->
    <ListView x:Name="ForegroundElement">
       <x:String>Item 1</x:String> 
       <x:String>Item 2</x:String> 
       <x:String>Item 3</x:String> 
       <x:String>Item 4</x:String> 
       <x:String>Item 5</x:String>  
       <x:String>Item 6</x:String> 
       <x:String>Item 7</x:String> 
       <x:String>Item 8</x:String> 
       <x:String>Item 9</x:String> 
       <x:String>Item 10</x:String>     
       <x:String>Item 11</x:String> 
       <x:String>Item 13</x:String> 
       <x:String>Item 14</x:String> 
       <x:String>Item 15</x:String> 
       <x:String>Item 16</x:String>     
       <x:String>Item 17</x:String> 
       <x:String>Item 18</x:String> 
       <x:String>Item 19</x:String> 
       <x:String>Item 20</x:String> 
       <x:String>Item 21</x:String>        
    </ListView>
</Grid>
``` 

ParallaxView 自动调整图像的大小，以便它适合视差操作，让你无需担心图像滚动到视线之外。

## <a name="customizing-the-parallax-effect"></a>自定义视差效果 

VerticalShift 和 HorizontalShift 属性让你可以控制视差效果的程度。

* VerticalShift 属性指定在整个视差操作期间，我们想要背景垂直移动多远。 值 0 表示背景根本不移动。
* HorizontalShift 属性指定在整个视差操作期间，我们想要背景水平移动多远。 值 0 表示背景根本不移动。

更大的值可创建更显著的效果。 

有关自定义视差方式的完整列表，请参阅 ParallaxView 类。 

## <a name="dos-and-donts"></a>应做事项和禁止事项

- 在有背景图像的列表中使用视差
- 当 ListViewItems 包含图像时，考虑在 ListViewItems 中使用视差
- 请勿到处使用它，过度使用可削弱它的影响力

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

- [ParallaxView 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) 
- [UWP 的 Fluent 设计](../fluent-design-system/index.md)
- [系统内的科学：Fluent 设计和深色](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
