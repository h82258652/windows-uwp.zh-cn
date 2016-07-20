---
author: msatranjr
title: "使用 C++ 创建一个基本 Windows 运行时组件并通过 JavaScript 或 C 调用此组件#"
description: "本演练演示了如何创建一个可通过 JavaScript、C# 或 Visual Basic 调用的基本 Windows 运行时组件 DLL。"
ms.assetid: 764CD9C6-3565-4DFF-88D7-D92185C7E452
translationtype: Human Translation
ms.sourcegitcommit: 4c32b134c704fa0e4534bc4ba8d045e671c89442
ms.openlocfilehash: 860333e3239198cd54eea061195e2a51d786821b

---

<h1>演练：使用 C++ 创建一个 Windows 运行时组件并从 JavaScript 或 C 中调用此组件#</h1>


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本演练演示了如何创建一个可从 JavaScript、C# 或 Visual Basic 调用的基本 Windows 运行时组件 DLL。 在开始本演练之前，请确保你已了解抽象二进制接口 (ABI)、ref 类以及简化使用 ref 类的 Visual C++ 组件扩展等概念。 有关详细信息，请参阅[使用 C++ 创建 Windows 运行时组件](creating-windows-runtime-components-in-cpp.md)和 [Visual C++ 语言参考 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx)。

## 创建 C++ 组件 DLL

在本例中，我们首先创建组件项目，但你可以首先创建 JavaScript 项目。 顺序无关紧要。

请注意，组件的主类包含属性和方法定义以及事件声明的示例。 只是为了向你演示如何实现该目的才提供它们。 它们不是必需的，并且在本例中，我们将使用自己的代码替换所有生成的代码。

## **创建 C++ 组件项目**

在 Visual Studio 菜单栏上，依次选择“文件”、“新建”、“项目”****。

在**“新建项目”**对话框的左侧窗格中，展开**“Visual C++”**，然后选择通用 Windows 应用的节点。

在中心窗格中，选择**“Windows 运行时组件”**，然后将该项目命名为 WinRT\_CPP。

选择**“确定”**按钮。

## **将可激活类添加到组件**

可激活类是客户端代码可以使用 **new** 表达式（Visual Basic 中为 **New**，C++ 中为 **ref new**）创建的类。 在你的组件中，将其声明为 **public ref class sealed**。 其实，Class1.h 和 .cpp 文件中已具有一个 ref 类。 你可以更改名称，但在本例中我们将使用默认名称 Class1。 你可以根据需要在组件中定义额外的 ref 类或常规类。 有关 ref 类的详细信息，请参阅[类型系统 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx)。

将以下 \#include 指令添加到 Class1.h：

```cpp
#include <collection.h>
#include <ppl.h>
#include <amp.h>
#include <amp_math.h>
```

collection.h 是 C++ 具体类（例如 Platform::Collections::Vector 类和 Platform::Collections::Map 类）的头文件，可实现由 Windows 运行时定义的中性语言接口。 amp 头文件用于在 GPU 上运行计算。 它们没有 Windows 运行时等效项，但没有关系，因为它们是私有的。 通常出于性能原因，应在组件内部使用 ISO C++ 代码和标准库；这只是必须采用 Windows 运行时类型表示的 Windows 运行时接口。

## 在命名空间作用域添加委托

委托是一种定义参数和方法返回类型的构造。 事件是特定委托类型的实例，任何订阅事件的事件处理程序方法都必须具有委托中指定的签名。 下面的代码定义采用 int 并返回 void 的委托类型。 接下来，代码声明此类型的公共事件；这使客户端代码能够提供在触发事件时调用的方法。

在 Class1.h 的命名空间作用域添加以下委托声明，就在 Class1 声明之前。

```cpp
public delegate void PrimeFoundHandler(int result);
```

将代码粘贴到 Visual Studio 时如果代码未正确对齐，只需按 Ctrl+K+D 即可修复整个文件的缩进。

## 添加公共成员

