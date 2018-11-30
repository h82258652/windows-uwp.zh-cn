---
title: 设备门户远程输入 API 参考
description: 了解如何在 Xbox 上远程发送控制器、键盘和鼠标输入。
ms.localizationpriority: medium
ms.openlocfilehash: e0db86ad50bfb1cb27f516243542a554e710c3ea
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "8200512"
---
# <a name="remote-input-api-reference"></a>远程输入 API 参考   
可以通过此 API 远程实时发送控制器、键盘和鼠标输入。

**请求**

方法      | 请求 URI
:------     | :-----
Websocket | /ext/remoteinput
<br />
**URI 参数**

- 无

**请求标头**

- 无

**请求**

Websocket 是一系列字节数组消息。 每条消息格式如下。

第一个字节表示输入类型。 支持以下输入类型：

| 输入类型        | 字节值 |
|------------|-------------|
Keyboard KeyCodes | 0x01
Keyboard ScanCodes | 0x02
鼠标 | 0x03
全部清除 | 0x04

对于 KeyboardKeyCodes 和 KeyboardScanCodes，第二个字节是键盘代码或扫描代码的值，第三个字节如果是 0x01 则表示按下，如果是 0x00 则表示松开。

对于鼠标消息，下一个值为网络顺序下的 UINT16（2 字节），指示鼠标事件的类型：

| 操作类型        | UINT16 值 |
|------------|-------------|
移动 | 0x0001
LeftDown | 0x0002
LeftUp | 0x0004
RightDown | 0x0008
RightUp | 0x0010
MiddleDown | 0x0020
MiddleUp | 0x0040
X1Down | 0x0080
X1Up | 0x0100
X2Down | 0x0200
X2Up | 0x0400
VerticalWheelMoved | 0x0800
HorizontalWheelMoved | 0x1000

此字节后跟两个网络顺序下的 UINT32 值，以及一个表示滚轮操作的可选 UINT32。 前两个值是 X 和 Y 坐标，第三个值是滚轮增量。 X 和 Y 坐标应为介于 0 和 65535 之间的值，表示鼠标相对于水平及垂直平面的位置。

ClearAll 表示应松开当前按下的所有键。 不应使用任何其他字节。

若要发送游戏板输入，可将表示游戏版按钮按下操作的键盘代码值与 KeyboardKeyCodes 结合使用。 这些值如下所示：

| 游戏板类型        | 字节值 |
|------------|-------------|
VK_GAMEPAD_A                       |  0xC3
VK_GAMEPAD_B                       |  0xC4
VK_GAMEPAD_X                       |  0xC5
VK_GAMEPAD_Y                       |  0xC6
VK_GAMEPAD_RIGHT_SHOULDER          |  0xC7
VK_GAMEPAD_LEFT_SHOULDER           |  0xC8
VK_GAMEPAD_LEFT_TRIGGER            |  0xC9
VK_GAMEPAD_RIGHT_TRIGGER           |  0xCA
VK_GAMEPAD_DPAD_UP                 |  0xCB
VK_GAMEPAD_DPAD_DOWN               |  0xCC
VK_GAMEPAD_DPAD_LEFT               |  0xCD
VK_GAMEPAD_DPAD_RIGHT              |  0xCE
VK_GAMEPAD_MENU                    |  0xCF
VK_GAMEPAD_VIEW                    |  0xD0
VK_GAMEPAD_LEFT_THUMBSTICK_BUTTON  |  0xD1
VK_GAMEPAD_RIGHT_THUMBSTICK_BUTTON |  0xD2
VK_GAMEPAD_LEFT_THUMBSTICK_UP      |  0xD3
VK_GAMEPAD_LEFT_THUMBSTICK_DOWN    |  0xD4
VK_GAMEPAD_LEFT_THUMBSTICK_RIGHT   |  0xD5
VK_GAMEPAD_LEFT_THUMBSTICK_LEFT    |  0xD6
VK_GAMEPAD_RIGHT_THUMBSTICK_UP     |  0xD7
VK_GAMEPAD_RIGHT_THUMBSTICK_DOWN   |  0xD8
VK_GAMEPAD_RIGHT_THUMBSTICK_RIGHT  |  0xD9
VK_GAMEPAD_RIGHT_THUMBSTICK_LEFT   |  0xDA


**响应**   

- 无

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 请求已成功
4XX | 错误代码
5XX | 错误代码

<br />
**可用设备系列**

* Windows Xbox