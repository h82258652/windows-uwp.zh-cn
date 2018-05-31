---
author: serenaz
title: 如何在 Visual Studio 中训练 Windows ML 模型
description: 通过此分步教程了解如何使用 Visual Studio Tools for AI 训练 Windows ML 模型。
ms.author: sezhen
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Windows 机器学习, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: 5d8b50b6b8779b98de1d93f449aa560b5dcda893
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
ms.locfileid: "1690213"
---
# <a name="how-to-train-a-model-for-windows-ml-in-visual-studio"></a>如何在 Visual Studio 中训练 Windows ML 模型
在本教程中，我们将使用 [Visual Studio Tools for AI](http://aka.ms/vstoolsforai)（一个用于生成、测试和部署深度学习与 AI 解决方案的开发扩展）来为[入门](get-started.md)中的 MNIST 示例应用训练模型。

我们将使用 [Microsoft Cognitive Toolkit (CNTK)](http://www.microsoft.com/en-us/cognitive-toolkit) 框架和 [MNIST 数据集](http://yann.lecun.com/exdb/mnist/)来训练该模型，该数据集包含 60,000 个示例的训练集和 10,000 个手写数字示例的测试集。 然后我们将使用[开放神经网络交换 (ONNX)](https://onnx.ai/)格式保持该模型，以与 Windows ML 一起使用。 

## <a name="prerequisites"></a>先决条件
### <a name="install-visual-studio-tools-for-ai"></a>安装 Visual Studio Tools for AI
若要开始，首先需要下载和安装 [Visual Studio](https://www.visualstudio.com/downloads/)。 打开 Visual Studio 之后，激活 **Visual Studio Tools for AI** 扩展：

1. 单击 Visual Studio 菜单栏，然后选择“扩展和更新...”
2. 单击“联机”选项卡，然后选择“搜索 Visual Studio Marketplace”。
3. 搜索“Visual Studio Tools for AI”。 
3. 单击**下载**按钮。 
4. 安装后，重启 Visual Studio。 

Visual Studio 重启后，扩展将处于活动状态。 如果你遇到问题，请查看[查找 Visual Studio 扩展](hhttps://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions)。

### <a name="download-sample-code"></a>下载示例代码
下载 GitHub 上的 [AI 示例](https://github.com/Microsoft/samples-for-ai)存储库。 这些示例包括跨 TensorFlow、CNTK 和 Theano 等的深度学习的入门示例。

### <a name="install-cntk"></a>安装 CNTK
安装 [CNTK for Python on Windows](https://docs.microsoft.com/en-us/cognitive-toolkit/setup-windows-python?tabs=cntkpy24)。 请注意，如果尚未安装 Python，则需安装。

或者，为了为深度学习模型开发准备你的计算机，请参阅[准备开发环境](https://github.com/Microsoft/samples-for-ai/blob/master/README.md)，获取用于安装 Python、CNTK、TensorFlow、NVIDIA GPU 驱动程序（可选）等的简化安装程序。

## <a name="1-open-project"></a>1. 打开项目

启动 Visual Studio，然后依次选择 **文件 > 打开 > 项目/解决方案**。 从 AI 存储库中的示例中，选择 **examples\cntk\python** 文件夹，然后打开 **CNTKPythonExamples.sln** 文件。

![打开解决方案](images/open-solution.png)

## <a name="2-train-the-model"></a>2. 训练模型

若要将 MNIST 项目设置为启动项目，请右键单击该 python 项目，然后选择 **设为启动项目**。

![打开解决方案](images/mnist-startup.png)

接下来，打开 ConvNet_MNIST.py 文件，按 F5 或绿色的运行按钮**运行**该项目。

## <a name="3-view-the-model-and-add-it-to-your-app"></a>3. 查看该模型，并将其添加到你的应用

打开 AI 存储库中的实例中的 **output/Models** 文件夹。 训练实验中的每个时段都会有一个经过培训的 DNN 模型，最后一步将写入 **MNIST.onnx** 模型文件。 

![打开解决方案](images/onnx-model-output.png)

现在，你可以使用此经过训练的 **MNIST.onnx** 模型文件来生成[入门](get-started.md)中的那个 MNIST 示例应用了！ 

## <a name="4-learn-more"></a>4. 了解详细信息
若要了解如何通过使用 [Azure GPU 虚拟机](https://docs.microsoft.com/en-us/visualstudio/ai/tensorflow-vm)等加快培训深度学习模型的速度，请访问 [Microsoft 人工智能](https://www.microsoft.com/ai)和 [Microsoft 机器学习技术](https://docs.microsoft.com/en-us/azure/machine-learning/#More-Microsoft-Machine-Learning-Technologies)。