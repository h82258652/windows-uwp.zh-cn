---
title: 写入样式
description: 要让应用的文本看上去与其设计自然融为一体，使用正确的语音和语气非常重要。
keywords: UWP, Windows 10, 文本, 编写, 语音, 语气, 设计, UI, UX
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: aaaf13c455d3d05d5ccfac6b2bd61418f3e8e5bb
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "63820524"
---
# <a name="writing-style"></a>写入样式

![标题图像](images/header-writing-style.gif)

错误消息的措词方式、帮助文档的编写方式，甚至为按钮选择的文本都会对应用的可用性产生重大影响。 写作风格可能对用户体验的好坏产生重要影响。

## <a name="voice-and-tone-principles"></a>语音和声调原则

调查显示用户对于友好、有用和简洁的写作风格反应最佳。 作为此研究的一部分，Microsoft 确定了三个所有内容均适用的语音和声调原则，它们是 Fluent Design 不可或缺的组成部分。

### <a name="be-warm-and-relaxed"></a>温暖、轻松

最重要的是不要吓跑用户。 要通俗易懂，不要使用他们不理解的术语。 即使有错误发生，也不要因为任何问题而责怪用户。 相反，你的应用应该承担责任并提供将用户操作放在首位的热情指导。

### <a name="be-ready-to-lend-a-hand"></a>随时准备提供帮助

撰写风格始终表达出感同身受。 专注于介绍正在进行的操作，并提供用户所需的信息，而不要向他们加载过多不必要的信息。 如果可能，应始终在出现问题时提供解决方案。

### <a name="be-crisp-and-clear"></a>简洁、清晰

大多数时候，文本并非应用的焦点。 它的目的是指导用户，告诉他们正在进行的操作以及他们接下来应该怎么做。 不要在撰写应用文本时忽略这一点，不要假定用户会阅读每个字。 使用你的受众熟悉的语言，并确保能够轻松地过眼即懂。

## <a name="lead-with-whats-important"></a>突出重点

用户需要能够快速阅读和了解你的文本。 不要将篇幅浪费在不必要的介绍上。 最大限度地提高关键字的可见性，并且在添加之前始终展现核心思想。

