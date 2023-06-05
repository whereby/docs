---
description: >-
  If you’re using React Native, you can embed Whereby rooms with just a few
  simple steps as laid out in this guide.
---

# In React Native

Before starting, you will need to add the corresponding permissions to be able to access both the camera and microphone as described in the [Android](in-android-apps.md) and [iOS](in-ios-apps.md) sections.

Follow this [guide](https://github.com/react-native-webview/react-native-webview/blob/master/docs/Getting-Started.md#react-native-webview-getting-started-guide) to add and setup the `react-native-webview` library in your React Native project.

Finally, add the WebView component to your code, setup the properties and fill the room URL and parameters. Here is a short example:

{% tabs %}
{% tab title="React native" %}
```jsx
import React, { Component } from 'react';
import { WebView } from 'react-native-webview';

const roomUrl = ''; // Replace by your own
const roomParameters = '?needancestor&skipMediaPermissionPrompt';

export default class App extends Component {
  render() {
    return (
      <WebView
        startInLoadingState
        source={{ uri: roomUrl + roomParameters }}
        mediaPlaybackRequiresUserAction={false}
        mediaCapturePermissionGrantType={'grant'}

        // iOS specific:
        allowsInlineMediaPlayback

        // Android specific:
        javaScriptEnabled
        domStorageEnabled
      />
    );
  }
}
```
{% endtab %}
{% endtabs %}
