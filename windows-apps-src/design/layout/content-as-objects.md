---
description: ''
title: 作为对象的内容
template: detail.hbs
ms.localizationpriority: medium
ms.openlocfilehash: ed2ac8530d69929cc0e0e921cfb1cc5368058cd2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593112"
---
# <a name="content-as-objects"></a>作为对象的内容

 

您可以操作元素的深度或 z 顺序来创建可帮助应用更易于使用的视觉层次结构。  

> 注意：本文是一项新功能的 Windows 10 RS2 的早期草稿。 特征名称、术语和功能并非最终版本。 

## <a name="why-visual-hierarchy-is-important"></a>为什么说视觉层次结构如此重要

用户不断被请求围攻以引起其注意。 屏幕上的每个元素都请求被查看，文本的每个字符串都想要被读取，每个按钮都想要被单击。 随着视觉环境日益混乱，解析场景并查明情况变得更加困难。  

这就是谨慎选择用户界面的元素如此重要的原因，而且还是在 UI 元素中创建建立清晰视觉层次结构的布局如此重要的原因。 <!-- Every element is competing for the user's attention, and every time you add an element, you add a mental tax to the user. -->

清晰的视觉层次结构可告知用户哪些元素是最重要的，并且在这些元素之间创建关系。 利用良好的视觉层次结构，用户一览即可了解页面的布局并且专注于他们努力完成的任务。 

<p></p>


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  <p>所以，你如何创建一个清晰的视觉层次结构？ 使用早期版本的 Windows 10，你可以使用空白、位置和版式来定义视觉层次结构。 </p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/flat-layout.png">平面布局</a>
    
  </div>
</div>
</div>

使用 Windows 10 RS2，我们逐个添加了另一个维度: 深度。 

<a href="images/content-as-objects/depth-in-layout2.png">在布局中的深度</a>


## <a name="use-depth-to-establish-a-hierarchy"></a>使用深度建立层次结构 

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
     <p>你可以使用深度（z 顺序）以及其他设计工具（空白、位置、版式）来建立层次结构。 将最重要元素提升至最上层；使用较低的层显示不太重要的 UI。 

    The relative importance of an element can change throughout an experience, so you can bring elements forward as they become more important and backward as they become less important. 
    </p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">在布局中的深度</a> 
    
  </div>
</div>
</div>

## <a name="how-does-it-work"></a>如何运作？
> TODO:如何控制元素的 z 顺序的简短说明。 对你来说，是否对 z 顺序显式进行硬编码，或是否有语义排序系统？ 如果将项目从一个层移动到另一个层？ 该系统将自动执行什么操作以及设计人员/开发人员需要担心什么？ 

## <a name="the-four-layers-of-a-typical-app-layers"></a>一个典型应用层的四个层

<p>一个典型应用具有 4 层。</p>
<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>超出背景</b>此层位于后面应用。  当元素迁移到此层时，我们建议使这些元素变为非交互。 此层上的元素具有最慢的视差且将剪裁到应用窗口。 TODO:此层作用范围？ 

<p>示例后台元素包括后面的内容，TODO 图像：示例中，TODO:示例。</p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">超越后台应用程序层</a>
    
  </div>
</div>
</div>

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>被动层</b>这是默认情况下，UI 元素所在的应用程序中，基本层。  元素在此层上实时移动（无视差），剪裁到应用窗口，并且以 100% 的比例呈现。 

<p>示例元素：应用背景，文本，如应用程序导航用户界面的辅助用户界面。</p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">应用在被动层</a>
    
  </div>
</div>
</div>

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>对操作的调用</b>这一层是交互式上面被动层元素确定优先级的项。 此层上的元素具有中等视差且将剪裁到应用窗口。 TODO:是否在此层规模较大的元素或具有投影？

<p>示例元素： 列表、 网格、 主命令 (TODO:Such as。...)。</p> 
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">应用程序在调用操作层</a>
    
  </div>
</div>
</div>

<p></p>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>Hero 层</b>时，这一层是最高的优先级元素在屏幕上。  此层上的元素可以打破应用窗口的边界，可以缩放，而且会自动获取投影。

<p>示例元素：照片元素、当前所选的项目。</p>  
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">应用在特大层</a>
    
  </div>
</div>
</div>



<!--
Depth is meaningful; it establishes visual and interactive hierarchy for users to efficiently complete tasks. Depth orients users in our system. 
-->

## <a name="example-tbd"></a>示例：TBD
> TODO:演示如何改编使用 z 顺序的通用 UI 模式。 我们应显示说明和代码。 

## <a name="download-the-code-samples"></a>下载代码示例
>TODO:链接到演示此功能的示例。 


## <a name="related-articles"></a>相关文章
* [内容基础知识](../basics/content-basics.md)
