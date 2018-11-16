---
author: jwmsft
ms.assetid: E1943DCE-833F-48AE-8402-CD48765B24FC
title: 优化暂停/恢复
description: 创建通用 Windows 平台 (UWP) 应用，以简化其进程生存期系统的使用，从而在暂停或终止后高效地恢复。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4cbaa56f9c25c0e4ea1f10c79b4f7d1100748532
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6970255"
---
# <a name="optimize-suspendresume"></a>优化暂停/恢复


创建通用 Windows 平台 (UWP) 应用，以简化其生存期系统的使用过程，从而在暂停或终止后高效地恢复。

## <a name="launch"></a>启动

当重新激活暂停/终止后的应用时，请检查是否已过去了很长的时间。 如果是这样，请考虑返回到应用的主登录页面，而不是显示用户过时数据。 这样也可以更快地启动。

在激活期间，请随时检查事件实参形参的 PreviousExecutionState（例如，对于启动的激活，请检查 LaunchActivatedEventArgs.PreviousExecutionState）。 如果该值是 ClosedByUser 或 NotRunning，则不必花费时间还原以前保存的状态。 在此情况下，应当提供全新的体验 - 从而可以更快地启动。

请考虑跟踪以前保存的状态，并且仅在需要时进行还原，而不是急着还原该状态。 例如，请考虑以下情况：你的应用之前已暂停，保存了 3 个页面的状态，然后终止了。 在重新启动后，如果你决定将用户返回到第三页，请不要急着还原前两页的状态。 相反，保持此状态，并且仅在你得知需要它时才使用。

## <a name="while-running"></a>在运行时

最佳做法是不要等待暂停事件，也不要保留大量的状态。 相反，你的应用程序在运行时应当以增量方式保留少量的状态。 这一点对于在暂停期间存在时间不足风险的较大应用来说尤其重要（如果它们尝试一次性保存所有内容）。

但是，你需要在运行时在应用的增量保存和性能之间找到良好的平衡。 良好的平衡就是以增量方式跟踪已发生更改的数据（从而需要进行保存），并使用暂停事件来实际保存该数据（这要比保存所有数据或检查应用的整个状态来确定要保存的内容更快速）。

不要使用窗口“已激活”或 VisibilityChanged 事件来确定何时保存状态。 当用户离开你的应用时，将停用该窗口，但系统会在暂停应用之前等待一小段时间（大约 10 秒）。 这是为了提供响应速度更快的体验，以防用户快速切换回你的应用。 在运行暂停逻辑前等待暂停事件。

## <a name="suspend"></a>暂停

在暂停期间，请减少你的应用的占用。 如果你的应用在暂停时使用的内存较少，整个系统将可以更快速地响应，而且将终止更少的已暂停应用（包括你的应用）。 但是，对此进行平衡需要快速的恢复：在应用将大量数据重新加载到内存中时，不要减少太多的占用，否则恢复速度会大幅降低。

对于托管的应用，系统会在应用的暂停事件处理程序完成后运行垃圾回收过程。 请务必利用此过程，方法是释放对在暂停时有助于减少应用占用的对象的引用。

理想情况下，你的应用将在 1 秒内结束暂停逻辑。 暂停速度越快，效果越好。这样会为其他应用或部分系统带来更快速的用户体验。 如果必须这样做，你的暂停逻辑在桌面设备上可能需要 5 秒钟，在移动设备上可能需要 10 秒钟。 如果超出了这些时间，你的应用会突然终止。 你不希望发生这种情况 - 因为如果发生此情况，当用户切换回你的应用时，将启动一个新进程，并且与恢复暂停的应用相比，体验也会慢很多。

## <a name="resume"></a>恢复

大多数应用无需在恢复时执行任何特殊操作，因此通常情况下，你不会处理此事件。 某些应用使用恢复来还原暂停期间关闭的连接，或者刷新可能已过时的数据。 设计你的应用以根据需要启动这些活动，而不是急着执行此类工作。 这将在用户切换回暂停的应用时带来更快速的体验，并确保你仅执行用户真正需要的工作。

## <a name="avoid-unnecessary-termination"></a>避免不必要的终止

UWP 进程生存期系统可以出于各种原因暂停或终止应用。 此进程专用于使应用快速返回到暂停或终止前所处的状态。 如果此进程执行情况良好，用户将不会注意到应用曾经停止运行。 以下是一些技巧，你的 UWP 应用可以使用这些技巧在应用的生存期内帮助系统简化过渡。

