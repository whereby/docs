# Using Whereby's Native iOS SDK

{% hint style="info" %}
The native iOS SDK is currently in active development. For any comments, suggestions, or questions, please [submit an Issue to the GitHub repo](https://github.com/whereby/ios-sdk/issues), or reach out to our Solutions team at embedded@whereby.com.
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=ZX27LEH_Ut4" %}

The Whereby iOS SDK enables you to integrate Whereby into your iOS app as a native component, rather than a web-based experience. This offers control and customization over the meeting experience, and allows you to hook into powerful features such as listening to room events and sending commands to the room from the host application. Please note that certain features available on web may not yet be supported in the native iOS SDK.&#x20;

## Quickstart guide

Below you'll find some basic guidance to help you get started. But for detailed instructions check out the [SDK documentation on GitHub](https://github.com/whereby/ios-sdk), or play around with our [iOS demo app](https://github.com/whereby/ios-sdk-demo), which showcases the available customization options.

### Prerequisites

This guide assumes that you’ve already created a Whereby Embedded account and a meeting room. If you’ve not already taken these steps, you can do so by following the [Whereby Embedded initial setup guide](/broken/pages/jynvpq8niu7lTvuvEx25).

### Setting up the app

Create a new project for a simple app using the latest version of Xcode.

### Installing the Whereby native iOS SDK

#### Using Swift Package Manager

1. Open your project in Xcode and select File > Add Package Dependencies...
2.  In the Search or Enter Package URL text field, search for `Whereby`  or enter the repository URL:

    ```
    https://github.com/whereby/ios-sdk.git
    ```
3. Add the package

For more details, see [Adding Package Dependencies to Your App](https://developer.apple.com/documentation/xcode/adding-package-dependencies-to-your-app).

#### Using CocoaPods

1.  Add the `WherebySDK` pod to your project's Podfile:

    ```bash
    target 
        ...
        pod 'WherebySDK'
    end
    ```
2.  In the Terminal, navigate to the directory containing your Podfile and run:

    ```bash
    pod install
    ```

For more details, see [Cocoapods Instructions](https://cocoapods.org/pods/Instructions).

### Setting the required permissions <a href="#setting-the-required-permissions" id="setting-the-required-permissions"></a>

To get a Whereby instance running on iOS, you need to set a series of permissions — Whereby requires access to the phone's camera, microphone, and photo library (for file sharing).&#x20;

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

### Embedding Whereby <a href="#embedding-whereby" id="embedding-whereby"></a>

Replace the initial/main `UIViewController` with the code below and update the room URL in the example to your own Whereby room URL.

{% tabs %}
{% tab title="Swift" %}
```swift
import UIKit
import WherebySDK

class ViewController: UIViewController {
    
    private var url = URL(string: "https://your-whereby-room-URL")!

    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
        
        // Configure how to join the room. The only mandatory parameter is the room URL.
        let roomConfig = WherebyRoomConfig(url: url)
        
        // Instantiate the WherebyRoomViewController and set the presentation style.
        let roomViewController = WherebyRoomViewController(config: roomConfig, isPushedInNavigationController: false)
        roomViewController.modalPresentationStyle = .fullScreen

        // Finally, present the WherebyRoomViewController and join the room.
        present(roomViewController, animated: true) {
            roomViewController.join()
        }
    }
}
```
{% endtab %}
{% endtabs %}

Run this simple app on a device to explore the full potential of the Whereby native iOS SDK in action.

***

## Customization

{% hint style="info" %}
Note that URL Parameters are not supported with the Native iOS SDK
{% endhint %}

You can modify the room configuration (`WherebyRoomConfig`) to preselect the media mode (audio-only or video).

You can also customize the `WherebyRoomViewController` to tailor the user experience to your use case. For example, you can:&#x20;

* Show or hide specific UI elements (header, buttons, control bar, etc.).
* Set a background color, like any standard `UIViewController`.
* Send commands such as toggling the microphone or camera if you prefer to implement your own controls.

Additionally, you can set up a delegate (`WherebyRoomViewControllerDelegate`) to listen for events, such as when a participant joins or leaves the room.

For a complete list of supported customization options, check out our [Demo App](https://github.com/whereby/ios-sdk-demo) repository. If there’s a customization feature you’d like to see in the future, please submit an issue for review.
