---
author: serenaz
title: Windows ML 概述
description: 了解有关 Windows 机器学习和如何使用 Windows ML 开发的信息。
ms.author: sezhen
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, windows, 机器学习
ms.localizationpriority: medium
ms.openlocfilehash: 2ef6ea1a4e1dab23f5ff6a09aec9b8c49c135f5e
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2018
ms.locfileid: "1817658"
---
# <a name="windows-ml-overview"></a>Windows ML 概述

![Windows 机器学习图形](images/brain.png)

## <a name="what-is-machine-learning"></a>什么是机器学习？

机器学习 (ML) 允许计算机使用现有数据来预测预期的结果和行为。 通过处理之前收集的数据，ML 算法将生成可预测使用新输入时的正确输出的模型。 例如，可以训练模型以评估电子邮件消息（输入）是否为垃圾邮件（输出）。

模型生成阶段称为“训练”。 使用现有数据训练之后，该模型可以使用以前未见过的新数据来执行预测，这叫做“推断”、“评估”或“评分”。

经过训练的模型通常能获得比严格执行一系列指令的程序更好的效果，对于具有许多可能的输入输出组合的复杂任务而言尤其是这样。 例如，建议算法为电子商务和媒体流式处理站点的数以百万级的用户提供个性化的建议，若不使用机器学习，几乎是不可能实现的。 利用机器学习的另一个领域是计算机视觉，这项技术让计算机能够通过基于之前标记的图像进行训练后，对图像进行分类和识别。

机器学习的可能性和应用是无穷无尽的；有关研究和解决方案的更多信息，请访问 [Microsoft 人工智能](https://www.microsoft.com/ai)和 [Microsoft AI 平台](https://azure.microsoft.com/en-us/overview/ai-platform/)。 如果你想生成机器学习和 AI 模型，也可以了解一下 [Azure 机器学习服务](https://docs.microsoft.com/azure/machine-learning/preview/overview-what-is-azure-ml)。

## <a name="what-is-windows-ml"></a>什么是 Windows ML？

Windows ML 是一个评估 Windows 10 这边上经过训练的机器学习模型的平台，它让开发人员能够在其 Windows 应用程序中使用机器学习技术。

Windows ML 一些重要功能包括：

- **硬件加速**
    
    在支持 DirectX12 的设备上，Windows ML 能使用 GPU 加快评估深度学习模型的速度。 此外，CPU 优化又帮助实现对经典 ML 和深度学习算法的高性能评估。

- **本地评估**

    Windows ML 在本地硬件上进行评估，因此无需担忧连接、带宽和数据隐私等问题。 本地评估还有低延迟和高性能的优势，能快速获得评估结果。

- **图像处理**

    对于计算机视觉情形，Windows ML 通过执行帧预处理和为模型输入提供相机管道设置，简化并优化图像、视频和相机数据的使用。

## <a name="how-to-develop-with-windows-ml"></a>如何使用 Windows ML 进行开发

> [!VIDEO https://www.youtube.com/embed/8MCDSlm326U]

### <a name="system-requirements"></a>系统要求

要生成使用 Windows ML 的应用程序，你将需要 [Windows SDK - 版本 17110 或更高版本](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK)。

### <a name="onnx-models"></a>ONNX 模型

若要使用 Windows ML，你需要预先训练的[开放神经网络交换 (ONNX)](https://onnx.ai) 格式的机器学习模型。 Windows ML 支持 ONNX 格式的 v1.0 发布，从而允许开发人员使用不同训练框架生成的模型。

有关公开发布的 ONNX 模型的列表，请参阅 GitHub 上的 [ONNX 模型](https://github.com/onnx/models)。

若要了解如何使用 Visual Studio Tools for AI 训练 ONNX，请参阅[训练模型](train-ai-model.md)。

### <a name="convert-existing-models-to-onnx"></a>将现有模型转换为 ONNX

许多训练框架已经本机支持 ONNX 模型，并且对于许多框架和库都有转换器工具。 若要了解如何从 Caffe 2、PyTorch、CNTK、Chainer 等框架导出，请参阅 GitHub 上的 [ONNX 教程](https://github.com/onnx/tutorials)。

你还可以使用 [WinMLTools](https://pypi.org/project/winmltools/) 以将经过训练的机器学习模型转换为 Windows ML 接受的 ONNX 格式。 WinMLTools 支持从以下格式转换：

- Core ML
- Scikit-Learn
- XGBoost
- LibSVM

若要了解如何安装和使用 WinMLTools，请参阅[转换模型](conversion-samples.md)。

### <a name="onnx-operators"></a>ONNX 运算符

Windows ML 在 CPU 上支持 100 多个 ONNX 运算器，在兼容 DirectX12 GPU 上可加速计算。 有关完整运算器签名的列表，请参阅 [ai.onnx](https://github.com/onnx/onnx/blob/rel-1.0/docs/Operators.md) ONNX 运算器架构文档（默认）和[ai.onnx.ml](https://github.com/onnx/onnx/blob/rel-1.0/docs/Operators-ml.md)命名空间。

Windows ML 支持所有 ONNX v1.0 文档中定义的所有运算符，但有以下区别：

- 受 Windows ML 支持的标记为“实验”的运算器：
    - Affine
    - Crop
    - FC
    - Identity
    - ImageScaler
    - MeanVarianceNormalization
    - ParametricSoftplus
    - ScaledTanh
    - ThresholdedRelu
    - Upsample
- MatMul - 目前不支持大于 2D 的矩阵乘法，仅在 CPU 上支持
- Cast - 仅在 CPU 上受支持
- 目前不支持以下运算器：
    - RandomUniform
    - RandomUniformLike
    - RandomNormal
    - RandomNormalLike

### <a name="automatic-interface-code-generation"></a>自动接口代码生成

对于 ONNX 型号文件，Windows ML 的代码生成器会创建一个接口与你的应用中的模型交互。 生成的接口包括表示模型、输入和输出的包装类。 生成的代码为你调用 [Windows ML API](/uwp/api/windows.ai.machinelearning.preview)，这样你就可以轻松在项目中加载、绑定和评估模型。 代码生成器目前支持 C# 和 C + + / CX。

对于 UWP 开发人员，Windows ML 的自动代码生成器在本机与 [Visual Studio（版本 15.7 - Preview 1）](https://www.visualstudio.com/vs/preview/)集成。 (**注意**：在 Visual Studio 安装程序内，你将需要查看可选 Windows 10 Insider 预览版 SDK，版本 17110 ），在 Visual Studio 项目中，只需将你的 ONNX 文件添加为现有项目，VS 将在新接口文件中生成 Windows ML 包装类。

你还可以使用 Windows SDK 附带的命令行工具 `mlgen.exe` 以生成 Windows ML 包装类。 该工具位于`(SDK_root)\bin\<version>\x64` 或 `(SDK_root)\bin\<version>\x86`，其中 SDK_root 是 SDK 安装目录。 要允许该工具，请使用以下命令。

```
mlgen -i INPUT-FILE -l LANGUAGE -n NAMESPACE [-o OUTPUT-FILE]
```

输入参数定义：

- `INPUT-FILE`：ONNX 模型文件
- `LANGUAGE`：CPPCX 或 CS
- `NAMESPACE`: 生成的代码的命名空间
- `OUTPUT-FILE`: 生成的代码将写入其中的文件路径 如果未指定输出文件，所生成的代码将写入标准输出

若要了解如何在你的应用中使用生成的代码，请参阅[集成模型](integrate-model.md)。

## <a name="next-steps"></a>后续步骤

使用[入门](get-started.md)中的分步教程尝试创建你的第一个 Windows ML 应用。