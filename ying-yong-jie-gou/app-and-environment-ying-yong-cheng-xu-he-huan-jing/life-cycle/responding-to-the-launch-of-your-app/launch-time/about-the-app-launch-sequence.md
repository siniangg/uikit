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
