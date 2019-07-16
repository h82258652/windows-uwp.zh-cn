---
Description: 使用 utf-8 字符编码的 web 应用程序之间的最佳兼容性和其他 * nix 基于平台 （Unix、 Linux 和变体），最大程度减少本地化 bug 并降低开销测试。
title: 使用 Windows utf-8 代码页
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: windows 10, uwp, 全球化, 可本地化性, 本地化
ms.localizationpriority: medium
ms.openlocfilehash: a9386b31d16796c68d41a27ab48a5b2c9a9a342b
ms.sourcegitcommit: 734aa941dc675157c07bdeba5059cb76a5626b39
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68141808"
---
# <a name="use-the-utf-8-code-page"></a>使用 UTF-8 代码页

使用[utf-8](http://www.utf-8.com/)字符编码的 web 应用和其他之间的最佳兼容性 * nix 基于平台 （Unix、 Linux 和变体），最大程度减少本地化 bug 并降低开销测试。

Utf-8 是国际化的通用代码页，支持使用 1-4 字节宽度可变的编码的所有 Unicode 码位。 它在 web 上范围内得到广泛使用的默认值为 * nix 基于平台。

## <a name="-a-vs--w-apis"></a>-A-W Api 与
  
Win32 Api 通常支持同时-A 和-W 变体。

-一个变体识别系统和支持上配置的 ANSI 代码页`char*`，而在 utf-16 和支持操作-W 变体`WCHAR`。

直到最近，Windows 已转移-A Api 强调"Unicode"-W 变体。 但是，最新版本使用了 ANSI 代码页并且-A Api 作为一种方式引入 utf-8 支持添加到应用程序。 如果为 utf-8 配置的 ANSI 代码页，则-A Api 进行操作以 UTF – 8。 此模式具有支持使用-A Api 无需更改任何代码生成的现有代码的优点。

## <a name="set-a-process-code-page-to-utf-8"></a>将进程的代码页设置为 utf-8

从 Windows 版本 1903 （可能 2019年更新），可用于 ActiveCodePage 属性在 appxmanifest 中打包应用程序或未打包应用程序的合成清单来强制执行过程与过程代码页使用 utf-8。

您可以声明此属性和目标/运行早期 Windows 上生成，但您必须像往常一样处理旧代码页检测和转换。 使用最低目标版本的 Windows 版本 1903年，过程代码页将始终为 utf-8，因此可以避免旧代码页检测和转换。

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

因为 Windows 的本机同类 utf-16 (`WCHAR`)，可能需要转换为 utf-16 （反之亦然） 的 utf-8 数据与 Windows Api 进行互操作。

[MultiByteToWideChar](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar)并[WideCharToMultiByte](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) let utf-8 和 utf-16 之间转换 (`WCHAR`) （和其他代码页）。 这是特别有用旧版 Win32 API 可能仅能理解`WCHAR`。 这些函数允许您将转换到 utf-8 输入`WCHAR`若要将传递到-W API，然后将转换的任何结果，如有必要则返回。
使用与这些函数时`CodePage`设置为`CP_UTF8`，使用`dwFlags`其中任何`0`或`MB_ERR_INVALID_CHARS`，否则为`ERROR_INVALID_FLAGS`时发生。

注意：`CP_ACP`相当于`CP_UTF8`仅当在 Windows 版本 1903 （可能 2019年更新） 上运行或更高版本，并且前面所述的 ActiveCodePage 属性设置为 utf-8。 否则，它遵循的旧系统代码页。 我们建议使用`CP_UTF8`显式。

## <a name="related-topics"></a>相关主题

- [代码页](https://docs.microsoft.com/windows/desktop/Intl/code-pages)
- [代码页标识符](https://docs.microsoft.com/windows/desktop/Intl/code-page-identifiers)
