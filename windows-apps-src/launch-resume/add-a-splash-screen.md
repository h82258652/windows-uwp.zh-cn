---
author: TylerMSFT
title: "添加初始屏幕"
description: "使用 Microsoft Visual Studio 2015 设置你的应用的初始屏幕图像和背景色。"
ms.assetid: 41F53046-8AB7-4782-9E90-964D744B7D66
translationtype: Human Translation
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 261b52d1835e992a784aa5fa356230fdd326b8c5

---

# 添加初始屏幕


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


使用 Microsoft Visual Studio 2015 设置你的应用的初始屏幕图像和背景色。

## 在 Visual Studio 2015 中设置初始屏幕图像和背景色


使用 Visual Studio 2015 模板创建你的应用，一个默认的图像已添加到你的项目并设置为初始屏幕图像。 初始屏幕的背景色默认为浅灰色。 如果要更改应用初始屏幕的默认图像或颜色，请按照以下步骤进行操作：

1.  在 Visual Studio 2015 中打开现有通用 Windows 平台 (UWP) 应用项目。
2.  在“解决方案资源管理器”****中，打开“Package.appxmanifest”文件。 也可以通过依次选择“项目”****&gt;“存储”****&gt;“编辑应用清单”****，从菜单栏中打开此文件。
3.  打开“可视资源”****选项卡并从“Package.appxmanifest”窗口左侧的“所有图像资源”****窗格中选择“初始屏幕”****。 如果你是首次更改初始屏幕，你将看到“初始屏幕”****字段中的“Assets\SplashScreen.png”路径。

    以下屏幕快照显示了 Visual Studio 2015 中的 “Package.appxmanifest”窗口。 根据对象的类型，你将看到一组稍有不同的可视资源。

    ![Visual Studio 2013 中“Package.appxmanifest”窗口的屏幕快照](images/appmanifest.png)

    如果你在文本编辑器中打开“Package.appxmanifest”，[**SplashScreen 元素**](https://msdn.microsoft.com/library/windows/apps/br211467)会显示为 [**VisualElements 元素**](https://msdn.microsoft.com/library/windows/apps/br211471)的子元素。 清单文件中的默认初始屏幕标记在文本编辑器中显示如下：

    ```xml
    <uap:SplashScreen Image="Assets\SplashScreen.png" />
    ```

4.  若要选择适用于 UWP 应用的新初始屏幕图像，请按下在“比例资源”****下的“1240 x 600 px”****标签旁边显示的带省略号的按钮。 选择你要用于初始屏幕图像的 1240 x 600 像素图像（.png、.jpg 或 .jpeg）。

    **重要提示** 所选初始屏幕图像必须为使用 1 倍比例系数的 620 x 300 像素。 此外，在设计你的初始屏幕时，请注意它比屏幕小，并且居中。 它不像 Windows Phone 应用商店应用的初始屏幕一样会填满屏幕。

     

5.  若要选择适用于 Windows Phone 应用商店应用的新初始屏幕图像，请按下在“比例资源”****下的“1152 x 1920 px”****标签旁边显示的带省略号的按钮。 选择你要用于初始屏幕图像的 1152 x 1920 像素图像（.png、.jpg 或 .jpeg）。

    **重要提示** 所选初始屏幕图像必须为 1152 x 1920 像素，即 2.4 倍比例系数的正确大小。 如果这是你提供的唯一资源，那么它将缩小为 1.4 倍和 1 倍比例系数。

     

6.  在“初始屏幕”****部分的“背景色”****字段中，将背景色设置为与初始屏幕图像一起显示。 你可以输入颜色的名称，也可以输入“#”和颜色的十六进制值。 有关可用颜色的名称列表，请参阅 [**SplashScreen 元素**](https://msdn.microsoft.com/library/windows/apps/br211467)。

    你也可以为初始屏幕设置背景色。 如果你未为 UWP 应用指定颜色，则初始屏幕背景色默认为浅灰色（十六进制值 \#464646）。 这与默认的“磁贴”****背景色相同（请参阅“可视资源”****选项卡的“磁贴图像和徽标”****部分中的“背景色”****字段）。 如果你未为 Windows Phone 指定颜色，或将其设置为“透明”，则初始屏幕背景色将为透明。

## 摘要和后续步骤


如果应用加载所需的时间较长，请考虑添加一个延长的初始屏幕。 有关分步指南，请参阅[创建自定义初始屏幕](create-a-customized-splash-screen.md)。

**注意**  
本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你要针对 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## 相关主题

* [创建自定义初始屏幕](create-a-customized-splash-screen.md)

**参考**

* [**程序包清单架构参考：SplashScreen 元素**](https://msdn.microsoft.com/library/windows/apps/br211467)
* [**Windows.ApplicationModel.Activation.SplashScreen 类**](https://msdn.microsoft.com/library/windows/apps/br224763)

 

 



<!--HONumber=Jun16_HO5-->


