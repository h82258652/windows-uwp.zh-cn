---
author: serenaz
title: 使用 Windows ML 将模型集成到你的应用中
description: 按照加载、绑定和评估的模式，将模型集成到你的应用中
ms.author: sezhen
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, winml, Windows 机器学习
ms.localizationpriority: medium
ms.openlocfilehash: 87454c46a08b80b8a315ede1d2e6a1f6cda909df
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2018
ms.locfileid: "1815472"
---
# <a name="integrate-a-model-into-your-app-with-windows-ml"></a>使用 Windows ML 将模型集成到你的应用中

Windows ML 的[自动代码生成](overview.md#automatic-interface-code-generation)功能可创建一个接口来为你调用 [Windows ML API](/uwp/api/windows.ai.machinelearning.preview)，从而让你轻松地与你的模型进行交互。 使用该接口生成的包装类，你将按照加载、绑定和评估模式将你的 ML 模型集成到应用中。

![加载、绑定、评估](images/load-bind-evaluate.png)

在本文中，我们将使用[入门](get-started.md)中的 MNIST 模型演示如何在你的应用中加载、绑定和评估模型。

## <a name="add-the-model"></a>添加模型

首先，你需要将 ONNX 模型添加到 Visual Studio 项目的资产。 如果你要使用 [Visual Studio（v15.7 - Preview 1）](https://www.visualstudio.com/vs/preview/)生成 UWP 应用，则 Visual Studio 将自动在新文件中生成包装类。 对于其他工作流，你将需要使用 [mlgen](overview.md#automatic-interface-code-generation) 工具来生成包装类。

下面是 Windows ML 为 MNIST 型号生成的包装类。 我们将在本文的其余部分说明如何在应用中使用这些类。

```csharp
public sealed class MNISTModelInput
{
    public VideoFrame Input3 { get; set; }
}

public sealed class MNISTModelOutput
{
    public IList<float> Plus214_Output_0 { get; set; }
    public MNISTModelOutput()
    {
        this.Plus214_Output_0 = new List<float>();
    }
}

public sealed class MNISTModel
{
    private LearningModelPreview learningModel;
    public static async Task<MNISTModel> CreateMNISTModel(StorageFile file)
    {
        LearningModelPreview learningModel = await LearningModelPreview.LoadModelFromStorageFileAsync(file);
        MNISTModel model = new MNISTModel();
        model.learningModel = learningModel;
        return model;
    }
    public async Task<MNISTModelOutput> EvaluateAsync(MNISTModelInput input) {
        MNISTModelOutput output = new MNISTModelOutput();
        LearningModelBindingPreview binding = new LearningModelBindingPreview(learningModel);
        binding.Bind("Input3", input.Input3);
        binding.Bind("Plus214_Output_0", output.Plus214_Output_0);
        LearningModelEvaluationResultPreview evalResult = await learningModel.EvaluateAsync(binding, string.Empty);
        return output;
    }
}
```

**注意**：要在确保模型在你编译你的应用程序生成，请右键单击 `.onnx` 文件，然后选择**属性**。 对于**版本操作**，选择**内容**。

## <a name="load"></a>加载

生成包装类之后，你将在应用中加载模型。

MNISTModel 类表示 MNIST 模型。为了加载模型，我们要将 ONNX 文件作为参数传入以调用 CreateMNISTModel 方法。

```csharp
// Load the model
StorageFile modelFile = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/MNIST.onnx"));
MNISTModel model = MNISTModel.CreateMNISTModel(modelFile);
```

## <a name="bind"></a>绑定

生成的代码还包括输入和输出包装类。 输入类表示预期模型的预期输入，输出类表示模型的预期输出。

若要初始化模型的输入对象，请传入你的应用程序数据来调用输入类构造程序，并确保输入数据与模型预期的输入类型匹配。 MNISTModelInput 类需要一个 VideoFrame，因此我们使用帮助程序方法来获取一个 VideoFrame 作为输入。

```csharp
//Bind the input data to the model
MNISTModelInputs ModelInput = new MNISTModelInputs();
ModelInput.Input3 = await helper.GetHandWrittenImage(inkGrid);
```

输出对象初始化为 *Null* 值，并将在下一步即评估之后使用模型结果更新。

## <a name="evaluate"></a>评估

一旦输入初始化完成，请调用模型的 EvaluateAsync 方法对输入数据评估你的模式。 EvaluateAsync 将你的输入和输出绑定到模型对象，并对输入评估模型。

```csharp
// Evaluate the model
MNISTModelOuput ModelOutput = model.EvaluateAsync(ModelInput);
```

评估后，您的输出中包含模型的结果，你现在可以查看和分析这些结果。 由于 MNIST 模型输出一个概率的列表，我们将循环访问该列表，以查找并显示具有最高可能性的数字。

```csharp
 //Iterate through output to determine highest probability digit
float maxProb = 0;
int maxIndex = 0;
for (int i = 0; i < 10; i++)
{
    if (ModelOutput.Plus214_Output_0[i] > maxProb)
    {
        maxIndex = i;
        maxProb = ModelOutput.Plus214_Output_0[i];
    }
}
numberLabel.Text = maxIndex.ToString();
```

## <a name="using-the-windows-ml-apis-directly"></a>直接使用 Windows ML API

虽然 Windows ML 的自动代码生成器提供了一个方便的接口与你的模型进行交互，但你不是一定要使用包装类。 你可以直接在应用中调用 Windows ML API。
如果你选择执行此操作，则需遵循相同的模式：加载你的模型、绑定你的输入和输出，以及评估。

有关使用 API 的详细信息，请参阅 [Windows ML API 参考](/uwp/api/windows.ai.machinelearning.preview)。
