---
description: 本教程演示如何添加 UWP XAML 用户界面、 创建 MSIX 包和其他新式组件合并到 WPF 应用。
title: 添加 Windows 10 用户活动和通知
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、 uwp、 windows 窗体、 wpf、 xaml 群岛
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 8443ac25ba678986046b967a90a8899eaffb76aa
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420117"
---
# <a name="part-4-add-windows-10-user-activities-and-notifications"></a>第 4 部分：添加 Windows 10 用户活动和通知

这是本教程演示如何实现名为 Contoso 费用的示例 WPF 桌面应用的现代化的第四个部分。 教程、 先决条件和说明下载示例应用程序的概述，请参阅[教程：使 WPF 应用现代化](modernize-wpf-tutorial.md)。 本文假定你已完成[第 3 部分](modernize-wpf-tutorial-3.md)。

在本教程前面部分中添加 UWP XAML 控件添加到使用 XAML 岛的应用。 为此产品，还启用应用程序调用的任何 WinRT API。 这会打开应用程序以使用提供的 Windows 10，而不仅仅是 UWP XAML 控件的很多其他功能的机会。

在本教程的虚构方案中，Contoso 开发团队已决定将两个新功能添加到应用： 活动和通知。 在本教程的此部分演示如何实现这些功能。

## <a name="add-a-user-activity"></a>添加用户活动

在 Windows 10 中，应用可以跟踪如打开的文件或显示的特定页面的用户执行的活动。 然后，这些活动是可通过时间线，Windows 10 版本 1803 中, 引入的功能这样用户就可以快速返回到过去并恢复它们先前开始的活动。

![Windows 时间线图像](images/wpf-modernize-tutorial/WindowsTimeline.png)

