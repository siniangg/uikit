---
description: Create a version of your iPad app that users can run on a Mac device.
icon: list-ul
---

# Mac Catalyst (Mac 催化剂)

> API Collection

## [Overview](mac-catalyst-mac-cui-hua-ji.md#overview) <a href="#overview" id="overview"></a>

With Mac Catalyst, you can make a Mac version of your iPad app. Click the Mac checkbox in your iPad app’s project settings to configure the project to build both Mac and iPad versions of your app. The two apps share the same project and source code, making it easy to change your code in one place.

<figure><img src="https://docs-assets.developer.apple.com/published/2f984659cf444e400663090fbc7851db/media-3671148%402x.png" alt=""><figcaption></figcaption></figure>

For information about designing a Mac version of your iPad app, see [Mac Catalyst](https://developer.apple.com/design/human-interface-guidelines/ios/overview/mac-catalyst/) in the Human Interface Guidelines.

{% hint style="info" %}
<mark style="color:orange;">**Important**</mark>

Mac apps built with Mac Catalyst can only use [AppKit](https://developer.apple.com/documentation/AppKit?language=objc) APIs marked as available in Mac Catalyst, such as [`NSToolbar`](https://developer.apple.com/documentation/AppKit/NSToolbar?language=objc) and [`NSTouchBar`](https://developer.apple.com/documentation/AppKit/NSTouchBar?language=objc). Mac Catalyst doesn’t support accessing unavailable AppKit APIs.
{% endhint %}

## [Topics](mac-catalyst-mac-cui-hua-ji.md#topics) <a href="#topics" id="topics"></a>

## [Essentials](mac-catalyst-mac-cui-hua-ji.md#essentials) <a href="#essentials" id="essentials"></a>

📄[Creating a Mac version of your iPad app](https://developer.apple.com/documentation/uikit/creating-a-mac-version-of-your-ipad-app?language=objc)

&#x20;   Bring your iPad app to macOS with Mac Catalyst.

## [App support](https://developer.apple.com/documentation/uikit/mac-catalyst?language=objc#App-support) <a href="#app-support" id="app-support"></a>

[Bring an iPad App to the Mac with Mac Catalyst](https://developer.apple.com/tutorials/Mac-Catalyst?language=objc)

Build a native Mac app from the same codebase as your iPad app.

[Choosing a user interface idiom for your Mac app](https://developer.apple.com/documentation/uikit/choosing-a-user-interface-idiom-for-your-mac-app?language=objc)

Select the iPad or the Mac user interface idiom in your Mac app built with Mac Catalyst.

[Optimizing your iPad app for Mac](https://developer.apple.com/documentation/uikit/optimizing-your-ipad-app-for-mac?language=objc)

Make your iPad app more like a Mac app by taking advantage of system features in macOS.

[`LSMinimumSystemVersion`](https://developer.apple.com/documentation/BundleResources/Information-Property-List/LSMinimumSystemVersion?language=objc)

The minimum version of the operating system required for the app to run in macOS.

[`UIApplicationSupportsTabbedSceneCollection`](https://developer.apple.com/documentation/BundleResources/Information-Property-List/UIApplicationSceneManifest/UIApplicationSupportsTabbedSceneCollection?language=objc)

A Boolean value indicating whether an app built with Mac Catalyst supports automatic tabbing mode.

## [User interface](https://developer.apple.com/documentation/uikit/mac-catalyst?language=objc#User-interface) <a href="#user-interface" id="user-interface"></a>

[UIKit Catalog: Creating and customizing views and controls](https://developer.apple.com/documentation/uikit/uikit-catalog-creating-and-customizing-views-and-controls?language=objc)

Customize your app’s user interface with views and controls.

[Building and improving your app with Mac Catalyst](https://developer.apple.com/documentation/uikit/building-and-improving-your-app-with-mac-catalyst?language=objc)

Improve your iPadOS app with Mac Catalyst by supporting native controls, multiple windows, sharing, printing, menus and keyboard shortcuts.

[Displaying a checkbox in your Mac app built with Mac Catalyst](https://developer.apple.com/documentation/uikit/displaying-a-checkbox-in-your-mac-app-built-with-mac-catalyst?language=objc)

Present a switch control as a Mac-style checkbox when your app runs in the Mac user interface idiom.

[Removing the title bar in your Mac app built with Mac Catalyst](https://developer.apple.com/documentation/uikit/removing-the-title-bar-in-your-mac-app-built-with-mac-catalyst?language=objc)

Display content that fills the entire height of a window by removing the title bar.

[Toolbar](https://developer.apple.com/documentation/uikit/toolbar?language=objc)

Provide a space for controls under a window’s title bar and above your custom content.

[Touch Bar](https://developer.apple.com/documentation/AppKit/touch-bar?language=objc)

Display interactive content and controls in the Touch Bar.

## [User interactions](https://developer.apple.com/documentation/uikit/mac-catalyst?language=objc#User-interactions) <a href="#user-interactions" id="user-interactions"></a>

[Navigating an app’s user interface using a keyboard](https://developer.apple.com/documentation/uikit/navigating-an-app-s-user-interface-using-a-keyboard?language=objc)

Navigate between user interface elements using a keyboard and focusable UI elements in iPad apps and apps built with Mac Catalyst.

[Adding menus and shortcuts to the menu bar and user interface](https://developer.apple.com/documentation/uikit/adding-menus-and-shortcuts-to-the-menu-bar-and-user-interface?language=objc)

Provide quick access to useful actions by adding menus and keyboard shortcuts to your Mac app built with Mac Catalyst.

[Handling key presses made on a physical keyboard](https://developer.apple.com/documentation/uikit/handling-key-presses-made-on-a-physical-keyboard?language=objc)

Detect when someone presses and releases keys on a physical keyboard.

[UIHoverGestureRecognizer](https://developer.apple.com/documentation/uikit/uihovergesturerecognizer?language=objc)

A continuous gesture recognizer that interprets pointer movement over a view.

## [User preferences](https://developer.apple.com/documentation/uikit/mac-catalyst?language=objc#User-preferences) <a href="#user-preferences" id="user-preferences"></a>

[Displaying a Preferences window](https://developer.apple.com/documentation/uikit/displaying-a-preferences-window?language=objc)

Provide a Preferences window in your Mac app built with Mac Catalyst so users can manage app preferences defined in a Settings bundle.

[Detecting changes in the preferences window](https://developer.apple.com/documentation/uikit/detecting-changes-in-the-preferences-window?language=objc)

Listen for and respond to a user’s preference changes in your Mac app built with Mac Catalyst using Combine.

## [Tooltips](https://developer.apple.com/documentation/uikit/mac-catalyst?language=objc#Tooltips) <a href="#tooltips" id="tooltips"></a>

[Showing help tags for views and controls using tooltip interactions](https://developer.apple.com/documentation/uikit/showing-help-tags-for-views-and-controls-using-tooltip-interactions?language=objc)

Explain the purpose of interface elements by showing a tooltip when a person positions the pointer over the element.

[UIToolTipInteraction](https://developer.apple.com/documentation/uikit/uitooltipinteraction?language=objc)

An interaction object that makes it possible to show a tooltip when hovering a pointer over a view or control.

[UIToolTipInteractionDelegate](https://developer.apple.com/documentation/uikit/uitooltipinteractiondelegate?language=objc)

An interface that provides tooltip settings to an interaction.

## [See Also](https://developer.apple.com/documentation/uikit/mac-catalyst?language=objc#see-also) <a href="#see-also" id="see-also"></a>

## [App structure](https://developer.apple.com/documentation/uikit/mac-catalyst?language=objc#App-structure) <a href="#app-structure" id="app-structure"></a>

[App and environment](https://developer.apple.com/documentation/uikit/app-and-environment?language=objc)

Manage life-cycle events and your app’s UI scenes, and get information about traits and the environment in which your app runs.

[Documents, data, and pasteboard](https://developer.apple.com/documentation/uikit/documents-data-and-pasteboard?language=objc)

Organize your app’s data and share that data on the pasteboard.

[Resource management](https://developer.apple.com/documentation/uikit/resource-management?language=objc)

Manage the images, strings, storyboards, and nib files that you use to implement your app’s interface.

[App extensions](https://developer.apple.com/documentation/uikit/app-extensions?language=objc)

Extend your app’s basic functionality to other parts of the system.

[Interprocess communication](https://developer.apple.com/documentation/uikit/interprocess-communication?language=objc)

Display activity-based services to people.



