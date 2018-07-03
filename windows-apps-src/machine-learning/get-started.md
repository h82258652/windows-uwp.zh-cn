---
author: serenaz
title: Windows ML 入门
description: 使用此分步教程创建你的第一个 Windows ML 应用。
ms.author: sezhen
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, windows 机器学习, winml, windows ML
ms.localizationpriority: medium
ms.openlocfilehash: eec2ada8e3aadad134381a93bca2652133912b2e
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843620"
---
# <a name="get-started-with-windows-ml"></a>Windows ML 入门

在此教程中，我们将生成一个简单的 UWP 应用，使用经过训练的机器学习模型来识别用户绘制的数字。 此教程主要介绍如何在应用中加载和使用 Windows ML。

## <a name="prerequisites"></a>必备软件

- [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)（版本 17110 或更高版本）
- [Visual Studio](https://developer.microsoft.com/windows/downloads)

## <a name="1-download-the-sample"></a>1. 下载示例

首先，你需要从 GitHub 下载我们的 [MNIST_GetStarted 示例](https://github.com/Microsoft/Windows-Machine-Learning)。 我们提供了已实现 XAML 控件和事件的模板，包括：

- 一个 [InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)，用于绘制数字。
- [按钮](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button)，用于解释数字并清除画布。
- 帮助程序例程，用于将 InkCanvas 输出转换为 [VideoFrame](https://docs.microsoft.com/uwp/api/windows.media.videoframe)。

GitHub 中还有一个完整的 MNIST 示例可供下载。

## <a name="2-open-project-in-visual-studio-preview"></a>2. 在 Visual Studio 预览版中打开项目

启动 Visual Studio 预览版，并打开 MNIST 示例应用程序。 （如果解决方案显示为不可用，你将需要单击鼠标右键，然后选择"重新加载项目"。）

在解决方案资源管理器内，项目有三个主要代码文件：

- `MainPage.xaml` - 我们的所有 XAML 代码，用来创建 InkCanvas、按钮和标签的 UI。
- `MainPage.xaml.cs` - 我们的应用程序代码所在位置。
- `Helper.cs` - 帮助程序例程，用于裁剪和转换图像格式。

![包含项目文件的 Visual Studio 解决方案资源管理器](images/get-started1.png)

## <a name="3-build-and-run-project"></a>3. 生成并运行项目

在 Visual Studio 工具栏中，将解决方案平台从“ARM”更改为“x64”以在你的本地计算机上运行该项目。

要运行该项目，请单击工具栏上的**开始调试**按钮或按 F5 键。 应用程序应显示用户可写入数字的 InkCanvas，一个用来解释数字的“识别”按钮，一个将经过解释的数字显示为文本的空标签字段，以及一个用来清除 InkCanvas 内容的“清除数字”按钮。

![应用程序的屏幕截图](images/get-started2.png)

**注意**：如果下载比 17110 更高的版本，则你可能需要更改项目的部署目标版本。 在项目解决方案下，转到**属性**。 在“应用程序”选项卡下，设置与你的操作系统和 SDK 匹配的目标版本。

## <a name="4-download-a-model"></a>4. 下载模型

接下来，让我们来了解要添加到应用中的机器学习模型。 本教程中将使用预先经过训练的 **MNIST 模型**，它是使用 [Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/) 训练的，并已[导出到 ONNX 格式](https://github.com/onnx/tutorials/blob/master/tutorials/CntkOnnxExport.ipynb)。

如果你要使用来自 GitHub 的 MNIST_GetStarted 示例，MNIST 模型已包含在你的资产文件夹中，你将需要将它作为现有项添加到应用程序中。 你还可以从 GitHub 上的 [ONNX Model Zoo](https://github.com/onnx/models) 下载经过预先训练的模型。

如果你对训练自己的模型有兴趣，可以按照我们用来训练此 MNIST 模型的[教程](train-ai-model.md)操作。

## <a name="5-add-the-model"></a>5. 添加模型

下载 MNIST 模型之后，请在解决方案资源管理器中右键单击“资源”文件夹，然后选择**添加** > **现有项目**。 将文件选取器指向 ONNX 模型的位置，然后单击“添加”。

该项目现在应该有两个新文件：

- `MNIST.onnx` - 经过训练的模型。
- `MNIST.cs` - Windows ML 生成的代码。

![包含新文件的解决方案资源管理器](images/get-started3.png)

要确保在我们编译应用程序时模型能够生成，请右键单击 `mnist.onnx` 文件，然后选择**属性**。 对于**版本操作**，选择**内容**。

现在，让我们来看看 `MNIST.cs` 文件中新生成的代码。 我们有三个类：

- **MNISTModel** 创建机器学习模型表示，将特定输入和输出绑定到模型，并异步评估模型。 
- **MNISTModelInput** 初始化模型所需的输入类型。 在本例中，输入应该是 VideoFrame。
- **MNISTModelOutput** 初始化模型将输出的类型。 在本例中，输出将是一个名为“classLabel”的 `<long>` 类型的列表以及一个名为“prediction”的以下类型的字典 `<long, float>`

现在我们将使用这些类在我们的项目中加载、绑定并评估模型。

## <a name="6-load-bind-and-evaluate-the-model"></a>6. 加载、绑定并评估模型

对于 Windows ML 应用程序，我们想要遵循的模式是：加载 > 绑定 > 评估。

- 加载机器学习模型。
- 将输入和输出绑定到模型。
- 评估模型并查看结果。

我们将使用在 `MNIST.cs` 中生成的界面代码在我们的应用中加载、绑定和评估模型。

首先，在 `MainPage.xaml.cs` 中实例化模型、输入和输出。

```csharp
namespace MNIST_Demo
{
    public sealed partial class MainPage : Page
    {
        private MNISTModel ModelGen = new MNISTModel();
        private MNISTModelInput ModelInput = new MNISTModelInput();
        private MNISTModelOutput ModelOutput = new MNISTModelOutput();
        ...
    }
}
```

然后，在 LoadModel() 中加载模型。

```csharp
private async void LoadModel()
{
    //Load a machine learning model
    StorageFile modelFile = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/MNIST.onnx"));
    ModelGen = await MNISTModel.CreateMNISTModel(modelFile);
}
```

接下来，我们要将输入和输出绑定到模型。 

在本例中，我们的模型需要 VideoFrame 类型的输入。 使用我们在 `helper.cs` 中包含的帮助程序函数，我们将复制 InkCanvas 的内容、将它转换为 VideoFrame 类型，然后绑定到模型。

```csharp
private async void recognizeButton_Click(object sender, RoutedEventArgs e)
{
     //Bind model input with contents from InkCanvas
     ModelInput.Input3 = await helper.GetHandWrittenImage(inkGrid);
}
```

对于输出，我们只需使用指定输入调用 EvaluateAsync()。 由于模型将返回一个具有对应可能性的数字的列表，我们需要分析返回的列表，以确定哪个数字具有最高的可能性，然后显示该数字。

```csharp
private async void recognizeButton_Click(object sender, RoutedEventArgs e)
{
    //Bind model input with contents from InkCanvas
    ModelInput.Input3 = await helper.GetHandWrittenImage(inkGrid);
    
    //Evaluate the model
    ModelOutput = await ModelGen.EvaluateAsync(ModelInput);
            
    //Iterate through evaluation output to determine highest probability digit
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
}
```

最后，我们要清除 InkCanvas，以便用户绘制另一个数字。

```csharp
private void clearButton_Click(object sender, RoutedEventArgs e)
{
    inkCanvas.InkPresenter.StrokeContainer.Clear();
    numberLabel.Text = "";
}
```

## <a name="7-launch-the-app"></a>7. 启动应用

生成和启动该应用后，我们将能够识别在 InkCanvas 上绘制的一个数字。

![完成应用](images/get-started4.png)

## <a name="8-next-steps"></a>8. 后续步骤

大功告成，你已开发出你的第一个 Windows ML 应用！ 有关更多展示如何使用 Windows ML 的示例，请查看[示例应用](samples.md)。