---
description: 了解如何自定义UIKit状态保存流程。
---

# About the UI preservation process

## Overview <a href="#overview" id="overview"></a>

以下图表展示了界面保留过程中发生的调用顺序。在询问应用程序委托是否要保留应用程序状态后，UIKit 会对应用程序视图控制器层级结构中当前的对象进行编码。只有具有有效[`restorationIdentifier`](https://developer.apple.com/documentation/uikit/uiviewcontroller/restorationidentifier?language=objc)的视图控制器才会被保留。

<figure><img src="https://docs-assets.developer.apple.com/published/efccc1e96d24bdb1968ccf71485a4d33/media-2928966%402x.png" alt="" width="563"><figcaption></figcaption></figure>

状态保留过程会遍历视图控制器层级结构，并递归编码找到的对象。该过程从应用程序窗口的根视图控制器开始，这些根视图控制器会将其数据写入提供的存档中。如果根视图控制器的数据包含对其他视图控制器的引用，UIKit 会要求每个新的视图控制器在存档的不同部分中编码其数据。这些子视图控制器随后可能会编码它们自己的子视图控制器，依此类推。

UIKit视图控制器会在适当的时候自动对其 child 视图控制器进行编码。如果你定义了一个自定义容器视图控制器，那么你的视图控制器的[`encodeRestorableStateWithCoder:`](https://developer.apple.com/documentation/uikit/uistaterestoring/encoderestorablestate\(with:\)?language=objc)方法也必须同样地将任何子视图控制器写入到提供的存档中。

### 从状态保存中排除视图控制器

有两种方法可以将视图控制器（及其视图）从状态恢复过程中排除：

* 将其[`restorationIdentifier`](https://developer.apple.com/documentation/uikit/uiviewcontroller/restorationidentifier?language=objc)属性设置为nil。
* 提供一个恢复类，并从[`viewControllerWithRestorationIdentifierPath:coder:`](https://developer.apple.com/documentation/uikit/uiviewcontrollerrestoration/viewcontroller\(withrestorationidentifierpath:coder:\)?language=objc)方法返回nil。

排除一个视图控制器会阻止该视图控制器保存在存档中。这也会排除该视图控制器的所有子视图控制器，使其不会被保留。

### 对应用程序中的任何对象进行编码

状态恢复不仅限于应用的视图和视图控制器。任何采用[`UIStateRestoring`](https://developer.apple.com/documentation/uikit/uistaterestoring?language=objc)协议的对象也可以包含在恢复归档中。例如，您可能会在存储应用全局配置数据的对象中采用此协议。要将此类对象添加到归档中：

1. 在应用运行时，通过调用[`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication?language=objc)的[`registerObjectForStateRestoration:restorationIdentifier:`](https://developer.apple.com/documentation/uikit/uiapplication/registerobject\(forstaterestoration:restorationidentifier:\)?language=objc)方法来注册对象。例如，你可以在创建配置对象后立即注册它。
2. 在你的某个 [`encodeRestorableStateWithCoder:`](https://developer.apple.com/documentation/uikit/uistaterestoring/encoderestorablestate\(with:\)?language=objc)方法中，将对象编码到恢复存档中。你也可以在应用程序委托的[`application:willEncodeRestorableStateWithCoder:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:willencoderestorablestatewith:\)?language=objc)方法中对其进行编码。

你可以为自定义对象编码任何所需的数据，只要这些数据足以在下次启动周期中将该对象恢复到之前的状态。编码那些对应用行为并非至关重要的数据，并且绝不要编码应以其他方式持久化的数据。例如，不要编码应用的设置或必须在多次启动周期之间保持不变的用户数据。

\
