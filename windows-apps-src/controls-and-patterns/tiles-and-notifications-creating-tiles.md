---
磁贴是应用在“开始”菜单上的表示形式。 每个应用都有一个磁贴。 在 Microsoft Visual Studio 中创建新的通用 Windows 平台 (UWP) 应用项目时，它将包含显示应用名称和徽标的默认磁贴。
磁贴
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
磁贴
template: detail.hbs
---

# 适用于 UWP 应用的磁贴


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


*磁贴*是应用在“开始”菜单上的表示形式。 每个应用都有一个磁贴。 在 Microsoft Visual Studio 中创建新的通用 Windows 平台 (UWP) 应用项目时，它将包含显示应用名称和徽标的默认磁贴。 应用首次安装时，Windows 将显示此磁贴。 应用安装后，可通过通知更改磁贴内容；例如，可更改磁贴以向用户传达新信息（如头条新闻或最近未读邮件的主题）。

## <span id="Configure_the_default_tile"> </span> <span id="configure_the_default_tile"> </span> <span id="CONFIGURE_THE_DEFAULT_TILE"> </span>配置默认磁贴


当在 Visual Studio 中创建新项目时，它将创建可显示应用名称和徽标的简单默认磁贴。

```XML
  <Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="ExampleApp.App">
      <uap:VisualElements
        DisplayName="ExampleApp"
        Square150x150Logo="Assets\Logo.png"
        Square44x44Logo="Assets\SmallLogo.png"
        Description="ExampleApp"
        BackgroundColor="#464646">
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
    </Application>
  </Applications>
```

应更新以下项：

-   DisplayName：将要在磁贴上显示的名称替换此值。
-   ShortName：由于磁贴上容纳显示名称的空间有限，我们也建议指定 ShortName，以确保应用名称不会被截断。
-   徽标图像：

    应用自己的图像替换这些图像。 可选择为不同的缩放提供图像，但无需为所有缩放提供。 若要确保应用在一系列设备上具有不俗的外观，我们建议针对每个图像提供其 100%、200% 和 400% 缩放的版本。

    缩放的图像应遵循此命名约定：
    
    *<image name>*.scale-*<scale factor>*.*<image file extension>* 


     

    For example: SmallLogo.scale-100.png

    When you refer to the image, you refer to it as *&lt;image name&gt;*.*&lt;image file extension&gt;* ("SmallLogo.png" in this example). The system will automatically select the appropriate scaled image for the device from the images you've provided.

-   虽然不是必须的，但我们强烈建议你提供适用于宽磁贴和大磁贴的徽标，以便用户可以将应用磁贴重新调整到这些大小。 若要提供这些附加图像，可创建 `DefaultTile` 元素并使用 `Wide310x150Logo` 和 `Square310x310Logo` 属性指定附加图像：
```    XML
  <Applications>
        <Application Id="App"
          Executable="$targetnametoken$.exe"
          EntryPoint="ExampleApp.App">
          <uap:VisualElements
            DisplayName="ExampleApp"
            Square150x150Logo="Assets\Logo.png"
            Square44x44Logo="Assets\SmallLogo.png"
            Description="ExampleApp"
            BackgroundColor="#464646">
            <uap:DefaultTile
              Wide310x150Logo="Assets\WideLogo.png"
              Square310x310Logo="Assets\LargeLogo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
      </Applications>
```

## <span id="Use_notifications_to_customize_your_tile"> </span> <span id="use_notifications_to_customize_your_tile"> </span> <span id="USE_NOTIFICATIONS_TO_CUSTOMIZE_YOUR_TILE"> </span>使用通知自定义磁贴


应用安装后，可使用通知自定义磁贴。 可在首次启动应用或响应某个事件（如推送通知）时执行此操作。

1.  创建描述磁贴的 XML 负载（以 [**Windows.Data.Xml.Dom.XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br206173) 的形式）。

    -   Windows 10 引入了可供你使用的新自适应磁贴架构。 有关说明，请参阅[自适应磁贴](tiles-and-notifications-create-adaptive-tiles.md)。 有关架构信息，请参阅[自适应磁贴架构](tiles-and-notifications-adaptive-tiles-schema.md)。 

    -   可使用 Windows 8.1 磁贴模板定义磁贴。 有关详细信息，请参阅[创建磁贴和锁屏提醒 (Windows 8.1)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868260)。

2.  创建磁贴通知对象并将其传递到所创建的 [**XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br206173)。 通知对象分为以下几类：
    -   可立即更新磁贴的 [**Windows.UI.NotificationsTileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616) 对象。
    -   在未来某个时间更新磁贴的 [**Windows.UI.Notifications.ScheduledTileNotification**](https://msdn.microsoft.com/library/windows/apps/hh701637) 对象。

3.  使用 [**Windows.UI.Notifications.TileUpdateManager.CreateTileUpdaterForApplication**](https://msdn.microsoft.com/library/windows/apps/br208623) 创建 [**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628) 对象。
4.  调用 [**TileUpdater.Update**](https://msdn.microsoft.com/library/windows/apps/br208632) 方法，并将其传递到在步骤 2 中创建的磁贴通知对象。

 

 






<!--HONumber=Mar16_HO1-->


