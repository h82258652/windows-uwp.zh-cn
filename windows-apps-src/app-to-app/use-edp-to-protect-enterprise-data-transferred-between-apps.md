---
Description: '本主题演示了实现一些与数据传输相关的最常见企业数据保护 EDP 方案所需的编码任务示例。'
MS-HAID: 'dev\_app\_to\_app.use\_edp\_to\_protect\_enterprise\_data\_transferred\_between\_apps'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: 使用 EDP 保护在应用之间传输的企业数据
---

# 使用 EDP 保护在应用之间传输的企业数据

__注意__ 在版本 1511（版本 10586）或更早版本的 Windows 10 上无法应用企业数据保护 (EDP) 策略。

本主题演示了实现一些与数据传输相关的最常见企业数据保护 EDP 方案所需的编码任务示例。 有关 EDP 如何关联到文件、数据流、剪贴板、网络、后台任务以及锁屏下的数据保护的完整开发人员蓝图，请参阅[企业数据保护 (EDP)](../enterprise/edp-hub.md)。

**注意** [企业数据保护 (EDP) 示例](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)涵盖本主题中展示的许多相同方案。

## 先决条件


-   **完成 EDP 设置**

    请参阅[设置用于 EDP 的计算机](../enterprise/edp-hub.md#set-up-your-computer-for-EDP)。

-   **致力于生成企业启发式应用**

    自发确保企业数据处于管理企业控制之下的应用称为企业启发式应用。 启发式应用比未启发式应用更强大、更智能、更灵活，也更受信赖。 你的应用可以通过声明受限的 **enterpriseDataPolicy** 功能向系统声明它为启发式应用。 但要实现启发式的应用比要设置功能的应用多。 若要了解详细信息，请参阅[企业启发式应用](../enterprise/edp-hub.md#enterprise-enlightened-apps)。

-   **了解通用 Windows 平台 (UWP) 应用的异步编程**

    若要了解如何使用 C\# 或 Visual Basic 编写异步应用，请参阅[使用 C\# 或 Visual Basic 调用异步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)。 若要了解如何使用 C++ 编写异步应用，请参阅[使用 C++ 进行异步编程](https://msdn.microsoft.com/library/windows/apps/mt187334)。

## 简单剪贴板源


在此方案中，你的应用是处理个人和企业文件的记事本类型应用。 此时，你的应用完全无需更改其复制粘贴逻辑；每当用户打开并显示企业文档中的内容时，只需调用 [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791) 即可。 当内容显示在应用的 UI 中时，用户随后可能将其复制并粘贴到另一个应用，这便是设置 UI 策略非常重要的原因。 在这种情况下，操作系统可以将当前设置的策略应用于涉及受保护数据的粘贴操作。 同样，当不再需要 UI 策略时将其清除也非常重要，以便用户可以再次随意复制粘贴个人数据。 如果 **TryApplyProcessUIPolicy** 的标识参数未由企业策略管理，它将返回 False。

```CSharp
using Windows.Security.EnterpriseData;

...

private void OnFileLoaded(FileProtectionInfo fileProtectionInfo, string contentsOfFile)
{
    if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy
            (fileProtectionInfo.Identity);
        if (isIdentityManaged)
        {
            // UI policy is now in effect for the file's identity.
        }
        else
        {
            // Enterprise policy is not in effect, because the file&#39;s identity
            // is not managed. In this case, we have a file protected to an
            // unmanaged identity, which is not a valid situation.
            // We still have to call ClearProcessUIPolicy if we want to clear the policy.
            ProtectionPolicyManager.ClearProcessUIPolicy();
        }
    }
    else
    {
        // In case we applied the policy for the previous file, now we need to clear it.
        ProtectionPolicyManager.ClearProcessUIPolicy();
    }
    // Code goes here to display "contentsOfFile" in your UI. Ready for copy-paste...
}
```

## 简单剪贴板目标


在此方案中，你的应用是处理个人和企业帐户的电子邮件应用。 用户尝试使用其个人帐户将数据粘贴到电子邮件答复中。 在此情况下，你的应用只需在检索内容之前调用 [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636) 即可。 如果我们已经具有访问权限，该方法将立即返回。 但如果我们没有访问权限，该方法将显示一个对话框，请求用户同意并在取得同意后“解锁”数据包。 这是为用户提供机会来取消失误的粘贴操作。

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private async void OnPasteSimple()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        // In case we don't already have acccess, request consent from the user
        // for us to access the clipboard data.
        await dataPackageView.RequestAccessAsync();

        try
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            // Code goes here to insert the text into the email...
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the clipboard.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

## 剪贴板目标是中性的空白文档


在此方案中，你的应用是一个文字处理应用。 创建新文档后，只要文档保留为空，你的应用便将该文档视为中性，既非企业也非个人。 用户随后将企业数据粘贴到此中性上下文文档。 由于文档处于中性上下文中，我们实际上只需将文档切换到企业上下文并且避免完全调用 [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636) 即可（因为本例中不需要显示对话框）。 因此，如果数据受到保护，我们则只需切换到企业上下文然后粘贴数据即可。

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private async void OnPasteWithApplyPolicy()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        // If the data has no enterprise identity, then we already have access.
        if (!string.IsNullOrEmpty(dataPackageView.Properties.EnterpriseId))
        {
            ProtectionPolicyEvaluationResult policyResult =
                await dataPackageView.RequestAccessAsync(dataPackageView.Properties.EnterpriseId);
            if (this.isNewEmptyDocument &amp;&amp;
                policyResult == ProtectionPolicyEvaluationResult.Allowed)
            {
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog.
                bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy(dataPackageView.Properties.EnterpriseId);
                // You can use "isIdentityManaged", but it's not necessary.
            }
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.
                await dataPackageView.RequestAccessAsync();
            }
        }

        try
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            // Code goes here to insert the text into the email...
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the clipboard.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

## 带有显式企业标识的剪贴板源


在此方案中，你的应用是一个文字处理应用。 它使用后台线程提交用户执行的复制操作。 用户从企业文件中复制一些数据，然后立即切换到个人文件，此时应用的全局上下文成为个人上下文。 此时（由于全局状态已更改并且不应使用），应用的后台线程必须明确告知剪贴板传入数据为企业数据。 可通过设置 [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861) 属性来完成此操作。

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private void CopyToClipboard(string stringToCopy, string identity)
{
    // Copy the string to the clipboard.
    var dataPackage = new DataPackage();
    dataPackage.SetText(stringToCopy);
    dataPackage.Properties.EnterpriseId = identity;
    Clipboard.SetContent(dataPackage);
}
```

## 使用企业标识标记特定窗口


在此方案中，你的应用是一个在不同窗口中处理多个文档的文字处理应用，某些窗口为企业窗口而其余为个人窗口。 应用要确保任何要粘贴到个人文档窗口的数据都需进行正确审查（如果是企业数据，即拒绝或同意）并且任何从企业文档窗口传出的数据都要正确标记。 可通过设置 [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) 属性来完成此操作。

```CSharp
using Windows.Security.EnterpriseData;

...

private void TagCurrentViewWithEnterpriseId(string identity)
{
    ProtectionPolicyManager.GetForCurrentView().Identity = identity;
}
```

## 使用企业标识标记传出拖动对象


你的应用具有一个通过某些可拖动企业内容打开的个人窗口。 用户开始拖动这样的一些内容，而你的应用需要确保这些内容标记为企业。 此方案未涉及任何新的 API。 对于此方案，你的应用将设置 [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861) 属性（请参阅上述[带有显式企业标识的剪贴板源方案](#clipboard_source_explicit_id)）。

## 查询已接收拖动对象的企业标识


你的应用具有一个打开的新空白文档窗口，只要它为空便假定为中性，然后用户将某些内容拖放到该文档。 应用现在必须确定对象的企业标识才能相应更改文档的状态。 对于此方案，你的应用将通过读取 [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861) 从数据包获取 **EnterpriseId** 属性（请参阅上述[带有显式企业标识的剪贴板源方案](#clipboard_source_explicit_id)）。

## 你的应用作为“共享”合约源


当你的应用支持“共享”合约时，若要设置共享源，请在 [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/br205873) 中设置企业标识上下文，如此代码示例所示。

**注意** 此代码示例取决于你是否已为当前视图的保护策略管理器对象设置标识（请参阅[使用企业标识标记特定窗口](#tag_window_with_id)），否则 [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) 属性将包含空字符串。



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private void OnShareSourceOperation(object sender, RoutedEventArgs e)
{
    // Register the current page as a share source (or you could do this earlier in your app).
    DataTransferManager.GetForCurrentView().DataRequested += OnDataRequested;
    DataTransferManager.ShowShareUI();
}

private void OnDataRequested(DataTransferManager sender, DataRequestedEventArgs args)
{
    if (!string.IsNullOrEmpty(this.shareSourceContent))
    {
        var protectionPolicyManager = ProtectionPolicyManager.GetForCurrentView();
        DataPackage requestData = args.Request.Data;
        requestData.Properties.Title = this.shareSourceTitle;
        requestData.Properties.EnterpriseId = protectionPolicyManager.Identity;
        requestData.SetText(this.shareSourceContent);
    }
}
```

## 你的应用作为“共享”合约目标


只要我们使用的文件为空，此代码示例便继续使用我们的策略。 随后，我们可根据传入数据的情况随意切换上下文，并且尽可能避免弹出同意 UI。 因此，当应用将数据作为“共享”合约目标接收时，如果没有现有内容，它应调用 [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791)，否则应调用 [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636)。 代码示例演示了执行方式。

```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.ApplicationModel.DataTransfer.ShareTarget;

...

string identity = "microsoft.com";

protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    ShareOperation shareOperation = args.ShareOperation;
    if (shareOperation.Data.Contains(StandardDataFormats.Text))
    {
        // If the data has no enterprise identity, then we already have access.
        if (!string.IsNullOrEmpty(shareOperation.Data.Properties.EnterpriseId))
        {
            ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult =
                await ProtectionPolicyManager.RequestAccessAsync
                    (shareOperation.Data.Properties.EnterpriseId, identity);
            if (this.isNewEmptyDocument && protectionPolicyEvaluationResult ==
                ProtectionPolicyEvaluationResult.Allowed)
            {
                // If this is a new and empty document, and we&#39;re allowed to access
                // the data, then we can avoid popping the consent dialog.
                bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy
                    (shareOperation.Data.Properties.EnterpriseId);
                // You can use "isIdentityManaged", but it';s not necessary.
            }
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.
                protectionPolicyEvaluationResult = await shareOperation.Data.RequestAccessAsync();
            }
        }

        try
        {
            // Get the text from the share operation...
            App.shareTargetContent = await shareOperation.Data.GetTextAsync();
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the share operation.
        }
    }
    else
    {
        // The value in the share operation is not in the text format.
    }
}
```

## 被动查询复制粘贴策略


在此方案中，你的应用仅在数据位于剪贴板时才启用粘贴 UI。 对于此功能，你可以使用 [**ProtectionPolicyManager.CheckAccess**](https://msdn.microsoft.com/library/windows/apps/dn705783) 方法，以允许被动检查策略。

**注意** 此代码示例取决于你是否已为当前视图的保护策略管理器对象设置标识（请参阅[使用企业标识标记特定窗口](#tag_window_with_id)），否则 [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) 属性将包含空字符串。



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private bool IsClipboardPeekAllowedAsync()
{
    ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = ProtectionPolicyEvaluationResult.Blocked;
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);
    }

    // Since this is a convenience feature to allow a peek of clipboard content,
    // if state is Blocked or ConsentRequired, do not show peek if this helper
    // method returns false.
    return (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed);
}
```

## 请求复制粘贴操作的访问权限


此方案演示了如何检查粘贴操作的访问权限。

**注意** 此代码示例取决于你是否已为当前视图的保护策略管理器对象设置标识（请参阅[使用企业标识标记特定窗口](#tag_window_with_id)），否则 [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) 属性将包含空字符串。



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private async void OnPasteWithRequestAccess()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);

        if (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed)
        {
            try
            {
                string contentsOfClipboard = await dataPackageView.GetTextAsync();
                // Code goes here to insert the text into the app...
                this.fileContentsTextBox.Text = contentsOfClipboard;
            }
            catch (Exception)
            {
                // Code goes here to handle the exception retrieving text from the clipboard.
            }
        }
        else
        {
            // Paste from the enterprise context is not allowed by policy.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

**注意** 本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你面向 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。



## 相关主题


[企业数据保护 (EDP) 示例](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Windows.Security.EnterpriseData 命名空间**](https://msdn.microsoft.com/library/windows/apps/dn279153)







<!--HONumber=Mar16_HO5-->


