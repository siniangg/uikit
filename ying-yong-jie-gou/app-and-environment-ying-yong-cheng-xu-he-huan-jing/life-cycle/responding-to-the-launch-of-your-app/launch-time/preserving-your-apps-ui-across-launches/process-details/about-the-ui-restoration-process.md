---
description: 了解如何自定义UIKit状态恢复过程。
---

# About the UI restoration process

## Overview <a href="#overview" id="overview"></a>

以下图表展示了从应用启动到恢复期间发生的调用序列。恢复过程发生在应用初始化的中间阶段，并且只有在状态恢复存档可用且应用委托的 [`application:shouldRestoreApplicationState:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:shouldrestoreapplicationstate:\)?language=objc) 方法返回 [`true`](https://developer.apple.com/documentation/swift/true?language=objc) 时才会继续进行。

<figure><img src="https://docs-assets.developer.apple.com/published/8ae9ad9ee6c3db2adb3b3d2ed01aa1a6/media-2934436%402x.png" alt="" width="563"><figcaption></figcaption></figure>

恢复过程的第一步是为你的界面创建视图控制器对象（显式或隐式创建均可）。第二步是解码并恢复这些对象的状态。这两个步骤对于重新创建视图控制器层次结构都是必需的。例如，在创建导航控制器及其子视图控制器之后，这些对象之间不会立即建立连接。实际上，是导航控制器的 [`decodeRestorableStateWithCoder:`](https://developer.apple.com/documentation/uikit/uistaterestoring/decoderestorablestate\(with:\)?language=objc) 方法重新建立了与子视图控制器的关系。

状态恢复完成后，UIKit 会调用应用程序委托的[`application:didFinishLaunchingWithOptions:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:didfinishlaunchingwithoptions:\)?language=objc)方法。使用该方法对界面进行最后一刻的更改或添加。例如，你可以向视图控制器层次结构中添加一个登录屏幕。

### **重新创建你的视图控制器**

在恢复过程中，UIKit 会尝试从保留的界面中创建或定位视图控制器对象。UIKit 首先会要求你提供视图控制器对象。如果你不提供视图控制器，UIKit 会隐式地查找它。以下是 UIKit 重新创建视图控制器所遵循的步骤顺序：

1. 询问视图控制器的恢复类。恢复类知道如何创建特定的视图控制器。你可以通过将某个类赋值给视图控制器的[`restorationClass`](https://developer.apple.com/documentation/uikit/uiviewcontroller/restorationclass?language=objc)属性来指定恢复类。在恢复过程中，UIKit会调用恢复类的 [`viewControllerWithRestorationIdentifierPath:coder:`](https://developer.apple.com/documentation/uikit/uiviewcontrollerrestoration/viewcontroller\(withrestorationidentifierpath:coder:\)?language=objc)方法来请求视图控制器的新实例，然后你的方法返回该实例。如果你返回`nil`，UIKit将停止尝试创建视图控制器，并将其排除在恢复过程之外。
2. 询问应用程序委托。如果视图控制器没有恢复类，UIKit 会调用应用程序委托的[`application:viewControllerWithRestorationIdentifierPath:coder:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:viewcontrollerwithrestorationidentifierpath:coder:\)?language=objc)方法。如果你从该方法返回 nil，UIKit 会继续查找。
3. 检查是否存在现有对象。UIKit 会查找具有完全相同恢复路径的已创建视图控制器。
4. 从故事板实例化视图控制器。如果仍然没有视图控制器，UIKit 会自动从应用的故事板中实例化它。

甚至在状态恢复开始之前，UIKit 就会从故事板中加载应用的默认视图控制器。由于 UIKit 会自动加载这些视图控制器，因此最好不要使用恢复类或应用委托来创建它们。对于所有其他视图控制器，仅当视图控制器未在故事板中定义时，才为其指定恢复类。你也可以指定一个恢复类，以防止在特定情况下创建视图控制器。例如，如果相关的恢复存档引用了过时或缺失的数据，你可能希望避免显示该视图控制器。

在通过代码重新创建视图控制器时，除了进行任何其他初始化操作外，始终要为视图控制器的[`restorationIdentifier`](https://developer.apple.com/documentation/uikit/uiviewcontroller/restorationidentifier?language=objc)属性重新赋值。还要根据情况为[`restorationClass`](https://developer.apple.com/documentation/uikit/uiviewcontroller/restorationclass?language=objc)属性赋值。在创建时设置这些值可确保视图控制器在下一个周期中得以保留。

```
func viewController(withRestorationIdentifierPath 
                    identifierComponents: [Any], 
                    coder: NSCoder) -> UIViewController? {
   let vc = MyViewController()
        
   vc.restorationIdentifier = identifierComponents.last as? String
   vc.restorationClass = MyViewController.self
        
   return vc
}
```

{% hint style="info" %}
**Note**

你的恢复类应该始终返回UIKit所期望的类。恢复存档包含每个保留的视图控制器的类。如果你的恢复类返回一个不同类的实例，UIKit将不会调用该视图控制器的[`decodeRestorableStateWithCoder:`](https://developer.apple.com/documentation/uikit/uiviewcontroller/decoderestorablestate\(with:\)?language=objc)方法。
{% endhint %}
