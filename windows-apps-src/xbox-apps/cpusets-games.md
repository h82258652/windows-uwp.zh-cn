---
title: "适用于游戏开发的 CPUSets"
description: "本文概述了通用 Windows 平台 (UWP) 新增的 CPUSets API，涵盖了对于游戏和应用程序开发相当重要的核心信息。"
author: hammondsp
ms.openlocfilehash: 6065435dc3add0d9bde15dc6bdd355935b8f53cd
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: HT
ms.contentlocale: zh-CN
---
# <a name="cpusets-for-game-development"></a>适用于游戏开发的 CPUSets

## <a name="introduction"></a>简介

通用 Windows 平台 (UWP) 是范围广泛的消费电子设备的核心。 因此，要求通用 API 满足从游戏到嵌入式应用再到服务器上运行的企业软件在内的所有应用程序类型的需求。 通过利用该 API 提供的正确信息，你可以确保你的游戏在任何硬件上都可以完美运行。

## <a name="cpusets-api"></a>CPUSets API

CPUSets API 控制可用于在其上调度线程的 CPU 集。 两个函数可用于控制调度线程的位置：
- **SetProcessDefaultCpuSets** – 如果新线程未分配给特定的 CPU 设置，可使用此函数指定新线程可在其上运行的 CPU 设置。
- **SetThreadSelectedCpuSets** – 此函数允许你限制特定线程可在其上运行的 CPU 设置。

如果从未使用过 **SetProcessDefaultCpuSets** 函数，则新创建的线程可以在适用于你的进程的任何 CPU 设置上调度。 此部分介绍 CPUSets API 的基础知识。

### <a name="getsystemcpusetinformation"></a>GetSystemCpuSetInformation

用于收集信息的第一个 API 是 **GetSystemCpuSetInformation** 函数。 此函数将信息填充于标题代码提供的 **SYSTEM_CPU_SET_INFORMATION** 对象数组中。 目标内存必须由游戏代码进行分配，而具体大小将通过调用 **GetSystemCpuSetInformation** 本身来确定。 这需要调用 **GetSystemCpuSetInformation** 两次，如以下示例中所示。

```
unsigned long size;
HANDLE curProc = GetCurrentProcess();
GetSystemCpuSetInformation(nullptr, 0, &size, curProc, 0);

std::unique_ptr<uint8_t[]> buffer(new uint8_t[size]);

PSYSTEM_CPU_SET_INFORMATION cpuSets = reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>(buffer.get());
  
GetSystemCpuSetInformation(cpuSets, size, &size, curProc, 0);
```

每个返回的 **SYSTEM_CPU_SET_INFORMATION** 实例包含有关一个唯一的处理单元（也称为“CPU 集”）的信息。 这并不一定意味着它表示硬件的独特物理部分。 利用超线程的 CPU 将具有在单个物理处理内核上运行的多个逻辑核心。 在不同逻辑核心（位于同一物理核心上）上调度多个线程允许执行硬件级别的资源优化，否则会以内核级别执行额外工作。 在同一物理核心的单独逻辑核心上调度的两个线程必须共享 CPU 时间，但相比于它们在同一逻辑核心上调度而言，可以更高效地运行。

### <a name="systemcpusetinformation"></a>SYSTEM_CPU_SET_INFORMATION

从 **GetSystemCpuSetInformation** 中返回的此数据结构的每个实例中的信息包含有关可以在其上调度的线程唯一处理单元的信息。 根据给定的可能目标设备范围，**SYSTEM_CPU_SET_INFORMATION** 数据结构中的许多信息可能不适用于游戏开发。 表 1 提供适用于游戏开发的数据成员的说明。

 **表 1. 适用于游戏开发的数据成员。**

| 成员名称  | 数据类型 | 提要 |
| ------------- | ------------- | ------------- |
| Type  | CPU_SET_INFORMATION_TYPE  | 结构中信息的类型。 如果此成员的值不是 **CpuSetInformation**，应忽略它。  |
| Id  | 无符号长整型  | 指定 CPU 设置的 ID。 这是应与 CPU 设置函数（如 **SetThreadSelectedCpuSets**）结合使用的 ID。  |
| Group  | 无符号短整型  | 指定 CPU 设置的“处理器组”。 处理器组允许电脑具有超过 64 个逻辑核心，并允许在系统运行期间热交换 CPU。 不是服务器但具有多个组的电脑并不常见。 除非你要编写的应用程序打算在大型服务器或服务器场上运行，否则最好使用单个组中的 CPU 设置，因为大多数消费者电脑只具有一个处理器组。 此结构中的其他所有值都与 Group 有关。  |
| LogicalProcessorIndex  | 无符号字符型  | CPU 设置的 Group 相关索引  |
| CoreIndex  | 无符号字符型  | CPU 设置所在的物理 CPU 核心的 Group 相关索引  |
| LastLevelCacheIndex  | 无符号字符型  | 与此 CPU 设置关联的最后一级缓存的 Group 相关索引。 此缓存的速度最慢，除非系统利用 NUMA 节点，通常为 L2 或 L3 缓存。  |

