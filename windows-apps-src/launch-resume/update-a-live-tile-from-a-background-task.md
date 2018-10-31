---
author: TylerMSFT
title: 使用后台任务更新动态磁贴
description: 使用后台任务，以最新内容更新应用的动态磁贴。
Search.SourceType: Video
ms.assetid: 9237A5BD-F9DE-4B8C-B689-601201BA8B9A
ms.author: twhitney
ms.date: 01/11/2018
ms.topic: article
keywords: windows 10，uwp，后台任务
ms.localizationpriority: medium
ms.openlocfilehash: 3c379097efaef65357bc1c6b036695ef84671ea6
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5825980"
---
# <a name="update-a-live-tile-from-a-background-task"></a>通过后台任务更新动态磁贴

**重要的 API**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)

使用后台任务，以最新内容更新应用的动态磁贴。

下面的视频演示如何将动态磁贴添加到应用。

<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Updating-a-live-tile-from-a-background-task/player" width="720" height="405" allowFullScreen="true" frameBorder="0"></iframe>

## <a name="create-the-background-task-project"></a>创建后台任务项目  

要为应用启用动态磁贴，请向你的解决方案中添加一个新的 Windows 运行时组件项目。 这是一个独立程序集，当用户安装你的应用时，OS 需要在后台加载并运行该程序集。

1.  在“解决方案资源管理器”中，右键单击该解决方案、单击**添加**，然后单击**新建项目**。
2.  在**添加新项目**对话框的**已安装 &gt; 其他语言 &gt; Visual C# &gt; Windows Universal** 部分中，选择 **Windows 运行时组件**模板。
3.  将项目命名为 BackgroundTasks，然后单击或点击“确定”****。 Microsoft Visual Studio 即会将这个新项目添加到该解决方案。
4.  在主项目中，向 BackgroundTasks 项目添加一个引用。

## <a name="implement-the-background-task"></a>实施后台任务


实现 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 接口，以创建用于更新应用动态磁贴的类。 后台工作将采用 Run 方法。 在这种情况下，该任务将收到一个 MSDN 博客联合源。 为了防止该任务在异步代码仍在运行时过早关闭，请延期执行。

1.  在解决方案资源管理器中，将自动生成的文件 Class1.cs 重命名为 BlogFeedBackgroundTask.cs。
2.  在 BlogFeedBackgroundTask.cs 中，将自动生成的代码替换为 **BlogFeedBackgroundTask** 类的存根代码。
3.  在 Run 方法的实现过程中，添加 **GetMSDNBlogFeed** 和 **UpdateTile** 方法的代码。

```cs
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

// Added during quickstart
using Windows.ApplicationModel.Background;
using Windows.Data.Xml.Dom;
using Windows.UI.Notifications;
using Windows.Web.Syndication;

namespace BackgroundTasks
{
    public sealed class BlogFeedBackgroundTask  : IBackgroundTask
    {
        public async void Run( IBackgroundTaskInstance taskInstance )
        {
            // Get a deferral, to prevent the task from closing prematurely
            // while asynchronous code is still running.
            BackgroundTaskDeferral deferral = taskInstance.GetDeferral();

            // Download the feed.
            var feed = await GetMSDNBlogFeed();

            // Update the live tile with the feed items.
            UpdateTile( feed );

            // Inform the system that the task is finished.
            deferral.Complete();
        }

        private static async Task<SyndicationFeed> GetMSDNBlogFeed()
        {
            SyndicationFeed feed = null;

            try
            {
                // Create a syndication client that downloads the feed.  
                SyndicationClient client = new SyndicationClient();
                client.BypassCacheOnRetrieve = true;
                client.SetRequestHeader( customHeaderName, customHeaderValue );

                // Download the feed.
                feed = await client.RetrieveFeedAsync( new Uri( feedUrl ) );
            }
            catch( Exception ex )
            {
                Debug.WriteLine( ex.ToString() );
            }

            return feed;
        }

        private static void UpdateTile( SyndicationFeed feed )
        {
            // Create a tile update manager for the specified syndication feed.
            var updater = TileUpdateManager.CreateTileUpdaterForApplication();
            updater.EnableNotificationQueue( true );
            updater.Clear();

            // Keep track of the number feed items that get tile notifications.
            int itemCount = 0;

            // Create a tile notification for each feed item.
            foreach( var item in feed.Items )
            {
                XmlDocument tileXml = TileUpdateManager.GetTemplateContent( TileTemplateType.TileWideText03 );

                var title = item.Title;
                string titleText = title.Text == null ? String.Empty : title.Text;
                tileXml.GetElementsByTagName( textElementName )[0].InnerText = titleText;

                // Create a new tile notification.
                updater.Update( new TileNotification( tileXml ) );

                // Don't create more than 5 notifications.
                if( itemCount++ > 5 ) break;
            }
        }

        // Although most HTTP servers do not require User-Agent header, others will reject the request or return
        // a different response if this header is missing. Use SetRequestHeader() to add custom headers.
        static string customHeaderName = "User-Agent";
        static string customHeaderValue = "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";

        static string textElementName = "text";
        static string feedUrl = @"http://blogs.msdn.com/b/MainFeed.aspx?Type=BlogsOnly";
    }
}
```

