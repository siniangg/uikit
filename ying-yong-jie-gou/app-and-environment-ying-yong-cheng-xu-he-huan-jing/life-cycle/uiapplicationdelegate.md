---
description: 一组用于管理应用程序共享行为的方法。
---

# UIApplicationDelegate

> Protocol

```
iOS | iPadOS | Mac CatalysttvOS | visionOS
```

```objectivec
@protocol UIApplicationDelegate <NSObject>
```

***

## [Overview](https://developer.apple.com/documentation/uikit/uiapplicationdelegate?language=objc#overview) <a href="#overview" id="overview"></a>

应用程序代理对象管理应用程序的共享行为。应用程序代理实际上是应用程序的根对象，它与 [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication?language=objc) 协同工作，管理与系统的某些交互。与 [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication?language=objc) 对象一样，UIKit 会在应用程序启动周期的早期创建应用程序代理对象，因此它始终存在。

使用你的应用程序代理对象来处理以下任务：

* 初始化应用程序的核心数据结构
* 配置你的应用程序的场景
* 响应源自应用程序外部的通知，例如低内存警告、下载完成通知等
* 响应以应用程序自身为目标的事件，这些事件并非特定于应用程序的场景、视图或视图控制器
* 在启动时注册任何所需的服务，例如苹果推送通知服务

有关如何使用**应用程序代理对象(**&#x61;pp delegate objec&#x74;**)**&#x5728;启动时初始化应用程序的更多信息，请参阅[Responding to the launch of your app](https://developer.apple.com/documentation/uikit/responding-to-the-launch-of-your-app?language=objc)。

## **iOS 12 及更早版本的生命周期管理**

在 iOS 12 及更早版本中，需通过 **应用委托（App Delegate）** 管理应用的主要生命周期事件。具体而言，需使用应用委托的方法来响应以下状态变更：

* **应用进入前台时**更新状态
* **应用移至后台时**保存数据或释放资源

**1. 应用进入前台**

&#x20;       关于应用进入前台时的处理方式，请参阅：[Preparing your UI to run in the foreground](https://developer.apple.com/documentation/uikit/preparing-your-ui-to-run-in-the-foreground?language=objc)

**2. 应用进入后台**

&#x20;       关于应用进入后台时的处理方式，请参阅：[Preparing your UI to run in the background](https://developer.apple.com/documentation/uikit/preparing-your-ui-to-run-in-the-background?language=objc)

**3. 通用生命周期管理**

&#x20;       关于应用生命周期的综合管理，请参阅：[Managing your app’s life cycle](https://developer.apple.com/documentation/uikit/managing-your-app-s-life-cycle?language=objc).

***

## Topics

### **应用初始化**

[`- application:willFinishLaunchingWithOptions:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:willfinishlaunchingwithoptions:\)?language=objc)

&#x20;       **作用**：通知应用委托，**应用启动流程已开始**（此时根视图控制器尚未加载）。

[`- application:didFinishLaunchingWithOptions:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:didfinishlaunchingwithoptions:\)?language=objc)

&#x20;       **作用**：通知应用委托，**应用启动流程即将完成**（此时界面已准备就绪）。

