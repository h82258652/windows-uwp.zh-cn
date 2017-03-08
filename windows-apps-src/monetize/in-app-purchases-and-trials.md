---
author: mcleanbyron
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: "了解如何在 UWP 应用中启用应用内购买和试用。"
title: "应用内购买和试用"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 应用内购买, IAP, 加载项, 试用, 消耗品, 耐用型"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ade314f79edf73527f29e937f8eab987c902802f
ms.lasthandoff: 02/07/2017

---

# <a name="in-app-purchases-and-trials"></a>应用内购买和试用

Windows SDK 提供可用于实现以下功能的 API，以从通用 Windows 平台 (UWP) 应用获取更多收益：

* **应用内购买**&nbsp;&nbsp;无论你的应用是否免费，你都可以直接从应用中销售内容或新的应用功能（例如解锁游戏的下一关卡）。

* **试用功能**&nbsp;&nbsp;如果你[在 Windows 开发人员中心仪表板中将应用配置为免费试用](../publish/set-app-pricing-and-availability.md#free-trial)，则可通过在试用期内排除或限制某些功能吸引客户购买应用的完整版。 也可以在客户购买你的应用之前，启用仅在试用期才会出现的某些功能，如横幅或水印。

本文提供应用内购买和试用在 UWP 应用内的工作原理概述。

<span id="choose-namespace" />
## <a name="choose-which-namespace-to-use"></a>选择要使用哪个命名空间

有两个不同的命名空间可用于向 UWP 应用添加应用内购买和试用功能，具体取决于应用面向 Windows 10 的哪个版本。 尽管这些命名空间中的 API 用于相同目标，但它们的设计完全不同，并且代码在两个 API 之间并不兼容。

* **[Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)**&nbsp;&nbsp;从 Windows 10 版本 1607 开始，应用可使用此命名空间中的 API 实现应用内购买和试用。 如果应用面向 Windows 10 版本 1607 或更高版本，我们建议使用此命名空间中的成员。 此命名空间支持最新的加载项类型（如应用商店管理的易耗型加载项），并且设计为与 Windows 开发人员中心和应用商店将来支持的产品和功能类型兼容。 有关此命名空间的详细信息，请参阅本文中的[使用 Windows.Services.Store 命名空间进行应用内购买和试用](#api_intro)部分。

* **[Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)**&nbsp;&nbsp;Windows 10 的所有版本还支持此命名空间中用于应用内购买和试用的较早 API。 尽管适用于 Windows 10 的任何 UWP 应用都可以使用此命名空间，但将来可能不会更新此命名空间以支持开发人员中心和应用商店中的新产品和功能类型。 有关此命名空间的信息，请参阅[使用 Windows.ApplicationModel.Store 命名空间的应用内购买和试用](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)。

<span id="concepts" />
## <a name="basic-concepts"></a>基本概念

本部分介绍 UWP 应用中的应用内购买和试用的基本概念。 除非另有说明，否则其中大多数概念同时适用于 **Windows.Services.Store** 和 **Windows.ApplicationModel.Store** 命名空间。

应用商店中提供的每一个项目通常称为*产品*。 大多数开发人员处理的都是以下类型的产品：*应用*和*加载项*（也称为应用内产品或 IAP）。

加载项是指你在应用的上下文中向客户提供的产品或功能：例如，要在应用或游戏中使用的货币、游戏的新地图或武器、使用无广告应用的功能，或数字内容，如应用的音乐或视频（前提是应用能够提供此类内容）。 每个应用和加载项都有关联的许可证，用于指示用户是否有权使用该应用或加载项。 如果用户有权将该应用或加载项作为试用来使用，则许可证还提供关于该试用的其他信息。

若要在应用中向客户提供加载项，必须[在开发人员中心仪表板中为应用定义加载项](../publish/iap-submissions.md)以便使应用商店知道它。 然后，应用可以使用 **Windows.Services.Store** 或 **Windows.ApplicationModel.Store** 命名空间中的 API 提供加载项，作为应用内购买向用户销售。

UWP 应用可提供以下类型的加载项。

| 加载项类型 |  说明  |
|---------|-------------------|
| 耐用品  |  持续时间为你在 [Windows 开发人员仪表板](../publish/enter-iap-properties.md)中所指定生存期的加载项。 <p/><p/>默认情况下，耐用型加载项永远不会过期，在此情况下只能购买它们一次。 如果你为加载项指定特定的持续时间，则用户可以在它过期后重新购买该加载项。 |
| 开发人员管理的易耗品  |  可以购买、使用并再次购买的加载项。 此类型的加载项通常用于获取应用内收入。 <p/><p/>对于此类型的易耗品，你负责跟踪加载项所表示的商品的用户余量，并在用户消耗完所有商品后向应用商店将加载项购买报告为已完成。 在你的应用将之前的加载项购买报告为已完成前，用户无法再次购买该加载项。 <p/><p/>例如，如果你的加载项表示游戏中的 100 个硬币，并且用户消耗了 10 个硬币，则你的用户或服务必须为该用户保留 90 个硬币的新剩余余额。 在用户消耗完全部 100 个硬币后，你的应用必须将加载项报告为已完成，然后用户才可以再次购买 100 个硬币的加载项。    |
| 应用商店管理的易耗品  |  可以购买、使用并再次购买的加载项。 此类型的加载项通常用于获取应用内收入。<p/><p/>对于此类型的易耗品，应用商店会跟踪用户拥有的加载项所表示商品的余量。 当用户消耗任何商品时，你负责向应用商店报告这些商品已完成，然后应用商店会更新用户的余量。 你的应用可以随时查询用户的当前余量。 用户消耗完所有商品后，可以再次购买加载项。  <p/><p/> 例如，如果你的加载项在游戏中表示 100 个硬币的初始数量，并且用户消耗了 10 个硬币，则你的应用将向应用商店报告 10 个单位的加载项已完成，然后应用商店会更新剩余余额。 用户消耗完全部 100 个硬币后，可以再次购买 100 个硬币的加载项。 <p/><p/>**注意**&nbsp;&nbsp;应用商店管理的易耗品从 Windows 10 版本 1607 开始可用。 若要使用应用商店管理的易耗品，应用必须面向 Windows 10 版本 1607 或更高版本，并且必须使用 **Windows.Services.Store** 命名空间（而不是 **Windows.ApplicationModel.Store** 命名空间）中的 API。  |

<span />

>**注意**&nbsp;&nbsp;其他类型的加载项，如使用软件包的耐用型加载项（也称为可下载内容或 DLC），仅适用于有限的开发人员群体，本文档中未涉及。

<span id="api_intro" />
## <a name="in-app-purchases-and-trials-using-the-windowsservicesstore-namespace"></a>使用 Windows.Services.Store 命名空间的应用内购买和试用

本文的其余部分介绍了如何使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空间实现应用内购买和试用。 此命名空间仅适用于面向 Windows 10 版本 1607 或更高版本的应用，我们建议应用使用此命名空间，而非 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)（如果可能）。

如果要查看有关 **Windows.ApplicationModel.Store** 命名空间的信息，请参阅[本文](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)。

### <a name="get-started-with-the-storecontext-class"></a>StoreContext 类入门

**Windows.Services.Store** 命名空间的主要入口点是 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 类。 此类提供的方法可用于获取当前应用及其可用加载项的信息、获取当前应用或其加载项的许可证信息、为当前用户购买应用或加载项以及执行其他任务。 若要获取 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 对象，请执行以下操作之一：

* 在单用户应用（即，仅在启动该应用的用户上下文中运行的应用），使用静态 [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) 方法获取一个 **StoreContext** 对象，你可以使用该对象访问与用户的 Windows 应用商店相关数据。 大多数通用 Windows 平台 (UWP) 应用是单用户应用。

  > [!div class="tabbedCodeSnippets"]
  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* 在[多用户应用](../xbox-apps/multi-user-applications.md)中，使用静态 [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx) 方法获取一个 **StoreContext** 对象，你可以使用该对象访问特定用户（使用该应用时使用 Microsoft 帐户登录的用户）的 Windows 应用商店相关数据。 以下示例为第一个可用用户获取 **StoreContext** 对象。

  > [!div class="tabbedCodeSnippets"]
  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

>**注意**&nbsp;&nbsp;使用[桌面桥](https://developer.microsoft.com/windows/bridges/desktop)的 Windows 桌面应用程序必须执行额外步骤来配置 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 对象，然后才可以使用此对象。 有关详情，请参阅[本部分](#desktop)。

拥有 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 对象后，可开始调用此对象的方法来获取当前应用以及加载项的应用商店产品信息、检索当前应用及其加载项的许可证信息、为当前用户购买应用或加载项以及执行其他任务。 有关可使用此对象执行的常见任务的详细信息，请参阅以下文章：

* [获取应用和加载项的产品信息](get-product-info-for-apps-and-add-ons.md)
* [获取应用和加载项的许可证信息](get-license-info-for-apps-and-add-ons.md)
* [支持应用内购买应用和加载项](enable-in-app-purchases-of-apps-and-add-ons.md)
* [支持购买易耗型加载项](enable-consumable-add-on-purchases.md)
* [实现应用的试用版](implement-a-trial-version-of-your-app.md)

有关演示如何使用 **StoreContext** 和 **Windows.Services.Store** 命名空间中的其他类型的示例应用，请参阅[应用商店示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)。

<span id="implement-iap" />
### <a name="implement-in-app-purchases"></a>实现应用内购买

若要使用 **Windows.Services.Store** 命名空间在应用中向客户提供应用内购买：

1. 如果应用提供可供客户购买的加载项，[请在开发人员中心仪表板中为应用创建加载项提交](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions)。

2. 在应用中编写代码以[检索应用或应用提供的加载项的产品信息](get-product-info-for-apps-and-add-ons.md)，然后[确定许可证是否处于活动状态](get-license-info-for-apps-and-add-ons.md)（即，用户是否具有使用该应用或加载项的许可证）。 如果许可证不处于活动状态，请显示提供应用或加载项以作为应用内购买向用户销售的 UI。

3. 如果用户选择购买应用或加载项，请使用相应的方法购买该产品：

  * 如果用户正在购买应用或耐用型加载项，请按照[启用应用或加载项的应用内购买](enable-in-app-purchases-of-apps-and-add-ons.md)中的说明操作。

  * 如果用户正在购买易耗型加载项，请按照[启用易耗型加载项购买](enable-consumable-add-on-purchases.md)中的说明操作。

4. 按照本文中的[测试指南](#testing)测试实现。

<span id="implement-trial" />
### <a name="implement-trial-functionality"></a>实现试用功能

若要使用 **Windows.Services.Store** 命名空间排除或限制应用的试用版中的功能：

1. [在 Windows 开发人员中心仪表板中将应用配置为免费试用](../publish/set-app-pricing-and-availability.md#free-trial)。

2. 在应用中编写代码以[检索应用或应用提供的加载项的产品信息](get-product-info-for-apps-and-add-ons.md)，然后[确定与应用关联的许可证是否是试用许可证](get-license-info-for-apps-and-add-ons.md)。

3. 如果是试用，则排除或限制应用中的特定功能，然后在用户购买完整许可证时启用这些功能。 有关详细信息和代码示例，请参阅[实现应用的试用版](implement-a-trial-version-of-your-app.md)。

4. 按照本文中的[测试指南](#testing)测试实现。

<span id="testing" />
### <a name="test-your-in-app-purchase-or-trial-implementation"></a>测试你的应用内购买或试用实现

**Windows.Services.Store** 命名空间不提供可用于在测试期间模拟许可证信息的类。 必须将应用发布到应用商店，并将该应用下载到开发设备，才能将其许可证用于测试。 这是与使用 **Windows.ApplicationModel.Store** 命名空间的应用不同的体验，该命名空间可使用 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) 类在测试期间模拟许可证信息。

如果应用使用 **Windows.Services.Store** 命名空间中的 API 访问应用及其加载项的信息，请按照此过程测试代码：

1. 如果应用尚未在应用商店中发布和可用，请确保应用满足 [Windows 应用认证工具包](https://developer.microsoft.com/windows/develop/app-certification-kit)最低要求，[将应用提交](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)到 Windows 开发人员中心仪表板，并确保应用通过认证过程以便可在应用商店中使用。 你可以[从应用商店隐藏应用](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability)，以便在你测试它时，它对客户不可用。

2. 接下来，确保已完成以下操作：

  * 在使用 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 类和 **Windows.Services.Store** 命名空间中的其他相关类型的应用中编写代码以实现[应用内购买](#implement-iap)或[试用功能](#implement-trial)。

  * 如果应用提供一个可供客户购买的加载项，[请在开发人员中心仪表板中为应用创建加载项提交](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions)。

  * 如果要在应用的试用版中排除或限制某些功能，请[在 Windows 开发人员中心仪表板中将应用配置为免费试用](../publish/set-app-pricing-and-availability.md#free-trial)。

3. 在项目在 Visual Studio 中打开的情况下，单击**项目菜单**、指向**应用商店**，然后单击**将应用与应用商店关联**。 完成向导中的说明以将应用项目与 Windows 开发人员中心帐户中要用于测试的应用关联。

  >**注意**&nbsp;&nbsp;如果你未将项目与应用商店中的应用关联，则 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 方法会将其返回值的 **ExtendedError** 属性设置为错误代码值 0x803F6107。 此值指示应用商店并不了解关于该应用的任何信息。

4. 如果您尚未关联，请从应用商店安装您在上一步中指定的应用、运行该应用一次，然后关闭此应用。 这可确保将应用的有效许可证安装到你的开发设备。

5. 在 Visual Studio 中，开始运行或调试你的项目。 你的代码应从与你的本地项目关联的应用商店应用中检索应用和加载项数据。 如果系统提示你重新安装该应用，请按照说明操作，然后运行或调试项目。

>**注意**&nbsp;&nbsp;完成步骤 1-5 后，你可以继续更新应用代码并在开发计算机上调试更新的项目，而无需将新的应用包提交到应用商店。 获取将要用于测试的本地许可证后，只需要将应用的应用商店版本下载到开发计算机。 完成测试后，只需要将新的应用包提交到应用商店，并允许客户使用应用内购买或应用中的试用相关功能。

<span id="receipts" />
### <a name="receipts-for-in-app-purchases"></a>应用内购买的收据

**Windows.Services.Store** 命名空间不在应用代码中提供可用于获取成功购买的交易收据的 API。 这是与使用 **Windows.ApplicationModel.Store** 命名空间的应用不同的体验，该命名空间可[使用客户端 API 检索交易收据](use-receipts-to-verify-product-purchases.md)。

如果使用 **Windows.Services.Store** 命名空间实现应用内购买并且希望验证给定客户是否已购买某个应用或加载项，可使用 [Windows Store collection REST API](query-for-products.md) 中的[查询产品方法](view-and-grant-products-from-a-service.md)。 此方法的返回数据确认指定客户是否具有对给定产品的权利，并为用户获取产品的交易提供数据。 Windows 应用商店收集 API 使用 Azure AD 身份验证检索此信息。

<span id="desktop" />
### <a name="using-the-storecontext-class-with-the-desktop-bridge"></a>通过桌面桥使用 StoreContext 类

使用[桌面桥](https://developer.microsoft.com/windows/bridges/desktop)的桌面应用程序可使用 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 类实现应用内购买和试用。 但是，如果你有 Win32 桌面应用程序或具有与呈现框架（如 WPF 应用程序）相关联的窗口句柄 (HWND) 的桌面应用程序，则该应用程序必须配置 **StoreContext** 对象以为该对象所显示的模式对话框指定哪个应用程序窗口是所有者窗口。

许多 **StoreContext** 成员（以及通过 **StoreContext** 对象访问的其他相关类型的成员）针对应用商店相关的操作（如购买产品）向用户显示模式对话框。 如果桌面应用程序未配置 **StoreContext** 对象以指定模式对话框的所有者窗口，则此对象将返回不准确的数据或错误。

若要在使用桌面桥的桌面应用程序中配置 **StoreContext** 对象，请按照这些步骤操作。

  1. 进行以下操作之一，使你的应用可以访问 [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx) 接口：

    * 如果应用程序使用托管语言（如 C# 或 Visual Basic）编写，则在应用代码中使用 [ComImport](https://msdn.microsoft.com/library/system.runtime.interopservices.comimportattribute.aspx) 属性声明 **IInitializeWithWindow** 接口，如以下 C# 示例所示。 此示例假设代码文件具有 **System.Runtime.InteropServices** 命名空间的 **using** 语句。

      > [!div class="tabbedCodeSnippets"]
      ```csharp
      [ComImport]
      [Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
      [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
      public interface IInitializeWithWindow
      {
          void Initialize(IntPtr hwnd);
      }
      ```

    * 如果应用程序是采用 C++ 编写的，请在代码中添加对 shobjidl.h 头文件的引用。 此头文件包含 **IInitializeWithWindow** 接口的声明。

  2. 按照本文前面部分所述，使用 [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) 方法（或 [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx)，如果你的应用是[多用户应用](../xbox-apps/multi-user-applications.md)）获取 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 对象并将此对象转换为 [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx) 对象。 然后，调用 [IInitializeWithWindow.Initialize](https://msdn.microsoft.com/library/windows/desktop/hh706982.aspx) 方法，并传递你希望成为所有者（对于 **StoreContext** 方法显示的任何模式对话框）的窗口的句柄。 以下示例 C# 显示如何将应用主窗口的句柄传递到该方法。

    > [!div class="tabbedCodeSnippets"]
    ```csharp
    StoreContext context = StoreContext.GetDefault();
    IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)context;
    initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
    ```

<span id="products-skus" />
### <a name="products-skus-and-availabilities"></a>产品、SKU 和可用性

应用商店中的每个产品都至少有一个 *SKU*，而每个 SKU 都至少有一个*可用性*。 这些概念抽象自 Windows 开发人员中心仪表板中的大多数开发人员，并且大多数开发人员永远不需要为其应用或加载项定义 SKU 或可用性。 但是，由于 **Windows.Services.Store** 命名空间中的应用商店产品的对象模型包括 SKU 和可用性，因此大致了解这些概念可能很有帮助。

| 对象类型 |  说明  |
|---------|-------------------|
| [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)  |  此类表示在应用商店中提供的任何类型的产品，包括应用或加载项。 此类提供可用于访问数据的属性，如产品的应用商店 ID、应用商店列表的图像和视频以及定价信息。 它还提供可用于购买产品的方法。 |
| [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) |  此类表示产品的 *SKU*。 SKU 是带有其自己的说明、价格和其他独特产品详细信息的产品特定版本。 每个应用或加载项都有默认的 SKU。 大多数开发人员拥有针对一个应用的多个 SKU 的唯一情况是，他们要发布应用的完整版和试用版（在应用商店目录中，其中每一个版本都是同一个应用的不同 SKU）。 <p/><p/> 某些发布者能够定义他们自己的 SKU。 例如，大型游戏发布者可能发布具有以下两个 SKU 的游戏：一个 SKU 在不允许红色血液的市场中显示绿色血液，另一个 SKU 在所有其他市场中显示红色血液。 另外，销售数字视频内容的发布者可能针对一个视频发布两个 SKU，一个 SKU 用于高清版本，另一个 SKU 用于标清版本。 <p/><p/> 每个产品都有可用于访问 SKU 的 [Skus](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.skus.aspx) 属性。 |
| [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx)  |  此类表示 SKU 的*可用性*。 可用性是带有自己独特定价信息的 SKU 的特定版本。 每个 SKU 都有默认的可用性。 某些发布者能够定义他们自己的可用性来为给定 SKU 引入不同的价格选项。 <p/><p/> 每个 SKU 都有可用于访问可用性的 [Availabilities](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.availabilities.aspx) 属性。 对于大多数开发人员来说，每个 SKU 都有单个默认可用性。  |

<span id="store_ids" />
### <a name="store-ids"></a>应用商店 ID

应用商店中的每个应用和加载项都有关联的**应用商店 ID**。 **Windows.Services.Store** 命名空间中的许多 API 都需要应用商店 ID 才能执行有关应用或加载项的操作。 产品、SKU 和可用性具有不同的应用商店 ID 格式。

| 对象类型 |  应用商店 ID 格式  |
|---------|-------------------|
| [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)  |  应用商店中的任何产品的应用商店 ID 都是 12 个字符的字母数字字符串，例如 ```9NBLGGH4R315```。 此应用商店 ID 在应用或加载项的 Windows 开发人员中心仪表板中提供，由 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.storeid.aspx) 对象的 [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 属性返回。 此 ID 有时称为*产品应用商店 ID*。 |
| [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) |  对于 SKU，应用商店 ID 的格式为 ```<product Store ID>/xxxx```，其中 ```xxxx``` 是 4 个字符的字母数字字符串，用于标识产品的 SKU。 例如，```9NBLGGH4R315/000N```。 此 ID 由 [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) 对象的 [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.storeid.aspx) 属性返回，并且有时称为 *SKU 应用商店 ID*。 |
| [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx)  |  对于可用性，应用商店 ID 的格式为 ```<product Store ID>/xxxx/yyyyyyyyyyyy```，其中 ```xxxx``` 是标识产品 SKU 的 4 字符数字字母字符串，而 ```yyyyyyyyyyyy``` 是标识 SKU 可用性的 12 字符字母数字字符串。 例如，```9NBLGGH4R315/000N/4KW6QZD2VN6X```。 此 ID 由 [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) 对象的 [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.storeid.aspx) 属性返回，并且有时称为*可用性应用商店 ID*。  |

## <a name="related-topics"></a>相关主题

* [获取应用和加载项的产品信息](get-product-info-for-apps-and-add-ons.md)
* [获取应用和加载项的许可证信息](get-license-info-for-apps-and-add-ons.md)
* [支持应用内购买应用和加载项](enable-in-app-purchases-of-apps-and-add-ons.md)
* [支持购买易耗型加载项](enable-consumable-add-on-purchases.md)
* [实现应用的试用版](implement-a-trial-version-of-your-app.md)
* [使用 Windows.ApplicationModel.Store 命名空间的应用内购买和试用](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)

