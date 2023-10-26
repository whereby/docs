---
description: >-
  Below are the recommended approaches to embed a Whereby room depending on the
  iOS version.
---

# In iOS apps

{% hint style="info" %}
We offer native SDKs that allow you to tap into powerful features such as listening to room events and use custom buttons to send commands to the room from your application.\
[Read more](../../whereby-embedded-sdk-beta/#ios-sdk)
{% endhint %}

## iOS 14.5 and onwards

[WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) supports embedding pages that use WebRTC from iOS 14.5 onwards. To access the microphone and camera, it is necessary to add both [NSMicrophoneUsageDescription](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple\_ref/doc/uid/TP40009251-SW25) and [NSCameraUsageDescription](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple\_ref/doc/plist/info/NSCameraUsageDescription) keys to the app’s Info.plist file.

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

## iOS 14.3 and 14.4

For iOS 14.3 and 14.4 use [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) to open a website containing an iframe with its `src` specified as a Whereby room, alongside a custom user interface:

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

## iOS 14.2 and earlier

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
