---
author: mcleanbyron
ms.assetid: 32572890-26E3-4FBB-985B-47D61FF7F387
description: "了解如何启用 UWP 应用中的应用内购买和试用（定向 Windows10 版本 1607 之前的版本）。"
title: "使用 Windows.ApplicationModel.Store 命名空间进行应用内购买和试用"
translationtype: Human Translation
ms.sourcegitcommit: 812fa1789c5c86657b8e73e45a851c7a58a1c84e
ms.openlocfilehash: 5a4f943357660a22217351f04d735c14cab828ff

---

# 使用 Windows.ApplicationModel.Store 命名空间进行应用内购买和试用

Windows SDK 提供可用于将应用内购买和试用功能添加到通用 Windows 平台 (UWP) 应用的 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空间中的成员，帮助通过应用盈利和添加新功能。 这些 API 还提供应用的许可证信息访问权限。

本部分中的文章提供有关针对多个常见方案使用 **Windows.ApplicationModel.Store** 命名空间中的成员的深入指南和代码示例。 有关与 UWP 中的应用内购买相关的概念概述，请参阅[应用内购买和试用](in-app-purchases-and-trials.md)。

有关演示如何使用 **Windows.ApplicationModel.Store** 命名空间实现试用和应用内购买的完整示例，请参阅[应用商店示例](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)。

>**注意**&nbsp;&nbsp;
>
> * 如果你的应用面向 Windows10 版本 1607 或更高版本，我们建议使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空间的成员，而非使用 **Windows.ApplicationModel.Store** 命名空间。 **Windows.Services.Store** 命名空间支持最新的加载项类型（如应用商店管理的易耗型加载项），并且设计为与 Windows 开发人员中心和应用商店将来支持的产品和功能类型兼容。 **Windows.Services.Store** 命名空间还设计用于提供更好的性能。 有关详细信息，请参阅[应用内购买和试用](in-app-purchases-and-trials.md)。
<br/><br/>
> * **Windows.ApplicationModel.Store** 命名空间在使用[桌面桥](https://developer.microsoft.com/windows/bridges/desktop)的 Windows 桌面应用程序中不受支持。 这些应用程序必须使用 **Windows.Services.Store** 命名空间才能实现应用内购买和试用。

## 本部分内容


| 主题                                                                                                       | 描述                 |
|-------------------------------------------------------------------------------------------------------------|-----------------------------|
| [启用应用内产品购买](enable-in-app-product-purchases.md)      |  无论你的应用是否免费，你都可以直接从应用中销售内容、其他应用或新的应用功能（例如解锁游戏的下一关）。 我们在此处显示了如何在应用中启用这些产品。  |
| [启用可消费应用内产品购买](enable-consumable-in-app-product-purchases.md)      | 通过应用商店商业平台提供可消费应用内产品（这些项目可以进行购买、使用和再次购买），以便为客户提供强大可靠的购买体验。 这对游戏内货币（金子、金币等）等来说尤为有用，可以购买此类货币，然后将其用于购买特定道具。 |
| [排除或限制试用版中的功能](exclude-or-limit-features-in-a-trial-version-of-your-app.md) | 如果允许客户在试用期内免费使用你的应用，则可以通过排除或限制试用期内的某些功能，吸引客户升级到完整版应用。 |
| [管理应用内产品的大目录](manage-a-large-catalog-of-in-app-products.md)      |   如果你的应用提供较大的应用内产品目录，可以选择按照本主题中描述的过程帮助管理你的目录。    |
| [使用收据验证产品购买](use-receipts-to-verify-product-purchases.md)      |   每个促成成功产品购买的 Windows 应用商店交易都可以选择返回交易收据，该收据可向客户提供有关所列产品和货币成本的信息。 有权访问此信息可支持以下方案：你的应用需要验证用户是否购买了你的应用，或者是否已从 Windows 应用商店进行了应用内产品购买。 |



<!--HONumber=Nov16_HO1-->


