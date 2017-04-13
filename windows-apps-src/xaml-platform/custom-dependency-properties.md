---
author: jwmsft
description: "介绍如何为使用 C++、C# 或 Visual Basic 的 Windows 运行时应用定义和实现自定义依赖属性。"
title: "自定义依赖属性"
ms.assetid: 5ADF7935-F2CF-4BB6-B1A5-F535C2ED8EF8
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: b26ee59be9c309326eeb93546d3702bc161513f3
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="custom-dependency-properties"></a>自定义依赖属性

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

我们在此处介绍了如何为使用 C++、C# 或 Visual Basic 的 Windows 运行时应用定义和实现你自己的依赖属性。 我们列出了应用开发人员和组件作者可能希望创建自定义依赖属性的原因。 我们描述了自定义依赖属性的实现步骤，以及一些可改善依赖属性的性能、实用性或通用性的最佳做法。

## <a name="prerequisites"></a>先决条件


我们假设你已阅读[依赖属性概述](dependency-properties-overview.md)，并且从现有依赖属性用户的角度理解依赖属性。 要理解本主题中的示例，你还应该理解 XAML，知道如何编写使用 C++、C# 或 Visual Basic 的基本 Windows 运行时应用。

## <a name="what-is-a-dependency-property"></a>什么是依赖属性？


若要为属性支持样式设置、数据绑定、动画和默认值，它应实现为依赖属性。 依赖属性值不会存储为类中的字段，它们由 XAML 框架进行存储，并且使用密钥进行引用，该密钥会在通过调用 [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 方法以使用 Windows 运行时属性系统注册该属性时检索。   依赖属性只能由从 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 派生的类型使用。 但 **DependencyObject** 位于类层次结构中很高的级别，所以大部分用于 UI 和演示支持的类都能支持依赖属性。 有关依赖属性以及本文档中用于描述它们的一些术语和约定的详细信息，请参阅[依赖属性概述](dependency-properties-overview.md)。

Windows 运行时中的依赖属性示例如下：[**Control.Background**](https://msdn.microsoft.com/library/windows/apps/br209395)、[**FrameworkElement.Width**](https://msdn.microsoft.com/library/windows/apps/br208751) 和 [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/br209702) 等等。

约定如下：一个类公开的每个依赖属性都有一个 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 类型的相应 **public static readonly** 属性，该属性在同一个类上公开并提供依赖属性的标识符。 标识符的名称遵循以下约定：已向名称末尾添加字符串“Property”的依赖属性的名称。 例如，**Control.Background** 属性对应的 **DependencyProperty** 标识符是 [**Control.BackgroundProperty**](https://msdn.microsoft.com/library/windows/apps/br209396)。 标识符在注册依赖属性时存储其相关信息，然后可用于其他涉及依赖属性的操作，例如调用 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361)。

##  <a name="property-wrappers"></a>属性包装器

依赖属性通常有一个包装器实现。 没有包装器，获取或设置属性的唯一方式就是使用依赖属性实用程序方法 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 和 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 并将标识符作为参数传递给它们。 从表面上看，这是一个明显很奇怪的属性用法。 但有了包装器，你的代码和任何其他引用依赖属性的代码都可使用一种直观的对象-属性语法，这对你所使用的语言而言显得很正常。

如果自行实现一个自定义依赖属性，并且希望它是公共的且易于调用，也可定义属性包装器。 属性包装器对向反射或静态分析流程报告有关依赖属性的基本信息也很有用。 具体来讲，包装器是放置特性（例如 [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011)）的地方。

## <a name="when-to-implement-a-property-as-a-dependency-property"></a>何时将属性实现为依赖属性

在类上实现公共读/写属性，只要你的类派生自 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)，就可以选择让属性以依赖属性的方式工作。 有时使用一个私有字段来支持属性的典型技术就足够了。 将自定义属性定义为依赖属性并非总是必要或合适的。 具体选择将取决于你希望属性支持的场景。

当你希望属性支持 Windows 运行时或 Windows 运行时应用的以下一个或多个功能时，你可以考虑将属性实现为依赖属性：

-   通过 [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) 设置属性
-   用作通过 [**{Binding}**](binding-markup-extension.md) 绑定数据的有效目标属性
-   通过 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490) 支持动画值
-   报告属性值何时被以下实体更改：
    -   属性系统本身执行的操作
    -   环境
    -   用户操作
    -   读取和写入样式

## <a name="checklist-for-defining-a-dependency-property"></a>定义依赖属性的检查列表

一个依赖属性的定义可视为一组概念。 这些概念不一定是顺序步骤，因为在实现的一行代码中可解决多个概念。 这个列表只是提供了简短概述。 我们将在本主题后面更详细地介绍每个概念，并且显示多种语言的示例代码。

-   向属性系统注册属性名称（调用 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)），指定所有者类型和属性值的类型。 
    -  [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 有一个必需的参数需要使用属性元数据。 为 Register 指定 **null**，或者如果你希望通过调用 [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357) 可还原属性已更改的行为或基于元数据的默认值，请指定 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.propertymetadata) 的实例。
