---
title: 窗口与全屏模式
description: Direct3D 应用程序可以在窗口模式或全屏模式中运行。
ms.assetid: EE8B9F87-822B-4576-A446-CA603E786862
keywords:
- 窗口与全屏模式
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d84bbebfaf19b756e6abc6c592187b6b0ee92200
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8692968"
---
# <a name="span-iddirect3dconceptswindowedvsfull-screenmodespanwindowed-vs-full-screen-mode"></a><span id="direct3dconcepts.windowed_vs__full-screen_mode"></span>窗口与全屏模式


Direct3D 应用程序可以在窗口模式或全屏模式中运行。 在*窗口模式*中，此应用程序与所有运行的应用共享可用的桌面屏幕空间。 在*全屏模式*中，应用程序运行于的窗口将覆盖整个桌面，并隐藏所有运行的应用（包括你的开发环境）。 通常，游戏默认为全屏模式，以便通过隐藏所有运行的应用程序来使用户完全沉浸在游戏中。

全屏模式和窗口模式之间的代码差异非常小。

由于在全屏模式中运行的应用程序会占用整个屏幕，因此调试应用程序需要单独的监视器或需要使用远程调试器。 窗口模式应用程序的一个优势是，你可以在调试器中单步执行代码，而无需多个监视器或一个远程调试器。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[设备](devices.md)

 

 




