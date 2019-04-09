---
title: 创建客户数据库应用程序
description: 创建客户数据库应用程序，并了解如何实现基本的企业应用函数。
keywords: 企业版教程，客户数据，创建读取、 更新删除，其余部分中，身份验证
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: 7bd3a180762c3ef06d7c24ae001fb2c7fb7fc55e
ms.sourcegitcommit: 6df46d7d5b5522805eab11a9c0e07754f28673c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2019
ms.locfileid: "58808295"
---
# <a name="tutorial-create-a-customer-database-application"></a>教程：创建客户数据库应用程序

本教程将创建一个简单的应用程序，用于管理客户的列表。 这样，它引入了一系列 UWP 中针对企业应用的基本概念。 你将了解如何：

* 实现创建、 读取、 更新和删除操作对本地 SQL 数据库。
* 添加数据网格，以显示和编辑你的 UI 中的客户数据。
* 排列在一起在基本窗体布局中的 UI 元素。

本教程的起始点是包含最小 UI 和功能，基于的简化版本的单页面应用程序[客户订单数据库示例应用](https://github.com/Microsoft/Windows-appsample-customers-orders-database)。 它编写的C#和 XAML，和我们希望，您已基本熟悉这些这两种语言。

![正在运行的应用的主页](images/customer-database-tutorial/customer-list.png)

### <a name="prerequisites"></a>先决条件

* [确保拥有最新版本的 Visual Studio 和 Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* [克隆或下载的客户数据库教程示例](https://aka.ms/customer-database-tutorial)

已克隆/下载后存储库，可以通过打开编辑项目**CustomerDatabaseTutorial.sln**使用 Visual Studio。

> [!NOTE]
> 请查看[完整的客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)查看本教程的基础的应用。

## <a name="part-1-code-of-interest"></a>第 1 部分：感兴趣的代码

如果它在打开后立即运行您的应用程序，你会看到空白屏幕顶部的几个按钮。 尽管不是对你可见的该应用已包括与几个测试客户预配本地 SQLite 数据库。 在这里，将首先实现 UI 控件来显示这些客户，，然后转到在对数据库执行的操作中添加。 在开始之前，下面是您将正常工作。

### <a name="views"></a>Views

**CustomerListPage.xaml**是应用程序的视图，它会在本教程中定义单个页面的 UI。 每当您需要添加或更改可视元素在 UI 中，您将执行此操作此文件中。 本教程将引导你完成添加这些元素：

* 一个**RadDataGrid**用于显示和编辑你的客户。 
* 一个**StackPanel**的初始值设置为新客户。

### <a name="viewmodels"></a>ViewModels

**ViewModels\CustomerListPageViewModel.cs**是应用程序的基本逻辑所在的位置。 在视图中执行每个用户操作将传递到处理此文件。 在本教程中，将添加一些新代码，实现以下方法：

* **CreateNewCustomerAsync**，其初始化新的 CustomerViewModel 对象。
* **DeleteNewCustomerAsync**，这将显示在 UI 中之前删除新客户。
* **DeleteAndUpdateAsync**，用于处理删除按钮的逻辑。
* **GetCustomerListAsync**，从数据库中检索客户列表。
* **SaveInitialChangesAsync**，从而将新客户的信息添加到数据库。
* **UpdateCustomersAsync**，这会刷新 UI 以反映添加或删除任何客户。

**CustomerViewModel**是客户的信息，跟踪是否最近修改了它的包装器。 无需将任何内容添加到此类，但某些其他位置，您将添加的代码将引用它。

有关如何构造该示例的详细信息，请参阅[应用程序结构概述](../enterprise/customer-database-app-structure.md)。

## <a name="part-2-add-the-datagrid"></a>第 2 部分：添加数据网格

在开始对客户数据执行操作之前，你将需要添加 UI 控件来显示这些客户。 若要执行此操作，我们将使用第三方预制**RadDataGrid**控件。 **Telerik.UI.for.UniversalWindowsPlatform** NuGet 包已包含在此项目。 让我们将在网格添加到我们的项目。

1. 打开**Views\CustomerListPage.xaml**从解决方案资源管理器。 添加以下行中的代码**页**标记声明到包含数据网格的 Telerik 命名空间的映射。

    ```xaml
        xmlns:telerikGrid="using:Telerik.UI.Xaml.Controls.Grid"
    ```

2. 下面**CommandBar**内主**RelativePanel**视图中，添加**RadDataGrid**控件，其中包含一些基本配置选项：

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

3. 已添加数据网格中，但它需要要显示的数据。 将以下代码行添加到它：

    ```xaml
    ItemsSource="{x:Bind ViewModel.Customers}"
    UserEditMode="Inline"
    ```
    现在，已经定义要显示的数据源**RadDataGrid**会为您处理大部分 UI 逻辑。 但是，如果运行你的项目，你仍不会看到任何数据在显示器上。 这是因为 ViewModel 不尚未加载。

![空白应用程序，而没有客户](images/customer-database-tutorial/blank-customer-list.png)

## <a name="part-3-read-customers"></a>第 3 部分：阅读客户

它在初始化时， **ViewModels\CustomerListPageViewModel.cs**调用**GetCustomerListAsync**方法。 方法需要测试的客户数据检索 SQLite 数据库，包括在本教程中。

1. 在中**ViewModels\CustomerListPageViewModel.cs**，更新你**GetCustomerListAsync**方法，此代码：

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
    **GetCustomerListAsync** ViewModel 已加载，但在此步骤中之前, 它不执行任何操作时，调用方法。 在这里，我们已添加了对**GetAsync**中的方法**存储库/SqlCustomerRepository**。 这使得它可以与联系以检索客户对象的可枚举集合的存储库。 它然后对它们进行分析为单个对象，然后将它们添加到其内部**ObservableCollection**以便它们可以显示和编辑。

2. 运行你的应用-现在将看到数据网格中显示客户列表。

![初始客户列表](images/customer-database-tutorial/initial-customers.png)

## <a name="part-4-edit-customers"></a>第 4 部分：编辑客户

可以通过双击它们，编辑数据网格中的条目，但您需要确保在 UI 中进行任何更改还对在代码隐藏中的客户的集合。 这意味着您必须实现双向数据绑定。 如果你想更多相关信息，请查看我们[简介数据绑定](../get-started/display-customers-in-list-learning-track.md)。

1. 首先，声明的**ViewModels\CustomerListPageViewModel.cs**实现**INotifyPropertyChanged**接口：

    ```csharp
    public class CustomerListPageViewModel : INotifyPropertyChanged
    ```

2. 然后，在类的主要部分，添加以下事件和方法：

    ```csharp
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    ```

    **OnPropertyChanged**方法，可轻松为你的 setter 引发**PropertyChanged**事件时所需的双向数据绑定。

3. 更新的 setter **SelectedCustomer**使用此函数调用：

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

4. 在中**Views\CustomerListPage.xaml**，添加**SelectedCustomer**向数据网格的属性。

    ```xaml
    SelectedItem="{x:Bind ViewModel.SelectedCustomer, Mode=TwoWay}"
    ```

    这将在数据网格中的用户的选择与代码隐藏中相应的客户对象相关联。 TwoWay 绑定模式，才会反映对该对象在 UI 中所做的更改。

5. 运行你的应用。 现在可以看到显示在网格中，客户，并通过您的 UI 对基础数据进行更改。

![编辑数据网格中的客户](images/customer-database-tutorial/edit-customer-inline.png)

## <a name="part-5-update-customers"></a>第 5 部分：更新的客户

现在，您可以查看和编辑你的客户，你将需要能够将更改推送到数据库，并获取其他人所做的任何更新。

1. 返回到**ViewModels\CustomerListPageViewModel.cs**，并导航到**UpdateCustomersAsync**方法。 使用此代码中，将更改推送到数据库并检索任何最新信息更新它：

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
    此代码利用**IsModified**的属性**ViewModels\CustomerViewModel.cs**，这会自动更新客户更改时。 这允许您以避免不必要的调用，并仅将更改从更新的客户推送到数据库。

## <a name="part-6-create-a-new-customer"></a>第 6 部分：创建新客户

添加新客户提供了一种挑战，如客户将显示为一个空白行，如果您将其添加到 UI 之前为其属性提供值。 这不是个问题，但此处我们将使其更轻松地设置客户的初始值。 在本教程中，我们将添加一个简单的可折叠面板，但如果有要添加的详细信息可以实现此目的创建一个单独的页。

### <a name="update-the-code-behind"></a>更新代码隐藏

1. 添加新的私有字段和公共属性设置为**ViewModels\CustomerListPageViewModel.cs**。 这将用于控制在面板可见。

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

2. 将新的公共属性添加到 ViewModel 的值的逆**AddingNewCustomer**。 这将用于禁用常规命令栏按钮时面板可见。

    ```csharp
    public bool EnableCommandBar => !AddingNewCustomer;
    ```
    现在将需要一种方法来显示可折叠的面板中，并创建要在其中编辑客户。 

3. 将新的专用 fiend 和公共属性添加到 ViewModel 中，以保存新创建的客户。

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

2.  更新你**CreateNewCustomerAsync**方法以创建新客户，将其添加到存储库，并将其设置为所选客户：

    ```csharp
    public async Task CreateNewCustomerAsync()
    {
        CustomerViewModel newCustomer = new CustomerViewModel(new Models.Customer());
        NewCustomer = newCustomer;
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        AddingNewCustomer = true;
    }
    ```

3. 更新**SaveInitialChangesAsync**方法来将新创建的客户添加到存储库中，更新 UI，并关闭面板。

    ```csharp
    public async Task SaveInitialChangesAsync()
    {
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        await UpdateCustomersAsync();
        AddingNewCustomer = false;
    }
    ```
4. 将以下代码行添加为中的 setter 的最后一行**AddingNewCustomer**:

    ```csharp
    OnPropertyChanged(nameof(EnableCommandBar));
    ```

    这将确保**EnableCommandBar**每当自动更新**AddingNewCustomer**发生更改。

### <a name="update-the-ui"></a>更新 UI

1. 导航回到**Views\CustomerListPage.xaml**，并添加**StackPanel**具有以下属性之间你**CommandBar**和数据网格：

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
    </StackPanel>
    ```
    **X： 负载**属性可确保在添加新客户仅显示此面板。

2. 在数据网格，以确保它将下移的，当新面板出现时的位置进行以下更改：

    ```xaml
    RelativePanel.Below="newCustomerPanel"
    ```

3. 具有四个更新堆叠面板**文本框中**控件。 它们将绑定到单个属性的新客户，并允许您编辑它的值，然后将其添加到数据网格。

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

4. 将一个简单的按钮添加到新的堆栈面板以保存新建的客户：

    ```xaml
    <StackPanel>
        <!--Text boxes from step 3-->
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

5. 更新**CommandBar**，因此正则创建、 删除和更新按钮已禁用时的堆栈面板是可见：

    ```xaml
    <CommandBar
        x:Name="mainCommandBar"
        HorizontalAlignment="Stretch"
        IsEnabled="{x:Bind ViewModel.EnableCommandBar, Mode=OneWay}"
        Background="AliceBlue">
        <!--App bar buttons-->
    </CommandBar>
    ```

6. 运行你的应用。 现在可以创建客户，并输入其堆叠面板中的数据。

![创建新客户](images/customer-database-tutorial/add-new-customer.png)

## <a name="part-7-delete-a-customer"></a>第 7 部分：删除客户

删除客户是您需要实现的最后一个基本操作。 当您删除所选数据网格中的客户时，你将想要立即调用**UpdateCustomersAsync**以更新 UI。 但是，不需要调用该方法，如果你删除刚创建的客户。

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

2. 在中**Views\CustomerListPage.xaml**，更新将添加新客户，使其包含第二个按钮的堆栈面板：

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

3. 在中**ViewModels\CustomerListPageViewModel.cs**，更新**DeleteNewCustomerAsync**方法来删除新的客户：

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

4. 运行你的应用。 现在可以删除在数据网格中或堆叠面板中的客户。

![删除新客户](images/customer-database-tutorial/delete-new-customer.png)

## <a name="conclusion"></a>结束语

祝贺你！ 所有此操作完成后，您的应用程序现在有一系列完整的本地数据库操作。 可以创建、 读取、 更新和删除应用 UI 中的客户和这些更改保存到你的数据库，并且将在您的应用程序的不同启动中保持原样。

现在，完成后，考虑以下方面：
* 如果你尚未这样做，请查看[应用程序结构概述](../enterprise/customer-database-app-structure.md)的原因的详细信息如何生成应用。
* 探索[完整的客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)查看本教程的基础的应用。

或者，如果您为一项挑战，您可以及更高版本继续...

## <a name="going-further-connect-to-a-remote-database"></a>深入了解：连接到远程数据库

我们提供了如何实现这些调用对本地 SQLite 数据库的分步演练。 但如果你想要改为使用远程数据库？

如果你想要试一试，将需要您自己的 Azure Active Directory (AAD) 帐户和承载数据源的能力。

你将需要添加身份验证，处理 REST 调用，然后创建要与之交互的远程数据库的函数。 没有代码的完整[客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)，可以引用的每个所需的操作。

### <a name="settings-and-configuration"></a>设置和配置

连接到远程数据库的必要步骤中将逐一[示例的自述文件](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/README.md)。 你将需要执行以下操作：

* 提供你的 Azure 帐户的客户端 Id 向[Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)。
* 提供到的远程数据库的 url [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)。
* 提供用于为数据库连接字符串[Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)。
* 将您的应用程序与 Microsoft Store 相关联。
* 通过复制[服务项目](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoService)到你的应用，并将其部署到 Azure。

### <a name="authentication"></a>身份验证

你将需要创建一个按钮以启动身份验证序列和一个弹出窗口或单独的页来收集用户的信息。 一旦你创建的您将需要提供代码，请求用户的信息并使用它来获取访问令牌。 客户订单数据库示例包装来调用 Microsoft Graph **WebAccountManager**库，以获取令牌和处理的 AAD 帐户的身份验证。

* 在中实现的身份验证逻辑[ **AuthenticationViewModel.cs**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/ViewModels/AuthenticationViewModel.cs)。
* 身份验证过程显示在自定义[ **AuthenticationControl.xaml** ](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/UserControls/AuthenticationControl.xaml)控件。

### <a name="rest-calls"></a>REST 调用

无需修改任何代码的我们添加了在本教程中为了实现 REST 调用。 相反，你将需要执行以下操作：

* 创建新的实现**ICustomerRepository**并**ITutorialRepository**接口，实现相同的完成而不是 SQLite 的其余部分的函数集。 将需要进行序列化和反序列化的 JSON，并可将 REST 调用包装在一个单独**HttpHelper**类在需要时。 请参阅[完整的示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoRepository/Rest)了解详细信息。
* 在中**App.xaml.cs**，创建一个新函数以便初始化 REST 存储库，并调用它而不是**SqliteDatabase**时初始化应用程序。 同样，请参阅[完整的示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/App.xaml.cs)。

完成所有这三个步骤后，您应能够对你通过你的应用的 AAD 帐户进行身份验证。 到远程数据库的 REST 调用将替换本地 SQLite 电话，但用户体验应相同。 如果感觉甚至更高，则可以添加设置页，以便允许用户动态两者之间切换。
