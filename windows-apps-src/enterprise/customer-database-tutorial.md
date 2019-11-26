---
title: 创建客户数据库应用程序
description: 创建客户数据库应用程序，并了解如何实现基本的企业应用程序功能。
keywords: 企业，教程，客户，数据，创建读取更新删除，REST，身份验证
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: b8cf0554464b56337e3d57b58db543092682ffa3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258620"
---
# <a name="tutorial-create-a-customer-database-application"></a>教程：创建客户数据库应用程序

本教程创建一个用于管理客户列表的简单应用。 在此过程中，它引入了对 UWP 中企业应用程序的基本概念的选择。 你将了解如何：

* 针对本地 SQL 数据库执行创建、读取、更新和删除操作。
* 添加一个数据网格，用于在用户界面中显示和编辑客户数据。
* 使用基本窗体布局将 UI 元素排列在一起。

本教程的起点是使用最小 UI 和功能的单页面应用程序，该应用程序基于简化版本的[客户订单数据库示例应用](https://github.com/Microsoft/Windows-appsample-customers-orders-database)。 它是使用C#和 XAML 编写的，因此，我们希望你已经大致熟悉这两种语言。

![正在运行的应用程序的主页](images/customer-database-tutorial/customer-list.png)

### <a name="prerequisites"></a>先决条件

* [确保已安装最新版本的 Visual Studio 和 Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* [克隆或下载客户数据库教程示例](https://github.com/microsoft/windows-tutorials-customer-database)

克隆/下载存储库后，可以通过使用 Visual Studio 打开**CustomerDatabaseTutorial**来编辑项目。

> [!NOTE]
> 查看完整的[客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)，查看本教程所基于的应用。

## <a name="part-1-code-of-interest"></a>第1部分：感兴趣的代码

如果在打开应用后立即运行它，将在空白屏幕顶部看到几个按钮。 尽管这对你不可见，但应用已经包含了一个预配的本地 SQLite 数据库，其中包含几个测试客户。 从这里开始，你将首先实现 UI 控件以显示这些客户，然后转到针对数据库添加操作。 在开始之前，您将在这里工作。

### <a name="views"></a>视图

**CustomerListPage**是应用程序的视图，它为本教程中的单个页面定义 UI。 每当需要在 UI 中添加或更改视觉对象时，就会在此文件中执行此操作。 本教程将指导你添加以下元素：

* 用于显示和编辑客户的**RadDataGrid** 。 
* **System.windows.controls.stackpanel>** 设置新客户的初始值。

### <a name="viewmodels"></a>Viewmodel

**ViewModels\CustomerListPageViewModel.cs**是应用程序的基本逻辑所在的位置。 在视图中执行的每个用户操作将被传递到此文件中进行处理。 在本教程中，你将添加一些新代码并实现以下方法：

* **CreateNewCustomerAsync**，它初始化新的 CustomerViewModel 对象。
* **DeleteNewCustomerAsync**，它在 UI 中显示新客户之前删除该客户。
* **DeleteAndUpdateAsync**，用于处理 "删除" 按钮的逻辑。
* **GetCustomerListAsync**，可从数据库中检索客户列表。
* **SaveInitialChangesAsync**，它将新客户的信息添加到数据库中。
* **UpdateCustomersAsync**，它将刷新 UI，以反映添加或删除的所有客户。

**CustomerViewModel**是客户信息的包装器，用于跟踪是否最近修改了此信息。 你无需向此类添加任何内容，但你将在其他地方添加的一些代码将引用它。

有关如何构造示例的详细信息，请参阅[应用结构概述](../enterprise/customer-database-app-structure.md)。

## <a name="part-2-add-the-datagrid"></a>第2部分：添加 DataGrid

在开始对客户数据进行操作之前，您需要添加一个 UI 控件以显示这些客户。 为此，我们将使用预置的第三方**RadDataGrid**控件。 **Telerik Microsoft.netcore.universalwindowsplatform** NuGet 包已包含在此项目中。 让我们向项目添加网格。

1. 从解决方案资源管理器中打开**Views\CustomerListPage.xaml** 。 在**页**标记中添加以下代码行，以声明指向包含数据网格的 Telerik 命名空间的映射。

    ```xaml
        xmlns:telerikGrid="using:Telerik.UI.Xaml.Controls.Grid"
    ```

2. 在视图的 main **RelativePanel**中的**命令栏**下方，添加一个**RadDataGrid**控件，其中包含一些基本配置选项：

    ```xaml
    <Grid
        x:Name="CustomerListRoot"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <RelativePanel>
            <CommandBar
                x:Name="mainCommandBar"
                HorizontalAlignment="Stretch"
                Background="AliceBlue">
                <!--CommandBar content-->
            </CommandBar>
            <telerikGrid:RadDataGrid
                x:Name="DataGrid"
                BorderThickness="0"
                ColumnDataOperationsMode="Flyout"
                GridLinesVisibility="None"
                GroupPanelPosition="Left"
                RelativePanel.AlignLeftWithPanel="True"
                RelativePanel.AlignRightWithPanel="True"
                RelativePanel.Below="mainCommandBar" />
        </RelativePanel>
    </Grid>
    ```

3. 您已经添加了数据网格，但需要显示数据。 向其中添加以下代码行：

    ```xaml
    ItemsSource="{x:Bind ViewModel.Customers}"
    UserEditMode="Inline"
    ```
    定义要显示的数据源后， **RadDataGrid**将为你处理大部分 UI 逻辑。 但是，如果您运行的是您的项目，则在显示时仍看不到任何数据。 这是因为 ViewModel 尚未加载。

![空白应用，无客户](images/customer-database-tutorial/blank-customer-list.png)

## <a name="part-3-read-customers"></a>第3部分：读取客户

初始化后， **ViewModels\CustomerListPageViewModel.cs**将调用**GetCustomerListAsync**方法。 该方法需要从该教程附带的 SQLite 数据库中检索测试客户数据。

1. 在**ViewModels\CustomerListPageViewModel.cs**中，将**GetCustomerListAsync**方法更新为以下代码：

    ```csharp
    public async Task GetCustomerListAsync()
    {
        var customers = await App.Repository.Customers.GetAsync(); 
        if (customers == null)
        {
            return;
        }
        await DispatcherHelper.ExecuteOnUIThreadAsync(() =>
        {
            Customers.Clear();
            foreach (var c in customers)
            {
                Customers.Add(new CustomerViewModel(c));
            }
        });
    }
    ```
    在加载 ViewModel 时调用**GetCustomerListAsync**方法，但在此步骤之前，它不执行任何操作。 在这里，我们添加了对**存储库/SqlCustomerRepository**中**GetAsync**方法的调用。 这允许它与存储库联系以检索客户对象的可枚举集合。 然后，将它们分析为单独的对象，然后将它们添加到其内部**ObservableCollection**中，以便可以显示和编辑它们。

2. 运行应用-现在会看到显示客户列表的数据网格。

![客户的初始列表](images/customer-database-tutorial/initial-customers.png)

## <a name="part-4-edit-customers"></a>第4部分：编辑客户

您可以通过双击数据网格中的条目来对其进行编辑，但您需要确保在 UI 中进行的任何更改也会在代码隐藏中对客户的集合进行。 这意味着必须实现双向数据绑定。 如果需要有关此问题的详细信息，请查看我们[的数据绑定简介](../get-started/display-customers-in-list-learning-track.md)。

1. 首先，声明**ViewModels\CustomerListPageViewModel.cs**实现**INotifyPropertyChanged**接口：

    ```csharp
    public class CustomerListPageViewModel : INotifyPropertyChanged
    ```

2. 然后，在类的主体中，添加以下事件和方法：

    ```csharp
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    ```

    利用**OnPropertyChanged**方法，你可以轻松地让你的 Setter 引发**PropertyChanged**事件，这对于双向数据绑定是必需的。

3. 通过此函数调用更新**SelectedCustomer**的 setter：

    ```csharp
    public CustomerViewModel SelectedCustomer
    {
        get => _selectedCustomer;
        set
        {
            if (_selectedCustomer != value)
            {
                _selectedCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

4. 在**Views\CustomerListPage.xaml**中，将**SelectedCustomer**属性添加到数据网格。

    ```xaml
    SelectedItem="{x:Bind ViewModel.SelectedCustomer, Mode=TwoWay}"
    ```

    这会将数据网格中用户的选择与代码隐藏中的相应 Customer 对象相关联。 双向绑定模式允许在 UI 中进行的更改反映在该对象上。

5. 运行你的应用程序。 现在，可以看到网格中显示的客户，并通过用户界面对基础数据进行更改。

![在数据网格中编辑客户](images/customer-database-tutorial/edit-customer-inline.png)

## <a name="part-5-update-customers"></a>第5部分：更新客户

现在你可以查看和编辑你的客户，你将需要能够将你的更改推送到数据库，并提取其他人所做的任何更新。

1. 返回到**ViewModels\CustomerListPageViewModel.cs**，并导航到**UpdateCustomersAsync**方法。 将其更新为以下代码，以将更改推送到数据库并检索任何新信息：

    ```csharp
    public async Task UpdateCustomersAsync()
    {
        foreach (var modifiedCustomer in Customers
            .Where(x => x.IsModified).Select(x => x.Model))
        {
            await App.Repository.Customers.UpsertAsync(modifiedCustomer);
        }
        await GetCustomerListAsync();
    }
    ```
    此代码使用**ViewModels\CustomerViewModel.cs**的**IsModified**属性，每当客户发生更改时，它都会自动更新。 这样可以避免不必要的调用，并且仅将更改从已更新的客户推送到数据库。

## <a name="part-6-create-a-new-customer"></a>第6部分：创建新客户

添加新客户会提出一项挑战，因为如果将客户添加到用户界面，则在提供其属性的值之前将其显示为空白行。 这并不是问题，但在这里，我们可以更轻松地设置客户的初始值。 在本教程中，我们将添加一个简单的可折叠面板，但如果要添加更多的信息，可以创建一个单独的页面来实现此目的。

### <a name="update-the-code-behind"></a>更新代码隐藏

1. 向**ViewModels\CustomerListPageViewModel.cs**添加新的私有字段和公共属性。 这将用于控制面板是否可见。

    ```csharp
    private bool _addingNewCustomer = false;

    public bool AddingNewCustomer
    {
        get => _addingNewCustomer;
        set
        {
            if (_addingNewCustomer != value)
            {
                _addingNewCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2. 将一个新的公共属性添加到 ViewModel，它是**AddingNewCustomer**值的逆值。 这将用于在面板可见时禁用常规命令栏按钮。

    ```csharp
    public bool EnableCommandBar => !AddingNewCustomer;
    ```
    现在，您需要一种方法来显示可折叠面板，并创建要在其中进行编辑的客户。 

3. 向 ViewModel 添加新的私有 fiend 和公共属性，以保存新创建的客户。

    ```csharp
    private CustomerViewModel _newCustomer;

    public CustomerViewModel NewCustomer
    {
        get => _newCustomer;
        set
        {
            if (_newCustomer != value)
            {
                _newCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2.  更新**CreateNewCustomerAsync**方法以创建新客户，将其添加到存储库，并将其设置为所选客户：

    ```csharp
    public async Task CreateNewCustomerAsync()
    {
        CustomerViewModel newCustomer = new CustomerViewModel(new Models.Customer());
        NewCustomer = newCustomer;
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        AddingNewCustomer = true;
    }
    ```

3. 更新**SaveInitialChangesAsync**方法，将新创建的客户添加到存储库，更新 UI，并关闭面板。

    ```csharp
    public async Task SaveInitialChangesAsync()
    {
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        await UpdateCustomersAsync();
        AddingNewCustomer = false;
    }
    ```
4. 将以下代码行添加为**AddingNewCustomer**的资源库中的最后一行：

    ```csharp
    OnPropertyChanged(nameof(EnableCommandBar));
    ```

    这将确保每当更改**AddingNewCustomer**时， **EnableCommandBar**都会自动更新。

### <a name="update-the-ui"></a>更新 UI

1. 导航回到**Views\CustomerListPage.xaml**，并在**命令栏**和数据网格之间添加具有以下属性的**system.windows.controls.stackpanel>** ：

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
    </StackPanel>
    ```
    **X:Load**属性确保在添加新客户时仅显示此面板。

2. 对数据网格的位置进行以下更改，以确保在显示新面板时它向下移动：

    ```xaml
    RelativePanel.Below="newCustomerPanel"
    ```

3. 更新具有四个**TextBox**控件的堆栈面板。 它们将绑定到新客户的各个属性，并允许您在将其添加到数据网格之前编辑其值。

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
        <TextBox
            Header="First name"
            PlaceholderText="First"
            Margin="8,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.FirstName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Last name"
            PlaceholderText="Last"
            Margin="0,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.LastName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Address"
            PlaceholderText="1234 Address St, Redmond WA 00000"
            Margin="0,8,16,8"
            MinWidth="280"
            Text="{x:Bind ViewModel.NewCustomer.Address, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Company"
            PlaceholderText="Company"
            Margin="0,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.Company, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
    </StackPanel>
    ```

4. 将一个简单的按钮添加到新的堆栈面板，以保存新创建的客户：

    ```xaml
    <StackPanel>
        <!--Text boxes from step 3-->
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

5. 更新**命令栏**，以便在 "堆栈" 面板可见时禁用常规的 "创建"、"删除" 和 "更新" 按钮：

    ```xaml
    <CommandBar
        x:Name="mainCommandBar"
        HorizontalAlignment="Stretch"
        IsEnabled="{x:Bind ViewModel.EnableCommandBar, Mode=OneWay}"
        Background="AliceBlue">
        <!--App bar buttons-->
    </CommandBar>
    ```

6. 运行你的应用程序。 你现在可以在 "堆栈" 面板中创建客户并输入其数据。

![创建新客户](images/customer-database-tutorial/add-new-customer.png)

## <a name="part-7-delete-a-customer"></a>第7部分：删除客户

删除客户是需要实现的最后基本操作。 如果删除在数据网格中选择的客户，则需要立即调用**UpdateCustomersAsync**以更新 UI。 但是，如果要删除刚刚创建的客户，则无需调用此方法。

1. 导航到**ViewModels\CustomerListPageViewModel.cs**，并更新**DeleteAndUpdateAsync**方法：

    ```csharp
    public async void DeleteAndUpdateAsync()
    {
        if (SelectedCustomer != null)
        {
            await App.Repository.Customers.DeleteAsync(_selectedCustomer.Model.Id);
        }
        await UpdateCustomersAsync();
    }
    ```

2. 在**Views\CustomerListPage.xaml**中，更新用于添加新客户的堆栈面板，使其包含第二个按钮：

    ```xaml
    <StackPanel>
        <!--Text boxes for adding a new customer-->
        <AppBarButton
            x:Name="DeleteNewCustomer"
            Click="{x:Bind ViewModel.DeleteNewCustomerAsync}"
            Icon="Cancel"/>
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

3. 在**ViewModels\CustomerListPageViewModel.cs**中，更新**DeleteNewCustomerAsync**方法以删除新客户：

    ```csharp
    public async Task DeleteNewCustomerAsync()
    {
        if (NewCustomer != null)
        {
            await App.Repository.Customers.DeleteAsync(_newCustomer.Model.Id);
            AddingNewCustomer = false;
        }
    }
    ```

4. 运行你的应用程序。 你现在可以在数据网格或 "堆栈" 面板中删除客户。

![删除新客户](images/customer-database-tutorial/delete-new-customer.png)

## <a name="conclusion"></a>总结

恭喜你！ 完成此操作后，你的应用程序将具有完整的本地数据库操作范围。 你可以在用户界面中创建、读取、更新和删除客户，这些更改将保存到你的数据库，并将在应用程序的不同启动范围内保留。

完成后，请考虑以下事项：
* 如果尚未这样做，请查看[应用结构概述](../enterprise/customer-database-app-structure.md)，了解有关应用构建方式的详细信息。
* 浏览[完整的客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)，以查看本教程所基于的应用。

或者，如果您有一个挑战，您可以继续 。

## <a name="going-further-connect-to-a-remote-database"></a>进一步：连接到远程数据库

我们提供了有关如何针对本地 SQLite 数据库实现这些调用的分步演练。 但是，如果您想要使用远程数据库，应该怎么办？

如果你想要进行尝试，你将需要自己的 Azure Active Directory （AAD）帐户和托管你自己的数据源的能力。

需要添加身份验证和函数来处理 REST 调用，然后创建远程数据库以与进行交互。 完整[客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)中有一些代码，你可以为每个必要的操作引用。

### <a name="settings-and-configuration"></a>设置和配置

用于连接到您自己的远程数据库的必要步骤在[示例的自述文件](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/README.md)中进行了介绍。 你需要执行以下操作：

* 向[Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)提供 Azure 帐户客户端 Id。
* 向[Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)提供远程数据库的 url。
* 提供要[Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)的数据库的连接字符串。
* 将应用与 Microsoft Store 相关联。
* 将[服务项目](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoService)复制到你的应用，并将其部署到 Azure。

### <a name="authentication"></a>身份验证

你需要创建一个按钮来启动身份验证序列，并使用一个弹出窗口或单独的页来收集用户的信息。 创建后，需要提供请求用户信息的代码，并使用该信息获取访问令牌。 客户订单数据库示例使用**WebAccountManager**库包装 Microsoft Graph 调用，以获取令牌并处理 AAD 帐户的身份验证。

* 身份验证逻辑是在[**AuthenticationViewModel.cs**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/ViewModels/AuthenticationViewModel.cs)中实现的。
* 身份验证过程显示在自定义[**AuthenticationControl**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/UserControls/AuthenticationControl.xaml)控件中。

### <a name="rest-calls"></a>REST 调用

您无需修改我们在本教程中添加的任何代码即可实现 REST 调用。 相反，您需要执行以下操作：

* 创建**ICustomerRepository**和**ITutorialRepository**接口的新实现，通过 REST 而不是 SQLite 实现一组相同的函数。 需要序列化并反序列化 JSON，并可以在需要时将 REST 调用包装在单独的**HttpHelper**类中。 有关详细信息，请参阅[完整示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoRepository/Rest)。
* 在**App.xaml.cs**中，创建一个新函数以初始化 REST 存储库，并在初始化应用程序时调用它，而不是**SqliteDatabase** 。 同样，请参阅[完整示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/App.xaml.cs)。

完成这三个步骤后，你应该能够通过应用向 AAD 帐户进行身份验证。 对远程数据库的 REST 调用将替换本地 SQLite 调用，但用户体验应该是相同的。 如果感觉更具雄心，可以添加 "设置" 页，以允许用户在两者之间动态切换。
