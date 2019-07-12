---
Description: 使用 utf-8 字符编码的 web 应用程序之间的最佳兼容性和其他 * nix 基于平台 （Unix、 Linux 和变体），最大程度减少本地化 bug 并降低开销测试。
title: 使用 Windows utf-8 代码页
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: windows 10, uwp, 全球化, 可本地化性, 本地化
ms.localizationpriority: medium
ms.openlocfilehash: 453d58b0d52aaa24461784b6f393b26b93e572a1
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827373"
---
# <a name="use-the-utf-8-code-page"></a>使用 utf-8 代码页

使用[utf-8](http://www.utf-8.com/)字符编码的 web 应用和其他之间的最佳兼容性 * nix 基于平台 （Unix、 Linux 和变体），最大程度减少本地化 bug 并降低开销测试。

Utf-8 是国际化的通用代码页，支持使用 1-4 字节宽度可变的编码的所有 Unicode 码位。 它在 web 上范围内得到广泛使用的默认值为 * nix 基于平台。

## <a name="-a-vs--w-apis"></a>-A-W Api 与
  
Win32 Api 通常支持同时-A 和-W 变体。

-一个变体识别系统和支持 char * 上, 配置并且在 utf-16 和支持操作-W 变体的 ANSI 代码页`WCHAR`。

直到最近，Windows 已转移-A Api 强调"Unicode"-W 变体。 但是，最新版本使用了 ANSI 代码页并且-A Api 作为一种方式引入 utf-8 支持添加到应用程序。 如果为 utf-8 配置的 ANSI 代码页，则-A Api 进行操作以 UTF – 8。 此模式具有支持使用-A Api 无需更改任何代码生成的现有代码的优点。

## <a name="set-a-process-code-page-to-utf-8"></a>将进程的代码页设置为 utf-8

您可以强制执行过程与过程代码页的 appxmanifest 的打包应用程序或使用 ActiveCodePage 属性未打包应用程序的合成清单通过使用 utf-8。

您可以声明此属性和目标/运行早期 Windows 上生成，但您必须像往常一样处理旧代码页检测和转换 （19 小时 1 的最低目标版本，使用过程代码页将始终为 utf-8）。

## <a name="examples"></a>示例

**打包应用程序的 Appx 清单：**

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         ...
         xmlns:uap7="http://schemas.microsoft.com/appx/manifest/uap/windows10/7"
         xmlns:uap8="http://schemas.microsoft.com/appx/manifest/uap/windows10/8"
         ...
         IgnorableNamespaces="... uap7 uap8 ...">

  <Applications>
    <Application ...>
      <uap7:Properties>
        <uap8:ActiveCodePage>UTF-8</uap8:ActiveCodePage>
      </uap7:Properties>
    </Application>
  </Applications>
</Package>
```

**未打包的 Win32 应用程序的合成清单：**

``` xaml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
  <assemblyIdentity type="win32" name="..." version="6.0.0.0"/>
  <application>
    <windowsSettings>
      <activeCodePage xmlns="http://schemas.microsoft.com/SMI/2019/WindowsSettings">UTF-8</activeCodePage>
    </windowsSettings>
  </application>
</assembly>
```

> [!NOTE]
> 将一个清单添加到现有的可执行文件，从命令行中使用 `mt.exe -manifest <MANIFEST> -outputresource:<EXE>;#1`

## <a name="code-page-conversion"></a>代码页转换

因为 Windows 本机运行 utf-16 (WCHAR)，可能需要转换为 utf-16 （反之亦然） 的 utf-8 数据与 Windows Api 进行互操作。

[MultiByteToWideChar](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar)并[WideCharToMultiByte](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) let utf-8 和 utf-16 (WCHAR) （和其他代码页） 之间进行转换。 这在传统的 Win32 API 可能仅了解 WCHAR 时特别有用。 这些函数允许您将转换为 WCHAR 若要将传递到-W API，然后将转换的任何结果，如有必要则返回的 utf-8 输入。
使用时这些函数中使用 CP_UTF8 代码页的 Windows，使用为 0 或 MB_ERR_INVALID_CHARS，dwFlags ERROR_INVALID_FLAGS 否则会发生。

注意:仅当在 Windows 版本 1903 （可能 2019年更新） 上运行，CP_ACP 等同于 CP_UTF8 和上面所述的 ActiveCodePage 属性设置为 utf-8。 否则，它遵循的旧系统代码页。 我们建议显式使用 CP_UTF8。

## <a name="related-topics"></a>相关主题

- [代码页](https://docs.microsoft.com/windows/desktop/Intl/code-pages)
- [代码页标识符](https://docs.microsoft.com/windows/desktop/Intl/code-page-identifiers)
