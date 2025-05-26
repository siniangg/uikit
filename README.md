---
description: 为您的 iOS、iPadOS 或 Apple tvOS App 构建和管理图形化、事件驱动的用户界面。
---

# 🌲 UIKit

&#x20;iOS 版 2.0+ | iPadOS 2.0+ | Mac Catalyst 13.0+ | tvOS 9.0+ | visionOS 1.0+作系统 | watchOS 2.0+作系统

### [概述](./#overview) <a href="#overview" id="overview"></a>

UIKit 提供了多种用于构建 App 的功能，包括可用于构建 iOS、iPadOS 或 Apple tvOS App 核心基础架构的组件。该框架提供用于实现 UI 的窗口和视图架构、用于向应用程序提供多触点和其他类型的输入的事件处理基础设施，以及用于管理用户、系统和应用程序之间交互的主运行循环。

<figure><img src="https://docs-assets.developer.apple.com/published/565389bad84bdfaa6976f12e08b39c5b/media-4087315~dark%402x.png" alt=""><figcaption></figcaption></figure>

UIKit 还包括对动画、文档、绘图和打印、文本管理和显示、搜索、应用程序扩展、资源管理以及获取有关当前设备的信息的支持。您还可以自定义辅助功能支持，并针对不同的语言、国家/地区或文化区域本地化应用程序的界面。