使用跟踪用户活动[Microsoft Graph](https://developer.microsoft.com/graph/)。 但是，当您正在构建的 Windows 10 应用，你不需要直接与 Microsoft Graph 提供的 REST 终结点进行交互。 相反，您可以使用一组方便的 WinRT Api。 我们将在 Contoso Expenses 应用程序中使用这些 WinRT Api 来跟踪每次用户打开应用内的费用并使用自适应卡使用户能够创建活动。

### <a name="introduction-to-adaptive-cards"></a>自适应卡简介

本部分提供的简要概述[自适应卡](https://docs.microsoft.com/adaptive-cards/)。 如果不需要此信息，则可以跳过此部分，并从右到转[添加自适应卡](#add-an-adaptive-card)说明。

自适应卡使开发人员能够按常见且一致的方式交换卡内容。 自适应卡由 JSON 有效负载，用于定义其内容，可以包括文本、 图像、 操作和详细描述。

自适应卡定义的内容和不可视外观的内容。 从中接收自适应卡平台可以呈现使用最合适的样式设置的内容。 自适应卡的设计的方式是通过[呈现器](https://docs.microsoft.com/adaptive-cards/rendering-cards/getting-started)，这是能够在需要 JSON 有效负载，并将其转换为本机 UI。 例如，UI 可能对于 WPF 或 UWP 应用，AXML Android 应用程序或 HTML 的网站或机器人聊天是 XAML。

下面是简单的自适应卡有效负载的示例。

```json
{
    "type": "AdaptiveCard",
    "body": [
        {
            "type": "Container",
            "items": [
                {
                    "type": "TextBlock",
                    "size": "Medium",
                    "weight": "Bolder",
                    "text": "Publish Adaptive Card schema"
                },
                {
                    "type": "ColumnSet",
                    "columns": [
                        {
                            "type": "Column",
                            "items": [
                                {
                                    "type": "Image",
                                    "style": "Person",
                                    "url": "https://pbs.twimg.com/profile_images/3647943215/d7f12830b3c17a5a9e4afcc370e3a37e_400x400.jpeg",
                                    "size": "Small"
                                }
                            ],
                            "width": "auto"
                        },
                        {
                            "type": "Column",
                            "items": [
                                {
                                    "type": "TextBlock",
                                    "weight": "Bolder",
                                    "text": "Matt Hidinger",
                                    "wrap": true
                                },
                                {
                                    "type": "TextBlock",
                                    "spacing": "None",
                                    "text": "Created {{DATE(2017-02-14T06:08:39Z,SHORT)}}",
                                    "isSubtle": true,
                                    "wrap": true
                                }
                            ],
                            "width": "stretch"
                        }
                    ]
                }
            ]
        }
    ],
    "actions": [
        {
            "type": "Action.ShowCard",
            "title": "Set due date",
            "card": {
                "type": "AdaptiveCard",
                "style": "emphasis",
                "body": [
                    {
                        "type": "Input.Date",
                        "id": "dueDate"
                    },
                    {
                        "type": "Input.Text",
                        "id": "comment",
                        "placeholder": "Add a comment",
                        "isMultiline": true
                    }
                ],
                "actions": [
                    {
                        "type": "Action.OpenUrl",
                        "title": "OK",
                        "url": "http://adaptivecards.io"
                    }
                ],
                "$schema": "http://adaptivecards.io/schemas/adaptive-card.json"
            }
        },
        {
            "type": "Action.OpenUrl",
            "title": "View",
            "url": "http://adaptivecards.io"
        }
    ],
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.0"
}
```

下图显示了此 JSON 的呈现方式由不同的方式 ta 团队通道、 Cortana 和 Windows 通知。

![自适应卡呈现图像](images/wpf-modernize-tutorial/AdaptiveCards.png)

自适应卡在时间线中发挥重要作用，因为它是 Windows 中的活动的呈现的方式。 显示在时间线内每个缩略图是实际上自适应卡。 在这种情况下，当您将创建您的应用程序内的用户活动，您将需要提供自适应卡来呈现它。

> [!NOTE]
> 使用集体讨论的自适应卡设计的好办法[online 设计器](https://adaptivecards.io/designer/)。 将有机会设计构建基块 （图像、 文本、 列等） 的数据卡和获取相应的 JSON。 有了大致最终设计后，可以使用一个名为库[自适应卡](https://www.nuget.org/packages/AdaptiveCards/)若要使其更轻松地生成您的自适应卡使用C#而不是纯 JSON，这可能会很难进行调试和生成的类。

### <a name="add-an-adaptive-card"></a>添加自适应卡

1. 右键单击**ContosoExpenses.Core**项目在解决方案资源管理器中，选择**管理 NuGet 包**。

2. 在中**NuGet 包管理器**窗口中，单击**浏览**。 搜索`Newtonsoft.Json`包并安装最新的可用版本。 这是一个受欢迎的 JSON 操作库将用于帮助的 mainipulate JSON 字符串所需的自适应卡。

    ![NewtonSoft.Json NuGet 包](images/wpf-modernize-tutorial/JsonNetNuGet.png)

    > [!NOTE]
    > 如果你不安装`Newtonsoft.Json`单独打包、 自适应卡库将引用较旧版本的`Newtonsoft.Json`不支持.NET Core 3.0 的包。

2. 在中**NuGet 包管理器**窗口中，单击**浏览**。 搜索`AdaptiveCards`包并安装最新的可用版本。

    ![自适应卡 NuGet 包](images/wpf-modernize-tutorial/AdaptiveCardsNuGet.png)

3. 在中**解决方案资源管理器**，右键单击**ContosoExpenses.Core**项目中，选择**添加-> 类**。 将类命名**TimelineService.cs**然后单击**确定**。

4. 在中**TimelineService.cs**文件中，将以下语句添加到文件顶部。

    ```csharp
    using AdaptiveCards;
    using ContosoExpenses.Data.Models;
    ```

5. 从文件中声明的命名空间的更改`ContosoExpenses.Core`到`ContosoExpenses`。

5. 添加以下方法`TimelineService`类。

   ```csharp
    private string BuildAdaptiveCard(Expense expense)
    {
        AdaptiveCard card = new AdaptiveCard("1.0");

        AdaptiveTextBlock title = new AdaptiveTextBlock
        {
            Text = expense.Description,
            Size = AdaptiveTextSize.Medium,
            Wrap = true
        };

        AdaptiveColumnSet columnSet = new AdaptiveColumnSet();
        AdaptiveColumn photoColumn = new AdaptiveColumn
        {
            Width = "auto"
        };

        AdaptiveImage image = new AdaptiveImage
        {
            Url = new Uri("https://appmodernizationworkshop.blob.core.windows.net/contosoexpenses/Contoso192x192.png"),
            Size = AdaptiveImageSize.Small,
            Style = AdaptiveImageStyle.Default
        };
        photoColumn.Items.Add(image);

        AdaptiveTextBlock amount = new AdaptiveTextBlock
        {
            Text = expense.Cost.ToString(),
            Weight = AdaptiveTextWeight.Bolder,
            Wrap = true
        };

        AdaptiveTextBlock date = new AdaptiveTextBlock
        {
            Text = expense.Date.Date.ToShortDateString(),
            IsSubtle = true,
            Spacing = AdaptiveSpacing.None,
            Wrap = true
        };

        AdaptiveColumn expenseColumn = new AdaptiveColumn
        {
            Width = "stretch"
        };
        expenseColumn.Items.Add(amount);
        expenseColumn.Items.Add(date);

        columnSet.Columns.Add(photoColumn);
        columnSet.Columns.Add(expenseColumn);

        card.Body.Add(title);
        card.Body.Add(columnSet);

        string json = card.ToJson();
        return json;
    }
    ```

#### <a name="about-the-code"></a>有关代码

此方法接收**费用**包含有关要呈现的开销和它的所有信息的对象将生成新**AdaptiveCard**对象。 该方法将添加到卡的以下：

- 一个标题，使用费用的说明。
- 图像，这是 Contoso 徽标。
- 支出金额。
- 支出的日期。

最后 3 个元素分为两个不同的列，以便可以并排放置 Contoso 徽标和有关费用的详细信息。 生成该对象后，该方法返回相应的 JSON 字符串的帮助**ToJson**方法。

### <a name="define-the-user-activity"></a>定义用户活动

现在，已经定义了自适应卡，可以创建基于该用户活动。

1. 将以下语句添加到顶部**TimelineService.cs**文件：

    ```csharp
    using Windows.ApplicationModel.UserActivities;
    using System.Threading.Tasks;
    using Windows.UI.Shell;
    ```

    > [!NOTE]
    > 这些是 UWP 命名空间。 这些解决，因为`Microsoft.Toolkit.Wpf.UI.Controls`在步骤 2 中安装的 NuGet 程序包中包括的引用`Microsoft.Windows.SDK.Contracts`包，它使**ContosoExpenses.Core**项目引用 WinRT Api，即使它是一种.NETCore 3 项目。

2. 添加到以下字段声明`TimelineService`类。

    ```csharp
    private UserActivityChannel _userActivityChannel;
    private UserActivity _userActivity;
    private UserActivitySession _userActivitySession;
    ```

3. 添加以下方法`TimelineService`类。

    ```csharp
    public async Task AddToTimeline(Expense expense)
    {
        _userActivityChannel = UserActivityChannel.GetDefault();
        _userActivity = await _userActivityChannel.GetOrCreateUserActivityAsync($"Expense-{expense.ExpenseId}");

        _userActivity.ActivationUri = new Uri($"contosoexpenses://expense/{expense.ExpenseId}");
        _userActivity.VisualElements.DisplayText = "Contoso Expenses";

        string json = BuildAdaptiveCard(expense);

        _userActivity.VisualElements.Content = AdaptiveCardBuilder.CreateAdaptiveCardFromJson(json);

        await _userActivity.SaveAsync();
        _userActivitySession?.Dispose();
        _userActivitySession = _userActivity.CreateSession();
    }
    ```

4. 保存对更改**TimelineService.cs**。

#### <a name="about-the-code"></a>有关代码

`AddToTimeline`方法首先获取**UserActivityChannel**存储用户活动所需的对象。 然后它会创建新的用户活动使用**GetOrCreateUserActivityAsync**方法，它需要的唯一标识符。 这样一来，如果已存在一个活动，该应用程序可以更新它;否则，它将创建一个新。 通过要生成的应用程序的类型取决于要传递的标识符：

* 如果你想要始终更新同一个活动，使时间线将只显示最新，则可以使用固定的标识符 (如**开支**)。
* 如果你想要跟踪的另一个作为每个活动，以使时间线将显示所有这些，您可以使用动态标识符。

在此方案中，应用将跟踪每个打开的费用为不同的用户活动，因此该代码通过使用关键字创建的每个标识符**费用-** 后面是唯一的费用 id。

该方法将创建后**UserActivity**对象，它将填充以下信息的对象：

* **ActivationUri**用户单击时间线中的活动上时调用。 该代码使用名为的自定义协议**contosoexpenses**应用程序将处理更高版本。
* **VisualElements**对象，它包含一组定义的活动的可视外观的属性。 此代码将设置**DisplayText** （这是在时间线中的项之上显示的标题） 和**内容**。 

这是在其中自适应卡前面定义的作用。 应用程序将您设计自适应卡前面作为内容传入该方法。 但是，Windows 10 使用不同的对象来表示相比所用的卡`AdaptiveCards`NuGet 包。 因此，该方法将卡使用重新创建**CreateAdaptiveCardFromJson**方法公开**AdaptiveCardBuilder**类。 该方法将创建的用户活动后，它将保存该活动并创建一个新会话。

当用户单击时间线中的某个活动**contosoexpenses: / /** 将激活协议，URL 将包括应用程序需要检索所选的费用信息。 是可选的任务，您可以实现协议激活，以便应用程序正确地响应用户使用时间线时。

### <a name="integrate-the-application-with-timeline"></a>应用程序时间线与集成

现在，你已创建时间线进行交互的类，我们可以开始使用它来增强应用程序的体验。 若要使用的最佳位置**AddToTimeline**方法公开**TimelineService**类是在用户打开费用的详细信息页时。

1. 在中**ContosoExpenses.Core**项目中，展开**Viewmodel**文件夹，然后打开**ExpenseDetailViewModel.cs**文件。 这是支持费用详细信息窗口 ViewModel。

2. 找到的公共构造函数**ExpenseDetailViewModel**类，并在构造函数的末尾添加以下代码。 每次打开费用窗口中，该方法会调用**AddToTimeline**方法并传递当前费用。 **TimelineService**类使用此信息来创建使用费用信息的用户活动。

    ```csharp
    TimelineService timeline = new TimelineService();
    timeline.AddToTimeline(expense);
    ```

    完成后，构造函数应如下所示。

    ```csharp
    public ExpensesDetailViewModel(IDatabaseService databaseService, IStorageService storageService)
    {
        var expense = databaseService.GetExpense(storageService.SelectedExpense);

        ExpenseType = expense.Type;
        Description = expense.Description;
        Location = expense.Address;
        Amount = expense.Cost;

        TimelineService timeline = new TimelineService();
        timeline.AddToTimeline(expense);
    }
    ```

3. 按 F5 生成并在调试器中运行应用程序。 从列表中选择某位员工，然后选择支出。 在详细信息页中，记下说明费用、 日期和数量。

4. 按**启动 + 选项卡上**打开时间线。

5. 向下滚动直到您看到一节的当前打开应用程序的列表**今天早些**。 本部分显示了一些最新的用户活动。 单击**看到所有活动**链接旁边**今天早些**标题。

6. 确认看到有关您刚才选择的应用程序中的开销的信息的新卡。

    ![Contoso 费用时间线](images/wpf-modernize-tutorial/ContosoExpensesTimeline.png)

7. 如果您现在可以打开其他费用，您将看到新卡添加为用户活动。 请记住，代码为每个活动使用不同的标识符，因此它会创建每个费用的卡应用中打开。

8. 关闭应用。

## <a name="add-a-notification"></a>添加通知

Contoso 开发团队想要添加的第二个功能是每当新费用将保存到数据库时向用户显示的通知。 若要执行此操作，可以利用在 Windows 10 中，通过 WinRT Api 向开发人员公开的内置通知系统。 此通知系统具有很多优点：

- 通知是与操作系统的其余部分保持一致。
- 它们是可操作。
- 它们存储在操作中心以便可以稍后查看。

若要添加到应用的通知：

1. 在中**解决方案资源管理器**，右键单击**ContosoExpenses.Core**项目中，选择**添加-> 类**。 将类命名**NotificationService.cs**然后单击**确定**。

2. 在中**NotificationService.cs**文件中，将以下语句添加到文件顶部。

    ```csharp
    using Windows.Data.Xml.Dom;
    using Windows.UI.Notifications;
    ```

3. 从文件中声明的命名空间的更改`ContosoExpenses.Core`到`ContosoExpenses`。

4. 添加以下方法`NotificationService`类。

    ```csharp
    public void ShowNotification(string description, double amount)
    {
        string xml = $@"<toast>
                          <visual>
                            <binding template='ToastGeneric'>
                              <text>Expense added</text>
                              <text>Description: {description} - Amount: {amount} </text>
                            </binding>
                          </visual>
                        </toast>";

        XmlDocument doc = new XmlDocument();
        doc.LoadXml(xml);

        ToastNotification toast = new ToastNotification(doc);
        ToastNotificationManager.CreateToastNotifier().Show(toast);
    }
    ```

    由 XML 负载，其中可以包括文本、 图像、 操作和表示 toast 通知。 您可以找到所有受支持的元素[此处](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-schema)。 此代码使用一个非常简单的架构具有两行文本： 标题和正文。 代码定义的 XML 有效负载，并将其在加载后**XmlDocument**对象中，它包装中的 XML **ToastNotification**对象，并显示使用**ToastNotificationManager**类。

5. 在中**ContosoExpenses.Core**项目中，展开**Viewmodel**文件夹，然后打开**AddNewExpenseViewModel.cs**文件。 

6. 找到`SaveExpenseCommand`方法，该按钮的在用户按以保存新费用时触发。 紧靠在调用此方法中，添加以下代码`SaveExpense`方法。

    ```csharp
    NotificationService notificationService = new NotificationService();
    notificationService.ShowNotification(expense.Description, expense.Cost);
    ```

    完成后，`SaveExpenseCommand`方法应如下所示。

    ```csharp
    private RelayCommand _saveExpenseCommand;
    public RelayCommand SaveExpenseCommand
    {
        get
        {
            if (_saveExpenseCommand == null)
            {
                _saveExpenseCommand = new RelayCommand(() =>
                {
                    Expense expense = new Expense
                    {
                        Address = Address,
                        City = City,
                        Cost = Cost,
                        Date = Date,
                        Description = Description,
                        EmployeeId = storageService.SelectedEmployeeId,
                        Type = ExpenseType
                    };

                    databaseService.SaveExpense(expense);

                    NotificationService notificationService = new NotificationService();
                    notificationService.ShowNotification(expense.Description, expense.Cost);

                    Messenger.Default.Send<UpdateExpensesListMessage>(new UpdateExpensesListMessage());
                    Messenger.Default.Send<CloseWindowMessage>(new CloseWindowMessage());
                }, () => IsFormFilled
                );
            }

            return _saveExpenseCommand;
        }
    }
    ```

7. 按 F5 生成并在调试器中运行应用程序。 从列表中选择某位员工，然后单击**添加新的支出**按钮。 完成窗体和按中的所有字段**保存**。

8. 您将收到以下异常。

    ![Toast 通知错误](images/wpf-modernize-tutorial/ToastNotificationError.png)

这一事实： Contoso Expenses 应用程序尚不具有包标识引发此异常。 一些 WinRT Api，包括通知 API，需要包标识才可以在应用中使用。 UWP 应用收到的包标识默认情况下，因为它们可以通过 MSIX 包进行分发。 也可以将其他类型的 Windows 应用，包括 WPF 应用程序部署通过 MSIX 包来获取程序包标识。 本教程的下一部分将探讨如何执行此操作。

## <a name="next-steps"></a>后续步骤

此时在教程中，你已成功添加用户活动到的应用程序与 Windows 时间线、 集成和你亦已添加到的应用程序的用户创建新的费用时，会触发通知。 但是，因为该应用程序需要包标识要使用通知 API，尚不起作用通知。 若要了解如何生成应用程序以获取包标识，并获得其他部署好处 MSIX 包，请参阅[第 5 部分：打包和部署与 MSIX](modernize-wpf-tutorial-5.md)。
