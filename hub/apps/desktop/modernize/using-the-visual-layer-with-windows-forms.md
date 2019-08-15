---
title: 可视化层中使用 Windows 窗体
description: 了解使用 Visual 层 Api 与现有 Windows 窗体内容结合使用来创建高级的动画和效果的方法。
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 23515f8254b026b255491a90c1c8b3a2a8ab12ba
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215164"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>可视化层中使用 Windows 窗体

可以使用 Windows 运行时组合 Api (也称为[可视化层](/windows/uwp/composition/visual-layer)) 在 Windows 窗体应用程序以创建现代的轻型注册 Windows 10 用户体验。

GitHub 上提供了本教程的完整代码：[Windows 窗体 HelloComposition 示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)。

## <a name="prerequisites"></a>先决条件

托管 API UWP 具有这些系统必备组件。

- 我们假定您具有一定熟悉使用 Windows 窗体和 UWP 应用开发。 有关详细信息，请参阅：
  - [Windows 窗体入门](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [开始使用 Windows 10 应用](/windows/uwp/get-started/)
  - [增强适用于 Windows 10 桌面应用程序](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET framework 4.7.2 或更高版本
- Windows 10 版本 1803 版或更高版本
- Windows 10 SDK 17134 或更高版本

## <a name="how-to-use-composition-apis-in-windows-forms"></a>如何在 Windows 窗体中使用组合 Api

在本教程中，可以创建一个简单的 Windows 窗体 UI 和向其中添加动画的组合元素。 Windows 窗体和组合组件保持简单，但所示的互操作代码无论是相同的组件的复杂性。 已完成的应用外观如下所示。

![在正在运行的应用程序 UI](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>创建 Windows 窗体项目

第一步是创建 Windows 窗体应用程序项目，其中包括用于 UI 的应用程序定义和主窗体。

若要在视觉对象中创建新的 Windows 窗体应用程序项目C#名为_HelloComposition_:

1. 打开 Visual Studio 并选择**文件** > **新建** > **项目**。<br/>**新的项目**对话框随即打开。
1. 下**已安装**类别中，展开**Visual C#** 节点，，然后选择**Windows Desktop**。
1. 选择**Windows 窗体应用 (.NET Framework)** 模板。
1. 输入的名称_HelloComposition，_ 选择框架 **.NET Framework 4.7.2**，然后单击**确定**。

Visual Studio 创建项目并打开名为 Form1.cs 的默认应用程序窗口的设计器。

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>将项目配置为使用 Windows 运行时 Api

若要使用 Windows 运行时 (WinRT) Api 在 Windows 窗体应用中，您需要配置 Visual Studio 项目以访问 Windows 运行时。 此外，向量广泛用于由组合 Api，因此需要添加所需使用矢量的引用。

NuGet 包是可用于处理这两种要求。 安装这些包，需要将引用添加到你的项目的最新版本。  

- [Microsoft.Windows.SDK.Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) （需要默认包管理格式设置为 PackageReference。）
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> 尽管我们建议使用 NuGet 包配置项目，您可以手动添加所需的引用。 有关详细信息，请参阅[增强适用于 Windows 10 桌面应用程序](/windows/uwp/porting/desktop-to-uwp-enhance)。 下表显示了您需要将引用添加到的文件。

|文件|Location|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.UniversalApiContract\<*version*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.FoundationContract\<*version*>|
|System.Numerics.Vectors.dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.Numerics.dll|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="create-a-custom-control-to-manage-interop"></a>创建自定义控件以管理互操作

到内容使用可视化层创建的主机，您可以创建自定义控件派生自[控制](/dotnet/api/system.windows.forms.control)。 此控件可让你访问窗口[处理](/dotnet/api/system.windows.forms.control.handle)，若要创建你可视化层的内容的容器需要的。

这是配置的你在其中完成大部分托管组合 Api。 在控件中，您将使用[平台调用服务 (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code)并[COM 互操作](/dotnet/api/system.runtime.interopservices.comimportattribute)组合 Api 引入您的 Windows 窗体应用程序。 PInvoke 和 COM 互操作的详细信息，请参阅[与非托管代码交互操作](/dotnet/framework/interop/index)。

> [!TIP]
> 如果需要检查的教程，请确保所有代码都都在正确位置放置在演练本教程末尾的完整代码。

1. 将新的自定义控制文件添加到项目中，这些派生自[控制](/dotnet/api/system.windows.forms.control)。
    - 在中**解决方案资源管理器**，右键单击_HelloComposition_项目。
    - 在上下文菜单中，选择**外** > **新项...** .
    - 在中**添加新项**对话框中，选择**自定义控件**。
    - 将控件_CompositionHost.cs_，然后单击**添加**。 在设计视图中打开 CompositionHost.cs。

1. 切换到 CompositionHost.cs 的代码视图，并将以下代码添加到类。

    ```csharp
    // Add
    // using Windows.UI.Composition;

    IntPtr hwndHost;
    object dispatcherQueue;
    protected ContainerVisual containerVisual;
    protected Compositor compositor;

    private ICompositionTarget compositionTarget;

    public Visual Child
    {
        set
        {
            if (compositor == null)
            {
                InitComposition(hwndHost);
            }
            compositionTarget.Root = value;
        }
    }
    ```

1. 将代码添加到构造函数。

    在构造函数中，可以调用_InitializeCoreDispatcher_并_InitComposition_方法。 在下一步骤中创建这些方法。

    ```csharp
    public CompositionHost()
    {
        InitializeComponent();

        // Get the window handle.
        hwndHost = Handle;

        // Create dispatcher queue.
        dispatcherQueue = InitializeCoreDispatcher();

        // Build Composition tree of content.
        InitComposition(hwndHost);
    }

1. Initialize a thread with a [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). The core dispatcher is responsible for processing window messages and dispatching events for WinRT APIs. New instances of **Compositor** must be created on a thread that has a CoreDispatcher.
    - Create a method named _InitializeCoreDispatcher_ and add code to set up the dispatcher queue.

    ```csharp
    // Add
    // using System.Runtime.InteropServices;

    private object InitializeCoreDispatcher()
    {
        DispatcherQueueOptions options = new DispatcherQueueOptions();
        options.apartmentType = DISPATCHERQUEUE_THREAD_APARTMENTTYPE.DQTAT_COM_STA;
        options.threadType = DISPATCHERQUEUE_THREAD_TYPE.DQTYPE_THREAD_CURRENT;
        options.dwSize = Marshal.SizeOf(typeof(DispatcherQueueOptions));

        object queue = null;
        CreateDispatcherQueueController(options, out queue);
        return queue;
    }
    ```

    - 调度程序队列需要的 PInvoke 声明。 将此声明放置在类的代码末尾。 （我们将此代码以使类代码井然有序的区域内。）

    ```csharp
    #region PInvoke declarations

    //typedef enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
    //{
    //    DQTAT_COM_NONE,
    //    DQTAT_COM_ASTA,
    //    DQTAT_COM_STA
    //};
    internal enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
    {
        DQTAT_COM_NONE = 0,
        DQTAT_COM_ASTA = 1,
        DQTAT_COM_STA = 2
    };

    //typedef enum DISPATCHERQUEUE_THREAD_TYPE
    //{
    //    DQTYPE_THREAD_DEDICATED,
    //    DQTYPE_THREAD_CURRENT
    //};
    internal enum DISPATCHERQUEUE_THREAD_TYPE
    {
        DQTYPE_THREAD_DEDICATED = 1,
        DQTYPE_THREAD_CURRENT = 2,
    };

    //struct DispatcherQueueOptions
    //{
    //    DWORD dwSize;
    //    DISPATCHERQUEUE_THREAD_TYPE threadType;
    //    DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
    //};
    [StructLayout(LayoutKind.Sequential)]
    internal struct DispatcherQueueOptions
    {
        public int dwSize;

        [MarshalAs(UnmanagedType.I4)]
        public DISPATCHERQUEUE_THREAD_TYPE threadType;

        [MarshalAs(UnmanagedType.I4)]
        public DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
    };

    //HRESULT CreateDispatcherQueueController(
    //  DispatcherQueueOptions options,
    //  ABI::Windows::System::IDispatcherQueueController** dispatcherQueueController
    //);
    [DllImport("coremessaging.dll", EntryPoint = "CreateDispatcherQueueController", CharSet = CharSet.Unicode)]
    internal static extern IntPtr CreateDispatcherQueueController(DispatcherQueueOptions options,
                                            [MarshalAs(UnmanagedType.IUnknown)]
                                            out object dispatcherQueueController);

    #endregion PInvoke declarations
    ```

    现在已准备就绪的调度程序队列，并可以开始初始化和创建内容的组合。

1. 初始化[复合器](/uwp/api/windows.ui.composition.compositor)。 复合器是一个用于创建各种类型中的工厂[Windows.UI.Composition](/uwp/api/windows.ui.composition)横跨可视化层、 效果系统和动画系统的命名空间。 排序器类还管理从工厂创建对象的生存期。

    ```csharp
    private void InitComposition(IntPtr hwndHost)
    {
        ICompositorDesktopInterop interop;

        compositor = new Compositor();
        object iunknown = compositor as object;
        interop = (ICompositorDesktopInterop)iunknown;
        IntPtr raw;
        interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

        object rawObject = Marshal.GetObjectForIUnknown(raw);
        compositionTarget = (ICompositionTarget)rawObject;

        if (raw == null) { throw new Exception("QI Failed"); }

        containerVisual = compositor.CreateContainerVisual();
        Child = containerVisual;
    }
    ```

    - **ICompositorDesktopInterop**并**ICompositionTarget**需要 COM 导入。 将此代码后的放_CompositionHost_类，但内部命名空间声明。

    ```csharp
    #region COM Interop

    /*
    #undef INTERFACE
    #define INTERFACE ICompositorDesktopInterop
        DECLARE_INTERFACE_IID_(ICompositorDesktopInterop, IUnknown, "29E691FA-4567-4DCA-B319-D0F207EB6807")
        {
            IFACEMETHOD(CreateDesktopWindowTarget)(
                _In_ HWND hwndTarget,
                _In_ BOOL isTopmost,
                _COM_Outptr_ IDesktopWindowTarget * *result
                ) PURE;
        };
    */
    [ComImport]
    [Guid("29E691FA-4567-4DCA-B319-D0F207EB6807")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface ICompositorDesktopInterop
    {
        void CreateDesktopWindowTarget(IntPtr hwndTarget, bool isTopmost, out IntPtr test);
    }

    //[contract(Windows.Foundation.UniversalApiContract, 2.0)]
    //[exclusiveto(Windows.UI.Composition.CompositionTarget)]
    //[uuid(A1BEA8BA - D726 - 4663 - 8129 - 6B5E7927FFA6)]
    //interface ICompositionTarget : IInspectable
    //{
    //    [propget] HRESULT Root([out] [retval] Windows.UI.Composition.Visual** value);
    //    [propput] HRESULT Root([in] Windows.UI.Composition.Visual* value);
    //}

    [ComImport]
    [Guid("A1BEA8BA-D726-4663-8129-6B5E7927FFA6")]
    [InterfaceType(ComInterfaceType.InterfaceIsIInspectable)]
    public interface ICompositionTarget
    {
        Windows.UI.Composition.Visual Root
        {
            get;
            set;
        }
    }

    #endregion COM Interop
    ```

## <a name="create-a-custom-control-to-host-composition-elements"></a>创建自定义控件与主机组合元素

它是将代码生成和管理你从 CompositionHost 派生而来的单独控件中的组合元素放在一个好办法。 这会使您 CompositionHost 在类中创建可重用的互操作代码。

在这里，您创建自定义控件派生自 CompositionHost。 此控件添加到 Visual Studio 工具箱中，以便可以将其添加到你的窗体。

1. 将新的自定义控制文件添加到你从 CompositionHost 派生而来的项目。
    - 在中**解决方案资源管理器**，右键单击_HelloComposition_项目。
    - 在上下文菜单中，选择**外** > **新项...** .
    - 在中**添加新项**对话框中，选择**自定义控件**。
    - 将控件_CompositionHostControl.cs_，然后单击**添加**。 在设计视图中打开 CompositionHostControl.cs。

1. 在 CompositionHostControl.cs 设计视图中的属性窗格中，将**BackColor**属性设置为**ControlLight**。

    设置背景色是可选的。 我们在以下位置执行以便您可以看到自定义控件与窗体背景。

1. 切换到 CompositionHostControl.cs 的代码视图，并更新从 CompositionHost 派生的类声明。

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. 更新构造函数来调用基构造函数。

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>添加组合元素

与基础结构，现在可以向应用程序 UI 中添加组合内容。

对于此示例中，您可以将代码添加到创建并进行动画处理一个简单的 CompositionHostControl 类[SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)。

1. 添加组合元素。

    在 CompositionHostControl.cs，将这些方法添加到 CompositionHostControl 类。

    ```csharp
    // Add
    // using Windows.UI.Composition;

    public void AddElement(float size, float offsetX, float offsetY)
    {
        var visual = compositor.CreateSpriteVisual();
        visual.Size = new Vector2(size, size); // Requires references
        visual.Brush = compositor.CreateColorBrush(GetRandomColor());
        visual.Offset = new Vector3(offsetX, offsetY, 0);
        containerVisual.Children.InsertAtTop(visual);

        AnimateSquare(visual, 3);
    }

    private void AnimateSquare(SpriteVisual visual, int delay)
    {
        float offsetX = (float)(visual.Offset.X);
        Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
        float bottom = Height - visual.Size.Y;
        animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
        animation.Duration = TimeSpan.FromSeconds(2);
        animation.DelayTime = TimeSpan.FromSeconds(delay);
        visual.StartAnimation("Offset", animation);
    }

    private Windows.UI.Color GetRandomColor()
    {
        Random random = new Random();
        byte r = (byte)random.Next(0, 255);
        byte g = (byte)random.Next(0, 255);
        byte b = (byte)random.Next(0, 255);
        return Windows.UI.Color.FromArgb(255, r, g, b);
    }
    ```

## <a name="add-the-control-to-your-form"></a>将控件添加到你的窗体

具有自定义控件来承载组合内容，可以将其添加到应用程序 UI 中。 在这里，您添加上一步中创建的 CompositionHostControl 的实例。 CompositionHostControl 自动添加到 Visual Studio 工具箱下 **_项目名称_组件**。

1. 在 Form1.CS 设计视图中，添加一个按钮的 ui。

    - 将一个按钮从工具箱拖到 Form1 上。 将其放在窗体的左上角。 （请参阅在本教程，若要检查的控件位置的启动映像。）
    - 在属性窗格中更改**文本**属性从_button1_到_添加组合元素_。
    - 调整按钮大小，以便所有文本都显示。

    (有关详细信息，请参阅[如何：将控件添加到 Windows 窗体](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms)。)

1. 向 UI 添加 CompositionHostControl。

    - 从工具箱拖到 Form1 上拖动 CompositionHostControl。 将它放到按钮的右侧。
    - 调整 CompositionHost 大小，以便它填充窗体的其余部分。

1. 句柄按钮单击事件。

   - 在属性窗格中，单击闪电形，若要切换到事件视图。
   - 在事件列表中，选择 **单击** 事件中，键入 *Button_Click* ，然后按 Enter。
   - 在 Form1.cs 中添加此代码：

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. 将代码添加到按钮单击处理程序来创建新元素。

    - 在 Form1.cs 中，将代码添加到*Button_Click*之前创建的事件处理程序。 此代码将调用_CompositionHostControl1.AddElement_使用随机生成的大小和偏移量创建新的元素。 (自动命名的实例 CompositionHostControl _compositionHostControl1_时您将其拖动到窗体。)

    ```csharp
    // Add
    // using System;

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        Random random = new Random();
        float size = random.Next(50, 150);
        float offsetX = random.Next(0, (int)(compositionHostControl1.Width - size));
        float offsetY = random.Next(0, (int)(compositionHostControl1.Height/2 - size));
        compositionHostControl1.AddElement(size, offsetX, offsetY);
    }
    ```

你现在可以生成并运行您的 Windows 窗体应用程序。 当单击按钮时，应看到添加到 UI 中的动画的方块。

## <a name="next-steps"></a>后续步骤

在同一基础结构为基础的更完整示例，请参阅[Windows 窗体 Visual 层集成示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)GitHub 上。

## <a name="additional-resources"></a>其他资源

- [Windows 窗体入门](/dotnet/framework/winforms/getting-started-with-windows-forms)(.NET)
- [与进行互操作非托管代码](/dotnet/framework/interop/)(.NET)
- [开始使用 Windows 10 应用](/windows/uwp/get-started/)(UWP)
- [增强适用于 Windows 10 桌面应用程序](/windows/uwp/porting/desktop-to-uwp-enhance)(UWP)
- [Windows.UI.Composition 命名空间](/uwp/api/windows.ui.composition)(UWP)

## <a name="complete-code"></a>完成代码

下面是本教程的完整代码。

### <a name="form1cs"></a>Form1.cs

```csharp
using System;
using System.Windows.Forms;

namespace HelloComposition
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, EventArgs e)
        {
            Random random = new Random();
            float size = random.Next(50, 150);
            float offsetX = random.Next(0, (int)(compositionHostControl1.Width - size));
            float offsetY = random.Next(0, (int)(compositionHostControl1.Height/2 - size));
            compositionHostControl1.AddElement(size, offsetX, offsetY);
        }
    }
}
```

### <a name="compositionhostcontrolcs"></a>CompositionHostControl.cs

```csharp
using System;
using System.Numerics;
using Windows.UI.Composition;

namespace HelloComposition
{
    class CompositionHostControl : CompositionHost
    {
        public CompositionHostControl() : base()
        {

        }

        public void AddElement(float size, float offsetX, float offsetY)
        {
            var visual = compositor.CreateSpriteVisual();
            visual.Size = new Vector2(size, size); // Requires references
            visual.Brush = compositor.CreateColorBrush(GetRandomColor());
            visual.Offset = new Vector3(offsetX, offsetY, 0);
            containerVisual.Children.InsertAtTop(visual);

            AnimateSquare(visual, 3);
        }

        private void AnimateSquare(SpriteVisual visual, int delay)
        {
            float offsetX = (float)(visual.Offset.X);
            Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
            float bottom = Height - visual.Size.Y;
            animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
            animation.Duration = TimeSpan.FromSeconds(2);
            animation.DelayTime = TimeSpan.FromSeconds(delay);
            visual.StartAnimation("Offset", animation);
        }

        private Windows.UI.Color GetRandomColor()
        {
            Random random = new Random();
            byte r = (byte)random.Next(0, 255);
            byte g = (byte)random.Next(0, 255);
            byte b = (byte)random.Next(0, 255);
            return Windows.UI.Color.FromArgb(255, r, g, b);
        }
    }
}
```

### <a name="compositionhostcs"></a>CompositionHost.cs

```csharp
using System;
using System.Runtime.InteropServices;
using System.Windows.Forms;
using Windows.UI.Composition;

namespace HelloComposition
{
    public partial class CompositionHost : Control
    {
        IntPtr hwndHost;
        object dispatcherQueue;
        protected ContainerVisual containerVisual;
        protected Compositor compositor;
        private ICompositionTarget compositionTarget;

        public Visual Child
        {
            set
            {
                if (compositor == null)
                {
                    InitComposition(hwndHost);
                }
                compositionTarget.Root = value;
            }
        }

        public CompositionHost()
        {
            // Get the window handle.
            hwndHost = Handle;

            // Create dispatcher queue.
            dispatcherQueue = InitializeCoreDispatcher();

            // Build Composition tree of content.
            InitComposition(hwndHost);
        }

        private object InitializeCoreDispatcher()
        {
            DispatcherQueueOptions options = new DispatcherQueueOptions();
            options.apartmentType = DISPATCHERQUEUE_THREAD_APARTMENTTYPE.DQTAT_COM_STA;
            options.threadType = DISPATCHERQUEUE_THREAD_TYPE.DQTYPE_THREAD_CURRENT;
            options.dwSize = Marshal.SizeOf(typeof(DispatcherQueueOptions));

            object queue = null;
            CreateDispatcherQueueController(options, out queue);
            return queue;
        }

        private void InitComposition(IntPtr hwndHost)
        {
            ICompositorDesktopInterop interop;

            compositor = new Compositor();
            object iunknown = compositor as object;
            interop = (ICompositorDesktopInterop)iunknown;
            IntPtr raw;
            interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

            object rawObject = Marshal.GetObjectForIUnknown(raw);
            compositionTarget = (ICompositionTarget)rawObject;

            if (raw == null) { throw new Exception("QI Failed"); }

            containerVisual = compositor.CreateContainerVisual();
            Child = containerVisual;
        }

        protected override void OnPaint(PaintEventArgs pe)
        {
            base.OnPaint(pe);
        }

        #region PInvoke declarations

        //typedef enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
        //{
        //    DQTAT_COM_NONE,
        //    DQTAT_COM_ASTA,
        //    DQTAT_COM_STA
        //};
        internal enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
        {
            DQTAT_COM_NONE = 0,
            DQTAT_COM_ASTA = 1,
            DQTAT_COM_STA = 2
        };

        //typedef enum DISPATCHERQUEUE_THREAD_TYPE
        //{
        //    DQTYPE_THREAD_DEDICATED,
        //    DQTYPE_THREAD_CURRENT
        //};
        internal enum DISPATCHERQUEUE_THREAD_TYPE
        {
            DQTYPE_THREAD_DEDICATED = 1,
            DQTYPE_THREAD_CURRENT = 2,
        };

        //struct DispatcherQueueOptions
        //{
        //    DWORD dwSize;
        //    DISPATCHERQUEUE_THREAD_TYPE threadType;
        //    DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
        //};
        [StructLayout(LayoutKind.Sequential)]
        internal struct DispatcherQueueOptions
        {
            public int dwSize;

            [MarshalAs(UnmanagedType.I4)]
            public DISPATCHERQUEUE_THREAD_TYPE threadType;

            [MarshalAs(UnmanagedType.I4)]
            public DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
        };

        //HRESULT CreateDispatcherQueueController(
        //  DispatcherQueueOptions options,
        //  ABI::Windows::System::IDispatcherQueueController** dispatcherQueueController
        //);
        [DllImport("coremessaging.dll", EntryPoint = "CreateDispatcherQueueController", CharSet = CharSet.Unicode)]
        internal static extern IntPtr CreateDispatcherQueueController(DispatcherQueueOptions options,
                                                 [MarshalAs(UnmanagedType.IUnknown)]
                                        out object dispatcherQueueController);

        #endregion PInvoke declarations
    }

    #region COM Interop

    /*
    #undef INTERFACE
    #define INTERFACE ICompositorDesktopInterop
        DECLARE_INTERFACE_IID_(ICompositorDesktopInterop, IUnknown, "29E691FA-4567-4DCA-B319-D0F207EB6807")
        {
            IFACEMETHOD(CreateDesktopWindowTarget)(
                _In_ HWND hwndTarget,
                _In_ BOOL isTopmost,
                _COM_Outptr_ IDesktopWindowTarget * *result
                ) PURE;
        };
    */
    [ComImport]
    [Guid("29E691FA-4567-4DCA-B319-D0F207EB6807")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface ICompositorDesktopInterop
    {
        void CreateDesktopWindowTarget(IntPtr hwndTarget, bool isTopmost, out IntPtr test);
    }

    //[contract(Windows.Foundation.UniversalApiContract, 2.0)]
    //[exclusiveto(Windows.UI.Composition.CompositionTarget)]
    //[uuid(A1BEA8BA - D726 - 4663 - 8129 - 6B5E7927FFA6)]
    //interface ICompositionTarget : IInspectable
    //{
    //    [propget] HRESULT Root([out] [retval] Windows.UI.Composition.Visual** value);
    //    [propput] HRESULT Root([in] Windows.UI.Composition.Visual* value);
    //}

    [ComImport]
    [Guid("A1BEA8BA-D726-4663-8129-6B5E7927FFA6")]
    [InterfaceType(ComInterfaceType.InterfaceIsIInspectable)]
    public interface ICompositionTarget
    {
        Windows.UI.Composition.Visual Root
        {
            get;
            set;
        }
    }
    #endregion COM Interop
}
```