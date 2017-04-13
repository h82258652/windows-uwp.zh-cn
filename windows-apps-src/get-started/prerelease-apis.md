---
author: drewbat
ms.assetid: 
title: "开发具有预发行版 API 的 UWP 应用"
description: "了解使用 UWP SDK 预览版中所包括的预发行版 API 的好处和注意事项。"
ms.openlocfilehash: ede7e8d5e9cce39850edbdb70a715d76c78f0c01
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="developing-uwp-apps-with-pre-release-apis"></a>开发具有预发行版 API 的 UWP 应用

Microsoft 会发布通用 Windows 平台 (UWP) SDK 的预览版本，以允许开发人员在新平台功能完成之前执行这些新功能。 这样，你可以抢先将功能集成到你的应用中，并且有助于你在 SDK 的官方 RTM 版本发布后尽早发布应用。 通过使用预发行版 API，你还可以给 Microsoft 提供反馈，以便有助于在以后的版本中影响平台发展方向。

## <a name="important-limitations-on-the-use-of-pre-release-apis"></a>使用预发行版 API 的重要限制
在应用中使用预发行版 API 之前，请注意执行此操作的以下重要意义： 
* 在 RTM 版本中正式发布 API 之前，无法将使用预发行版 API 的应用提交到 Windows 应用商店。 我们强烈建议你将预发行版开发代码与当前已发布应用的代码分开。 
* 即使你未使用任何预发行版 API，也无法将使用预览版 SDK 开发的应用提交到应用商店。 你应该将预览工具安装在与用于主要开发的生产计算机分开的计算机或 VM 上。 
* 在发布 RTM 之前，预发行版 API 很可能会发生更改。 预览版 SDK 中包括 API 时，最终的 SDK 中将包括这些 API 所启用的功能或方案。 但在最终发布之前，特定 API 的名称、签名和行为可能会发生更改，也可能将彻底删除 API。 

## <a name="how-to-identify-a-prerelease-api"></a>如何确定预发行版 API 
在 UWP 的 API 参考文档中，本身为预发行版的 API 标有下列文本： 

本文讨论在商业发行之前可能发生重大修改的预发行版 API 或功能。 你可以立即使用此功能进行开发和测试，但使用此功能的应用直到在商业发行之后才能提交到 Windows 应用商店。 Microsoft 不对此处提供的信息作任何明示或默示的担保。 有关使用预发行版 API 进行开发的详细信息，请参阅“使用预发行版 API 开发 UWP 应用”。 

## <a name="get-the-latest-sdk-insider-preview"></a>获取最新的 SDK Insider Preview 
1. [注册加入 Windows 预览体验计划](https://insider.windows.com/) 以访问 SDK 的预览版本。 
3. 在安装开发人员预览工具之前，请查看[当前 SDK 和移动模拟器的发行说明](http://go.microsoft.com/fwlink/?LinkId=829180)。
4. 安装 [SDK Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK)。
5. 查看 [Windows Insider Preview 社区论坛](http://go.microsoft.com/fwlink/p/?LinkId=507620)
