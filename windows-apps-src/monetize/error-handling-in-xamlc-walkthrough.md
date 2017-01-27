---
author: mcleanbyron
ms.assetid: cf0d2709-21a1-4d56-9341-d4897e405f5d
description: "了解如何在应用中捕获 AdControl 错误。"
title: "XAML/C# 演练中的错误处理"
translationtype: Human Translation
ms.sourcegitcommit: f88a71491e185aec84a86248c44e1200a65ff179
ms.openlocfilehash: c9f2ad67413380a8393c8e00871e69af4bb2905a

---

# <a name="error-handling-in-xamlc-walkthrough"></a>XAML/C# 演练中的错误处理

本主题演示如何在应用中捕获 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 错误。

这些示例假定你拥有一个 XAML/C# 应用，它包含一个 **AdControl**。 有关演示如何向你的应用添加 **AdControl** 的分步说明，请参阅 [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)。 有关演示如何使用 C# 和 C++ 将横幅广告添加到 XAML 应用的完整示例项目，请参阅 [GitHub 上的广告示例](http://aka.ms/githubads)。

1.  在 MainPage.xaml 文件中，找到 **AdControl** 的定义。 该代码如下所示。

  > [!div class="tabbedCodeSnippets"]
  ``` xml
  <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="10865270"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300" />
  ```

2.   在 **Width** 属性之后，但在结束标记之前，将错误事件处理程序的名称分配给 [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.erroroccurred.aspx) 事件。 在本演练中，错误事件处理程序的名称为 **OnAdError**。

  > [!div class="tabbedCodeSnippets"]
  ``` xml
  <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="10865270"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300"
      ErrorOccurred="OnAdError"/>
  ```

2.  若要生成运行时错误，请创建具有不同应用程序 ID 的第二个 **AdControl**。 因为应用中的所有 **AdControl** 对象必须使用相同的应用程序 ID，所以创建具有不同应用程序 ID 的其他 **AdControl** 会引发错误。

    仅在 MainPage.xaml 中的第一个 **AdControl** 后面，定义第二个 **AdControl**，然后将 [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) 属性设置为零 (“0”)。

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl
        ApplicationId="0"
        AdUnitId="10865270"
        HorizontalAlignment="Left"
        Height="250"
        Margin="10,265,0,0"
        VerticalAlignment="Top"
        Width="300"
        ErrorOccurred="OnAdError" />
    ```

3.  在 MainPage.xaml.cs 中，将以下 **OnAdError** 事件处理程序添加到 **MainPage** 类。 此事件处理程序会将信息写入 Visual Studio“输出”****窗口。

  > [!div class="tabbedCodeSnippets"]
  ``` csharp
  private void OnAdError(object sender, AdErrorEventArgs e)
  {
      System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name +
          "): " + e.ErrorMessage + " ErrorCode: " + e.ErrorCode.ToString());
  }
  ```

4.  生成并运行该项目。 在应用运行后，你将在 Visual Studio 的“输出”****窗口中看到与以下内容类似的消息。

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  AdControl error (): MicrosoftAdvertising.Shared.AdException: all ad requests must use the same application ID within a single application (0, d25517cb-12d4-4699-8bdc-52040c712cab) ErrorCode: ClientConfiguration
  ```

## <a name="related-topics"></a>相关主题

* [GitHub 上的广告示例](http://aka.ms/githubads)

 



<!--HONumber=Dec16_HO2-->


