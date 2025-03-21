---
description: >-
  Below are the recommended approaches to embed Whereby in a native iOS app
  using a WebView.
---

# Embedding Whereby in iOS

WebViews are powerful built-in components that allow loading URLs within a native app. When embedding Whereby, depending on your implementation, you can use the following types of URLs:

* A Whereby room URL (which takes the user directly to the Whereby pre-call screen).
* A custom web app that embeds Whereby using the Embedded element.
* A React app that embeds Whereby with the React Hooks SDK.

{% hint style="info" %}
This solution is based on `Whereby Web`, which provides advanced tools such as integrations and breakout groups. However, we also offer a native iOS SDK that allows you to customize the user experience in your iOS app. With the iOS SDK, you can create custom iOS UIButtons and UIViews, send commands to the room, and listen to room events to implement custom callbacks.\
[Read more](using-wherebys-native-sdk.md)
{% endhint %}

## Permissions

Ensure the following permissions are declared in your `Info.plist` file:

* **NSMicrophoneUsageDescription** - [Microphone Access](https://developer.apple.com/documentation/bundleresources/information_property_list/nsmicrophoneusagedescription)
* **NSCameraUsageDescription** - [Camera Access](https://developer.apple.com/documentation/bundleresources/information_property_list/nscamerausagedescription)
* **NSPhotoLibraryUsageDescription** - [Photo Library Access](https://developer.apple.com/documentation/bundleresources/information_property_list/nsphotolibraryusagedescription) (for file sharing)
* **NSPhotoLibraryAddUsageDescription** - [Photo Library Additions](https://developer.apple.com/documentation/bundleresources/information_property_list/nsphotolibraryaddusagedescription) (for file sharing)

### WKWebView (WebKit)

[WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) supports embedding pages that use WebRTC from iOS 14.5 onwards. Below is a basic example of how to implement a WKWebView.

{% tabs %}
{% tab title="Swift" %}
```swift
import WebKit

class WKWebViewController: UIViewController, WKNavigationDelegate {

    public var roomUrlString = "" // Replace by your own
    private var webView: WKWebView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        let config = WKWebViewConfiguration()
        config.allowsInlineMediaPlayback = true
        webView = WKWebView(frame: view.frame, configuration: config)
        webView.navigationDelegate = self
        view = webView
        guard let roomUrl = URL(string: roomUrlString) else {
            return
        }
        webView.load(URLRequest(url: roomUrl))
    }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
This is a minimal implementation that sticks to the default behavior of WKWebView. It is recommended to extend the configuration by implementing delegate methods to improve the user experience. For example, you can:

* Avoid repeated media permission prompts.
* Enable and manage file sharing.
* Handle external link redirections.

For a complete example, check out our public repository: [ios-webview-demo](https://github.com/whereby/ios-webview-demo).
{% endhint %}

## SFSafariViewController

For iOS 14.3 and 14.4, use [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) to open a website containing an iframe with its `src` specified as a Whereby room, alongside a custom user interface:

{% tabs %}
{% tab title="Swift" %}
```swift
import SafariServices

class ViewController: UIViewController, SFSafariViewControllerDelegate {

    public var roomUrlString = "" // Replace by your own
    
    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
        guard let roomUrl = URL(string: roomUrlString) else {
            return
        }
        let safariVC = SFSafariViewController(url: roomUrl)
        safariVC.delegate = self
        present(safariVC, animated: true)
    }
}
```
{% endtab %}
{% endtabs %}

## Redirect to browser

Redirect to a browser (Safari by default) for iOS versions lower than 14.3:

{% tabs %}
{% tab title="Swift" %}
```swift
import UIKit

class ViewController: UIViewController {

    public var roomUrlString = "" // Replace by your own
    
    override func viewDidLoad() {
        super.viewDidLoad()
        guard let roomUrl = URL(string: roomUrlString),
            UIApplication.shared.canOpenURL(roomUrl) else {
            return
        }
        UIApplication.shared.open(roomUrl)
    }
}sw
```
{% endtab %}
{% endtabs %}

## Handling multiple iOS versions

Here is an example on how to handle different solutions, depending on the iOS version:

{% tabs %}
{% tab title="Swift" %}
```swift
if #available(iOS 14.5, *) {
    // Use WKWebView
} else if #available(iOS 14.3, *) {
    // Use SFSafariViewController
} else {
    // Redirect to browser app
}
```
{% endtab %}
{% endtabs %}

When the app is sent to background, the camera is disabled. If you need the microphone to continue working while the app is in the background, we recommend redirecting to Safari app.

To use Whereby with Cordova (Phonegap) please use the plugin for [SafariViewController](https://github.com/EddyVerbruggen/cordova-plugin-safariviewcontroller)