UIKit 可与 [SwiftUI](https://developer.apple.com/documentation/SwiftUI?language=objc) 框架无缝协作，因此您可以在 SwiftUI 中实现 UIKit App 的各个部分，或在两个框架之间混合界面元素。例如，您可以将 UIKit 视图和视图控制器放在 SwiftUI 视图中，反之亦然。

要构建 macOS App，您可以使用 [SwiftUI](https://developer.apple.com/documentation/SwiftUI?language=objc) 创建适用于所有 Apple 平台的 App，或使用 [AppKit](https://developer.apple.com/documentation/AppKit?language=objc) 创建仅适用于 Mac 的 App。或者，您也可以使用 [Mac Catalyst](https://developer.apple.com/documentation/uikit/mac-catalyst?language=objc) 将 UIKit iPad 应用程序带到 Mac。

{% hint style="warning" %}
**重要**

请仅使用应用的主线程或主调度队列中的 UIKit 类，除非这些类的文档中另有说明。此限制尤其适用于从 [`UIResponder`](https://developer.apple.com/documentation/uikit/uiresponder?language=objc) 派生的类或涉及以任何方式作应用程序的用户界面的类。
{% endhint %}

## [主题](./#topics) <a href="#topics" id="topics"></a>

### [<mark style="color:red;">基本会话</mark>](https://developer.apple.com/documentation/uikit?language=objc#Essentials) <a href="#topics" id="topics"></a>

[UIKit 更新](https://developer.apple.com/documentation/Updates/UIKit?language=objc)

* 了解 UIKit 的重要更改。

[关于使用 UIKit 进行应用程序开发](https://developer.apple.com/documentation/uikit/about-app-development-with-uikit?language=objc)

* 了解 UIKit 和 Xcode 为您的 iOS 和 Apple tvOS 应用程序提供的基本支持。

[保护用户的隐私](https://developer.apple.com/documentation/uikit/protecting-the-user-s-privacy?language=objc)

* 保护个人数据，并尊重用户对数据使用方式的偏好。

### [<mark style="color:red;">应用结构</mark>](https://developer.apple.com/documentation/uikit?language=objc#App-structure) <a href="#app-structure" id="app-structure"></a>

UIKit 管理您的应用与系统的交互，并提供类供您管理应用的数据和资源。

[应用程序和环境](https://developer.apple.com/documentation/uikit/app-and-environment?language=objc)

* 管理生命周期事件和应用程序的 UI 场景，并获取有关特征和应用程序运行环境的信息。

[文档、数据和粘贴板](https://developer.apple.com/documentation/uikit/documents-data-and-pasteboard?language=objc)

* 组织应用程序的数据并在粘贴板上共享该数据。

[资源管理](https://developer.apple.com/documentation/uikit/resource-management?language=objc)

* 管理用于实现应用程序界面的图像、字符串、情节提要和 nib 文件。

[应用扩展](https://developer.apple.com/documentation/uikit/app-extensions?language=objc)

* 将应用的基本功能扩展到系统的其他部分。

[进程间通信](https://developer.apple.com/documentation/uikit/interprocess-communication?language=objc)

* 向人员显示基于活动的服务。

[Mac 催化剂](https://developer.apple.com/documentation/uikit/mac-catalyst?language=objc)

* 创建用户可以在 Mac 设备上运行的 iPad 应用程序版本。

### [<mark style="color:red;">用户界面</mark>](https://developer.apple.com/documentation/uikit?language=objc#User-interface) <a href="#user-interface" id="user-interface"></a>

视图可帮助您在屏幕上显示内容并促进用户交互;视图控制器可帮助您管理视图和界面的结构。

[视图和控件](https://developer.apple.com/documentation/uikit/views-and-controls?language=objc)

* 在屏幕上显示您的内容并定义允许与该内容进行的交互。

[视图控制器](https://developer.apple.com/documentation/uikit/view-controllers?language=objc)

* 使用视图控制器管理您的界面，并促进在应用程序内容中的导航。

[视图布局](https://developer.apple.com/documentation/uikit/view-layout?language=objc)

* 使用堆栈视图自动布置界面的视图。当您需要精确放置视图时，请使用 Auto Layout。

[外观定制](https://developer.apple.com/documentation/uikit/appearance-customization?language=objc)

* 为您的应用程序添加深色模式支持，自定义条形的外观，并使用外观代理修改您的 UI。

[动画和触觉反馈](https://developer.apple.com/documentation/uikit/animation-and-haptics?language=objc)

* 使用基于视图的动画和触觉向用户提供反馈。

[窗户和屏幕](https://developer.apple.com/documentation/uikit/windows-and-screens?language=objc)

* 为您的视图层次结构和其他内容提供容器。

### [<mark style="color:red;">用户交互</mark>](https://developer.apple.com/documentation/uikit?language=objc#User-interactions) <a href="#user-interactions" id="user-interactions"></a>

响应程序和手势识别器可帮助您处理触摸和其他事件。拖放、聚焦、速览和弹出以及辅助功能处理其他用户交互。

[触摸、按下和手势](https://developer.apple.com/documentation/uikit/touches-presses-and-gestures?language=objc)

* 将应用的事件处理逻辑封装在手势识别器中，以便您可以在整个应用中重复使用该代码。

[菜单和快捷方式](https://developer.apple.com/documentation/uikit/menus-and-shortcuts?language=objc)

* 使用菜单系统、上下文菜单、主屏幕快速作和键盘快捷键简化与 App 的交互。

[拖放](https://developer.apple.com/documentation/uikit/drag-and-drop?language=objc)

* 通过将交互 API 与视图结合使用，将拖放功能引入您的应用。

[指针交互](https://developer.apple.com/documentation/uikit/pointer-interactions?language=objc)

* 支持自定义控件和视图中的指针交互。

[Apple Pencil 交互](https://developer.apple.com/documentation/uikit/apple-pencil-interactions?language=objc)

* 处理用户交互，例如在 Apple Pencil 上双击和握压。

[基于焦点的导航](https://developer.apple.com/documentation/uikit/focus-based-navigation?language=objc)

* 使用遥控器、游戏控制器或键盘浏览 UIKit 应用程序的界面。

[UIKit 的辅助功能](https://developer.apple.com/documentation/uikit/accessibility-for-uikit?language=objc)

* 让 iOS 和 tvOS 的每个人都可以访问您的 UIKit 应用程序。

### [<mark style="color:red;">图形、绘图和打印</mark>](https://developer.apple.com/documentation/uikit?language=objc#Graphics-drawing-and-printing) <a href="#graphics-drawing-and-printing" id="graphics-drawing-and-printing"></a>

UIKit 提供了可帮助您配置绘制环境和呈现内容的类和协议。

[图片和 PDF](https://developer.apple.com/documentation/uikit/images-and-pdf?language=objc)

* 创建和管理图像，包括使用位图和 PDF 格式的图像。

[绘图](https://developer.apple.com/documentation/uikit/drawing?language=objc)

* 使用颜色、渲染器、绘制路径、字符串和阴影配置应用的绘制环境。

[印刷](https://developer.apple.com/documentation/uikit/printing?language=objc)

* 显示系统打印面板并管理打印过程。
