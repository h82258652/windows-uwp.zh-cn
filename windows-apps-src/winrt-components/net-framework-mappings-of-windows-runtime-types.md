---
title: Windows 运行时类型的.NET framework 映射
description: 下表列出了 .NET Framework 在通用 Windows 平台 (UWP) 类型和 .NET Framework 类型之间产生的映射。
ms.assetid: 5317D771-808D-4B97-8063-63492B23292F
---

# Windows 运行时类型的.NET framework 映射


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

\[有些信息与可能在商业发行之前就经过实质性修改的预发布产品相关。 Microsoft 不对此处提供的信息作任何明示或默示的担保。\]

下表列出了 .NET Framework 在通用 Windows 平台 (UWP) 类型和 .NET Framework 类型之间产生的映射。 在使用托管代码编写的通用 Windows 应用中，IntelliSense 将显示 .NET Framework 类型而不是 UWP 类型。 例如，如果 Windows 运行时方法采用类型 IVector<string> 的参数，IntelliSense 将显示类型 IList<string> 的参数。 同样，在使用托管代码编写的 Windows 运行时组件中，你将使用成员签名中的 .NET Framework 类型。 当 [Windows 运行时元数据导出工具 (Winmdexp.exe)](https://msdn.microsoft.com/library/hh925576.aspx) 生成 Windows 运行时组件时，.NET Framework 类型将变为相应的 UWP 类型。

## 映射表


在 UWP 和 .NET Framework 中，大多数具有相同的命名空间名称和类型名称的类型都是结构（或者与结构关联的类型，例如枚举）。 在 UWP 中，结构没有字段以外的成员，并且需要 .NET Framework 隐藏的帮助程序类型。 这些 .NET Framework 版本的结构具有可提供隐藏帮助程序类型的功能的属性和方法。

有关 .NET Framework 使用 Windows 元数据来通过 Windows 运行时简化编程的方式的详细信息，请从 Windows 开发人员中心下载 [CLR 和 Windows 运行时](http://download.microsoft.com/download/2/3/E/23E1E9BE-41AA-4716-A7B3-82040271394C/CLR%20and%20the%20Windows%20Runtime.docx)白皮书。

表 1：使用不同的名称和/或命名空间映射到 .NET Framework 类型的 UWP 类型。

| UWP 类型/命名空间                                            | .NET Framework 类型/命名空间                                          | .NET framework 程序集                           |
|---------------------------------------------------------------|------------------------------------------------------------------------|---------------------------------------------------|
| AttributeUsageAttribute (Windows.Foundation.Metadata)         | AttributeUsageAttribute (System)                                       | System.Runtime.dll                                |
| AttributeTargets (Windows.Foundation.Metadata)                | AttributeTargets (System)                                              | System.Runtime.dll                                |
| DateTime (Windows.Foundation)                                 | DateTimeOffset (System)                                                | System.Runtime.dll                                |
| EventHandler<T> (Windows.Foundation)                    | EventHandler<T> (System)                                         | System.Runtime.dll                                |
| EventRegistrationToken (Windows.Foundation)                   | EventRegistrationToken (System.Runtime.InteropServices.WindowsRuntime) | System.Runtime.InteropServices.WindowsRuntime.dll |
| HResult (Windows.Foundation)                                  | Exception (System)                                                     | System.Runtime.dll                                |
| IReference<T> (Windows.Foundation)                      | Nullable<T> (System)                                             | System.Runtime.dll                                |
| TimeSpan (Windows.Foundation)                                 | TimeSpan (System)                                                      | System.Runtime.dll                                |
| Uri (Windows.Foundation)                                      | Uri (System)                                                           | System.Runtime.dll                                |
| IClosable (Windows.Foundation)                                | IDisposable (System)                                                   | System.Runtime.dll                                |
| IIterable<T> (Windows.Foundation.Collections)           | IEnumerable<T> (System.Collections.Generic)                      | System.Runtime.dll                                |
| IVector<T> (Windows.Foundation.Collections)             | IList<T> (System.Collections.Generic)                            | System.Runtime.dll                                |
| IVectorView<T> (Windows.Foundation.Collections)         | IReadOnlyList<T> (System.Collections.Generic)                    | System.Runtime.dll                                |
| IMap<K,V> (Windows.Foundation.Collections)              | IDictionary<TKey,TValue> (System.Collections.Generic)            | System.Runtime.dll                                |
| IMapView<K,V> (Windows.Foundation.Collections)          | IReadOnlyDictionary<TKey,TValue> (System.Collections.Generic)    | System.Runtime.dll                                |
| IKeyValuePair<K,V> (Windows.Foundation.Collections)     | KeyValuePair<TKey,TValue> (System.Collections.Generic)           | System.Runtime.dll                                |
| IBindableIterable (Windows.UI.Xaml.Interop)                   | IEnumerable (System.Collections)                                       | System.Runtime.dll                                |
| IBindableVector (Windows.UI.Xaml.Interop)                     | IList (System.Collections)                                             | System.Runtime.dll                                |
| INotifyCollectionChanged (Windows.UI.Xaml.Interop)            | INotifyCollectionChanged (System.Collections.Specialized)              | System.ObjectModel.dll                            |
| NotifyCollectionChangedEventHandler (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventHandler (System.Collections.Specialized)   | System.ObjectModel.dll                            |
| NotifyCollectionChangedEventArgs (Windows.UI.Xaml.Interop)    | NotifyCollectionChangedEventArgs (System.Collections.Specialized)      | System.ObjectModel.dll                            |
| NotifyCollectionChangedAction (Windows.UI.Xaml.Interop)       | NotifyCollectionChangedAction (System.Collections.Specialized)         | System.ObjectModel.dll                            |
| INotifyPropertyChanged (Windows.UI.Xaml.Data)                 | INotifyPropertyChanged (System.ComponentModel)                         | System.ObjectModel.dll                            |
| PropertyChangedEventHandler (Windows.UI.Xaml.Data)            | PropertyChangedEventHandler (System.ComponentModel)                    | System.ObjectModel.dll                            |
| PropertyChangedEventArgs (Windows.UI.Xaml.Data)               | PropertyChangedEventArgs (System.ComponentModel)                       | System.ObjectModel.dll                            |
| TypeName (Windows.UI.Xaml.Interop)                            | Type (System)                                                          | System.Runtime.dll                                |

 

表 2：使用相同的名称和命名空间映射到 .NET Framework 类型的 UWP 类型。

| 命名空间                           | 类型               | .NET framework 程序集                   |
|-------------------------------------|--------------------|-------------------------------------------|
| Windows.UI                          | Color              | System.Runtime.WindowsRuntime.dll         |
| Windows.Foundation                  | Point              | System.Runtime.WindowsRuntime.dll         |
| Windows.Foundation                  | Rect               | System.Runtime.WindowsRuntime.dll         |
| Windows.Foundation                  | Size               | System.Runtime.WindowsRuntime.dll         |
| Windows.UI.Xaml.Input               | ICommand           | System.ObjectModel.dll                    |
| Windows.UI.Xaml                     | CornerRadius       | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | Duration           | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | DurationType       | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | GridLength         | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | GridUnitType       | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | Thickness          | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Controls.Primitives | GeneratorPosition  | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media               | Matrix             | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Animation     | KeyTime            | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Animation     | RepeatBehavior     | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Animation     | RepeatBehaviorType | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Media3D       | Matrix3D           | System.Runtime.WindowsRuntime.UI.Xaml.dll |

 

## 相关主题

* [使用 C# 和 Visual Basic 创建 Windows 运行时组件](creating-windows-runtime-components-in-csharp-and-visual-basic.md)



<!--HONumber=Mar16_HO1-->