[`UIApplicationLaunchOptionsKey`](https://developer.apple.com/documentation/uikit/uiapplication/launchoptionskey?language=objc)

&#x20;       **作用**：系统传递给应用的 **启动选项字典键名集合**，用于识别启动场景。

[`UIApplicationDidFinishLaunchingNotification`](https://developer.apple.com/documentation/uikit/uiapplication/didfinishlaunchingnotification?language=objc)

&#x20;       **作用**：**应用完成启动后立即发送** 的系统通知（无论是否使用 AppDelegate 均可监听）。

### **配置与销毁场景**

[`- application:configurationForConnectingSceneSession:options:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:configurationforconnecting:options:\)?language=objc)

&#x20;       **作用**：**为 UIKit 提供新场景的配置数据**，当系统需要创建新场景时调用。

[`- application:didDiscardSceneSessions:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:diddiscardscenesessions:\)?language=objc)

&#x20;       **作用**：**通知应用用户已从应用切换器中关闭一个或多个场景**。

### **响应应用生命周期事件**

#### **1. 应用状态切换方法**

<table><thead><tr><th width="301.66015625">方法名</th><th>调用时机</th><th>典型用途</th></tr></thead><tbody><tr><td><a href="https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationdidbecomeactive(_:)?language=objc"><code>- applicationDidBecomeActive:</code></a></td><td>应用已进入前台并处于活动状态（首次启动、从后台返回、来电/弹窗消失后）</td><td>恢复动画/刷新数据/继续任务</td></tr><tr><td><a href="https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationwillresignactive(_:)?language=objc"><code>- applicationWillResignActive:</code></a></td><td>应用即将失去活动状态（如来电、弹窗、下拉控制中心）</td><td>暂停游戏/保存临时状态/停止敏感操作</td></tr><tr><td><a href="https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationdidenterbackground(_:)?language=objc"><code>- applicationDidEnterBackground:</code></a></td><td>应用已完全进入后台（约5秒内未调用<code>beginBackgroundTask</code>则可能被挂起）</td><td>释放资源/保存用户数据/清理内存</td></tr><tr><td><a href="https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationwillenterforeground(_:)?language=objc"><code>- applicationWillEnterForeground:</code></a></td><td>应用即将从后台返回前台（但尚未激活）</td><td>预加载数据/恢复UI状态</td></tr><tr><td><a href="https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationwillterminate(_:)?language=objc"><code>- applicationWillTerminate:</code></a></td><td>应用即将被系统终止（后台挂起状态被清理时）</td><td>执行最终清理操作</td></tr></tbody></table>

[`- applicationDidBecomeActive:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationdidbecomeactive\(_:\)?language=objc)

&#x20;       告诉委托应用程序已经激活。

[`- applicationWillResignActive:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationwillresignactive\(_:\)?language=objc)

&#x20;       告诉委托应用程序即将进入非活动状态。

[`- applicationDidEnterBackground:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationdidenterbackground\(_:\)?language=objc)

&#x20;       告诉委托应用现在在后台。

[`- applicationWillEnterForeground:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationwillenterforeground\(_:\)?language=objc)

&#x20;       告诉委托应用即将进入前台。

[`- applicationWillTerminate:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationwillterminate\(_:\)?language=objc)

&#x20;       告诉委托应用何时即将终止。

#### **2. 等效通知事件**

<table><thead><tr><th width="413.3046875">通知名</th><th width="300.87109375">对应方法</th><th>特殊说明</th></tr></thead><tbody><tr><td><a href="https://developer.apple.com/documentation/uikit/uiapplication/didbecomeactivenotification?language=objc"><code>UIApplicationDidBecomeActiveNotification</code></a></td><td><code>applicationDidBecomeActive:</code></td><td>可通过任意对象监听</td></tr><tr><td><a href="https://developer.apple.com/documentation/uikit/uiapplication/didenterbackgroundnotification?language=objc"><code>UIApplicationDidEnterBackgroundNotification</code></a></td><td><code>applicationDidEnterBackground:</code></td><td>进入后台后立即发送</td></tr><tr><td><a href="https://developer.apple.com/documentation/uikit/uiapplication/willenterforegroundnotification?language=objc"><code>UIApplicationWillEnterForegroundNotification</code></a></td><td><code>applicationWillEnterForeground:</code></td><td>从后台返回前触发</td></tr><tr><td><a href="https://developer.apple.com/documentation/uikit/uiapplication/willresignactivenotification?language=objc"><code>UIApplicationWillResignActiveNotification</code></a></td><td><code>applicationWillResignActive:</code></td><td>在失去焦点前触发</td></tr><tr><td><a href="https://developer.apple.com/documentation/uikit/uiapplication/willterminatenotification?language=objc"><code>UIApplicationWillTerminateNotification</code></a></td><td><code>applicationWillTerminate:</code></td><td>可能不会100%触发（优先使用<code>didEnterBackground</code>）</td></tr></tbody></table>

[`UIApplicationDidBecomeActiveNotification`](https://developer.apple.com/documentation/uikit/uiapplication/didbecomeactivenotification?language=objc)

&#x20;       当应用程序激活时发出的通知。

[`UIApplicationDidEnterBackgroundNotification`](https://developer.apple.com/documentation/uikit/uiapplication/didenterbackgroundnotification?language=objc)

&#x20;       当应用程序进入后台时发布的通知。

[`UIApplicationWillEnterForegroundNotification`](https://developer.apple.com/documentation/uikit/uiapplication/willenterforegroundnotification?language=objc)

&#x20;       在应用程序离开后台状态成为活动应用程序之前不久发布的

[`UIApplicationWillResignActiveNotification`](https://developer.apple.com/documentation/uikit/uiapplication/willresignactivenotification?language=objc)

&#x20;       当应用不再活跃或失去焦点时，会发出通知。

[`UIApplicationWillTerminateNotification`](https://developer.apple.com/documentation/uikit/uiapplication/willterminatenotification?language=objc)

&#x20;       当应用程序即将终止时发布的通知。

### **环境变化响应**

[`- applicationProtectedDataDidBecomeAvailable:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationprotecteddatadidbecomeavailable\(_:\)?language=objc)

&#x20;       告知delegate受保护的文件现已可用。

[`- applicationProtectedDataWillBecomeUnavailable:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationprotecteddatawillbecomeunavailable\(_:\)?language=objc)

&#x20;       告知delegate受保护的文件即将不可用。

[`- applicationDidReceiveMemoryWarning:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationdidreceivememorywarning\(_:\)?language=objc)

&#x20;       当App收到系统发出的内存警告时，告知delegate。

[`- applicationSignificantTimeChange:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationsignificanttimechange\(_:\)?language=objc)

&#x20;       当时间发生重大变化时，告知delegate。

[`UIApplicationProtectedDataDidBecomeAvailable`](https://developer.apple.com/documentation/uikit/uiapplication/protecteddatadidbecomeavailablenotification?language=objc)

&#x20;       当受保护的文件可供你的代码访问时发布的通知。

[`UIApplicationProtectedDataWillBecomeUnavailable`](https://developer.apple.com/documentation/uikit/uiapplication/protecteddatawillbecomeunavailablenotification?language=objc)

&#x20;       在受保护文件被锁定并无法访问前不久发布的通知。

[`UIApplicationDidReceiveMemoryWarningNotification`](https://developer.apple.com/documentation/uikit/uiapplication/didreceivememorywarningnotification?language=objc)

&#x20;       当App收到操作系统发出的内存不足警告时发布的通知。

[`UIApplicationSignificantTimeChangeNotification`](https://developer.apple.com/documentation/uikit/uiapplication/significanttimechangenotification?language=objc)

&#x20;       当时间发生重大变化时发布的通知。

### **管理应用状态恢复**

[`- application:shouldSaveSecureApplicationState:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:shouldsavesecureapplicationstate:\)?language=objc)\
&#x20;       询问代理是否应安全保存应用状态

