---
Description: ''
title: 屏幕读取器和硬件按钮事件
label: Screen readers and hardware button events
template: detail.hbs
ms.date: 02/20/2020
ms.topic: article
keywords: windows 10，uwp，辅助功能，讲述人，屏幕阅读器
ms.localizationpriority: medium
ms.openlocfilehash: 41c6ed531f21a922b407ff3ba5aae93afb8d917e
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234930"
---
# <a name="screen-readers-and-hardware-system-buttons"></a>屏幕阅读器和硬件系统按钮

屏幕阅读器（如[讲述人](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)）必须能够识别和处理硬件系统按钮事件并将其状态传达给用户。 在某些情况下，屏幕阅读器可能需要以独占方式处理这些硬件按钮事件，而不会让它们向上冒泡到其他处理程序。

从 Windows 10 版本2004开始，UWP 应用程序可以以与其他硬件按钮相同的方式侦听和处理**Fn**硬件系统按钮事件。 以前，此系统按钮仅作为其他硬件按钮报告其事件和状态的方式的修饰符。

> [!NOTE]
> Fn 按钮支持特定于 OEM，并可以包含开关/锁定（与按下键组合）等功能，以及相应的锁定指示灯（对于盲人或视觉障碍的用户可能不会有帮助）。

Fn 按钮事件通过[Windows](/uwp/api/windows.ui.input) SystemButtonEventController 命名空间中的新的[类](/uwp/api/windows.ui.input.systembuttoneventcontroller)公开。 SystemButtonEventController 对象支持以下事件：

- [SystemFunctionButtonPressed](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionbuttonpressed)
- [SystemFunctionButtonReleased](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionbuttonreleased)
- [SystemFunctionLockChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockchanged)
- [SystemFunctionLockIndicatorChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockindicatorchanged)

> [!Important]
> 如果这些事件已经由较高优先级处理程序处理，则 SystemButtonEventController 无法接收这些事件。

## <a name="examples"></a>示例

在下面的示例中，我们将演示如何基于 DispatcherQueue 创建[SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller) ，并处理此对象支持的四个事件。

按下 Fn 按钮时，通常会触发多个受支持的事件。 例如，按 Surface 键盘上的 Fn 按钮会同时触发 SystemFunctionButtonPressed、SystemFunctionLockChanged 和 SystemFunctionLockIndicatorChanged。

1. 在第一个代码段中，我们只包含所需的命名空间，并指定一些全局对象，包括[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue)和用于管理[SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller)线程的[DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)对象。

   然后，在注册[SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller)事件处理委托时，指定返回的[事件令牌](/uwp/cpp-ref-for-winrt/event-token)。

    ```cppwinrt
    namespace winrt
    {
        using namespace Windows::System;
        using namespace Windows::UI::Input;
    }

    ...

    // Declare related members
    winrt::DispatcherQueueController _queueController;
    winrt::DispatcherQueue _queue;
    winrt::SystemButtonEventController _controller;
    winrt::event_token _fnKeyDownToken;
    winrt::event_token _fnKeyUpToken;
    winrt::event_token _fnLockToken;
    ```

2. 还会为[SystemFunctionLockIndicatorChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockindicatorchanged)事件和布尔值指定事件标记，以指示应用程序是否处于 "学习模式" （其中，用户只是尝试浏览键盘，而不执行任何功能）。

    ```cppwinrt
    winrt::event_token _fnLockIndicatorToken;
    bool _isLearningMode = false;
    ```

3. 此第三个代码段包含[SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller)对象支持的每个事件对应的事件处理程序委托。

   每个事件处理程序都会公布发生的事件。 此外，FunctionLockIndicatorChanged 处理程序还控制应用是否处于 "学习" 模式（ `_isLearningMode` = true），这会阻止事件冒泡到其他处理程序，并允许用户浏览键盘功能，而无需实际执行该操作。

    ```cppwinrt
    void SetupSystemButtonEventController()
    {
        // Create dispatcher queue controller and dispatcher queue
        _queueController = winrt::DispatcherQueueController::CreateOnDedicatedThread();
        _queue = _queueController.DispatcherQueue();

        // Create controller based on new created dispatcher queue
        _controller = winrt::SystemButtonEventController::CreateForDispatcherQueue(_queue);

        // Add Event Handler for each different event
        _fnKeyDownToken = _controller->FunctionButtonPressed(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionButtonEventArgs& args)
            {
                // Mock function to read the sentence "Fn button is pressed"
                PronounceFunctionButtonPressedMock();
                // Set Handled as true means this event is consumed by this controller
                // no more targets will receive this event
                args.Handled(true);
            });

            _fnKeyUpToken = _controller->FunctionButtonReleased(
                [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionButtonEventArgs& args)
                {
                    // Mock function to read the sentence "Fn button is up"
                    PronounceFunctionButtonReleasedMock();
                    // Set Handled as true means this event is consumed by this controller
                    // no more targets will receive this event
                    args.Handled(true);
                });

        _fnLockToken = _controller->FunctionLockChanged(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionLockChangedEventArgs& args)
            {
                // Mock function to read the sentence "Fn shift is locked/unlocked"
                PronounceFunctionLockMock(args.IsLocked());
                // Set Handled as true means this event is consumed by this controller
                // no more targets will receive this event
                args.Handled(true);
            });

        _fnLockIndicatorToken = _controller->FunctionLockIndicatorChanged(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionLockIndicatorChangedEventArgs& args)
            {
                // Mock function to read the sentence "Fn lock indicator is on/off"
                PronounceFunctionLockIndicatorMock(args.IsIndicatorOn());
                // In learning mode, the user is exploring the keyboard. They expect the program
                // to announce what the key they just pressed WOULD HAVE DONE, without actually
                // doing it. Therefore, handle the event when in learning mode so the key is ignored
                // by the system.
                args.Handled(_isLearningMode);
            });
    }
    ```

## <a name="see-also"></a>另请参阅

[SystemButtonEventController 类](/uwp/api/windows.ui.input.systembuttoneventcontroller)
