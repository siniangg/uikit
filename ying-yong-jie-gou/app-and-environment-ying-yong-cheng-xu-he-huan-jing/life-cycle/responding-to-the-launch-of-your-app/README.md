---
description: 初始化应用的数据结构，准备应用运行，并响应系统的任何启动时请求。
icon: list-ul
---

# Responding to the launch of your app

## Overview <a href="#overview" id="overview"></a>

用户点击主屏幕上的应用图标时，系统会启动您的应用。如果您的应用请求了特定事件，系统也可能在后台启动您的应用以处理这些事件。

所有应用都有一个关联的进程和至少一个场景，这些由 [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication?language=objc) 和 [`UIScene`](https://developer.apple.com/documentation/uikit/uiscene?language=objc) 对象表示。应用还包含一个应用委托对象（遵循 [`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate?language=objc) 协议的对象），用于响应该进程内发生的重要事件。即使实现了场景生命周期的应用，也会使用应用委托来管理启动和终止等基础事件。启动时，UIKit 会自动创建 [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication?language=objc) 对象和应用委托，然后在连接到应用的一个或多个场景之前，启动应用的主事件循环。

### **提供启动故事板**

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

### 在页面链接中初始化应用程序的数据结构

将应用的启动时初始化代码放在以下一个或两个方法中：

* [`application:willFinishLaunchingWithOptions:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:willfinishlaunchingwithoptions:\)?language=objc)
* [`application:didFinishLaunchingWithOptions:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:didfinishlaunchingwithoptions:\)?language=objc)

UIKit会在应用启动周期开始时调用这些方法。使用它们来：

