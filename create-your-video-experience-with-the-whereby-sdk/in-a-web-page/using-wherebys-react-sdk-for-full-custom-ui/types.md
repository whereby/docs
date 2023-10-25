# Types

### MediaStreamConstraints

{% embed url="https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia#constraints" %}

### MediaStream

{% embed url="https://developer.mozilla.org/en-US/docs/Web/API/MediaStream" %}

### MediaDeviceInfo

{% embed url="https://developer.mozilla.org/en-US/docs/Web/API/MediaDeviceInfo" %}

### RoomConnectionOptions

| Property                                                                              | Description                                                                                                    |
| ------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| `displayName?: string`                                                                | <p>The name to use for the local participant </p><p>(you) in the call</p>                                      |
| `localMediaConstraints?:` [`MediaStreamConstraints`](types.md#mediastreamconstraints) | Constraints to use for the local participant media (camera and microphone)                                     |
| `localMedia:` [`LocalMedia`](broken-reference)                                        | Existing local media to use, as provided by the [`useLocalMedia`](broken-reference) hook                       |
| `roomKey?: string`                                                                    | Room key to use if the local participant should assume a non-standard role in the room, such as host or viewer |

### ChatMessage

<table><thead><tr><th>Property</th><th width="286.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>senderId</td><td><code>string</code></td><td>Id of the participant who sent the chat message</td></tr><tr><td>text</td><td><code>string</code></td><td>Content of the chat message</td></tr><tr><td>timestamp</td><td><code>string</code></td><td>Timestamp of when the message was sent (TODO: format)</td></tr></tbody></table>

### CloudRecordingState

<table><thead><tr><th>Property</th><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>status</td><td><code>"" | "recording"</code></td><td>The current cloud recording status. <code>""</code> if there is no active recording</td></tr><tr><td>startedAt</td><td><code>number | null</code></td><td>When the recording started</td></tr></tbody></table>

### LocalParticipant

<table><thead><tr><th>Property</th><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>displayName</td><td><code>string</code></td><td>Display name of the local participant (you)</td></tr><tr><td>id</td><td><code>string</code></td><td>Local participant id</td></tr><tr><td>stream</td><td><code>MediaStream?</code></td><td><p>When set, the media stream (audio &#x26; video) of </p><p>the local participant</p></td></tr><tr><td>isAudioEnabled</td><td><code>boolean</code></td><td></td></tr><tr><td>isVideoEnabled</td><td><code>boolean</code></td><td></td></tr><tr><td>isLocalParticipant</td><td><code>true</code></td><td>Always set to true. Can be used to easily identify the local participant if in an array with eg remote participants</td></tr></tbody></table>

### RemoteParticipant

<table><thead><tr><th>Property</th><th width="294.3333333333333">Types</th><th>Description</th></tr></thead><tbody><tr><td>displayName</td><td><code>string</code></td><td>Display name of the remote participant</td></tr><tr><td>id</td><td><code>string</code></td><td><p>Id of the remote </p><p>participant</p></td></tr><tr><td>stream</td><td><code>MediaStream?</code></td><td>Stream of the remote participant</td></tr><tr><td>isAudioEnabled</td><td><code>boolean</code></td><td></td></tr><tr><td>isVideoEnabled</td><td><code>boolean</code></td><td></td></tr><tr><td>newJoiner</td><td><code>boolean</code></td><td>TODO: Deprecate</td></tr><tr><td>streams</td><td></td><td>TODO: Deprecate</td></tr></tbody></table>

### Screenshare

<table><thead><tr><th>Property</th><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>participantId</td><td><code>string</code></td><td>Id of the participant owning the screenshare</td></tr><tr><td>id</td><td><code>string</code></td><td>Id of the screenshare</td></tr><tr><td>hasAudioTrack</td><td><code>boolean</code></td><td></td></tr><tr><td>stream</td><td><code>MediaStream?</code></td><td></td></tr><tr><td>isLocal</td><td><code>boolean</code></td><td>Is the screenshare owned by the local participant?</td></tr></tbody></table>

### RoomConnectionStatus

<table><thead><tr><th>Type</th><th width="294.3333333333333">Properties</th><th>Description</th></tr></thead><tbody><tr><td>Enum</td><td><code>"" |</code><br><code>"connecting" |</code> <br><code>"connected" |</code><br><code>"room_locked" |</code><br><code>"knocking" |</code><br><code>"disconnecting" |</code><br><code>"accepted" |</code><br><code>"rejected"</code></td><td>The connection status of the SDK client.</td></tr></tbody></table>

### StreamingState

<table><thead><tr><th>Property</th><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>status</td><td><code>"" | "streaming"</code></td><td>The streaming status, <code>""</code> if there is no ongoing stream</td></tr><tr><td>startedAt</td><td><code>number | null</code></td><td></td></tr></tbody></table>

### WaitingParticipant

<table><thead><tr><th>Property</th><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td><code>string</code></td><td>Id of the participant waiting to be let in</td></tr><tr><td>displayName</td><td><code>string</code></td><td></td></tr></tbody></table>
