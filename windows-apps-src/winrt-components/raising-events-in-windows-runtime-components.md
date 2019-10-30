---
title: 在 Windows 运行时组件中引发事件
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: 如何在后台线程上引发用户定义的委托类型事件，使 JavaScript 能够接收该事件。
ms.date: 07/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 78bc43c26a73a6184e5788ad7d003813e567a8d0
ms.sourcegitcommit: 5dfa98a80eee41d97880dba712673168070c4ec8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2019
ms.locfileid: "73052021"
---
# <a name="raising-events-in-windows-runtime-components"></a>在 Windows 运行时组件中引发事件
> [!NOTE]
> 若要了解如何在[ C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) Windows 运行时组件中引发事件，请参阅[/WinRT 中C++的创作事件](../cpp-and-winrt-apis/author-events.md)。

如果你的 Windows 运行时组件在后台线程（工作线程）中引发了用户定义的委托类型的事件，并且你希望 JavaScript 能够接收该事件，则可以使用以下方法之一实现和/或引发它：

-   （选项 1）通过 [Windows.UI.Core.CoreDispatcher](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher) 引发该事件，以便将该事件封送到 JavaScript 线程上下文。 尽管通常情况下，这是最佳选项，但在某些情况中，它可能不会提供最快性能。
-   （选项 2）使用 [Windows.Foundation.EventHandler](https://docs.microsoft.com/uwp/api/windows.foundation.eventhandler)&lt;Object&gt;，但会丢失类型信息（即丢失事件类型信息）。 如果选项 1 不可行或者其性能不够，这将是不错的第二选项（如果可接受丢失类型信息）。
-   （选项 3）为组件创建你自己的代理和存根。 此选项是最难以实现的选项，但它会保留类型信息，并且在需要的情况下，可能提供比选项 1 更为出色的性能。

如果你只是在后台线程中引发了一个事件而不使用其中任一选项，JavaScript 客户端将不会接收该事件。

## <a name="background"></a>后台

所有 Windows 运行时组件和应用基本都是 COM 对象，不管你使用哪种语言创建它们。 在 Windows API 中，大多数组件都是敏捷的 COM 对象，它们可以与后台线程和 UI 线程上的对象很好地进行对等通信。 如果 COM 对象无法变得敏捷，那么它需要帮助程序对象（即代理和存根）来与跨 UI 线程后台的线程边界的其他 COM 对象进行通信。 （就 COM 而言，这称为线程单元之间的通信。）

Windows API 中的大多数对象是敏捷对象，或者内置了代理和存根。 但是，无法为泛型类型创建代理和存根，例如 Windows.Foundation.[TypedEventHandler&lt;TSender, TResult&gt;](https://docs.microsoft.com/uwp/api/windows.foundation.typedeventhandler)，因为它们在提供类型参数之前都不是完整的类型。 这只是缺少代理或存根的 JavaScript 客户端问题，但如果你希望从 JavaScript 以及 C++ 或 .NET 语言中都可以使用你的组件，则必须使用以下三个选项之一。

## <a name="option-1-raise-the-event-through-the-coredispatcher"></a>（选项 1）通过 CoreDispatcher 引发事件

你可以通过使用 [Windows.UI.Core.CoreDispatcher](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher) 来发送任何用户定义的委托类型的事件，而且 JavaScript 也能够接收它们。 如果你不确定使用哪个选项，可以先尝试这个选项。 如果在事件引发和事件处理之间出现延迟问题，请尝试另外一个选项。

以下示例演示了如何使用 CoreDispatcher 引发强类型的事件。 请注意，类型参数为 Toast，而不是 Object。

```csharp
public event EventHandler<Toast> ToastCompletedEvent;
private void OnToastCompleted(Toast args)
{
    var completedEvent = ToastCompletedEvent;
    if (completedEvent != null)
    {
        completedEvent(this, args);
    }
}

public void MakeToastWithDispatcher(string message)
{
    Toast toast = new Toast(message);
    // Assume you have a CoreDispatcher at class scope.
    // Initialize it here, then use it from the background thread.
    var window = Windows.UI.Core.CoreWindow.GetForCurrentThread();
    m_dispatcher = window.Dispatcher;

    Task.Run( () =>
    {
        if (ToastCompletedEvent != null)
        {
            m_dispatcher.RunAsync(CoreDispatcherPriority.Normal,
            new DispatchedHandler(() =>
            {
                this.OnToastCompleted(toast);
            })); // end m_dispatcher.RunAsync
         }
     }); // end Task.Run
}
```

## <a name="option-2-use-eventhandlerltobjectgt-but-lose-type-information"></a>（选项 2）使用 EventHandler&lt;Object&gt;，但会丢失类型信息。

另外一种从后台线程发送事件的方法是使用 [Windows.Foundation.EventHandler](https://docs.microsoft.com/uwp/api/windows.foundation.eventhandler)&lt;Object&gt; 作为事件的类型。 Windows 提供了此泛型类型的具体实例化，并为其提供了一个代理和存根。 缺点是会丢失事件参数和发送者的类型信息。 C++ 和 .NET 客户端必须通过文档了解在收到事件时要转换回的类型。 JavaScript 客户端不需要原始类型信息。 它们会根据元数据中的名称查找参数属性。

本示例演示了如何在 C# 中使用 Windows.Foundation.EventHandler&lt;Object&gt;：

```csharp
public sealed Class1
{
// Declare the event
public event EventHandler<Object> ToastCompletedEvent;

    // Raise the event
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message);
        // Fire the event from a background thread to allow this thread to continue
        Task.Run(() =>
        {
            if (ToastCompletedEvent != null)
            {
                OnToastCompleted(toast);
            }
        });
    }

    private void OnToastCompleted(Toast args)
    {
        var completedEvent = ToastCompletedEvent;
        if (completedEvent != null)
        {
            completedEvent(this, args);
        }
    }
}
```

你可以在 JavaScript 端上使用此事件，如下所示：

```javascript
toastCompletedEventHandler: function (event) {
   var toastType = event.toast.toastType;
   document.getElementById("toasterOutput").innerHTML = "<p>Made " + toastType + " toast</p>";
}
```

## <a name="option-3-create-your-own-proxy-and-stub"></a>（选项 3）创建你自己的代理和存根

若要在类型信息保留完整的用户定义的事件类型上获取潜在的性能，你需要创建自己的代理和存根对象，并将它们嵌入你的应用包中。 通常，只有在另外两个选项都不合适时（这种情况很少出现），你才需要使用此选项。 另外，无法保证此选项会提供比另外两个选项更好的性能。 实际性能取决于多种因素。 使用 Visual Studio 探查器或其他分析工具来测量应用程序中的实际性能，并确定该事件是否真的影响性能。

本文的其余部分演示如何使用 C# 创建基本 Windows 运行时组件，然后使用 C++ 为代理和存根创建 DLL，从而使 JavaScript 可以在异步操作中使用由组件引发的 Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt; 事件。 （你也可以使用 C++ 或 Visual Basic 来创建组件。 与创建代理和存根相关的步骤都相同。）本演练基于创建 Windows 运行时进程内组件示例 (C++/CX)，并帮助说明其目的。

本演练包含以下部分：

-   你将在此处创建两个基本 Windows 运行时类。 一个类显示类型 [Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;](https://docs.microsoft.com/uwp/api/windows.foundation.typedeventhandler) 的事件，而另一个类是作为 TValue 的参数返回到 JavaScript 的类型。 这些类在你完成后面的步骤之前无法与 JavaScript 通信。
-   此应用会激活主类对象、调用方法并处理由 Windows 运行时组件引发的事件。
-   生成代理和存根类的工具需要这些内容。
-   然后，你使用 IDL 文件为代理和存根生成 C 源代码。
-   注册代理/存根对象，以便 COM 运行时可以找到它们，以及引用应用项目中的代理/存根 DLL。

## <a name="to-create-the-windows-runtime-component"></a>创建 Windows 运行时组件

在 Visual Studio 中的菜单栏上，依次选择“文件”&gt;“新建项目”。 在“新建项目”对话框中，依次展开JavaScript&gt;“通用 Windows”，然后选择“空白应用”。 将该项目命名为 ToasterApplication，然后选择“确定”按钮。

向解决方案中添加一个 C# Windows 运行时组件：在“解决方案资源管理器”中，打开解决方案的快捷菜单，然后依次选择“添加”&gt;“新建项目”。 展开 **" C# Visual&gt;Microsoft Store** ，然后选择" **Windows 运行时组件**"。 将该项目命名为 ToasterComponent，然后选择“确定”按钮。 ToasterComponent 将是你在后面步骤中创建的组件的根命名空间。

在“解决方案资源管理器”中，打开解决方案的快捷菜单，然后选择“属性”。 在“属性页”对话框中，选择左侧窗格中的“配置属性”，然后在该对话框顶部，将“配置”设置为“调试”以及将“平台”设置为 x86、x64 或 ARM。 选择“确定”按钮。

**重要** 平台 = 任何 CPU 都不起作用，因为它对你稍后将添加到解决方案中的本机代码 Win32 DLL 无效。

在“解决方案资源管理器”中，将 class1.cs 重命名为 ToasterComponent.cs，以便它与项目名相匹配。 Visual Studio 会自动重命名文件中的类，以便与新的文件名相匹配。

在 .cs 文件中，添加适用于 Windows.Foundation 命名空间的 using 指令以便将 TypedEventHandler 引入到作用域。

当你需要代理和存根时，你的组件必须使用接口来显示其公共成员。 在 ToasterComponent.cs 中，为 Toaster 定义一个接口，并为该 Toaster 生成的 Toast 定义另一个接口。

**请注意** C#在中可以跳过此步骤。 改为先创建一个类，然后打开其快捷菜单并依次选择“重构”&gt;“提取接口”。 在生成的代码中，手动提供接口公共辅助功能。

```csharp
    public interface IToaster
        {
            void MakeToast(String message);
            event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

        }
        public interface IToast
        {
            String ToastType { get; }
        }
```

IToast 接口具有一个字符串，可以检索该字符串来描述 Toast 的类型。 IToaster 接口具有一种生成 Toast 的方法，以及用来指示是否已生成该 Toast 的事件。 因为此事件会返回 Toast 的特定部分（即类型），因此它被称为类型化事件。

接下来，我们需要可实现这些接口而且是公共和密封型的类，以便可以从你稍后会进行编程的 JavaScript 应用访问它们。

```csharp
    public sealed class Toast : IToast
        {
            private string _toastType;

            public string ToastType
            {
                get
                {
                    return _toastType;
                }
            }
            internal Toast(String toastType)
            {
                _toastType = toastType;
            }

        }
        public sealed class Toaster : IToaster
        {
            public event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

            private void OnToastCompleted(Toast args)
            {
                var completedEvent = ToastCompletedEvent;
                if (completedEvent != null)
                {
                    completedEvent(this, args);
                }
            }

            public void MakeToast(string message)
            {
                Toast toast = new Toast(message);
                // Fire the event from a thread-pool thread to enable this thread to continue
                Windows.System.Threading.ThreadPool.RunAsync(
                (IAsyncAction action) =>
                {
                    if (ToastCompletedEvent != null)
                    {
                        OnToastCompleted(toast);
                    }
                });
           }
        }
```

在前面的代码中，我们将创建 Toast，然后向上旋转线程池工组项以引发通知。 尽管 IDE 可能会建议你将 await 关键字应用到异步调用，但是在这种情况下没有必要，因为该方法不会执行任何依赖操作结果的工作。

**请注意** 前面代码中的异步调用只使用 RunAsync，以演示一种在后台线程上激发事件的简单方式。 你可以编写这种特定方法（如以下示例所示），并且它会正常工作，因为 .NET 任务计划程序会自动封装回调到 UI 线程的 async/await。
  
```csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
```

如果现在生成项目，项目则会干净彻底地生成。

## <a name="to-program-the-javascript-app"></a>对 JavaScript 应用进行编程

现在，我们可以向 JavaScript 应用添加一个按钮，使它使用我们刚刚定义为生成 toast 的类。 在我们执行此操作之前，必须添加对刚刚创建的 ToasterComponent 项目的引用。 在“解决方案资源管理器”中，打开 ToasterApplication 项目的快捷菜单，选择**添加 &gt; 引用**，然后选择**添加新引用**按钮。 在“添加引用”对话框中，在“解决方案”下的左窗格中，选择组件项目，然后在中间的窗格中选择 ToasterComponent。 选择“确定”按钮。

在“解决方案资源管理器”中，打开 ToasterApplication 项目的快捷菜单，然后选择**设置为启动项目**。

在 default.js 文件的末尾处添加一个命名空间，以包括调用组件以及被它回调的函数。 命名空间将包含两个函数，一个用于生成 toast，一个用于处理 toast 完成事件。 实现 makeToast 将创建 Toaster 对象，注册事件处理程序并生成 toast。 到目前为止，该注册事件处理程序未执行过多的操作，如下所示：

```javascript
    WinJS.Namespace.define("ToasterApplication"), {
       makeToast: function () {

          var toaster = new ToasterComponent.Toaster();
          //toaster.addEventListener("ontoastcompletedevent", ToasterApplication.toastCompletedEventHandler);
          toaster.ontoastcompletedevent = ToasterApplication.toastCompletedEventHandler;
          toaster.makeToast("Peanut Butter");
       },

       toastCompletedEventHandler: function(event) {
           // The sender of the event (the delegate's first type parameter)
           // is mapped to event.target. The second argument of the delegate
           // is contained in event, which means in this case event is a
           // Toast class, with a toastType string.
           var toastType = event.toastType;

           document.getElementById('toastOutput').innerHTML = "<p>Made " + toastType + " toast</p>";
        },
    });
```

必须将 makeToast 函数与某个按钮挂钩。 更新 default.html 以包含一个按钮和一些用于输出生成 toast 结果的空间：

```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
```

如果我们没有使用 TypedEventHandler，现在则可以在本地计算机上运行应用，然后单击按钮以生成 toast。 但在我们的应用中，无任何反应。 若要找出原因，让我们调试触发 ToastCompletedEvent 的托管代码。 停止此项目，然后在菜单栏上选择**调试 &gt; Toaster 应用程序属性**。 将**调试程序类型**更改为**仅限托管**。 再次在菜单栏上选择**调试 &gt; 异常**，然后选择**公共语言运行时异常**。

现在运行该应用，然后单击 make-toast 按钮。 调试程序会捕获无效的强制转换异常。 尽管从消息中来看并不明显，但这是因为该接口缺少代理才导致了此异常。

![缺少代理](./images/debuggererrormissingproxy.png)

为组件创建代理和存根的第一步是向接口添加唯一 ID 或 GUID。 但是，要使用的 GUID 格式有所不同，具体取决于是使用 C#、Visual Basic 或其他 .NET 语言，还是在 C++ 中进行的编码。

## <a name="to-generate-guids-for-the-components-interfaces-c-and-other-net-languages"></a>为组件的接口生成 GUID（C# 和其他 .NET 语言）

在菜单栏上，选择“工具”&gt;“创建 GUID”。 在对话框中，选择“5. \[Guid （"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx .。。xxxx "）\]。 选择“新建 GUID”按钮，然后选择“复制”按钮。

![GUID 生成器工具](./images/guidgeneratortool.png)

返回到接口定义，然后将新的 GUID 粘贴到 IToaster 接口前，如以下示例中所示。 （请勿使用示例中的 GUID。 每个唯一接口应有其自己的 GUID。）

```cpp
[Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
```

为 System.Runtime.InteropServices 命名空间添加 using 指令。

针对 IToast 接口重复这些步骤。

## <a name="to-generate-guids-for-the-components-interfaces-c"></a>为组件的接口 (C++) 生成 GUID

在菜单栏上，选择“工具”&gt;“创建 GUID”。 在对话框中，选择“3. 静态常量结构 GUID = {...}”。 选择“新建 GUID”按钮，然后选择“复制”按钮。

将 GUID 粘贴到 IToaster 接口定义前。 粘贴后，GUID 应类似于以下示例。 （请勿使用示例中的 GUID。 每个唯一接口应有其自己的 GUID。）
```cpp
// {F8D30778-9EAF-409C-BCCD-C8B24442B09B}
    static const GUID <<name>> = { 0xf8d30778, 0x9eaf, 0x409c, { 0xbc, 0xcd, 0xc8, 0xb2, 0x44, 0x42, 0xb0, 0x9b } };
```
为 Windows.Foundation.Metadata 添加 using 指令，以将 GuidAttribute 引入范围中。

现在手动将常量 GUID 转换为 GuidAttribute，使其格式如以下示例中所示。 注意，方括号和圆括号替换了大括号，并且删除了尾随分号。
```cpp
// {E976784C-AADE-4EA4-A4C0-B0C2FD1307C3}
    [GuidAttribute(0xe976784c, 0xaade, 0x4ea4, 0xa4, 0xc0, 0xb0, 0xc2, 0xfd, 0x13, 0x7, 0xc3)]
    public interface IToaster
    {...
```
针对 IToast 接口重复这些步骤。

在接口具有了唯一 ID 后，我们便可以通过将 .winmd 文件提供到 winmdidl 命令行工具中来创建 IDL 文件，然后将该 IDL 文件提供到 MIDL 命令行工具中来为代理和存根生成 C 源代码。 如果我们如以下步骤中所示创建生成后事件，Visual Studio 则会为我们执行此操作。

## <a name="to-generate-the-proxy-and-stub-source-code"></a>生成代理和存根源代码

若要添加自定义生成后事件，请在“解决方案资源管理器”中，打开 ToasterComponent 项目的快捷菜单，然后选择“属性”。 在属性页的左窗格中，选择“生成事件”，然后选择“编辑生成后事件”按钮。 将以下命令添加到生成后事件命令行中。 （必须首先调用批处理文件设置环境变量以查找 winmdidl 工具。）

```cpp
call "$(DevEnvDir)..\..\vc\vcvarsall.bat" $(PlatformName)
winmdidl /outdir:output "$(TargetPath)"
midl /metadata_dir "%WindowsSdkDir%References\CommonConfiguration\Neutral" /iid "$(ProjectDir)$(TargetName)_i.c" /env win32 /h "$(ProjectDir)$(TargetName).h" /winmd "Output\$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(ProjectDir)dlldata.c" /proxy "$(ProjectDir)$(TargetName)_p.c" "Output\$(TargetName).idl"
```

**重要**  对于 ARM 或 x64 项目配置，请将 MIDL/env 参数更改为 x64 或 arm32。

为了确保每次 .winmd 文件更改时都会重新生成 IDL 文件，请将**运行生成后事件**更改为**生成更新项目输出时**。
"生成事件" 属性页应如下所示： ![生成事件](./images/buildevents.png)

重新生成解决方案以生成并编译 IDL。

可以通过在 ToasterComponent 项目目录中查找 ToasterComponent.h、ToasterComponent_i.c、ToasterComponent_p.c 和 dlldata.c 来验证 MIDL 是否正确编译了解决方案。

## <a name="to-compile-the-proxy-and-stub-code-into-a-dll"></a>将代理和存根代码编译到 DLL 中

在有了所需的文件后，便可以将它们编译为生成 DLL，即一个 C++ 文件。 为了使此操作尽量简单，请添加新的项目以支持生成代理。 打开 ToasterApplication 解决方案的快捷菜单，然后选择**添加 > 新项目**。 在 "**新建项目**" 对话框的左窗格中，展开 **" C++ Visual&gt;windows&gt;通用 windows**"，然后在中间窗格中选择 " **DLL （UWP 应用）** "。 （请注意，这不是 C++ Windows 运行时组件项目。）将项目命名为 Proxies，然后选择**确定**按钮。 当 C# 类中发生更改时，这些文件将被生成后事件更新。

默认情况下，Proxies 项目会生成标头 .h 文件和 C++ .cpp 文件。 由于 DLL 是从由 MIDL 生成的文件构建的，因此不需要 .h 和 .cpp 文件。 在“解决方案资源管理器”中，打开它们的快捷菜单，选择**删除**，然后确认删除。

当项目中没有任何文件后，便可以重新添加 MIDL 生成的文件。 打开 Proxies 项目的快捷菜单，然后选择**添加 > 现有项目**。 在对话框中，导航到 ToasterComponent 项目目录，然后选择这些文件：ToasterComponent.h、ToasterComponent_i.c、ToasterComponent_p.c 和 dlldata.c 文件。 选择**添加**按钮。

在 Proxies 项目中，创建 .def 文件以定义 dlldata.c 中所述的 DLL 导出。 打开该项目的快捷菜单，然后选择**添加 > 新项目**。 在对话框的左窗格中选择“代码”，然后在中间的窗格中选择“模块定义文件”。 将文件命名为 proxies.def，然后选择**添加**按钮。 打开此 .def 文件并进行修改，以包括 dlldata.c 中所定义的 EXPORTS：

```cpp
EXPORTS
    DllCanUnloadNow         PRIVATE
    DllGetClassObject       PRIVATE
```

如果现在生成项目，则将失败。 若要正确编译此项目，需要更改编译和链接项目的方式。 在“解决方案资源管理器”中，打开 Proxies 项目的快捷菜单，然后选择**属性**。 按以下步骤更改属性页。

在左窗格中，选择 **C/C++ > 预处理器**，接着在右窗格中选择**预处理器定义**，选择下拉箭头按钮，然后选择**编辑**。 在框中添加这些定义：

```cpp
WIN32;_WINDOWS
```
在 **C/C++ > 预编译标头**下，将**预编译标头**更改为**不使用预编译标头**，然后选择**应用**按钮。

在**链接器 > 常规**下，将**忽略导入库**更改为**是**，然后选择**应用**按钮。

在**链接器 > 输入**下，选中**其他依赖关系**，选择下拉箭头按钮，然后选择**编辑**。 在框中添加此文本：

```cpp
rpcrt4.lib;runtimeobject.lib
```

请勿将这些库直接粘贴到列表行中。 使用**编辑**框确保 Visual Studio 中的 MSBuild 将维持正确的其他依赖关系。

进行这些更改后，在**属性页**对话框中选择**确定**按钮。

接下来，与 ToasterComponent 项目建立依赖关系。 这将确保将在代理项目生成之前生成 Toaster。 这是必需的操作，因为 Toaster 项目负责生成文件以生成代理。

打开 Proxies 项目的快捷菜单，然后选择“项目依赖关系”。 选中复选框，以指示 Proxies 项目依赖于 ToasterComponent 项目，从而确保 Visual Studio 按正确的顺序生成它们。

在 Visual Studio 菜单栏上选择**生成 > 重新生成解决方案**以验证解决方案是否正确生成。


## <a name="to-register-the-proxy-and-stub"></a>注册代理和存根

在 ToasterApplication 项目中，打开 package.appxmanifest 的快捷菜单，然后选择**打开方式**。 在“打开方式”对话框中，选择 **XML 文本编辑器**，然后选择**确定**按钮。 我们将粘贴在提供 windows.activatableClass.proxyStub 扩展注册的某些 XML 中，它们基于代理中的 GUID。 若要查找要在 .appxmanifest 文件中使用的 GUID，请打开 ToasterComponent_i.c。 查找与以下示例中的条目类似的条目。 此外，请注意 IToast、IToaster 和第三个接口（一个类型化事件处理程序，它有以下两个参数：Toaster 和 Toast）的定义。 这与在 Toaster 类中所定义的事件相匹配。 请注意，IToast 和 IToaster 的 GUID 与在 C# 文件中的接口上定义的 GUID 相匹配。 因为类型化事件处理程序接口是自动生成的，因此此接口的 GUID 也会自动生成。

```cpp
MIDL_DEFINE_GUID(IID, IID___FITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast,0x1ecafeff,0x1ee1,0x504a,0x9a,0xf5,0xa6,0x8c,0x6f,0xb2,0xb4,0x7d);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToast,0xF8D30778,0x9EAF,0x409C,0xBC,0xCD,0xC8,0xB2,0x44,0x42,0xB0,0x9B);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToaster,0xE976784C,0xAADE,0x4EA4,0xA4,0xC0,0xB0,0xC2,0xFD,0x13,0x07,0xC3);
```

现在我们复制 GUID，将它们粘贴到我们添加并命名为 Extensions 的节点中的 package.appxmanifest 中，然后对其进行重格式化。 清单条目类似于以下示例，但同样，请记住使用你自己的 GUID。 请注意，XML 中的 ClassId GUID 与 ITypedEventHandler2 相同。 这是因为该 GUID 是 ToasterComponent_i.c 中列出的第一个 GUID。 此处的 GUID 不区分大小写。 你可以返回到接口定义并获取正确格式的 GuidAttribute 值，而无需手动重格式化 IToast 和 IToaster 的 GUID。 在 C++ 中，注释中有一个格式正确的 GUID。 在任何情况下，必须手动重格式化用于 ClassId 和事件处理程序的 GUID。

```cpp
      <Extensions> <!--Use your own GUIDs!!!-->
        <Extension Category="windows.activatableClass.proxyStub">
          <ProxyStub ClassId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d">
            <Path>Proxies.dll</Path>
            <Interface Name="IToast" InterfaceId="F8D30778-9EAF-409C-BCCD-C8B24442B09B"/>
            <Interface Name="IToaster"  InterfaceId="E976784C-AADE-4EA4-A4C0-B0C2FD1307C3"/>  
            <Interface Name="ITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast" InterfaceId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d"/>
          </ProxyStub>      
        </Extension>
      </Extensions>
```

将扩展 XML 节点粘贴为包节点的直接子节点，以及如资源节点等对等节点。

在继续之前，请务必确保：

-   ProxyStub ClassId 已设置为 ToasterComponent\_文件中的第一个 GUID。 使用此文件中为 classId 定义的第一个 GUID。 （这可能与 ITypedEventHandler2 的 GUID 相同。）
-   路径是代理二进制文件的包相对路径。 （在本演练中，proxies.dll 与 ToasterApplication.winmd 位于同一文件夹中。）
-   GUID 的格式正确。 （这很容易出错。）
-   清单中的接口 Id 与 ToasterComponent\_文件中的 Iid 匹配。
-   接口名称在清单中是唯一的。 由于系统不使用它们，因此你可以选择值。 较好的做法是选择与你定义的接口明确匹配的接口名称。 对于生成的接口，名称应该指示生成的接口。 你可以使用\_ToasterComponent 文件来帮助你生成接口名称。

如果你尝试现在运行解决方案，将收到以下错误：proxies.dll 不是有效负载的一部分。 在 ToasterApplication 项目中打开**引用**文件夹的快捷菜单，然后选择**添加引用**。 选中 Proxies 项目旁边的复选框。 此外，请确保也选中了 ToasterComponent 旁边的复选框。 选择“确定”按钮。

项目现在应该生成了。 运行该项目，然后验证你是否可以生成 Toast。

## <a name="related-topics"></a>相关主题

* [使用 C++/CX 创建 Windows 运行时组件](creating-windows-runtime-components-in-cpp.md)
