---
Description: 桌面应用程序和 UWP 应用之间共享代码
title: 桌面应用程序和 UWP 应用之间共享代码
ms.date: 10/03/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 344ee7060edcee3376e271fc21e104490d8724d7
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319713"
---
# <a name="move-from-a-desktop-application-to-uwp"></a>移到 UWP 的桌面应用程序

如果必须使用.NET Framework （包括 WPF 和 Windows 窗体） 构建的现有桌面应用程序或C++Win32 Api，你有多个选项将移动到通用 Windows 平台 (UWP) 和 Windows 10。

## <a name="package-your-desktop-application-in-an-msix-package"></a>MSIX 包中的将桌面应用程序打包

可以打包桌面应用程序 MSIX 包中获取到更多的 Windows 10 功能的访问权限。 MSIX 是一种新式的 Windows 应用包格式，提供所有 Windows 应用（包括 UWP、WPF、Windows 窗体和 Win32 应用）的通用打包体验。 将桌面 Windows 应用打包到 MSIX 包中即可访问可靠的安装和更新体验、功能系统灵活的托管安全模型、对 Microsoft Store 的支持、企业管理以及许多自定义分发模型。 您可以打包应用程序是否有源代码，或如果您只需一个现有的安装程序文件 （如 MSI 或 APP-V 安装程序）。 打包你的应用程序后，可以将集成 UWP 功能，例如打包扩展和其他 UWP 组件。

有关详细信息，请参阅[打包桌面应用程序 （桌面桥）](/windows/msix/desktop/desktop-to-uwp-root)并[要求包标识的功能](/windows/apps/desktop/modernize/modernize-packaged-apps)。

## <a name="use-uwp-apis"></a>使用 UWP Api

可以在 WPF、Windows 窗体或 C++ Win32 桌面应用中直接调用许多 UWP API，以便集成对 Windows 10 用户来说焕然一新的体验。 例如，可以调用 UWP API，以便将 Toast 通知添加到桌面应用。

有关详细信息，请参阅[在桌面应用中使用 UWP API](/windows/apps/desktop/modernize/desktop-to-uwp-enhance)。

## <a name="migrate-a-net-framework-app-to-a-uwp-app"></a>迁移.NET Framework 应用程序向 UWP 应用

在.NET Framework 上运行应用程序，可以直接将其迁移到 UWP 应用，通过利用.NET Standard 2.0。 移动大量的代码如您可以到.NET Standard 2.0 类库，，然后创建 UWP 应用引用.NET Standard 2.0 库。 

### <a name="share-code-in-a-net-standard-20-library"></a>在 .NET Standard 2.0 库中共享代码

如果你的应用程序在.NET Framework 上运行，将太多代码，因为您可以到.NET Standard 2.0 类库。 只要你的代码使用标准中定义的 API，就可以在 UWP 应用中重复使用你的代码。 在 .NET Standard 库中共享代码比以前更容易，因为 .NET Standard 2.0 中包含的 API 相当多。

下面是告诉您有关它的详细信息的视频。

