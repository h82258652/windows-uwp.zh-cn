---
author: QuinnRadich
Description: "设计说明性用户界面 (UI)，可向用户解释如何使用你的 Windows 应用商店应用。"
title: "设计说明性 UI 指南"
label: Instructional UI
template: detail.hbs
ms.author: quradic
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: c87e2f06-339d-4413-b585-172752964f56
ms.openlocfilehash: 3d1d1e139baf82800ca999c0ab931f7965882b0c
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="instructional-ui-guidelines"></a>说明性 UI 指南



在某些情况下，向用户解释应用中他们不熟悉的功能（例如特定的触摸交互）会很有用。 在这些情况下，你需要通过用户界面 (UI) 向用户显示说明，以便他们可以使用可能错过的这些功能。

## <a name="when-to-use-instructional-ui"></a>何时使用说明性 UI

说明性 UI 必须谨慎使用。 如果过度使用，它很容易被忽略或使用户感到厌烦，从而导致效率很低。

说明性 UI 应用于帮助用户发现重要但不明显的应用功能，例如触摸手势或他们可能感兴趣的设置。 它还可以用于提醒用户有关他们可能忽略的应用中的新功能或更改。

除非应用依赖触摸手势，否则说明性 UI 不应用于向用户解释应用的基本功能。

## <a name="principles-of-writing-instructional-ui"></a>编写说明性 UI 的原则

好的说明性 UI 与用户息息相关，并对用户有教育意义，还能增强用户体验。 它应该：

-   **简单：**用户不会想让他们的体验被复杂信息打断。
-   **记忆深刻：**用户不会想在每次尝试某项任务时都看到相同的说明，因此说明应能让他们过目不忘。
-   **直接相关：**如果说明性 UI 不能向用户解释他们想立即执行的操作，他们也就没有理由关注它。

避免过度使用说明性 UI，并确保选择正确的主题。 不要解释：

-   **基本功能：**如果用户需要说明才能使用你的应用，请考虑将应用设计制作的更为直观。
-   **明显的功能：**如果用户不用说明就能自行了解某项功能，则说明性 UI 会成为阻碍。
-   **复杂的功能：**说明性 UI 需要保持简洁，并且对复杂功能感兴趣的用户通常愿意去查找说明，而无需向他们提供这些说明。

避免由于说明性 UI 而给用户造成不便。 错误做法：

-   **遮盖重要信息：**说明性 UI 应永远不能妨碍应用的其他功能。
-   **强制用户参与：**用户应能够忽略说明性 UI 并仍能继续使用应用。
-   **显示重复信息：**不要让说明性 UI 给用户造成困扰，即使在他们第一次忽略此 UI 时也是如此。 添加用于重新显示说明性 UI 的设置是更好的解决方案。

## <a name="examples-of-instructional-ui"></a>说明性 UI 的示例

下面提供一些实例，其中的说明性 UI 可帮助你的用户了解：

-   **帮助用户发现触控交互。** 以下屏幕截图显示，说明性 UI 教玩家如何在“Cut the Rope”游戏中使用触控笔势。

    ![显示说明性 UI 消息“Slide acress to cut the rope”的游戏屏幕截图](images/in-game-controls-3.png)

-   **留下良好的第一印象。** 当影音时光首次启动时，说明性 UI 会在不阻碍用户体验的情况下提示他们开始制作电影。

    ![影音时光应用的启动屏幕](images/instructional-ui-movie.png)

-   **指导用户在复杂的任务中采取下一步骤。** 在“Windows 邮件”应用中，收件箱底部的提示将用户指向“设置”****以访问较早的消息。

    ![显示说明性 UI 消息的“Windows 邮件”应用的裁剪屏幕截图](images/instructional-ui-mail-inbox.png)

    在用户单击该消息时，应用的“设置”****浮出控件会立即显示在屏幕的右侧，以便用户完成该任务。 下面的屏幕截图显示了当用户单击说明性 UI 消息之前和之后的“邮件”应用。

    | 之前                                                               | 调整后的文本                                                                                                        |
    |----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
    | ![“Windows 邮件”应用的屏幕截图](images/instructional-ui-mail.png) | ![带有扩展设置浮出控件的 Windows 邮件应用的屏幕截图](images/instructional-ui-mail-flyout.png) |

## <a name="related-articles"></a>相关文章

* [应用帮助指南](guidelines-for-app-help.md)
