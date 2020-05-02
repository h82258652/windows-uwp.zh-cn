---
description: 本教程演示如何添加 UWP XAML 用户界面、创建 MSIX 包以及将其他现代组件合并到 WPF 应用中。
title: 添加 Windows 10 用户活动和通知
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows 窗体, wpf, xaml 岛
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 8443ac25ba678986046b967a90a8899eaffb76aa
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "67420117"
---
# <a name="part-4-add-windows-10-user-activities-and-notifications"></a>第 4 部分：添加 Windows 10 用户活动和通知

本文是演示如何现代化名为 Contoso Expenses 的示例 WPF 桌面应用的教程的第 4 部分。 有关本教程的概述、先决条件以及有关下载示例应用的说明，请参阅[教程：现代化 WPF 应用](modernize-wpf-tutorial.md)。 本文假设已完成[第 3 部分](modernize-wpf-tutorial-3.md)。

在本教程的前面部分，你已使用 XAML Islands 向应用添加了 UWP XAML 控件。 作为此操作的附带效果，该应用还能够调用任何 WinRT API。 这样，该应用便有机会使用 Windows 10 提供的许多其他功能，而不仅仅是 UWP XAML 控件。

在本教程的虚构场景中，Contoso 开发团队决定将以下两项新功能添加到应用：活动和通知。 本教程部分介绍如何实现这些功能。

## <a name="add-a-user-activity"></a>添加用户活动

在 Windows 10 中，应用可以跟踪用户执行的活动，例如打开文件或显示特定的页面。 然后，可以通过时间线使用这些活动。时间线是 Windows 10 版本 1803 中引入的一项功能，它使用户能够快速回到过去，恢复他们以前启动的某个活动。

![Windows 时间线图像](images/wpf-modernize-tutorial/WindowsTimeline.png)

