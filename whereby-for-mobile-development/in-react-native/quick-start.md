# Whereby React Native SDK

The Whereby React Native SDK can be found on [GitHub](https://github.com/whereby/sdk/tree/main/packages/react-native-sdk). It loads the Whereby web component through a WebView and integrates with your editor to make it easier to customize the Whereby Embedded experience. You can also listen to room events and send commands to the room from the host application, giving you the possibility to implement your own controls.

## Quickstart guide

This guide shows you how to embed a Whereby room in an Expo app using the Whereby React Native SDK. For a complete running example, check out our [React Native SDK demo app](https://github.com/whereby/sdk/tree/main/apps/react-native-webview-example) repository.&#x20;

### Prerequisites

This guide assumes that you’ve already created a Whereby Embedded account and a meeting room. If you’ve not already taken these steps, you can do so by following the [Whereby Embedded initial setup guide](../../getting-started/overview-of-whereby-embedded/initial-setup.md).

### Setting up the app

To start, open the terminal to install the latest version of Expo CLI (if you haven’t already), and then create a new Expo app:

```bash
npm install -g expo-cli
npx create-expo-app whereby-rn-app --template blank
cd whereby-rn-app
```

### Installing the Whereby SDK

The official Whereby React Native SDK can be installed via npm or yarn. You will need `expo-camera` and `expo-av` packages to manage the media permissions on Android.

```bash
npm install typescript @types/react
npm install @whereby.com/react-native-sdk
npm install expo-camera
npm install expo-audio
```

### Implementing the app

Open your editor, rename the main file to `App.tsx`, and replace its contents with the following TypeScript code (make sure to update the URL in the example with your own Whereby room URL):

```typescript
import { WherebyEmbed } from "@whereby.com/react-native-sdk/embed";
import { AudioModule } from "expo-audio";
import { Camera } from "expo-camera";
import * as React from "react";
import { Platform, View, Text, Alert } from "react-native";

const ROOM_URL = "https://your-whereby-room-URL";

export default function Room() {

    // iOS renders immediately; Android gates on runtime permissions
    const [hasMediaPermission, setHasMediaPermission] = React.useState(Platform.OS !== "android");

    React.useEffect(() => {
        if (Platform.OS !== "android") return;
        (async () => {
            const [cameraStatus, audioRecordingStatus] = await Promise.all([
                Camera.requestCameraPermissionsAsync(),
                AudioModule.requestRecordingPermissionsAsync(),
            ]);
            if (cameraStatus.granted && audioRecordingStatus.granted) {
                setHasMediaPermission(true);
            } else {
                Alert.alert("Camera and microphone permissions are required", "Please enable them in Settings.");
                setHasMediaPermission(false);
            }
        })();
    }, []);

    if (!hasMediaPermission) {
        return <View />;
    }

    return (
        <View style={{ flex: 1 }}>
            <WherebyEmbed
                style={{ flex: 1 }}
                room={ROOM_URL}
                skipMediaPermissionPrompt
            />
        </View>
    );
}

```

### Running the app

From the terminal, start Expo and use your preferred device (Android or iOS), or a simulator to load the app. Accept the media permissions, and you’ll be able to join the room.

```bash
npx expo start
```

{% hint style="info" %}
This is a minimal implementation. It is recommended to extend the configuration to handle cases where the user denies media permissions and to further improve the user experience. For example, you can:

* Send commands to toggle the camera or microphone, allowing you to implement your own controls.
* Listen for room events, such as when the user leaves the room, to redirect them to the previous screen.

For a complete example, check out our public repository: [react-native-sdk](https://github.com/whereby/sdk/tree/main/apps/react-native-webview-example)[-demo](https://github.com/whereby/android-sdk-demo).
{% endhint %}

### Note for particular implementation

The `expo-audio` and `expo-camera` packages already handle permissions in the manifest and Info.plist. However, if you use a custom implementation, you may need to add the following to your `app.json` or `app.config.ts`:

```json
{
  "expo": {
    "plugins": ["expo-camera", "expo-audio"],
    "android": {
      "permissions": ["CAMERA", "RECORD_AUDIO"]
    },
    "ios": {
      "infoPlist": {
        "NSCameraUsageDescription": "Allow access to camera for video calls.",
        "NSMicrophoneUsageDescription": "Allow access to microphone for calls."
      }
    }
  }
}

```

And then run:&#x20;

```
eas run -p android
eas run -p ios
```
