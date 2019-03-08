---
description: 了解改进广告单元可见性的方法。
title: 优化广告单元的可见性
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, 广告, 指南, 可见性
ms.localizationpriority: medium
ms.openlocfilehash: 87e21f4e98c58f79f397c369891212eccb196c18
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662262"
---
# <a name="optimize-the-viewability-of-your-ad-units"></a>优化广告单元的可见性

[广告效果报告](../publish/advertising-performance-report.md)包括广告单元的可见性指标。 广告行业日趋重视可视化展示，而不只限于提供展示，所以可见性是一项重要指标。 广告厂商倾向于对可视化展示出价，因为他们的广告有更多机会被用户看到。  

根据 IAB 可见性指南，横幅广告展示如果满足以下条件将被视为可查看：

* 像素要求：大于或等于 50%的像素中播发已在应用程序在可视空间上。
* 时间要求：满足像素要求的时间为大于或等于一个连续秒，发布 ad 呈现。

视频广告展示如果满足以下条件被视为可查看：

* 像素要求：大于或等于播发中 50%的像素是应用程序的可查看部分上。
* 时间要求：视频满足像素要求，post ad 呈现的两个连续秒播放。

可见性使用以下公式计算:

**在工作场所内 = [查看印记] * 100 / [总量广告印记重要]**

## <a name="guidelines-to-improve-ad-unit-viewability"></a>改进广告单元可见性指南

若要优化广告单元的可视化展示，请按照以下指南操作。

1. **测量效果**&nbsp;&nbsp;使用[广告效果报告](../publish/advertising-performance-report.md)查看每个广告单元的可见性指标。
2.  **适当放置广告控件**&nbsp;&nbsp;一般情况下，广告被查看最多的位置是折叠上方（即用户无需滚动便可以看到的页面可见部分的底部）。 显示在页面顶部的广告不可以查看。 请考虑检查页面的每个元素，以了解如何优化应用中的可见空间。 请确保不要将广告控件放置在应用后台。
3.  **管理页面长度**&nbsp;&nbsp;较短的页面内容会提高可见性。 如果你的应用页面需要更长长度，应实现无限滚动。
4.  **固定广告位置**&nbsp;&nbsp;确保广告位于固定位置，即使在用户滚动时。 这将帮助你获得更多的查看时间并增加可见性比率。
5.  **设置不透明度**&nbsp;&nbsp;确保包含广告控件的容器的透明度为 100%。
6.  **实现延迟加载**&nbsp;&nbsp;在用户滚动到包含广告控件的视图时再加载广告。 这将确保广告显示更久以供用户查看。
7.  **试验各种广告大小**&nbsp;&nbsp;选择一些广告大小，并使用[广告效果报告](../publish/advertising-performance-report.md)测量每个大小的可见性。 选择最适合你的那一个。
8.  **重新审查应用设计**&nbsp;&nbsp;对应用的页面设计加以改进，以减少页面加载时间。 用户常常在加载更快的页面上呆得更久。
