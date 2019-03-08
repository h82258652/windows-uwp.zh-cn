---
title: 使用连接存储缓冲区
description: 了解如何使用连接存储缓冲区。
ms.assetid: 1d9d1b52-4bfe-4cd9-8b80-50ca6c0e9ae1
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 连接存储
ms.localizationpriority: medium
ms.openlocfilehash: 3df95e4807e8d3457143e67eebfb62011bf365cc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595022"
---
# <a name="working-with-connected-storage-buffers"></a>使用连接存储缓冲区

连接存储 API 使用 **Windows::Storage::Streams::Buffer** 实例从应用程序传入/传出数据。 由于 WinRT 类型无法公开原始指针，因此，对缓冲区实例的数据访问将通过 **DataReader** 和 **DataWriter** 类发生。 但是，**缓冲区**还会实现 COM 接口 **IBufferByteAccess**，这样就可以获取直接指向缓冲区数据的指针。

### <a name="to-get-a-pointer-to-a-buffer-instances-data"></a>要获取指向缓冲区实例数据的指针

1.  使用**重新解释\_强制转换**若要强制转换为的缓冲区实例**IUnknown**。

```cpp
        IUnknown* unknown = reinterpret_cast<IUnknown*>(buffer);
```

2.  查询 **IBufferByteAccess** COM 接口的 **IUnknown** 接口。

```cpp
        Microsoft::WRL::ComPtr<IBufferByteAccess> bufferByteAccess;
        HRESULT hr = unknown->QueryInterface(_uuidof(IBufferByteAccess), &bufferByteAccess);
        if (FAILED(hr))
        return nullptr;
```

3.  使用 **IBufferByteAccess::Buffer** 获取直接指向缓冲区数据的指针。

```cpp
        byte* bytes = nullptr;
        bufferByteAccess->Buffer(&bytes);
```

例如，以下代码示例显示了如何创建保留当前系统时间的缓冲区。 由于缓冲区有独立的容量和长度值，因此，需要显式设置容量和长度。 默认情况下，长度为 0。

```cpp
    inline byte* GetBufferData(Windows::Storage::Streams::IBuffer^ buffer)
    {
        using namespace Windows::Storage::Streams;
        IUnknown* unknown = reinterpret_cast<IUnknown*>(buffer);
        Microsoft::WRL::ComPtr<IBufferByteAccess> bufferByteAccess;
        HRESULT hr = unknown->QueryInterface(_uuidof(IBufferByteAccess), &bufferByteAccess);
        if (FAILED(hr))
        return nullptr;
        byte* bytes = nullptr;
        bufferByteAccess->Buffer(&bytes);
        return bytes;
    }

    IBuffer^ WrapRawBuffer( void* ptr, size_t size )
    {
        using namespace Windows::Storage::Streams;

        //uint32 size = sizeof(FILETIME);
        Buffer^ buffer = ref new Buffer(size);
        buffer->Length = size;

        memcpy(GetBufferData(buffer),ptr,size);


        return buffer;

    };
```
