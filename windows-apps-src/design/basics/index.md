---
description: 了解如何设计和编码在各种设备和屏幕大小上易于导航且外观优美的 UWP 应用。
title: 设计基础知识
author: mijacobs
keywords: uwp 应用布局, 通用 windows 平台, 应用设计, 界面
ms.author: mijacobs
ms.date: 3/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: d092a3fe1120dc5763b5c30ed834c1902a1f8752
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842374"
---
# <a name="design-basics-for-uwp-apps"></a>UWP 应用的设计基础知识

![主图](images/header-design-basics.svg)

通用 Windows 平台 (UWP) 设计指南可帮助你设计和构建美观、优化的应用。 它不是一份说明性规则的清单 - 它是一个动态文档，旨在适应我们不断演变的 Fluent Design 系统以及应用构建社区的需求。 

<!-- This introduction provides an overview of the Fluent Design System, UWP app design basics, and the XAML platform, helping you build user interfaces (UI) that scale beautifully across a range of devices. -->

## <a name="overview"></a>概述

[**UWP 应用设计简介**](design-and-ui-intro.md)

介绍结合了最佳实践的 UWP 功能，用于创建在所有类型的支持 Windows 的设备上都表现出色的应用。

[**Fluent Design 系统**](../fluent-design-system/index.md)

Fluent Design 系统展示了我们创建自适应、共鸣和美观用户界面的目标和原则。

## <a name="basics"></a>入门

[**导航基础知识**](navigation-basics.md)

UWP 应用中的导航基于导航结构、导航元素和系统级功能的灵活模型。 本文将向你介绍这些组件，并向你展示如何结合使用它们来创建良好的导航体验。

[**命令基础知识**](commanding-basics.md)

命令元素是使用户能够执行发送电子邮件、删除项或提交表单等操作的交互式 UI 元素。 本文将介绍命令元素（如按钮和复选框）、它们支持的交互以及承载它们的命令图面（如命令栏和上下文菜单）。

[**内容基础知识**](content-basics.md)

任何应用的主要用途都是提供对内容的访问权限：在照片编辑应用中，照片即是内容；在旅行应用中，地图和旅行目的地信息即是内容；等等。 本文提供了适用于三种内容方案的内容设计建议：使用、创建和交互。

## <a name="tutorials"></a>教程

了解如何在 XAML 和 C# 中创建基本的照片编辑应用程序。
<!-- <img src="images/landing-page/photolab-50.png" style="{height: 339px}" alt=" " /> -->

[**1. 创建基本 UI**](xaml-basics-ui.md)

使用 XAML 创建基本用户界面。

[**2. 创建自适应布局**](xaml-basics-adaptive-layout.md)

为照片编辑应用程序提供自适应布局。

[**3. 创建自定义样式**](xaml-basics-style.md)

通过创建自定义样式为我们的 UWP 控件提供你自己的外观。