---
description: 在系统终止应用后，将其恢复到之前的状态。
---

# Preserving your app’s UI across launches

## Overview

保留应用程序的用户界面有助于维持应用程序始终在运行的假象。在iOS设备上，中断情况可能频繁发生，长时间的中断可能会导致系统终止您的应用程序以释放资源。然而，用户并不知道您的应用程序已被终止，也不会期望应用程序的状态发生变化。相反，他们期望应用程序保持与他们离开时相同的状态。状态保存和恢复可确保您的应用程序在再次启动时恢复到先前的状态。

在适当的时候，UIKit 会将应用程序视图和视图控制器的状态保存到磁盘上的一个加密文件中。当应用程序终止并在稍后重新启动时，UIKit 会根据保存的数据重建视图和视图控制器。保存和恢复过程会自动启动，但你还必须进行一些特定的工作来支持这些过程：

* 启用对状态保存和恢复的支持。
* 为你想要保留的视图控制器分配恢复标识符。
* 在恢复时，根据需要重新创建视图控制器。
* 对恢复视图控制器到其先前状态所需的自定义数据进行编码和解码。

如果您完全在故事板中定义界面，UIKit 便知道如何重新创建视图控制器，并且会自动执行此操作。如果您不使用故事板，或者希望对视图控制器的创建和初始化有更多控制权，则可以自行创建它们。

要查看状态保留和恢复的示例，请参阅[Restoring your app’s state](https://developer.apple.com/documentation/uikit/restoring-your-app-s-state?language=objc)。

### 为你的应用启用状态保留和恢复功能(Enable state preservation and restoration for your app) <a href="#enable-state-preservation-and-restoration-for-your-app" id="enable-state-preservation-and-restoration-for-your-app"></a>

通过实现应用程序代理的[`application:shouldSaveSecureApplicationState:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:shouldsavesecureapplicationstate:\)?language=objc)和 [`application:shouldRestoreSecureApplicationState:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:shouldrestoresecureapplicationstate:\)?language=objc)方法，您可以选择加入状态保留和恢复功能。这两个方法都返回一个布尔值，指示相关进程是否应该发生，在大多数情况下，您只需返回[`true`](https://developer.apple.com/documentation/swift/true?language=objc)。但是，在恢复应用程序界面可能不合适的时候，您可以返回[`false`](https://developer.apple.com/documentation/swift/false?language=objc)。

当UIKit调用你的 [`application:shouldSaveSecureApplicationState:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:shouldsavesecureapplicationstate:\)?language=objc)方法时，除了返回[`true`](https://developer.apple.com/documentation/swift/true?language=objc)之外，你还可以保存数据。你可能会保存打算在恢复过程中使用的数据。例如，以下代码展示了一个保存应用当前版本号的示例。在恢复时，[`application:shouldRestoreSecureApplicationState:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:shouldrestoresecureapplicationstate:\)?language=objc)方法会检查归档文件中的版本号，如果与预期版本不匹配，则阻止恢复操作。

```
func application(_ application: UIApplication, 
            shouldSaveSecureApplicationState coder: NSCoder) -> Bool {
   // Save the current app version to the archive.
   coder.encode(11.0, forKey: "MyAppVersion")
        
   // Always save state information.
   return true
}
    
func application(_ application: UIApplication, 
            shouldRestoreSecureApplicationState coder: NSCoder) -> Bool {
   // Restore the state only if the app version matches.
   let version = coder.decodeFloat(forKey: "MyAppVersion")
   if version == 11.0 {
      return true
   }
    
   // Don't restore from old data.    
   return false
}
```

