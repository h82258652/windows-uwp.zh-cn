---
title: 自动启动 Windows 10 通用 Windows 平台 (UWP) 应用
description: 开发人员可以使用协议激活和启动激活来自动启动他们的 UWP 应用或游戏以进行自动化测试。
ms.topic: article
ms.localizationpriority: medium
ms.date: 02/08/2017
ms.openlocfilehash: fb68b4bbd1b751591e9f336efe5dad3c22b3bf92
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8890277"
---
# <a name="automate-launching-windows-10-uwp-apps"></a>自动启动 Windows 10 UWP 应用

## <a name="introduction"></a>简介

开发人员有多种选项可用于实现自动启动通用 Windows 平台 (UWP) 应用。 在本文中，我们将探讨通过使用协议激活和启动激活来启动应用的方法。

*协议激活*允许应用根据给定协议将自身注册为处理程序。 

*启动激活*是正常的应用启动，例如从应用磁贴启动。

通过每个激活方法，你可以选择使用命令行或启动器应用程序。 对于所有的激活方法，如果应用当前正在运行，激活会将应用显示到前台（这将重新激活它）并提供新的激活参数。 这允许灵活使用激活命令向应用提供新消息。 请务必注意，需要针对激活方法编译和部署项目才能运行新更新的应用。 

## <a name="protocol-activation"></a>协议激活

按照以下步骤来设置适用于应用的协议激活： 

1. 在 Visual Studio 中打开 **Package.appxmanifest** 文件。
2. 选择“声明”**** 选项卡。
3. 在“可用声明”**** 下拉列表中，选择“协议”****，然后选择“添加”****。
4. 在“属性”**** 下的“名称”**** 字段中，输入唯一名称以启动应用。 

    ![协议激活](images/automate-uwp-apps-1.png)

5. 保存文件并部署项目。 
6. 在部署项目后，应设置协议激活。 
7. 转到“控制面板”\“所有控制面板项”\“默认程序”****，然后选择“将文件类型或协议与特定程序关联”****。 滚动到“协议”**** 部分，查看协议是否列出。 

现在设置了协议激活，你可以通过两个选项（命令行或启动器应用程序）使用协议激活应用。 

### <a name="command-line"></a>命令行

可以通过使用命令行（命令启动后跟之前设置的协议名称、冒号（“:”）以及任何参数）来协议激活应用。 这些参数可以是任意字符串；但是，为了充分利用统一资源标识符 (URI) 功能，建议遵循标准的 URI 格式： 

  ```
  scheme://username:password@host:port/path.extension?query#fragment
  ```

