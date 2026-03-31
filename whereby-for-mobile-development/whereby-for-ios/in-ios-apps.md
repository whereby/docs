# Using a WKWebView

When embedding Whereby in a WebView, you will have access to all features available on the web, directly within your iOS app. While it is relatively simple to build a basic app this way, please note that it comes with certain limitations compared to a native implementation, such as restricted background mode support, additional logic required for file sharing, etc.

## Quickstart guide

This guide shows you how to embed a Whereby room in a native iOS app using the [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) class. After that, it also introduces some customization options to give you an idea of what’s available. For a complete running example, checkout our [iOS WebView demo app](https://github.com/whereby/ios-webview-demo) repository.&#x20;

{% hint style="info" %}
The example built up in this article is based on `Whereby Web`, which provides advanced tools, such as integrations and Breakout Groups. However, we also offer a [native iOS SDK](using-wherebys-native-sdk.md) that allows you to customize the user experience in your iOS app. With the iOS SDK, you can create custom iOS UIButtons and UIViews, send commands to the room, and listen to room events to implement custom callbacks.
{% endhint %}

### Prerequisites

This guide assumes that you’ve already created a Whereby Embedded account and a meeting room. If you’ve not already taken these steps, you can do so by following the [Whereby Embedded initial setup guide](../../getting-started/overview-of-whereby-embedded/initial-setup.md).

When embedding Whereby in a WebView, depending on your implementation, you can use the following types of URLs:

* A Whereby room URL (which takes the user directly to the Whereby pre-call screen).
* A custom web app that embeds Whereby using the Embedded element.
* A React app that embeds Whereby with the React Hooks SDK.

### Setting up the app

Create a new project for a simple app using the latest version of Xcode.

### Setting the required permissions

To get a Whereby instance running on iOS, you need to set a series of permissions — Whereby requires access to the phone's camera, microphone, and photo library (for file sharing).

Declare the required permissions in your `Info.plist` file like so:

```xml
<key>NSCameraUsageDescription</key>
<string>This app requires access to your camera</string>
<key>NSMicrophoneUsageDescription</key>
<string>This app requires access to your microphone</string>
<key>NSPhotoLibraryAddUsageDescription</key>
<string>This app requires access to your photo library to save photos or videos</string>
<key>NSPhotoLibraryUsageDescription</key>
<string>This app requires access to your photo library to view and select photos for uploading or sharing</string>
```

### Embedding Whereby&#x20;

The next step is to set up a [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) class ready for embedding the Whereby instance. This supports embedding pages that use WebRTC from iOS 14.5 onwards.

Replace the initial/main `UIViewController` with the code below and update the URL in the example to your own Whereby room URL or web app URL.

{% tabs %}
{% tab title="Swift" %}
```swift
import UIKit
import WebKit

class ViewController: UIViewController {
    
    private var webView: WKWebView!
    private var url = URL(string: "https://your-whereby-room-URL")!

    override func viewDidLoad() {
        super.viewDidLoad()

        // Create a configuration for the WKWebView:
        let config = WKWebViewConfiguration()
        config.allowsInlineMediaPlayback = true
        config.mediaTypesRequiringUserActionForPlayback = []
        
        // Initialize the WKWebView with the custom configuration and delegate:
        webView = WKWebView(frame: .zero, configuration: config)
        
        // Add the webView to the view hierarchy:
        view.addSubview(webView)
        
        // Apply constraints to the webView
        webView.translatesAutoresizingMaskIntoConstraints = false
        NSLayoutConstraint.activate([
            webView.leadingAnchor.constraint(equalTo: view.safeAreaLayoutGuide.leadingAnchor, constant: 0),
            webView.trailingAnchor.constraint(equalTo: view.safeAreaLayoutGuide.trailingAnchor, constant: 0),
            webView.topAnchor.constraint(equalTo: view.safeAreaLayoutGuide.topAnchor, constant: 0),
            webView.bottomAnchor.constraint(equalTo: view.bottomAnchor, constant: 0)
        ])

        // Finally: load the webView
        webView.load(URLRequest(url: url))
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

***

## Customizing your app

Now that you’ve got a basic app running, you’ll want to customize it to your liking. Whereby has many options for customizing your app:

* Global settings via your account dashboard
* Per-room settings via component [attributes](../../whereby-for-web-browser/web-component-and-pre-built-ui/configuring-with-attributes.md) and [room creation settings](../../whereby-product-features/using-the-rest-api/).

In this section, we’ll go through a couple of options to give you an idea of what’s available.&#x20;

#### Setting custom brand colors

1. In your account dashboard: Go to the **Configure** screen and access the **Appearance** panel.
2. Set new primary, secondary, and focus highlight colors.
3. Set a new background color, and upload a background image.
4. Upload a company logo.
5. Re-run your app to see how these settings change the look and feel.

#### Setting web component attributes

There are many different attributes you can set on the web component to configure various aspects of the video call experience. Let's add some to the room URL:

```swift
private var url = URL(string: "https://your-whereby-room-URL?skipMediaPermissionPrompt&minimal")!
```

The effect of the attributes is as follows:

* `skipMediaPermissionPrompt`: Skip the screen prompting the user to grant permission, as this can be handled directly by the WebView itself.
* `minimal`: Uses the minimal Whereby UI.

Save your code and re-run your app to see the effect of the attributes.

### See also

* [Meeting controls](https://whereby.helpscoutdocs.com/article/458-meeting-controls)