## <a name="set-up-the-package-manifest"></a>设置包清单


若要设置包清单，请打开它并添加一个新的后台任务声明。 将该任务的入口点设置为类名称，包括其命名空间。

1.  在解决方案资源管理器中，打开“Package.appxmanifest”。
2.  单击或点击“声明”**** 选项卡。
3.  在“可用声明”**** 下，选择BackgroundTasks****，然后单击“添加”****。 Visual Studio 即会将BackgroundTasks**** 添加到“支持的声明”**** 下。
4.  在“支持的任务类型”**** 下，确保已选中“计时器”****。
5.  在“应用设置”**** 下，将入口点设置为BackgroundTasks.BlogFeedBackgroundTask****。
6.  单击或点击“应用程序 UI”**** 选项卡。
7.  将“锁屏界面通知”**** 设置为“锁屏提醒和磁贴文本”****。
8.  在“锁屏提醒徽标”**** 字段中，将路径设置为 24x24 像素图标。
    **重要提示**此图标必须使用单色透明像素。
9.  在“小徽标”**** 字段中，将路径设置为 30x30 像素图标。
10. 在“宽徽标”**** 字段中，将路径设置为 310x150 像素图标。

## <a name="register-the-background-task"></a>注册后台任务


创建 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 以注册你的任务。

> **注意**从 Windows8.1 开始，后台任务注册参数验证注册的时间。 如果有任何注册参数无效，则会返回一个错误。 你的应用必须能够处理后台任务注册失败的情况，例如，使用条件语句检查注册错误，然后使用其他参数值重试失败的注册。
 

在应用的主页中，添加 **RegisterBackgroundTask** 方法并在 **OnNavigatedTo** 事件处理程序中进行调用。

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Windows.ApplicationModel.Background;
using Windows.Data.Xml.Dom;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;
using Windows.Web.Syndication;

// The Blank Page item template is documented at http://go.microsoft.com/fwlink/p/?LinkID=234238

namespace ContosoApp
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }

        /// <summary>
        /// Invoked when this page is about to be displayed in a Frame.
        /// </summary>
        /// <param name="e">Event data that describes how this page was reached.  The Parameter
        /// property is typically used to configure the page.</param>
        protected override void OnNavigatedTo( NavigationEventArgs e )
        {
            this.RegisterBackgroundTask();
        }


        private async void RegisterBackgroundTask()
        {
            var backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();
            if( backgroundAccessStatus == BackgroundAccessStatus.AllowedMayUseActiveRealTimeConnectivity ||
                backgroundAccessStatus == BackgroundAccessStatus.AllowedWithAlwaysOnRealTimeConnectivity )
            {
                foreach( var task in BackgroundTaskRegistration.AllTasks )
                {
                    if( task.Value.Name == taskName )
                    {
                        task.Value.Unregister( true );
                    }
                }

                BackgroundTaskBuilder taskBuilder = new BackgroundTaskBuilder();
                taskBuilder.Name = taskName;
                taskBuilder.TaskEntryPoint = taskEntryPoint;
                taskBuilder.SetTrigger( new TimeTrigger( 15, false ) );
                var registration = taskBuilder.Register();
            }
        }

        private const string taskName = "BlogFeedBackgroundTask";
        private const string taskEntryPoint = "BackgroundTasks.BlogFeedBackgroundTask";
    }
}
```

## <a name="debug-the-background-task"></a>调试后台任务


若要调试后台任务，请在该任务的 Run 方法中设置一个断点。 在“调试位置”**** 工具栏中，选择你的后台任务。 这将导致系统立即调用 Run 方法。

1.  在该任务的 Run 方法中设置一个断点。
2.  按 F5 或点击“调试”&gt;“开始调试”**** 以部署和运行该应用。
3.  应用启动后，切换回 Visual Studio。
4.  确保显示“调试位置”**** 工具栏。 该工具栏位于“查看”&gt;“工具栏”**** 菜单上。
5.  在“调试位置”**** 工具栏上，单击“暂停”**** 下拉菜单，然后选择BlogFeedBackgroundTask****。
6.  Visual Studio 会在断点位置暂停执行。
7.  按 F5 或点击“调试”&gt;“继续”**** 以继续运行该应用。
8.  按 Shift+F5 或点击“调试”&gt;“停止调试”**** 以停止调试。
9.  返回到“开始”屏幕上的该应用磁贴。 几秒钟后，你的应用磁贴上将会显示磁贴通知。

## <a name="related-topics"></a>相关主题


* [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
* [**TileUpdateManager**](https://msdn.microsoft.com/library/windows/apps/br208622)
* [**TileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616)
* [使用后台任务支持应用](support-your-app-with-background-tasks.md)
* [磁贴和锁屏提醒指南和清单](https://msdn.microsoft.com/library/windows/apps/hh465403)

 

 
