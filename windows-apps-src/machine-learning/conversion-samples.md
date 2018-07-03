---
author: wschin
title: 将现有 ML 模型转换为 ONNX
description: 代码示例演示如何使用 WinMLTools 将 scikit-learn 和 Core ML 中的现有模型转换为 ONNX 格式。
ms.author: sezhen
ms.date: 3/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, windows 机器学习, WinML, WinMLTools, ONNX, ONNXMLTools, scikit-learn, Core ML
ms.localizationpriority: medium
ms.openlocfilehash: 7b2e9c8b661ccd2b0358882992da6c4f160b49f0
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842997"
---
# <a name="convert-existing-ml-models-to-onnx"></a>将现有 ML 模型转换为 ONNX

[WinMLTools](https://pypi.org/project/winmltools/) 可用来将在其他框架中训练的模型转换为 ONNX 格式。 下面我们演示如何安装 WinMLTools 程序包，以及如何通过 Python 代码将 scikit-learn 和 Core ML 中的现有模型转换为 ONNX。

## <a name="install-winmltools"></a>安装 WinMLTools

WinMLTools 是一个支持 Python 版本 2.7 和 3.6 的 Python 程序包 (winmltools)。 如果你在开展数据科学项目，我们建议你安装 Python 科学发行版，如 Anaconda。

WinMLTools 遵循[标准 python 包安装过程](https://packaging.python.org/installing/)。 从你的 python 环境中运行：

```
pip install winmltools
```

WinMLTools 要依赖以下几项才能运行：

- numpy v1.10.0+
- onnxmltools 0.1.0.0+
- Protobuf v.3.1.0+

若要更新相关的程序包，请带 -U 参数运行 pip 命令。

```
pip install -U winmltools
```

请访问 GitHub 上的 [onnxmltools](https://github.com/onnx/onnxmltools) 了解有关 onnxmltools 依赖项的更多信息。

在有关帮助函数的程序包特定文档中找不到有关如何使用 WinMLTools 的更多详细信息。

```
help(winmltools)
```

> [!NOTE]
> 借助 Visual Studio Tools for AI 扩展，还可以在 Visual Studio IDE 中使用 WinMLTools，获得更友好的点击式体验，将模型转换为 ONNX 格式。 要了解详细信息，请访问 [VS Tools for AI](https://github.com/Microsoft/vs-tools-for-ai/)。

## <a name="scikit-learn-models"></a>Scikit-learn 模型

下面的代码段在 scikit-learn 中训练线性支持向量机，并将该模型转换为 ONNX。

~~~python
# First, we create a toy data set.
# The training matrix X contains three examples, with two features each.
# The label vector, y, stores the labels of all examples.
X = [[0.5, 1.], [-1., -1.5], [0., -2.]]
y = [1, -1, -1]

# Then, we create a linear classifier and train it.
from sklearn.svm import LinearSVC
linear_svc = LinearSVC()
linear_svc.fit(X, y)

# To convert scikit-learn models, we need to specify the input feature's name and type for our converter. 
# The following line means we have a 2-D float feature vector, and its name is "input" in ONNX.
# The automatic code generator (mlgen) uses the name parameter to generate class names.
from winmltools import convert_sklearn
linear_svc_onnx = convert_sklearn(linear_svc, name='LinearSVC', input_features=[('input', 'float', 2)])    

# Now, we save the ONNX model into binary format.
from winmltools.utils import save_model
save_model(linear_svc_onnx, 'linear_svc.onnx')

# If you'd like to load an ONNX binary file, our tool can also help.
from winmltools.utils import load_model
linear_svc_onnx = load_model('linear_svc.onnx')

# To see the produced ONNX model, we can print its contents or save it in text format.
print(linear_svc_onnx)
from winmltools.utils import save_text
save_text(linear_svc_onnx, 'linear_svc.txt')

# The conversion of linear regression is very similar. See the example below.
from sklearn.svm import LinearSVR
linear_svr = LinearSVR()
linear_svr.fit(X, y)
linear_svr_onnx = convert_sklearn(linear_svr, name='LinearSVR', input_features=[('input', 'float', 2)])   
~~~

用户可将 `LinearSVC` 替换为其他 scikit-learn 模型，如 `RandomForestClassifier`。 请注意，[自动代码生成器](overview.md#automatic-interface-code-generation) 使用 `name` 参数来生成类名称和变量。 如果未提供 `name`，则会生成一个 GUID，它将不符合 C++/C# 等语言的变量命名约定。 

## <a name="scikit-learn-pipelines"></a>Scikit-learn 管道

接下来，我们将介绍 scikit-learn 管道可如何转换为 ONNX。

~~~python
# First, we create a toy data set.
# Notice that the first example's last feature value, 300, is large.
X = [[0.5, 1., 300.], [-1., -1.5, -4.], [0., -2., -1.]]
y = [1, -1, -1]

# Then, we declare a linear classifier.
from sklearn.svm import LinearSVC
linear_svc = LinearSVC()

# One common trick to improve a linear model's performance is to normalize the input data.
from sklearn.preprocessing import Normalizer
normalizer = Normalizer()

# Here, we compose our scikit-learn pipeline. 
# First, we apply our normalization. 
# Then we feed the normalized data into the linear model.
from sklearn.pipeline import make_pipeline
pipeline = make_pipeline(normalizer, linear_svc)
pipeline.fit(X, y)

# Now, we convert the scikit-learn pipeline into ONNX format. 
# Compared to the previous example, notice that the specified feature dimension becomes 3.
# The automatic code generator (mlgen) uses the name parameter to generate class names.
from winmltools import convert_sklearn
pipeline_onnx = convert_sklearn(linear_svc, name='NormalizerLinearSVC', input_features=[('input', 'float', 3)])   

# We can print the fresh ONNX model.
print(pipeline_onnx)

# We can also save the ONNX model into binary file for later use.
from winmltools.utils import save_model
save_model(pipeline_onnx, 'pipeline.onnx')

# Our conversion framework provides limited support of heterogeneous feature values.
# We cannot have numerical types and string type in one feature vector. 
# Let's assume that the first two features are floats and the last feature is integer (encoded a categorical attribute).
X_heter = [[0.5, 1., 300], [-1., -1.5, 400], [0., -2., 100]]

# One popular way to represent categorical is one-hot encoding.
from sklearn.preprocessing import OneHotEncoder
one_hot_encoder = OneHotEncoder(categorical_features=[2])

# Let's initialize a classifier. 
# It will be right after the one-hot encoder in our pipeline.
linear_svc = LinearSVC()

# Then, we form a two-stage pipeline.
another_pipeline = make_pipeline(one_hot_encoder, linear_svc)
another_pipeline.fit(X_heter, y)

# Now, we convert, save, and load the converted model. 
# For heterogeneous feature vectors, we need to seperately specify their types for all homogeneous segments.
# The automatic code generator (mlgen) uses the name parameter to generate class names.
another_pipeline_onnx = convert_sklearn(another_pipeline, name='OneHotLinearSVC', input_features=[('input', 'float', 2), ('another_input', 'int64', 1)])
save_model(another_pipeline_onnx, 'another_pipeline.onnx')
from winmltools.utils import load_model
loaded_onnx_model = load_model('another_pipeline.onnx')

# Of course, we can print the ONNX model to see if anything went wrong.
print(another_pipeline_onnx)
~~~

## <a name="core-ml-models"></a>Core ML 模型

下面，我们假设一个示例 Core ML 模型文件的路径为 *example.mlmodel*。

~~~python
from coremltools.models.utils import load_spec
# Load model file
model_coreml = load_spec('example.mlmodel')
from winmltools import convert_coreml
# Convert it!
# The automatic code generator (mlgen) uses the name parameter to generate class names.
model_onnx = convert_coreml(model_coreml, name='ExampleModel')   
~~~

`model_onnx` 是一个 ONNX [ModelProto](https://github.com/onnx/onnxmltools/blob/0f453c3f375c1ae928b83a4c7909c82c013a5bff/onnxmltools/proto/onnx-ml.proto#L176) 对象。 我们可以用两种不同的格式保存它。

~~~python
from winmltools.utils import save_model
# Save the produced ONNX model in binary format
save_model(model_onnx, 'example.onnx')
# Save the produced ONNX model in text format
from winmltools.utils import save_text
save_text(model_onnx, 'example.txt')
~~~

**注意**: CoreMLTools 是 Apple 提供的一个 Python 包，但在 Windows 上不可用。 如果你需要在 Windows 上安装该包，可直接从资源库安装：

```
pip install git+https://github.com/apple/coremltools
```

## <a name="core-ml-models-with-image-inputs-or-outputs"></a>具有图像输入或输出的 Core ML 模型

由于在 ONNX 中缺乏图像类型，转换 Core ML 图像模型（即使用图像作为输入或输出的模型）需要执行一些预处理和后处理步骤。

预处理的目标是确保输入图像格式正确，可作为 ONNX 张量。 假设 *X* 是 Core ML 中具有形状 [C, H, W] 的图像输入。 在 ONNX 中，变量 *X* 会是具有相同形状的浮点张量，*X*[0, :, :]/*X*[1, :, :]/*X*[2, :, :] 存储图像的红/绿/蓝通道。 对 Core ML 中的灰度图像，其格式是 [1，H，W] - 在 ONNX 中的张量，因为我们只有一个通道。

如果原始 Core ML 模型输出一个图像，请手动将 ONNX 的浮点输出张量重新转换为图像。 以下是两个基本步骤。 第一步是将超过 255 的值截断为 255，并将所有负值更改为 0。 第 2 步是将所有像素值取整（先增加 0.5，然后去掉小数部分）。 输出通道顺序（例如，RGB 或 BGR）在 Core ML 模型中指出。 若要生成正确的图像输出，我们可能需要反转或打乱以恢复所需的格式。

我们将从 [GitHub](https://github.com/likedan/Awesome-CoreML-Models) 下载的 Core ML 模型 FNS-Candy 视为演示 ONNX 和 Core ML 之间区别的一个具体转换示例。 请注意，所有后续的命令都在 python 环境中执行。

首先，我们加载 Core ML 模型，然后检查其输入和输出格式。

~~~python
from coremltools.models.utils import load_spec
model_coreml = load_spec('FNS-Candy.mlmodel')
model_coreml.description # Print the content of Core ML model description
~~~

屏幕输出：

~~~
...
input {
    ...
      imageType {
      width: 720
      height: 720
      colorSpace: BGR
    ...
}
...
output {
    ...
      imageType {
      width: 720
      height: 720
      colorSpace: BGR
    ...
}
...
~~~

在本例中，输入和输出都是 720 x 720 BGR 图像。 接下来我们要使用 WinMLTools 转换该 Core ML 模型。

~~~python
# The automatic code generator (mlgen) uses the name parameter to generate class names.
from onnxmltools import convert_coreml
model_onnx = convert_coreml(model_coreml, name='FNSCandy')    
~~~

在 ONNX 中查看模型输入和输出格式的另一种方法是使用以下命令：

~~~python
model_onnx.graph.input # Print out the ONNX input tensor's information
~~~

屏幕输出：

~~~
...
  tensor_type {
    elem_type: FLOAT
    shape {
      dim {
        dim_param: "None"
      }
      dim {
        dim_value: 3
      }
      dim {
        dim_value: 720
      }
      dim {
        dim_value: 720
      }
    }
  }
...
~~~

在 ONNX 中生成的输入（由 *X* 指定） 是一个 4D 张量。 后 3 个轴分别是 C、H 和 W 轴。 第一个维是“无”，这就是说此 ONNX 模型可应用于任意数量的图像。 要应用此模型来处理 2 个图像，第一幅图像对应于 *X*[0, :, :, :] ，而第二个图像对应于 *X*[1:，:，:]。 第一个图像的蓝/绿/红通道是 *X*[0, 0, :, :]/*X*[0, 1, :, :]/*X*[0, 2, :, :]，第二个图像与此类似。

~~~python
model_onnx.graph.output # Print out the ONNX output tensor's information
~~~

屏幕输出：

~~~
...
  tensor_type {
    elem_type: FLOAT
    shape {
      dim {
        dim_param: "None"
      }
      dim {
        dim_value: 3
      }
      dim {
        dim_value: 720
      }
      dim {
        dim_value: 720
      }
    }
  }
...
~~~

如你所见，所生成的格式与原始模型输入格式相同。 但是，在此例中，它不是图像，因为像素值是整数而不是浮点数。 若要重新转换为图像，请将大于 255 的值截断为 255，将负值更改为 0，再对所有小数取整，使之变为整数。