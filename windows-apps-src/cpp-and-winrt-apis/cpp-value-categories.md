---
author: stevewhims
description: 本主题介绍各种类别的 c + + 中存在的值。 您肯定会听到左值和 rvalues，但有其他类型，太。
title: 值类别
ms.author: stwhi
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10 uwp、 标准、 c + +、 cpp、 winrt、 投影、 移动、 转接、 值类别、 移动语义，完全转发、 左值、 rvalue、 glvalue、 prvalue，xvalue
ms.localizationpriority: medium
ms.openlocfilehash: 75176334909e0e6bcf81763b543a19b70a4cba73
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/10/2018
ms.locfileid: "2739068"
---
# <a name="value-categories"></a>值类别
本主题介绍各种类别的 c + + 中存在的值。 您肯定会听到*左值*和*rvalues*，但有其他类型，太。 C + + 中的每个表达式的结果值属于某个本主题中讨论的类别。 有 c + + 语言，其 facilies 和需求值类别正确理解的规则的方面。

## <a name="an-lvalue-has-identity"></a>左值具有标识
对于具有*标识*的值，它意味着什么？ 如果您有 （或者您可以采取） 值的内存地址的值将具有标识。 这样，您可以执行多个比较值的内容： 可比较或 identity 区分它们。

*左值*具有标识。 现在是"l"左"值"中的为缩写形式 （如所，左左工作分配）"left"的仅历史利息空格。 在 c + +，左值会显示上的左侧*或*右侧的工作分配。 在"左值"，"l"然后不实际帮助您理解也不定义它们是什么。 您只需了解，我们所说左值是具有标识的值。

现在，左值具有标识的则返回 true 语句时，因此不要 xvalues。 我们可以转到稍微更多到哪些*xvalue*是本主题后面 （尽管完全处理它们不在范围中）。 现在，只需注意存在名为"常用左值"为 glvalue，值类别。 Glvalues 超集包含左值 （也称为"经典左值"） 和 xvalues。 因此，时"左值具有标识"为 true 时，具有标识的操作的完整集是一套 glvalues，在此图中所示。

![左值具有标识](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Rvalue 是可移动;左值不是
但有不 glvalues 的值。 因此，没有您*无法*获取的内存地址 （或不能依赖有效） 的值。 这听起来像一个缺点。 但实际上值利用 like 即，您可以*移动*它 （这是通常便宜），而不是副本 it （这是通常昂贵）。 移动一个值，意味着它不再是它用于就地。 因此，尝试访问它用于是就地不是以避免。 从讨论了何时和*如何*移动超出此主题的范围，则值。 本主题中，我们只需了解一个值，是可移动称为*rvalue*。

"Rvalue"中的"r"是"右"（如所，右键左工作分配） 的缩写。 但是，您可以使用 rvalues 和 rvalues，外部工作分配的引用。 "Rvalues"中的"r"，则不需要重点关注的一点。 您只需了解，我们所说 rvalue 是可移动的值。

左值，相反，不是可移动的在此图中所示。 您无法移动左值，因为如果可能，则它将不安全 （或甚至灾难性） 继续此后访问。 请记住，您具有其身份。

![Rvalue 是可移动;左值不是](images/is-movable.png)

不能移动左值。 但存在** 一种 glvalue （标识为操作的一组），可以移动&mdash;如果您知道您正在制作&mdash;，xvalue。 我们将随着此主意一个更多时间下，当我们查看完成值类别的图片。

## <a name="an-lvalue-has-identity-a-prvalue-does-not"></a>左值具有标识;未显示 prvalue
在此阶段，我们知道什么具有标识。 和我们知道什么是可移动，什么不是。 但我们尚未尚未命名集的值** 具有标识。 该集被称为*prvalue*或"纯 rvalue"。

![左值具有标识;未显示 prvalue](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>值类别的完整图片
它仅待合并信息和到单一的大图片上面的插图。

![值类别的完整图片](images/value-categories.png)

- Glvalue （通用左值） 具有标识。
- 左值 （一种 glvalue） 具有标识，但不可移动。 这些是通常的读写值传递给周围通过引用 const 参考，或按值便宜复制是否。
- Xvalue （一种 glvalue，但还一种 rvalue） 具有标识，并且还可移动。 这可能已决定移动复制很高，因为 erstwhile 左值，您会注意不要此后访问。 左值变为 xvalue 和用于移动一个，原因的方式将需要等待另一个主题。 但是，您可以将"xvalue"中的"x"视为含义"专家"如果的帮助。
- Prvalue （纯 rvalue; 一种 rvalue） 不具有标识，但是是可移动。 这些方案通常是文字，临时变量，返回值&mdash;（不是 glvalue 表达式），表达式的计算结果的任何内容或任何从函数返回的值。
- 可移动 rvalue。