如果阻止恢复操作发生，你仍可以在应用程序委托的[`application:didFinishLaunchingWithOptions:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:didfinishlaunchingwithoptions:\)?language=objc)方法中手动配置应用程序的界面。

### 为你的视图控制器分配恢复标识符(Assign restoration identifiers to your view controllers) <a href="#assign-restoration-identifiers-to-your-view-controllers" id="assign-restoration-identifiers-to-your-view-controllers"></a>

通过为视图控制器分配恢复标识符，您可以明确告知UIKit要保留哪些视图控制器。恢复标识符是一个唯一的字符串，您可以通过编程方式或在Interface Builder中为视图控制器分配该标识符。视图控制器类的名称通常是一个合适的恢复标识符，但您也可以使用任何字符串。在故事板文件中将该字符串添加到视图控制器，或者在运行时将其分配给视图控制器的[`restorationIdentifier`](https://developer.apple.com/documentation/uikit/uiviewcontroller/restorationidentifier?language=objc)属性。

<figure><img src="https://docs-assets.developer.apple.com/published/25397412f3e6dcacf2f7f99ff64bf48e/media-2928946%402x.png" alt="" width="563"><figcaption></figcaption></figure>

在保存状态时，UIKit 会尝试保留应用程序窗口的根视图控制器。对于每个带有恢复标识符的根视图控制器，UIKit 会要求该视图控制器将其自定义数据编码到一个归档文件中。容器视图控制器可以将对其子视图控制器的引用作为其自定义数据的一部分进行编码。如果它这样做了，并且这些子视图控制器也有恢复标识符，UIKit 会尝试保留子视图控制器及其内容。这个过程会递归进行，沿着从一个视图控制器到下一个视图控制器的连接，直到所有视图控制器都被保存或被忽略。

你无需为每个视图控制器分配恢复标识符。事实上，有时你可能并不想保留所有视图控制器。例如，如果你的应用显示一个临时登录屏幕，你可能不希望保留该屏幕。相反，你会希望在恢复时决定是否显示它。在这种情况下，你不会为登录屏幕的视图控制器分配恢复标识符。

有关保留内容的详细信息，请参阅 [About the UI preservation process](https://developer.apple.com/documentation/uikit/about-the-ui-preservation-process?language=objc).

### 为你的应用程序编码和解码自定义信息(Encode and decode custom information for your app)

在保存过程中，UIKit 会调用每个要保存的视图和视图控制器的[`encodeRestorableStateWithCoder:`](https://developer.apple.com/documentation/uikit/uistaterestoring/encoderestorablestate\(with:\)?language=objc)方法。使用此方法来保存将视图或视图控制器恢复到当前状态所需的信息。

* 一定要保存视图和控件视觉状态的详细信息。(**Do** save details about the visual state of views and controls.)
* 一定要保存你也希望保留的子视图控制器的引用。(**Do** save references to child view controllers that you also want to preserve.)
* 一定要保存即便丢弃也不影响用户数据的信息。(**Do** save information that can be discarded without affecting the user’s data.)
* 不要包含已存在于应用持久存储中的数据。相反，应包含一个标识符，以便日后用于定位该数据。(**Don’t** include data that’s already in your app’s persistent storage. Instead, include an identifier that you can use to locate that data later.)

状态保留并不能替代将应用程序的数据保存到磁盘。UIKit 可以自行决定丢弃状态保留数据，使您的应用程序恢复到默认状态。使用保留过程来存储有关应用程序用户界面状态的信息，例如表格中当前选定的行。不要用它来存储表格中包含的数据。

以下代码展示了一个视图控制器的示例，该视图控制器包含用于收集名字和姓氏的文本字段。如果其中一个文本字段包含未保存的值，该方法会保存未保存的值以及包含该值的文本字段的标识符。在这种情况下，未保存的值不属于应用程序的持久数据；它是一个临时值，必要时可以丢弃。

```
override func encodeRestorableState(with coder: NSCoder) {
   super.encodeRestorableState(with: coder)
        
   // Save the user ID so that we can load that user later.
   coder.encode(userID, forKey: "UserID")


   // Write out any temporary data if editing is in progress.
   if firstNameField!.isFirstResponder {
      coder.encode(firstNameField?.text, forKey: "EditedText")
      coder.encode(Int32(1), forKey: "EditField")
   }
   else if lastNameField!.isFirstResponder {
      coder.encode(lastNameField?.text, forKey: "EditedText")
      coder.encode(Int32(2), forKey: "EditField")
   }
   else {
      // No editing was in progress.
      coder.encode(Int32(0), forKey: "EditField")
   }
}
```

有关UIKit如何保留您应用的视图、视图控制器和状态信息的更多信息，请参阅“关于UI保留过程”。see [About the UI preservation process](https://developer.apple.com/documentation/uikit/about-the-ui-preservation-process?language=objc).

### 接到请求时创建视图控制器(Create view controllers when asked) <a href="#create-view-controllers-when-asked" id="create-view-controllers-when-asked"></a>

如果在启动您的应用时存在保留的状态信息，系统会尝试使用保留的数据恢复您应用的界面。

1. UIKit会调用应用程序代理的[`application:shouldRestoreSecureApplicationState:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:shouldrestoresecureapplicationstate:\)?language=objc)方法，以确定是否应继续进行恢复操作。
2. UIKit会使用应用程序的故事板来重新创建视图控制器。
3. UIKit会调用每个视图控制器的[`decodeRestorableStateWithCoder:`](https://developer.apple.com/documentation/uikit/uistaterestoring/decoderestorablestate\(with:\)?language=objc)方法来恢复其状态信息。

最初，UIKit 会从您的故事板中加载视图控制器及其视图。在加载并初始化这些对象后，UIKit 便开始恢复它们的状态信息。使用[`decodeRestorableStateWithCoder:`](https://developer.apple.com/documentation/uikit/uistaterestoring/decoderestorablestate\(with:\)?language=objc)方法将视图控制器恢复到先前的状态。

以下代码展示了对上一示例中编码的状态进行解码的方法。此方法从保留的用户ID中恢复视图控制器的数据。如果某个文本字段正在编辑，此方法还会恢复进行中的值，并使相应的文本字段成为第一响应者，从而为该文本字段显示键盘。

```
override func decodeRestorableState(with coder: NSCoder) {
   super.decodeRestorableState(with: coder)
   
   // Restore the first name and last name from the user ID.
   let identifier = coder.decodeObject(forKey: "UserID") as! String
   setUserID(identifier: identifier)


   // Restore an in-progress values that was not saved.
   let activeField = coder.decodeInteger(forKey: "EditField")
   let editedText = coder.decodeObject(forKey: "EditedText") as! 
                         String?


   switch activeField {
      case 1:
         firstNameField?.text = editedText
         firstNameField?.becomeFirstResponder()
         break
            
      case 2:
         lastNameField?.text = editedText
         lastNameField?.becomeFirstResponder()
         break
            
     default:
         break  // Do nothing.
  }
}
```

在故事板中定义视图控制器是管理状态恢复最简单的方法，但这并非唯一的方法。有关重新创建视图控制器的其他方法的更多信息，请参阅“关于UI恢复过程”。see [About the UI restoration process](https://developer.apple.com/documentation/uikit/about-the-ui-restoration-process?language=objc).