Uri 对象有分析此格式的 URI 字符串的方法。 有关详细信息，请参阅 [Uri 类 (MSDN)](https://msdn.microsoft.com/library/windows/apps/windows.foundation.uri.aspx)。 

示例：

  ```
  >start bingnews:
  >start myapplication:protocol-parameter
  >start myapplication://single-player/level3?godmode=1&ammo=200
  ```

协议命令行激活在原始 URI 上最多支持 2038 个 Unicode 字符。 

### <a name="launcher-application"></a>启动器应用程序

若要启动，请单独创建一个支持 WinRT API 的应用程序。 以下示例中显示了启动程序中用于通过协议激活启动的 C++ 代码，其中 **PackageURI** 是适用于具有任何参数的应用程序的 URI；例如 `myapplication:` 或 `myapplication:protocol activation arguments`。

```
bool ProtocolLaunchURI(Platform::String^ URI)
{
       IAsyncOperation<bool>^ protocolLaunchAsyncOp;
       try
       {
              protocolLaunchAsyncOp = Windows::System::Launcher::LaunchUriAsync(ref new 
Uri(URI));
       }
       catch (Platform::Exception^ e)
       {
              Platform::String^ dbgStr = "ProtocolLaunchURI Exception Thrown: " 
+ e->ToString() + "\n";
              OutputDebugString(dbgStr->Data());
              return false;
       }

       concurrency::create_task(protocolLaunchAsyncOp).wait();

       if (protocolLaunchAsyncOp->Status == AsyncStatus::Completed)
       {
              bool LaunchResult = protocolLaunchAsyncOp->GetResults();
              Platform::String^ dbgStr = "ProtocolLaunchURI " + URI 
+ " completed. Launch result " + LaunchResult + "\n";
              OutputDebugString(dbgStr->Data());
              return LaunchResult;
       }
       else
       {
              Platform::String^ dbgStr = "ProtocolLaunchURI " + URI + " failed. Status:" 
+ protocolLaunchAsyncOp->Status.ToString() + " ErrorCode:" 
+ protocolLaunchAsyncOp->ErrorCode.ToString() + "\n";
              OutputDebugString(dbgStr->Data());
              return false;
       }
}
```
启动器应用程序的协议激活与命令行的协议激活具有相同的参数限制。 二者在原始 URI 上都最多支持 2038 个 Unicode 字符。 

## <a name="launch-activation"></a>启动激活

你还可以通过使用启动激活来启动应用。 不需要进行设置，但需要 UWP 应用的应用程序用户模型 ID (AUMID)。 AUMID 是程序包系列名称，后跟一个感叹号和应用程序 ID。 

获取程序包系列名称的最佳方法是完成以下步骤：

1. 打开 **Package.appxmanifest** 文件。
2. 在“打包”**** 选项卡上，输入“程序包名称”****。

    ![启动激活](images/automate-uwp-apps-2.png)

3. 如果“程序包系列名称”**** 未列出，请打开 PowerShell 并运行 `>get-appxpackage MyPackageName` 来查找 **PackageFamilyName**。

在 `<Applications>` 元素下的 **Package.appxmanifest** 文件（在 XML 视图中打开）中可找到应用程序 ID。

### <a name="command-line"></a>命令行

用于执行 UWP 应用启动激活的工具随 Windows 10 SDK 一起安装。 该工具可以从命令行运行，并且它会将应用的 AUMID 作为一个参数启动。

```
C:\Program Files (x86)\Windows Kits\10\App Certification Kit\microsoft.windows.softwarelogo.appxlauncher.exe <AUMID>
```

它看起来如下所示：

```
"C:\Program Files (x86)\Windows Kits\10\App Certification Kit\microsoft.windows.softwarelogo.appxlauncher.exe" MyPackageName_ph1m9x8skttmg!AppId
```

此选项不支持命令行参数。 

### <a name="launcher-application"></a>启动器应用程序

你可以单独创建一个支持使用 COM 的应用程序以用于启动。 以下示例显示启动程序中用于通过启动激活启动的 C++ 代码。 使用此代码，你可以创建 **ApplicationActivationManager** 对象并调用传入之前查找的 AUMID 的 **ActivateApplication** 和任何参数。 有关其他参数的详细信息，请参阅 [IApplicationActivationManager::ActivateApplication 方法 (MSDN)](https://msdn.microsoft.com/library/windows/desktop/hh706903(v=vs.85).aspx)。

```
#include <ShObjIdl.h>
#include <atlbase.h>

HRESULT LaunchApp(LPCWSTR AUMID)
{
     HRESULT hr = CoInitializeEx(nullptr, COINIT_APARTMENTTHREADED);
     if (FAILED(hr))
     {
            wprintf(L"LaunchApp %s: Failed to init COM. hr = 0x%08lx \n", AUMID, hr);
     }
     {
            CComPtr<IApplicationActivationManager> AppActivationMgr = nullptr;
            if (SUCCEEDED(hr))
            {
                   hr = CoCreateInstance(CLSID_ApplicationActivationManager, nullptr,  
CLSCTX_LOCAL_SERVER, IID_PPV_ARGS(&AppActivationMgr));
                   if (FAILED(hr))
                   {
                         wprintf(L"LaunchApp %s: Failed to create Application Activation 
Manager. hr = 0x%08lx \n", AUMID, hr);
                   }
            }
            if (SUCCEEDED(hr))
            {
                   DWORD pid = 0;
                   hr = AppActivationMgr->ActivateApplication(AUMID, nullptr, AO_NONE, 
&pid);
                   if (FAILED(hr))
                   {
                         wprintf(L"LaunchApp %s: Failed to Activate App. hr = 0x%08lx 
\n", AUMID, hr);
                   }
            }
     }
     CoUninitialize();
     return hr;
}
```

值得注意的是，与前面的启动方法（即，使用命令行）不同，此方法支持传入的参数。

## <a name="accepting-arguments"></a>接受参数

若要在激活 UWP 应用时接受传入的参数，必须向该应用添加一些代码。 若要确定进行的是协议激活还是启动激活，请替代 **OnActivated** 事件，并检查参数类型，然后获取原始字符串或 Uri 对象的预分析的值。 

此示例介绍如何获取原始字符串。

```
void OnActivated(IActivatedEventArgs^ args)
{
        // Check for launch activation
        if (args->Kind == ActivationKind::Launch)
        {
            auto launchArgs = static_cast<LaunchActivatedEventArgs^>(args); 
            Platform::String^ argval = launchArgs->Arguments;
            // Manipulate arguments …
        }

        // Check for protocol activation
        if (args->Kind == ActivationKind::Protocol)
        {
            auto protocolArgs = static_cast< ProtocolActivatedEventArgs^>(args);
            Platform::String^ argval = protocolArgs->Uri->ToString();
            // Manipulate arguments …
        }
}
```

## <a name="summary"></a>摘要
总之，你可以使用各种方法来启动 UWP 应用。 根据要求和使用情况，可能还有更适合的其他方法。 

## <a name="see-also"></a>另请参阅
- [Xbox One 上的 UWP](index.md)