> [!VIDEO https://www.youtube-nocookie.com/embed/YI4MurjfMn8?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=1]

#### <a name="add-net-standard-libraries"></a>添加 .NET Standard 库

首先，将一个或多个 .NET Standard 类库添加到你的解决方案中。  

![添加 dotnet 标准项目](images/desktop-to-uwp/dot-net-standard-project-template.png)

根据你打算如何组织你的代码来决定要往解决方案中添加的库的数量。

请确保每个类库都面向 **.NET Standard 2.0**。

![面向 .NET Standard 2.0](images/desktop-to-uwp/target-standard-20.png)

你可以在类库项目的属性页中查找此设置。

从你的桌面应用程序项目中，添加对类库项目的引用。

![类库参考](images/desktop-to-uwp/class-library-reference.png)

接下来，使用工具确定符合标准的代码量。 这样，在将代码移到库中之前，可以确定哪些部分可以重复使用、哪些部分需要最少量的修改，以及哪些部分将仍然特定于应用程序。

#### <a name="check-library-and-code-compatibility"></a>检查库和代码的兼容性

我们将从 Nuget 包以及从第三方获得的其他 dll 文件开始。

如果你的应用程序使用其中任何一项，请确定它们是否与 .NET Standard 2.0 兼容。 你可以使用 Visual Studio 扩展或命令行实用工具执行该操作。

使用这些相同的工具分析你的代码。 在此处下载工具 ([dotnet-apiport](https://github.com/Microsoft/dotnet-apiport/releases))，然后观看此视频以了解如何使用它们。
&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/rzs_FGPyAlY?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=2]

如果你的代码不符合此标准，请考虑其他可以实现该代码的方法。 首先打开 [.NET API 浏览器](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0)。 你可以使用该浏览器查看 .NET Standard 2.0 中可用的 API。 请确保将列表作用域限定为 .NET Standard 2.0。

![.NET 选项](images/desktop-to-uwp/dot-net-option.png)

某些代码将特定于平台，并且将需要保留在你的桌面应用程序项目中。

#### <a name="example-migrating-data-access-code-to-a-net-standard-20-library"></a>例如：迁移到.NET Standard 2.0 库的数据访问代码

假设我们有一个非常基本的 Windows 窗体应用程序显示了我们 Northwind 示例数据库中的客户。

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

### <a name="create-a-uwp-app"></a>创建 UWP 应用

现在，你可以将 UWP 应用添加到你的解决方案中。

![桌面到 UWP 桥图像](images/desktop-to-uwp/adaptive-ui.png)

你仍然必须在 XAML 中设计 UI 页面，并编写任何特定于设备或平台的代码，但是当你完成后，你将能够覆盖所有 Windows 10 设备，并且应用页面将具有一种现代感，完全能够适应不同的屏幕大小和分辨率。

你的应用将响应输入机制，而不仅仅是键盘和鼠标，并且功能和设置在各个设备中都将非常直观。 这意味着用户只需了解一次如何执行操作，应用就能以用户非常熟悉的方式运作，不管使用的是何种设备。

这些只是 UWP 带来的一小部分优势。 若要了解详细信息，请参阅[使用 Windows 打造出色的体验](https://developer.microsoft.com/windows/why-build-for-uwp)。

#### <a name="add-a-uwp-project"></a>添加 UWP 项目

首先，向你的解决方案中添加一个 UWP 项目。

![UWP 项目](images/desktop-to-uwp/new-uwp-app.png)

然后，从你的 UWP 项目中，添加对 .NET Standard 2.0 库项目的引用。

![类库参考](images/desktop-to-uwp/class-library-reference2.png)

#### <a name="build-your-pages"></a>生成页面

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

### <a name="reach-ios-and-android-devices"></a>覆盖 iOS 和 Android 设备

通过添加 Xamarin 项目，你可以覆盖 Android 和 iOS 设备。  

![Xamarin 应用](images/desktop-to-uwp/xamarin-apps.png)

利用这些项目，你可以使用 C# 构建 Android 和 iOS 应用，这些应用能够完全访问特定于平台和特定于设备的 API。 这些应用可充分利用特定于平台的硬件加速性能，并且是为本机性能而编译的。

它们有权访问基础平台和设备公开的全套功能，包括特定于平台的功能，如 iBeacons 和 Android Fragments，并且你将使用标准本机用户界面控件来构建外观符合用户预期的 UI。

就像 UWP 一样，添加 Android 或 iOS 应用的成本较低，因为你可以重复使用 .NET Standard 2.0 类库中的业务逻辑。 你将必须在 XAML 中设计 UI 页面，并编写任何特定于设备或平台的代码。

#### <a name="add-a-xamarin-project"></a>添加 Xamarin 项目

首先，将 **Android**、**iOS** 或**跨平台**项目添加到你的解决方案中。

你可以在 **Visual C#** 组下面的**添加新项目**对话框中找到这些模板。

![Xamarin 应用](images/desktop-to-uwp/xamarin-projects.png)

>[!NOTE]
>跨平台项目非常适合具有极少特定于平台的功能的应用。 你可以使用它们构建一个在 iOS、Android 和 Windows 上运行的基于 XAML 的本机 UI。 在[此处](https://docs.microsoft.com/xamarin/xamarin-forms/)了解详细信息。

然后，从你的 Android、iOS 或跨平台项目中，添加对类库项目的引用。

![类库参考](images/desktop-to-uwp/class-library-reference3.png)

#### <a name="build-your-pages"></a>生成页面

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

若要开始使用 Android、iOS 和跨平台项目，请参阅 [Xamarin 开发人员门户](https://docs.microsoft.com/xamarin)。

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