:::row:::
    :::column:::
        ![Do](images/do.svg)
        Select **filters** to add effects to your image.
    :::column-end:::
    :::column:::
        ![Don't](images/dont.svg)
        If you want to add visual effects or alterations to your image, select **filters.**
    :::column-end:::
:::row-end:::

## <a name="emphasize-action"></a>强调操作

应用是由操作定义的。 用户在使用应用时会进行操作，而应用在响应用户时也会进行操作。 确保整个应用中的文本均使用主动语态  。 用户和功能应描述为它们主动执行操作，而不是对它们执行了操作。

:::row:::
    :::column:::
        ![Do](images/do.svg)
        Restart the app to see your changes.
    :::column-end:::
    :::column:::
        ![Don't](images/dont.svg)
        The changes will be applied when the app is restarted.
    :::column-end:::
:::row-end:::

## <a name="short-and-sweet"></a>简短扼要

用户快速阅读文本时，通常会完全跳过大块的文字。 请不要牺牲必要的信息和展示效果，但也不要使用不必要的字词。 有时候，这意味着可以使用许多更简短的句子或片段。 其他情况下，这意味着需要谨慎地选择较长句子的措辞和结构。

:::row:::
    :::column:::
        ![Do](images/do.svg)
        We couldn't upload the picture. If this happens again, try restarting the app. But don't worry — your picture will be waiting when you come back.
    :::column-end:::
    :::column:::
        ![Don't](images/dont.svg)
        An error occured, and we weren't able to upload the picture. Please try again, and if you encounter this problem again, you may need to restart the app. But don't worry — we've saved your work locally, and it'll be waiting for you when you come back.
    :::column-end:::
:::row-end:::

## <a name="style-conventions"></a>风格惯例

如果你认为自己不会成为一名作家，那么尝试实施这些原则和建议可能会具有挑战性。 但不用担心，使用简单直接的语言是提供良好用户体验的好方法。 如果你仍然不确定如何组织语言，这里有一些有用的指南以供参考。 如果要了解详细信息，请查看 [Microsoft 样式指南](https://docs.microsoft.com/style-guide/welcome/)。

### <a name="addressing-the-user"></a>称呼用户

直接与用户进行交流。
* 始终称呼用户为“你”。
* 用“我们”来指代你自己的角度。 这样既热情，又有助于让用户感觉像是属于体验的一部分。
* 不要使用“我”来指代应用的角度，即使你是唯一创建它的人。

:::row:::
    :::column:::
        ![Do](images/do.svg)
        We couldn't save your file to that location.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="abbreviations"></a>缩写词

 当你需要在整个应用中多次提及产品、地点或技术概念时，缩写词可能会很有用。 它们可以节省空间，并且感觉上更加自然，前提是只要用户能够理解它们。
* 请勿假定用户已经熟悉了任何的缩写词，即使你认为它们很常见。
* 始终在用户第一次看到新的缩写词时定义它的含义。
* 请勿使用过于相似的缩写词。
* 如果你正在本地化自己的应用，或者如果你的用户将英语作为第二语言，请勿使用缩写。

:::row:::
    :::column:::
        ![Do](images/do.svg)
        The Universal Windows Platform (UWP) design guidance is a resource to help you design and build beautiful, polished apps. With the design features that are included in every UWP app, you can build user interfaces (UI) that scale across a range of devices.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="contractions"></a>语言简练

人们习惯于使用简练的语言，并且很乐意阅读简练的文字。 如果语言不够简练，你的应用会看起来过于正式，甚至是生硬。
* 当简练的语言能够自然融入到文本中时，请使用它们。
* 不要只是为了节省空间或者当这些简练的语言会让你的文字听起来拗口的情况下使用。

:::row:::
    :::column:::
        ![Do](images/do.svg)
        When you're happy with your image, select **save** to add it to your gallery. From there, you'll be able to share it with friends.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="periods"></a>句号

 用句号结尾的文本意味着该文本是一个完整的句子。 将句号用于较大的文本块，并且避免用于比一个完整句子较短的文本。
* 将句号用作工具提示、错误消息和对话中完整句子的结尾。
* 请勿将句号用作按钮、单选按钮、标签或复选框文本的结尾。

:::row:::
    :::column:::
        ![Do](images/do.svg)
        <b>You’re not connected.</b>
        * Check that your network cables are plugged in.
        * Make sure you're not in airplane mode.
        * See if your wireless switch is turned on.
        * Restart your router.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="capitalization"></a>大写

尽管大写字母很重要，但容易过度使用。
* 大写专有名词。
* 大写应用中文本的每个字符串的开头：每个句子、标签和标题的开头。

:::row:::
    :::column:::
        ![Do](images/do.svg)
        <b>Which part is giving you trouble?</b>
        * I forgot your password.
        * It won't accept password.
        * Someone else might be using my account.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

## <a name="error-messages"></a>错误消息

当应用中出现问题时，用户会注意到这一点。 由于用户在遇到错误消息时可能会感到困惑或懊恼，因此良好的语音和语气可能会在这方面产生特别重要的影响。

最重要的是，错误消息不能责怪用户。 但是，同样重要的是，不要用他们不理解的信息使他们感到不知所措。 大多数时间，遇到错误的用户只是想尽快轻松地返回到他们之前正在进行的操作。 因此，编写的任何错误消息应该：

* 温暖、轻松  ：使用谈话的口吻，避免使用陌生的术语和技术行话。

* 随时准备提供帮助  ：尽可能告诉用户出现了什么问题，告知他们将会发生什么，并提供他们能够实施的可行解决方案。

* 简洁、清晰  ：去除无关的信息。

:::row:::
    :::column:::
        ![Do](images/do.svg)
        <b>You’re not connected.</b>
        * Check that your network cables are plugged in.
        * Make sure you're not in airplane mode.
        * See if your wireless switch is turned on.
        * Restart your router.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end::: 

## <a name="dialogs"></a>对话框

:::row:::
    :::column:::
        Many of the same advice for writing error messages also applies when creating the text for any dialogs in your app. While dialogs are expected by the user, they still interrupt the normal flow of the app, and need to be helpful and concise so the user can get back to what they were doing.

        But most important is the "call and response" between the title of a dialog and its buttons. Make sure that your buttons are clear answers to the question posed by the title, and that their format is consistent across your app.
    :::column-end:::
    :::column:::
        ![Do](images/do.svg)
        <b>Which part is giving you trouble?</b>
        1. I forgot my password
        2. It won't accept my password
        3. Someone else might be using my account
    :::column-end:::
:::row-end:::

## <a name="buttons"></a>按钮

:::row:::
    :::column:::
        Text on buttons needs to be concise enough that users can read it all at a glance and clear enough that the button's function is immediately obvious. The longest the text on a button should ever be is a couple short words, and many should be shorter than that.
        When writing the text for buttons, remember that every button represents an action. Be sure to use the *active voice* in button text, to use words that represent actions rather than reactions.
    :::column-end:::
    :::column:::
        ![Do](images/do.svg)
        * Install now
        * Share
    :::column-end:::
:::row-end:::

## <a name="spoken-experiences"></a>口语体验

在为 Cortana 等口语体验编写文本时，相同的通用原则和建议也适用。 在这些功能中，良好的写作原则甚至更为重要，因为你无法为用户提供其他视觉设计元素来补充说出的字词。

* 温暖、轻松  ：以谈话的口吻与用户进行互动。 比其他任何方面都重要的是，要让口语体验听上去热情且平易近人，并且使用户敢于与之进行交谈。

* 随时准备提供帮助  ：当用户询问无法做到的要求时，请提供替代建议。 就像在错误消息中一样，如果出现问题并且应用无法满足请求，它应该为用户提供一个可以尝试进行询问的可行选择。

* 简洁、清晰  ：保持语言的简单。 口语体验中不适合使用长句或复杂的字词。

## <a name="accessibility-and-localization"></a>辅助功能和本地化

如果在编写文本时考虑到辅助功能和本地化，你的应用可能会接触到更多的受众。 这是仅通过文本无法达成的，尽管简单友好的语言是一个很好的开始。 有关详细信息，请参阅[辅助功能概述](https://docs.microsoft.com/windows/uwp/design/accessibility/accessibility-overview)和[本地化指南](https://docs.microsoft.com/windows/uwp/design/globalizing/globalizing-portal)。

* 随时准备提供帮助  ：考虑到不同的体验。 避免使用可能对国际受众无意义的短语，并且请勿使用假设用户可以和不可以做什么的字词。

* 简洁、清晰  ：避免使用不必要的特殊和专业词汇。 文本越简单，就越容易进行本地化。


## <a name="techniques-for-non-writers"></a>适合非作家的技巧

无需成为训练有素或经验丰富的作家即可为用户提供良好的体验。 选择那些对你来说听起来舒服的字词，它们同样也会为其他人带来舒适感。 但有时，这并没有听上去的那么容易。 如果遇到问题，这些技巧可以帮助你。 

* 想象一下，你正在和一位朋友谈论你的应用。 如何向他们介绍此应用？ 如何介绍其功能或如何为他们提供说明？ 最好向尚未使用过的真实用户介绍此应用。 

* 想象一下你会如何描述一个完全不同的应用。 例如，如果你正在编写游戏应用，想象一下该如何用语言或文字来介绍一款财务或新闻应用。 通过对比所使用的语言和结构，可以让更深入地了解适用于你所编写应用的正确字词。

* 关注类似的应用以获取灵感。 

找到正确的字词是许多人纠结的一个问题，因此，难以确定最自然的字词时也不要感到沮丧。
