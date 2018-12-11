---
Description: Use a label to indicate to the user what they should enter into an adjacent control. You can also label a group of related controls, or display instructional text near a group of related controls.
title: 标签
ms.assetid: CFACCCD4-749F-43FB-947E-2591AE673804
label: Labels
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4345daf5b879fed7ba9805e4a448c473299031d7
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2018
ms.locfileid: "8828928"
---
# <a name="labels"></a>标签

 

标签是一个控件或一组相关控件的名称或标题。

> **重要 API**：Header 属性，[TextBlock 类](https://msdn.microsoft.com/library/windows/apps/br209652)

在 XAML 中，许多控件都具有用于显示标签的内置 Header 属性。 对于没有 Header 属性的控件，或要为多组控件添加标签，你可以改用 [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)。

![说明标准标签控件的屏幕截图](images/label-standard.png)

## <a name="recommendations"></a>建议


-   使用标签向用户指示他们应向邻近控件输入的内容。 还可以为一组相关控件添加标签，或在一组相关控件旁显示说明文本。
-   在为控件添加标签时，请将标签写成名词或简洁的名词短语，不要写成一个句子，也不要写成说明文本。 避免使用冒号或其他标点符号。
-   当标签中确实有说明文本时，文本字符串长度可以增加，并且可以使用标点符号。


## <a name="get-the-sample-code"></a>获取示例代码
* [XAML UI 基本示例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>相关主题
* [文本控件](text-controls.md)
* [TextBox.Header 属性](https://msdn.microsoft.com/library/windows/apps/dn252861)
* [PasswordBox.Header 属性](https://msdn.microsoft.com/library/windows/apps/dn299051)
* [ToggleSwitch.Header 属性](https://msdn.microsoft.com/library/windows/apps/br209713)
* [DatePicker.Header 属性](https://msdn.microsoft.com/library/windows/apps/dn279460)
* [TimePicker.Header 属性](https://msdn.microsoft.com/library/windows/apps/dn299286)
* [Slider.Header 属性](https://msdn.microsoft.com/library/windows/apps/dn252829)
* [ComboBox.Header 属性](https://msdn.microsoft.com/library/windows/apps/dn279416)
* [RichEditBox.Header 属性](https://msdn.microsoft.com/library/windows/apps/dn252726)
* [TextBlock 类](https://msdn.microsoft.com/library/windows/apps/br209652)

 

 




