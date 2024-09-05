# WherebyEmbed

The `WherebyEmbed` renders a [react-native-webview](https://github.com/react-native-webview/react-native-webview), with some added custom Whereby properties. It accepts a room property, various [attributes](wherebyembed.md#attributes), [event listeners](wherebyembed.md#event-listeners) and [commands](wherebyembed.md#commands), as well as all other [WebView props](https://github.com/react-native-webview/react-native-webview/blob/master/docs/Reference.md).

```tsx
<WherebyEmbed
    // Room URL. (required)
    room={"https://subdomain.whereby.com/your_room"}
    // Removes some UI elements. Useful for small screens. (optional)
    minimal
    // Skips the media permission prompt. (optional)
    skipMediaPermissionPrompt
    // Catch-all for any Whereby event (optional)
    onWherebyMessage={(event) => {
        console.log(event);
    }}
    // Specific callbacks for each Whereby event (optional)
    onReady={() => {
        console.log("ready");
    }}
/>
```

## Attributes

The `WherebyEmbed` component accepts attributes that you can use to customize the room and user experience. Every one of these attributes can be passed in as props.&#x20;

{% hint style="info" %}
Attributes that accept `on/off` values in the list, can be used as normal boolean props in the react-native-sdk.
{% endhint %}

[#attributes-of-the-component](../using-the-whereby-embed-element.md#attributes-of-the-component "mention")

## Event listeners

These are optional props, that will trigger the callback on the corresponding event.

| Property                   | Parameters                                                                                                                                                                                         | Description                                                                                                                             |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `onWherebyMessage`         | `(data: WherebyEvent) => void`                                                                                                                                                                     | Catch-all listener, that triggers on all the below events.                                                                              |
| `onReady`                  | `() => void`                                                                                                                                                                                       | Basic dependencies have loaded and the room is ready to be used                                                                         |
| `onKnock`                  | `() => void`                                                                                                                                                                                       | The local user knocks to get into the room                                                                                              |
| `onParticipantUpdate`      | `({ count: number }) => void`                                                                                                                                                                      | A new participant join/leave                                                                                                            |
| `onJoin`                   | `() => void`                                                                                                                                                                                       | The local user joins                                                                                                                    |
| `onLeave`                  | `({ removed: boolean }) => void`                                                                                                                                                                   | The local user leaves                                                                                                                   |
| `onParticipantJoin`        | `{ participant: { metadata: string \| null; externalId: string \| null } }) => void`                                                                                                               | A new participant joins the room                                                                                                        |
| `onParticipantLeave`       | `{ participant: { metadata: string \| null; externalId: string \| null } }) => void`                                                                                                               | A participant leaves the room                                                                                                           |
| `onMeetingEnd`             | `() => void`                                                                                                                                                                                       | A host has ended the meeting via "end meeting for all"                                                                                  |
| `onMicrophoneToggle`       | `({ enabled: boolean }) => void`                                                                                                                                                                   | The local user toggles the microphone                                                                                                   |
| `onCameraToggle`           | `({ enabled: boolean }) => void`                                                                                                                                                                   | The local user toggles the camera                                                                                                       |
| `onChatToggle`             | `({ open: boolean }) => void`                                                                                                                                                                      | The local user toggles the chat                                                                                                         |
| `onPeopleToggle`           | `({ open: boolean }) => void`                                                                                                                                                                      | The local user toggles the people pane                                                                                                  |
| `onPipToggle`              | `({ open: boolean }) => void`                                                                                                                                                                      | The local user toggles Picture-in-Picture mode                                                                                          |
| `onGrantDevicePermission`  | `({ granted: boolean }) => void`                                                                                                                                                                   | The local user grants permission to camera and microphone in the pre-call screen                                                        |
| `onDenyDevicePermission`   | `({ denied: boolean }) => void`                                                                                                                                                                    | The local user denies permission to camera and microphone in the pre-call screen                                                        |
| `onScreenshareToggle`      | `({ enabled: boolean }) => void`                                                                                                                                                                   | The local user toggles the screenshare                                                                                                  |
| `onStreamingStatusChange`  | `({ status: string }) => void`                                                                                                                                                                     | <p>Streaming status changes.</p><p>Possible values: " | requested | starting | streaming | stopping | stopped"</p>                      |
| `onConnectionStatusChange` | `({ status: string }) => void`                                                                                                                                                                     | <p>User connection status changes.</p><p>Possible values: "stable | unstable"</p>                                                       |
| `onPrecallCheckSkipped`    | `() => void`                                                                                                                                                                                       | The local user skipped the presented pre-call check steps                                                                               |
| `onPrecallCheckCompleted`  | <pre><code>(status: String,
steps: {
camera: { status: String; },
speaker: { status: String; },
microphone: { status: String; },
bandwidth: { status: String; },
}
}) => void
</code></pre><p></p> | <p>The local user completed the presented pre-call check steps.</p><p>Possible values for all status properties: "passed | failed".</p> |



## Commands

Commands can be triggered by assigning a `React.useRef` to the `WherebyEmbed` component. Then use that to call the specific function:

```tsx
const wherebyRoomRef = React.useRef<WherebyWebView>(null);

// Call this from a button in your app, for example.
wherebyRoomRef.current.endMeeting()
```

Full list of available commands:

[#sending-commands](../using-the-whereby-embed-element.md#sending-commands "mention")
