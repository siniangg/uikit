---
icon: file-lines
---

# About the app launch sequence

## Overview

应用启动涉及一系列复杂的步骤，其中大部分由系统自动处理。在启动序列期间，UIKit 会调用应用委托和场景委托中的方法，以便您让应用准备好与用户交互，并执行应用需求特定的任何任务。以下说明了从用户或系统启动应用到序列完成的启动序列各个步骤：

<figure><img src="https://docs-assets.developer.apple.com/published/683ce287be9be981a5284fb4adf54a9d/app-launch-sequence~dark%402x.png" alt="" width="563"><figcaption></figcaption></figure>

1. 系统会执行 Xcode 在 Objective-C 项目中提供的 `main()` 函数，或在 Swift 项目中使用 `@main` 时提供的 `main()` 函数。
2. `main()` 函数会调用 `UIApplicationMain`，该函数会创建 `UIApplication` 实例和应用委托。
3. UIKit 会调用应用委托中的 `application:willFinishLaunchingWithOptions:` 方法。
4. UIKit 执行 **视图控制器状态恢复**，这会导致在应用委托和应用的视图控制器中执行其他方法。有关详细信息，请参阅 [关于 UI 恢复流程](https://developer.apple.com/documentation/uikit/views_and_controls/preserving_your_app_s_state/about_the_ui_restoration_process)。
5. UIKit 调用应用委托的 `application:didFinishLaunchingWithOptions:` 方法。
6. 应用启动完成后，UIKit 会准备一个场景以连接到应用，然后调用 `scene:willConnectToSession:options:`。UIKit 可能会向此方法传递一个用户活动（`user activity`），供您在场景连接期间处理。

启动序列完成后，系统会显示应用的用户界面，并在生命周期事件发生时通知应用或场景委托。

### 为应用预热做好准备

在iOS 15及更高版本中，系统可能会根据设备状况对您的App进行预热——启动未运行的应用程序进程，以减少用户等待应用可用的时间。预热会执行应用的启动序列，直至main()调用[`UIApplicationMain`](https://developer.apple.com/documentation/uikit/uiapplicationmain\(_:_:_:_:\)-1yub7?language=objc)之前，但不包括此步骤。这为系统提供了一个机会，以便在应用完全启动前构建并缓存其所需的任何底层结构。

{% hint style="info" %}
**Note**

如需了解系统在应用启动期间所需的底层结构的更多信息，请参阅 WWDC 会议视频[App Startup Time: Past, Present, and Future](https://developer.apple.com/videos/play/wwdc2017/413)。
{% endhint %}

系统对您的应用进行预热后，其启动序列将保持暂停状态，直到应用启动且序列恢复，或者系统为回收资源将已预热的应用从内存中移除。系统可以在设备重启后，以及系统条件允许时定期对您的应用进行预热。

如果您的应用在调用[`UIApplicationMain`](https://developer.apple.com/documentation/uikit/uiapplicationmain\(_:_:_:_:\)-1yub7?language=objc)之前执行代码，例如在诸如[`load`](https://developer.apple.com/documentation/ObjectiveC/NSObject-swift.class/load\(\)?language=objc)之类的静态初始化器中，请勿假定哪些服务和资源可用。例如，钥匙串项可能不可用，因为其数据保护策略要求设备解锁，而即使设备处于锁定状态也会进行预热。如果您的代码依赖于对特定服务或资源的访问，请将该代码迁移到启动序列的后续部分。

对应用进行预热会导致从预热阶段完成到用户或系统完全启动应用之间的时间不确定。因此，应使用[MetricKit](https://developer.apple.com/documentation/MetricKit?language=objc)准确测量由用户驱动的启动和恢复时间，而不是手动在启动序列的各个点设置标记。