<br />

其他数据成员提供的信息不太可能描述消费者电脑或其他消费者设备中的 CPU，也不太可能非常有用。 然后返回的数据提供的信息可用于以多种方式组织线程。 此白皮书的[游戏开发注意事项](#considerations-for-game-development)部分详细介绍了利用此数据优化线程分配的多种方法。

以下是一些从各种类型硬件上运行的 UWP 应用程序中收集的信息类型的示例。

**表 2. 从 Microsoft Lumia 950 上运行的 UWP 应用中返回的信息。 这是一个具有多个最后一级缓存的系统示例。 Lumia 950 配备 Qualcomm 808 Snapdragon 处理器，该处理器包含双核 ARM Cortex A57 和四核 ARM Cortex A53 CPU。**

  ![表 2](images/cpusets-table2.png)

**表 3. 从典型的电脑上运行的 UWP 应用中返回的信息。 这是一个使用超线程的系统示例；每个物理核心具有两个可在其上调度线程的逻辑核心。 在此情况下，该系统包含了一个 Intel Xenon CPU E5-2620。**

  ![表 3](images/cpusets-table3.png)

**表 4. 从四核 Microsoft Surface Pro 4 上运行的 UWP 应用中返回的信息。 此系统已有一个 Intel Core i5-6300 CPU。**

  ![表 4](images/cpusets-table4.png)

### <a name="setthreadselectedcpusets"></a>SetThreadSelectedCpuSets

既然提供了 CPU 设置的相关信息，就可以使用该信息来组织线程。 将向此函数传递使用 **CreateThread** 创建的线程的句柄以及可在其上调度线程的 CPU 设置的 ID 数组。 使用以下代码演示函数使用情况的一个示例。

```
HANDLE audioHandle = CreateThread(nullptr, 0, AudioThread, nullptr, 0, nullptr);
unsigned long cores [] = { cpuSets[0].CpuSet.Id, cpuSets[1].CpuSet.Id };
SetThreadSelectedCpuSets(audioHandle, cores, 2);
```
在此示例中，线程基于声明为 **AudioThread** 的函数创建。 然后，允许在两个 CPU 设置之一上调度此线程。 CPU 设置的线程所有权不独占。 在未锁定到特定 CPU 设置的情况下，通过 **AudioThread** 创建线程可能需要一些时间。 同样，创建的其他线程稍后也可以锁定到这些 CPU 设置中的一个或两个。

### <a name="setprocessdefaultcpusets"></a>SetProcessDefaultCpuSets

与 **SetThreadSelectedCpuSets** 相反的是 **SetProcessDefaultCpuSets**。 创建线程时，不需要将它们锁定到特定 CPU 设置。 如果你不希望这些线程在特定 CPU 设置上运行（例如，呈现线程或音频线程使用的 CPU 设置），可以使用此函数指定允许在其上调度这些线程的核心。

## <a name="considerations-for-game-development"></a>游戏开发注意事项

正如我们所见，CPUSets API 涉及到调度线程时，它可提供大量信息和灵活性。 与采取自下而上的方法来尝试查找此数据的使用相比，采取自上而下的方法查找如何将数据用于适应常见方案会更有效。

### <a name="working-with-time-critical-threads-and-hyperthreading"></a>使用时间关键线程和超线程

如果你的游戏所具有的多个线程必须实时运行，而其他工作线程所需的 CPU 时间相对较少，则此方法非常有效。 某些任务（如连续背景音乐）必须不间断地运行才可以实现最佳游戏体验。 因此每帧接收必要数量的 CPU 时间至关重要，即使音频线程的单帧匮乏可能导致爆音或噪音干扰。

结合 **SetProcessDefaultCpuSets** 使用 **SetThreadSelectedCpuSets** 可以确保你的大量线程不会因任何工作线程而中断。 **SetThreadSelectedCpuSets** 可用于将你的大量线程分配给特定的 CPU 设置。 然后 **SetProcessDefaultCpuSets** 可用于确保任何已创建的未分配线程放置在其他 CPU 设置上。 即使 CPU 利用超线程，在同一物理核心上考虑使用逻辑核心也很重要。 不应允许工作线程在共享物理核心（与你想要以实时响应性运行的线程相同）的逻辑核心上运行。 以下代码演示如何确定电脑是否使用超线程。

```
unsigned long retsize = 0;
(void)GetSystemCpuSetInformation( nullptr, 0, &retsize,
    GetCurrentProcess(), 0);
 
std::unique_ptr<uint8_t[]> data( new uint8_t[retsize] );
if ( !GetSystemCpuSetInformation(
    reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>( data.get() ),
    retsize, &retsize, GetCurrentProcess(), 0) )
{
    // Error!
}
 
std::set<DWORD> cores;
std::vector<DWORD> processors;
uint8_t const * ptr = data.get();
for( DWORD size = 0; size < retsize; ) {
    auto info = reinterpret_cast<const SYSTEM_CPU_SET_INFORMATION*>( ptr );
    if ( info->Type == CpuSetInformation ) {
         processors.push_back( info->CpuSet.Id );
         cores.insert( info->CpuSet.CoreIndex );
    }
    ptr += info->Size;
    size += info->Size;
}
 
bool hyperthreaded = processors.size() != cores.size();
```

如果系统利用超线程，则默认 CPU 设置的设置在与任何实时线程的相同物理核心上不包含任何逻辑核心，这一点很重要。 如果系统不是超线程，只需确保默认 CPU 设置不包含与运行你的音频线程的 CPU 设置相同的核心即可。

可以在[其他资源](#additional-resources)部分链接的 GitHub 存储库上提供的 CPUSets 示例中找到基于物理核心组织线程的示例。

### <a name="reducing-the-cost-of-cache-coherence-with-last-level-cache"></a>减少最后一级缓存的缓存一致性的开销

缓存一致性的概念为缓存的内存在处理同一数据的多个硬件资源之间是相同的。 如果线程在不同核心上调度，但处理同一数据，则这些线程就可以处理不同缓存中该数据的单独副本。 为了获取正确的结果，这些缓存相互之间必须保持一致。 保持多个缓存之间的一致性相当耗费资源，但却是运行任何多核系统所必需的。 此外，它完全失去对客户端代码的控制；基础系统独立工作以通过访问核心之间共享的内存资源来保持缓存的最新状态。

如果你的游戏具有共享特别庞大的数据的多个线程，可以通过确保这些线程在共享最后一级缓存的 CPU 设置上调度来最大程度地减少缓存一致性的开销。 最后一级缓存是速度最慢的缓存，可用于不利用 NUMA 节点的系统上的核心。 对于利用 NUMA 节点的游戏电脑而言相当少见。 如果核心不共享最后一级缓存，保持一致性就需要访问更高级别的内存资源，因此速度较慢。 将两个线程锁定到共享一个缓存和一个物理核心的单独 CPU 设置可以提供更好的性能（相比于在单独的物理核心上调度它们，如果它们在任何给定的框架中不需要超过 50% 的时间）。 

此代码示例显示如何确定频繁通信的线程是否可以共享最后一级缓存。

```
unsigned long retsize = 0;
(void)GetSystemCpuSetInformation(nullptr, 0, &retsize,
    GetCurrentProcess(), 0);
 
std::unique_ptr<uint8_t[]> data(new uint8_t[retsize]);
if (!GetSystemCpuSetInformation(
    reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>(data.get()),
    retsize, &retsize, GetCurrentProcess(), 0))
{
    // Error!
}
 
unsigned long count = retsize / sizeof(SYSTEM_CPU_SET_INFORMATION);
bool sharedcache = false;
 
std::map<unsigned char, std::vector<SYSTEM_CPU_SET_INFORMATION>> cachemap;
for (size_t i = 0; i < count; ++i)
{
    auto cpuset = reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>(data.get())[i];
    if (cpuset.Type == CPU_SET_INFORMATION_TYPE::CpuSetInformation)
    {
        if (cachemap.find(cpuset.CpuSet.LastLevelCacheIndex) == cachemap.end())
        {
            std::pair<unsigned char, std::vector<SYSTEM_CPU_SET_INFORMATION>> newvalue;
            newvalue.first = cpuset.CpuSet.LastLevelCacheIndex;
            newvalue.second.push_back(cpuset);
            cachemap.insert(newvalue);
        }
        else
        {
            sharedcache = true;
            cachemap[cpuset.CpuSet.LastLevelCacheIndex].push_back(cpuset);
        }
    }
}
```

图 1 中所示的缓存布局是你可以从系统中看到的布局的类型示例。 此图是在 Microsoft Lumia 950 中找到的缓存示意图。 CPU 256 和 CPU 260 之间发生的线程间通信将导致大量开销，因为需要系统保持它们的 L2 缓存一致性。

**图 1. Microsoft Lumia 950 设备上找到的缓存体系结构。**

![Lumia 950 缓存](images/cpusets-lumia950cache.png)

## <a name="summary"></a>摘要

适用于 UWP 开发的 CPUSets API 提供了与你的多线程选项有关的大量信息和控制。 相比于以前的适用于 Windows 开发的多线程 API，增加的复杂性具有一些学习曲线，但提升的灵活性最终允许在一些消费者电脑和其他硬件目标之间实现较好的性能。

## <a name="additional-resources"></a>其他资源
- [CPU 设置 (MSDN)](https://msdn.microsoft.com/library/windows/desktop/mt186420(v=vs.85).aspx)
- [ATG 提供的 CPUSets 示例](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/CPUSets)
- [Xbox One 上的 UWP](index.md)

