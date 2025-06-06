---
icon: file-lines
---

# Performing one-time setup for your app

## Overview

当用户首次启动您的应用时，您可能需要通过执行一些一次性任务来准备应用环境。例如，您可能想要：

* 从您的服务器下载所需数据。
* 将文档模板或可修改的数据文件从您的应用程序包复制到可写入目录。
* 为用户配置默认首选项。
* 设置用户账户或收集其他所需数据。

在应用程序委托的 [`application:willFinishLaunchingWithOptions:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:willfinishlaunchingwithoptions:\)?language=objc) 或 [`application:didFinishLaunchingWithOptions:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:didfinishlaunchingwithoptions:\)?language=objc) 方法中执行任何一次性任务。对于不需要用户输入的任务，切勿阻塞应用程序的主线程。相反，使用调度队列异步启动任务，并让它们在应用程序完成启动时在后台运行。对于需要用户输入的任务，在 [`application:didFinishLaunchingWithOptions:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/application\(_:didfinishlaunchingwithoptions:\)?language=objc)  方法中对用户界面进行所有更改。

### 将文件安装到合适的位置

您的应用有自己的容器目录来存储文件，并且您应始终将特定于应用的文件放在 \~/Library 子目录中。具体来说，将您的文件存储在以下 \~/Library 子目录中：

* \~/Library/Application Support/ — 存储您希望与用户的其他内容一起备份的特定于应用的文件。（您可以根据需要在此处创建自定义子目录。）将此目录用于数据文件、配置文件、文档模板等。
* \~/Library/Caches/ — 存储可轻松重新生成或下载的临时数据文件。

要获取应用容器中某个目录的URL，请使用[`NSFileManager`](https://developer.apple.com/documentation/Foundation/FileManager?language=objc)的[`URLsForDirectory:inDomains:`](https://developer.apple.com/documentation/foundation/filemanager/1407726-urls?language=objc) 方法。

```objectivec
let appSupportURL = FileManager.default.urls(for: 
      .applicationSupportDirectory, in: .userDomainMask)


let cachesURL = FileManager.default.urls(for: 
      .cachesDirectory, in: .userDomainMask)
```

将任何临时文件放在您应用程序的tmp/目录中。临时文件可能包括压缩文件，一旦其内容被解压并安装到其他位置，您就打算删除这些压缩文件。使用[`NSFileManager`](https://developer.apple.com/documentation/Foundation/FileManager?language=objc)的[`temporaryDirectory`](https://developer.apple.com/documentation/foundation/filemanager/1642996-temporarydirectory?language=objc)方法来获取您应用程序临时目录的URL。

***
