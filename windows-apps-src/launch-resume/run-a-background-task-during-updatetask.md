---
author: TylerMSFT
title: 在 UWP 应用更新时运行后台任务
description: 了解如何创建在通用 Windows 平台 (UWP) 应用商店应用更新时运行的后台任务。
ms.author: twhitney
ms.date: 04/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp、 更新、 后台任务、 updatetask，后台任务
ms.localizationpriority: medium
ms.openlocfilehash: fcba2cb736f86cebc6d2664e2ec3b557d47c86d7
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2018
ms.locfileid: "2787059"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>在 UWP 应用更新时运行后台任务

了解如何编写运行您通用 Windows 平台 (UWP) 应用商店应用更新后的背景任务。

更新任务背景任务调用操作系统后用户在设备上安装应用程序安装更新。 这样，您的应用程序以执行初始化任务，如初始化新的推送通知通道，更新数据库架构等之前用户启动更新应用程序。

更新任务不同于启动后台任务使用[ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)触发器，因为在这种情况下您的应用程序必须至少运行一次更新数据以便注册**将激活该背景任务之前ServicingComplete**触发。  更新任务未注册，因此应用程序的从不已运行，但的升级，仍将具有触发其更新任务。

## <a name="step-1-create-the-background-task-class"></a>步骤 1： 创建背景任务类

为与其他类型的后台任务，您实现更新任务背景任务作为 Windows Runtime 组件。 若要创建此组件，请按照[创建和注册的进程外背景任务](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)的**创建背景任务类**部分中的步骤。 包括以下步骤：

- 将一个 Windows 运行时组件项目添加到你的解决方案。
- 到组件从您的应用程序创建一个引用。
- 组件中创建公用、 密封类实现[**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)。
- 实现[**Run**](https://msdn.microsoft.com/library/windows/apps/br224811)方法，这是运行更新任务时调用的所需的入口点。 如果您打算进行异步调用从背景任务，[创建和注册的进程外背景任务](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)说明如何在**运行**方法中使用延迟。

您无需注册此背景任务 （的"注册要运行的背景任务"部分中**创建和注册的进程外后台任务**主题），以使用更新任务。 这是因为您无需任何代码添加到您的应用程序注册该任务和应用程序没有至少运行一次更新注册背景任务前使用更新任务的主要原因。

下面的代码示例演示在 C# 中更新任务背景任务类的基本起始点。 需要为**公用**和**密封**背景任务类本身-和背景任务项目中的所有其他类的。 背景任务类必须从**IBackgroundTask**派生和公共 **（** ） 方法具有如下所示的签名：

```cs
using Windows.ApplicationModel.Background;

namespace BackgroundTasks
{
    public sealed class UpdateTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // your app migration/update code here
        }
    }
}
```

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>步骤 2： 声明您包清单中的背景任务

在 Visual Studio 解决方案资源管理器，右键单击**Package.appxmanifest** ，然后单击**查看代码**以查看程序包清单。 添加以下`<Extensions>`XML 声明更新任务：

```XML
<Package ...>
    ...
  <Applications>  
    <Application ...>  
        ...
      <Extensions>  
        <Extension Category="windows.updateTask"  EntryPoint="BackgroundTasks.UpdateTask">  
        </Extension>  
      </Extensions>

    </Application>  
  </Applications>  
</Package>
```

在上面 XML 中，确保`EntryPoint`属性设置为在更新任务类为名称。 Name 是区分大小写。

## <a name="step-3-debugtest-your-update-task"></a>步骤 3： 调试/测试更新任务

确保已部署您的应用程序到您的计算机，以便要更新的东西。

后台任务的执行 run （） 方法中设置断点。

![设置断点](images/run-func-breakpoint.png)

接下来，在解决方案资源管理器，右键单击您的应用程序项目 （不是背景任务项目），然后单击**属性**。 在应用程序属性窗口中，单击**调试**的左侧，，然后选择**未启动，但调试我的代码在启动时**:

![调试设置](images/do-not-launch-but-debug.png)

接下来，以确保 UpdateTask 被触发，增加包的版本号。 在解决方案资源管理器中，双击您的应用程序**Package.appxmanifest**文件以打开包设计器，然后更新的**内部**版本号:

![更新版本](images/bump-version.png)

现在，在 Visual Studio 2017 按 F5 时，将更新您的应用程序，系统将激活在后台 UpdateTask 组件。 将自动将调试器附加到后台进程中。 将获取命中您断点，并可以逐行执行您更新代码逻辑。

后台任务完成后，您可以在同一调试会话中启动前景应用程序从 Windows 开始菜单。 调试器将自动重新连接，这次为您前景色的过程，并且可以通过您的应用程序逻辑单步。

> [!NOTE]
> Visual Studio 2015 用户： 上面的步骤适用于 Visual Studio 2017。 如果您使用 Visual Studio 2015，您可以使用触发器和测试 UpdateTask，除 Visual Studio 将不将附加到相同的技术。 VS 2015 中的替代过程是安装程序将 UpdateTask 设置为其入口点， [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app)并触发直接从前景应用程序的执行。

## <a name="see-also"></a>另请参阅

[创建和注册进程外后台任务](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
