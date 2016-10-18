---
author: mcleanbyron
ms.assetid: 571697B7-6064-4C50-9A68-1374F2C3F931
description: "了解如何使用 Windows.Services.Store 命名空间实现应用的试用版。"
title: "实现应用的试用版"
keywords: "免费试用代码示例"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 22f355c23f4cc87932563e9885f390e9a5ac4130

---

# 实现应用的试用版

如果允许客户在试用期内免费使用你的应用，则可以通过排除或限制试用期内的某些功能，吸引客户升级到完整版应用。 请在开始编码之前确定哪些功能应受到限制，然后确保你的应用只在已购买完整版许可之后才允许这些功能运作。 也可以在客户购买你的应用之前，启用仅在试用期才会出现的某些功能，如横幅或水印。

面向 Windows 10 版本 1607 或更高版本的应用可以使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空间中 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 类的成员，确定用户是否有应用的试用许可证，并在应用运行时许可证的状态发生更改的情况下获得通知。

>**注意**&nbsp;&nbsp;本文适用于面向 Windows 10 版本 1607 或更高版本的应用。 如果你的应用面向 Windows 10 的较早版本，则必须使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空间来替代 **Windows.Services.Store** 命名空间。 有关详细信息，请参阅[使用 Windows.ApplicationModel.Store 命名空间进行应用内购买和试用](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)。

## 实现试用版的指南

应用的当前许可证状态存储为 [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) 类的属性。 通常，将取决于许可证状态的功能放在我们在下一步介绍的条件块中。 在考虑这些功能时，确保实现该功能的方式允许这些功能在所有许可证状态下均能正常工作。

另外，决定你希望在应用运行时如何处理对应用许可证的更改。 你的试用版可以是全功能的，但具有付费版所没有的应用内广告横幅。 或者，你的试用应用可以禁用某些功能，或定期显示消息要求用户购买应用。

考虑你正设计的应用类型，什么是适合它的试用或到期策略。 对于试用版的游戏，一个好的策略是限制用户可以玩的游戏内容量。 对于试用版的实用工具，可能需要考虑设置一个到期日期，或限制潜在购买者可以使用的功能。

对于大部分非游戏应用，设置一个过期日期很有用，因为用户可很好地理解整个应用。 以下是一些常见的过期场景和处理它们的选项。

-   **试用许可证在应用正在运行时过期。**

    如果应用正在运行时试用许可证过期，应用可以：

    -   不执行任何操作。
    -   向客户显示一条消息。
    -   关闭。
    -   提示客户购买应用。

    最佳做法是显示一条消息，提示客户购买应用，如果客户购买它，则继续启用所有功能。 如果用户决定不购买应用，则关闭它或定期提醒他们购买应用。

-   **在应用启动前试用许可证过期。**

    如果在应用启动前试用许可证过期，应用将不会启动。 相反，用户将看到一个对话框，该对话框为用户提供从应用商店购买你的应用的选项。

-   **客户在应用运行时购买它**

    如果客户在应用运行时购买它，以下是应用可执行的一些操作。

    -   不执行任何操作，让他们继续在试用模式下操作，直到重新启动应用。
    -   感谢他们购买，或者显示一条消息。
    -   静默启用在完整许可证下可用的功能（或禁用仅限试用的通知）。

请务必阐述你的应用在免费试用期间及之后的行为，以便客户不会对应用行为感到惊讶。 有关描述应用的详细信息，请参阅[创建应用提要](https://msdn.microsoft.com/library/windows/apps/mt148529)。

## 先决条件

本示例有以下先决条件：
* 适用于面向 Windows 10 版本 1607 或更高版本的通用 Windows 平台 (UWP) 应用的 Visual Studio 项目。
* 你已在 Windows 开发人员中心仪表板中创建了一个应用（已配置为[免费试用](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability)，没有时间限制），并且该应用已发布，可在应用商店中上架。 该应用可以是你想要发布给客户的应用，也可以是符合 [Windows 应用认证工具包](https://developer.microsoft.com/windows/develop/app-certification-kit)最低要求、仅用于测试目的的基本应用。 有关详细信息，请参阅[测试指南](in-app-purchases-and-trials.md#testing)。

此示例中的代码假设：
* 代码在含有 [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx)（名为 ```workingProgressRing```）和 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)（名为 ```textBlock```）的 [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) 上下文中运行。 这些对象分别用于指示是否正在进行异步操作和显示输出消息。
* 代码文件有一个适用于 **Windows.Services.Store** 命名空间的 **using** 语句。
* 该应用是单用户应用，仅在启动该应用的用户上下文中运行。 有关详细信息，请参阅[应用内购买和试用](in-app-purchases-and-trials.md#api_intro)。

## 代码示例

初始化你的应用时，为你的应用获取 [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) 对象并处理 [OfflineLicensesChanged](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.offlinelicenseschanged.aspx) 事件，以在应用运行时许可证发生更改的情况下收到通知。 例如，如果试用期到期或客户通过应用商店购买应用，应用的许可证将发生更改。 当许可证发生更改时，获取新的许可证并相应地启用或禁用应用的功能。

此时，如果用户购买了应用，则向用户提供许可状态已发生更改的反馈是一个好做法。 你可能需要请求用户重新启动应用（如果你已经这样编码）。 但是一定要让这一过渡尽可能无缝和轻松地进行。


```csharp
private StoreContext context = null;
private StoreAppLicense appLicense = null;

// Call this while your app is initializing.
private async void InitializeLicense()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    workingProgressRing.IsActive = true;
    appLicense = await context.GetAppLicenseAsync();
    workingProgressRing.IsActive = false;

    // Register for the licenced changed event.
    context.OfflineLicensesChanged += context_OfflineLicensesChanged;
}

private async void context_OfflineLicensesChanged(StoreContext sender, object args)
{
    // Reload the license.
    workingProgressRing.IsActive = true;
    appLicense = await context.GetAppLicenseAsync();
    workingProgressRing.IsActive = false;

    if (appLicense.IsActive)
    {
        if (appLicense.IsTrial)
        {
            textBlock.Text = $"This is the trial version. Expiration date: {appLicense.ExpirationDate}";

            // Show the features that are available during trial only.
        }
        else
        {
            // Show the features that are available only with a full license.
        }
    }
}
```

有关完整的示例应用程序，请参阅[应用商店示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)。

## 相关主题

* [应用内购买和试用](in-app-purchases-and-trials.md)
* [获取应用和加载项的产品信息](get-product-info-for-apps-and-add-ons.md)
* [获取应用和加载项的许可证信息](get-license-info-for-apps-and-add-ons.md)
* [支持应用内购买应用和加载项](enable-in-app-purchases-of-apps-and-add-ons.md)
* [支持购买易耗型加载项](enable-consumable-add-on-purchases.md)
* [应用商店示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Aug16_HO5-->


