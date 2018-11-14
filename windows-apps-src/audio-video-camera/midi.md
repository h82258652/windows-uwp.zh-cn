---
author: drewbatgit
ms.assetid: 9146212C-8480-4C16-B74C-D7F08C7086AF
description: 本文向你演示了如何枚举 MIDI（乐器数字接口）设备以及从通用 Windows 应用发送和接收 MIDI 消息。
title: MIDI
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 36d1e4afd620b871d4273699aea5c02cc9faec80
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6262891"
---
# <a name="midi"></a>MIDI



本文向你演示了如何枚举 MIDI（乐器数字接口）设备以及从通用 Windows 应用发送和接收 MIDI 消息。 Windows 10 支持通过 USB （类符合要求和最专有驱动程序），通过蓝牙 LE MIDI 的 MIDI (Windows 10 周年纪念版及更高版本)，并通过免费提供的第三方产品、 MIDI 以太网和路由 MIDI。

## <a name="enumerate-midi-devices"></a>枚举 MIDI 设备

在枚举和使用 MIDI 设备之前，将以下命名空间添加到你的项目。

[!code-cs[Using](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetUsing)]

将 [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868) 控件添加到你的 XAML 页面，该控件允许用户选择附加到系统的 MIDI 输入设备之一。 添加另一个控件以列出 MIDI 输出设备。

[!code-xml[MidiListBoxes](./code/MIDIWin10/cs/MainPage.xaml#SnippetMidiListBoxes)]

[**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) 方法的 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) 类用于枚举许多不同类型的由 Windows 识别的设备。 若要指明你仅希望使用该方法查找 MIDI 输入设备，请使用 [**MidiInPort.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn894779) 返回的选择器字符串。 **FindAllAsync** 将返回包含通过系统注册的每个 MIDI 输入设备的 **DeviceInformation** 的 [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395)。 如果返回的集合不包含任何项，则没有可用的 MIDI 输入设备。 如果集合中包含项，循环浏览 **DeviceInformation** 对象，并将每台设备的名称添加到 MIDI 输入设备 **ListBox**。

[!code-cs[EnumerateMidiInputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiInputDevices)]

枚举 MIDI 输出设备与枚举输入设备的工作原理完全相同，不同之处在于调用 **FindAllAsync** 时，你应该指定由 [**MidiOutPort.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn894845) 返回的选择器字符串。

[!code-cs[EnumerateMidiOutputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiOutputDevices)]



## <a name="create-a-device-watcher-helper-class"></a>创建设备观察程序帮助程序类

[**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459) 命名空间提供了 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446)，它可在设备已在系统中添加或删除时，或者设备的信息已更新时通知你的应用。 因为支持 MIDI 的应用通常会关注输入和输出设备，所以此示例将创建可实现 **DeviceWatcher** 模式的帮助程序类，以便可针对 MIDI 输入和 MIDI 输出设备使用相同的代码，从而避免重复。

将新类添加到你的项目以作为设备观察程序。 在此示例中，该类名为 **MyMidiDeviceWatcher**。 此部分中的剩余代码将用于实现帮助程序类。

将某些成员变量添加到类中：

-   监视设备更改的 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446) 对象。
-   包含一个实例的 MIDI 输入端口选择器字符串和另一个实例的 MIDI 输出端口选择器的设备选择器字符串。
-   使用可用设备的名称填充的 [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868) 控件。
-   从除 UI 线程之外的某个线程更新 UI 所需的 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)。

[!code-cs[WatcherVariables](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherVariables)]

添加用于从帮助程序类外部访问当前设备列表的 [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) 属性。

[!code-cs[DeclareDeviceInformationCollection](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetDeclareDeviceInformationCollection)]

在类构造函数中，调用方将传入 MIDI 设备选择器字符串、用于列出设备的 **ListBox**，以及更新 UI 所需的 **Dispatcher**。

调用 [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/br225427) 以创建 **DeviceWatcher** 类的新实例，从而传入 MIDI 设备选择器字符串。

为观察程序的事件处理程序注册处理程序。

[!code-cs[WatcherConstructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherConstructor)]

**DeviceWatcher** 具有以下事件：

