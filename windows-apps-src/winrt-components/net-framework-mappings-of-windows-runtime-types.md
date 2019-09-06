---
title: Windows 运行时类型的 .NET 映射
description: 下表列出了 .NET 在通用 Windows 平台（UWP）类型和 .NET 类型之间进行的映射。
ms.assetid: 5317D771-808D-4B97-8063-63492B23292F
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c035db58fc6aa484f9d47a9af61176a2b05d55ee
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393659"
---
# <a name="net-mappings-of-windows-runtime-types"></a>Windows 运行时类型的 .NET 映射

下表列出了 .NET 在通用 Windows 平台（UWP）类型和 .NET 类型之间进行的映射。 在用托管代码编写的通用 Windows 应用程序中，Visual Studio IntelliSense 显示 .NET 类型而不是 UWP 类型。 例如，如果 Windows 运行时方法采用&lt;IVector 字符串&gt;类型的参数，则 IntelliSense 将显示类型为 IList&lt;string&gt;的参数。 同样，在使用托管代码编写的 Windows 运行时组件中，你可以在成员签名中使用 .NET 类型。 当[Windows 运行时元数据导出工具（Winmdexp）](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool)生成 Windows 运行时组件时，.net 类型将转换为相应的 UWP 类型。

UWP 和 .NET 中具有相同命名空间名称和类型名称的大多数类型都是结构（或与结构（如枚举）关联的类型。 在 UWP 中，结构除了字段外没有其他成员，并且需要 .NET 隐藏的帮助器类型。 这些结构的 .NET 版本具有属性和方法，可提供隐藏帮助程序类型的功能。

## <a name="uwp-types-that-map-to-net-types-with-the-same-name-and-namespace"></a>映射到具有相同名称和命名空间的 .NET 类型的 UWP 类型

### <a name="in-net-assembly-systemobjectmodeldll"></a>在 .NET 程序集中 System.collections.objectmodel.collection

| 命名空间 | 类型 |
|-|-|
| Windows.UI.Xaml.Input | ICommand |

### <a name="in-net-assembly-systemruntimewindowsruntimedll"></a>在 .NET 程序集 WindowsRuntime 中

| 命名空间 | 类型 |
|-|-|
| Windows.Foundation | 点 |
| Windows.Foundation | Rect |
| Windows.Foundation | Size |
| Windows.UI | 颜色 |

### <a name="in-net-assembly-systemruntimewindowsruntimeuixamldll"></a>在 .NET 程序集的 WindowsRuntime 中

| 命名空间 | 类型 |
|-|-|
| Windows.UI.Xaml | CornerRadius |
| Windows.UI.Xaml | Duration |
| Windows.UI.Xaml | DurationType |
| Windows.UI.Xaml | GridLength |
| Windows.UI.Xaml | GridUnitType |
| Windows.UI.Xaml | Thickness |
| Windows.UI.Xaml.Controls.Primitives | GeneratorPosition |
| Windows.UI.Xaml.Media | Matrix |
| Windows.UI.Xaml.Media.Animation | KeyTime |
| Windows.UI.Xaml.Media.Animation | RepeatBehavior |
| Windows.UI.Xaml.Media.Animation | RepeatBehaviorType |
| Windows.UI.Xaml.Media.Media3D | Matrix3D |

## <a name="uwp-types-that-map-to-net-types-with-a-different-name-andor-namespace"></a>映射到具有不同名称和/或命名空间的 .NET 类型的 UWP 类型

### <a name="in-net-assembly-systemobjectmodeldll"></a>在 .NET 程序集中 System.collections.objectmodel.collection

| UWP 类型/命名空间 | .NET 类型/命名空间 |
|-|-|
| INotifyCollectionChanged (Windows.UI.Xaml.Interop) | INotifyCollectionChanged (System.Collections.Specialized) | 
| NotifyCollectionChangedEventHandler (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventHandler (System.Collections.Specialized) | 
| NotifyCollectionChangedEventArgs (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventArgs (System.Collections.Specialized) | 
| NotifyCollectionChangedAction (Windows.UI.Xaml.Interop) | NotifyCollectionChangedAction (System.Collections.Specialized) | 
| INotifyPropertyChanged (Windows.UI.Xaml.Data) | INotifyPropertyChanged (System.ComponentModel) | 
| PropertyChangedEventHandler (Windows.UI.Xaml.Data) | PropertyChangedEventHandler (System.ComponentModel) | 
| PropertyChangedEventArgs (Windows.UI.Xaml.Data) | PropertyChangedEventArgs (System.ComponentModel) | 

### <a name="in-net-assembly-systemruntimedll"></a>在 .NET 程序集中

| UWP 类型/命名空间 | .NET 类型/命名空间 |
|-|-|
| AttributeUsageAttribute (Windows.Foundation.Metadata) | AttributeUsageAttribute (System) |
| AttributeTargets (Windows.Foundation.Metadata) | AttributeTargets (System) |
| DateTime (Windows.Foundation) | DateTimeOffset (System) |
| EventHandler&lt;T&gt; (Windows.Foundation) | EventHandler&lt;T&gt; (System) |
| HResult (Windows.Foundation) | Exception (System) |
| IReference&lt;T&gt; (Windows.Foundation) | Nullable&lt;T&gt; (System) |
| TimeSpan (Windows.Foundation) | TimeSpan (System) |
| Uri (Windows.Foundation) | Uri (System) |
| IClosable (Windows.Foundation) | IDisposable (System) |
| IIterable&lt;T&gt; (Windows.Foundation.Collections) | IEnumerable&lt;T&gt; (System.Collections.Generic) |
| IVector&lt;T&gt; (Windows.Foundation.Collections) | IList&lt;T&gt; (System.Collections.Generic) |
| IVectorView&lt;T&gt; (Windows.Foundation.Collections) | IReadOnlyList&lt;T&gt; (System.Collections.Generic) |
| IMap&lt;K,V&gt; (Windows.Foundation.Collections) | IDictionary&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IMapView&lt;K,V&gt; (Windows.Foundation.Collections) | IReadOnlyDictionary&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IKeyValuePair&lt;K,V&gt; (Windows.Foundation.Collections) | KeyValuePair&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IBindableIterable (Windows.UI.Xaml.Interop) | IEnumerable (System.Collections) |
| IBindableVector (Windows.UI.Xaml.Interop) | IList (System.Collections) |
| TypeName (Windows.UI.Xaml.Interop) | Type (System) |

### <a name="in-net-assembly-systemruntimeinteropserviceswindowsruntimedll"></a>在 .NET 程序集 InteropServices. WindowsRuntime 中

| UWP 类型/命名空间 | .NET 类型/命名空间 |
|-|-|
| EventRegistrationToken (Windows.Foundation) | EventRegistrationToken (System.Runtime.InteropServices.WindowsRuntime) |

## <a name="related-topics"></a>相关主题

* [用C#和 Visual Basic Windows 运行时组件](creating-windows-runtime-components-in-csharp-and-visual-basic.md)