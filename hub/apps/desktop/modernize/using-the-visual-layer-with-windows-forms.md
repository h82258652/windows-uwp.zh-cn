---
title: 将视觉层与 Windows 窗体结合使用
description: 了解将视觉层 API 与现有 Windows 窗体内容结合使用以创建高级动画和效果的技术。
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 9da9dee48beef6e3c1cd38ffbe9761ed89fd940d
ms.sourcegitcommit: 93d0b2996b4742b33cd6d641e036f42672cf5238
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2019
ms.locfileid: "69999642"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>将视觉层与 Windows 窗体结合使用

可以在 Windows 窗体应用中使用 Windows 运行时 Composition API（也称为[视觉层](/windows/uwp/composition/visual-layer)）来创建面向 Windows 10 用户的现代特色体验。

GitHub 上提供了本教程的完整代码：[Windows 窗体 HelloComposition 示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)。

## <a name="prerequisites"></a>必备条件

UWP 宿主 API 的先决条件如下。

- 假设你对使用 Windows 窗体和 UWP 进行应用开发有一定的了解。 有关详细信息，请参阅：
  - [Windows 窗体入门](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [Windows 10 应用入门](/windows/uwp/get-started/)
  - [增强适用于 Windows 10 的桌面应用程序](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 或更高版本
- Windows 10 版本 1803 或更高版本
- Windows 10 SDK 17134 或更高版本

## <a name="how-to-use-composition-apis-in-windows-forms"></a>如何在 Windows 窗体中使用 Composition API

在本教程中，你将创建一个简单的 Windows 窗体 UI，并在其中添加动画合成元素。 Windows 窗体和合成组件都比较简单，但无论组件的复杂性如何，显示的互操作代码都是相同的。 完成的应用如下所示。

![正在运行的应用 UI](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>创建 Windows 窗体项目

第一步是创建 Windows 窗体应用项目，包括应用程序定义以及 UI 的主窗体。

在 Visual C# 中创建名为 HelloComposition 的新 Windows 窗体应用程序项目： 

1. 打开 Visual Studio 并选择“文件” > “新建” > “项目”。   <br/>此时会打开“新建项目”对话框。 
1. 在“安装”类别下展开“Visual C#”节点，然后选择“Windows 桌面”。   
1. 选择“Windows 窗体应用(.NET Framework)”模板。 
1. 输入名称 HelloComposition，选择框架“.NET Framework 4.7.2”，然后单击“确定”。   

Visual Studio 将创建该项目，并打开名为 Form1.cs 的默认应用程序窗口的设计器。

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>将项目配置为使用 Windows 运行时 API

若要在 Windows 窗体应用中使用 Windows 运行时 (WinRT) API，需将 Visual Studio 项目配置为访问 Windows 运行时。 此外，Composition API 广泛使用向量，因此你需要添加所需的引用来使用向量。

NuGet 包可用于满足这两项需求。 请安装这些包的最新版本，以添加对项目的必要引用。  

- [Microsoft.Windows.SDK.Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts)（需将默认包管理格式设置为 PackageReference。）
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> 尽管我们建议使用 NuGet 包来配置项目，但你可以手动添加所需的引用。 有关详细信息，请参阅[增强适用于 Windows 10 的桌面应用程序](/windows/uwp/porting/desktop-to-uwp-enhance)。 下表显示了需要对其添加引用的文件。

|文件|位置|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<sdk version>\Windows.Foundation.UniversalApiContract\<version>  |
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<sdk version>\Windows.Foundation.FoundationContract\<version>  |
|System.Numerics.Vectors.dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.Numerics.dll|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="create-a-custom-control-to-manage-interop"></a>创建自定义控件用于管理互操作

若要使用视觉层承载创建的内容，请创建派生自 [Control](/dotnet/api/system.windows.forms.control) 的自定义控件。 此控件可让你访问在为视觉层内容创建容器时所需的窗口 [Handle](/dotnet/api/system.windows.forms.control.handle)。

将在此类中完成用于承载 Composition API 的大部分配置。 在此控件中，使用[平台调用服务 (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code) 和 [COM 互操作性](/dotnet/api/system.runtime.interopservices.comimportattribute)将 Composition API 引入 Windows 窗体应用中。 有关 PInvoke 和 COM 互操作性的详细信息，请参阅[互操作性与非托管代码](/dotnet/framework/interop/index)。

> [!TIP]
> 如果需要，请检查本教程末尾的完整代码，确保在完成本教程的过程中，所有代码位于正确的位置。

1. 将派生自 [Control](/dotnet/api/system.windows.forms.control) 的新自定义控件文件添加到项目。
    - 在“解决方案资源管理器”中右键单击“HelloComposition”项目。  
    - 在上下文菜单中，选择“添加” > “新建项...”。  
    - 在“添加新项”对话框中，选择“自定义控件”。  
    - 将控件命名为 CompositionHost.cs，然后单击“添加”。   CompositionHost.cs 将在设计视图中打开。

1. 切换到 CompositionHost.cs 的代码视图，并在类中添加以下代码。

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

    在构造函数中调用 InitializeCoreDispatcher 和 InitComposition 方法。   将在后续步骤中创建这些方法。

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
    ```

1. 使用 [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher) 初始化某个线程。 核心调度程序负责处理窗口消息并调度 WinRT API 的事件。 必须在具有 CoreDispatcher 的线程中创建 Compositor 的新实例。 
    - 创建名为 InitializeCoreDispatcher 的方法，并添加用于设置调度程序队列的代码。 

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

    - 调度程序队列需要 PInvoke 声明。 请将此声明置于类代码的末尾。 （我们将此代码放在某个区域，使类代码保持整洁。）

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

    现已准备好调度程序队列，接下来可以开始初始化并创建合成内容。

1. 初始化 [Compositor](/uwp/api/windows.ui.composition.compositor)。 Compositor 是一个工厂，它在跨视觉层、效果系统和动画系统的 [Windows.UI.Composition](/uwp/api/windows.ui.composition) 命名空间中创建各种类型。 Compositor 类还用于管理从工厂创建的对象的生存期。

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

    - ICompositorDesktopInterop 和 ICompositionTarget 需要 COM 导入。   请将此代码置于 CompositionHost 类的后面，但请确保它位于命名空间声明的内部。 

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

## <a name="create-a-custom-control-to-host-composition-elements"></a>创建自定义控件用于承载合成元素

最好是将生成和管理合成元素的代码放在派生自 CompositionHost 的单独控件中。 这使得在 CompositionHost 类中创建的互操作代码可重用。

此处，你将创建一个派生自 CompositionHost 的自定义控件。 此控件会添加到 Visual Studio 工具箱，因此可将其添加到窗体。

1. 将派生自 CompositionHost 的新自定义控件文件添加到项目。
    - 在“解决方案资源管理器”中右键单击“HelloComposition”项目。  
    - 在上下文菜单中，选择“添加” > “新建项...”。  
    - 在“添加新项”对话框中，选择“自定义控件”。  
    - 将控件命名为 CompositionHostControl.cs，然后单击“添加”。   CompositionHostControl.cs 将在设计视图中打开。

1. 在 CompositionHostControl.cs 设计视图的“属性”窗格中，将 BackColor 属性设置为 ControlLight。  

    设置背景色是可选操作。 此处我们将执行此操作，使你能够在窗体背景的衬托下查看自定义控件。

1. 切换到 CompositionHostControl.cs 的代码视图，并将类声明更新为派生自 CompositionHost。

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. 更新构造函数以调用基构造函数。

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>添加合成元素

准备好基础结构后，接下来可将合成内容添加到应用 UI。

对于本示例，请将用于创建并以动画显示一个简单 [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) 的代码添加到 CompositionHostControl 类。

1. 添加合成元素。

    在 CompositionHostControl.cs 中，将这些方法添加到 CompositionHostControl 类。

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

## <a name="add-the-control-to-your-form"></a>将控件添加到窗体

添加用于承载合成内容的自定义控件后，接下来可将该控件添加到应用 UI。 此处，你将添加在上一步骤中创建的 CompositionHostControl 的实例。 CompositionHostControl 会自动添加到“<项目名称> 组件”下的 Visual Studio 工具箱中。 

1. 在 Form1.CS 设计视图中，将一个按钮添加到 UI。

    - 将工具箱中的一个按钮拖放到 Form1。 将该按钮置于窗体的左上角。 （请查看本教程开头的插图来检查控件位置。）
    - 在“属性”窗格中，将“文本”属性从“button1”更改为“添加合成元素”。   
    - 调整按钮大小以显示所有文本。

    （有关详细信息，请参阅[如何：将控件添加到 Windows 窗体](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms)。）

1. 将 CompositionHostControl 添加到 UI。

    - 将工具箱中的 CompositionHostControl 拖放到 Form1。 将其置于按钮右侧。
    - 调整 CompositionHost 的大小，使其填充窗体的剩余空间。

1. 处理按钮单击事件。

   - 在“属性”窗格中，单击闪电图标切换到“事件”视图。
   - 在事件列表中选择“Click”事件，键入 Button_Click，然后按 Enter。  
   - 此代码将添加到 Form1.cs 中：

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. 将代码添加到按钮单击处理程序以创建新元素。

    - 在 Form1.cs 中，将代码添加到前面创建的 Button_Click 事件处理程序。  此代码将调用 CompositionHostControl1.AddElement 来创建使用随机生成的大小和偏移量的新元素。  （将 CompositionHostControl 的实例拖放到窗体中时，该实例会自动命名为 compositionHostControl1。） 

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

现在，可以生成并运行你的 Windows 窗体应用。 单击按钮时，应会看到已添加到 UI 中的动画正方形。

## <a name="next-steps"></a>后续步骤

有关在同一基础结构上构建的更完整示例，请参阅 GitHub 上的 [Windows 窗体视觉层集成示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)。

## <a name="additional-resources"></a>其他资源

- [Windows 窗体入门](/dotnet/framework/winforms/getting-started-with-windows-forms) (.NET)
- [与非托管代码的互操作](/dotnet/framework/interop/) (.NET)
- [Windows 10 应用入门](/windows/uwp/get-started/) (UWP)
- [增强适用于 Windows 10 的桌面应用程序](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition 命名空间](/uwp/api/windows.ui.composition) (UWP)

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