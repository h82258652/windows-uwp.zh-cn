---
title: 使用 C++/CX 创建 Windows 运行时组件
description: 本主题展示了如何使用 C++/CX 创建 Windows 运行时组件 &mdash; 一种可以从使用任何 Windows 运行时语言构建的通用 Windows 应用调用的组件。
ms.assetid: F7E06AA2-DCEC-427E-BD5D-9CA2A0ED2612
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c18b2ab7af1b83ab54f98dc85d73ae621524f52f
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493192"
---
# <a name="windows-runtime-components-with-ccx"></a>使用 C++/CX 创建 Windows 运行时组件

> [!NOTE]
> 本主题旨在帮助你维护 C++/CX 应用程序。 不过，我们建议你使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 编写新应用程序。 C++/WinRT 是 Windows 运行时 (WinRT) API 的完全标准新式 C++17 语言投影，以基于标头文件的库的形式实现，旨在为你提供对新式 Windows API 的一流访问。 若要了解如何使用 c + +/WinRT 创建 Windows 运行时组件，请参阅[使用 c + +/WinRT 组件 Windows 运行时](/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt)。

本主题演示如何使用 c + +/CX 创建 Windows 运行时组件 &mdash; ，该组件可从使用任意 Windows 运行时语言（c #、Visual Basic、c + + 或 Javascript）生成的通用 Windows 应用程序调用。

用 c + + 构建 Windows 运行时组件有多种原因。
- 在复杂或计算密集型操作中获取 C++ 的性能优势。
- 重复使用已编写和测试的代码。

在生成包含 JavaScript 或 .NET 项目的解决方案时，Windows 运行时组件项目、JavaScript 项目文件和编译的 DLL 会合并到一个程序包中，可以在模拟器中本地调试或在受限设备上远程调试该程序包。 你还可以仅将组件项目作为扩展 SDK 进行分配。 有关详细信息，请参阅[创建软件开发工具包](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)。

通常，当你编写 c + +/CX 组件的代码时，请使用常规 c + + 库和内置类型，但在将数据传递到另一个 winmd 包的代码的抽象二进制接口（ABI）边界中除外。 在那里，使用 Windows 运行时类型和 c + +/CX 支持的用于创建和操作这些类型的特殊语法。 此外，在 c + +/CX 代码中，可以使用委托和事件等类型来实现可从你的组件引发并在 JavaScript、Visual Basic、c + + 或 c # 中进行处理的事件。 有关 c + +/CX 语法的详细信息，请参阅[Visual C++ 语言参考（c + +/cx）](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx)。

## <a name="casing-and-naming-rules"></a>大小写和命名规则

### <a name="javascript"></a>JavaScript
JavaScript 区分大小写。 因此，必须遵循以下大小写约定：

-   引用 C++ 命名空间和类时，采用在 C++ 端上使用的相同大小写。
-   调用方法时，使用 Camel 大小写格式，即使方法名在 C++ 端上是大写。 例如，C++ 方法 GetDate() 必须作为 getDate() 从 JavaScript 调用。
-   可激活类名和命名空间名称不能包含 UNICODE 字符。

### <a name="net"></a>.NET
.NET 语言遵循其正常大小写规则。

## <a name="instantiating-the-object"></a>实例化对象
仅 Windows 运行时类型可以跨 ABI 边界传递。 如果组件在公共方法中具有作为返回类型或参数的类型（例如 std::wstring），编译器将引发错误。 Visual C++ 组件扩展 (C++/CX) 内置类型包括诸如整型和双精度型的常用标量及其 typedef 等效项 int32、float64 等。 有关详细信息，请参阅[类型系统 (C++/CX)](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx)。

```cpp
// ref class definition in C++
public ref class SampleRefClass sealed
{
    // Class members...

    // #include <valarray>
public:
    double LogCalc(double input)
    {
        // Use C++ standard library as usual.
        return std::log(input);
    }

};
```

```javascript
//Instantiation in JavaScript (requires "Add reference > Project reference")
var nativeObject = new CppComponent.SampleRefClass();
```

```csharp
//Call a method and display result in a XAML TextBlock
var num = nativeObject.LogCalc(21.5);
ResultText.Text = num.ToString();
```