该类公布了三个公共方法和一个公共事件。 第一个方法是同步的，因为它的执行始终非常快速。 由于其他两个方法可能会花费一些时间，因此它们是异步方法，这样便不会阻止 UI 线程。 这些方法将返回 IAsyncOperationWithProgress 和 IAsyncActionWithProgress。 前者定义返回结果的异步方法，后者定义返回 void 的异步方法。 这些接口还可以使客户端代码接收操作的进度更新。

```cpp
public:

        // Synchronous method.
        Windows::Foundation::Collections::IVector<double>^  ComputeResult(double input);

        // Asynchronous methods
        Windows::Foundation::IAsyncOperationWithProgress<Windows::Foundation::Collections::IVector<int>^, double>^
            GetPrimesOrdered(int first, int last);
        Windows::Foundation::IAsyncActionWithProgress<double>^ GetPrimesUnordered(int first, int last);

        // Event whose type is a delegate "class"
        event PrimeFoundHandler^ primeFoundEvent;

```
## 添加私有成员

该类包含三个私有成员：两个用于数值计算的帮助程序方法，以及用于将来自 worker 线程的事件调用封送回 UI 线程的 CoreDispatcher 对象。

```cpp
private:
        bool is_prime(int n);
        Windows::UI::Core::CoreDispatcher^ m_dispatcher;
```

## 添加头文件和命名空间指令

在 Class1.cpp 中，添加以下 #include 指令：

```cpp
#include <ppltasks.h>
#include <concurrent_vector.h>
```

现在，使用语句添加它们以拉入所需命名空间：

```cpp
using namespace concurrency;
using namespace Platform::Collections;
using namespace Windows::Foundation::Collections;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
```

## 添加 ComputeResult 的实现

在 Class1.cpp 中，添加以下方法实现。 此方法在调用线程上同步执行，但它的速度非常快，因为它使用 C++ AMP 在 GPU 上并行处理计算。 有关详细信息，请参阅 C++ AMP 概述。 结果将附加到 Platform::Collections::Vector<T> 具体类型，它将在返回时隐式转换为 Windows::Foundation::Collections::IVector<T>。

```cpp
//Public API
IVector<double>^ Class1::ComputeResult(double input)
{
    // Implement your function in ISO C++ or
    // call into your C++ lib or DLL here. This example uses AMP.
    float numbers[] = { 1.0, 10.0, 60.0, 100.0, 600.0, 10000.0 };
    array_view<float, 1> logs(6, numbers);

    // See http://msdn.microsoft.com/en-us/library/hh305254.aspx
    parallel_for_each(
        logs.extent,
        [=] (index<1> idx) restrict(amp)
    {
        logs[idx] = concurrency::fast_math::log10(logs[idx]);
    }
    );

    // Return a Windows Runtime-compatible type across the ABI
    auto res = ref new Vector<double>();
    int len = safe_cast<int>(logs.extent.size());
    for(int i = 0; i < len; i++)
    {      
        res->Append(logs[i]);
    }

    // res is implicitly cast to IVector<double>
    return res;
}
```
## 添加 GetPrimesOrdered 的实现及其帮助程序方法

在 Class1.cpp 中，添加 GetPrimesOrdered 的实现和 is_prime helper 方法。 GetPrimesOrdered 使用 concurrent_vector 类和 parallel_for 函数循环来划分工作，并使用运行该程序的计算机的最大资源来生成结果。 在结果完成计算、存储和排序后，它们将添加到 Platform::Collections::Vector<T> 并作为 Windows::Foundation::Collections::IVector<T> 返回到客户端代码。

注意进度报告器的代码，这使客户端能够连接进度栏或其他 UI 以向用户显示操作将花费多长时间。 进度报告将产生成本。 事件必须在组件端触发并在 UI 线程上处理，而进度值必须存储在每个迭代上。 最大程度减少成本的一个方法是限制触发进度事件的频率。 如果成本仍然过高，或者如果无法估计操作的时长，请考虑使用进度环，它可显示操作正在进行但不显示完成前的剩余时间。