[`- application:shouldRestoreSecureApplicationState:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:shouldrestoresecureapplicationstate:\)?language=objc)\
&#x20;       询问代理是否应恢复已保存的应用状态

[`- application:viewControllerWithRestorationIdentifierPath:coder:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:viewcontrollerwithrestorationidentifierpath:coder:\)?language=objc)\
&#x20;       要求代理根据恢复标识符路径和编码器提供指定的视图控制器

[`- application:willEncodeRestorableStateWithCoder:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:willencoderestorablestatewith:\)?language=objc)\
&#x20;       通知代理在状态保存流程开始时编码高层状态信息

[`- application:didDecodeRestorableStateWithCoder:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:diddecoderestorablestatewith:\)?language=objc)\
&#x20;       通知代理在状态恢复流程中解码高层状态信息

[`UIApplicationStateRestorationBundleVersionKey`](https://developer.apple.com/documentation/uikit/uiapplication/staterestorationbundleversionkey?language=objc)\
&#x20;       标识创建恢复存档的应用版本

[`UIApplicationStateRestorationSystemVersionKey`](https://developer.apple.com/documentation/uikit/uiapplication/staterestorationsystemversionkey?language=objc)\
&#x20;       标识创建恢复存档时的系统版本

[`UIApplicationStateRestorationTimestampKey`](https://developer.apple.com/documentation/uikit/uiapplication/staterestorationtimestampkey?language=objc)\
&#x20;       记录恢复存档的创建时间戳

[`UIApplicationStateRestorationUserInterfaceIdiomKey`](https://developer.apple.com/documentation/uikit/uiapplication/staterestorationuserinterfaceidiomkey?language=objc)\
&#x20;       存档创建时的用户界面类型（如 iPhone/iPad）

[`UIStateRestorationViewControllerStoryboardKey`](https://developer.apple.com/documentation/uikit/uiapplication/staterestorationviewcontrollerstoryboardkey?language=objc)\
&#x20;       指向包含视图控制器的故事板文件的引用

### **后台数据下载管理**

**方法**

[`- application:handleEventsForBackgroundURLSession:completionHandler:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:handleeventsforbackgroundurlsession:completionhandler:\)?language=objc)\
&#x20;       当应用处于后台时，通知代理有 **与 URL 会话相关的事件**（如下载完成、任务重试或身份验证挑战）需要处理。

[`UIBackgroundFetchResult`](https://developer.apple.com/documentation/uikit/uibackgroundfetchresult?language=objc)

&#x20;       指示后台数据获取操作结果的常量：

* `.newData`：成功获取新数据
* `.noData`：未获取到新数据
* `.failed`：获取失败（如网络错误或超时）

### **处理远程通知注册**

[`- application:didRegisterForRemoteNotificationsWithDeviceToken:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:didregisterforremotenotificationswithdevicetoken:\)?language=objc)

&#x20;       告知**delegate**应用已成功向苹果推送通知服务 (APNs) 注册。\
&#x20;       当应用成功向 **Apple 推送通知服务 (APNs)** 注册时调用，返回设备的唯一标识符 **Device Token**。\
&#x20;       **典型用途**：将 Device Token 发送至服务器以启用推送功能。

[`- application:didFailToRegisterForRemoteNotificationsWithError:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:didfailtoregisterforremotenotificationswitherror:\)?language=objc)

&#x20;       告知**delegate**苹果推送通知服务无法成功完成注册流程的情况。\
&#x20;       当 APNs 注册失败时调用（如用户拒绝权限、网络问题等）。\
&#x20;       **典型响应**：记录错误并禁用推送相关功能。

[`- application:didReceiveRemoteNotification:fetchCompletionHandler:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:didreceiveremotenotification:fetchcompletionhandler:\)?language=objc)

&#x20;       告知**App**有远程通知到达，表明有数据需要获取。\
&#x20;       当远程通知到达且包含需后台获取的数据时调用（需启用 `Remote notifications` 后台模式）。\
&#x20;       **关键操作**：处理数据后 **必须调用 `fetchCompletionHandler`** 并返回结果。

### **持续用户活动与处理快捷操作**

[`- application:willContinueUserActivityWithType:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:willcontinueuseractivitywithtype:\)?language=objc)

&#x20;       当用户活动（如 Handoff）需要较长时间加载时，询问**delegate**是否接管等待期间的提示责任。

[`- application:continueUserActivity:restorationHandler:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:continue:restorationhandler:\)?language=objc)

&#x20;       通知**delegate**用户活动数据已就绪，需恢复相关任务。

&#x20;       **关键操作**：必须调用 `restorationHandler` 并传递需恢复的对象。

[`- application:didUpdateUserActivity:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:didupdate:\)?language=objc)

&#x20;       告知**delegate**该活动已更新。用户活动内容更新时触发（如用户修改了正在同步的文档）。

[`- application:didFailToContinueUserActivityWithType:error:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:didfailtocontinueuseractivitywithtype:error:\)?language=objc)

&#x20;       告知**delegate**该活动无法继续进行。用户活动无法继续时调用（如设备不兼容或数据损坏）。

[`- application:performActionForShortcutItem:completionHandler:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:performactionfor:completionhandler:\)?language=objc)

&#x20;       告诉**delegate**用户为你的应用选择了主屏幕快捷操作，但你在启动方法中拦截了该交互的情况除外。

&#x20;       用户通过 **主屏幕 3D Touch/Haptic Touch 快捷操作** 启动应用时触发。

### 与 WatchKit 交互

[`- application:handleWatchKitExtensionRequest:reply:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:handlewatchkitextensionrequest:reply:\)?language=objc)

