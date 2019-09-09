---
title: 使用带有 Windows 窗体的视觉层
description: 了解结合使用视觉对象层 Api 和现有 Windows 窗体内容来创建高级动画和效果的方法。
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 9da9dee48beef6e3c1cd38ffbe9761ed89fd940d
ms.sourcegitcommit: 93d0b2996b4742b33cd6d641e036f42672cf5238
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2019
ms.locfileid: "69999642"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>使用带有 Windows 窗体的视觉层

你可以在你的 Windows 窗体应用中使用 Windows 运行时组合 Api （也称为[可视层](/windows/uwp/composition/visual-layer)）来创建适用于 Windows 10 用户的新式体验。

GitHub 上提供了本教程的完整代码：[Windows 窗体 HelloComposition 示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)。

## <a name="prerequisites"></a>先决条件

UWP 宿主 API 具有这些先决条件。

- 假设你已熟悉如何使用 Windows 窗体和 UWP 来开发应用程序。 有关详细信息，请参阅：
  - [入门与 Windows 窗体](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [Windows 10 应用入门](/windows/uwp/get-started/)
  - [增强适用于 Windows 10 的桌面应用程序](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 或更高版本
- Windows 10 版本1803或更高版本
- Windows 10 SDK 17134 或更高版本

## <a name="how-to-use-composition-apis-in-windows-forms"></a>如何在 Windows 窗体中使用组合 Api

在本教程中，将创建一个简单的 Windows 窗体 UI，并向其中添加动画组合元素。 Windows 窗体和组合组件都保持简单，但无论组件的复杂性如何，显示的互操作代码都是相同的。 完成的应用如下所示。

![正在运行的应用程序 UI](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>创建 Windows 窗体项目

第一步是创建 Windows 窗体应用项目，其中包括应用程序定义和 UI 的主窗体。

若要在 Visual C#命名_HelloComposition_中创建新的 Windows 窗体应用程序项目：

1. 打开 Visual Studio 并选择 "**文件** > " "**新建** > **项目**"。<br/>此时将打开 "**新建项目**" 对话框。
1. 在 "**已安装**" 类别下，展开 "**视觉对象C#**  " 节点，然后选择 " **Windows 桌面**"。
1. 选择 " **Windows 窗体应用（.NET Framework）** " 模板。
1. 输入名称 " _HelloComposition"，_ 选择 "框架 **.NET Framework" 4.7.2**"，然后单击 **" 确定 "** 。

Visual Studio 将创建项目并打开名为 Form1.cs 的默认应用程序窗口的设计器。

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>将项目配置为使用 Windows 运行时 Api

若要在 Windows 窗体应用中使用 Windows 运行时（WinRT） Api，需要配置 Visual Studio 项目以访问 Windows 运行时。 此外，组合 Api 广泛地使用向量，因此需要添加使用向量所需的引用。

NuGet 包可用于满足这两项需求。 安装这些包的最新版本，以便将必要的引用添加到你的项目。  

- PackageReference （需要将默认包管理格式设置为 " ["。）](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts)
- [System.object](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> 尽管我们建议使用 NuGet 包来配置你的项目，但你可以手动添加所需的引用。 有关详细信息，请参阅[增强适用于 Windows 10 的桌面应用程序](/windows/uwp/porting/desktop-to-uwp-enhance)。 下表显示了需要添加引用的文件。

|文件|Location|
|--|--|
|System.Runtime.WindowsRuntime|C：\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files （x86） \windows Kits\10\References\<*sdk 版本*> \Windows.Foundation.UniversalApiContract\<*版本*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files （x86） \windows Kits\10\References\<*sdk 版本*> \Windows.Foundation.FoundationContract\<*版本*>|
|System.object。|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.object|C:\Program Files （x86） \Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7。2|

## <a name="create-a-custom-control-to-manage-interop"></a>创建自定义控件以管理互操作

若要承载使用可视层创建的内容，可创建从[控件](/dotnet/api/system.windows.forms.control)派生的自定义控件。 此控件可让你访问窗口[句柄](/dotnet/api/system.windows.forms.control.handle)，以便为你的可视层内容创建容器。

这是你在其中进行大多数配置以承载组合 Api 的位置。 在此控件中，使用[平台调用服务（PInvoke）](/cpp/dotnet/calling-native-functions-from-managed-code)和[COM 互操作](/dotnet/api/system.runtime.interopservices.comimportattribute)将组合 api 引入 Windows 窗体应用。 有关 PInvoke 和 COM 互操作的详细信息，请参阅[与非托管代码进行互](/dotnet/framework/interop/index)操作。

> [!TIP]
> 如果需要，请在本教程末尾查看完整的代码，以确保在完成本教程时，所有代码都在正确的位置。

1. 将新的自定义控件文件添加到从[控件](/dotnet/api/system.windows.forms.control)派生的项目。
    - 在**解决方案资源管理器**中，右键单击_HelloComposition_项目。
    - 在上下文菜单中，选择 "**添加** > **新项 ...** "。
    - 在 "**添加新项**" 对话框中，选择 "**自定义控件**"。
    - 将控件命名为 " _CompositionHost.cs_"，然后单击 "**添加**"。 在设计视图中打开 CompositionHost.cs。

1. 切换到 CompositionHost.cs 的代码视图，并向类中添加以下代码。

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

    在构造函数中，可以调用_InitializeCoreDispatcher_和_InitComposition_方法。 可在后续步骤中创建这些方法。

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

1. 使用[CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher)初始化线程。 核心调度程序负责处理 WinRT Api 的窗口消息和调度事件。 必须在具有 CoreDispatcher 的线程上创建**组合**器的新实例。
    - 创建一个名为_InitializeCoreDispatcher_的方法，并添加代码以设置调度程序队列。

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

    - 调度程序队列需要 PInvoke 声明。 将此声明放置在类的代码末尾。 （我们将此代码放在某个区域内，以使类代码整洁。）

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

    你现在已准备就绪，可以开始初始化和创建撰写内容。

1. 初始化[组合](/uwp/api/windows.ui.composition.compositor)器。 该组合器是一个工厂，它在[Windows](/uwp/api/windows.ui.composition)中创建各种类型，跨越可视层、效果系统和动画系统。 组合器类还管理从工厂创建的对象的生存期。

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

    - **ICompositorDesktopInterop**和**ICompositionTarget**需要 COM 导入。 将此代码放在_CompositionHost_类之后，但位于命名空间声明内。

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

## <a name="create-a-custom-control-to-host-composition-elements"></a>创建自定义控件以承载复合元素

最好将生成和管理复合元素的代码放在一个派生自 CompositionHost 的单独控件中。 这会使在 CompositionHost 类中创建的互操作代码可重复使用。

此处，将创建一个派生自 CompositionHost 的自定义控件。 此控件将添加到 Visual Studio "工具箱" 中，以便可以将其添加到窗体中。

1. 向派生自 CompositionHost 的项目添加一个新的自定义控件文件。
    - 在**解决方案资源管理器**中，右键单击_HelloComposition_项目。
    - 在上下文菜单中，选择 "**添加** > **新项 ...** "。
    - 在 "**添加新项**" 对话框中，选择 "**自定义控件**"。
    - 将控件命名为 " _CompositionHostControl.cs_"，然后单击 "**添加**"。 在设计视图中打开 CompositionHostControl.cs。

1. 在 CompositionHostControl.cs 设计视图的 "属性" 窗格中，将 "**背景颜色**" 属性设置为 " **ControlLight**"。

    设置背景色是可选的。 我们在此处执行此操作，以便可以查看针对窗体背景的自定义控件。

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

### <a name="add-composition-elements"></a>添加复合元素

准备好基础结构后，即可将撰写内容添加到应用 UI。

在此示例中，向 CompositionHostControl 类添加代码，该代码创建一个简单的[SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)并对其进行动画处理。

1. 添加复合元素。

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

## <a name="add-the-control-to-your-form"></a>向窗体添加控件

现在，你有了自定义控件来托管撰写内容，接下来可以将其添加到应用 UI。 在此处添加在上一步中创建的 CompositionHostControl 的实例。 CompositionHostControl 会自动添加到 **_项目名称_组件**下的 Visual Studio 工具箱。

1. 在 Form1.CS 的 "设计" 视图中，将一个按钮添加到 UI。

    - 将一个按钮从工具箱拖到 Form1 上。 将其放在窗体的左上角。 （请参阅教程开始处的图像以检查控件的位置。）
    - 在 "属性" 窗格中，将 " **Text** " 属性从 " _button1_ " 更改为 "_添加组合元素_"。
    - 调整按钮大小，使文本显示。

    （有关详细信息，请[参阅如何：向 Windows 窗体](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms)添加控件。）

1. 将 CompositionHostControl 添加到 UI。

    - 将 CompositionHostControl 从工具箱拖到 Form1 上。 将其置于按钮右侧。
    - 调整 CompositionHost 的大小，使其填满窗体的其余部分。

1. 处理按钮单击事件。

   - 在 "属性" 窗格中，单击闪电以切换到 "事件" 视图。
   - 在事件列表中，选择 **单击** 事件中，键入 *Button_Click* ，然后按 Enter。
   - 此代码在 Form1.cs 中添加：

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. 将代码添加到按钮单击处理程序以创建新元素。

    - 在 Form1.cs 中，将代码添加到之前创建的*Button_Click*事件处理程序。 此代码调用_CompositionHostControl1_来创建具有随机生成的大小和偏移量的新元素。 （将 CompositionHostControl 的实例拖到窗体上时，它会自动命名为_compositionHostControl1_ 。）

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

现在，可以生成并运行 Windows 窗体应用。 单击该按钮时，应会看到添加到 UI 的动态方形。

## <a name="next-steps"></a>后续步骤

有关在同一基础结构上构建的更完整的示例，请参阅 GitHub 上的[Windows 窗体可视层集成示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)。

## <a name="additional-resources"></a>其他资源

- [入门与 Windows 窗体](/dotnet/framework/winforms/getting-started-with-windows-forms)ADO.NET
- [与非托管代码互](/dotnet/framework/interop/)操作ADO.NET
- [Windows 10 应用入门](/windows/uwp/get-started/)UWP
- [增强适用于 Windows 10 的桌面应用程序](/windows/uwp/porting/desktop-to-uwp-enhance)UWP
- [WINDOWS UI 复合命名空间](/uwp/api/windows.ui.composition)UWP

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