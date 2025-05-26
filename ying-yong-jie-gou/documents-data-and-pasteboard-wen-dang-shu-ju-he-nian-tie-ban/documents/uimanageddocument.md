---
description: A managed document object that integrates with Core Data.
---

# UIManagedDocument

> Class

```objectivec
@interface UIManagedDocument : UIDocument
```

### [Overview](https://developer.apple.com/documentation/uikit/uimanageddocument?language=objc#overview) <a href="#overview" id="overview"></a>

[`UIManagedDocument`](https://developer.apple.com/documentation/uikit/uimanageddocument?language=objc) is a concrete subclass of [`UIDocument`](https://developer.apple.com/documentation/uikit/uidocument?language=objc). When you initialize a managed document, you specify the URL for the document location. The document object then creates a Core Data stack to use to access the document’s persistent store using a managed object model from the app’s main bundle. [`UIManagedDocument`](https://developer.apple.com/documentation/uikit/uimanageddocument?language=objc) performs all the basic setup you need for Core Data, and in some cases you may use instances of the class directly (without a need to subclass). You can supply configuration options for the creation of the coordinator using [`persistentStoreOptions`](https://developer.apple.com/documentation/uikit/uimanageddocument/persistentstoreoptions?language=objc), and for the model using [`modelConfiguration`](https://developer.apple.com/documentation/uikit/uimanageddocument/modelconfiguration?language=objc). You can also perform additional customization by creating a subclass of [`UIManagedDocument`](https://developer.apple.com/documentation/uikit/uimanageddocument?language=objc):

* Override [`persistentStoreName`](https://developer.apple.com/documentation/uikit/uimanageddocument/persistentstorename?language=objc) to customize the name of the persistent store file inside the document’s file package.
* Override [`managedObjectModel`](https://developer.apple.com/documentation/uikit/uimanageddocument/managedobjectmodel?language=objc) to customize creation of the managed object model.

You do this if, for example, your app supports multiple document types, each of which uses a different model. You want to ensure that the models aren’t merged for each document class.

* Override [`persistentStoreTypeForFileType:`](https://developer.apple.com/documentation/uikit/uimanageddocument/persistentstoretype\(forfiletype:\)?language=objc) to customize the type of persistent store used by a document.
* Override [`configurePersistentStoreCoordinatorForURL:ofType:modelConfiguration:storeOptions:error:`](https://developer.apple.com/documentation/uikit/uimanageddocument/configurepersistentstorecoordinator\(for:oftype:modelconfiguration:storeoptions:\)?language=objc) to customize the loading or creation of a persistent store.

\