请求**delegate**响应配对的watchOS应用程序发出的请求。

当配对的 watchOS 应用 通过 WatchKit 扩展发起请求时调用此方法。

### 与HealthKit交互

[`- applicationShouldRequestHealthAuthorization:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationshouldrequesthealthauthorization\(_:\)?language=objc)

&#x20;       告诉**delegate**你的应用程序何时应该请求用户访问他或她的HealthKit数据。

&#x20;       当App需要请求用户授权访问其 **HealthKit 数据**时，系统调用此方法通知代理。

&#x20;       健康授权

### **通过 URL 打开指定资源**

[`- application:openURL:options:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:open:options:\)?language=objc)

&#x20;       要求**delegate**打开由URL指定的资源，并提供启动选项的字典。

&#x20;       当系统或其他应用请求通过 **URL Scheme** 打开当前应用时调用此方法。

[`UIApplicationOpenURLOptionsKey`](https://developer.apple.com/documentation/uikit/uiapplication/openurloptionskey?language=objc)

&#x20;       打开URL时用于访问选项字典中的值的键。

### **禁用指定应用扩展类型**

[`- application:shouldAllowExtensionPointIdentifier:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:shouldallowextensionpointidentifier:\)?language=objc)

&#x20;       请求**delegate**授予使用基于指定**扩展点标识符**的应用程序扩展的权限。

&#x20;       询问代理是否允许使用基于 **指定扩展点标识符** 的应用扩展

[`UIApplicationExtensionPointIdentifier`](https://developer.apple.com/documentation/uikit/uiapplication/extensionpointidentifier?language=objc)

&#x20;       用于标识扩展类型的结构体

[`UIApplicationKeyboardExtensionPointIdentifier`](https://developer.apple.com/documentation/uikit/uiapplication/extensionpointidentifier/keyboard?language=objc)

&#x20;       自定义键盘扩展的标识符

### **处理 SiriKit 意图**

[`- application:handlerForIntent:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:handlerfor:\)?language=objc)

&#x20;       要求**delegate**返回能够处理 **指定 SiriKit 意图** 的处理器对象

#### **处理 CloudKit 共享邀请**

[`- application:userDidAcceptCloudKitShareWithMetadata:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:userdidacceptcloudkitsharewith:\)?language=objc)

&#x20;       告诉**delegate，APP**现在可以访问CloudKit中的共享信息。

&#x20;       当用户接受 **CloudKit** 共享邀请时调用，通知代理**APP**已获得共享数据的访问权限。

### **键盘快捷键本地化**

[`- applicationShouldAutomaticallyLocalizeKeyCommands:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationshouldautomaticallylocalizekeycommands\(_:\)?language=objc)

&#x20;       返回布尔值，指示系统是否应 **自动重新映射菜单快捷键** 以适配本地化键盘布局。

&#x20;       **使用场景：**

&#x20;       **多语言支持**\
&#x20;               当应用支持多种语言键盘（如 QWERTY 与 AZERTY 布局差异）时，自动转换快捷键映射

&#x20;       **快捷键冲突避免**\
&#x20;               防止因键盘布局不同导致的快捷键失效（如 `Ctrl+C` 在法语布局中可能映射到不同物理按键）

&#x20;       **无障碍适配**\
&#x20;               确保使用非标准键盘布局的用户能正确触发快捷键

