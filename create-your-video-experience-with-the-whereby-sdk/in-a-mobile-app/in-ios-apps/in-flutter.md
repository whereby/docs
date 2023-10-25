---
description: >-
  Flutter is Google's free and open-source UI framework for creating native
  mobile applications.
---

# Using Flutter

{% hint style="success" %}
Before starting, you will need to add the corresponding permissions to be able to access both the camera and microphone as described in the [Android](../in-android-apps/) and [iOS](./) sections.
{% endhint %}

We recommend using the [flutter\_inappwebview](https://pub.dev/packages/flutter\_inappwebview) and [permission\_handler](https://pub.dev/packages/permission\_handler) modules to handle media permissions in the webview. Update the settings in your iOS and Android projects to match the requirements. Note that there is a [known issue](https://github.com/flutter/flutter/issues/19718) to show the keyboard in Android webviews.

Finally, add the WebView component to your code, setup the properties and fill the room URL and parameters. Here is a short example:

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
                androidOnPermissionRequest: (InAppWebViewController controller,
                    String origin, List<String> resources) async {
                  await Permission.camera.request();
                  await Permission.microphone.request();
                  return PermissionRequestResponse(
                      resources: resources,
                      action: PermissionRequestResponseAction.GRANT);
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
