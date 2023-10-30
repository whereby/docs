# Types

### MediaStreamConstraints

{% embed url="https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia#constraints" %}

### MediaStream

{% embed url="https://developer.mozilla.org/en-US/docs/Web/API/MediaStream" %}

### MediaDeviceInfo

{% embed url="https://developer.mozilla.org/en-US/docs/Web/API/MediaDeviceInfo" %}

### RoomConnectionOptions: <mark style="color:green;">\<Object></mark>

| Property                                                                              | Description                                                                                                                                                      |
| ------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `displayName?: string`                                                                | <p>The name to use for the local participant </p><p>(you) in the call</p>                                                                                        |
| `localMediaConstraints?:` [`MediaStreamConstraints`](types.md#mediastreamconstraints) | Constraints to use for the local participant media (camera and microphone)                                                                                       |
| `localMedia:` [`LocalMedia`](broken-reference)                                        | Existing local media to use, as provided by the [`useLocalMedia`](uselocalmedia.md) hook                                                                         |
| `roomKey?: string`                                                                    | Room key to use if the local participant should assume a non-standard role in the room, such as [host or viewer](../../whereby-101/user-roles-and-privileges.md) |

### ChatMessage: <mark style="color:green;">\<Object></mark>

<table><thead><tr><th>Property</th><th width="286.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>senderId</td><td><code>string</code></td><td>Id of the participant who sent the chat message</td></tr><tr><td>text</td><td><code>string</code></td><td>Content of the chat message</td></tr><tr><td>timestamp</td><td><code>string</code></td><td>Timestamp of when the message was sent (TODO: format)</td></tr></tbody></table>

### CloudRecordingState:  <mark style="color:green;">\<Object></mark>

<table><thead><tr><th>Property</th><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>status</td><td><code>"recording"</code></td><td>Cloud recording is active</td></tr><tr><td>startedAt</td><td><code>number</code></td><td>When the recording started</td></tr></tbody></table>

### LocalScreenshareStatus: <mark style="color:green;">\<string></mark>

<table><thead><tr><th width="210">Value</th><th>Description</th></tr></thead><tbody><tr><td><code>"starting"</code></td><td>Local screenshare is currently starting, eg the local user is selecting what to share</td></tr><tr><td><code>"active"</code></td><td>Local screenshare is active</td></tr></tbody></table>

### LocalParticipant: <mark style="color:green;">\<Object></mark>

<table><thead><tr><th>Property</th><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>displayName</td><td><code>string</code></td><td>Display name of the local participant (you)</td></tr><tr><td>id</td><td><code>string</code></td><td>Local participant id</td></tr><tr><td>stream</td><td><a href="types.md#mediastream"><code>MediaStream</code></a><code>?</code></td><td><p>When set, the media stream (audio &#x26; video) of </p><p>the local participant</p></td></tr><tr><td>isAudioEnabled</td><td><code>boolean</code></td><td>The local participant has their microphone enabled</td></tr><tr><td>isVideoEnabled</td><td><code>boolean</code></td><td>The local participant has their camera enabled</td></tr><tr><td>isLocalParticipant</td><td><code>true</code></td><td>Always set to true. Can be used to easily identify the local participant if in an array with eg remote participants</td></tr></tbody></table>

### RemoteParticipant: <mark style="color:green;">\<Object></mark>

<table><thead><tr><th>Property</th><th width="294.3333333333333">Types</th><th>Description</th></tr></thead><tbody><tr><td>displayName</td><td><code>string</code></td><td>Display name of the remote participant</td></tr><tr><td>id</td><td><code>string</code></td><td><p>Id of the remote </p><p>participant</p></td></tr><tr><td>stream</td><td><a href="types.md#mediastream"><code>MediaStream</code></a><code>?</code></td><td>Stream of the remote participant</td></tr><tr><td>isAudioEnabled</td><td><code>boolean</code></td><td>Stream of the remote participant</td></tr><tr><td>isVideoEnabled</td><td><code>boolean</code></td><td>The remote participant has their microphone enabled</td></tr></tbody></table>

### Screenshare: <mark style="color:green;">\<Object></mark>

<table><thead><tr><th>Property</th><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>participantId</td><td><code>string</code></td><td>Id of the participant owning the screenshare</td></tr><tr><td>id</td><td><code>string</code></td><td>Id of the screenshare</td></tr><tr><td>hasAudioTrack</td><td><code>boolean</code></td><td></td></tr><tr><td>stream</td><td><a href="types.md#mediastream"><code>MediaStream</code></a><code>?</code></td><td></td></tr><tr><td>isLocal</td><td><code>boolean</code></td><td>Is the screenshare owned by the local participant?</td></tr></tbody></table>

### RoomConnectionStatus: <mark style="color:green;">\<string></mark>

<table><thead><tr><th width="222">Value</th><th>Description</th></tr></thead><tbody><tr><td><code>"connecting"</code></td><td>Currently in the process of doing the initial connection in the room</td></tr><tr><td><code>"connected"</code></td><td>Connected in the room, this is the "stable" state</td></tr><tr><td><code>"room_locked"</code></td><td>Connection failed due to the room being locked, a knock is required to proceed</td></tr><tr><td><code>"knocking"</code></td><td>Waiting for the room host to respond to the knock request</td></tr><tr><td><code>"knock_rejected"</code></td><td>The host rejected your knock request</td></tr></tbody></table>

### LiveStreamState: <mark style="color:green;">\<Object></mark>

<table><thead><tr><th>Property</th><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>status</td><td><code>"streaming"</code></td><td>Live streaming is in progress</td></tr><tr><td>startedAt</td><td><code>number</code></td><td>When the live stream started</td></tr></tbody></table>

### WaitingParticipant: <mark style="color:green;">\<Object></mark>

<table><thead><tr><th>Property</th><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td><code>string</code></td><td>Id of the participant waiting to be let in</td></tr><tr><td>displayName</td><td><code>string</code></td><td></td></tr></tbody></table>