```cpp
// Determines whether the input value is prime.
bool Class1::is_prime(int n)
{
    if (n < 2)
        return false;
    for (int i = 2; i < n; ++i)
    {
        if ((n % i) == 0)
            return false;
    }
    return true;
}

// This method computes all primes, orders them, then returns the ordered results.
IAsyncOperationWithProgress<IVector<int>^, double>^ Class1::GetPrimesOrdered(int first, int last)
{
    return create_async([this, first, last]
    (progress_reporter<double> reporter) -> IVector<int>^ {
        // Ensure that the input values are in range.
        if (first < 0 || last < 0) {
            throw ref new InvalidArgumentException();
        }
        // Perform the computation in parallel.
        concurrent_vector<int> primes;
        long operation = 0;
        long range = last - first + 1;
        double lastPercent = 0.0;

        parallel_for(first, last + 1, [this, &primes, &operation,
            range, &lastPercent, reporter](int n) {

                // Increment and store the number of times the parallel
                // loop has been called on all threads combined. There
                // is a performance cost to maintaining a count, and
                // passing the delegate back to the UI thread, but it's
                // necessary if we want to display a determinate progress
                // bar that goes from 0 to 100%. We can avoid the cost by
                // setting the ProgressBar IsDeterminate property to false
                // or by using a ProgressRing.
                if(InterlockedIncrement(&operation) % 100 == 0)
                {
                    reporter.report(100.0 * operation / range);
                }

                // If the value is prime, add it to the local vector.
                if (is_prime(n)) {
                    primes.push_back(n);
                }
        });

        // Sort the results.
        std::sort(begin(primes), end(primes), std::less<int>());        
        reporter.report(100.0);

        // Copy the results to a Vector object, which is
        // implicitly converted to the IVector return type. IVector
        // makes collections of data available to other
        // Windows Runtime components.
        return ref new Vector<int>(primes.begin(), primes.end());
    });
}
```

## 添加 GetPrimesUnordered 的实现

创建 C++ 组件的最后一步是在 Class1.cpp 中添加 GetPrimesUnordered 的实现。 此方法只要找到一个结果便将其返回，而不是直到找到所有结果才返回。 每个结果都在事件处理程序中返回，并实时显示在 UI 中。 同样，请注意已使用进度报告器。 此方法还使用了 is_prime 帮助程序方法。

```cpp
// This method returns no value. Instead, it fires an event each time a
// prime is found, and passes the prime through the event.
// It also passes progress info.
IAsyncActionWithProgress<double>^ Class1::GetPrimesUnordered(int first, int last)
{

    auto window = Windows::UI::Core::CoreWindow::GetForCurrentThread();
    m_dispatcher = window->Dispatcher;


    return create_async([this, first, last](progress_reporter<double> reporter) {

        // Ensure that the input values are in range.
        if (first < 0 || last < 0) {
            throw ref new InvalidArgumentException();
        }

        // In this particular example, we don't actually use this to store
        // results since we pass results one at a time directly back to
        // UI as they are found. However, we have to provide this variable
        // as a parameter to parallel_for.
        concurrent_vector<int> primes;
        long operation = 0;
        long range = last - first + 1;
        double lastPercent = 0.0;

        // Perform the computation in parallel.
        parallel_for(first, last + 1,
            [this, &primes, &operation, range, &lastPercent, reporter](int n)
        {
            // Store the number of times the parallel loop has been called  
            // on all threads combined. See comment in previous method.
            if(InterlockedIncrement(&operation) % 100 == 0)
            {
                reporter.report(100.0 * operation / range);
            }

            // If the value is prime, pass it immediately to the UI thread.
            if (is_prime(n))
            {                
                // Since this code is probably running on a worker
                // thread, and we are passing the data back to the
                // UI thread, we have to use a CoreDispatcher object.
                m_dispatcher->RunAsync( CoreDispatcherPriority::Normal,
                    ref new DispatchedHandler([this, n, operation, range]()
                {
                    this->primeFoundEvent(n);

                }, Platform::CallbackContext::Any));

            }
        });
        reporter.report(100.0);
    });
}
```

## 创建 JavaScript 客户端应用

如果你只想要创建 C# 客户端，你可以跳过此部分。

## 创建 JavaScript 项目

在“解决方案资源管理器”中，打开“解决方案”节点的快捷菜单，然后依次选择“添加”、“新建项目”****。

