---
author: mcleblanc
title: 在应用程序清单中声明后台任务
description: 通过在应用清单中将后台任务声明为扩展，以实现对后台任务的使用。
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
---

# 在应用程序清单中声明后台任务


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**BackgroundTasks 架构**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

通过在应用清单中将后台任务声明为扩展，以实现对后台任务的使用。

必须在应用清单中声明后台任务，否则你的应用将无法注册它们（仅引发异常）。 此外，必须在应用程序清单中声明后台任务才能通过认证。

本主题假定你已创建一个或多个后台任务类，并且你的应用注册了为响应至少一个触发器而运行的所有后台任务。

## 手动添加扩展


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

## 添加背景任务扩展


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

    **注意** 确保列出你所使用的每个触发器类型，否则后台任务将不会注册未声明的触发器类型（[**Register**](https://msdn.microsoft.com/library/windows/apps/br224772) 方法将失败并引发异常）。

    此代码段示例指示使用系统事件触发器和推送通知：

    ```xml
                <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
                  <BackgroundTasks>
                    <Task Type="systemEvent" />
                    <Task Type="pushNotification" />
                  </BackgroundTasks>
                </Extension>
    ```

    > **注意** 正常情况下，应用将在名为“BackgroundTaskHost.exe”的特殊进程中运行。 这可能会将一个 Executable 元素添加到 Extension 元素，从而可以在应用上下文中运行后台任务。 仅将 Executable 元素与需要它的后台任务结合使用，例如 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)。    

## 添加其他后台任务扩展


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

## 相关主题

* [调试后台任务](debug-a-background-task.md)
* [注册后台任务](register-a-background-task.md)
* [后台任务指南](guidelines-for-background-tasks.md)


<!--HONumber=May16_HO2-->


