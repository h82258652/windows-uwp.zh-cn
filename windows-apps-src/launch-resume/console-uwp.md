---
author: TylerMSFT
title: 创建通用 Windows 平台控制台应用
description: 本主题介绍如何编写在控制台窗口中运行的 UWP 应用。
keywords: 控制台 UWP
ms.author: twhitney
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7e5923176f5f9fff1a6e4f30a7ba2419e99c074b
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2018
ms.locfileid: "6443876"
---
# <a name="create-a-universal-windows-platform-console-app"></a>创建通用 Windows 平台控制台应用

本主题介绍了如何创建[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)或 C + + /CX 通用 Windows 平台 (UWP) 控制台应用。

从 Windows 10，版本 1803，你可以编写 C + + WinRT 或 C + + /CX UWP 控制台应用在控制台窗口中，如 DOS 或 PowerShell 控制台窗口中运行。 控制台应用使用控制台窗口进行输入和输出，并且可以使用[通用的 C 运行时](/cpp/c-runtime-library/reference/crt-alphabetical-function-reference)功能，如**printf**和**getchar**。 UWP 控制台应用可以发布到 Microsoft Store。 它们在应用列表中有对应条目，并有可以固定到“开始”菜单的主要磁贴。 可以从开始菜单启动 UWP 控制台应用，但你通常将从命令行启动它们。

若要查看操作中，下面是有关创建 UWP 控制台应用的视频。

> [!VIDEO https://www.youtube.com/embed/bwvfrguY20s]

## <a name="use-a-uwp-console-app-template"></a>使用 UWP 控制台应用模板 

若要创建 UWP 控制台应用，请首先安装 [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.ConsoleAppUniversal)中提供的**控制台应用（通用）项目模板**。 就可在**新建项目**下的已安装的模板 > **已安装** > **其他语言** > **Visual c + +** > **Windows 通用**作为**控制台应用 C + WinRT (通用 Windows)** 和**控制台应用 C + + /CX (通用 Windows)**。

## <a name="add-your-code-to-main"></a>将代码添加到 main()

模板添加了 **Program.cpp**，其中包含 `main()` 函数。 这是 UWP 控制台应用中执行开始的位置。 使用 `__argc` 和 `__argv` 形式参数访问命令行实际参数。 控制从 `main()` 返回时，UWP 控制台应用会退出。

通过添加下面的示例的**Program.cpp** **控制台应用 C + WinRT**模板：

```cppwinrt
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

- 仅 C + + /winrt 与 C + + /CX UWP 应用可能是控制台应用。
- UWP 控制台应用必须针对桌面或 IoT 项目类型。
- UWP 控制台应用可能无法创建一个窗口。 他们无法使用 messagebox （） 或 Location()，或者可能出于任何原因，创建一个窗口的任何其他 API，如用户同意提示。
- UWP 控制台应用可能不使用后台任务，也不会作为后台任务运行。
- 除[命令行激活](https://blogs.windows.com/buildingapps/2017/07/05/command-line-activation-universal-windows-apps/#5YJUzjBoXCL4MhAe.97)外，UWP 控制台应用不支持激活合约，包括文件关联、协议关联等。
- 尽管 UWP 控制台应用支持多实例，但它们不支持[多实例重定向](multi-instance-uwp.md)
- 有关适用于 UWP 应用的 Win32 API 的列表，请参阅[适用于 UWP 应用的 Win32 和 COM API](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps)