---
description: 当应用处于前台或后台时，对系统通知做出响应，并处理其他重要的系统相关事件。
icon: list-ul
---

# Managing your app’s life cycle

## [Overview](managing-your-apps-life-cycle.md#overview) <a href="#overview" id="overview"></a>

应用程序的当前状态决定了它在任何时候能做什么和不能做什么。例如，前台应用程序能吸引用户的注意力，因此它在系统资源（包括CPU）上具有优先权。相比之下，后台应用程序必须尽量少做工作，最好是什么都不做，因为它不在屏幕上显示。随着应用程序从一种状态转变为另一种状态，你必须相应地调整其行为。

当你的应用程序状态发生变化时，UIKit会通过调用相应委托对象的方法来通知你：

* 在iOS 13及更高版本中，使用[`UISceneDelegate`](https://developer.apple.com/documentation/uikit/uiscenedelegate?language=objc) 对象来响应生命周期事件。
* 在iOS 12及更早版本中，使用[`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate?language=objc)对象来响应生命周期事件。

> 注意：
>
> 当你在应用中启用**场景支持(scene support)**&#x65F6;，在iOS 13及更高版本中，iOS始终会使用你&#x7684;_**scene delegates**_。在iOS 12及更早版本中，系统会使用你&#x7684;_**app delegate**_。

## [Respond to scene-based life-cycle events](https://developer.apple.com/documentation/uikit/managing-your-app-s-life-cycle?language=objc#Respond-to-scene-based-life-cycle-events) <a href="#respond-to-scene-based-life-cycle-events" id="respond-to-scene-based-life-cycle-events"></a>

UIKit 为每个场景提供单独的**生命周期事件**(**life-cycle events**)。场景(**scene**)表示应用程序的用户界面在设备上运行的一个**实例**(**instance**)。用户可以为每个应用程序创建多个场景(**scene**)，并分别显示和隐藏它们。由于每个场景都有自己的**生命周期**(**life cycle**)，因此每个场景都可以处于不同的执行状态(state of execution)。例如，一个场景可能处于**前台(foreground)**，而其他场景可能处于**后台(background)**&#x6216;**暂停状态**(**suspended**)。

{% hint style="warning" %}
<mark style="color:orange;">I</mark><mark style="color:orange;">**mportant**</mark>

Scene support is an opt-in feature. To enable basic support, add the [`UIApplicationSceneManifest`](https://developer.apple.com/documentation/BundleResources/Information-Property-List/UIApplicationSceneManifest?language=objc) key to your app’s `Info.plist` file as described in [Specifying the scenes your app supports](https://developer.apple.com/documentation/uikit/specifying-the-scenes-your-app-supports?language=objc).

场景支持是一项可选功能。若要启用基本支持，请按照“指定应用支持的场景”中的说明，将UIApplicationSceneManifest键添加到应用的Info.plist文件中。
{% endhint %}

下图展示了场景的状态转换。当用户或系统为你的应用请求一个新场景时，UIKit 会创建该场景并将其置于未附加状态。用户请求的场景会迅速移动到前台，并显示在屏幕上。系统请求的场景通常会移动到后台，以便它可以处理一个事件。例如，系统可能会在后台启动场景以处理位置事件。当有人关闭你的应用程序用户界面时，UIKit 会将相关场景移动到后台状态，最终进入暂停状态。UIKit 可以随时断开处于后台或暂停状态的场景，以回收其资源，使该场景返回到未附加状态。

<div align="center" data-full-width="false"><figure><img src="https://docs-assets.developer.apple.com/published/4caedb3f877d455a204a4a942d39f17b/media-3233330~dark%402x.png" alt="" width="375"><figcaption></figcaption></figure></div>

使用场景转换执行以下任务：

* 当UIKit将一个场景连接到你的应用程序时，配置场景的初始用户界面并加载场景所需的数据。
* 当转换到前台活动状态时，配置你的用户界面并准备与用户进行交互。请参阅[Preparing your UI to run in the foreground](https://developer.apple.com/documentation/uikit/preparing-your-ui-to-run-in-the-foreground?language=objc)。
* 当离开前台活动状态时，保存数据并使应用程序行为安静。请参阅[Preparing your UI to run in the background](https://developer.apple.com/documentation/uikit/preparing-your-ui-to-run-in-the-background?language=objc)。
* 进入后台状态后，完成关键任务，尽可能释放内存，并为应用程序快照做好准备。请参阅[Preparing your UI to run in the background](https://developer.apple.com/documentation/uikit/preparing-your-ui-to-run-in-the-background?language=objc)。
* 在场景断开连接时，清理与该场景相关的所有共享资源。
* 除了与场景相关的事件，你还必须使用[`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate?language=objc)对象来响应应用程序的启动。有关应用程序启动时要执行的操作的信息，请参阅[Responding to the launch of your app](https://developer.apple.com/documentation/uikit/responding-to-the-launch-of-your-app?language=objc)。

## [Respond to app-based life-cycle events](https://developer.apple.com/documentation/uikit/managing-your-app-s-life-cycle?language=objc#Respond-to-app-based-life-cycle-events) <a href="#respond-to-app-based-life-cycle-events" id="respond-to-app-based-life-cycle-events"></a>

在iOS 12及更早版本中，UIKit会将所有生命周期事件传递给[`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate?language=objc)对象。应用程序代理管理应用程序的所有窗口，包括在独立屏幕上显示的窗口。因此，应用程序状态转换会影响应用程序的整个用户界面，包括外部显示器上的内容。

下图展示了与应用程序代理对象相关的状态转换。启动后，系统会根据用户界面是否即将显示在屏幕上，将应用程序置于非活动状态或后台状态。当启动到前台时，系统会自动将应用程序转换到活动状态。在此之后，状态会在活动状态和后台状态之间波动，直到应用程序终止。

<figure><img src="https://docs-assets.developer.apple.com/published/c77f342d24483a444994fe138737aed0/media-3233333~dark%402x.png" alt="" width="375"><figcaption></figcaption></figure>

使用应用程序转换执行以下任务：

* 启动时，初始化应用程序的数据结构和用户界面。请参阅[Responding to the launch of your app](https://developer.apple.com/documentation/uikit/responding-to-the-launch-of-your-app?language=objc)。
* 激活时，完成用户界面的配置并准备与用户进行交互。 See [Preparing your UI to run in the foreground](https://developer.apple.com/documentation/uikit/preparing-your-ui-to-run-in-the-foreground?language=objc).
* 停用应用程序时，保存数据并使应用程序行为安静。 See [Preparing your UI to run in the background](https://developer.apple.com/documentation/uikit/preparing-your-ui-to-run-in-the-background?language=objc).
* 进入后台状态后，完成关键任务，尽可能释放内存，并为应用程序快照做好准备。 See [Preparing your UI to run in the background](https://developer.apple.com/documentation/uikit/preparing-your-ui-to-run-in-the-background?language=objc).
* 终止时，立即停止所有工作并释放任何共享资源。See [`applicationWillTerminate:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationwillterminate\(_:\)?language=objc).

## [Respond to other significant events](https://developer.apple.com/documentation/uikit/managing-your-app-s-life-cycle?language=objc#Respond-to-other-significant-events) <a href="#respond-to-other-significant-events" id="respond-to-other-significant-events"></a>

除了处理生命周期事件，应用程序还必须准备好处理下表中列出的事件。大多数情况下，使用 [`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate?language=objc) 对象来处理这些事件。在某些情况下，你也可以使用**通知(notifications)**&#x6765;处理它们，这样你就可以从应用程序的其他部分进行响应。

| Event事件                                                    | Response响应                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ---------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Memory warnings内存警告                                        | Received when your app’s memory usage is too high. Reduce the amount of memory your app uses; see [Responding to memory warnings](https://developer.apple.com/documentation/uikit/responding-to-memory-warnings?language=objc).                                                                                                                                                                                                                 |
| Protected data becomes available/unavailable受保护的数据变为可用/不可用 | Received when someone locks or unlocks their device. See [`applicationProtectedDataDidBecomeAvailable:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationprotecteddatadidbecomeavailable\(_:\)?language=objc) and [`applicationProtectedDataWillBecomeUnavailable:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationprotecteddatawillbecomeunavailable\(_:\)?language=objc). |
| Handoff tasks交接任务                                          | Received when an [`NSUserActivity`](https://developer.apple.com/documentation/Foundation/NSUserActivity?language=objc) object needs to be processed. See [`application:didUpdateUserActivity:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:didupdate:\)?language=objc).                                                                                                                               |
| Time changes时间变化                                           | Received for several different time changes, such as when the phone carrier sends a time update. See [`applicationSignificantTimeChange:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/applicationsignificanttimechange\(_:\)?language=objc).                                                                                                                                                                         |
| Open URLs打开网址                                              | Received when your app needs to open a resource. See [`application:openURL:options:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:open:options:\)?language=objc).                                                                                                                                                                                                                                      |

***

## [Topics](https://developer.apple.com/documentation/uikit/managing-your-app-s-life-cycle?language=objc#topics) <a href="#topics" id="topics"></a>

### [Behavioral events](https://developer.apple.com/documentation/uikit/managing-your-app-s-life-cycle?language=objc#Behavioral-events) <a href="#behavioral-events" id="behavioral-events"></a>

📄[Responding to memory warnings](https://developer.apple.com/documentation/uikit/responding-to-memory-warnings?language=objc)

&#x20;   当系统要求释放内存时，就进行释放。

***

## [See Also](https://developer.apple.com/documentation/uikit/managing-your-app-s-life-cycle?language=objc#see-also) <a href="#see-also" id="see-also"></a>

### [Life cycle](https://developer.apple.com/documentation/uikit/managing-your-app-s-life-cycle?language=objc#Life-cycle) <a href="#life-cycle" id="life-cycle"></a>

[Responding to the launch of your app](https://developer.apple.com/documentation/uikit/responding-to-the-launch-of-your-app?language=objc)

初始化应用程序的数据结构，为应用程序的运行做好准备，并响应系统在启动时发出的任何请求。

[UIApplication](https://developer.apple.com/documentation/uikit/uiapplication?language=objc)

iOS中运行的应用程序的集中控制与协调点。

[UIApplicationDelegate](https://developer.apple.com/documentation/uikit/uiapplicationdelegate?language=objc)

一组用于管理应用程序共享行为的方法。

[Scenes](https://developer.apple.com/documentation/uikit/scenes?language=objc)

同时管理应用程序用户界面的多个实例，并将资源定向到相应的用户界面实例。
