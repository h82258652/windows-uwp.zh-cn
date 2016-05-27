---
author: mijacobs
Description: 良好的图标会与版式和设计语言的其余部分相协调。 它们不会混合隐喻，并且会尽快且尽量简单地仅交流所需内容。 
title: 图标
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
---

# 适用于 UWP 应用的图标

良好的图标会与版式和设计语言的其余部分相协调。 它们不会混合隐喻，并且会尽快且尽量简单地仅交流所需内容。 

## 线性缩放大小渐变 

<table>
    <tr> 
        <td>16px x 16px</td>
        <td>24px x 24px</td>
        <td>32px x 32px</td>
        <td>48px x 48px</td>
    </tr>
    <tr> 
        <td>![Icons at 16x16 effective pixels](images/icons-16x16.png)</td>
        <td>![Icons at 24x24 effective pixels](images/icons-24x24.png)</td>
        <td>![Icons at 32x32 effective pixels](images/icons-32x32.png)</td>
        <td>![Icons at 48x48 effective pixels](images/icons-48x48.png)</td>
    </tr>
</table>

## 常见的形状

图标通常应无需太多填充即可最大化给定的空间。 这些形状提供起始点以调整基本形状的大小。 

![32px × 32px 网格](images/icons-common-shapes.png)

使用对应于图标方向的形状，并围绕这些基本参数进行撰写。 图标并不一定要填充或完全适合形状内部，并且可能根据需要进行调整以确保最佳平衡。 

<table>
    <tr>
        <td>圆形<td>
        <td>正方形</td>
        <td>三角形</td>
    </tr>
    <tr>
        <td>![A circle](images/icons-common-shapes-examples-1.png)<td>
        <td>![A square](images/icons-common-shapes-examples-2.png)</td>
        <td>![A triangle ](images/icons-common-shapes-examples-3.png)</td>
    </tr>
        <tr>
        <td>水平矩形<td>
        <td colspan="2">垂直矩形</td>        
        </tr>
    <tr>
        <td>![A horizontal rectangle](images/icons-common-shapes-examples-4.png)<td>
        <td colspan="2">![A vertical rectangle](images/icons-common-shapes-examples-5.png)</td>
         
    </tr>

</table>

## 角度

除了使用相同的网格和线条粗细外，图标也使用常见元素构建。 

在生成形状时仅使用这些角度会在所有图标上产生一致性，并确保图标正确呈现。 

在创建图标时，可以组合、连接、旋转和反射这些线条。 

<table>
    <tr>
        <td>**1:1**<br/>45°</td>
        <td>**1:2**<br />26.57°（垂直）<br/>63.43°（水平）</td>
        <td>**1:3**<br/>18.43°（垂直）<br/>71.57°（水平）</td>
        <td>**1:4**<br/>14.04°（垂直）<br/>75.96°（水平）</td>
    </tr>
    <tr>
        
        <td>![1:1](images/icons-grid-1-1.png)</td>
        <td>![1:2](images/icons-grid-1-2.png)</td>
        <td>![1:3](images/icons-grid-1-3.png)</td>
        <td>![1:4](images/icons-grid-1-4.png)</td>
    </tr>  
</table>

<p>下面提供了一些示例：</p>

<table>
    <tr>
        <td>![A 1:1 angle example](images/icons-angles-examples-1.png)</td>
        <td>![A 1:2 angle example](images/icons-angles-examples-2.png)</td>
        <td>![A 1:3 angle example](images/icons-angles-examples-3.png)</td>
        <td>![A 1:4 angle example](images/icons-angles-examples-4.png)</td>
    </tr>
</table>

## 曲线

曲线构建于整个圆的各个部分，并且在不需要贴靠到像素网格时不应扭曲。 

<table>
    <tr>
        <td>1/4 圆形</td>
        <td>1/8 圆形</td>
    </tr>
    <tr>
        <td>![1/4 circle](images/icons-curves-14circle.png)</td>
        <td>![1/8 circle](images/icons-curves-18circle.png)</td>
    </tr>
    <tr>
        <td>![1/4 cirlce example](images/icons-curves-examples-1.png)</td>
        <td>![1/8 circle example](images/icons-curves-examples-2.png)</td>
    </tr>    
</table>

## 几何构造

我们建议在构建图标时仅使用纯几何形状。

![带有几何覆盖的吉他图标 ](images/icons-geometric-construction.png)

## 填充的形状 

图标在需要时可以包含填充的形状，但它们在 32px × 32px 上不应超过 4px。 实心圆不应大于 6px × 6px。 

![5px × 8px 填充 ](images/icons-filled-shapes.png)

## 锁屏提醒

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
    <td>![Status badge ](images/icons-badge-common-states-1.png)</td>
    <td>![Action badge ](images/icons-badge-common-states-2.png)</td>
</tr>
</table>
<p></p>

### 锁屏提醒颜色 

颜色锁屏提醒应仅用于传达图标状态。 在状态锁屏提醒中使用的颜色会向用户传达特定的情感消息。 

<table>
<tr><td>绿色 - #128B44</td><td>蓝色 - #2C71B9</td><td>黄色 - #FDC214</td></tr>
<tr><td>积极：完结、完成 </td><td>中性：帮助、通知 </td><td>警告：警报、警告 </td></tr>
<tr><td>![Green status](images/icons-color-inbadging-1.png)</td><td>![Blue status](images/icons-color-inbadging-2.png)</td>
<td>![Yellow status](images/icons-color-inbadging-3.png)</td></tr>
</table>
<p></p>

### 锁屏提醒位置

任何状态或操作的默认位置都在右下方。 仅在设计不允许该位置时才使用其他位置。 

### 锁屏提醒大小调整

在 32 px × 32 px 网格上，锁屏提醒大小应设置为 10 - 18 px。 

## 相关文章

* [磁贴和图标资源指南](../controls-and-patterns/tiles-and-notifications-app-assets.md)


<!--HONumber=May16_HO2-->