-   将一个 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 标识符定义为所有者类型上的一个 **public static readonly** 属性成员。
-   按照你正在实现的语言中所用的属性访问器模型，定义一个包装器属性。 包装器属性名称应该与 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 中使用的 *name* 字符串匹配。 实现 **get** 和 **set** 访问器将包装器与它包装的依赖属性相连接，方法是调用 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 和 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 并将你自己的属性标识符作为一个参数传递。
-   （可选）将 [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011) 等特性放在包装器上。

**注意**  如果定义一个自定义附加属性，一般会省略包装器。 而是编写一种可供 XAML 处理器使用的不同访问器样式。 查看[自定义附加属性](custom-attached-properties.md)。 

## <a name="registering-the-property"></a>注册属性

若要使你的属性成为依赖属性，必须将该属性注册到由 Windows 运行时属性系统所维护的属性存储中。  若要注册属性，请调用 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 方法。

对于 Microsoft .NET 语言（C# 和 Microsoft Visual Basic），你可以在类的主体中调用 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)（在类中，但在任何成员定义外部）。 该标识符由 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 方法调用以返回值的形式提供。 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 通常调用为静态构造函数，或作为类中包括的 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 类型的 **public static readonly** 属性初始化的一部分。 此属性会公开你的依赖属性的标识符。 以下是 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 调用的一些示例。

> [!div class="tabbedCodeSnippets"]
```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(String),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null)
);
```
```vb
Public Shared ReadOnly LabelProperty As DependencyProperty = 
    DependencyProperty.Register("Label", 
      GetType(String), 
      GetType(ImageWithLabelControl), 
      New PropertyMetadata(Nothing))
```

**注意**  将依赖属性注册为标识符属性定义的一部分是典型的实现方式，但也可以在类静态构造函数中注册依赖属性。 如果需要多行代码来初始化依赖属性，此方法可能很有用。

对于 C++，你可以选择在标头和代码文件之间拆分实现的方式。 典型的拆分方式是在标头中将标识符本身声明为 **public static** 属性，它具有一个 **get** 实现但没有 **set**。 **get** 实现引用一个私有字段，该字段是一个未初始化的 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 实例。 你也可以声明包装器和包装器的 **get** 和 **set** 实现。 在此情况下，标头文件包含一些极小的实现。 如果包装器需要归属于 Windows 运行时，标头文件中的特性也需要。 将 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 调用放置在代码文件内仅在应用首次初始化时运行的 helper 函数中。 使用 **Register** 的返回值填充你在标头文件中声明的静态但未初始化的标识符，你最初已在实现文件的根作用域上将其设置为 **nullptr**。

