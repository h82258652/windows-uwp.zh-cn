---
author: mcleanbyron
ms.assetid: 414ACC73-2A72-465C-BD15-1B51CB2334F2
title: 从 Microsoft Store 下载并安装程序包更新
description: 了解如何在开发人员中心仪表板中将程序包标记为必需，以及如何在应用中编写代码以下载并安装程序包更新。
ms.author: mcleans
ms.date: 04/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 39b3ba05b06b6d484804999a935accc7223b5c60
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "5437918"
---
# <a name="download-and-install-package-updates-from-the-store"></a>从 Microsoft Store 下载并安装程序包更新

从 Windows10 版本 1607 开始，你可以使用 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空间中 [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 类的方法，以编程方式从 Microsoft Store 检查当前应用的程序包更新，下载并安装更新的程序包。 还可以查询已在 Windows 开发人员中心仪表板上标记为必需的程序包，并在安装必需更新前禁用应用中的功能。

Windows 10 版本 1803 中引入的其他 [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 方法让你能够无提示（不向用户显示通知 UI）下载和安装程序包更新，卸载[可选包](optional-packages.md)和获取有关你的应用的下载和安装队列中的程序包相关信息。

这些功能有助于通过最新版本的应用、可选包和 Microsoft Store 中的相关服务，自动使你的用户群保持最新状态。

## <a name="download-and-install-package-updates-with-the-users-permission"></a>获得用户许可后下载和安装程序包更新

该代码示例演示如何使用 [GetAppAndOptionalStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync) 方法从 Microsoft Store 发现所有可用的程序包更新，然后调用 [RequestDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadandinstallstorepackageupdatesasync) 方法下载和安装更新。 使用该方法下载和安装更新时，操作系统在下载更新前会显示一个对话框，请求获得用户的许可。

> [!NOTE]
> 这些方法支持应用的必需包和[可选包](optional-packages.md)。 可选包可用于可下载内容 (DLC) 加载项，因为大小限制而划分大型应用，或者用于随附从核心应用中单独分隔出来的任何其他内容。 要获取将使用可选包（包括 DLC 加载项）的应用提交到 Microsoft Store 的权限，请参阅 [Windows 开发人员支持](https://developer.microsoft.com/windows/support)。

该代码示例假定：

* 代码在 [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) 的上下文中运行。
* **Page** 包含名为 ```downloadProgressBar``` 的 [ProgressBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressbar.aspx)，可提供下载操作的状态。
* 代码文件具有一个适用于 **Windows.Services.Store**、**Windows.Threading.Tasks** 和 **Windows.UI.Popups** 命名空间的 **using** 语句。
* 该应用是单用户应用，仅在启动该应用的用户上下文中运行。 对于[多用户应用](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications)，使用 [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) 方法（而不是 [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault) 方法）获取 **StoreContext** 对象。

```csharp
private StoreContext context = null;

public async Task DownloadAndInstallAllUpdatesAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> updates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        // Alert the user that updates are available and ask for their consent
        // to start the updates.
        MessageDialog dialog = new MessageDialog(
            "Download and install updates now? This may cause the application to exit.", "Download and Install?");
        dialog.Commands.Add(new UICommand("Yes"));
        dialog.Commands.Add(new UICommand("No"));
        IUICommand command = await dialog.ShowAsync();

        if (command.Label.Equals("Yes", StringComparison.CurrentCultureIgnoreCase))
        {
            // Download and install the updates.
            IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
                context.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

            // The Progress async method is called one time for each step in the download
            // and installation process for each package in this request.
            downloadOperation.Progress = async (asyncInfo, progress) =>
            {
                await this.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                () =>
                {
                    downloadProgressBar.Value = progress.PackageDownloadProgress;
                });
            };

            StorePackageUpdateResult result = await downloadOperation.AsTask();
        }
    }
}
```

> [!NOTE]
> 要只下载（但不安装）可用的程序包更新，请使用 [RequestDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadstorepackageupdatesasync) 方法。

### <a name="display-download-and-install-progress-info"></a>显示下载和安装进度信息

当你调用 [RequestDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadstorepackageupdatesasync) 或 [RequestDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadandinstallstorepackageupdatesasync) 方法时，你可以指定一个 [Progress](https://docs.microsoft.com/uwp/api/windows.foundation.iasyncoperationwithprogress-2.progress) 处理程序，系统会为此请求中每个程序包的下载（或下载和安装）过程中的每个步骤调用一次该程序。 此处理程序接收 [StorePackageUpdateStatus](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdatestatus) 对象，该对象提供有关发出进度通知的更新程序包的信息。 前面的示例使用 **StorePackageUpdateStatus** 对象的 **PackageDownloadProgress** 字段显示下载和安装过程的进度。

请注意，当你调用 **RequestDownloadAndInstallStorePackageUpdatesAsync** 在单一操作中下载和安装程序包更新时，**PackageDownloadProgress** 字段会在程序包下载过程中从 0.0 增加到 0.8，然后在安装过程中从 0.8 增加到 1.0。 因此，如果你将自定义进度 UI 中所显示的百分比直接映射到 **PackageDownloadProgress** 字段的值，则当程序包完成下载并且操作系统显示安装对话框时，你的 UI 将显示 80%。 如果你希望在程序包已下载并且可以安装时自定义进度 UI 显示 100%，则可以修改代码，以在 **PackageDownloadProgress** 字段达到 0.8 时将 100% 分配给你的进度 UI。

## <a name="download-and-install-package-updates-silently"></a>无提示下载和安装程序包更新

从 Windows 10 版本 1803 开始，你可以使用 [TrySilentDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadstorepackageupdatesasync) 和 [TrySilentDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadandinstallstorepackageupdatesasync) 方法以无提示方式下载和安装程序包更新 - 即不向用户显示通知 UI。 只有当用户在 Microsoft Store 中启用了**自动更新应用**设置并且用户使用的不是按流量计费的网络时，该操作才会成功。 在调用这些方法之前，你可以先检查一下 [CanSilentlyDownloadStorePackageUpdates](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.cansilentlydownloadstorepackageupdates) 属性，确定当前是否满足上述条件。

该代码示例演示如何使用 [GetAppAndOptionalStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync) 方法发现所有可用的程序包更新，然后调用 [TrySilentDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadstorepackageupdatesasync) 和 [TrySilentDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadandinstallstorepackageupdatesasync) 方法以无提示方式下载和安装更新。

该代码示例假定：
* 该代码文件需要使用 **using** 语句导入 **Windows.Services.Store** 和 **System.Threading.Tasks** 命名空间。
* 该应用是单用户应用，仅在启动该应用的用户上下文中运行。 对于[多用户应用](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications)，使用 [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) 方法（而不是 [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault) 方法）获取 **StoreContext** 对象。

> [!NOTE]
> 本示例中代码调用的 **IsNowAGoodTimeToRestartApp**、**RetryDownloadAndInstallLater** 和 **RetryInstallLater** 方法是占位符方法，你可以根据自己的应用设计按需实现它们。

```csharp
private StoreContext context = null;

public async Task DownloadAndInstallAllUpdatesInBackgroundAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> storePackageUpdates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (storePackageUpdates.Count > 0)
    {

        if (!context.CanSilentlyDownloadStorePackageUpdates)
        {
            return;
        }

        // Start the silent downloads and wait for the downloads to complete.
        StorePackageUpdateResult downloadResult =
            await context.TrySilentDownloadStorePackageUpdatesAsync(storePackageUpdates);

        switch (downloadResult.OverallState)
        {
            case StorePackageUpdateState.Completed:
                // The download has completed successfully. At this point, confirm whether your app
                // can restart now and then install the updates (for example, you might only install
                // packages silently if your app has been idle for a certain period of time). The
                // IsNowAGoodTimeToRestartApp method is not implemented in this example, you should
                // implement it as needed for your own app.
                if (IsNowAGoodTimeToRestartApp())
                {
                    await InstallUpdate(storePackageUpdates);
                }
                else
                {
                    // Retry/reschedule the installation later. The RetryInstallLater method is not  
                    // implemented in this example, you should implement it as needed for your own app.
                    RetryInstallLater();
                    return;
                }
                break;
            // If the user cancelled the download or you can't perform the download for some other
            // reason (for example, Wi-Fi might have been turned off and the device is now on
            // a metered network) try again later. The RetryDownloadAndInstallLater method is not  
            // implemented in this example, you should implement it as needed for your own app.
            case StorePackageUpdateState.Canceled:
            case StorePackageUpdateState.ErrorLowBattery:
            case StorePackageUpdateState.ErrorWiFiRecommended:
            case StorePackageUpdateState.ErrorWiFiRequired:
            case StorePackageUpdateState.OtherError:
                RetryDownloadAndInstallLater();
                return;
            default:
                break;
        }
    }
}

private async Task InstallUpdate(IReadOnlyList<StorePackageUpdate> storePackageUpdates)
{
    // Start the silent installation of the packages. Because the packages have already
    // been downloaded in the previous method, the following line of code just installs
    // the downloaded packages.
    StorePackageUpdateResult downloadResult =
        await context.TrySilentDownloadAndInstallStorePackageUpdatesAsync(storePackageUpdates);

    switch (downloadResult.OverallState)
    {
        // If the user cancelled the installation or you can't perform the installation  
        // for some other reason, try again later. The RetryInstallLater method is not  
        // implemented in this example, you should implement it as needed for your own app.
        case StorePackageUpdateState.Canceled:
        case StorePackageUpdateState.ErrorLowBattery:
        case StorePackageUpdateState.OtherError:
            RetryInstallLater();
            return;
        default:
            break;
    }
}
```

## <a name="mandatory-package-updates"></a>必需程序包更新

当为面向 Windows10 版本 1607 或更高版本的应用创建程序包提交时，可以[将该程序包标记为必需](../publish/upload-app-packages.md#mandatory-update)并标记变为必需的日期和时间。 当设置此属性，并且你的应用发现有程序包更新可用时，你的应用可以确定该更新包是否为必需，并在安装更新前更改其行为（例如你的应用可以禁用功能）。

> [!NOTE]
> Microsoft 不强制程序包更新处于必需状态，并且操作系统不提供向用户指示必须安装必需应用更新的 UI。 开发人员旨在使用必需设置通过其自己的代码强制执行必需的应用更新。  

若要将软件包提交标记为必需：

1. 登录到[开发人员中心仪表板](https://dev.windows.com/overview)并导航到你的应用的概述页面。
2. 单击包含要成为必需的程序包更新的提交名称。
3. 导航到提交的**程序包**页面。 在此页面底部附近，选择**使此更新为必需**，然后选择该程序包更新变为必需的日期和时间。 此选项适用于提交中的所有 UWP 程序包。

有关详细信息，请参阅[上传应用包](../publish/upload-app-packages.md)。

> [!NOTE]
> 如果创建[软件包外部测试版](../publish/package-flights.md)，可在外部测试版的**软件包**页面上使用类似 UI 将软件包标记为必需。 在此情况下，必需包更新仅适用于属于外部测试版组的客户。

### <a name="code-example-for-mandatory-packages"></a>必需程序包的代码示例

以下代码示例演示如何确定是否有任何更新包是必需的。 通常，如果无法成功下载或安装必需程序包更新，应顺利降级用户的应用体验。

```csharp
private StoreContext context = null;

// Downloads and installs package updates in separate steps.
public async Task DownloadAndInstallAllUpdatesAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }  

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> updates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count != 0)
    {
        // Download the packages.
        bool downloaded = await DownloadPackageUpdatesAsync(updates);

        if (downloaded)
        {
            // Install the packages.
            await InstallPackageUpdatesAsync(updates);
        }
    }
}

// Helper method for downloading package updates.
private async Task<bool> DownloadPackageUpdatesAsync(IEnumerable<StorePackageUpdate> updates)
{
    bool downloadedSuccessfully = false;

    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
        this.context.RequestDownloadStorePackageUpdatesAsync(updates);

    // The Progress async method is called one time for each step in the download process for each
    // package in this request.
    downloadOperation.Progress = async (asyncInfo, progress) =>
    {
        await this.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () =>
        {
            downloadProgressBar.Value = progress.PackageDownloadProgress;
        });
    };

    StorePackageUpdateResult result = await downloadOperation.AsTask();

    switch (result.OverallState)
    {
        case StorePackageUpdateState.Completed:
            downloadedSuccessfully = true;
            break;
        default:
            // Get the failed updates.
            var failedUpdates = result.StorePackageUpdateStatuses.Where(
                status => status.PackageUpdateState != StorePackageUpdateState.Completed);

            // See if any failed updates were mandatory
            if (updates.Any(u => u.Mandatory && failedUpdates.Any(
                failed => failed.PackageFamilyName == u.Package.Id.FamilyName)))
            {
                // At least one of the updates is mandatory. Perform whatever actions you
                // want to take for your app: for example, notify the user and disable
                // features in your app.
                HandleMandatoryPackageError();
            }
            break;
    }

    return downloadedSuccessfully;
}

// Helper method for installing package updates.
private async Task InstallPackageUpdatesAsync(IEnumerable<StorePackageUpdate> updates)
{
    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> installOperation =
        this.context.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

    // The package updates were already downloaded separately, so this method skips the download
    // operatation and only installs the updates; no download progress notifications are provided.
    StorePackageUpdateResult result = await installOperation.AsTask();

    switch (result.OverallState)
    {
        case StorePackageUpdateState.Completed:
            break;
        default:
            // Get the failed updates.
            var failedUpdates = result.StorePackageUpdateStatuses.Where(
                status => status.PackageUpdateState != StorePackageUpdateState.Completed);

            // See if any failed updates were mandatory
            if (updates.Any(u => u.Mandatory && failedUpdates.Any(failed => failed.PackageFamilyName == u.Package.Id.FamilyName)))
            {
                // At least one of the updates is mandatory, so tell the user.
                HandleMandatoryPackageError();
            }
            break;
    }
}

// Helper method for handling the scenario where a mandatory package update fails to
// download or install. Add code to this method to perform whatever actions you want
// to take, such as notifying the user and disabling features in your app.
private void HandleMandatoryPackageError()
{
}
```

## <a name="uninstall-optional-packages"></a>卸载可选包

从 Windows 10 版本 1803 开始，你可以使用 [RequestUninstallStorePackageAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestuninstallstorepackageasync) 或 [RequestUninstallStorePackageByStoreIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestuninstallstorepackagebystoreidasync) 方法卸载当前应用的[可选包](optional-packages.md)（包括 DLC 包）。 例如，如果你的应用包含通过可选包安装的内容，则你可能需要提供 UI，以便用户可以卸载可选包来释放磁盘空间。

以下代码示例演示如何调用 [RequestUninstallStorePackageAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestuninstallstorepackageasync)。 此示例假定：
* 该代码文件需要使用 **using** 语句导入 **Windows.Services.Store** 和 **System.Threading.Tasks** 命名空间。
* 该应用是单用户应用，仅在启动该应用的用户上下文中运行。 对于[多用户应用](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications)，使用 [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) 方法（而不是 [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault) 方法）获取 **StoreContext** 对象。

```csharp
public async Task UninstallPackage(Windows.ApplicationModel.Package package)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    var storeContext = StoreContext.GetDefault();
    IAsyncOperation<StoreUninstallStorePackageResult> uninstallOperation =
        storeContext.RequestUninstallStorePackageAsync(package);

    // At this point, you can update your app UI to show that the package
    // is installing.

    uninstallOperation.Completed += (asyncInfo, status) =>
    {
        StoreUninstallStorePackageResult result = uninstallOperation.GetResults();
        switch (result.Status)
        {
            case StoreUninstallStorePackageStatus.Succeeded:
                {
                    // Update your app UI to show the package as uninstalled.
                    break;
                }
            default:
                {
                    // Update your app UI to show that the package uninstall failed.
                    break;
                }
        }
    };
}
```

## <a name="get-download-queue-info"></a>获取下载队列信息

从 Windows 10 版本 1803 开始，你可以使用 [GetAssociatedStoreQueueItemsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstorequeueitemsasync) 和 [GetStoreQueueItemsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstorequeueitemsasync) 方法从 Microsoft Store 获取当前下载和安装队列中的包的相关信息。 如果你的应用或游戏支持很大的可选包（包括 DLC），这些包可能需要几小时或几天的时间下载和安装，并且你希望适当地处理客户在下载和安装过程完成前关闭你的应用或游戏的情况，则这些方法非常有用。 当客户再次启动你的应用或游戏时，你的代码可以使用这些方法获取仍在下载和安装队列中的包的状态信息，以便向客户显示每个包的状态。

以下代码示例演示如何调用 [GetAssociatedStoreQueueItemsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstorequeueitemsasync) 来获取正在为当前应用处理的程序包更新并检索每个包的状态信息。 此示例假定：
* 该代码文件需要使用 **using** 语句导入 **Windows.Services.Store** 和 **System.Threading.Tasks** 命名空间。
* 该应用是单用户应用，仅在启动该应用的用户上下文中运行。 对于[多用户应用](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications)，使用 [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) 方法（而不是 [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault) 方法）获取 **StoreContext** 对象。

> [!NOTE]
> 本示例中代码所调用的 **MarkUpdateInProgressInUI**、**RemoveItemFromUI**、**MarkInstallCompleteInUI**、**MarkInstallErrorInUI** 和 **MarkInstallPausedInUI** 方法是占位符方法，你可以根据自己的应用设计按需实现它们。

```csharp
private StoreContext context = null;

private async Task GetQueuedInstallItemsAndBuildInitialStoreUI()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the Store packages in the install queue.
    IReadOnlyList<StoreQueueItem> storeUpdateItems = await context.GetAssociatedStoreQueueItemsAsync();

    foreach (StoreQueueItem storeItem in storeUpdateItems)
    {
        // In this example we only care about package updates.
        if (storeItem.InstallKind != StoreQueueItemKind.Update)
            continue;

        StoreQueueItemStatus currentStatus = storeItem.GetCurrentStatus();
        StoreQueueItemState installState = currentStatus.PackageInstallState;
        StoreQueueItemExtendedState extendedInstallState =
            currentStatus.PackageInstallExtendedState;

        // Handle the StatusChanged event to display current status to the customer.
        storeItem.StatusChanged += StoreItem_StatusChanged;

        switch (installState)
        {
            // Download and install are still in progress, so update the status for this  
            // item and provide the extended state info. The following methods are not
            // implemented in this example; you should implement them as needed for your
            // app's UI.
            case StoreQueueItemState.Active:
                MarkUpdateInProgressInUI(storeItem, extendedInstallState);
                break;
            case StoreQueueItemState.Canceled:
                RemoveItemFromUI(storeItem);
                break;
            case StoreQueueItemState.Completed:
                MarkInstallCompleteInUI(storeItem);
                break;
            case StoreQueueItemState.Error:
                MarkInstallErrorInUI(storeItem);
                break;
            case StoreQueueItemState.Paused:
                MarkInstallPausedInUI(storeItem, installState, extendedInstallState);
                break;
        }
    }
}

private void StoreItem_StatusChanged(StoreQueueItem sender, object args)
{
    StoreQueueItemStatus currentStatus = sender.GetCurrentStatus();
    StoreQueueItemState installState = currentStatus.PackageInstallState;
    StoreQueueItemExtendedState extendedInstallState = currentStatus.PackageInstallExtendedState;

    switch (installState)
    {
        // Download and install are still in progress, so update the status for this  
        // item and provide the extended state info. The following methods are not
        // implemented in this example; you should implement them as needed for your
        // app's UI.
        case StoreQueueItemState.Active:
            MarkUpdateInProgressInUI(sender, extendedInstallState);
            break;
        case StoreQueueItemState.Canceled:
            RemoveItemFromUI(sender);
            break;
        case StoreQueueItemState.Completed:
            MarkInstallCompleteInUI(sender);
            break;
        case StoreQueueItemState.Error:
            MarkInstallErrorInUI(sender);
            break;
        case StoreQueueItemState.Paused:
            MarkInstallPausedInUI(sender, installState, extendedInstallState);
            break;
    }
}
```

## <a name="related-topics"></a>相关主题

* [可选包和相关集创作](optional-packages.md)
