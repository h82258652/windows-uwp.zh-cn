---
title: 在你的测试环境中授权 Xbox Live 帐户
description: 了解如何授权公用 Xbox Live 帐户以用于在开发环境中进行测试。
ms.assetid: 9772b8f1-81db-4c57-a54d-4e9ca9142111
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 帐户, 测试帐户
ms.localizationpriority: medium
ms.openlocfilehash: 5f48f9baa574128530f5794b441c45a8c8f980eb
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2018
ms.locfileid: "8738466"
---
# <a name="authorize-xbox-live-accounts-for-testing-in-your-environment"></a>在你的环境中授权 Xbox Live 帐户以进行测试

本主题将介绍使用发布者测试环境设置 Xbox Live 帐户的过程

## <a name="prerequisites"></a>先决条件

你需要以下内容才能授权 Xbox Live 测试帐户：

* [Xbox Live 帐户](https://support.xbox.com/browse/my-account/manage-account/Create%20account)

## <a name="navigate-to-the-xbox-test-account-page"></a>导航到“Xbox 测试帐户”页

该文件位于合作伙伴中心的帐户设置部分

你可以使用两种方式中的任一种到达此部分

1. 在[合作伙伴中心](https://partner.microsoft.com/dashboard/windows/overview)，单击设置齿轮 ⚙ 的右上方在仪表板中，从下拉列表中选择**开发人员设置**。 此操作将打开你将在其中选择 Xbox Live 下拉列表，然后选择**Xbox 测试帐户**链接在屏幕左侧导航菜单。
2. 从“Xbox Live 创意者配置”页中找到测试部分，然后单击名为**为测试环境授权 Xbox Live 帐户**的链接

## <a name="authorize-an-xbox-live-account-for-your-test-environment"></a>为测试环境授权 Xbox Live 帐户

* 在“Xbox 测试帐户”页中，你应该能看到所有当前授权帐户的列表。 要授权一个新帐户，请单击“添加帐户”按钮

![添加 Xbox Live 帐户](../images/creators_udc/add_test_account.png)

* 屏幕中应该会弹出一个模式对话框和一个文本框，你可以在文本框中输入所需帐户的电子邮件地址

![添加 Xbox Live 帐户模式对话框](../images/creators_udc/add_test_account_modal.png)

* 单击“添加”按钮验证电子邮件地址是否存在和是否具有关联的 Xbox Live 帐户。 如果检查通过，模式对话框将会消失，你会在表中看到新的帐户，指示它已经成功获得用于测试环境的授权。

![添加 Xbox Live 帐户成功](../images/creators_udc/add_test_account_success.png)

## <a name="troubleshooting"></a>疑难解答

在模式对话框中输入的电子邮件会经过一些检查，包括查找是否有与其关联的 Xbox Live 帐户。 如果任意检查未通过，则帐户不会添加到表中且不会获得授权，你可能会收到“抱歉，添加电子邮件地址时出现问题”错误。

如果遇到问题，正确的检查方法是，尝试在 [Xbox.com](http://www.xbox.com/live/) 上使用帐户登录。 如果你无法登录，则说明此帐户不是 Xbox Live 帐户。
