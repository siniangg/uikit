---
description: iOS中运行的应用程序的集中控制和协调点。
---

# UIApplication

> Class

```
iOS 2.0+ | iPadOS 2.0+ | Mac Catalyst 13.1+ | tvOS | visionOS 1.0+
```

{% code overflow="wrap" %}
```objectivec
@interface UIApplication : UIResponder
```
{% endcode %}

***

## [Mentioned in](https://developer.apple.com/documentation/uikit/uiapplication?language=objc#mentions) <a href="#mentions" id="mentions"></a>

📄[**使用响应者及响应链处理事件**](https://developer.apple.com/documentation/uikit/using-responders-and-the-responder-chain-to-handle-events?language=objc)

📄[**关于应用启动流程**](https://developer.apple.com/documentation/uikit/about-the-app-launch-sequence?language=objc)

📄[**处理物理键盘的按键输入**](https://developer.apple.com/documentation/uikit/handling-key-presses-made-on-a-physical-keyboard?language=objc)

📄[**关于界面状态保存机制**](https://developer.apple.com/documentation/uikit/about-the-ui-preservation-process?language=objc)

📄[**UIKit 应用开发指南**](https://developer.apple.com/documentation/uikit/about-app-development-with-uikit?language=objc)

## [Overview](https://developer.apple.com/documentation/uikit/uiapplication?language=objc#overview)

每个 iOS 应用都包含且仅包含一个 [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication?language=objc) 实例(在极少数情况下可能是[`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication?language=objc)子类)。应用启动时，系统会调用 [`UIApplicationMain`](https://developer.apple.com/documentation/uikit/uiapplicationmain\(_:_:_:_:\)-1yub7?language=objc) 函数，该函数在执行诸多任务的过程中会创建一个 [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication?language=objc) 单例对象，您可通过 [`sharedApplication`](https://developer.apple.com/documentation/uikit/uiapplication/shared?language=objc) 方法访问该实例。

应用中的UIApplication对象负责对用户事件的初始路由分发。它会将控件对象([`UIControl`](https://developer.apple.com/documentation/uikit/uicontrol?language=objc)类的实例)转发来的动作消息派发给相应目标对象。该对象同时维护着一个已打开窗口([`UIWindow`](https://developer.apple.com/documentation/uikit/uiwindow?language=objc)对象)的列表，通过该列表可获取应用中任意 [`UIView`](https://developer.apple.com/documentation/uikit/uiview?language=objc) 对象。

[`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication?language=objc) 类通过遵循  [`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate?language=objc) 协议的代理对象（必须实现该协议的部分方法）进行事件通知。应用对象会在运行时关键事件发生时（如应用启动、内存不足警告、应用终止等）通知代理对象，使其能够做出适当响应。

应用可通过 [`openURL:options:completionHandler:`](https://developer.apple.com/documentation/uikit/uiapplication/open\(_:options:completionhandler:\)?language=objc)  方法协作处理某些资源（例如电子邮件或图像文件）。例如，当应用调用此方法并传入邮件 URL 时，系统将启动邮件应用并显示对应邮件内容。

该类中的 API 可用于管理设备特定的行为。您可以通过  [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication?language=objc) 对象执行以下操作：

* 临时禁用触摸事件接收 ([`beginIgnoringInteractionEvents`](https://developer.apple.com/documentation/uikit/uiapplication/beginignoringinteractionevents\(\)?language=objc))
* 注册远程通知 ([`registerForRemoteNotifications`](https://developer.apple.com/documentation/uikit/uiapplication/registerforremotenotifications\(\)?language=objc))
* 触发撤销/重做界面 ([`applicationSupportsShakeToEdit`](https://developer.apple.com/documentation/uikit/uiapplication/applicationsupportsshaketoedit?language=objc))
* 检测是否有已安装应用可处理指定 URL 方案 ([`canOpenURL:`](https://developer.apple.com/documentation/uikit/uiapplication/canopenurl\(_:\)?language=objc))
* 延长应用执行时间以完成后台任务 ([`beginBackgroundTaskWithExpirationHandler:`](https://developer.apple.com/documentation/uikit/uiapplication/beginbackgroundtask\(expirationhandler:\)?language=objc) 和 [`beginBackgroundTaskWithName:expirationHandler:`](https://developer.apple.com/documentation/uikit/uiapplication/beginbackgroundtask\(withname:expirationhandler:\)?language=objc))
* 调度和取消本地通知 ([`scheduleLocalNotification:`](https://developer.apple.com/documentation/uikit/uiapplication/schedulelocalnotification\(_:\)?language=objc) 和 [`cancelLocalNotification:`](https://developer.apple.com/documentation/uikit/uiapplication/cancellocalnotification\(_:\)?language=objc))
* 协调远程控制事件接收 ([`beginReceivingRemoteControlEvents`](https://developer.apple.com/documentation/uikit/uiapplication/beginreceivingremotecontrolevents\(\)?language=objc) 和 [`endReceivingRemoteControlEvents`](https://developer.apple.com/documentation/uikit/uiapplication/endreceivingremotecontrolevents\(\)?language=objc))
* 执行应用级状态恢复任务 ([Managing state restoration](https://developer.apple.com/documentation/uikit/uiapplication?language=objc#Managing-state-restoration)任务组中的方法)

## [Subclassing notes](https://developer.apple.com/documentation/uikit/uiapplication?language=objc#Subclassing-notes)子类化说明 <a href="#subclassing-notes" id="subclassing-notes"></a>

绝大多数应用无需继承 [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication?language=objc) 类。通常，使用应用代理（App Delegate）即可管理系统与应用之间的交互。

若您的应用需要先于系统处理传入事件（这种情况极为罕见），可通过实现自定义事件或操作派发机制来实现。要做到这一点，子类化 UIApplication,重写以下方法之一或全部：[`sendEvent:`](https://developer.apple.com/documentation/uikit/uiapplication/sendevent\(_:\)?language=objc)和/或[`sendAction:to:from:forEvent:`](https://developer.apple.com/documentation/uikit/uiapplication/sendaction\(_:to:from:for:\)?language=objc)方法。对于每个被拦截的事件，在处理完成后，必须通过调用父类实现将事件重新派发给系统:

```objectivec
super.sendEvent(event)
```

事件拦截通常仅在极少数特殊场景下需要，开发者应尽量避免使用此机制。

***

## [Topics](https://developer.apple.com/documentation/uikit/uiapplication?language=objc#topics) <a href="#topics" id="topics"></a>

### **访问共享应用对象**

[`sharedApplication`](https://developer.apple.com/documentation/uikit/uiapplication/shared?language=objc)\
&#x20;       应用的单例实例（全局唯一对象）。

### **配置应用行为**

[`delegate`](https://developer.apple.com/documentation/uikit/uiapplication/delegate?language=objc)\
&#x20;       应用对象的委托（遵循 [`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate?language=objc) 协议）。

[`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate?language=objc)\
&#x20;       一组用于管理应用共享行为的方法。

### **远程通知注册**

[`- registerForRemoteNotifications`](https://developer.apple.com/documentation/uikit/uiapplication/registerforremotenotifications\(\)?language=objc)

&#x20;       **功能**：向 Apple 推送通知服务 (APNs) 注册，以接收远程通知。

[`- unregisterForRemoteNotifications`](https://developer.apple.com/documentation/uikit/uiapplication/unregisterforremotenotifications\(\)?language=objc)

&#x20;       **功能**：取消所有通过 Apple 推送通知服务 (APNs) 注册的远程通知。

[`registeredForRemoteNotifications`](https://developer.apple.com/documentation/uikit/uiapplication/isregisteredforremotenotifications?language=objc)

&#x20;       **属性**：一个布尔值，表示当前应用是否已注册远程通知。

### **获取应用状态**

[`applicationState`](https://developer.apple.com/documentation/uikit/uiapplication/applicationstate?language=objc)

&#x20;       **属性**：应用当前的状态（或其最活跃场景的状态）。

[`UIApplicationState`](https://developer.apple.com/documentation/uikit/uiapplication/state?language=objc)

&#x20;       **枚举**：表示应用运行状态的常量。

### **获取场景信息**

[`supportsMultipleScenes`](https://developer.apple.com/documentation/uikit/uiapplication/supportsmultiplescenes?language=objc)

&#x20;       **属性**：一个布尔值，表示应用是否支持同时显示多个场景（适用于多窗口应用）。

[`connectedScenes`](https://developer.apple.com/documentation/uikit/uiapplication/connectedscenes?language=objc)

&#x20;       **属性**：应用当前已连接的所有场景（`UIScene` 对象集合）。

[`openSessions`](https://developer.apple.com/documentation/uikit/uiapplication/opensessions?language=objc)

&#x20;       **属性**：系统当前活跃或已归档的场景所对应的会话（`UISceneSession` 对象集合）。

### **管理场景生命周期**

[`- activateSceneSessionForRequest:errorHandler:`](https://developer.apple.com/documentation/uikit/uiapplication/activatescenesessionforrequest:errorhandler:?language=objc)

&#x20;       **功能**：请求系统激活现有场景或创建新场景，并将其与应用关联。

[`- requestSceneSessionDestruction:options:errorHandler:`](https://developer.apple.com/documentation/uikit/uiapplication/requestscenesessiondestruction\(_:options:errorhandler:\)?language=objc)

&#x20;       **功能**：请求系统销毁指定场景，并将其从应用切换器（App Switcher）中移除。

[`- requestSceneSessionRefresh:`](https://developer.apple.com/documentation/uikit/uiapplication/requestscenesessionrefresh\(_:\)?language=objc)

&#x20;       **功能**：请求系统更新与指定场景关联的系统 UI。

[`UISceneSessionActivationRequest`](https://developer.apple.com/documentation/uikit/uiscenesessionactivationrequest-c.class?language=objc)

&#x20;       **用途**：包含场景激活请求所需属性的集合。

[`UISceneActivationRequestOptions`](https://developer.apple.com/documentation/uikit/uiscene/activationrequestoptions?language=objc)

&#x20;       **用途**：存储场景会话激活时需传递的系统配置信息。

[`UISceneDestructionRequestOptions`](https://developer.apple.com/documentation/uikit/uiscenedestructionrequestoptions?language=objc)

&#x20;       **用途**：向 UIKit 传递以永久移除场景及其关联会话的配置对象。

### **管理后台任务**

### [`backgroundRefreshStatus`](https://developer.apple.com/documentation/uikit/uiapplication/backgroundrefreshstatus?language=objc)

&#x20;       **属性**：表示应用是否允许在后台刷新内容。

[`UIBackgroundRefreshStatus`](https://developer.apple.com/documentation/uikit/uibackgroundrefreshstatus?language=objc)

&#x20;       **枚举**：指示应用是否启用了后台执行的常量。

[`UIApplicationBackgroundRefreshStatusDidChangeNotification`](https://developer.apple.com/documentation/uikit/uiapplication/backgroundrefreshstatusdidchangenotification?language=objc)

&#x20;       **通知**：当应用的后台内容下载状态发生变化时发送。

[`- beginBackgroundTaskWithName:expirationHandler:`](https://developer.apple.com/documentation/uikit/uiapplication/beginbackgroundtask\(withname:expirationhandler:\)?language=objc)

&#x20;       **功能**：标记一个自定义名称的后台任务的开始，该任务在应用进入后台后仍可继续执行。

[`- beginBackgroundTaskWithExpirationHandler:`](https://developer.apple.com/documentation/uikit/uiapplication/beginbackgroundtask\(expirationhandler:\)?language=objc)

&#x20;       **功能**：标记一个后台任务的开始，该任务在应用进入后台后仍可继续执行。

[`- endBackgroundTask:`](https://developer.apple.com/documentation/uikit/uiapplication/endbackgroundtask\(_:\)?language=objc)

&#x20;       **功能**：标记特定长时间运行的后台任务的结束。

[`UIBackgroundTaskIdentifier`](https://developer.apple.com/documentation/uikit/uibackgroundtaskidentifier?language=objc)

&#x20;       **类型**：标识后台任务请求的唯一令牌。

[`backgroundTimeRemaining`](https://developer.apple.com/documentation/uikit/uiapplication/backgroundtimeremaining?language=objc)

&#x20;       **属性**：应用在后台运行的最大剩余时间（以秒为单位）。

### **后台内容获取配置**

[`UIApplicationBackgroundFetchIntervalMinimum`](https://developer.apple.com/documentation/uikit/uiapplication/backgroundfetchintervalminimum?language=objc)

&#x20;       **常量**：系统支持的最小后台获取间隔（由系统动态决定，通常约 30 分钟）。

[`UIApplicationBackgroundFetchIntervalNever`](https://developer.apple.com/documentation/uikit/uiapplication/backgroundfetchintervalnever?language=objc)

&#x20;       **常量**：禁用后台获取功能（不自动触发内容更新）。

### **打开 URL 资源**

[`- openURL:options:completionHandler:`](https://developer.apple.com/documentation/uikit/uiapplication/open\(_:options:completionhandler:\)?language=objc)

&#x20;       **功能**：尝试异步打开指定 URL 对应的资源（如链接、文件或其他应用）。

[`- canOpenURL:`](https://developer.apple.com/documentation/uikit/uiapplication/canopenurl\(_:\)?language=objc)

&#x20;       **功能**：返回一个布尔值，表示当前设备是否有应用能处理该 URL 方案（如 `https://` 或 `tel://`）。

[`UIApplicationOpenExternalURLOptionsKey`](https://developer.apple.com/documentation/uikit/uiapplication/openexternalurloptionskey?language=objc)

&#x20;       **配置项**：用于控制 URL 打开行为的选项键（如是否显示警告弹窗）。

### **深度链接至系统设置**

[`UIApplicationOpenSettingsURLString`](https://developer.apple.com/documentation/uikit/uiapplication/opensettingsurlstring?language=objc)

&#x20;       **功能**：用于深度链接至应用在 **系统设置（Settings App）** 中的自定义配置页面的 URL 字符串。

[~~`UIApplicationOpenNotificationSettingsURLString`~~](https://developer.apple.com/documentation/uikit/uiapplicationopennotificationsettingsurlstring?language=objc)

&#x20;       **功能**：用于深度链接至应用在 **系统设置** 中的 **通知权限管理页面** 的 URL 字符串。

<mark style="color:orange;background-color:orange;">Deprecated</mark>

[`UIApplicationOpenDefaultApplicationsSettingsURLString`](https://developer.apple.com/documentation/uikit/uiapplication/opendefaultapplicationssettingsurlstring?language=objc)

&#x20;       **功能**：用于跳转至 **系统设置** 中 **默认应用选择界面** 的 URL 字符串（如设置默认浏览器或邮件应用）。

### **管理应用休眠计时器**

[`idleTimerDisabled`](https://developer.apple.com/documentation/uikit/uiapplication/isidletimerdisabled?language=objc)

&#x20;       **属性作用**：控制是否禁用系统的 **休眠计时器**（防止屏幕自动锁定）。

&#x20;       **类型**：\
&#x20;       `Bool`（`true` 表示阻止屏幕休眠，`false` 表示允许系统正常休眠）

### **管理状态恢复**

[`- extendStateRestoration`](https://developer.apple.com/documentation/uikit/uiapplication/extendstaterestoration\(\)?language=objc)

&#x20;       **功能**：通知应用，当前正在 **异步执行状态恢复**（延长恢复周期）。

[`- completeStateRestoration`](https://developer.apple.com/documentation/uikit/uiapplication/completestaterestoration\(\)?language=objc)

&#x20;       **功能**：通知应用，**异步状态恢复已完成**（与 `extendStateRestoration` 配对使用）。

[`- ignoreSnapshotOnNextApplicationLaunch`](https://developer.apple.com/documentation/uikit/uiapplication/ignoresnapshotonnextapplicationlaunch\(\)?language=objc)

&#x20;       **功能**：**强制下次启动时不使用最近的快照图像**（避免显示过时界面）。

[`+ registerObjectForStateRestoration:restorationIdentifier:`](https://developer.apple.com/documentation/uikit/uiapplication/registerobject\(forstaterestoration:restorationidentifier:\)?language=objc)

&#x20;       **功能**：为 **状态恢复系统** 注册自定义对象（需实现 `UIStateRestoring` 协议）。

### **管理应用的快捷操作项**

[`shortcutItems`](https://developer.apple.com/documentation/uikit/uiapplication/shortcutitems?language=objc)

&#x20;       **属性作用**：管理应用在 **主屏幕** 上的动态快捷操作（3D Touch/Haptic Touch 菜单项）。

&#x20;       **类型**：\
&#x20;       `[UIApplicationShortcutItem]` 数组（每个元素代表一个快捷操作）

### **访问受保护内容**

[`protectedDataAvailable`](https://developer.apple.com/documentation/uikit/uiapplication/isprotecteddataavailable?language=objc)

&#x20;       **属性作用**：指示当前设备是否启用了 **数据保护**（文件级加密），并且受保护文件可被访问。

&#x20;       **类型**：`Bool`（`true` 表示文件可访问，`false` 表示文件被锁定）

[`UIApplicationProtectedDataDidBecomeAvailable`](https://developer.apple.com/documentation/uikit/uiapplication/protecteddatadidbecomeavailablenotification?language=objc)

&#x20;       **通知**：当设备解锁后，受保护文件 **变为可访问时** 系统发送的通知。

[`UIApplicationProtectedDataWillBecomeUnavailable`](https://developer.apple.com/documentation/uikit/uiapplication/protecteddatawillbecomeunavailablenotification?language=objc)

&#x20;       **通知**：当设备即将锁定时，系统 **提前通知** 受保护文件 **即将被锁定**。

### **接收远程控制事件**

[`- beginReceivingRemoteControlEvents`](https://developer.apple.com/documentation/uikit/uiapplication/beginreceivingremotecontrolevents\(\)?language=objc)

&#x20;       **功能**：启用应用接收 **远程控制事件**（如耳机线控、蓝牙设备按键、锁屏界面控制等）。

[`- endReceivingRemoteControlEvents`](https://developer.apple.com/documentation/uikit/uiapplication/endreceivingremotecontrolevents\(\)?language=objc)

&#x20;       **功能**：停止接收远程控制事件。

&#x20;       **使用场景**

&#x20;       音乐/播客类应用（播放/暂停/切歌）

&#x20;       视频应用（锁屏界面控制播放进度）

&#x20;       车载系统或外接设备控制

### **访问界面布局方向**

[`userInterfaceLayoutDirection`](https://developer.apple.com/documentation/uikit/uiapplication/userinterfacelayoutdirection?language=objc)

&#x20;       **属性作用**：获取当前用户界面的布局方向（左到右或右到左）。

[`UIUserInterfaceLayoutDirection`](https://developer.apple.com/documentation/uikit/uiuserinterfacelayoutdirection?language=objc)

&#x20;       指定用户界面方向流的常量。

&#x20;       **枚举值**：

&#x20;       `.leftToRight`：从左到右布局（如英文、中文界面）

&#x20;       `.rightToLeft`：从右到左布局（如阿拉伯语、希伯来语界面）

### **控制与处理事件**

[`- sendEvent:`](https://developer.apple.com/documentation/uikit/uiapplication/sendevent\(_:\)?language=objc)

&#x20;       **功能**：将事件（如触摸、摇动或远程控制事件）分发给应用内 **合适的响应者对(Responder Chain)。**

&#x20;       **使用场景**：

&#x20;       自定义事件分发逻辑（需继承 `UIApplication` 并重写此方法）

&#x20;       全局监控或修改原始事件（如实现防误触）

[`- sendAction:to:from:forEvent:`](https://developer.apple.com/documentation/uikit/uiapplication/sendaction\(_:to:from:for:\)?language=objc)

&#x20;       **功能**：向指定目标（`target`）发送 **动作消息**（Action Message）。若 `target` 为 `nil`，则沿响应者链传递。

&#x20;       **使用场景**：

&#x20;       动态触发按钮点击等动作

&#x20;       跨对象传递自定义事件

[`applicationSupportsShakeToEdit`](https://developer.apple.com/documentation/uikit/uiapplication/applicationsupportsshaketoedit?language=objc)

&#x20;       **属性作用**：\
&#x20;       控制是否启用 **摇动设备触发撤销/重做界面**（默认 `false`）。

&#x20;       **使用场景**：

&#x20;       文本编辑器类应用需支持撤销操作时

### **管理应用图标**

[`supportsAlternateIcons`](https://developer.apple.com/documentation/uikit/uiapplication/supportsalternateicons?language=objc)

&#x20;       **属性作用**：指示应用是否允许更改其图标（`Bool` 类型）。

[`alternateIconName`](https://developer.apple.com/documentation/uikit/uiapplication/alternateiconname?language=objc)

&#x20;       **属性作用**：获取当前系统显示的应用替代图标名称（`String?` 类型）。

[`- setAlternateIconName:completionHandler:`](https://developer.apple.com/documentation/uikit/uiapplication/setalternateiconname\(_:completionhandler:\)?language=objc)

&#x20;       **方法作用**：更改系统显示的应用图标。

&#x20;       **使用场景**

&#x20;       动态切换应用图标（如节日主题、深色模式适配等）

&#x20;       需在 `Info.plist` 中预定义所有可选图标

### **管理内容显示尺寸**

[`preferredContentSizeCategory`](https://developer.apple.com/documentation/uikit/uiapplication/preferredcontentsizecategory?language=objc)

&#x20;       **属性作用**：获取用户当前设置的 **文字大小偏好**（如“大标题”、“辅助功能超大文本”）。

&#x20;       **类型**：`UIContentSizeCategory`（枚举值，如 `.large`、`.extraLarge`）

[`UIContentSizeCategory`](https://developer.apple.com/documentation/uikit/uicontentsizecategory?language=objc)

&#x20;       **枚举作用**：定义系统支持的 **内容尺寸等级**（共 20+ 种，从 `.extraSmall` 到 `.accessibilityExtraExtraExtraLarge`）。

[`UIContentSizeCategoryAdjusting`](https://developer.apple.com/documentation/uikit/uicontentsizecategoryadjusting?language=objc)

&#x20;       **协议作用**：为控件提供 **自动适配内容尺寸变化** 的便捷方法（如 `adjustsFontForContentSizeCategory`）。

[`UIContentSizeCategoryDidChangeNotification`](https://developer.apple.com/documentation/uikit/uicontentsizecategory/didchangenotification?language=objc)

&#x20;       **通知作用**：当用户修改系统文字大小时触发，通知应用更新界面。

[`UIContentSizeCategoryNewValueKey`](https://developer.apple.com/documentation/uikit/uicontentsizecategory/newvalueuserinfokey?language=objc)

&#x20;       **键名作用**：从通知的 `userInfo` 中获取 **新的内容尺寸值**。

### **指定支持的界面方向**

[`- supportedInterfaceOrientationsForWindow:`](https://developer.apple.com/documentation/uikit/uiapplication/supportedinterfaceorientations\(for:\)?language=objc)

&#x20;       **功能**：返回指定窗口（`UIWindow`）中视图控制器所使用的 **默认界面方向集合**（如竖屏、横屏等）。

&#x20;       **类型**：`UIInterfaceOrientationMask`（枚举组合，如 `.portrait`、`.landscape`）

&#x20;       **使用场景**

&#x20;       全局控制应用支持的屏幕方向

&#x20;       针对不同窗口（如主窗口/弹窗）设置不同方向

### **追踪运行循环中的控件交互**

[`UITrackingRunLoopMode`](https://developer.apple.com/documentation/uikit/uitrackingrunloopmode?language=objc)

&#x20;       **作用**：一种特殊的 **运行循环模式（Run Loop Mode）**，当用户与控件（如 `UISlider`、`UIScrollView`）交互时，系统会自动切换到该模式。

&#x20;       **类型**：`RunLoop.Mode`（属于 `Foundation` 框架）

&#x20;       **使用场景:**

&#x20;       **1.优化性能**：

&#x20;       在滚动/拖动期间暂停高负载任务（如网络请求）

&#x20;       确保交互流畅性

&#x20;       **2.监听交互状态**

### **检测屏幕截图**

[`UIApplicationUserDidTakeScreenshotNotification`](https://developer.apple.com/documentation/uikit/uiapplication/userdidtakescreenshotnotification?language=objc)

&#x20;       **通知作用**：当用户在设备上 **截取屏幕截图** 时，系统会发送此通知。

&#x20;       **使用场景**

&#x20;       敏感内容保护（如银行/聊天应用）

&#x20;       截图后的用户引导（如提示“截图已保存”）

&#x20;       数据统计（记录用户截图行为）

### **检测应用是否为某类别的默认应用**

[`- defaultStatusForCategory:error:`](https://developer.apple.com/documentation/uikit/uiapplication/defaultstatusforcategory:error:?language=objc)

&#x20;       **功能**：查询当前应用是否为用户在 **指定类别**（如浏览器、邮件、音乐等）的 **默认应用**。

[`UIApplicationCategoryDefaultStatus`](https://developer.apple.com/documentation/uikit/uiapplicationcategorydefaultstatus?language=objc)

&#x20;       **作用**：表示应用在某个类别中的默认状态。\
&#x20;       **使用场景**：用于检查应用是否为系统默认应用（如默认浏览器、默认邮件客户端等）。

[`UIApplicationCategory`](https://developer.apple.com/documentation/uikit/uiapplication/category?language=objc)\
&#x20;       **作用**：描述系统中应用类型的枚举（如浏览器、邮件客户端等）。

[`UIApplicationCategoryDefaultErrorDomain`](https://developer.apple.com/documentation/uikit/uiapplicationcategorydefaulterrordomain?language=objc)

&#x20;       **作用**：标识系统在判断应用是否为默认应用时发生的错误域。

[`UIApplicationCategoryDefaultRetryAvailabilityDateErrorKey`](https://developer.apple.com/documentation/uikit/uiapplicationcategorydefaultretryavailabilitydateerrorkey?language=objc)

&#x20;       **作用**：字典键，其值为下次可重新尝试查询的日期（`Date`类型）。

[`UIApplicationCategoryDefaultStatusLastProvidedDateErrorKey`](https://developer.apple.com/documentation/uikit/uiapplicationcategorydefaultstatuslastprovideddateerrorkey?language=objc)

&#x20;       **作用**：字典键，其值为上次成功获取默认状态的日期（`Date`类型）。

### **已废弃符号**

[Deprecated symbols](https://developer.apple.com/documentation/uikit/uiapplication-deprecated-symbols?language=objc)

&#x20;       查看不受支持的符号及其替代符号。

***

## **关系(**&#x52;elationships)

### **继承自（Inherits From）**

[`UIResponder`](https://developer.apple.com/documentation/uikit/uiresponder?language=objc)

***

## [See Also](https://developer.apple.com/documentation/uikit/uiapplication?language=objc#see-also)(另请参阅) <a href="#see-also" id="see-also"></a>

### [Life cycle](https://developer.apple.com/documentation/uikit/uiapplication?language=objc#Life-cycle)(生命周期) <a href="#life-cycle" id="life-cycle"></a>

&#x20;       [Managing your app’s life cycle](https://developer.apple.com/documentation/uikit/managing-your-app-s-life-cycle?language=objc)

&#x20;               当应用处于前台或后台时响应系统通知，并处理其他重要的系统相关事件。

&#x20;       [Responding to the launch of your app](https://developer.apple.com/documentation/uikit/responding-to-the-launch-of-your-app?language=objc)

&#x20;               初始化应用的数据结构，准备应用运行环境，并响应系统在启动时的任何请求。



[`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate?language=objc)

&#x20;       一组用于管理应用程序共享行为的方法。

&#x20;       [Scenes](https://developer.apple.com/documentation/uikit/scenes?language=objc)

&#x20;               同时管理应用程序用户界面的多个实例，并将资源导向相应的用户界面实例。\