```cpp
//.h file
//using namespace Windows::UI::Xaml::Controls;
//using namespace Windows::UI::Xaml::Interop;
//using namespace Windows::UI::Xaml;
//using namespace Platform;

public ref class ImageWithLabelControl sealed : public Control
{  
private:
    static DependencyProperty^ _LabelProperty;
...
public:
    static void RegisterDependencyProperties(); 
    static property DependencyProperty^ LabelProperty
    {
        DependencyProperty^ get() {return _LabelProperty;}
    }
...
};
```

```cpp
//.cpp file
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml.Interop;

DependencyProperty^ ImageWithLabelControl::_LabelProperty = nullptr;

// This function is called from the App constructor in App.xaml.cpp 
// to register the properties
void ImageWithLabelControl::RegisterDependencyProperties() 
{ 
    if (_LabelProperty == nullptr) 
    { 
        _LabelProperty = DependencyProperty::Register(
          "Label", Platform::String::typeid, ImageWithLabelControl::typeid, nullptr); 
    } 
}
```

**注意**  对于 C++ 代码，有一个私有字段和一个包装 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 的公共只读属性的原因是，这样可让使用你的依赖属性的其他调用方也可以使用需要使该标识符公有的属性系统实用程序 API。 如果保持标识符为私有，人们将无法使用这些实用程序 API。 此类 API 示例和场景包括 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 或 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361)、[**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357)、[**GetAnimationBaseValue**](https://msdn.microsoft.com/library/windows/apps/br242358)、[**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257) 和 [**Setter.Property**](https://msdn.microsoft.com/library/windows/apps/br208836) 等。 不可将公共字段用于这些内容，因为 Windows 运行时元数据规则不支持公共字段。

## <a name="dependency-property-name-conventions"></a>依赖属性名称约定

依赖属性具有命名约定；需要在除一些例外情况外的所有情形中遵循这些约定。 依赖属性本身有一个基本名称（上一个示例中的“Label”），它作为 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 的第一个参数提供。 该名称必须在每个注册类型中是唯一的，这种唯一性需求也适用于任何继承的成员。 通过基础类型继承的依赖属性已被视为注册类型的一部分；不能再次注册继承属性的名称。

**警告**  尽管你在此处提供的名称可以是在你选择的编程语言中有效的任何字符串标识符，但通常你也希望能够在 XAML 中设置依赖属性。 要在 XAML 中设置，你选择的属性名称必须是有效的 XAML 名称。 有关详细信息，请参阅 [XAML 概述](xaml-overview.md)。

创建标识符属性时，将你注册属性时的属性名称与后缀“Property”结合在一起（例如“LabelProperty”）。 此属性是依赖属性的标识符，并且它用作你在自己的属性包装器中执行的 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 和 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 调用的输入。 它还供属性系统以及其他 XAML 处理器（例如 [**{x:Bind}**](x-bind-markup-extension.md)）使用

## <a name="implementing-the-wrapper"></a>实现包装器

属性包装器应该在 **get** 实现中调用 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359)，在 **set** 实现中调用 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361)。

**警告**  在除例外情形外的所有情形中，包装器实现仅应执行 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 和 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 操作。 否则，在通过 XAML 设置属性时的行为与通过代码设置属性时的行为不同。 为了提高效率，在设置依赖属性时，XAML 分析程序将绕过包装器，并通过 **SetValue** 与后备存储通信。

> [!div class="tabbedCodeSnippets"]
```csharp
public String Label
{
    get { return (String)GetValue(LabelProperty); }
    set { SetValue(LabelProperty, value); }
}
```
```vb
Public Property Label() As String 
    Get 
        Return DirectCast(GetValue(LabelProperty), String) 
    End Get 
    Set(ByVal value As String) 
        SetValue(LabelProperty, value) 
    End Set 
End Property
```
```cpp
//using namespace Platform;
public:
...
  property String^ Label
  {
    String^ get() {
      return (String^)GetValue(LabelProperty);
    }
    void set(String^ value) {
      SetValue(LabelProperty, value); 
    }
  }
```

