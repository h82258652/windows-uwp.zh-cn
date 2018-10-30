---
author: TylerMSFT
title: 在应用程序清单中声明后台任务
description: 通过在应用清单中将后台任务声明为扩展，以实现对后台任务的使用。
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp，后台任务
ms.localizationpriority: medium
ms.openlocfilehash: 343cca5b89dbe5fd7e1309b9487e8939218203d0
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5760730"
---
# <a name="declare-background-tasks-in-the-application-manifest"></a>在应用程序清单中声明后台任务




**重要的 API**

-   [**BackgroundTasks 架构**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

通过在应用清单中将后台任务声明为扩展，以实现对后台任务的使用。

> [!Important]
>  本文特定于进程外后台任务。 进程内后台任务未在该清单中声明。

必须在应用清单中声明进程外后台任务，否则你的应用将无法注册它们（会引发异常）。 此外，必须在应用程序清单中声明进程外后台任务才能通过认证。

本主题假定你已创建一个或多个后台任务类，并且你的应用注册了为响应至少一个触发器而运行的所有后台任务。

## <a name="add-extensions-manually"></a>手动添加扩展


打开应用程序清单 (Package.appxmanifest)，然后转到 Application 元素。 创建一个 Extensions 元素（如果尚不存在该元素）。

以下代码段来自[后台任务示例](http://go.microsoft.com/fwlink/p/?LinkId=618666)：

```xml
<Application Id="App"
   ...
   <Extensions>
     <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.SampleBackgroundTask">
       <BackgroundTasks>
         <Task Type="systemEvent" />
         <Task Type="timer" />
       </BackgroundTasks>
     </Extension>
     <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ServicingComplete">
       <BackgroundTasks>
         <Task Type="systemEvent"/>
       </BackgroundTasks>
     </Extension>
   </Extensions>
 </Application>
```

## <a name="add-a-background-task-extension"></a>添加背景任务扩展  

声明你的第一个后台任务。

将该代码复制到 Extensions 元素中（将在下面的步骤中添加属性）。

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="">
      <BackgroundTasks>
        <Task Type="" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```

1.  更改 EntryPoint 属性以让你的代码使用的字符串与注册后台任务时的入口点相同 (**namespace.classname**)。

    在此示例中，入口点为 ExampleBackgroundTaskNameSpace.ExampleBackgroundTaskClassName：

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ExampleBackgroundTaskClassName">
       <BackgroundTasks>
         <Task Type="" />
       </BackgroundTasks>
    </Extension>
</Extensions>
```

2.  更改 Task Type 属性列表以指示该后台任务所使用的任务注册类型。 如果后台任务注册了多个触发器类型，需要为每个触发器类型添加附加的 Task 元素和 Type 属性。

    **注意**确保列出的每个触发器类型你正在使用，或后台任务将不会注册未声明的触发器类型 （[**注册**](https://msdn.microsoft.com/library/windows/apps/br224772)方法将失败并引发异常）。

    此代码段示例指示使用系统事件触发器和推送通知：

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
    <BackgroundTasks>
        <Task Type="systemEvent" />
        <Task Type="pushNotification" />
    </BackgroundTasks>
</Extension>
```

### <a name="add-multiple-background-task-extensions"></a>添加多个后台任务扩展

对你的应用注册的每个额外的后台任务类重复步骤 2。

下面的示例是来自[后台任务示例]( http://go.microsoft.com/fwlink/p/?linkid=227509)的完整 Application 元素。 它显示使用了两个后台任务类，触发器类型总数为 3。 复制此示例的“扩展”部分，并根据需要进行修改，以在应用程序清单中声明后台任务。

```xml
<Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="BackgroundTask.App">
        <uap:VisualElements
          DisplayName="BackgroundTask"
          Square150x150Logo="Assets\StoreLogo-sdk.png"
          Square44x44Logo="Assets\SmallTile-sdk.png"
          Description="BackgroundTask"

          BackgroundColor="#00b2f0">
          <uap:LockScreen Notification="badgeAndTileText" BadgeLogo="Assets\smalltile-Windows-sdk.png" />
            <uap:SplashScreen Image="Assets\Splash-sdk.png" />
            <uap:DefaultTile DefaultSize="square150x150Logo" Wide310x150Logo="Assets\tile-sdk.png" >
                <uap:ShowNameOnTiles>
                    <uap:ShowOn Tile="square150x150Logo" />
                    <uap:ShowOn Tile="wide310x150Logo" />
                </uap:ShowNameOnTiles>
            </uap:DefaultTile>
        </uap:VisualElements>

      <Extensions>
        <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.SampleBackgroundTask">
          <BackgroundTasks>
            <Task Type="systemEvent" />
            <Task Type="timer" />
          </BackgroundTasks>
        </Extension>
        <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ServicingComplete">
          <BackgroundTasks>
            <Task Type="systemEvent"/>
          </BackgroundTasks>
        </Extension>
      </Extensions>
    </Application>
</Applications>
```

## <a name="declare-where-your-background-task-will-run"></a>声明将运行后台任务的位置

你可以指定运行后台任务的位置：

* 默认情况下，它们在 BackgroundTaskHost.exe 进程中运行。
* 与前台应用程序在同一进程内。
* 使用`ResourceGroup`将多个后台任务放到同一宿主进程中，或将其划分到不同进程中。
* 使用 `SupportsMultipleInstances` 在新进程中运行后台进程，每次触发新触发器时，该进程都会获取自己的资源限制（内存、cpu）。

### <a name="run-in-the-same-process-as-your-foreground-application"></a>与前台应用程序在同一进程内运行

下面的示例 XML 声明了在前台应用程序所处的同一进程中运行的后台任务。

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="ExecModelTestBackgroundTasks.ApplicationTriggerTask">
        <BackgroundTasks>
            <Task Type="systemEvent" />
        </BackgroundTasks>
    </Extension>
</Extensions>
```

当你指定 **EntryPoint** 时，你的应用程序将在触发器触发时接收对指定方法的回调。 如果你没有指定 **EntryPoint**，应用程序将通过 [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) 接收回调。  有关详细信息，请参阅[创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)。

### <a name="specify-where-your-background-task-runs-with-the-resourcegroup-attribute"></a>使用 ResourceGroup 属性指定运行后台任务的位置。

下面的示例 XML 声明了在独立于同一应用的其他后台任务实例的 BackgroundTaskHost.exe 进程中运行。 注意 `ResourceGroup` 属性，该属性可标识将同时运行哪些后台任务。

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.SessionConnectedTriggerTask" ResourceGroup="foo">
      <BackgroundTasks>
        <Task Type="systemEvent" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimeZoneTriggerTask" ResourceGroup="foo">
      <BackgroundTasks>
        <Task Type="systemEvent" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimerTriggerTask" ResourceGroup="bar">
      <BackgroundTasks>
        <Task Type="timer" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.ApplicationTriggerTask" ResourceGroup="bar">
      <BackgroundTasks>
        <Task Type="general" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.MaintenanceTriggerTask" ResourceGroup="foobar">
      <BackgroundTasks>
        <Task Type="general" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```

### <a name="run-in-a-new-process-each-time-a-trigger-fires-with-the-supportsmultipleinstances-attribute"></a>每次用 SupportsMultipleInstances 属性触发新触发器时，在新进程中运行

此示例声明在新进程中运行的后台任务，每次触发新触发器时，该进程会获取自己的资源限制（内存和 CPU）。 请注意使用可启用此行为的 `SupportsMultipleInstances`。 若要使用此属性，你必须面向 SDK 版本"10.0.15063"（Windows 10 创意者更新） 或更高版本。

```xml
<Package
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application ...>
            ...
            <Extensions>
                <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimerTriggerTask">
                    <BackgroundTasks uap4:SupportsMultipleInstances=“True”>
                        <Task Type="timer" />
                    </BackgroundTasks>
                </Extension>
            </Extensions>
        </Application>
    </Applications>
```

> [!NOTE]
> 不能与 `SupportsMultipleInstances` 一起指定 `ResourceGroup` 或 `ServerName`。

## <a name="related-topics"></a>相关主题

* [调试后台任务](debug-a-background-task.md)
* [注册后台任务](register-a-background-task.md)
* [后台任务指南](guidelines-for-background-tasks.md)
