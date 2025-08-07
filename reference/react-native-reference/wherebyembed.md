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
Attributes that accept `on/off` values in the list must be used as normal boolean props in the react-native-sdk.
{% endhint %}

[#attributes-of-the-component](../using-the-whereby-embed-element.md#attributes-of-the-component "mention")

## Event listeners

These are optional props, that will trigger the callback on the corresponding event.

<table><thead><tr><th>Property</th><th>Parameters</th><th>Description</th></tr></thead><tbody><tr><td><code>onWherebyMessage</code></td><td><code>(data: WherebyEvent) => void</code></td><td>Catch-all listener, that triggers on all the below events.</td></tr><tr><td><code>onReady</code></td><td><code>() => void</code></td><td>Basic dependencies have loaded and the room is ready to be used</td></tr><tr><td><code>onKnock</code></td><td><code>() => void</code></td><td>The local user knocks to get into the room</td></tr><tr><td><code>onParticipantUpdate</code></td><td><code>({ count: number }) => void</code></td><td>A new participant join/leave</td></tr><tr><td><code>onJoin</code></td><td><code>() => void</code></td><td>The local user joins</td></tr><tr><td><code>onLeave</code></td><td><code>({ removed: boolean }) => void</code> </td><td>The local user leaves</td></tr><tr><td><code>onParticipantJoin</code></td><td><code>{ participant: { metadata: string | null; externalId: string | null } }) => void</code></td><td>A new participant joins the room</td></tr><tr><td><code>onParticipantLeave</code></td><td><code>{ participant: { metadata: string | null; externalId: string | null } }) => void</code></td><td>A participant leaves the room</td></tr><tr><td><code>onMeetingEnd</code></td><td><code>() => void</code></td><td>A host has ended the meeting via "end meeting for all"</td></tr><tr><td><code>onMicrophoneToggle</code></td><td><code>({ enabled: boolean }) => void</code> </td><td>The local user toggles the microphone</td></tr><tr><td><code>onCameraToggle</code></td><td><code>({ enabled: boolean }) => void</code> </td><td>The local user toggles the camera</td></tr><tr><td><code>onChatToggle</code></td><td><code>({ open: boolean }) => void</code> </td><td>The local user toggles the chat</td></tr><tr><td><code>onPeopleToggle</code></td><td><code>({ open: boolean }) => void</code> </td><td>The local user toggles the people pane</td></tr><tr><td><code>onPipToggle</code></td><td><code>({ open: boolean }) => void</code> </td><td>The local user toggles Picture-in-Picture mode</td></tr><tr><td><code>onGrantDevicePermission</code></td><td><code>({ granted: boolean }) => void</code> </td><td>The local user grants permission to camera and microphone in the pre-call screen</td></tr><tr><td><code>onDenyDevicePermission</code></td><td><code>({ denied: boolean }) => void</code> </td><td>The local user denies permission to camera and microphone in the pre-call screen</td></tr><tr><td><code>onScreenshareToggle</code></td><td><code>({ enabled: boolean }) => void</code></td><td>The local user toggles the screenshare</td></tr><tr><td><code>onStreamingStatusChange</code></td><td><code>({ status: string }) => void</code></td><td><p>Streaming status changes.</p><p>Possible values: " | requested | starting | streaming | stopping | stopped"</p></td></tr><tr><td><code>onConnectionStatusChange</code></td><td><code>({ status: string }) => void</code></td><td><p>User connection status changes.</p><p>Possible values: "stable | unstable"</p></td></tr><tr><td><code>onPrecallCheckSkipped</code></td><td><code>() => void</code></td><td>The local user skipped the presented pre-call check steps</td></tr><tr><td><code>onPrecallCheckCompleted</code></td><td><pre><code>(status: String,
steps: {
camera: { status: String; },
speaker: { status: String; },
microphone: { status: String; },
bandwidth: { status: String; },
}
}) => void
</code></pre><p></p></td><td><p>The local user completed the presented pre-call check steps.</p><p>Possible values for all status properties: "passed | failed".</p></td></tr></tbody></table>



## Commands

Commands can be triggered by assigning a `React.useRef` to the `WherebyEmbed` component. Then use that to call the specific function:

```tsx
const wherebyRoomRef = React.useRef<WherebyWebView>(null);

// Call this from a button in your app, for example.
wherebyRoomRef.current.endMeeting()
```

Full list of available commands:

[#sending-commands](../using-the-whereby-embed-element.md#sending-commands "mention")
