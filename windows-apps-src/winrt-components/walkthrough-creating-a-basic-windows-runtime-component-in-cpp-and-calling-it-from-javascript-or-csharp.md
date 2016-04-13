---
使用 C++ 创建一个基本 Windows 运行时组件并通过 JavaScript 或 C 调用此组件#
本演练演示了如何创建一个可通过 JavaScript、C# 或 Visual Basic 调用的基本 Windows 运行时组件 DLL。
ms.assetid: 764CD9C6-3565-4DFF-88D7-D92185C7E452
---

# 演练：使用 C++ 创建一个基本 Windows 运行时组件并从 JavaScript 或 C 中调用此组件#


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


\[有些信息与可能在商业发行之前就经过实质性修改的预发布产品相关。 Microsoft 不对此处提供的信息作任何明示或默示的担保。\]

本演练演示了如何创建一个可从 JavaScript、C# 或 Visual Basic 调用的基本 Windows 运行时组件 DLL。 在开始本演练之前，请确保你已了解抽象二进制接口 (ABI)、ref 类以及简化使用 ref 类的 Visual C++ 组件扩展等概念。 有关详细信息，请参阅[使用 C++ 创建 Windows 运行时组件](creating-windows-runtime-components-in-cpp.md)和 [Visual C++ 语言参考 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx)。

## 创建 C++ 组件 DLL


在本例中，我们首先创建组件项目，但你可以首先创建 JavaScript 项目。 顺序无关紧要。

请注意，组件的主类包含属性和方法定义以及事件声明的示例。 只是为了向你演示如何实现该目的才提供它们。 它们不是必需的，并且在本例中，我们将使用自己的代码替换所有生成的代码。

## **创建 C++ 组件项目**

1.  在 Visual Studio 菜单栏上，依次选择“文件”、“新建”、“项目”****。
2.  在**“新建项目”**对话框的左侧窗格中，展开**“Visual C++”**，然后选择通用 Windows 应用的节点。
3.  在中心窗格中，选择**“Windows 运行时组件”**，然后将该项目命名为 WinRT\_CPP。
4.  选择**“确定”**按钮。

## **将可激活类添加到组件**

1.  可激活类是客户端代码可以使用 **new** 表达式（Visual Basic 中为 **New**，C++ 中为 **ref new**）创建的类。 在你的组件中，将其声明为 **public ref class sealed**。 其实，Class1.h 和 .cpp 文件中已具有一个 ref 类。 你可以更改名称，但在本例中我们将使用默认名称 Class1。 你可以根据需要在组件中定义额外的 ref 类或常规类。 有关 ref 类的详细信息，请参阅[类型系统 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx)。

2.  将以下 \#include 指令添加到 Class1.h：

    ```cpp
             private void PrimesUnOrderedButton_Click_1(object sender, RoutedEventArgs e)
            {
                var nativeObject = new WinRT_CPP.Class1();

                StringBuilder sb = new StringBuilder();
                sb.Append("Primes found (unordered): ");
                PrimesUnOrderedResult.Text = sb.ToString();

                // primeFoundEvent is a user-defined event in nativeObject
                // It passes the results back to this thread as they are produced
                // and the event handler that we define here immediately displays them.
                nativeObject.primeFoundEvent += (n) =>
                {
                    sb.Append(n.ToString()).Append(" ");
                    PrimesUnOrderedResult.Text = sb.ToString();
                };

                // Call the async method.
                var asyncResult = nativeObject.GetPrimesUnordered(2, 100000);

                // Provide a handler for the Progress event that the asyncResult
                // object fires at regular intervals. This handler updates the progress bar.
                asyncResult.Progress += (asyncInfo, progress) =>
                    {
                        PrimesUnOrderedProgress.Value = progress;
                    };
            }

            private void Clear_Button_Click(object sender, RoutedEventArgs e)
            {
                PrimesOrderedProgress.Value = 0;
                PrimesUnOrderedProgress.Value = 0;
                PrimesUnOrderedResult.Text = "";
                PrimesOrderedResult.Text = "";
                Result1.Text = "";
            }
    ```

## 运行该应用


选择 C# 项目或 JavaScript 项目作为启动项目，方法是在“解决方案资源管理器”中打开项目节点的快捷菜单，然后选择“设置为启动项目”****。 然后按 F5 运行并调试，或按 Ctrl+F5 运行但不调试。

## 在“对象浏览器”中检查组件（可选）


在“对象浏览器”中，可以检查在 .winmd 文件中定义的所有 Windows 运行时类型。 这包括 Platform 命名空间和默认命名空间中的类型。 但是，由于 Platform::Collections 命名空间在头文件 collections.h 而非 winmd 文件中定义，因此它们不会显示在“对象浏览器”中。

## **检查组件**

1.  在菜单栏上，依次选择**“视图”、“对象浏览器”** (Ctrl+Alt+J)。
2.  在“对象浏览器”的左侧窗格中，展开 WinRT\_CPP 节点以显示在你的组件上定义的类型和方法。

## 调试提示


为实现更好的调试体验，请从公共 Microsoft 符号服务器下载调试符号：

## **下载调试符号**

1.  在菜单栏上，依次选择“工具”>“选项”****。
2.  在“选项”****对话框中，展开“调试”****并选择“符号”****。
3.  选择“Microsoft 符号服务器”****，然后选择“确定”****按钮。

首次下载符号需要花费一些时间。 为实现更快的性能，在下次按 F5 时指定缓存符号的本地目录。

在调试具有组件 DLL 的 JavaScript 解决方案时，你可以将调试器设置为在组件中支持单步调试脚本或单步调试本机代码，但无法设置为同时进行。 若要更改设置，请在“解决方案资源管理器”中打开 JavaScript 项目节点的快捷菜单，然后依次选择“属性”、“调试”、“调试器类型”****。

请确保在程序包设计器中选择相应的功能。 可以通过打开 Package.appxmanifest 文件来打开程序包设计器。 例如，如果你尝试以编程方式访问 Pictures 文件夹中的文件，请确保在程序包设计器的“功能”****窗格中选中“图片库”****复选框。

如果 JavaScript 代码不识别组件中的公共属性或方法，请确保在 JavaScript 中使用的是 Camel 大小写格式。 例如，`ComputeResult` C++ 方法必须在 JavaScript 中引用为 `computeResult`。

如果你从某个解决方案中删除 C++ Windows 运行时组件项目，还必须从 JavaScript 项目中手动删除项目引用。 如果此操作无法完成，将阻止后续调试或生成操作。 如有必要，你可以稍后向 DLL 添加程序集引用。

## 相关主题

* [使用 C++ 创建 Windows 运行时组件](creating-windows-runtime-components-in-cpp.md)



<!--HONumber=Mar16_HO1-->