* 初始化应用跨场景使用的数据结构。
* 确认您的应用拥有运行所需的资源。
* 当您的应用首次启动时，执行任何一次性设置。例如，在可写入目录中安装模板或用户可修改的文件。 See [Performing one-time setup for your app](https://developer.apple.com/documentation/uikit/performing-one-time-setup-for-your-app?language=objc).
* 连接您的应用使用的任何关键服务。例如，如果您的应用支持远程通知，请连接到 Apple 推送通知服务。
* 检查启动选项字典，以获取有关应用启动原因的信息。 See [Determine why your app launched](https://developer.apple.com/documentation/uikit/responding-to-the-launch-of-your-app?language=objc#Determine-why-your-app-launched).

对于尚未采用场景生命周期的应用，UIKit 会在启动时自动加载默认用户界面。请使用 [`application:didFinishLaunchingWithOptions:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:didfinishlaunchingwithoptions:\)?language=objc)  方法，在界面显示到屏幕上之前对其进行额外更改。例如，您可以安装不同的视图控制器，以反映用户上次使用应用时所执行的操作。

### 将长时间运行的任务从主线程中移开

当用户启动您的应用时，通过快速启动给用户留下良好印象。在  [`application:didFinishLaunchingWithOptions:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:didfinishlaunchingwithoptions:\)?language=objc)、[`application:willFinishLaunchingWithOptions:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:willfinishlaunchingwithoptions:\)?language=objc) 或 [`scene:willConnectToSession:options:`](https://developer.apple.com/documentation/uikit/uiscenedelegate/scene\(_:willconnectto:options:\)?language=objc) 中执行长时间运行的任务，可能会让用户觉得您的应用反应迟缓。在启动到后台时，快速返回也很重要，因为系统会限制应用的后台执行时间。

将对应用初始化并非至关重要的任务移出启动时序列。例如：

* 推迟初始化应用并非立即需要的功能。
* 将重要的长时间运行任务从应用的主线程中移出。例如，在全局调度队列或任务中异步运行它们。[`Task`](https://developer.apple.com/documentation/Swift/Task?language=objc)

### 确定您的场景连接的原因

当UIKit连接到您应用程序中的一个场景时，它会传递一个[`UISceneConnectionOptions`](https://developer.apple.com/documentation/uikit/uiscene/connectionoptions?language=objc)对象，该对象包含有关UIKit连接到该场景原因的信息。例如，这可能表明用户请求您的应用程序打开一个 [`URL`](https://developer.apple.com/documentation/Foundation/URL?language=objc)，您可以使用该URL来显示与URL中提供的信息相关的屏幕。

以下代码展示了如何检查场景连接选项中包含查询项信息的URL：

{% code lineNumbers="true" fullWidth="false" %}
```objectivec
class SceneDelegate: UIResponder, UIWindowSceneDelegate {
    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        
        // Confirm the scene is a window scene in iOS or iPadOS. 
        guard let _ = (scene as? UIWindowScene) else { return }
        
        // Check if the scene connection options include a URL,
        // and whether the URL has query items.
        guard let linkedUrl = connectionOptions.urlContexts.first?.url,
              let linkedComponents = URLComponents(url: linkedUrl, resolvingAgainstBaseURL: false),
              let queryItems = linkedComponents.queryItems,
              !queryItems.isEmpty else {
            return
        }
        
        // Check and handle the URL and query items here.
    }
    // other methods…
}
```
{% endcode %}

### 确定应用启动的原因

当UIKit启动您的应用时，它会调用[`application:willFinishLaunchingWithOptions:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:willfinishlaunchingwithoptions:\)?language=objc)和[`application:didFinishLaunchingWithOptions:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:didfinishlaunchingwithoptions:\)?language=objc)方法，以指示应用启动过程已到达这些阶段。如果您的应用尚未实现场景生命周期，UIKit会将一个启动选项字典传递给这些方法，其中包含有关应用启动原因的信息。该字典中的键表示需要立即执行的重要任务。例如，它们可能反映用户在其他地方启动并希望在您的应用中继续的操作。始终检查启动选项字典的内容，查找您期望的键，并对其存在做出适当响应。更改提示类型

{% hint style="warning" %}
**Important**

当采用场景生命周期时，UIKit 会调用 [`application:willFinishLaunchingWithOptions:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:willfinishlaunchingwithoptions:\)?language=objc) 和 [`application:didFinishLaunchingWithOptions:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:didfinishlaunchingwithoptions:\)?language=objc)方法，但不再向这些方法提供启动选项字典。请改为实现场景连接方法，以接收有关应用启动和场景连接的详细信息。如需了解更多信息，请参阅[Scenes](https://developer.apple.com/documentation/uikit/scenes?language=objc)。
{% endhint %}

以下代码展示了处理接收URL的应用程序的应用程序委托方法。该代码检查传入URL的启动选项，以及该URL是否包含可用于配置应用程序的查询项。如果没有，则代码返回false，表示您的应用程序无法处理这些启动选项。

{% code lineNumbers="true" %}
```objectivec
class AppDelegate: UIResponder, UIApplicationDelegate {
    
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        
        // Check if the launch options include a URL,
        // and whether the URL has query items.
        guard let linkedUrl = launchOptions?[.url] as? URL,
              let linkedComponents = URLComponents(url: linkedUrl, resolvingAgainstBaseURL: false),
              let queryItems = linkedComponents.queryItems,
              !queryItems.isEmpty else {
            return false
        }
        
        // Check whether your app can handle the URL and query
        // items here. Then return `true` if so, otherwise `false`.
        // ...
        
        return true
    }
    // other methods…
}
```
{% endcode %}

除非您的应用支持相应功能，否则系统不会包含某个键。例如，对于不支持远程通知的应用，系统不会包含 [`UIApplicationLaunchOptionsRemoteNotificationKey`](https://developer.apple.com/documentation/uikit/uiapplication/launchoptionskey/remotenotification?language=objc) 键。

有关启动选项键列表以及如何处理这些键的信息，请参阅[`UIApplicationLaunchOptionsKey`](https://developer.apple.com/documentation/uikit/uiapplication/launchoptionskey?language=objc)。

***
