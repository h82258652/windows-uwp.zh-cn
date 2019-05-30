---
Description: 特殊磁贴模板是独特的模板，可以具有动画效果或只允许你执行自适应磁贴不支持的操作。
title: 特殊磁贴模板
ms.assetid: 1322C9BA-D5B2-45E2-B813-865884A467FF
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 739abc139eabc9f773938f55c15d3e18aaf562ce
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66365961"
---
# <a name="special-tile-templates"></a>特殊磁贴模板
 

特殊磁贴模板是独特的模板，可以具有动画效果或只允许你执行自适应磁贴不支持的操作。 每个特殊的磁贴模板是专门构建，适用于 Windows 10 除外图标磁贴模板中，已更新适用于 Windows 10 的经典特殊模板。 本文介绍了三个特殊的磁贴模板：图标，照片和人员。

## <a name="iconic-tile-template"></a>图标磁贴模板


图标模板（也称为“IconWithBadge”模板）允许你在磁贴中心显示较小的图像。 Windows 10 手机和平板电脑/桌面支持模板。

![小邮件磁贴和中邮件磁贴](images/iconic-template-mail-2sizes.png)

### <a name="how-to-create-an-iconic-tile"></a>如何创建图标磁贴

以下步骤涵盖所有需要了解适用于 Windows 10 创建图标磁贴。 具体而言，你首先需要图标图像资源，然后使用图标模板向磁贴发送通知，最后发送锁屏提醒通知以提供要在磁贴上显示的数目。

![图标磁贴的开发人员流程](images/iconic-template-dev-flow.png)

**步骤 1：创建图像资产、 PNG 格式**

为磁贴创建图标资源，然后将其放置在包含其他资源的项目资源中。 至少创建一个 200x200 像素的图标，这可用于手机和台式机上的小磁贴和中等磁贴。 若要提供最佳用户体验，请为每种大小创建一个图标。 这些资源上不需要填充。 请参阅下图中的调整大小详细信息。

采用 PNG 格式保存为透明的图标资源。 在 Windows Phone 上，每个非透明像素都显示为白色 (RGB 255, 255, 255)。 为了一致性和简单起见，同样为桌面图标使用白色。

平板电脑、笔记本电脑和台式机上的 Windows 10 仅支持方形图标资源。 手机支持方形资源和高度超过宽度的资源，最多支持 2:3 的宽度:高度比，这非常有利于手机图标之类的图像。

![手机和台式机的小磁贴以及中磁贴上的图标大小调整](images/iconic-template-sizing-info.png)

![带有和不带有锁屏提醒的资源的大小调整](images/assetguidance24.png)

对于方形资源，在容器内自动居中放置：

![方形资源大小调整（带有和不带有锁屏提醒）](images/assetguidance25.png)

对于非方形资源，会自动进行水平/垂直居中放置并贴靠到容器的宽度/高度：

![非方形资源大小调整（带有和不带有锁屏提醒）](images/assetguidance26a.png)

![非方形资源大小调整（带有和不带有锁屏提醒）](images/assetguidance26b.png)

**步骤 2：创建基本图块**

你可以在主要磁贴和辅助磁贴上使用图标模板。 如果要在辅助磁贴上使用它，首先需要创建辅助磁贴或使用已固定的辅助磁贴。 主要磁贴已隐式固定，并且始终可以向其发送通知。

**步骤 3：将通知发送到你的磁贴**

尽管此步骤将根据通知的发送方式（通过本地或通过服务器推送）而有所不同，但所发送的 XML 负载依然保持不变。 若要发送本地磁贴通知，请为磁贴（主要磁贴或辅助磁贴）创建 [**TileUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater)，然后向使用图标磁贴模板的磁贴发送通知，如下所示。 理想情况下，还应为使用[自适应磁贴模板](create-adaptive-tiles.md)的宽形磁贴和大磁贴包含绑定。

下面是 XML 负载的示例代码：

```xml
<tile>
  <visual>

    <binding template="TileSquare150x150IconWithBadge">
      <image id="1" src="Iconic.png" alt="alt text"/>
    </binding>
    
    <binding template="TileSquare71x71IconWithBadge">
      <image id="1" src="Iconic.png" alt="alt text"/>
    </binding>

  </visual>
</tile>
```

此图标磁贴模板 XML 负载使用的图像元素指向在步骤 1 中创建的图像。 现在，磁贴已经可以在图标旁边显示锁屏提醒；只需发送锁屏提醒通知即可。

**步骤 4：将徽章通知发送到你的磁贴**

与步骤 3 一样，此步骤将根据通知的发送方式（通过本地或通过服务器推送）而有所不同，但所发送的 XML 负载依然保持不变。 若要发送本地锁屏提醒通知，请为磁贴（主要磁贴或辅助磁贴）创建 [**BadgeUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.BadgeUpdater)，然后发送具有所需值的锁屏提醒（或清除锁屏提醒）通知。

下面是 XML 负载的示例代码：

```xml
<badge value="2"/>
```

磁贴的锁屏提醒将进行相应的更新。

**步骤 5：将它放在一起**

