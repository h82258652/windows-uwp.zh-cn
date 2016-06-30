---
author: TylerMSFT
title: "启动文件的默认应用"
description: "了解如何启动文件的默认应用。"
ms.assetid: BB45FCAF-DF93-4C99-A8B5-59B799C7BD98
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: b9b2d8ba6aeedea7d9db12565de191b1b6307fa6

---

# 启动文件的默认应用


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**Windows.System.Launcher.LaunchFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701461)

了解如何启动文件的默认应用。 很多应用需要使用它们自身无法处理的文件。 例如，电子邮件应用接收大量文件类型并且需要使用一种方式在其默认处理程序中启动这些文件。 这些步骤显示了如何使用 [**Windows.System.Launcher**](https://msdn.microsoft.com/library/windows/apps/br241801) API 为应用自身无法处理的文件启动默认处理程序。

## 获取文件对象


首先，获取该文件的 [**Windows.Storage.StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 对象。

如果该文件包含在应用的程序包中，则可以使用 [**Package.InstalledLocation**](https://msdn.microsoft.com/library/windows/apps/br224681) 属性获取 [**Windows.Storage.StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 对象并且使用 [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) 方法获取 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 对象。

如果该文件在已知的文件夹中，则可以使用 [**Windows.Storage.KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151) 类的属性获取 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 并且使用 [**GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) 方法获取 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 对象。

## 启动该文件


Windows 提供了用于为文件启动默认处理程序的多个不同选项。 这些选项将在此图表中和以下各节中进行介绍。

| 选项 | 方法 | 描述 |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 默认启动 | [**LaunchFileAsync(IStorageFile)**](https://msdn.microsoft.com/library/windows/apps/hh701471) | 使用默认处理程序启动指定的文件。 |
| 打开方式启动 | [**LaunchFileAsync(IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465) | 启动指定的文件，该文件让用户通过“打开方式”对话框选取处理程序。 |
| 使用推荐的应用反馈启动 | [**LaunchFileAsync(IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465) | 使用默认处理程序启动指定的文件。 如果系统上未安装处理程序，则向用户推荐应用商店中的应用。 |
| 以所需的其余视图启动 | [
            **LaunchFileAsync(IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465)（仅适用于 Windows） | 使用默认处理程序启动指定的文件。 指定首选项以便在启动后停留于屏幕上，然后请求特定的窗口大小。 |
|  |  |  |
|  |  | **移动设备系列：**[**LauncherOptions.DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) 在移动设备系列上不受支持。 |

 
### 默认启动

调用 [**Windows.System.Launcher.LaunchFileAsync(IStorageFile)**](https://msdn.microsoft.com/library/windows/apps/hh701471) 方法以启动默认的应用。 此示例会使用 [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) 方法启动应用包中包含的图像文件 test.png。


> [!div class="tabbedCodeSnippets"]
> ```vb
> async Sub DefaultLaunch()
>    ' Path to the file in the app package to launch
>    Dim imageFile = "images\test.png"
>    Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)
>    
>    If file IsNot Nothing Then
>       ' Launch the retrieved file
>       Dim success = await Windows.System.Launcher.LaunchFileAsync(file)
>
>       If success Then
>          ' File launched
>       Else
>          ' File launch failed
>       End If
>    Else
>       ' Could not find file
>    End If
> End Sub
> ```
> ```cpp
> void MainPage::DefaultLaunch()
> {
>    auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;
>
>    concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.png"));
>    getFileOperation.then([](Windows::Storage::StorageFile^ file)
>    {
>       if (file != nullptr)
>       {
>          // Launch the retrieved file
>          concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file));
>          launchFileOperation.then([](bool success)
>          {
>             if (success)
>             {
>                // File launched
>             }
>             else
>             {
>                // File launch failed
>             }
>          });
>       }
>       else
>       {
>          // Could not find file
>       }
>    });
> }
> ```
> ```cs
> async void DefaultLaunch()
> {
>    // Path to the file in the app package to launch
>    string imageFile = @"images\test.png";
>    
>    var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);
>    
>    if (file != null)
>    {
>       // Launch the retrieved file
>       var success = await Windows.System.Launcher.LaunchFileAsync(file);
>
>       if (success)
>       {
>          // File launched
>       }
>       else
>       {
>          // File launch failed
>       }
>    }
>    else
>    {
>       // Could not find file
>    }
> }
> ```

### 打开方式启动

在 [**LauncherOptions.DisplayApplicationPicker**](https://msdn.microsoft.com/library/windows/apps/hh701438) 设置为 **true** 的情况下调用 [**Windows.System.Launcher.LaunchFileAsync(IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465) 方法以启动用户从**“打开方式”**对话框中选择的应用。

当用户希望针对某个特定文件选择默认应用以外的应用时，我们建议你使用**“打开方式”**对话框。 例如，如果你的应用允许用户启动某个图像文件，则默认的处理程序将可能是查看器应用。 在某些情况下，用户可能需要编辑图像而不只是查看图像。 使用**“打开方式”**选项及**“应用程序栏”**或上下文菜单中的备用命令，让用户在此类情况下打开**“打开方式”**对话框并选择编辑器应用。

![用于 .png 文件启动的“打开方式”对话框。 该对话框包含一个复选框，用于指定用户的选择是应该用于所有 .png 文件还是只用于这一个 .png 文件。 该对话框包含四个应用选项，用于启动文件和“更多选项”链接。](images/checkboxopenwithdialog.png)

> [!div class="tabbedCodeSnippets"]
> ```vb
> async Sub DefaultLaunch()
>
>    ' Path to the file in the app package to launch
>    Dim imageFile = "images\test.png"
>
>    Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)
>
>    If file IsNot Nothing Then
>       ' Set the option to show the picker
>       Dim options = Windows.System.LauncherOptions()
>       options.DisplayApplicationPicker = True
>
>       ' Launch the retrieved file
>       Dim success = await Windows.System.Launcher.LaunchFileAsync(file)
>
>       If success Then
>          ' File launched
>       Else
>          ' File launch failed
>       End If
>    Else
>       ' Could not find file
>    End If
> End Sub
> ```
> ```cpp
> void MainPage::DefaultLaunch()
> {
>    auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;
>
>    concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.png"));
>    getFileOperation.then([](Windows::Storage::StorageFile^ file)
>    {
>       if (file != nullptr)
>       {
>          // Set the option to show the picker
>          auto launchOptions = ref new Windows::System::LauncherOptions();
>          launchOptions->DisplayApplicationPicker = true;
>
>          // Launch the retrieved file
>          concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
>          launchFileOperation.then([](bool success)
>          {
>             if (success)
>             {
>                // File launched
>             }
>             else
>             {
>                // File launch failed
>             }
>          });
>       }
>       else
>       {
>          // Could not find file
>       }
>    });
> }
> ```
> ```cs
> async void DefaultLaunch()
> {
>    // Path to the file in the app package to launch
>       string imageFile = @"images\test.png";
>       
>    var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);
>
>    if (file != null)
>    {
>       // Set the option to show the picker
>       var options = new Windows.System.LauncherOptions();
>       options.DisplayApplicationPicker = true;
>
>       // Launch the retrieved file
>       bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
>       if (success)
>       {
>          // File launched
>       }
>       else
>       {
>          // File launch failed
>       }
>    }
>    else
>    {
>       // Could not find file
>    }
> }
> ```

**使用推荐的应用反馈启动**

在某些情况下，用户可能未安装用以处理所启动文件的应用。 默认情况下，为处理此类情况，Windows 会向用户提供一个链接，帮助其在应用商店中搜索相应的应用。 如果你希望为用户提供具体的建议，告知他们在此情况下应获取何种应用，则可以随所启用的文件传递该建议。 为此，调用 [**Windows.System.Launcher.launchFileAsync(IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465) 方法，将 [**LauncherOptions.PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482) 设置为应用商店中要推荐的应用的程序包系列名称。 然后，将 [**LauncherOptions.PreferredApplicationDisplayName**](https://msdn.microsoft.com/library/windows/apps/hh965481) 设置为该应用的名称。 Windows 会使用此信息将在应用商店中搜索应用这一常规选项替换为从应用商店中获取推荐的应用这一具体选项。

> **注意** 必须设置这两个选项才能推荐应用。 设置一个而不设置另一个将导致故障。

![用于 .contoso 文件启动的“打开方式”对话框。 因为 .contoso 在计算机上未安装处理程序，所以该对话框中包含一个带有“应用商店”图标和文本的选项，将用户指引到应用商店中的正确处理程序。 该对话框还包含“更多选项”链接。](images/howdoyouwanttoopen.png)


> [!div class="tabbedCodeSnippets"]
> ```vb
> async Sub DefaultLaunch()
>
>    ' Path to the file in the app package to launch
>    Dim imageFile = "images\test.contoso"
>
>    ' Get the image file from the package's image directory
>    Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)
>
>    If file IsNot Nothing Then
>       ' Set the recommended app
>       Dim options = Windows.System.LauncherOptions()
>       options.PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
>       options.PreferredApplicationDisplayName = "Contoso File App";
>
>       ' Launch the retrieved file pass in the recommended app
>       ' in case the user has no apps installed to handle the file
>       Dim success = await Windows.System.Launcher.LaunchFileAsync(file)
>
>       If success Then
>          ' File launched
>       Else
>          ' File launch failed
>       End If
>    Else
>       ' Could not find file
>    End If
> End Sub
> ```
> ```cpp
> void MainPage::DefaultLaunch()
> {
>    auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;
>
>    concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.contoso"));
>    getFileOperation.then([](Windows::Storage::StorageFile^ file)
>    {
>       if (file != nullptr)
>       {
>          // Set the recommended app
>          auto launchOptions = ref new Windows::System::LauncherOptions();
>          launchOptions-> preferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
>          launchOptions-> preferredApplicationDisplayName = "Contoso File App";
>          
>          // Launch the retrieved file pass in the recommended app
>          // in case the user has no apps installed to handle the file
>          concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
>          launchFileOperation.then([](bool success)
>          {
>             if (success)
>             {
>                // File launched
>             }
>             else
>             {
>                // File launch failed
>             }
>          });
>       }
>       else
>       {
>          // Could not find file
>       }
>    });
> }
> ```
> ```cs
> async void DefaultLaunch()
> {
>    // Path to the file in the app package to launch
>    string imageFile = @"images\test.contoso";
>
>    // Get the image file from the package's image directory
>    var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);
>
>    if (file != null)
>    {
>       // Set the recommended app
>       var options = new Windows.System.LauncherOptions();
>       options.PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
>       options.PreferredApplicationDisplayName = "Contoso File App";
>
>
>       // Launch the retrieved file pass in the recommended app
>       // in case the user has no apps installed to handle the file
>       bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
>       if (success)
>       {
>          // File launched
>       }
>       else
>       {
>          // File launch failed
>       }
>    }
>    else
>    {
>       // Could not find file
>    }
> }
> ```

### 以所需的其余视图启动（仅适用于 Windows）

调用 [**LaunchFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701461) 的源应用可请求在文件启动后停留于屏幕上。 默认情况下，Windows 会尝试在处理该文件的源应用和目标应用之间平等地共享所有可用空间。 源应用可使用 [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) 属性向操作系统指示希望其应用占用较多或较少的可用空间。 此外，还可使用 **DesiredRemainingView** 以指示源应用在文件启动后无需停留于屏幕上，并可由目标应用完全替代。 此属性仅指定调用应用的首选窗口大小。 不指定可能会同时显示在屏幕上的其他应用的行为。

> **注意** Windows 在确定源应用的最终窗口尺寸时会考虑多个不同因素；例如，源应用的首选项、屏幕上的应用数量以及屏幕的方向等。 设置 [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) 并不保证为源应用设定具体的窗口化行为。

**移动设备系列：**[**LauncherOptions.DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) 在移动设备系列上不受支持。

> [!div class="tabbedCodeSnippets"]
> ```cpp
> void MainPage::DefaultLaunch()
> {
>    auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;
>
>    concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.png"));
>    getFileOperation.then([](Windows::Storage::StorageFile^ file)
>    {
>       if (file != nullptr)
>       {
>          // Set the desired remaining view
>          auto launchOptions = ref new Windows::System::LauncherOptions();
>          launchOptions->DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;
>
>          // Launch the retrieved file
>          concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
>          launchFileOperation.then([](bool success)
>          {
>             if (success)
>             {
>                // File launched
>             }
>             else
>             {
>                // File launch failed
>             }
>          });
>       }
>       else
>       {
>          // Could not find file
>       }
>    });
> }
> ```
> ```cs
> async void DefaultLaunch()
> {
>    // Path to the file in the app package to launch
>    string imageFile = @"images\test.png";
>    
>    var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);
>
>    if (file != null)
>    {
>       // Set the desired remaining view
>       var options = new Windows.System.LauncherOptions();
>       options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;
>
>       // Launch the retrieved file
>       bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
>       if (success)
>       {
>          // File launched
>       }
>       else
>       {
>          // File launch failed
>       }
>    }
>    else
>    {
>       // Could not find file
>    }
> }
> ```

## 备注

你的应用不能选择要启动的应用。 用户确定启动哪个应用。 用户可以选择通用 Windows 平台 (UWP) 应用或经典 Windows 平台 (CWP) 应用。

启动文件时，你的应用必须是前台应用，即对于用户必须是可见的。 此要求有助于确保用户保持控制。 为满足此要求，需确保将文件的所有启动都直接绑定到应用的 UI 中。 大多数情况下，用户总是必须采取某个操作来发起文件启动。

如果包含代码或脚本的文件类型（例如 .exe、.msi 和 .js 文件）由操作系统自动执行，则你无法启动这些文件类型。 此限制可防止用户遭受可能修改操作系统的潜在恶意文件的损害。 如果可以包含脚本的文件类型由可隔离脚本的应用来执行（例如 .docx 文件），则你可以使用此方法来启动这些文件类型。 Microsoft Word 之类的应用可防止 .docx 文件中的脚本修改操作系统。

如果你尝试启动受限制的文件类型，则启动将失败，且会调用错误回调。 如果你的应用处理许多不同类型的文件，并且你预计会遇到该错误，则应该为你的用户提供回退体验。 例如，你可以为用户提供将文件保存到桌面的选项，然后用户可以从桌面打开该文件。

> **注意** 本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你面向 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 
## 相关主题


**任务**

* [启动 URI 的默认应用](launch-default-app.md)
* [处理文件激活](handle-file-activation.md)

**指南**

* [文件类型和 URI 的指南](https://msdn.microsoft.com/library/windows/apps/hh700321)

**参考**

* [**Windows.Storage.StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)
* [**Windows.System.Launcher.LaunchFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701461)

 

 



<!--HONumber=Jun16_HO4-->