## <a name="ccx-built-in-types-library-types-and-windows-runtime-types"></a>C + +/CX 内置类型、库类型和 Windows 运行时类型
可激活类（也称为 ref 类）是可通过其他诸如 JavaScript、C# 或 Visual Basic 语言实例化的类。 若要能够通过其他语言使用，组件必须包含至少一项可激活类。

Windows 运行时组件可以包含多个公共的可激活类以及其他仅为组件内部所知的类。 将[WebHostHidden](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.webhosthiddenattribute)特性应用于不应向 JavaScript 显示的 c + +/cx 类型。

所有公共类必须驻留在与组件元数据文件具有相同名称的相同根命名空间中。 例如，名为 A.B.C.MyClass 的类只有在名为 A.winmd 或 A.B.winmd 或 A.B.C.winmd 的元数据文件中定义后才能实例化。 DLL 的名称不必与 .winmd 文件名匹配。

就像对任意类一样，客户端代码使用 **new**（在 Visual Basic 中是 **New**）关键字创建组件实例。

可激活类必须声明为 **public ref class sealed**。 **ref class** 关键字告知编译器将类创建为 Windows 运行时兼容类型，而 sealed 关键字指定该类无法继承。 Windows 运行时当前无法支持一般化的继承模型；有限的继承模型支持创建自定义 XAML 控件。 有关详细信息，请参阅 [Ref 类和结构 (C++/CX)](https://docs.microsoft.com/cpp/cppcx/ref-classes-and-structs-c-cx)。

对于 c + +/CX，所有数值基元都在默认命名空间中定义。 [平台](https://docs.microsoft.com/cpp/cppcx/platform-namespace-c-cx)命名空间包含特定于 Windows 运行时类型系统的 c + +/cx 类。 这些类包括 [Platform::String](https://docs.microsoft.com/cpp/cppcx/platform-string-class) 类和 [Platform::Object](https://docs.microsoft.com/cpp/cppcx/platform-object-class) 类。 诸如 [Platform::Collections::Map](https://docs.microsoft.com/cpp/cppcx/platform-collections-map-class) 类和 [Platform::Collections::Vector](https://docs.microsoft.com/cpp/cppcx/platform-collections-vector-class) 类的具体集合类型在 [Platform::Collections](https://docs.microsoft.com/cpp/cppcx/platform-collections-namespace) 命名空间中定义。 这些类型实现的公共接口在 [Windows::Foundation::Collections 命名空间 (C++/CX)](https://docs.microsoft.com/cpp/cppcx/windows-foundation-collections-namespace-c-cx) 中定义。 JavaScript、C# 和 Visual Basic 使用的正是这些接口类型。 有关详细信息，请参阅[类型系统 (C++/CX)](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx)。

## <a name="method-that-returns-a-value-of-built-in-type"></a>返回内置类型值的方法
```cpp
    // #include <valarray>
public:
    double LogCalc(double input)
    {
        // Use C++ standard library as usual.
        return std::log(input);
    }
```

```javascript
//Call a method
var nativeObject = new CppComponent.SampleRefClass;
var num = nativeObject.logCalc(21.5);
document.getElementById('P2').innerHTML = num;
```

## <a name="method-that-returns-a-custom-value-struct"></a>返回自定义值结构的方法
```cpp
namespace CppComponent
{
    // Custom struct
    public value struct PlayerData
    {
        Platform::String^ Name;
        int Number;
        double ScoringAverage;
    };

    public ref class Player sealed
    {
    private:
        PlayerData m_player;
    public:
        property PlayerData PlayerStats
        {
            PlayerData get(){ return m_player; }
            void set(PlayerData data) {m_player = data;}
        }
    };
}
```

若要跨 ABI 传递用户定义的值结构，请定义一个 JavaScript 对象，该对象具有与 c + +/CX 中定义的值结构相同的成员。 然后，可以将该对象作为参数传递给 c + +/CX 方法，以便将对象隐式转换为 c + +/CX 类型。

```javascript
// Get and set the value struct
function GetAndSetPlayerData() {
    // Create an object to pass to C++
    var myData =
        { name: "Bob Homer", number: 12, scoringAverage: .357 };
    var nativeObject = new CppComponent.Player();
    nativeObject.playerStats = myData;

    // Retrieve C++ value struct into new JavaScript object
    var myData2 = nativeObject.playerStats;
    document.getElementById('P3').innerHTML = myData.name + " , " + myData.number + " , " + myData.scoringAverage.toPrecision(3);
}
```

另一种方法是定义实现 IPropertySet 的类（未显示）。

在 .NET 语言中，只需创建一个在 c + +/CX 组件中定义的类型的变量。

```csharp
private void GetAndSetPlayerData()
{
    // Create a ref class
    var player = new CppComponent.Player();

    // Create a variable of a value struct
    // type that is defined in C++
    CppComponent.PlayerData myPlayer;
    myPlayer.Name = "Babe Ruth";
    myPlayer.Number = 12;
    myPlayer.ScoringAverage = .398;

    // Set the property
    player.PlayerStats = myPlayer;

    // Get the property and store it in a new variable
    CppComponent.PlayerData myPlayer2 = player.PlayerStats;
    ResultText.Text += myPlayer.Name + " , " + myPlayer.Number.ToString() +
        " , " + myPlayer.ScoringAverage.ToString();
}
```

## <a name="overloaded-methods"></a>重载方法
C + +/CX 公共 ref 类可以包含重载方法，但 JavaScript 的功能有限，可以区分重载方法。 例如，它可以区分以下签名之间的区别：

```cpp
public ref class NumberClass sealed
{
public:
    int GetNumber(int i);
    int GetNumber(int i, Platform::String^ str);
    double GetNumber(int i, MyData^ d);
};
```

但是它无法区分以下内容之间的区别：

```cpp
int GetNumber(int i);
double GetNumber(double d);
```

在不明确的情况下，通过在标头文件中将 [Windows::Foundation::Metadata::DefaultOverload](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.defaultoverloadattribute) 属性应用到方法签名，你可以确保 JavaScript 始终调用特定重载。

此 JavaScript 始终调用属性化重载：

```javascript
var nativeObject = new CppComponent.NumberClass();
var num = nativeObject.getNumber(9);
document.getElementById('P4').innerHTML = num;
```

## <a name="net"></a>.NET
.NET 语言识别 c + +/CX ref 类中的重载，就像在任何 .NET 类中一样。

## <a name="datetime"></a>DateTime
在 Windows 运行时中，[Windows::Foundation::DateTime](https://docs.microsoft.com/uwp/api/windows.foundation.datetime) 对象仅是一个 64 位有符号的整数，代表 1601 年 1 月 1 日前或后 100 纳秒间隔的数字。 Windows:Foundation::DateTime 对象上没有方法。 相反，每个语言以该语言的本机方式投影日期时间： JavaScript 中的 Date 对象和 .NET 中的 system.string 和 System.object 类型。

```cpp
public  ref class MyDateClass sealed
{
public:
    property Windows::Foundation::DateTime TimeStamp;
    void SetTime(Windows::Foundation::DateTime dt)
    {
        auto cal = ref new Windows::Globalization::Calendar();
        cal->SetDateTime(dt);
        TimeStamp = cal->GetDateTime(); // or TimeStamp = dt;
    }
};
```

将日期时间值从 c + +/CX 传递到 JavaScript 时，JavaScript 会接受它作为 Date 对象并默认显示为长格式日期字符串。

```javascript
function SetAndGetDate() {
    var nativeObject = new CppComponent.MyDateClass();

    var myDate = new Date(1956, 4, 21);
    nativeObject.setTime(myDate);

    var myDate2 = nativeObject.timeStamp;

    //prints long form date string
    document.getElementById('P5').innerHTML = myDate2;

}
```

当 .NET 语言将 system.exception 传递到 c + +/CX 组件时，该方法会将其接受为 Windows：： Foundation：:D ateTime。 当组件将 Windows：： Foundation：:D ateTime 传递到 .NET 方法时，框架方法会将其作为 DateTimeOffset 接受。

```csharp
private void DateTimeExample()
{
    // Pass a System.DateTime to a C++ method
    // that takes a Windows::Foundation::DateTime
    DateTime dt = DateTime.Now;
    var nativeObject = new CppComponent.MyDateClass();
    nativeObject.SetTime(dt);

    // Retrieve a Windows::Foundation::DateTime as a
    // System.DateTimeOffset
    DateTimeOffset myDate = nativeObject.TimeStamp;

    // Print the long-form date string
    ResultText.Text += myDate.ToString();
}
```

## <a name="collections-and-arrays"></a>集合和数组
集合始终作为 Windows 运行时类型（例如 Windows::Foundation::Collections::IVector^ 和 Windows::Foundation::Collections::IMap^）的句柄在 ABI 边界上传递。 例如，如果将句柄返回到 Platform::Collections::Map，它会隐式转换为 Windows::Foundation::Collections::IMap^。 集合接口在与提供具体实现的 c + +/CX 类不同的命名空间中定义。 JavaScript 和 .NET 语言使用接口。 有关详细信息，请参阅[集合 (C++/CX)](https://docs.microsoft.com/cpp/cppcx/collections-c-cx) 和[数组和 WriteOnlyArray (C++/CX)](https://docs.microsoft.com/cpp/cppcx/array-and-writeonlyarray-c-cx)。

## <a name="passing-ivector"></a>传递 IVector
```cpp
// Windows::Foundation::Collections::IVector across the ABI.
//#include <algorithm>
//#include <collection.h>
Windows::Foundation::Collections::IVector<int>^ SortVector(Windows::Foundation::Collections::IVector<int>^ vec)
{
    std::sort(begin(vec), end(vec));
    return vec;
}
```

```javascript
var nativeObject = new CppComponent.CollectionExample();
// Call the method to sort an integer array
var inVector = [14, 12, 45, 89, 23];
var outVector = nativeObject.sortVector(inVector);
var result = "Sorted vector to array:";
for (var i = 0; i < outVector.length; i++)
{
    outVector[i];
    result += outVector[i].toString() + ",";
}
document.getElementById('P6').innerHTML = result;
```

.NET 语言将 IVector&lt;T&gt; 视为 IList&lt;T&gt;。

```csharp
private void SortListItems()
{
    IList<int> myList = new List<int>();
    myList.Add(5);
    myList.Add(9);
    myList.Add(17);
    myList.Add(2);

    var nativeObject = new CppComponent.CollectionExample();
    IList<int> mySortedList = nativeObject.SortVector(myList);

    foreach (var item in mySortedList)
    {
        ResultText.Text += " " + item.ToString();
    }
}
```

## <a name="passing-imap"></a>传递 IMap
```cpp
// #include <map>
//#include <collection.h>
Windows::Foundation::Collections::IMap<int, Platform::String^> ^GetMap(void)
{    
    Windows::Foundation::Collections::IMap<int, Platform::String^> ^ret =
        ref new Platform::Collections::Map<int, Platform::String^>;
    ret->Insert(1, "One ");
    ret->Insert(2, "Two ");
    ret->Insert(3, "Three ");
    ret->Insert(4, "Four ");
    ret->Insert(5, "Five ");
    return ret;
}
```

```javascript
// Call the method to get the map
var outputMap = nativeObject.getMap();
var mStr = "Map result:" + outputMap.lookup(1) + outputMap.lookup(2)
    + outputMap.lookup(3) + outputMap.lookup(4) + outputMap.lookup(5);
document.getElementById('P7').innerHTML = mStr;
```

.NET 语言查看 IMap 和 IDictionary&lt;K, V&gt;。

```csharp
private void GetDictionary()
{
    var nativeObject = new CppComponent.CollectionExample();
    IDictionary<int, string> d = nativeObject.GetMap();
    ResultText.Text += d[2].ToString();
}
```

## <a name="properties"></a>属性
C + +/CX 组件扩展中的公共 ref 类通过使用 property 关键字将公共数据成员作为属性公开。 概念与 .NET 属性相同。 普通属性类似于数据成员，因为其功能是隐式的。 特殊属性具有显式获取和设置的访问器和作为值的“备份存储”的已命名私有变量。 在此示例中，私有成员变量 \_ propertyAValue 是 PropertyA 的后备存储。 属性可以在它的值更改时引发某个事件，并且客户端应用可注册为接收该事件。

```cpp
//Properties
public delegate void PropertyChangedHandler(Platform::Object^ sender, int arg);
public ref class PropertyExample  sealed
{
public:
    PropertyExample(){}

    // Event that is fired when PropertyA changes
    event PropertyChangedHandler^ PropertyChangedEvent;

    // Property that has custom setter/getter
    property int PropertyA
    {
        int get() { return m_propertyAValue; }
        void set(int propertyAValue)
        {
            if (propertyAValue != m_propertyAValue)
            {
                m_propertyAValue = propertyAValue;
                // Fire event. (See event example below.)
                PropertyChangedEvent(this, propertyAValue);
            }
        }
    }

    // Trivial get/set property that has a compiler-generated backing store.
    property Platform::String^ PropertyB;

private:
    // Backing store for propertyA.
    int m_propertyAValue;
};
```

```javascript
var nativeObject = new CppComponent.PropertyExample();
var propValue = nativeObject.propertyA;
document.getElementById('P8').innerHTML = propValue;

//Set the string property
nativeObject.propertyB = "What is the meaning of the universe?";
document.getElementById('P9').innerHTML += nativeObject.propertyB;
```

.NET 语言访问本机 c + +/CX 对象上的属性，就像它们在 .NET 对象上一样。

```csharp
private void GetAProperty()
{
    // Get the value of the integer property
    // Instantiate the C++ object
    var obj = new CppComponent.PropertyExample();

    // Get an integer property
    var propValue = obj.PropertyA;
    ResultText.Text += propValue.ToString();

    // Set a string property
    obj.PropertyB = " What is the meaning of the universe?";
    ResultText.Text += obj.PropertyB;

}
```

## <a name="delegates-and-events"></a>委托和事件
委派是表示函数对象的 Windows 运行时类型。 你可以将委派与事件、回调和异步方法调用结合起来，以指定稍后要执行的操作。 像函数对象一样，委派通过支持编译器验证返回类型和函数的参数类型来提供类型安全。 委派的声明类似于函数签名、实现类似于类定义，而调用类似于函数调用。

## <a name="adding-an-event-listener"></a>添加事件侦听器
你可以使用事件关键字声明指定的委派类型的公共成员。 客户端代码通过使用在特定语言中提供的标准机制订阅事件。

```cpp
public:
    event SomeHandler^ someEvent;
```

此示例使用与之前属性部分相同的 C++ 代码。

```javascript
function Button_Click() {
    var nativeObj = new CppComponent.PropertyExample();
    // Define an event handler method
    var singlecasthandler = function (ev) {
        document.getElementById('P10').innerHTML = "The button was clicked and the value is " + ev;
    };

    // Subscribe to the event
    nativeObj.onpropertychangedevent = singlecasthandler;

    // Set the value of the property and fire the event
    var propValue = 21;
    nativeObj.propertyA = 2 * propValue;

}
```

在 .NET 语言中，订阅 c + + 组件中的事件与订阅 .NET 类中的事件相同：

```csharp
//Subscribe to event and call method that causes it to be fired.
private void TestMethod()
{
    var objWithEvent = new CppComponent.PropertyExample();
    objWithEvent.PropertyChangedEvent += objWithEvent_PropertyChangedEvent;

    objWithEvent.PropertyA = 42;
}

//Event handler method
private void objWithEvent_PropertyChangedEvent(object __param0, int __param1)
{
    ResultText.Text = "the event was fired and the result is " +
         __param1.ToString();
}
```

## <a name="adding-multiple-event-listeners-for-one-event"></a>为一个事件添加多个事件侦听器
JavaScript 具有支持多个处理程序订阅单一事件的 addEventListener 方法。

```cpp
public delegate void SomeHandler(Platform::String^ str);

public ref class LangSample sealed
{
public:
    event SomeHandler^ someEvent;
    property Platform::String^ PropertyA;

    // Method that fires an event
    void FireEvent(Platform::String^ str)
    {
        someEvent(Platform::String::Concat(str, PropertyA->ToString()));
    }
    //...
};
```

```javascript
// Add two event handlers
var multicast1 = function (ev) {
    document.getElementById('P11').innerHTML = "Handler 1: " + ev.target;
};
var multicast2 = function (ev) {
    document.getElementById('P12').innerHTML = "Handler 2: " + ev.target;
};

var nativeObject = new CppComponent.LangSample();
//Subscribe to the same event
nativeObject.addEventListener("someevent", multicast1);
nativeObject.addEventListener("someevent", multicast2);

nativeObject.propertyA = "42";

// This method should fire an event
nativeObject.fireEvent("The answer is ");
```

在 C# 中，任意数量的事件处理程序可以使用 += 运算符订阅事件，如之前示例所示。

## <a name="enums"></a>枚举
C + +/CX 中的 Windows 运行时枚举是使用公共类枚举声明的;它类似于标准 c + + 中的范围枚举。

```cpp
public enum class Direction {North, South, East, West};

public ref class EnumExampleClass sealed
{
public:
    property Direction CurrentDirection
    {
        Direction  get(){return m_direction; }
    }

private:
    Direction m_direction;
};
```

枚举值作为整数在 c + +/CX 和 JavaScript 之间传递。 您可以选择声明一个 JavaScript 对象，该对象包含与 c + +/CX 枚举相同的命名值，并按如下所示使用。

```javascript
var Direction = { 0: "North", 1: "South", 2: "East", 3: "West" };
//. . .

var nativeObject = new CppComponent.EnumExampleClass();
var curDirection = nativeObject.currentDirection;
document.getElementById('P13').innerHTML =
Direction[curDirection];
```

C# 和 Visual Basic 均支持枚举语言。 这些语言会看到 c + + 公共枚举类，就像它们会看到 .NET 枚举一样。

## <a name="asynchronous-methods"></a>异步方法
若要使用其他 Windows 运行时对象公开的异步方法，请使用[任务类（并发运行时）](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class)。 有关详细信息，请参阅[任务并行度（并发运行时）](https://docs.microsoft.com/cpp/parallel/concrt/task-parallelism-concurrency-runtime)。

若要在 c + +/CX 中实现异步方法，请使用 ppltasks.h 中定义的[create \_ async](https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017)函数。 有关详细信息，请参阅[在适用于 UWP 应用的 c + +/cx 中创建异步操作](https://docs.microsoft.com/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps)。 有关示例，请参阅[创建 c + +/cx Windows 运行时组件并从 JavaScript 或 c # 中调用该组件的演练](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md)。 .NET 语言使用 c + +/CX 异步方法，就像它们是在 .NET 中定义的任何异步方法一样。

## <a name="exceptions"></a>异常
你可以引发任何由 Windows 运行时定义的异常类型。 你无法从任何 Windows 运行时异常类型中派生自定义类型。 但是，你可以引发 COMException 并提供可由捕获异常的代码访问的自定义 HRESULT。 无法在 COMException 中指定自定义消息。

## <a name="debugging-tips"></a>调试提示
在调试具有组件 DLL 的 JavaScript 解决方案时，你可以将调试器设置为在组件中支持单步调试脚本或单步调试本机代码，但无法设置为同时进行。 若要更改设置，请在解决方案资源管理器中选择 JavaScript 项目节点，然后选择“属性”、“调试”、“调试器类型”。

请确保在程序包设计器中选择相应的功能。 例如，如果你要尝试使用 Windows 运行时 API 打开用户的“图片”库中的图像文件，请确保在清单设计器中的“功能”窗格中选中“图片库”复选框。

如果 JavaScript 代码似乎无法识别组件中的公共属性或方法，请确保在 JavaScript 中使用的是 Camel 大小写格式。 例如，在 JavaScript 中，LogCalc c + +/CX 方法必须被引用为 logCalc。

如果从解决方案中删除 c + +/CX Windows 运行时组件项目，还必须从 JavaScript 项目中手动删除项目引用。 如果此操作无法完成，将阻止后续调试或生成操作。 如有必要，你可以稍后向 DLL 添加程序集引用。

## <a name="related-topics"></a>相关主题
* [创建 C++/CX Windows 运行时组件并通过 JavaScript 或 C# 调用此组件的演练](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md)