## <a name="property-metadata-for-a-custom-dependency-property"></a>自定义依赖属性的属性元数据

向一个依赖属性分配属性元数据时，针对属性所有者类型的每个实例或其子类，向该属性应用相同的元数据。 在属性元数据中，你可以指定两种行为：

-   属性系统在所有情况下向属性分配的默认值。
-   只要检测到属性值更改，就会在属性系统中自动调用静态回调方法。

### <a name="calling-register-with-property-metadata"></a>使用属性元数据调用注册

在调用 [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 的先前示例中，我们为 *propertyMetadata* 参数传递了一个 Null 值。 要使依存关系属性能够提供一个默认值，或使用某个属性已更改的回调，必须定义一个提供其中一项或全部两项功能的 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 实例。

通常，你将在 [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 的参数内提供一个 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771)，作为一个内联创建的参数。

**注意**  如果你要定义某个 [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) 实现，必须使用实用程序方法 [**PropertyMetadata.Create**](https://msdn.microsoft.com/library/windows/apps/hh702099)，而不是调用 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 构造函数来定义该 **PropertyMetadata** 实例。

下一个示例将通过使用 [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770) 值引用 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 实例，修改先前显示的 [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 示例。 本节的后续内容中将介绍“OnLabelChanged”回调的实现。

> [!div class="tabbedCodeSnippets"]
```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(String),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null,new PropertyChangedCallback(OnLabelChanged))
);
```
```vb
Public Shared ReadOnly LabelProperty As DependencyProperty = 
    DependencyProperty.Register("Label", 
      GetType(String), 
      GetType(ImageWithLabelControl), 
      New PropertyMetadata(
        Nothing, new PropertyChangedCallback(AddressOf OnLabelChanged)))
```
```cpp
DependencyProperty^ ImageWithLabelControl::_LabelProperty = 
    DependencyProperty::Register("Label", 
    Platform::String::typeid,
    ImageWithLabelControl::typeid, 
    ref new PropertyMetadata(nullptr,
      ref new PropertyChangedCallback(&ImageWithLabelControl::OnLabelChanged))
    );
```

### <a name="default-value"></a>默认值

你可以为依存关系属性指定一个默认值，这样，未设置该属性时，该属性将始终返回某个特定的默认值。 此值不同于该属性的类型的固有默认值。

如果未指定默认值，对于引用类型，依赖属性的默认值为空；对于值类型或语言原语，为该类型的默认值（例如 0 用于整型，或空字符串用于字符串）。 建立默认值的主要原因是，你在属性上调用 [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357) 时会还原此值。 为每个属性建立默认值可能比在构造函数中建立默认值更加方便，特别是对于值类型。 但是对于引用类型，请确保建立的默认值不会创建意外的单一实例模式。 有关详细信息，请参阅本主题后面的[最佳实践](#best-practices)

**注意**  请勿注册 [**UnsetValue**](https://msdn.microsoft.com/library/windows/apps/br242371) 的默认值。 如果注册了，它将让属性使用者难以理解，并且将在属性系统中产生意外的后果。

### <a name="createdefaultvaluecallback"></a>CreateDefaultValueCallback

在某些情况下，你将为在多个 UI 线程上使用的对象定义依存关系属性。 如果你要定义由多个应用使用的某个数据对象，或者要定义你在多个应用中使用的某个控件，则可能属于这种情况。 你可以通过提供一个 [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) 实现（而不是一个默认值实例）来启用在不同 UI 线程之间对象的交换，默认值实例被绑定到注册该属性的线程。 基本上，一个 [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) 为默认值定义一个工厂。 由 **CreateDefaultValueCallback** 返回的值始终与正在使用该对象的当前 UI **CreateDefaultValueCallback** 线程相关联。

要定义指定某个 [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) 的元数据，必须调用 [**PropertyMetadata.Create**](https://msdn.microsoft.com/library/windows/apps/hh702115) 来返回一个元数据实例；[**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 构造函数没有包含 **CreateDefaultValueCallback** 参数的签名。

[**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) 的典型实现模式是创建一个新 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 类，将 **DependencyObject** 的每个属性的特定属性值设置为预定的默认值，然后通过 **CreateDefaultValueCallback** 方法的返回值将新类返回为一个 **Object** 引用。

### <a name="property-changed-callback-method"></a>属性已更改的回调方法

你可以定义一个属性已更改的回调方法来定义你的属性与其他依赖属性的交互，或者更新一个内部属性或该属性更改时的对象状态。 如果调用该回调，则属性系统确定发生了有效的属性值更改。 因为回调方法是静态的，所以回调的 *d* 参数很重要，因为它会告诉你类的哪个实例报告了更改情况。 典型的实现使用事件数据的 [**NewValue**](https://msdn.microsoft.com/library/windows/apps/br242364) 属性并以某种方式处理该值，通常是在作为 *d* 传递的对象上执行其他某种更改。 对属性更改的其他响应包括拒绝 **NewValue** 报告的值，还原 [**OldValue**](https://msdn.microsoft.com/library/windows/apps/br242365)，或者将该值设置为应用于 **NewValue** 的编程约束。

下一个示例展示了一种 [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770) 实现。 它实现你在前面的 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 示例中引用的方法，作为 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 构造参数的一部分。 此回调解决的场景是，该类也有一个名为“HasLabelValue”的计算只读属性（未给出实现）。 只要重新计算了“Label”属性，就会调用此回调方法，该回调使依赖的计算值与依赖属性的更改保持同步。

> [!div class="tabbedCodeSnippets"]
```csharp
private static void OnLabelChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    ImageWithLabelControl iwlc = d as ImageWithLabelControl; //null checks omitted
    String s = e.NewValue as String; //null checks omitted
    if (s == String.Empty)
    {
        iwlc.HasLabelValue = false;
    } else {
        iwlc.HasLabelValue = true;
    }
}
```
```vb
    Private Shared Sub OnLabelChanged(d As DependencyObject, e As DependencyPropertyChangedEventArgs)
        Dim iwlc As ImageWithLabelControl = CType(d, ImageWithLabelControl) ' null checks omitted
        Dim s As String = CType(e.NewValue,String) ' null checks omitted
        If s Is String.Empty Then
            iwlc.HasLabelValue = False
        Else
            iwlc.HasLabelValue = True
        End If
    End Sub
```
```cpp
static void OnLabelChanged(DependencyObject^ d, DependencyPropertyChangedEventArgs^ e)
{
    ImageWithLabelControl^ iwlc = (ImageWithLabelControl^)d;
    Platform::String^ s = (Platform::String^)(e->NewValue);
    if (s->IsEmpty()) {
        iwlc->HasLabelValue=false;
    }
}
```

### <a name="property-changed-behavior-for-structures-and-enumerations"></a>结构和枚举的属性已更改行为

如果 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 的类型为枚举或结构，则可能会调用该回调，即使结构的内部值或枚举值未改变时也是如此。 这与系统基元（如仅当值改变时才会调用的字符串）不同。 这是在内部执行的对这些值的装箱和取消装箱操作的一个副作用。 如果你的值是枚举或结构时，你有一个针对某个属性的 [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770) 方法，那么你需要通过自己转换值并使用提供给即时转换值的超负荷的比较运算符来比较 [**OldValue**](https://msdn.microsoft.com/library/windows/apps/br242365) 和 [**NewValue**](https://msdn.microsoft.com/library/windows/apps/br242364)。 或者，如果没有这样的运算符（自定义结构可能是这种情形），那么你可能需要比较各个值。 如果结果是值未改变，那么你通常不会采取任何操作。

> [!div class="tabbedCodeSnippets"]
```csharp
private static void OnVisibilityValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    if ((Visibility)e.NewValue != (Visibility)e.OldValue)
    {
        //value really changed, invoke your changed logic here
    } // else this was invoked because of boxing, do nothing
}
```
```vb
Private Shared Sub OnVisibilityValueChanged(d As DependencyObject, e As DependencyPropertyChangedEventArgs)
    If CType(e.NewValue,Visibility) != CType(e.OldValue,Visibility) Then
        '  value really changed, invoke your changed logic here
    End If
    '  else this was invoked because of boxing, do nothing
End Sub
```
```cpp
static void OnVisibilityValueChanged(DependencyObject^ d, DependencyPropertyChangedEventArgs^ e)
{
    if ((Visibility)e->NewValue != (Visibility)e->OldValue)
    {
        //value really changed, invoke your changed logic here
    } 
    // else this was invoked because of boxing, do nothing
    }
}
```

## <a name="best-practices"></a>最佳做法

在定义自定义依赖属性时，将以下考虑因素作为最佳实践。

### <a name="dependencyobject-and-threading"></a>DependencyObject 和线程处理

所有 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 实例都必须在与 Windows 运行时应用所显示的当前 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 相关联的 UI 线程上创建。 虽然每个 **DependencyObject** 都必须在主 UI 线程上创建，但可以通过调用 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br230616) 从其他线程使用调度程序引用来访问这些对象。

[**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 的线程处理特性很重要，因为这通常意味着只有那些在 UI 线程上运行的代码才能更改或读取依赖属性的值。 在正确使用 **async** 模式和后台工作线程的典型 UI 代码中，通常可以避免线程处理问题。 通常，如果你定义自己的 **DependencyObject** 类型并尝试将这些类型用于 **DependencyObject** 未必适宜的数据源或其他场景，只会遇到与 **DependencyObject** 相关的线程处理问题。

### <a name="avoiding-unintentional-singletons"></a>避免意外的单一实例

如果声明一个接受引用类型的依赖属性，并且你在建立 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 的代码中针对该引用类型调用了一个构造函数，可能产生意外的单一实例。 发生的事情是，依赖属性的所有用途仅共享一个 **PropertyMetadata** 实例，进而尝试共享你构造的单个引用类型。 你通过依赖属性设置的该值类型的任何子属性随后会以你可能意想不到的方式传播到其他对象。

如果想要一个非空值，可以使用类构造函数设置一个引用类型依赖属性的初始值，但请注意，出于[依赖属性概述](dependency-properties-overview.md)用途，这将被视为一个局部值。 如果你的类支持模板，可能将模板用于此用途会更合适。 另一种避免单一实例模式，但仍然提供有用默认值的方式为，在引用类型上公开一个为该类的值提供合适默认值的静态属性。

### <a name="collection-type-dependency-properties"></a>集合类型依赖属性

集合类型依赖属性有另外一些实现问题需要考虑。

集合类型依赖属性在 Windows 运行时 API 中相对较少。 在大部分情况下，可以在各项内容是一个 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 子类时使用集合，但集合属性本身实现为一种传统的 CLR 或 C++ 属性。 这是因为集合不一定适用于某些调用依赖属性的典型场景。 例如：

-   你通常不会为集合制作动画。
-   你通常不会使用样式或模板预先填充集合中的各项。
-   尽管绑定到集合是一种主要的场景，但集合不需要将依赖属性用作绑定来源。 对于绑定目标，更典型的用法是使用 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/br242803) 或 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348) 的子类来支持集合项，或使用视图-模型模式。 有关绑定到集合和从集合绑定的详细信息，请参阅[深入了解数据绑定](https://msdn.microsoft.com/library/windows/apps/mt210946)。
-   集合更改通知问题最好通过 **INotifyPropertyChanged** 或 **INotifyCollectionChanged** 等接口，或通过从 [**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/ms668604.aspx) 派生的集合类型来解决。

但是，有些场景确实需要集合类型依赖属性。 接下来的 3 节提供了有关如何实现集合类型依赖属性的一些指南。

### <a name="initializing-the-collection"></a>初始化集合

创建依赖属性时，可通过依赖属性元数据的形式建立一个默认值。 但请注意，不要使用单一实例静态集合作为默认值。 相反，必须在集合属性的所有者类的类构造函数逻辑中特意将集合值设置为一个唯一的（实例）集合。

### <a name="change-notifications"></a>更改通知

将集合定义为依赖属性时，不会通过调用“PropertyChanged”回调方法的属性系统自动为集合中的各项提供更改通知。 如果想要集合或集合项的通知（例如用于数据绑定场景），可实现 **INotifyPropertyChanged** 或 **INotifyCollectionChanged** 接口。 有关详细信息，请参阅[深入了解数据绑定](https://msdn.microsoft.com/library/windows/apps/mt210946)。

### <a name="dependency-property-security-considerations"></a>依赖属性安全注意事项

将依赖属性声明为公共属性。 将依赖属性标识符声明为**公共静态只读**成员。 即使尝试声明语言所允许的其他访问级别（例如 **protected**），也始终可以结合使用标识符和属性系统 API 来访问依赖属性。 将依赖属性标识符声明为内部或私有不起作用，因为这样属性系统无法正常操作。

包装器属性实际上只是为了提供方便，应用于包装器的安全机制可通过调用 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 或 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 来绕过。 所以请保持包装器属性为公共的；否则只会使属性更难被合法调用方使用，并且不会提供任何切实的安全优势。

Windows 运行时没有提供将自定义依赖属性注册为只读的方式。

### <a name="dependency-properties-and-class-constructors"></a>依赖属性和类构造函数

一条一般原则是类构造函数不应调用虚拟方法。 这是因为可调用构造函数来完成派生类构造函数的基本初始化工作，并且在构造的对象实例未完成初始化时，可能发生通过构造函数进入虚拟方法的情形。 当通过任何派生自 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 的类进行派生时，请记住属性系统本身会在其服务中从内部调用和公开虚拟方法。 要避免运行时初始化的潜在问题，不要在类的构造函数中设置依赖属性值。

### <a name="registering-the-dependency-properties-for-ccx-apps"></a>注册 C++/CX 应用的依赖属性

由于分为标头文件和实现文件以及在实现文件的作用域上进行初始化是错误做法，所以在 C++/CX 中注册属性的实现比在 C# 中实现更为复杂。 （Visual C++ 组件扩展 (C++/CX) 将静态初始化器代码从根作用域直接放置在 **DllMain** 中，而 C# 编译器将静态初始化器分配到类，从而避免 **DllMain** 加载锁定问题。）。 此处执行的最佳做法是为某个类声明可注册所有依赖属性的 helper 函数，一个类对应一个函数。 然后，对于你的应用使用的每个自定义类，必须引用要使用的每个自定义类公开的 helper 注册函数。 在 `InitializeComponent` 之前，调用每个 helper 注册函数以作为 [**Application constructor**](https://msdn.microsoft.com/library/windows/apps/br242325) (`App::App()`) 的一部分。 例如，该构造函数仅在首次引用应用时运行，如果恢复暂停的应用，该构造函数不会再次运行。 同样，如之前 C++ 注册示例所示，每个 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 调用周围的 **nullptr** 标识非常重要：它确保该函数的任何调用方均不能注册此属性两次。 第二次注册调用可能会导致没有此类标识的应用崩溃，因为属性名称可能重复。 如果你要查找 C++/CX 版本示例的代码，请参阅 [XAML 用户和自定义控件示例](http://go.microsoft.com/fwlink/p/?linkid=238581)中的这一实现模式。

## <a name="related-topics"></a>相关主题

* [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)
* [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)
* [依赖属性概述](dependency-properties-overview.md)
* [XAML 用户和自定义控件示例](http://go.microsoft.com/fwlink/p/?linkid=238581)
 