展开 JavaScript（它可能嵌套在”其他语言“****下方）然后选择“空白应用(通用 Windows)”****。

通过选择“确定”****按钮接受默认名称 App1。

打开 App1 项目节点的快捷菜单，然后选择“设置为启动项目”****。

添加对 WinRT_CPP 的项目引用：

打开“引用”节点的快捷菜单，然后选择“添加引用”****。

在“引用管理器”对话框的左侧窗格中，依次选择“项目”****和“解决方案”****。

在中心窗格中，选择 WinRT_CPP，然后选择“确定”****按钮

## 添加调用 JavaScript 事件处理程序的 HTML

将此 HTML 粘贴到 default.html 页面的 <body> 节点：

```HTML
<div id="LogButtonDiv">
     <button id="logButton">Logarithms using AMP</button>
 </div>
 <div id="LogResultDiv">
     <p id="logResult"></p>
 </div>
 <div id="OrderedPrimeButtonDiv">
     <button id="orderedPrimeButton">Primes using parallel_for with sort</button>
 </div>
 <div id="OrderedPrimeProgress">
     <progress id="OrderedPrimesProgressBar" value="0" max="100"></progress>
 </div>
 <div id="OrderedPrimeResultDiv">
     <p id="orderedPrimes">
         Primes found (ordered):
     </p>
 </div>
 <div id="UnorderedPrimeButtonDiv">
     <button id="ButtonUnordered">Primes returned as they are produced.</button>
 </div>
 <div id="UnorderedPrimeDiv">
     <progress id="UnorderedPrimesProgressBar" value="0" max="100"></progress>
 </div>
 <div id="UnorderedPrime">
     <p id="unorderedPrimes">
         Primes found (unordered):
     </p>
 </div>
 <div id="ClearDiv">
     <button id="Button_Clear">Clear</button>
 </div>
```

## 添加样式

在 default.css 中，删除正文样式，然后添加以下样式：

```css
#LogButtonDiv {
border: orange solid 1px;
-ms-grid-row: 1; /* default is 1 */
-ms-grid-column: 1; /* default is 1 */
}
#LogResultDiv {
background: black;
border: red solid 1px;
-ms-grid-row: 1;
-ms-grid-column: 2;
}
#UnorderedPrimeButtonDiv, #OrderedPrimeButtonDiv {
border: orange solid 1px;
-ms-grid-row: 2;   
-ms-grid-column:1;
}
#UnorderedPrimeProgress, #OrderedPrimeProgress {
border: red solid 1px;
-ms-grid-column-span: 2;
height: 40px;
}
#UnorderedPrimeResult, #OrderedPrimeResult {
border: red solid 1px;
font-size:smaller;
-ms-grid-row: 2;
-ms-grid-column: 3;
-ms-overflow-style:scrollbar;
}
```

## 添加调用组件 DLL 的 JavaScript 事件处理程序

在 default.js 文件的末尾添加以下功能。 当选择主页面上的按钮时调用这些功能。 请注意 JavaScript 如何激活 C++ 类，然后调用其方法并使用返回值填充 HTML 标签。

```JavaScript
var nativeObject = new WinRT_CPP.Class1();

function LogButton_Click() {

    var val = nativeObject.computeResult(0);
    var result = "";

    for (i = 0; i < val.length; i++) {
        result += val[i] + "<br/>";
    }

    document.getElementById('logResult').innerHTML = result;
}

function ButtonOrdered_Click() {
    document.getElementById('orderedPrimes').innerHTML = "Primes found (ordered): ";

    nativeObject.getPrimesOrdered(2, 10000).then(
        function (v) {
            for (var i = 0; i < v.length; i++)
                document.getElementById('orderedPrimes').innerHTML += v[i] + " ";
        },
        function (error) {
            document.getElementById('orderedPrimes').innerHTML += " " + error.description;
        },
        function (p) {
            var progressBar = document.getElementById("OrderedPrimesProgressBar");
            progressBar.value = p;
        });
}

function ButtonUnordered_Click() {
    document.getElementById('unorderedPrimes').innerHTML = "Primes found (unordered): ";
    nativeObject.onprimefoundevent = handler_unordered;

    nativeObject.getPrimesUnordered(2, 10000).then(
        function () { },
        function (error) {
            document.getElementById("unorderedPrimes").innerHTML += " " + error.description;
        },
        function (p) {
            var progressBar = document.getElementById("UnorderedPrimesProgressBar");
            progressBar.value = p;
        });
}

var handler_unordered = function (n) {
    document.getElementById('unorderedPrimes').innerHTML += n.target.toString() + " ";
};

function ButtonClear_Click() {

    document.getElementById('logResult').innerHTML = "";
    document.getElementById("unorderedPrimes").innerHTML = "";
    document.getElementById('orderedPrimes').innerHTML = "";
    document.getElementById("UnorderedPrimesProgressBar").value = 0;
    document.getElementById("OrderedPrimesProgressBar").value = 0;
}
```

