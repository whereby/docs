# Quick Start

This code snippet shows a basic example of using the `react-native-sdk/embed` to create a simple video app.

```tsx
import * as React from "react";
import { ScrollView, StyleSheet, Text, View, Alert, Platform, Button } from "react-native";
import { Audio } from "expo-av";
import { Camera } from "expo-camera";
import { WherebyEmbed, type WherebyWebView, WherebyEvent } from "@whereby.com/react-native-sdk/embed";

const ROOM_URL = "";

export default function Room() {
    const wherebyRoomRef = React.useRef<WherebyWebView>(null);
    const [hasPermissionForAndroid, setHasPermissionForAndroid] = React.useState<boolean>(false);

    React.useEffect(() => {
        (async () => {
            if (Platform.OS === "android") {
                const cameraStatus = await Camera.requestCameraPermissionsAsync();
                const audioStatus = await Audio.requestPermissionsAsync();

                if (cameraStatus.status === "granted" && audioStatus.status === "granted") {
                    setHasPermissionForAndroid(true);
                } else {
                    Alert.alert("Permissions Required", "Camera and microphone permissions are required.");
                    setHasPermissionForAndroid(false);
                }
            }
        })();
    }, []);

    if (Platform.OS === "android" && !hasPermissionForAndroid) {
        return <View />;
    }

    return (
        <View style={{ flex: 1 }}>
            <Button
                onPress={() => {
                    wherebyRoomRef.current?.toggleMicrophone();
                }}
                title="Toggle Microphone"
            />
            <View style={{ flex: 1, height: "100%" }}>
                <WherebyEmbed
                    ref={wherebyRoomRef}
                    style={{ flex: 1 }}
                    room={ROOM_URL}
                    // Removes some UI elements. Useful for small screens.
                    minimal
                    // Skips the media permission prompt.
                    skipMediaPermissionPrompt
                    // Catch-all for any Whereby event
                    onWherebyMessage={(event) => {
                        console.log(event);
                    }}
                    // Specific callbacks for each Whereby event
                    onReady={() => {
                        console.log("ready");
                    }}
                />
            </View>
        </View>
    );
}
```