-   [**Added**](https://msdn.microsoft.com/library/windows/apps/br225450) - 在新设备添加到系统中时引发。
-   [**Removed**](https://msdn.microsoft.com/library/windows/apps/br225453) - 在从系统中删除设备时引发。
-   [**Updated**](https://msdn.microsoft.com/library/windows/apps/br225458) - 在更新与现有设备关联的信息时引发。
-   [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451) - 在观察程序完成其所请求的设备类型枚举时引发。

在以上每个事件的事件处理程序中，将调用帮助程序方法 **UpdateDevices**，以通过当前设备列表更新 **ListBox**。 因为未在 UI 线程上调用 **UpdateDevices** 更新 UI 元素和这些事件处理程序，因此每个调用都必须打包在对 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 的调用中，这将导致指定代码在 UI 线程上运行。

[!code-cs[WatcherEventHandlers](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherEventHandlers)]

**UpdateDevices** 帮助程序方法调用 [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) 并通过已返回设备的名称更新 **ListBox**，如本文的前面部分所述。

[!code-cs[WatcherUpdateDevices](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherUpdateDevices)]

添加多个方法，若要启动观察程序，使用 **DeviceWatcher** 对象的 [**Start**](https://msdn.microsoft.com/library/windows/apps/br225454) 方法；若要停止观察程序，使用 [**Stop**](https://msdn.microsoft.com/library/windows/apps/br225456) 方法。

[!code-cs[WatcherStopStart](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherStopStart)]

提供析构函数以取消注册观察程序事件处理程序，并将设备观察程序设置为 null。

[!code-cs[WatcherDestructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherDestructor)]

## <a name="create-midi-ports-to-send-and-receive-messages"></a>创建 MIDI 端口以发送和接收消息

在页面的代码隐藏中，声明成员变量以保留 **MyMidiDeviceWatcher** 帮助程序类的两个实例：一个用于输入设备，一个用于输出设备。

[!code-cs[DeclareDeviceWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatchers)]

创建观察程序帮助程序类的新实例，从而传入设备选择器字符串、要填充的 **ListBox**，以及可通过页面的 **Dispatcher** 属性访问的 **CoreDispatcher** 对象。 然后，调用该方法以启动每个对象的 **DeviceWatcher**。

在启动每个 **DeviceWatcher** 后不久，它将完成枚举连接到系统的当前设备，并引发其 [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451) 事件，这将导致使用当前 MIDI 设备更新每个 **ListBox**。

[!code-cs[StartWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetStartWatchers)]

当用户在 MIDI 输入 **ListBox** 中选择某一项时，将引发 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) 事件。 在此事件的处理程序中，访问帮助程序类的 **DeviceInformationCollection** 属性以获取当前设备列表。 如果列表中存在条目，使用对应于 **ListBox** 控件 [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/br209768) 的索引选择 **DeviceInformation** 对象。

通过调用 [**MidiInPort.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn894776) 创建表示所选输入设备的 [**MidiInPort**](https://msdn.microsoft.com/library/windows/apps/dn894770) 对象，从而传入所选设备的 [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437) 属性。

为 [**MessageReceived**](https://msdn.microsoft.com/library/windows/apps/dn894781) 事件注册处理程序，该事件将在通过指定的设备接收 MIDI 消息时引发。

[!code-cs[DeclareMidiPorts](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareMidiPorts)]

[!code-cs[InPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetInPortSelectionChanged)]

调用 **MessageReceived** 处理程序时，该消息将包含在 **MidiMessageReceivedEventArgs** 的 [**Message**](https://msdn.microsoft.com/library/windows/apps/dn894783) 属性中。 消息对象的 [**Type**](https://msdn.microsoft.com/library/windows/apps/dn894726) 是 [**MidiMessageType**](https://msdn.microsoft.com/library/windows/apps/dn894786) 枚举中的值，用于指示已接收的消息类型。 消息数据取决于消息类型。 此示例可查看该消息是否是消息相关注释；如果是，则输出该消息的 MIDI 通道、注释和速度。

[!code-cs[MessageReceived](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetMessageReceived)]

输出设备 **ListBox** 的 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) 处理程序与输入设备的处理程序的工作原理相同，不同之处在于未注册任何事件处理程序。

[!code-cs[OutPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetOutPortSelectionChanged)]

创建输出设备后，通过针对要发送的消息类型创建新的 [**IMidiMessage**](https://msdn.microsoft.com/library/windows/apps/dn911508)，可以发送一条消息。 在此示例中，该消息为 [**NoteOnMessage**](https://msdn.microsoft.com/library/windows/apps/dn894817)。 调用 [**IMidiOutPort**](https://msdn.microsoft.com/library/windows/apps/dn894727) 对象的 [**SendMessage**](https://msdn.microsoft.com/library/windows/apps/dn894730) 方法以发送消息。

[!code-cs[SendMessage](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetSendMessage)]

当你的应用停用时，请务必清理你的应用资源。 取消注册你的事件处理程序，并将 MIDI 输入端口和输出端口对象设置为 null。 停止设备观察程序并将其设置为 null。

[!code-cs[CleanUp](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetCleanUp)]

## <a name="using-the-built-in-windows-general-midi-synth"></a>使用内置的 Windows 通用 MIDI 合成器

当你使用上面所述的技术枚举输出 MIDI 设备时，你的应用会发现一个名为“Microsoft GS 波形表合成器”的 MIDI 设备。 这是一款你可以从应用中使用的内置通用 MIDI 合成器。 但是，如果没有将该内置合成器的 SDK 扩展包含在你的项目中，将无法为此设备创建 MIDI 输出端口。

**将通用 MIDI 合成器 SDK 扩展包含在应用项目中**

1.  在 **“解决方案资源管理器”** 中，在你的项目下，右键单击 **“引用”**，然后选择 **“添加引用...”**
2.  展开 **“通用 Windows”** 节点。
3.  选择 **“扩展”**。
4.  从扩展列表中，选择“适用于通用 Windows 应用的 Microsoft 通用 MIDI DLS”****。
    > [!NOTE] 
    > 如果存在多个版本的扩展，请务必选择与应用所面向的目标匹配的版本。 你可以在项目“属性”的“应用程序”**** 选项卡上查看应用所面向的 SDK 版本。

 

 




