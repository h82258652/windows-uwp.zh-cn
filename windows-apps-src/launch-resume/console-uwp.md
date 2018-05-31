---
author: TylerMSFT
title: 创建通用 Windows 平台控制台应用
description: 本主题介绍如何编写在控制台窗口中运行的 UWP 应用。
keywords: 控制台 UWP
ms.author: twhitney
ms.date: 03/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 413f4db427557460c267370f477e16d84c8af29c
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2018
ms.locfileid: "1815532"
---
# <a name="create-a-universal-windows-platform-console-app"></a>创建通用 Windows 平台控制台应用

本主题介绍如何创建 C++ /WinRT 或 /CX 通用 Windows 平台 (UWP) 控制台应用。

从 Windows 10 版本 1803 开始，可编写在控制台窗口（如 DOS 或 PowerShell 控制台窗口）中运行的 C++ /WinRT 或 /CX UWP 控制台应用。 控制台应用将控制台窗口用于输入和输出，并使用适用于 UWP 应用（如 **printf** 或 **getchar**）的 Win32 API。 UWP 控制台应用可以发布到 Microsoft Store。 它们在应用列表中有对应条目，并有可以固定到“开始”菜单的主要磁贴。 可以从“开始”菜单启动 UWP 控制台应用，但通常从命令行启动它们。

若要了解实际操作，请观看此视频，了解创建 UWP 控制台应用的有关信息：
> [!VIDEO https://www.youtube.com/embed/bwvfrguY20s]

## <a name="use-a-uwp-console-app-template"></a>使用 UWP 控制台应用模板 

若要创建 UWP 控制台应用，请首先安装 [Visual Studio Marketplace](https://aka.ms/E2nzbv)中提供的**控制台应用（通用）项目模板**。 然后，安装的模板将作为**控制台应用 C++/CX（通用 Windows）** 和**控制台应用 C++/WinRT（通用 Windows）** 在**新建项目** > **已安装** > **其他语言** > **Visual C++** > **Windows 通用**下提供。

## <a name="add-your-code-to-main"></a>将代码添加到 main()

模板添加了 **Program.cpp**，其中包含 `main()` 函数。 这是 UWP 控制台应用中执行开始的位置。 使用 `__argc` 和 `__argv` 形式参数访问命令行实际参数。 控制从 `main()` 返回时，UWP 控制台应用会退出。

以下 **Program.cpp** 示例由**控制台应用 C++ /WinRT** 模板添加：

```cpp
#include "pch.h"

using namespace winrt;

// This example code shows how you could implement the required main function
// for a Console UWP Application. You can replace all the code inside main
// with your own custom code.

int __cdecl main()
{
    // You can get parsed command-line arguments from the CRT globals.
    wprintf(L"Parsed command-line arguments:\n");
    for (int i = 0; i < __argc; i++)
    {
        wprintf(L"__argv[%d] = %S\n", i, __argv[i]);
    }

    // Keep the console window alive in case you want to see console output when running from within Visual Studio
      wprintf(L"Press 'Enter' to continue: ");
    getchar();
}
```

## <a name="uwp-console-app-behavior"></a>UWP 控制台应用行为

UWP 控制台应用可以从其运行的目录及其下方目录访问文件系统。 这是可能的，因为该模板向应用的 Package.appxmanifest 文件中添加了 [AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) 扩展。 此扩展还使用户可以从控制台窗口中键入别名，以启动该应用。 该应用不需要在系统路径中启动。

你还可以如[文件访问权限](https://docs.microsoft.com/windows/uwp/files/file-access-permissions)中所述，通过添加受限功能 `broadFileSystemAccess`，向 UWP 控制台应用提供对文件系统的广泛访问权限。 此功能适用于 [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) 命名空间中的 API。

可以一次运行多个 UWP 控制台应用的实例，因为该模板会向应用的 Package.appxmanifest 文件添加 [SupportsMultipleInstances](multi-instance-uwp.md) 功能。

该模板还会向 Package.appxmanifest 文件添加 `Subsystem="console"` 功能，文件表示此 UWP 应用是控制台应用。 请注意 `desktop4` 和 iot2`namespace` 前缀。 仅桌面和物联网 (IoT) 项目支持 UWP 控制台应用。

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4" 
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2" 
  IgnorableNamespaces="uap mp uap5 desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:Subsystem="console" 
      desktop4:SupportsMultipleInstances="true" 
      iot2:Subsystem="console" 
      iot2:SupportsMultipleInstances="true"  >
      ...
      <Extensions>
          <uap5:Extension 
            Category="windows.appExecutionAlias" 
            Executable="YourApp.exe" 
            EntryPoint="YourApp.App">
            <uap5:AppExecutionAlias desktop4:Subsystem="console">
              <uap5:ExecutionAlias Alias="YourApp.exe" />
            </uap5:AppExecutionAlias>
          </uap5:Extension>
      </Extensions>
    </Application>
  </Applications>
    ...
</Package>
```

## <a name="additional-considerations-for-uwp-console-apps"></a>UWP 控制台应用的其他注意事项

- 仅 C++ /WinRT 和 C++ /CX UWP 应用可能是控制台应用。
- UWP 控制台应用必须针对桌面或 IoT 项目类型。
- UWP 控制台应用可能无法创建窗口。 例如，它们会因为创建窗口而无法使用 MessageBox()。
- UWP 控制台应用可能不使用后台任务，也不会作为后台任务运行。
- 除[命令行激活](https://blogs.windows.com/buildingapps/2017/07/05/command-line-activation-universal-windows-apps/#5YJUzjBoXCL4MhAe.97)外，UWP 控制台应用不支持激活合约，包括文件关联、协议关联等。
- 尽管 UWP 控制台应用支持多实例，但它们不支持[多实例重定向](multi-instance-uwp.md)
- 有关适用于 UWP 应用的 Win32 API 的列表，请参阅[适用于 UWP 应用的 Win32 和 COM API](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps)