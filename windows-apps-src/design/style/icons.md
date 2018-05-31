---
author: mijacobs
Description: Good icons harmonize with typography and with the rest of the design language. They don’t mix metaphors, and they communicate only what’s needed, as speedily and simply as possible.
title: 图标
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 61157ad23eb55447137531922ea23fa0120e2b98
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
ms.locfileid: "1653916"
---
# <a name="icons-for-uwp-apps"></a>适用于 UWP 应用的图标



良好的图标会与版式和设计语言的其余部分相协调。 它们不会混合隐喻，并且会尽快且尽量简单地仅交流所需内容。 

## <a name="linear-scaling-size-ramps"></a>线性缩放大小渐变 

<table>
    <tr> 
        <td>16px x 16px</td>
        <td>24px x 24px</td>
        <td>32px x 32px</td>
        <td>48px x 48px</td>
    </tr>
    <tr> 
        <td><img src="images/icons-16x16.png" alt="Icons at 16x16 effective pixels" /></td>
        <td><img src="images/icons-24x24.png" alt="Icons at 24x24 effective pixels" /></td>
        <td><img src="images/icons-32x32.png" alt="Icons at 32x32 effective pixels" /></td>
        <td><img src="images/icons-48x48.png" alt="Icons at 48x48 effective pixels" /></td>
    </tr>
</table>

## <a name="common-shapes"></a>常见的形状

图标通常应无需太多填充即可最大化给定的空间。 这些形状提供起始点以调整基本形状的大小。 

![32px × 32px 网格](images/icons-common-shapes.png)

使用对应于图标方向的形状，并围绕这些基本参数进行撰写。 图标并不一定要填充或完全适合形状内部，并且可能根据需要进行调整以确保最佳平衡。 

<table class="uwpd-noborder">
    <tr>
        <td>圆形<td>
        <td>正方形</td>
        <td>三角形</td>
    </tr>
    <tr>
        <td><img src="images/icons-common-shapes-examples-1.png" alt="A circle" /><td>
        <td><img src="images/icons-common-shapes-examples-2.png" alt="A square" /></td>
        <td><img src="images/icons-common-shapes-examples-3.png" alt="A triangle " /></td>
    </tr>
        <tr>
        <td>水平矩形<td>
        <td colspan="2">垂直矩形</td>        
        </tr>
    <tr>
        <td><img src="images/icons-common-shapes-examples-4.png" alt="A horizontal rectangle" /><td>
        <td colspan="2"><img src="images/icons-common-shapes-examples-5.png" alt="A vertical rectangle" /></td>
         
    </tr>

</table>

## <a name="angles"></a>角度

除了使用相同的网格和线条粗细外，图标也使用常见元素构建。 

在生成形状时仅使用这些角度会在所有图标上产生一致性，并确保图标正确呈现。 

在创建图标时，可以组合、连接、旋转和反射这些线条。 

<table>
    <tr>
        <td><b>1:1</b><br/>45°</td>
        <td><b>1:2</b><br />26.57°（垂直）<br/>63.43°（水平）</td>
        <td><b>1:3</b><br/>18.43°（垂直）<br/>71.57°（水平）</td>
        <td><b>1:4</b><br/>14.04°（垂直）<br/>75.96°（水平）</td>
    </tr>
    <tr>
        
        <td><img src="images/icons-grid-1-1.png" alt="1:1" /></td>
        <td><img src="images/icons-grid-1-2.png" alt="1:2" /></td>
        <td><img src="images/icons-grid-1-3.png" alt="1:3" /></td>
        <td><img src="images/icons-grid-1-4.png" alt="1:4" /></td>
    </tr>  
</table>

<p>下面提供了一些示例：</p>

<table>
    <tr>
        <td><img src="images/icons-angles-examples-1.png" alt="A 1:1 angle example" /></td>
        <td><img src="images/icons-angles-examples-2.png" alt="A 1:2 angle example" /></td>
        <td><img src="images/icons-angles-examples-3.png" alt="A 1:3 angle example" /></td>
        <td><img src="images/icons-angles-examples-4.png" alt="A 1:4 angle example" /></td>
    </tr>
</table>

## <a name="curves"></a>曲线

曲线构建于整个圆的各个部分，并且在不需要贴靠到像素网格时不应扭曲。 

<table>
    <tr>
        <td>1/4 圆形</td>
        <td>1/8 圆形</td>
    </tr>
    <tr>
        <td><img src="images/icons-curves-14circle.png" alt="1/4 circle" /></td>
        <td><img src="images/icons-curves-18circle.png" alt="1/8 circle" /></td>
    </tr>
    <tr>
        <td><img src="images/icons-curves-examples-1.png" alt="1/4 cirlce example" /></td>
        <td><img src="images/icons-curves-examples-2.png" alt="1/8 circle example" /></td>
    </tr>    
</table>

## <a name="geometric-construction"></a>几何构造

我们建议在构建图标时仅使用纯几何形状。

![带有几何覆盖的吉他图标 ](images/icons-geometric-construction.png)

## <a name="filled-shapes"></a>填充的形状 

图标在需要时可以包含填充的形状，但它们在 32px × 32px 上不应超过 4px。 实心圆不应大于 6px × 6px。 

![5px × 8px 填充 ](images/icons-filled-shapes.png)

## <a name="badges"></a>锁屏提醒

“锁屏提醒”是用于描述添加到不会与基本图标元素集成的图标的元素的通用术语。 这些通常会传达关于图标的其他信息，例如状态或操作。 其他常用术语包括：覆盖、批注或修饰符。 

![状态锁屏提醒 ](images/icons-badge-status.png)

![操作锁屏提醒 ](images/icons-badge-action.png)

状态锁屏提醒使用图标顶部填充的、有颜色的对象，而操作锁屏提醒集成到单色样式和线条粗细都相同的图标。

<table>
<tr>
    <td>常见的状态锁屏提醒</td>
    <td>常见的操作锁屏提醒</td>
</tr>
<tr>
    <td><img src="images/icons-badge-common-states-1.png" alt="Status badge " /></td>
    <td><img src="images/icons-badge-common-states-2.png" alt="Action badge " /></td>
</tr>
</table>
<p></p>

### <a name="badge-color"></a>锁屏提醒颜色 

颜色锁屏提醒应仅用于传达图标状态。 在状态锁屏提醒中使用的颜色会向用户传达特定的情感消息。 

<table>
<tr><td>绿色 - #128B44</td><td>蓝色 - #2C71B9</td><td>黄色 - #FDC214</td></tr>
<tr><td>积极：完结、完成 </td><td>中性：帮助、通知 </td><td>警告：警报、警告 </td></tr>
<tr><td><img src="images/icons-color-inbadging-1.png" alt="Green status" /></td><td><img src="images/icons-color-inbadging-2.png" alt="Blue status" /></td>
<td><img src="images/icons-color-inbadging-3.png" alt="Yellow status" /></td></tr>
</table>
<p></p>

### <a name="badge-position"></a>锁屏提醒位置

任何状态或操作的默认位置都在右下方。 仅在设计不允许该位置时才使用其他位置。 

### <a name="badge-sizing"></a>锁屏提醒大小调整

在 32 px × 32 px 网格上，锁屏提醒大小应设置为 10 - 18 px。 

## <a name="related-articles"></a>相关文章

* [磁贴和图标资源指南](../shell/tiles-and-notifications/app-assets.md)
