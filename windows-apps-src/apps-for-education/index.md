---
title: 开发教育应用。
description: 本部分介绍可供你针对 Windows10 平台编写教育应用的通用 Window 应用资源。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp，教育版
ms.assetid: 2431f253-efe3-4895-b131-34653b61f13c
ms.localizationpriority: medium
ms.openlocfilehash: 7696cd785b4a8720f6eefb7bc897d13ffb0c7115
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7844219"
---
# <a name="develop-universal-windows-apps-for-education"></a>开发通用 Windows 应用教育版
![参加测验应用屏幕截图](images/take-a-test-screen-small.png)

以下资源将有助于你编写通用 Windows 应用教育版。

### <a name="accessibility"></a>辅助功能
教育应用需要可供访问。 有关详细信息，请参阅[针对辅助功能开发应用](https://developer.microsoft.com/windows/accessible-apps)。


### <a name="secure-assessments"></a>安全评估
为了防止学生在测试时使用其他计算机或 Internet 资源，评估/测试应用通常需要生成*锁定*环境。 此功能通过[参加测验 API](take-a-test-api.md)提供。 有关为高利害关系测试锁定了在线评估的测试环境的示例，请参阅 Windows IT 中心中的[参加测验](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10) Web 应用。

### <a name="user-input"></a>用户输入
用户输入是教育应用的关键部分；UI 控件必须具有响应性且直观显示，以便中断其用户的焦点。 有关可用于通用 Windows 应用的输入选项的一般概述，请参阅“设计和 UI”部分中的[输入基础版](https://docs.microsoft.com/windows/uwp/design/input/input-primer)及其下方的主题。 此外，以下示例应用展示了在通用 Windows 平台中处理的基本 UI。
- [基本输入示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)显示如何在通用 Windows 应用中处理输入。
- [用户交互模式示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)显示如何检测和响应用户交互模式。
- [焦点视觉对象示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)显示如何充分利用新的系统绘制焦点视觉对象，或者如果系统绘制的焦点视觉对象不符合你的需求，如何创建你自己的自定义焦点视觉对象。

Windows Ink 平台通过使教育应用适应学生惯于使用的输入模式，使这些应用引人注目。 有关在应用中实现 Windows Ink 的综合指南，请参阅[笔交互和 Windows Ink](https://docs.microsoft.com/windows/uwp/design/input/pen-and-stylus-interactions) 及其下方的主题。 以下示例应用提供此 API 的工作示例。
- [墨迹示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Ink)介绍如何在使用 JavaScript 的通用 Windows 应用中使用墨迹功能（如捕获、操作和解释笔划墨迹）。
- [简单的墨迹示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)介绍如何在使用 C# 的通用 Windows 应用中使用墨迹功能（如从用户输入中捕获墨迹和对笔划墨迹执行手写识别）。
- [复杂的墨迹示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)介绍如何使用高级 InkPresenter 功能使墨迹与其他对象交错、选择墨迹、复制/粘贴和处理事件。 它以使用 C++ 的通用 Windows 平台为基础生成，并且可以在桌面版和移动版 Windows10 SKU 上运行。


### <a name="microsoft-store"></a>Microsoft Store
教育应用通常在特定情况下发布给特定组织。 若要获取相关信息，请参阅[将业务线应用分配到企业](https://msdn.microsoft.com/windows/uwp/publish/distribute-lob-apps-to-enterprises)。

## <a name="related-topics"></a>相关主题
- Windows IT 中心上的 [Windows10 教育版](https://technet.microsoft.com/edu/windows/index)
