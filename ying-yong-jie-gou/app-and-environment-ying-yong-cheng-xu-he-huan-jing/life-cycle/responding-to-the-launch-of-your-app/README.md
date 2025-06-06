---
description: 初始化应用的数据结构，准备应用运行，并响应系统的任何启动时请求。
icon: list-ul
---

# Responding to the launch of your app

## Overview <a href="#overview" id="overview"></a>

用户点击主屏幕上的应用图标时，系统会启动您的应用。如果您的应用请求了特定事件，系统也可能在后台启动您的应用以处理这些事件。

所有应用都有一个关联的进程和至少一个场景，这些由 [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication?language=objc) 和 [`UIScene`](https://developer.apple.com/documentation/uikit/uiscene?language=objc) 对象表示。应用还包含一个应用委托对象（遵循 [`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate?language=objc) 协议的对象），用于响应该进程内发生的重要事件。即使实现了场景生命周期的应用，也会使用应用委托来管理启动和终止等基础事件。启动时，UIKit 会自动创建 [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication?language=objc) 对象和应用委托，然后在连接到应用的一个或多个场景之前，启动应用的主事件循环。

## **提供启动故事板**

当用户在设备上首次启动您的应用时，系统会显示您的启动故事板，直到应用准备好显示其用户界面。显示启动故事板可向用户保证应用已启动并正在执行操作。如果应用自行初始化并快速准备好用户界面，用户可能只会短暂看到启动故事板。

Xcode 项目会自动包含一个默认启动故事板供您自定义，您也可以根据需要添加更多启动故事板。要将新的启动故事板添加到项目中，请执行以下操作：

1. Open your project in Xcode.
2. Choose File > New > File.
3. Add a Launch Screen resource to your project.

在启动故事板中添加视图，并使用自动布局约束设置其尺寸和位置，使其适应基础环境。UIKit 会严格按照您提供的内容进行显示，并使用您的约束将视图适配到可用空间中。有关设计指导，请参阅《人机界面指南》（[Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/launching/)）。

{% hint style="warning" %}
**Important**

不要为启动屏幕使用静态图片。在 iOS 14 及更高版本中，启动屏幕的大小限制为 25 MB。
{% endhint %}