用户将应用移至后台或系统进入低功耗状态时，可将应用暂停。 暂停应用时，会引发暂停事件，而且最多有 5 秒时间保存其数据。 如果应用的暂停事件处理程序不能在 5 秒内完成，则系统会假定应用已停止响应并终止该应用。 当用户切换到已终止的应用时，该应用必须重新经历很长的启动过程，而不是立即加载到内存中。

### <a name="serialize-only-when-necessary"></a>仅在需要时序列化

许多应用会在挂起时序列化其所有数据。 但如果你仅需要存储少量的应用设置数据，则应使用 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/BR241622) 存储，而不应序列化数据。 对较多数量的数据和非设置数据使用序列化。

当执行数据序列化时，应避免在数据没有更改时重新对其进行序列化。 序列化和保存数据需要花费额外的时间，重新激活应用时还会花费额外的时间来读取和反序列化数据。 因而，我们建议应用确定其状态是否已实际更改，如果已更改，则仅需序列化和反序列化已更改的数据即可。 确保发生这种情况的一个好方法是在数据更改后在后台定期序列化数据。 如果使用此技术，那么暂停时需要进行序列化的所有数据都已进行过保存，因此，不需要进行任何操作，且应用会快速暂停。

### <a name="serializing-data-in-c-and-visual-basic"></a>使用 C# 和 Visual Basic 序列化数据

对于 .NET 应用，可供选择的序列化技术有 [**System.Xml.Serialization.XmlSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.xml.serialization.xmlserializer.aspx)、[**System.Runtime.Serialization.DataContractSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.runtime.serialization.datacontractserializer.aspx) 和 [**System.Runtime.Serialization.Json.DataContractJsonSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.runtime.serialization.json.datacontractjsonserializer.aspx) 类。

从性能角度看，我们建议使用 [**XmlSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.xml.serialization.xmlserializer.aspx) 类。 **XmlSerializer** 的序列化和反序列化时间最短，并保持很低的内存占用。 **XmlSerializer** 对 .NET 框架的依赖很少；这意味着与其他序列化技术相比较，使用 **XmlSerializer** 时需要加载到你的应用中的模块会更少。

[**DataContractSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.runtime.serialization.datacontractserializer.aspx) 使自定义类序列化更加容易，尽管它具有比 **XmlSerializer** 更高的性能影响。 如果需要更好的性能，请考虑切换。 一般情况下，不应加载多个序列化程序，并且应首选 **XmlSerializer**，除非需要使用其他序列化程序的功能。

### <a name="reduce-memory-footprint"></a>减少内存占用

系统尝试尽量在内存中保存多个已暂停应用，以便用户可快速可靠地在这些应用中进行切换。 如果应用暂停并保留在系统内存中，则可快速为用户将应用转至前台以供用户交互使用，而不必显示初始屏幕或执行长时间的加载操作。 如果没有足够的资源将应用保持在内存中，则应用将被终止。 以下两个原因使内存管理变得很重要：

-   在暂停时尽可能多地释放内存会最大程度地减少你的应用因暂停时缺少资源而被终止的几率。
-   减少你的应用使用的内存总量会减少其他应用在暂停时被终止的几率。

### <a name="release-resources"></a>释放资源

某些对象（如文件和设备）占用大量内存。 我们建议在挂起期间应用释放对这些对象的处理并在需要时重新创建处理。 此时也很适合清除恢复应用时不再有效的所有缓存。 对于 C# 和 Visual Basic，XAML 框架会为你运行的一个附加步骤是垃圾回收（如有必要）。 这可确保释放在应用代码中不再引用的任何对象。

## <a name="resume-quickly"></a>快速恢复

在用户将应用移至前台时或在系统退出低功耗状态时，可恢复已暂停的应用。 应用从已暂停状态恢复时，它会从暂停的位置和时间处继续运行。 由于应用数据存储在内存中，所以不会丢失任何应用数据，即使应用已暂停很长一段时间也是如此。

大多数应用不需要处理 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/BR205859) 事件。 恢复应用时，变量和对象具有应用暂停时其所处的相同状态。 仅当你需要更新在暂停应用的时间与恢复应用的时间之间可能已更改的数据或对象（比如，内容（例如，更新源数据）、可能已经过时的网络连接）时，或者需要重新获取对设备（如摄像头）的访问时，才处理 **Resuming** 事件。

## <a name="related-topics"></a>相关主题

* [应用暂停和恢复指南](https://msdn.microsoft.com/library/windows/apps/Hh465088)
 

 




