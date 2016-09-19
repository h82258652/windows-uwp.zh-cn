---
author: mcleanbyron
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: "了解如何在 UWP 应用中启用应用内购买和试用。"
title: "应用内购买和试用"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 99143d48a5f2155b0a47008574d0a78243dea925

---

# 应用内购买和试用

Windows SDK 提供可用于将应用内购买和试用功能添加到通用 Windows 平台 (UWP) 应用的 API，以帮助通过应用盈利和添加新功能。 这些 API 还提供应用的许可证信息访问权限。

对于这些方案，Windows 10 提供两个不同的 API：

* Windows 10 的所有版本都支持 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空间中用于应用内购买和许可证信息的 API。

* 从 Windows 10 版本 1607 开始，[Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空间中有用于应用内购买和许可证信息的备用 API。  

尽管这些命名空间中的 API 用于相同目标，但它们的设计完全不同，并且代码在两个 API 之间并不兼容。 如果你的应用面向 Windows 10 版本 1607 或更高版本，我们建议你使用 **Windows.Services.Store** 命名空间。 此命名空间支持最新的加载项类型（如应用商店管理的易耗型加载项），并且设计为与 Windows 开发人员中心和应用商店将来支持的产品和功能类型兼容。 **Windows.Services.Store** 命名空间还提升了性能。

本文介绍 UWP 应用的应用内购买，并提供 **Windows.Services.Store** 命名空间的概述，该命名空间从 Windows 10 版本 1607 开始可用。 有关使用 **Windows.ApplicationModel.Store** 命名空间中成员的信息，请参阅[使用 Windows.ApplicationModel.Store 命名空间的应用内购买和试用](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)。


## UWP 应用中的应用内购买概述

本部分介绍有关应用内购买和试用如何用于应用商店中 UWP 应用的核心概念。 这些概念中的大多数同时适用于 **Windows.Services.Store** 和 **Windows.ApplicationModel.Store** 命名空间。

应用商店提供的每一项通常称为*产品*。 大多数开发人员处理的都是以下类型的产品：*应用*和*加载项*（也称为应用内产品或 IAP）。 加载项是指你在应用的上下文中向客户提供的产品或功能。 加载项可以表示你向客户提供的任何功能：例如，要在应用或游戏中使用的货币、游戏的新地图或武器、使用无广告应用的功能，或数字内容，如应用的音乐或视频（前提是应用能够提供此类内容）。

每个应用和加载项都有关联的许可证，用于指示用户是否有权使用该应用或加载项。 如果用户有权将该应用或加载项作为试用来使用，则许可证还提供关于该试用的其他信息。

若要在应用中向客户提供加载项，请先[在开发人员中心仪表板中为应用定义加载项](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)。 然后，在应用中编写代码以确定用户是否有使用该加载项所表示功能的许可证，并且如果用户尚未拥有该功能的许可证，则可以提供以应用内购买形式向用户销售的加载项。 有关在面向 Windows 10 版本 1607 或更高版本的应用中使用 **Windows.Services.Store** 命名空间的相关任务的示例，请参阅以下文章：

* [获取应用和加载项的产品信息](get-product-info-for-apps-and-add-ons.md)
* [获取应用和加载项的许可证信息](get-license-info-for-apps-and-add-ons.md)
* [支持应用内购买应用和加载项](enable-in-app-purchases-of-apps-and-add-ons.md)
* [支持购买易耗型加载项](enable-consumable-add-on-purchases.md)
* [实现应用的试用版](implement-a-trial-version-of-your-app.md)

有关使用 **Windows.ApplicationModel.Store** 命名空间的相关任务的示例，请参阅[使用 Windows.ApplicationModel.Store 命名空间的应用内购买和试用](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)。

所有开发人员都可以创建以下类型的加载项。

| 加载项类型 |  说明  |
|---------|-------------------|
| 耐用品  |  持续时间为你在 [Windows 开发人员仪表板](https://msdn.microsoft.com/windows/uwp/publish/enter-iap-properties)中所指定生存期的加载项。 <p/><p/>默认情况下，耐用型加载项永远不会过期，在此情况下只能购买它们一次。 如果你为加载项指定特定的持续时间，则用户可以在它过期后重新购买该加载项。  |
| 开发人员管理的易耗品  |  可以购买、使用并再次购买的加载项。 此类型的加载项通常用作应用内货币。 <p/><p/>对于此类型的消耗品，你负责跟踪加载项所表示的商品的用户余量，并在用户消耗完所有商品后向应用商店将加载项购买报告为已完成。 在你的应用将之前的加载项购买报告为已完成前，用户无法再次购买该加载项。 <p/><p/>例如，如果你的加载项表示游戏中的 100 个硬币，并且用户消耗了 10 个硬币，则你的用户或服务必须为该用户保留 90 个硬币的新剩余余额。 在用户消耗完全部 100 个硬币后，你的应用必须将加载项报告为已完成，然后用户才可以再次购买 100 个硬币的加载项。    |
| 应用商店管理的易耗品  |  可以购买、使用并再次购买的加载项。 此类型的加载项通常用作应用内货币。<p/><p/>对于此类型的消耗品，应用商店会跟踪用户拥有的加载项所表示商品的余量。 当用户消耗任何商品时，你负责向应用商店报告这些商品已完成，然后应用商店会更新用户的余量。 你的应用可以随时查询用户的当前余量。 用户消耗完所有商品后，可以再次购买加载项。  <p/><p/> 例如，如果你的加载项在游戏中表示 100 个硬币的初始数量，并且用户消耗了 10 个硬币，则你的应用将向应用商店报告 10 个单位的加载项已完成，然后应用商店会更新剩余余额。 用户消耗完全部 100 个硬币后，可以再次购买 100 个硬币的加载项。 <p/><p/> **应用商店管理的易耗品从 Windows 10 版本 1607 开始可用。 在 Windows 开发人员中心仪表板中创建应用商店管理的易耗品的功能即将推出。**  |

<span />

>**注意**&nbsp;&nbsp;其他类型的加载项，如使用程序包的耐用型加载项（也称为可下载内容或 DLC），仅适用于有限的开发人员群体，本文档中未涉及。

<span id="api_intro" />
## Windows.Services.Store 命名空间简介

**Windows.Services.Store** 命名空间的主要入口点是 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 类。 此类提供的方法可用于获取当前应用及其可用加载项的信息、为当前用户购买应用或加载项、获取当前应用或其加载项的许可证信息以及其他任务。 若要获取 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 对象，请执行以下操作之一：

* 在单用户应用（即，仅在启动该应用的用户上下文中运行的应用），使用 [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) 方法获取一个 **StoreContext** 对象，你可以使用该对象为用户访问和管理与 Windows 应用商店相关的数据。 大多数通用 Windows 平台 (UWP) 应用都是单用户应用。

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* 在多用户应用中，使用 [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx) 方法获取一个 **StoreContext** 对象，你可以使用该对象为在使用该应用时登录 Microsoft 帐户的特定用户访问和管理与 Windows 应用商店相关的数据。 有关多用户应用的详细信息，请参阅[多用户应用程序简介](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications)。 以下示例为第一个可用用户获取 **StoreContext** 对象。

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

在拥有 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 后，你可以开始调用方法为当前用户购买应用或加载项以及执行其他任务。 有关详细信息，请参阅以下文章：

* [获取应用和加载项的产品信息](get-product-info-for-apps-and-add-ons.md)
* [获取应用和加载项的许可证信息](get-license-info-for-apps-and-add-ons.md)
* [支持应用内购买应用和加载项](enable-in-app-purchases-of-apps-and-add-ons.md)
* [支持购买易耗型加载项](enable-consumable-add-on-purchases.md)
* [实现应用的试用版](implement-a-trial-version-of-your-app.md)

有关演示如何使用 **Windows.Services.Store** 命名空间实现试用和应用内购买的完整示例，请参阅[应用商店示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)。

<span />
### 产品、SKU 和可用性的对象模型

应用商店中的每个产品都至少有一个 *SKU*，而每个 SKU 都至少有一个*可用性*。 这些概念抽象自 Windows 开发人员中心仪表板中的大多数开发人员，并且大多数开发人员永远不需要为其应用或加载项定义 SKU 或可用性。 但是，由于 **Windows.Services.Store** 命名空间中的应用商店产品的对象模型包括 SKU 和可用性，因此大致了解这些概念可能很有帮助。

| 对象类型 |  说明  |
|---------|-------------------|
| 产品  |  [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 类表示在应用商店中提供的任何类型的产品，包括应用或加载项。 此类提供可用于访问数据的属性，如产品的应用商店 ID、应用商店列表的图像和视频以及定价信息。 它还提供可用于购买产品的方法。 |
| SKU |  [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) 类表示产品的 *SKU*。 SKU 是带有其自己的说明、价格和其他独特产品详细信息的产品特定版本。 每个应用或加载项都有默认的 SKU。 大多数开发人员拥有针对一个应用的多个 SKU 的唯一情况是，他们要发布应用的完整版和试用版（在应用商店目录中，其中每一个版本都是同一个应用的不同 SKU）。 <p/><p/> 某些发布者能够定义他们自己的 SKU。 例如，大型游戏发布者可能发布具有以下两个 SKU 的游戏：一个 SKU 在不允许红色血液的市场中显示绿色血液，另一个 SKU 在所有其他市场中显示红色血液。 另外，销售数字视频内容的发布者可能针对一个视频发布两个 SKU，一个 SKU 用于高清版本，另一个 SKU 用于标清版本。 <p/><p/> 每个产品都有可用于访问 SKU 的 [Skus](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.skus.aspx) 属性。 |
| 可用性  |  [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) 类表示 SKU 的*可用性*。 可用性是带有自己独特定价信息的 SKU 的特定版本。 每个 SKU 都有默认的可用性。 某些发布者能够定义他们自己的可用性来为给定 SKU 引入不同的价格选项。 <p/><p/> 每个 SKU 都有可用于访问可用性的 [Availabilities](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.availabilities.aspx) 属性。 对于大多数开发人员来说，每个 SKU 都有单个默认可用性。  |

<span id="store_ids" />
### 应用商店 ID

应用商店中的每个应用和加载项都有关联的**应用商店 ID**。 **Windows.Services.Store** 命名空间中的许多 API 都需要应用商店 ID 才能执行有关应用或加载项的操作。 产品、SKU 和可用性具有不同的应用商店 ID 格式。

| 对象类型 |  应用商店 ID 格式  |
|---------|-------------------|
| 产品  |  应用商店中的任何产品的应用商店 ID 都是 12 个字符的字母数字字符串，例如 ```9NBLGGH4R315```。 此应用商店 ID 在应用或加载项的 Windows 开发人员中心仪表板中提供，由 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 对象的 [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.storeid.aspx) 属性返回。 此 ID 有时称为*产品应用商店 ID*。 |
| SKU |  对于 SKU，应用商店 ID 的格式为 ```<product Store ID>/xxxx```，其中 ```xxxx``` 是 4 个字符的字母数字字符串，用于标识产品的 SKU。 例如，```9NBLGGH4R315/000N```。 此 ID 由 [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) 对象的 [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.storeid.aspx) 属性返回，并且有时称为 *SKU 应用商店 ID*。 |
| 可用性  |  对于可用性，应用商店 ID 的格式为 ```<product Store ID>/xxxx/yyyyyyyyyyyy```，其中 ```xxxx``` 是标识产品 SKU 的 4 字符数字字母字符串，而 ```yyyyyyyyyyyy``` 是表示 SKU 可用性的 12 字符字母数字字符串。 例如，```9NBLGGH4R315/000N/4KW6QZD2VN6X```。 此 ID 由 [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) 对象的 [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.storeid.aspx) 属性返回，并且有时称为*可用性应用商店 ID*。  |

<span id="testing" />
### 测试使用 Windows.Services.Store 命名空间的应用

**Windows.Services.Store** 命名空间不提供可用于在测试期间模拟许可证信息的类。 必须将应用发布到应用商店，并将该应用下载到开发设备，才能将其许可证用于测试。 这是不同于使用 **Windows.ApplicationModel.Store** 命名空间的应用的体验，因为这些应用可以使用 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) 类在测试期间模拟许可证信息

如果你的应用使用 **Windows.Services.Store** 命名空间中的 API 访问你的应用及其加载项的信息，请按照以下过程测试代码：

1. 如果你的应用已经在应用商店中发布并提供，并且你希望更新此应用以使用 **Windows.Services.Store** 命名空间中的 API，则随时可以开始操作。 如果你希望为应用提供加载项，请确保在开发人员中心仪表板中[为应用定义加载项](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)。

  如果你尚未拥有在应用商店中发布并提供的应用，请生成一个满足最低 [Windows 应用认证工具包](https://developer.microsoft.com/windows/develop/app-certification-kit)要求的基本应用，并[将此应用提交](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)到 Windows 开发人员中心仪表板。 如果你希望为应用提供加载项，请确保[为应用定义加载项](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)。 或者，你可以在测试期间[在应用商店中隐藏该应用](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability)。

2. 在使用 **Windows.Services.Store** 命名空间中的 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 方法之一的应用中编写代码，以执行[获取可用于当前应用的加载项](get-product-info-for-apps-and-add-ons.md)、[购买应用或加载项](enable-in-app-purchases-of-apps-and-add-ons.md)或[获取你的应用的许可证信息](get-license-info-for-apps-and-add-ons.md)等任务。 有关更多示例，请参阅下方相关主题部分。

3. 在 Visual Studio 中，单击“项目菜单”****、指向“应用商店”****，然后单击“将应用程序与应用商店关联”****。 完成向导中的说明以将应用项目与 Windows 开发人员中心帐户中要用于测试的应用关联。

  >**注意**&nbsp;&nbsp;如果你未将项目与应用商店中的应用关联，则 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 方法会将其返回值的 **ExtendedError** 属性设置为错误代码值 0x803F6107。 此值指示应用商店并不了解关于该应用的任何信息。

4. 如果您尚未关联，请从应用商店安装您在上一步中指定的应用、运行该应用一次，然后关闭此应用。 这可确保将应用的有效许可证安装到你的开发设备。

5. 在 Visual Studio 中，开始运行或调试你的项目。 你的代码应从与你的本地项目关联的应用商店应用中检索应用和加载项数据。 如果系统提示你重新安装该应用，请按照说明进行操作，然后运行或调试你的项目。

## 相关主题

* [获取应用和加载项的产品信息](get-product-info-for-apps-and-add-ons.md)
* [获取应用和加载项的许可证信息](get-license-info-for-apps-and-add-ons.md)
* [支持应用内购买应用和加载项](enable-in-app-purchases-of-apps-and-add-ons.md)
* [支持购买易耗型加载项](enable-consumable-add-on-purchases.md)
* [实现应用的试用版](implement-a-trial-version-of-your-app.md)
* [使用 Windows.ApplicationModel.Store 命名空间的应用内购买和试用](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)



<!--HONumber=Aug16_HO5-->