通过添加代码来添加事件侦听器，方法是使用以下在 then 代码块中实现事件注册的代码，在 default.js 的 app.onactivated 中替换对 WinJS.UI.processAll 的现有调用。 有关此内容的详细说明，请参阅“创建‘Hello World’应用 (JS)”。

```JavaScript
args.setPromise(WinJS.UI.processAll().then( function completed() {
    var logButton = document.getElementById("logButton");
    logButton.addEventListener("click", LogButton_Click, false);
    var orderedPrimeButton = document.getElementById("orderedPrimeButton");
    orderedPrimeButton.addEventListener("click", ButtonOrdered_Click, false);
    var buttonUnordered = document.getElementById("ButtonUnordered");
    buttonUnordered.addEventListener("click", ButtonUnordered_Click, false);
    var buttonClear = document.getElementById("Button_Clear");
    buttonClear.addEventListener("click", ButtonClear_Click, false);
}));
```

按 F5 运行应用。

## 创建 C# 客户端应用

## 创建 C# 项目

在“解决方案资源管理器”中，打开“解决方案”节点的快捷菜单，然后依次选择“添加”、“新建项目”****。

展开 Visual C#（它可能嵌套在“其他语言”****下），在左侧窗格中依次选择“Windows”****和“通用”****，然后在中间窗格中选择“空白应用”****。

将此应用命名为 CS_Client，然后选择“确定”****按钮。

打开 CS_Client 项目节点的快捷菜单，然后选择“设置为启动项目”****。

添加对 WinRT_CPP 的项目引用：

打开“引用”****节点的快捷菜单，然后选择“添加引用”****。

在“引用管理器”****对话框的左侧窗格中，依次选择“项目”****和“解决方案”****。

在中心窗格中，选择 WinRT_CPP，然后选择“确定”****按钮。

## 添加定义用户界面的 XAML

将以下代码复制到 MainPage.xaml 中的 Grid 元素。

```xaml
<ScrollViewer>
            <StackPanel Width="1400">

                <Button x:Name="Button1" Width="340" Height="50"  Margin="0,20,20,20" Content="Synchronous Logarithm Calculation" FontSize="16" Click="Button1_Click_1"/>
                <TextBlock x:Name="Result1" Height="100" FontSize="14"></TextBlock>
            <Button x:Name="PrimesOrderedButton" Content="Prime Numbers Ordered" FontSize="16" Width="340" Height="50" Margin="0,20,20,20" Click="PrimesOrderedButton_Click_1"></Button>
            <ProgressBar x:Name="PrimesOrderedProgress" IsIndeterminate="false" Height="40"></ProgressBar>
                <TextBlock x:Name="PrimesOrderedResult" MinHeight="100" FontSize="10" TextWrapping="Wrap"></TextBlock>
            <Button x:Name="PrimesUnOrderedButton" Width="340" Height="50" Margin="0,20,20,20" Click="PrimesUnOrderedButton_Click_1" Content="Prime Numbers Unordered" FontSize="16"></Button>
            <ProgressBar x:Name="PrimesUnOrderedProgress" IsIndeterminate="false" Height="40" ></ProgressBar>
            <TextBlock x:Name="PrimesUnOrderedResult" MinHeight="100" FontSize="10" TextWrapping="Wrap"></TextBlock>

            <Button x:Name="Clear_Button" Content="Clear" HorizontalAlignment="Left" Margin="0,20,20,20" VerticalAlignment="Top" Width="341" Click="Clear_Button_Click" FontSize="16"/>
        </StackPanel>
</ScrollViewer>
```