下图说明了如何将各个 API 和负载与图标磁贴模板的每个方面关联在一起。 [磁贴通知](https://docs.microsoft.com/previous-versions/windows/apps/hh779724(v=win.10))（包含那些 &lt;binding&gt; 元素）用于指定图标模板和图像资源；[锁屏提醒通知](https://docs.microsoft.com/previous-versions/windows/apps/hh779719(v=win.10))指定数值；磁贴属性控制你的磁贴的显示名称、颜色等。

![与图标磁贴模板关联的 API 和负载](images/iconic-template-properties-info.png)

## <a name="photos-tile-template"></a>照片磁贴模板


照片磁贴模板允许你在动态磁贴上显示照片的幻灯片。 该模板受所有磁贴大小（包括小磁贴）的支持，并且对于每个磁贴大小的行为都相同。 下面的示例演示了使用照片模板的具有五个画面的中等磁贴。 该模板具有在选定的照片之间无限循环的缩放和交叉淡入淡出动画。

![使用照片磁贴模板的图像幻灯片放映](images/photo-tile-template-image01.jpg)

### <a name="how-to-use-the-photos-template"></a>如何使用照片模板

如果你已安装[通知库](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，则使用照片模板将非常简单。 尽管你可以使用原始的 XML，但我们强烈建议使用该库，这样你便无需担心生成有效的 XML 或 XML 转义内容。

Windows Phone 最多在幻灯片中显示 9 张照片；平板电脑、笔记本电脑和台式机最多显示 12 张。

有关发送磁贴通知的信息，请参阅[发送通知文章](index.md)。


```xml
<!--
 
To use the Photos template...
 
 - On any adaptive tile binding (like TileMedium or TileWide)
   - Set the hint-presentation attribute to "photos"
   - Add up to 12 images as children of the binding.
    
-->
 
<tile>
  <visual>
     
    <binding template="TileMedium" hint-presentation="photos">
       
      <image src="Assets/1.jpg" />
      <image src="ms-appdata:///local/Images/2.jpg"/>
      <image src="http://msn.com/images/3.jpg"/>
       
      <!--TODO: Can have 12 images total-->
       
    </binding>
     
    <!--TODO: Add bindings for other tile sizes-->
     
  </visual>
</tile>
```

```csharp
/*
 
To use the Photos template...
 
 - On any TileBinding object
   - Set Content property to new instance of TileBindingContentPhotos
   - Add up to 12 images to Images property of TileBindingContentPhotos.
 
*/
 
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentPhotos()
            {
                Images =
                {
                    new TileBasicImage() { Source = "Assets/1.jpg" },
                    new TileBasicImage() { Source = "ms-appdata:///local/Images/2.jpg" },
                    new TileBasicImage() { Source = "http://msn.com/images/3.jpg" }
 
                    // TODO: Can have 12 images total
                }
            }
        }
 
        // TODO: Add other tile sizes
    }
};
```

## <a name="people-tile-template"></a>“人脉”磁贴模板


Windows 10 中的“人脉”应用使用的特殊磁贴模板显示一系列在磁贴上垂直或水平滚动的圆形图像。 此磁贴模板以来提供 Windows 10 生成 10572，和任何人都是欢迎使用以在自己的应用程序中使用它。

“人脉”磁贴模板可用于以下大小的磁贴：

**中磁贴** (TileMedium)

![中等“人脉”磁贴](images/people-tile-medium.png)

 

**宽磁贴** (TileWide)

![宽“人脉”磁贴](images/people-tile-wide.png)

 

**大磁贴（仅限台式机）** (TileLarge)

![大“人脉”磁贴](images/people-tile-large.png)

 

如果你使用的是[通知库](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，若要使用“人脉”磁贴模板，只需为 *TileBinding* 内容创建新的 *TileBindingContentPeople* 对象即可。 *TileBindingContentPeople* 类具有一个 Images 属性，你可以在其中添加图像。

如果你使用的是原始 XML，请将 *hint-presentation* 设置为“people”并将图像添加为绑定元素的子元素。

以下 C# 代码示例假定你使用的是[通知库](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)。

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentPeople()
            {
                Images =
                {
                    new TileBasicImage() { Source = "Assets/ProfilePics/1.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/2.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/3.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/4.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/5.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/6.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/7.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/8.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/9.jpg" }
                }
            }
        }
    }
};
```

```xml
<tile>
  <visual>
 
    <binding template="TileMedium" hint-presentation="people">
      <image src="Assets/ProfilePics/1.jpg"/>
      <image src="Assets/ProfilePics/2.jpg"/>
      <image src="Assets/ProfilePics/3.jpg"/>
      <image src="Assets/ProfilePics/4.jpg"/>
      <image src="Assets/ProfilePics/5.jpg"/>
      <image src="Assets/ProfilePics/6.jpg"/>
      <image src="Assets/ProfilePics/7.jpg"/>
      <image src="Assets/ProfilePics/8.jpg"/>
      <image src="Assets/ProfilePics/9.jpg"/>
    </binding>
 
  </visual>
</tile>
```

若要提供最佳用户体验，我们建议为每种磁贴大小提供以下数量的照片：

-   中等磁贴：9 照片
-   宽磁贴：15 照片
-   大磁贴：20 照片

提供如此数量的照片将允许出现几个空圆圈，这意味着磁贴的视觉效果不会过于拥挤。 可随意调整照片数量以获得最合适的外观。

若要发送通知，请参阅[选择通知传递方法](choosing-a-notification-delivery-method.md)。

## <a name="related-topics"></a>相关主题


* [GitHub 上的完整代码示例](https://github.com/WindowsNotifications/quickstart-people-tile-template)
* [通知库](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)
* [磁贴、锁屏提醒和通知](index.md)
* [创建自适应磁贴](create-adaptive-tiles.md)
* [磁贴内容架构](../tiles-and-notifications/tile-schema.md)
 

 




