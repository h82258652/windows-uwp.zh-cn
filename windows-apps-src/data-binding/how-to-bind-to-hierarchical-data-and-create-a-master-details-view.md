---
ms.assetid: 0C69521B-47E0-421F-857B-851B0E9605F2
绑定分层数据和创建主视图/详细信息视图
你可以通过将项目控件绑定到 CollectionViewSource 实例（它们绑定在同一个链中），来生成分层数据的多级主视图/详细信息视图（也称为列表详细信息视图）。
---
# 绑定分层数据和创建大纲/细节视图

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


> **注意** 另请参阅[大纲/细节示例](http://go.microsoft.com/fwlink/p/?linkid=619991)。

你可以通过将项目控件绑定到 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 实例（它们绑定在同一个链中），来生成分层数据的多级主视图/详细信息视图（也称为列表详细信息视图）。 在本主题中，我们将尽可能使用 [{x:Bind} 标记扩展](https://msdn.microsoft.com/library/windows/apps/Mt204783)，并根据需要使用更为灵活（但性能较低）的 [{Binding} 标记扩展](https://msdn.microsoft.com/library/windows/apps/Mt204782)。

通用 Windows 平台 (UWP) 应用的一个常见结构就是当用户在主列表中进行选择时导航到不同的详细信息页面。 当你想提供某个层次结构中每个级别上每个项目的丰富视觉表示时，该功能非常有用。 另一种选择是在一个页面上显示多级数据。 当你要显示几个简单的列表，方便用户快速下拉到感兴趣的项目时，该功能非常有用。 本主题介绍如何实现此交互。 [
            **CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 实例将跟踪各分层级别的当前选择。

我们将创建一个有关运动团队的层次结构视图，它分为联盟、分支和团队的列表，并包含团队详细信息视图。 当你从任一列表中选择一个项目时，会自动更新后续视图。

![体育运动层次结构的主视图/详细信息视图](images/xaml-masterdetails.png)

## 先决条件

此主题假设你知晓如何创建基本的 UWP 应用。 有关创建你的第一个 UWP 应用的说明，请参阅[使用 C# 或 Visual Basic 创建你的第一个 UWP 应用](https://msdn.microsoft.com/library/windows/apps/Hh974581)。

## 创建项目

创建一个新的**“空白应用程序(Windows 通用)”**项目。 将其命名为“MasterDetailsBinding”。

## 创建数据模型

向项目添加一个新类、将其命名为 ViewModel.cs，并向其中添加此代码。 这将是你的绑定源类。

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace MasterDetailsBinding
{
    public class Team
    {
        public string Name { get; set; }
        public int Wins { get; set; }
        public int Losses { get; set; }
    }

    public class Division
    {
        public string Name { get; set; }
        public IEnumerable&lt;Team&gt; Teams { get; set; }
    }

    public class League
    {
        public string Name { get; set; }
        public IEnumerable&lt;Division&gt; Divisions { get; set; }
    }

    public class LeagueList : List&lt;League&gt;
    {
        public LeagueList()
        {
            this.AddRange(GetLeague().ToList());
        }

        public IEnumerable&lt;League&gt; GetLeague()
        {
            return from x in Enumerable.Range(1, 2)
                   select new League
                   {
                       Name = &quot;League &quot; + x,
                       Divisions = GetDivisions(x).ToList()
                   };
        }

        public IEnumerable&lt;Division&gt; GetDivisions(int x)
        {
            return from y in Enumerable.Range(1, 3)
                   select new Division
                   {
                       Name = String.Format(&quot;Division {0}-{1}&quot;, x, y),
                       Teams = GetTeams(x, y).ToList()
                   };
        }

        public IEnumerable&lt;Team&gt; GetTeams(int x, int y)
        {
            return from z in Enumerable.Range(1, 4)
                   select new Team
                   {
                       Name = String.Format(&quot;Team {0}-{1}-{2}&quot;, x, y, z),
                       Wins = 25 - (x * y * z),
                       Losses = x * y * z
                   };
        }
    }
}
```

## 创建视图

接下来，从表示标记页的类公开绑定源类。 我们通过将类型 **LeagueList** 的属性添加到 **MainPage** 来执行此操作。

```cs
namespace MasterDetailsBinding
{
    /// &lt;summary&gt;
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// &lt;/summary&gt;
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new LeagueList();
        }
        public LeagueList ViewModel { get; set; }
    }
}
```

最后，用以下标记来替代 MainPage.xaml 文件的内容，该标记声明了三个 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 实例并将其绑定到同一链中。 后续控件可以根据其在层次结构中的级别绑定到相应的 **CollectionViewSource**。

```xaml
<Page
    x:Class=&quot;MasterDetailsBinding.MainPage&quot;
    xmlns=&quot;http://schemas.microsoft.com/winfx/2006/xaml/presentation&quot;
    xmlns:x=&quot;http://schemas.microsoft.com/winfx/2006/xaml&quot;
    xmlns:local=&quot;using:MasterDetailsBinding&quot;
    xmlns:d=&quot;http://schemas.microsoft.com/expression/blend/2008&quot;
    xmlns:mc=&quot;http://schemas.openxmlformats.org/markup-compatibility/2006&quot;
    mc:Ignorable=&quot;d&quot;>

    <Page.Resources>
        <CollectionViewSource x:Name=&quot;Leagues&quot;
            Source=&quot;{x:Bind ViewModel}&quot;/>
        <CollectionViewSource x:Name=&quot;Divisions&quot;
            Source=&quot;{Binding Divisions, Source={StaticResource Leagues}}&quot;/>
        <CollectionViewSource x:Name=&quot;Teams&quot;
            Source=&quot;{Binding Teams, Source={StaticResource Divisions}}&quot;/>

        <Style TargetType=&quot;TextBlock&quot;>
            <Setter Property=&quot;FontSize&quot; Value=&quot;15&quot;/>
            <Setter Property=&quot;FontWeight&quot; Value=&quot;Bold&quot;/>
        </Style>

        <Style TargetType=&quot;ListBox&quot;>
            <Setter Property=&quot;FontSize&quot; Value=&quot;15&quot;/>
        </Style>

        <Style TargetType=&quot;ContentControl&quot;>
            <Setter Property=&quot;FontSize&quot; Value=&quot;15&quot;/>
        </Style>

    </Page.Resources>

    <Grid Background=&quot;{ThemeResource ApplicationPageBackgroundThemeBrush}&quot;>

        <StackPanel Orientation=&quot;Horizontal&quot;>

            <!-- All Leagues view -->

            <StackPanel Margin=&quot;5&quot;>
                <TextBlock Text=&quot;All Leagues&quot;/>
                <ListBox ItemsSource=&quot;{Binding Source={StaticResource Leagues}}&quot; 
                    DisplayMemberPath=&quot;Name&quot;/>
            </StackPanel>

            <!-- League/Divisions view -->

            <StackPanel Margin=&quot;5&quot;>
                <TextBlock Text=&quot;{Binding Name, Source={StaticResource Leagues}}&quot;/>
                <ListBox ItemsSource=&quot;{Binding Source={StaticResource Divisions}}&quot; 
                    DisplayMemberPath=&quot;Name&quot;/>
            </StackPanel>

            <!-- Division/Teams view -->

            <StackPanel Margin=&quot;5&quot;>
                <TextBlock Text=&quot;{Binding Name, Source={StaticResource Divisions}}&quot;/>
                <ListBox ItemsSource=&quot;{Binding Source={StaticResource Teams}}&quot; 
                    DisplayMemberPath=&quot;Name&quot;/>
            </StackPanel>

            <!-- Team view -->

            <ContentControl Content=&quot;{Binding Source={StaticResource Teams}}&quot;>
                <ContentControl.ContentTemplate>
                    <DataTemplate>
                        <StackPanel Margin=&quot;5&quot;>
                            <TextBlock Text=&quot;{Binding Name}&quot; 
                                FontSize=&quot;15&quot; FontWeight=&quot;Bold&quot;/>
                            <StackPanel Orientation=&quot;Horizontal&quot; Margin=&quot;10,10&quot;>
                                <TextBlock Text=&quot;Wins:&quot; Margin=&quot;0,0,5,0&quot;/>
                                <TextBlock Text=&quot;{Binding Wins}&quot;/>
                            </StackPanel>
                            <StackPanel Orientation=&quot;Horizontal&quot; Margin=&quot;10,0&quot;>
                                <TextBlock Text=&quot;Losses:&quot; Margin=&quot;0,0,5,0&quot;/>
                                <TextBlock Text=&quot;{Binding Losses}&quot;/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ContentControl.ContentTemplate>
            </ContentControl>

        </StackPanel>

    </Grid>
</Page>
```

请注意，如果直接绑定到 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833)，即表示你希望绑定到绑定控件中的当前项，其中在集合本身上无法找到路径。 无需将 **CurrentItem** 属性指定为绑定路径，尽管当存在歧义时你可以这样做。 例如，[**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365) 表示团队视图的 [**Content**](https://msdn.microsoft.com/library/windows/apps/BR209365-content) 属性被绑定到 `Teams`**CollectionViewSource**。 然而，[**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) 中的控件被绑定到 `Team` 类的属性，因为 **CollectionViewSource** 会自动按需提供团队列表中当前所选的团队。

 

 



<!--HONumber=Mar16_HO1-->