### **管理界面方向**

[`- application:supportedInterfaceOrientationsForWindow:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:supportedinterfaceorientationsfor:\)?language=objc)

&#x20;       要求代理返回指定窗口（`UIWindow`）中视图控制器支持的 **界面方向集合**。

[`UIInterfaceOrientation`](https://developer.apple.com/documentation/uikit/uiinterfaceorientation?language=objc)

&#x20;       指定**APP**用户界面方向的常量。

&#x20;       `.portrait`（竖屏）\
&#x20;       `.landscapeLeft`（左横屏）\
&#x20;       `.landscapeRight`（右横屏）\
&#x20;       `.portraitUpsideDown`（倒置竖屏）

[`UIInterfaceOrientationMask`](https://developer.apple.com/documentation/uikit/uiinterfaceorientationmask?language=objc)

&#x20;       指定视图控制器支持的接口方向的常量。

&#x20;       视图控制器支持的方向掩码：\
&#x20;       `.portrait`\
&#x20;       `.landscape`\
&#x20;       `.all`\
&#x20;       `.allButUpsideDown`

[`UIApplicationInvalidInterfaceOrientationException`](https://developer.apple.com/documentation/uikit/uiapplication/invalidinterfaceorientationexception?language=objc)

&#x20;       当**view controller**或**APP**返回一组无效的支持的界面朝向时抛出的异常。

&#x20;       当返回无效方向配置时抛出的异常（如未实现必需方法或返回空值）

### **为 Storyboard 提供窗口**

[`window`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/window?language=objc)

在呈现 Storyboard 时使用的窗口。

### 已弃用

[~~`- applicationDidFinishLaunching:`~~](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationdidfinishlaunching\(_:\)?language=objc)

&#x20;       当应用程序完成启动时告诉**delegate**。

&#x20;       <mark style="color:orange;background-color:orange;">Deprecated</mark>

[Deprecated symbols](https://developer.apple.com/documentation/uikit/uiapplicationdelegate-deprecated-symbols?language=objc)

&#x20;       不再支持的符号。

***

### Relationships(关系) <a href="#relationships" id="relationships"></a>

继承自

[`NSObject`](https://developer.apple.com/documentation/ObjectiveC/NSObjectProtocol?language=objc)

***

### [See Also](https://developer.apple.com/documentation/uikit/uiapplicationdelegate?language=objc#see-also) <a href="#see-also" id="see-also"></a>

#### [Life cycle](https://developer.apple.com/documentation/uikit/uiapplicationdelegate?language=objc#Life-cycle) <a href="#life-cycle" id="life-cycle"></a>

[Managing your app’s life cycle](https://developer.apple.com/documentation/uikit/managing-your-app-s-life-cycle?language=objc)

&#x20;       Respond to system notifications when your app is in the foreground or background, and handle other significant system-related events.

[Responding to the launch of your app](https://developer.apple.com/documentation/uikit/responding-to-the-launch-of-your-app?language=objc)

&#x20;       Initialize your app’s data structures, prepare your app to run, and respond to any launch-time requests from the system.

[`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication?language=objc)

&#x20;       The centralized point of control and coordination for apps running in iOS.

[Scenes](https://developer.apple.com/documentation/uikit/scenes?language=objc)

&#x20;       Manage multiple instances of your app’s UI simultaneously, and direct resources to the appropriate instance of your UI.
