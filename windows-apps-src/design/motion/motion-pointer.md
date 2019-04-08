---
Description: 使用指针动画向用户提供用户点击某个项目时的视觉反馈。
title: UWP 应用中的指针单击动画
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f1abd71cda8358e3f7e2fe36091f9c42f05bcb00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663792"
---
# <a name="pointer-click-animations"></a>指针单击动画



使用指针动画向用户提供用户点击某个项目时的视觉反馈。 当第一次点击某个项目时，指针向下动画会略微缩小和倾斜所按的项目，并且会进行播放。 当用户释放指针时，会播放指针向上动画，这会将该项目还原到其原始位置。


> **重要的 API**：[**PointerUpThemeAnimation 类**](https://msdn.microsoft.com/library/windows/apps/hh969168)， [ **PointerDownThemeAnimation 类**](https://msdn.microsoft.com/library/windows/apps/hh969164)


## <a name="dos-and-donts"></a>应做事项和禁止事项

-   在使用指针向上动画时，如果用户释放指针，将立即触发动画。 这样，即使点击触发的操作（如导航到新页面）的响应速度较慢，也可以为用户提供其操作已被识别的即时反馈。

## <a name="related-articles"></a>相关文章

* [动画概述](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [具有动画效果指针单击](https://msdn.microsoft.com/library/windows/apps/xaml/jj649432)
* [快速入门：对 UI 使用库动画进行动画处理](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**PointerUpThemeAnimation 类**](https://msdn.microsoft.com/library/windows/apps/hh969168)
* [**PointerDownThemeAnimation 类**](https://msdn.microsoft.com/library/windows/apps/hh969164)

 

 




