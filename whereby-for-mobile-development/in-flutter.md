---
description: >-
  Flutter is Google's free and open-source UI framework for creating natively
  compiled cross-platform mobile apps.
---

# Whereby with Flutter

### Prerequisites

Before starting, you will need to add the corresponding permissions to be able to access both the camera and microphone as described in the [Android](whereby-for-android/in-android-apps.md) and [iOS](whereby-for-ios/in-ios-apps.md) sections.

### Getting started <a href="#getting-started" id="getting-started"></a>

We recommend using the [flutter\_inappwebview](https://pub.dev/packages/flutter_inappwebview) and [permission\_handler](https://pub.dev/packages/permission_handler) modules to handle media permissions in the webview. Update the settings in your iOS and Android projects to match the requirements.&#x20;

{% hint style="warning" %}
Note that there is a [known issue](https://github.com/flutter/flutter/issues/19718) to show the keyboard in Android webviews.
{% endhint %}

Add the WebView component to your code, setup the properties, and fill the room URL and parameters.&#x20;

Here is a short example:

{% tabs %}
{% tab title="Dart" %}
```dart
import 'package:flutter/material.dart';
import 'dart:io' show Platform;
import 'package:flutter_inappwebview/flutter_inappwebview.dart';
import 'package:permission_handler/permission_handler.dart';

var _url = ""; // Replace by your own

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    if (Platform.isAndroid) {
      _url += '?skipMediaPermissionPrompt';
    }
    return MaterialApp(
      home: InAppWebViewPage(),
    );
  }
}

class InAppWebViewPage extends StatefulWidget {
  InAppWebViewPage();

  @override
  _InAppWebViewPageState createState() => new _InAppWebViewPageState();
}

class _InAppWebViewPageState extends State<InAppWebViewPage> {
  _InAppWebViewPageState();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(title: Text("Meeting")),
        body: Container(
            child: Column(children: <Widget>[
          Expanded(
            child: Container(
              child: InAppWebView(
                initialUrlRequest: URLRequest(url: Uri.parse(_url)),
                initialOptions: InAppWebViewGroupOptions(
                    crossPlatform: InAppWebViewOptions(
                      mediaPlaybackRequiresUserGesture: false,
                    ),
                    ios: IOSInAppWebViewOptions(
                      allowsInlineMediaPlayback: true,
                    )),
                onPermissionRequest: (controller, permissionRequest) async {
                  await Permission.camera.request();
                  await Permission.microphone.request();
                  return PermissionResponse(
                      resources: permissionRequest.resources,
                      action: PermissionResponseAction.GRANT);
                },
              ),
            ),
          ),
        ])));
  }
}
```
{% endtab %}
{% endtabs %}
