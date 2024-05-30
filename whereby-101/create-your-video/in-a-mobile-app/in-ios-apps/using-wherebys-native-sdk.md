# Using Whereby's Native SDK

{% hint style="info" %}
The native iOS SDK is currently in beta and actively being developed. For any comments, suggestions or questions, please [submit an Issue to the GitHub repo](https://github.com/whereby/ios-sdk/issues), or reach out to our solutions team at embedded@whereby.com.
{% endhint %}

The Whereby iOS SDK enables you to integrate Whereby into your iOS app as a native component, rather than a web-based experience. This offers much more control and customization over the meeting experience, and allows you to hook into powerful features such as listening to room events and sending commands to the room from the host application.&#x20;

## Basic Project Setup

Below you'll find some basic guidance on getting started, but for detailed instructions check out the [SDK documentation on GitHub](https://github.com/whereby/ios-sdk), or play around with our [iOS demo app](https://github.com/whereby/ios-sdk-demo).

***

## Installation

### Using Swift Package Manager

1. Open your project in Xcode and select File > Add Packages...
2.  In the Search or Enter Package URL text field, enter the repository URL:

    ```
    https://github.com/whereby/ios-sdk.git
    ```
3. Click Add Package.
4. In the Choose Package Product window, keep the WherebySDK product selected and click Add Package.

For more details see [Adding Package Dependencies to Your App](https://developer.apple.com/documentation/xcode/adding-package-dependencies-to-your-app).

### Using CocoaPods

1.  Add the following line to your project's Podfile:

    ```
    pod 'WherebySDK'
    ```

    Your Podfile should look like this:

    ```
    platform :ios, '14.0'

    target 'TargetName' do
    use_frameworks!
    pod 'WherebySDK'
    end
    ```
2.  In the Terminal, navigate to the directory containing your Podfile and run:

    ```
    pod install
    ```

### Manual Installation

We recommend using either Swift Package Manager or CocoaPods to install Whereby SDK. Alternatively, it's also possible to add the SDK to your project manually:

1.  Clone this repository:

    ```
    git clone https://github.com/whereby/ios-sdk.git
    ```
2. Copy `WherebySDK.xcframework`, `WebRTC.xcframework`, `mediasoup_client_ios.xcframework` from the repository to your project directory next to your `.xcodeproj` file
3. In Xcode select your project file and then your app's target. In the target settings select the General tab. Drag the newly copied `WherebySDK.xcframework`, `WebRTC.xcframework`, `mediasoup_client_ios.xcframework` frameworks into the Frameworks, Libraries, and Embedded Content section of your target.

## Customization

{% hint style="info" %}
Note that URL Parameters are not supported with the Native iOS SDK
{% endhint %}

You can customize the meeting experience by modifying the `embeddedRoomViewController`, for example:&#x20;

{% code overflow="wrap" %}
```swift
// Set room background color embeddedRoomViewController?.view.backgroundColor = .clear embeddedRoomViewController?.setRoomBackgroundColor(.clear)// Some code
```
{% endcode %}

To see a complete list of the supported customization options check out our [Demo App](https://github.com/whereby/ios-sdk-demo/blob/fa00d4f21646c0bb230fcd8d8163fc5fff8a363a/Demo-SwiftPM/WherebySDKDemo/DemoViewController.swift#L86) repo. If there's customization functionality that you'd like to see in the future please submit an issue for review!