可使用 [Microsoft Graph](https://developer.microsoft.com/graph/) 跟踪用户活动。 但是，在生成 Windows 10 应用时，不需要直接与 Microsoft Graph 提供的 REST 终结点交互， 而可以使用一组便利的 WinRT API。 我们将在 Contoso Expenses 应用中，使用这些 WinRT API 来跟踪用户每次在该应用中打开某项支出时的情况，并使用自适应卡片来让用户创建活动。

### <a name="introduction-to-adaptive-cards"></a>自适应卡片简介

本部分提供[自适应卡片](https://docs.microsoft.com/adaptive-cards/)的简要概述。 如果你不需要此类信息，可以直接跳到[添加自适应卡片](#add-an-adaptive-card)说明。

自适应卡片使开发人员能够以一致的常用方式交换卡片内容。 自适应卡片由定义其内容（可以包括文本、图像、操作等）的 JSON 有效负载描述。

自适应卡片仅定义内容，而不定义内容的视觉外观。 接收自适应卡片的平台可以使用最合适的样式呈现内容。 自适应卡片是通过[呈现器](https://docs.microsoft.com/adaptive-cards/rendering-cards/getting-started)设计的。呈现器可以提取 JSON 有效负载并将其转换为本机 UI。 例如，UI 可以是 WPF 或 UWP 应用的 XAML、Android 应用的 AXML，或者网站或机器人聊天应用的 HTML。

下面是一个简单自适应卡片有效负载的示例。

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

下图演示了 Teams 通道、Cortana 和 Windows 通知如何以不同的方式呈现此 JSON。

![自适应卡片呈现图像](images/wpf-modernize-tutorial/AdaptiveCards.png)

自适应卡片在时间线中发挥着重要作用，因为它是 Windows 呈现活动的方式。 时间线中显示的每个缩略图实际上是一个自适应卡片。 因此，在应用中创建用户活动时，系统会要求你提供一个自适应卡片来呈现该活动。

> [!NOTE]
> 使用[在线设计器](https://adaptivecards.io/designer/)是集体讨论如何设计自适应卡片的极佳方式。 这样，就有机会使用构建基块（图像、文本、列等）设计卡片，并获取相应的 JSON。 确定最终设计的思路后，可以使用一个名为[自适应卡片](https://www.nuget.org/packages/AdaptiveCards/)的库，更轻松地通过 C# 类而不是普通 JSON（普通 JSON 可能难以调试和生成）生成自适应卡片。

### <a name="add-an-adaptive-card"></a>添加自适应卡片

1. 在解决方案资源管理器中右键单击“ContosoExpenses.Core”项目，并选择“管理 NuGet 包”。  

2. 在“NuGet 包管理器”窗口中，单击“浏览”。   搜索 `Newtonsoft.Json` 包并安装最新可用版本。 这是一个常用的 JSON 操作库，可帮助操作自适应卡片所需的 JSON 字符串。

    ![NewtonSoft.Json NuGet 包](images/wpf-modernize-tutorial/JsonNetNuGet.png)

    > [!NOTE]
    > 如果不单独安装 `Newtonsoft.Json` 包，自适应卡片库将引用不支持 .NET Core 3.0 的旧版 `Newtonsoft.Json` 包。

2. 在“NuGet 包管理器”窗口中，单击“浏览”。   搜索 `AdaptiveCards` 包并安装最新可用版本。

    ![自适应卡片 NuGet 包](images/wpf-modernize-tutorial/AdaptiveCardsNuGet.png)

3. 在“解决方案资源管理器”中，右键单击“ContosoExpenses.Core”项目并选择“添加”->“类”。    将该类命名为 TimelineService.cs，然后单击“确定”。  

4. 将以下语句添加到 TimelineService.cs 文件的顶部。 

    ```csharp
    using AdaptiveCards;
    using ContosoExpenses.Data.Models;
    ```

5. 将该文件中声明的命名空间从 `ContosoExpenses.Core` 更改为 `ContosoExpenses`。

5. 将以下方法添加到 `TimelineService` 类。

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

#### <a name="about-the-code"></a>关于代码

此方法接收 Expense 对象（包含有关要呈现的支出的所有信息），并生成新的 AdaptiveCard 对象。   该方法将以下内容添加到卡片：

- 一个使用支出说明的标题。
- 一个图像，即 Contoso 徽标。
- 支出金额。
- 支出日期。

最后 3 个元素拆分为两个不同的列，使 Contoso 徽标和有关支出的详细信息可以并列放置。 生成对象后，该方法借助 ToJson 方法返回相应的 JSON 字符串。 

### <a name="define-the-user-activity"></a>定义用户活动

定义自适应卡片后，接下来可以基于自适应卡片创建用户活动。

1. 将以下语句添加到 TimelineService.cs 文件的顶部： 

    ```csharp
    using Windows.ApplicationModel.UserActivities;
    using System.Threading.Tasks;
    using Windows.UI.Shell;
    ```

    > [!NOTE]
    > 这些内容是 UWP 命名空间。 它们之所以能够解决需求，是因为在步骤 2 中安装的 `Microsoft.Toolkit.Wpf.UI.Controls` NuGet 包包含对 `Microsoft.Windows.SDK.Contracts` 包的引用，使 ContosoExpenses.Core 项目能够引用 WinRT API，即使它是一个 .NET Core 3 项目。 

2. 将以下字段声明添加到 `TimelineService` 类。

    ```csharp
    private UserActivityChannel _userActivityChannel;
    private UserActivity _userActivity;
    private UserActivitySession _userActivitySession;
    ```

3. 将以下方法添加到 `TimelineService` 类。

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

4. 保存对 TimelineService.cs 所做的更改。 

#### <a name="about-the-code"></a>关于代码

`AddToTimeline` 方法首先获取存储用户活动所需的 UserActivityChannel 对象。  然后，它使用一个需要唯一标识符的 GetOrCreateUserActivityAsync 方法创建新用户活动。  这样，如果某个活动已存在，则应用可以更新它；否则，应用将创建一个新活动。 要传递的标识符取决于要生成的应用程序类型：

* 若要始终更新同一活动，使时间线只显示最近的活动，可以使用固定的标识符（例如“Expenses”）。 
* 若要将每个活动作为不同的活动进行跟踪，使时间线显示所有活动，可以使用动态标识符。

在本场景中，应用将每个已打开的支出作为不同的用户活动进行跟踪，因此，代码将使用关键字“Expense-”后接唯一的支出 ID 来创建每个标识符。 

方法创建 UserActivity 对象后，将使用以下信息填充该对象： 

* 当用户单击时间线中的活动时调用的 ActivationUri。  代码使用名为 contosoexpenses 的自定义协议，应用稍后会处理该协议。 
* VisualElements 对象，其中包含一组用于定义活动视觉外观的属性。  此代码将设置 DisplayText（在时间线中的条目顶部显示的标题）和 Content。   

此时，前面定义的自适应卡片将起到作用。 应用将前面设计的自适应卡片作为内容传递给方法。 但是，Windows 10 使用不同的对象来表示卡片，而不是使用 `AdaptiveCards` NuGet 包所用的对象。 因此，方法将使用 AdaptiveCardBuilder 类公开的 CreateAdaptiveCardFromJson 方法重新创建卡片。   方法创建用户活动后，会保存该活动并创建新的会话。

当用户单击时间线中的某个活动时，contosoexpenses:// 协议将会激活，URL 将包含应用检索所选支出时所需的信息。  作为一个可选任务，可以实现协议激活，使应用程序在用户使用时间线时能够正确做出反应。

### <a name="integrate-the-application-with-timeline"></a>将应用程序与时间线集成

创建一个与时间线交互的类后，接下来我们可以开始使用该类来增强应用程序的体验。 使用 TimelineService 类公开的 AddToTimeline 方法的最佳时机是当用户打开支出的详细信息页时。  

1. 在“ContosoExpenses.Core”项目中展开“ViewModels”文件夹，然后打开“ExpenseDetailViewModel.cs”文件。    这是支持支出详细信息窗口的 ViewModel。

2. 找到 ExpenseDetailViewModel 类的公共构造函数，并在该构造函数的末尾添加以下代码。  每次打支出窗口时，该方法都会调用 AddToTimeline 方法并传递当前支出。  TimelineService 类使用此信息来创建使用支出信息的用户活动。 

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

3. 按 F5 在调试器中生成并运行应用。 从列表中选择一名员工，然后选择一项支出。 在详细信息页中，记下支出说明、日期和金额。

4. 按“开始”按钮 + TAB 打开“时间线”。 

5. 向下滚动当前打开的应用程序列表，直到出现标题为“今天早些时候”的部分。  此部分显示最近的一些用户活动。 单击“今天早些时候”标题旁边的“查看所有活动”链接。  

6. 确认看到了一个新卡片，其中包含有关刚刚在应用程序中选择的支出的信息。

    ![Contoso 支出时间线](images/wpf-modernize-tutorial/ContosoExpensesTimeline.png)

7. 如果现在打开其他支出，将看到正在添加为用户活动的新卡片。 请记住，代码对每个活动使用不同的标识符，因此它将为在应用中打开的每项支出创建一个卡片。

8. 关闭应用。

## <a name="add-a-notification"></a>添加通知

Contoso 开发团队想要添加的第二项功能是，每当在数据库中保存新支出时，就向用户显示一条通知。 为此，可以利用 Windows 10 中的内置通知系统，该系统通过 WinRT API 向开发人员公开。 此通知系统具有很多优势：

- 整个 OS 中显示的通知是一致的。
- 通知是可操作的。
- 通知存储在操作中心，因此以后可以查看。

若要将通知添加到应用：

1. 在“解决方案资源管理器”中，右键单击“ContosoExpenses.Core”项目并选择“添加”->“类”。    将该类命名为 NotificationService.cs，然后单击“确定”。  

2. 将以下语句添加到 NotificationService.cs 文件的顶部。 

    ```csharp
    using Windows.Data.Xml.Dom;
    using Windows.UI.Notifications;
    ```

3. 将该文件中声明的命名空间从 `ContosoExpenses.Core` 更改为 `ContosoExpenses`。

4. 将以下方法添加到 `NotificationService` 类。

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

    Toast 通知由 XML 有效负载（可以包括文本、图像、操作等）表示。 可在[此处](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-schema)找到所有支持的元素。 此代码使用一个非常简单的架构，其中包含两行文本：标题和正文。 代码在定义 XML 有效负载并将其载入 XmlDocument 对象后，会将 XML 包装在 ToastNotification 对象中，然后使用 ToastNotificationManager 类显示该对象。   

5. 在“ContosoExpenses.Core”项目中展开“ViewModels”文件夹，然后打开“AddNewExpenseViewModel.cs”文件。    

6. 找到 `SaveExpenseCommand` 方法。当用户按下保存新支出的按钮时，将触发该方法。 将以下代码添加到此方法中紧靠在 `SaveExpense` 方法调用后面的位置。

    ```csharp
    NotificationService notificationService = new NotificationService();
    notificationService.ShowNotification(expense.Description, expense.Cost);
    ```

    完成后，`SaveExpenseCommand` 方法应如下所示。

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

7. 按 F5 在调试器中生成并运行应用。 从列表中选择一名员工，然后单击“添加新的支出”按钮。  填写表单中的所有字段，然后按“保存”。 

8. 随后会收到以下异常。

    ![Toast 通知错误](images/wpf-modernize-tutorial/ToastNotificationError.png)

出现此异常的原因是 Contoso Expenses 应用目前没有包标识。 某些 WinRT API（包括通知 API）需要包标识才能在应用中使用。 默认情况下，UWP 应用会收到包标识，因为只能通过 MSIX 包分发这些应用。 还可以通过 MSIX 包部署其他类型的 Windows 应用（包括 WPF 应用），这样也可以获取包标识。 本教程的下一部分将介绍如何执行此操作。

## <a name="next-steps"></a>后续步骤

在本教程的此阶段，你已成功地在与 Windows 时间线集成的应用中添加了一个用户活动，并在应用中添加了一个在用户创建新支出时触发的通知。 但是，该通知尚不起作用，因为应用需要获得包标识才能使用通知 API。 若要了解如何为应用生成用于获取包标识的 MSIX 包，以及如何获得其他部署权益，请参阅[第 5 部分：使用 MSIX 打包和部署](modernize-wpf-tutorial-5.md)。