## 为按钮添加事件处理程序

在“解决方案资源管理器”中，打开“MainPage.xaml.cs”。 （文件可能嵌套在 MainPage.xaml 下。）为 System.Text 添加 using 指令，然后在 MainPage 类中为对数计算添加事件处理程序。

```csharp
private void Button1_Click_1(object sender, RoutedEventArgs e)
{
    // Create the object
    var nativeObject = new WinRT_CPP.Class1();

    // Call the synchronous method. val is an IList that
    // contains the results.
    var val = nativeObject.ComputeResult(0);
    StringBuilder result = new StringBuilder();
    foreach (var v in val)
    {
        result.Append(v).Append(System.Environment.NewLine);
    }
    this.Result1.Text = result.ToString();
}
```

为经过排序的结果添加事件处理程序：

```csharp
async private void PrimesOrderedButton_Click_1(object sender, RoutedEventArgs e)
{
    var nativeObject = new WinRT_CPP.Class1();

    StringBuilder sb = new StringBuilder();
    sb.Append("Primes found (ordered): ");

    PrimesOrderedResult.Text = sb.ToString();

    // Call the asynchronous method
    var asyncOp = nativeObject.GetPrimesOrdered(2, 100000);

    // Before awaiting, provide a lambda or named method
    // to handle the Progress event that is fired at regular
    // intervals by the asyncOp object. This handler updates
    // the progress bar in the UI.
    asyncOp.Progress = (asyncInfo, progress) =>
        {
            PrimesOrderedProgress.Value = progress;
        };

    // Wait for the operation to complete
    var asyncResult = await asyncOp;

    // Convert the results to strings
    foreach (var result in asyncResult)
    {
        sb.Append(result).Append(" ");
    }

    // Display the results
    PrimesOrderedResult.Text = sb.ToString();
}
```

为经过排序的结果以及清除结果的按钮添加事件处理程序，以便可以再次运行代码。

```csharp
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

在菜单栏上，依次选择**“视图”、“对象浏览器”** (Ctrl+Alt+J)。

在“对象浏览器”的左侧窗格中，展开 WinRT\_CPP 节点以显示在你的组件上定义的类型和方法。

## 调试提示

为实现更好的调试体验，请从公共 Microsoft 符号服务器下载调试符号：

## **下载调试符号**

在菜单栏上，依次选择“工具”&gt;“选项”****。

在“选项”****对话框中，展开“调试”****并选择“符号”****。

选择“Microsoft 符号服务器”****，然后选择“确定”****按钮。

首次下载符号需要花费一些时间。 为实现更快的性能，在下次按 F5 时指定缓存符号的本地目录。

在调试具有组件 DLL 的 JavaScript 解决方案时，你可以将调试器设置为在组件中支持单步调试脚本或单步调试本机代码，但无法设置为同时进行。 若要更改设置，请在“解决方案资源管理器”中打开 JavaScript 项目节点的快捷菜单，然后依次选择“属性”、“调试”、“调试器类型”****。

请确保在程序包设计器中选择相应的功能。 可以通过打开 Package.appxmanifest 文件来打开程序包设计器。 例如，如果你尝试以编程方式访问 Pictures 文件夹中的文件，请确保在程序包设计器的“功能”****窗格中选中“图片库”****复选框。

如果 JavaScript 代码不识别组件中的公共属性或方法，请确保在 JavaScript 中使用的是 Camel 大小写格式。 例如，`ComputeResult` C++ 方法必须在 JavaScript 中引用为 `computeResult`。

如果你从某个解决方案中删除 C++ Windows 运行时组件项目，还必须从 JavaScript 项目中手动删除项目引用。 如果此操作无法完成，将阻止后续调试或生成操作。 如有必要，你可以稍后向 DLL 添加程序集引用。

## 相关主题

* [使用 C++ 创建 Windows 运行时组件](creating-windows-runtime-components-in-cpp.md)



<!--HONumber=Jun16_HO5-->


