---
author: normesta
Description: Share code between a desktop application and a UWP app
Search.Product: eADQiWindows 10XVcnh
title: 桌面应用程序和 UWP 应用之间共享代码
ms.author: normesta
ms.date: 10/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ca5b722ea97202d57f05613bec88ae6bee1db5f2
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "5159345"
---
# <a name="share-code-between-a-desktop-application-and-a-uwp-app"></a>桌面应用程序和 UWP 应用之间共享代码

你可以将代码迁移到 .NET Standard 库中，然后创建通用 Windows 平台 (UWP) 应用以覆盖所有 Windows 10 设备。 虽然没有工具可以将桌面应用程序转换为 UWP 应用，但是，你可以重复使用大量现有代码来降低构建代码的成本。 本指南演示如何执行该操作。

## <a name="share-code-in-a-net-standard-20-library"></a>在 .NET Standard 2.0 库中共享代码

将尽可能多的代码放入 .NET Standard 2.0 类库中。  只要你的代码使用标准中定义的 API，就可以在 UWP 应用中重复使用你的代码。 在 .NET Standard 库中共享代码比以前更容易，因为 .NET Standard 2.0 中包含的 API 相当多。

下面的精彩视频向你介绍相关的详细信息。
&nbsp;
> [!VIDEO https://www.youtube.com/embed/YI4MurjfMn8]

### <a name="add-net-standard-libraries"></a>添加 .NET Standard 库

首先，将一个或多个 .NET Standard 类库添加到你的解决方案中。  

![添加 dotnet 标准项目](images/desktop-to-uwp/dot-net-standard-project-template.png)

根据你打算如何组织你的代码来决定要往解决方案中添加的库的数量。

请确保每个类库都面向 **.NET Standard 2.0**。

![面向 .NET Standard 2.0](images/desktop-to-uwp/target-standard-20.png)

你可以在类库项目的属性页中查找此设置。

从你的桌面应用程序项目中，添加对类库项目的引用。

![类库参考](images/desktop-to-uwp/class-library-reference.png)

接下来，使用工具确定符合标准的代码量。 这样，在将代码移到库中之前，可以确定哪些部分可以重复使用、哪些部分需要最少量的修改，以及哪些部分将仍然特定于应用程序。

### <a name="check-library-and-code-compatibility"></a>检查库和代码的兼容性

我们将从 Nuget 包以及从第三方获得的其他 dll 文件开始。

如果你的应用程序使用其中任何一项，请确定它们是否与 .NET Standard 2.0 兼容。 你可以使用 Visual Studio 扩展或命令行实用工具执行该操作。

使用这些相同的工具分析你的代码。 在此处下载工具 ([dotnet-apiport](https://github.com/Microsoft/dotnet-apiport/releases))，然后观看此视频以了解如何使用它们。
&nbsp;
> [!VIDEO https://www.youtube.com/embed/rzs_FGPyAlY]

如果你的代码不符合此标准，请考虑其他可以实现该代码的方法。 首先打开 [.NET API 浏览器](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0)。 你可以使用该浏览器查看 .NET Standard 2.0 中可用的 API。 请确保将列表作用域限定为 .NET Standard 2.0。

![.NET 选项](images/desktop-to-uwp/dot-net-option.png)

某些代码将特定于平台，并且将需要保留在你的桌面应用程序项目中。

### <a name="example-migrating-data-access-code-to-a-net-standard-20-library"></a>示例：将数据访问代码迁移到 .NET Standard 2.0 库

假设我们有的非常基本的 Windows 窗体应用程序显示我们 Northwind 示例数据库中的客户。

![Windows 窗体应用](images/desktop-to-uwp/win-forms-app.png)

项目包含一个 .NET Standard 2.0 类库，其中包含名为 **Northwind** 的静态类。 如果我们将此代码移到 **Northwind** 类中，它将不进行编译，因为它使用 ``SQLConnection``、``SqlCommand`` 和 ``SqlDataReader``类，以及 .NET Standard 2.0 中不可用的那些类。

```csharp
public static ArrayList GetCustomerNames()
{
    ArrayList customers = new ArrayList();

    using (SqlConnection conn = new SqlConnection())
    {
        conn.ConnectionString =
            @"Data Source=" +
            @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        SqlCommand command = new SqlCommand("select ContactName from customers order by ContactName asc", conn);

        using (SqlDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}

```
不过，我们可以使用 [.NET API 浏览器](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0) 找到另一种方法。 我们可以使用 ``DbConnection``、``DbCommand`` 和 ``DbDataReader`` 类，这些类都在 .NET Standard 2.0 中可用。  

此修订版本使用这些类来获取客户列表，但是，若要创建 ``DbConnection`` 类，我们将需要传递在客户端应用程序中创建的工厂对象。

```csharp
public static ArrayList GetCustomerNames(DbProviderFactory factory)
{
    ArrayList customers = new ArrayList();

    using (DbConnection conn = factory.CreateConnection())
    {
        conn.ConnectionString = @"Data Source=" +
                        @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        DbCommand command = factory.CreateCommand();
        command.Connection = conn;
        command.CommandText = "select ContactName from customers order by ContactName asc";

        using (DbDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}
```  

在 Windows 窗体的代码隐藏页面中，我们可以创建工厂实例并将其传递到我们的方法中。

```csharp
public partial class Customers : Form
{
    public Customers()
    {
        InitializeComponent();

        dataGridView1.Rows.Clear();

        SqlClientFactory factory = SqlClientFactory.Instance;

        foreach (string customer in Northwind.GetCustomerNames(factory))
        {
            dataGridView1.Rows.Add(customer);
        }
    }
}
```

## <a name="reach-all-windows-devices"></a>覆盖所有 Windows 设备

现在，你可以将 UWP 应用添加到你的解决方案中。

![桌面到 UWP 桥图像](images/desktop-to-uwp/adaptive-ui.png)

你仍然必须在 XAML 中设计 UI 页面，并编写任何特定于设备或平台的代码，但是当你完成后，你将能够覆盖所有 Windows 10 设备，并且应用页面将具有一种现代感，完全能够适应不同的屏幕大小和分辨率。

你的应用将响应输入机制，而不仅仅是键盘和鼠标，并且功能和设置在各个设备中都将非常直观。 这意味着用户只需了解一次如何执行操作，应用就能以用户非常熟悉的方式运作，不管使用的是何种设备。

这些只是 UWP 带来的一小部分优势。 若要了解详细信息，请参阅[使用 Windows 打造出色的体验](https://developer.microsoft.com/windows/why-build-for-uwp)。

### <a name="add-a-uwp-project"></a>添加 UWP 项目

首先，向你的解决方案中添加一个 UWP 项目。

![UWP 项目](images/desktop-to-uwp/new-uwp-app.png)

然后，从你的 UWP 项目中，添加对 .NET Standard 2.0 库项目的引用。

![类库参考](images/desktop-to-uwp/class-library-reference2.png)

### <a name="build-your-pages"></a>生成页面

添加 XAML 页面并调用 .NET Standard 2.0 库中的代码。

![UWP 应用](images/desktop-to-uwp/uwp-app.png)

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel x:Name="customerStackPanel">
        <ListView x:Name="customerList"/>
    </StackPanel>
</Grid>
```

```csharp
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        SqlClientFactory factory = SqlClientFactory.Instance;

        customerList.ItemsSource = Northwind.GetCustomerNames(factory);
    }
}
```


要开始使用 UWP，请参阅[什么是 UWP 应用](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)。

## <a name="reach-ios-and-android-devices"></a>覆盖 iOS 和 Android 设备

通过添加 Xamarin 项目，你可以覆盖 Android 和 iOS 设备。  

![Xamarin 应用](images/desktop-to-uwp/xamarin-apps.png)

利用这些项目，你可以使用 C# 构建 Android 和 iOS 应用，这些应用能够完全访问特定于平台和特定于设备的 API。 这些应用可充分利用特定于平台的硬件加速性能，并且是为本机性能而编译的。

它们有权访问基础平台和设备公开的全套功能，包括特定于平台的功能，如 iBeacons 和 Android Fragments，并且你将使用标准本机用户界面控件来构建外观符合用户预期的 UI。

就像 UWP 一样，添加 Android 或 iOS 应用的成本较低，因为你可以重复使用 .NET Standard 2.0 类库中的业务逻辑。 你将必须在 XAML 中设计 UI 页面，并编写任何特定于设备或平台的代码。

### <a name="add-a-xamarin-project"></a>添加 Xamarin 项目

首先，将 **Android**、**iOS** 或**跨平台**项目添加到你的解决方案中。

你可以在 **Visual C#** 组下面的**添加新项目**对话框中找到这些模板。

![Xamarin 应用](images/desktop-to-uwp/xamarin-projects.png)

>[!NOTE]
>跨平台项目非常适合具有极少特定于平台的功能的应用。 你可以使用它们构建一个在 iOS、Android 和 Windows 上运行的基于 XAML 的本机 UI。 在[此处](https://www.xamarin.com/forms)了解详细信息。

然后，从你的 Android、iOS 或跨平台项目中，添加对类库项目的引用。

![类库参考](images/desktop-to-uwp/class-library-reference3.png)

### <a name="build-your-pages"></a>生成页面

我们的示例显示了 Android 应用中的客户列表。

![Android 应用](images/desktop-to-uwp/android-app.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp" android:textSize="16sp"
    android:id="@android:id/list">
</TextView>
```

```csharp
[Activity(Label = "MyAndroidApp", MainLauncher = true)]
public class MainActivity : ListActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SqlClientFactory factory = SqlClientFactory.Instance;

        var customers = (string[])Northwind.GetCustomerNames(factory).ToArray(typeof(string));

        ListAdapter = new ArrayAdapter<string>(this, Resource.Layout.list_item, customers);
    }
}
```

若要开始使用 Android、iOS 和跨平台项目，请参阅 [Xamarin 开发人员门户](https://developer.xamarin.com/)。

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
