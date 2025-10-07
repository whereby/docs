# RoomConnection Types

### MediaStreamConstraints

{% embed url="https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia#constraints" %}

### MediaStream

{% embed url="https://developer.mozilla.org/en-US/docs/Web/API/MediaStream" %}

### MediaDeviceInfo

{% embed url="https://developer.mozilla.org/en-US/docs/Web/API/MediaDeviceInfo" %}

## LocalMediaOptions: <mark style="color:green;">\<Object></mark>

| Property          | Description  |
| ----------------- | ------------ |
| `audio:boolean`   | Enable audio |
| `video:boolean`   | Enable video |

## WherebyClientOptions: <mark style="color:green;">\<Object></mark>

| Property                                  | Description                                                                                                              |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| `localMediaOptions: localMediaOptions`    | Options for starting local media                                                                                         |
| `displayName?: string`                    | Display name of the client                                                                                               |
| `roomUrl?: string`                        | Room url of the session                                                                                                  |
| `assistantKey?: string \| null`           | If initializing [`Assistant`](../../assistant-sdk-reference/api-reference/assistant.md) the `assistantKey` is  required  |
| `roomKey?: string \| null`                | URL of the room to join                                                                                                  |
| `externalId?: string \| null`             | A custom identifier for the participant. Gets saved in Insights data                                                     |
| `isNodeSDK?: boolean`                     | True if initializing in a Node environment                                                                               |
| `isAssistant`                             | True if initializing an `Assistant`                                                                                      |

## RemoteParticipantState: <mark style="color:green;">\<Object></mark>

| Property                                  | Description                                                                                                                                                                                                                                                                         |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `displayName: string`                     | Display name of the remote participant                                                                                                                                                                                                                                              |
| `id: string`                              | <p>Id of the remote </p><p>participant</p>                                                                                                                                                                                                                                          |
| `roleName: string`                        | <p>The role of the local participant. This will be one of the following values:<br><br><code>none</code>, <code>visitor</code>, <code>granted_visitor</code>, <code>viewer</code>, <code>granted_viewer</code>, <code>host</code>, <code>recorder</code>, <code>streamer</code></p> |
| `stream?:MediaStream`                     | Stream of the remote participant                                                                                                                                                                                                                                                    |
| `isAudioEnabled:boolean`                  | The remote participant has their camera enabled                                                                                                                                                                                                                                     |
| `isVideoEnabled:boolean`                  | The remote participant has their microphone enabled                                                                                                                                                                                                                                 |
| `isLocalParticipant:boolean`              | For remote participants, this value is always false                                                                                                                                                                                                                                 |
| `presentationStream: MediaStream \| null` | Screenshare stream if the participant is presenting                                                                                                                                                                                                                                 |
| `externalId: string \| null`              | A custom identifier for the participant.                                                                                                                                                                                                                                            |
| `stickyReaction?: StickyReaction`         | Whether the participant has hand raised                                                                                                                                                                                                                                             |
| `isDialIn: boolean`                       | True if participant is a dial in agent                                                                                                                                                                                                                                              |

## WaitingParticipantState: <mark style="color:green;">\<Object></mark>

| Property                          | Description                                         |
| --------------------------------- | --------------------------------------------------- |
| `id: string`                      | Id of the participant in the waiting room           |
| `displayName?: string \| null`    | Display name of the participant in the waiting room |

## ChatMessage: <mark style="color:green;">\<Object></mark> <a href="#chatmessage" id="chatmessage"></a>

| Property             | Description                                     |
| -------------------- | ----------------------------------------------- |
| `senderId: string`   | Id of the participant who sent the chat message |
| `text: string`       | Content of the chat message                     |
| `timestamp: string`  | Timestamp of when the message was sent          |

## LocalScreenshareStatus: <mark style="color:green;">\<string></mark>

<table><thead><tr><th width="210">Value</th><th>Description</th></tr></thead><tbody><tr><td><code>"inactive"</code></td><td>Local screenshare is inactive</td></tr><tr><td><code>"starting"</code></td><td>Local screenshare is currently starting, eg the local user is selecting what to share</td></tr><tr><td><code>"active"</code></td><td>Local screenshare is active</td></tr></tbody></table>

## CloudRecordingState: <mark style="color:green;">\<Object></mark> <a href="#cloudrecordingstate" id="cloudrecordingstate"></a>

| Property                                           | Description                            |
| -------------------------------------------------- | -------------------------------------- |
| `status:"recording"\|"requested"\|"error"`         | Cloud recording is active              |
| `startedAt?: number`                               | When the recording started             |
| `error?: string`                                   | If error when starting cloud recording |

## ClientView: <mark style="color:green;">\<Object></mark> <a href="#clientview" id="clientview"></a>

A client view can be either a participant or a screenshare.

<table><thead><tr><th width="380.4270833333333">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>id: string</code></td><td>Internal id</td></tr><tr><td><code>clientId: string</code></td><td>Id of the participant</td></tr><tr><td><code>displayName?: string</code></td><td>Display name of the participant or screenshare</td></tr><tr><td><code>stream?: MediaStream</code></td><td>Media stream of the client view</td></tr><tr><td><code>isLocalCliend: boolean</code></td><td>Is the client view owned by the local participant?</td></tr><tr><td><code>isAudioEnabled: boolean</code></td><td>The client view has their microphone enabled</td></tr><tr><td><code>isVideoEnabled: boolean</code></td><td>The client view has their camera enabled</td></tr><tr><td><code>isPresentation: boolean</code></td><td>The client view is a presentation (screenshare)</td></tr></tbody></table>

## ScreenshareState: <mark style="color:green;">\<Object></mark> <a href="#screenshare" id="screenshare"></a>

<table><thead><tr><th width="294.3333333333333">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>participantId: string</code></td><td>Id of the participant owning the screenshare</td></tr><tr><td><code>id: string</code></td><td>Id of the screenshare</td></tr><tr><td><code>hasAudioTrack: boolean</code></td><td></td></tr><tr><td><code>stream?</code>:<a href="roomconnection-types.md#mediastream"><code>MediaStream</code></a></td><td>Media stream of the screenshare</td></tr><tr><td><code>isLocal: boolean</code></td><td>Is the screenshare owned by the local participant?</td></tr></tbody></table>

## LiveStreamState: <mark style="color:green;">\<Object></mark> <a href="#livestreamstate" id="livestreamstate"></a>

| Property              | Description                   |
| --------------------- | ----------------------------- |
| `status: "streaming"` | Live streaming is in progress |
| `startedAt: number`   | When the live stream started  |

## BreakoutState: <mark style="color:green;">\<Object></mark> <a href="#breakout" id="breakout"></a>

<table><thead><tr><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>isActive: boolean</code></td><td>The state of the breakout session. If it's true, a breakout session is currently ongoing in the room</td></tr><tr><td><code>currentGroup: { id: string, name: string } | null</code></td><td>An object containing the information of the breakout group that the participant is currently in. <code>null</code>if the SDK participant is not in a group.</td></tr><tr><td><code>groupedParticipants: { clients:</code> <a href="roomconnection-types.md#remoteparticipant"><code>RemoteParticipant</code></a> <code>[], group: { id: string, name: string } []</code><br></td><td>List of the groups in the current breakout session, including the participants in each group.</td></tr><tr><td><code>participantsInCurrentGroup:</code><a href="roomconnection-types.md#remoteparticipant"><code>RemoteParticipant</code></a> <code>[]</code></td><td>Participants in the current breakout group. Empty list if the SDK participants is not currently in a group.</td></tr></tbody></table>

## ConnectionStatus: <mark style="color:green;">\<string></mark>

<table><thead><tr><th width="222">Value</th><th>Description</th></tr></thead><tbody><tr><td><code>"ready"</code></td><td>Ready to join the room</td></tr><tr><td><code>"connecting"</code></td><td>Currently in the process of doing the initial connection in the room</td></tr><tr><td><code>"connected"</code></td><td>Connected in the room, this is the "stable" state</td></tr><tr><td><code>"room_locked"</code></td><td>Connection failed due to the room being locked, a knock is required to proceed</td></tr><tr><td><code>"knocking"</code></td><td>Waiting for the room host to respond to the knock request</td></tr><tr><td><code>"knock_rejected"</code></td><td>The host rejected your knock request</td></tr><tr><td><code>"kicked"</code></td><td><p>The current participant was kicked from the room. </p><p></p><p>This can happen in the following scenarios:</p><ul><li>when a meeting host ends the meeting for all participants in the room</li><li>when a meeting host kicks the current participant individually from the room</li></ul></td></tr><tr><td><code>"leaving"</code></td><td>The current participant has invoked the <code>leaveRoom</code> action to exit the room.</td></tr><tr><td><code>"left"</code></td><td>The current participant has now successfully left the room and all other room participants have been notified.</td></tr><tr><td><code>"disconnected"</code></td><td><p>The current participant has been disconnected from the room due to an unplanned loss of network connection.</p><p></p><p>This can happen during temporary network outage (e.g. loss of network or switching networks). If/when the network connection returns the SDK will change the room connection status to <code>reconnecting</code>automatically.</p></td></tr><tr><td><code>"reconnecting"</code></td><td><p>A lost internet connection has been re-established. </p><p></p><p>The SDK will now automatically re-connect the current participant to the previous room and the room connection state will change to either: a.) <code>knocking</code> if the room is locked, or; b.) <code>connected</code> if the room is unlocked.</p></td></tr></tbody></table>

## LocalParticipantState: <mark style="color:green;">\<Object></mark>

| Property                                                       | Description                                                                                                                                                                                                                                                                         |
| -------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `displayName: string`                                          | Display name of the local participant (you)                                                                                                                                                                                                                                         |
| `id:string`                                                    | Local participant id                                                                                                                                                                                                                                                                |
| `roleName:string`                                              | <p>The role of the local participant. This will be one of the following values:<br><br><code>none</code>, <code>visitor</code>, <code>granted_visitor</code>, <code>viewer</code>, <code>granted_viewer</code>, <code>host</code>, <code>recorder</code>, <code>streamer</code></p> |
| `stream?:`[`MediaStream`](roomconnection-types.md#mediastream) | <p>When set, the media stream (audio &#x26; video) of </p><p>the local participant</p>                                                                                                                                                                                              |
| `isAudioEnabled:boolean`                                       | The local participant has their microphone enabled                                                                                                                                                                                                                                  |
| `isVideoEnabled:boolean`                                       | The local participant has their camera enabled                                                                                                                                                                                                                                      |
| `isLocalParticipant:true`                                      | Always set to true. Can be used to easily identify the local participant if in an array with eg remote participants                                                                                                                                                                 |
| `isScreensharing:boolean`                                      | True if the local participant is screen sharing                                                                                                                                                                                                                                     |
| `clientClaim?:string`                                          |                                                                                                                                                                                                                                                                                     |
| `breakoutGroupAssigned:string`                                 | The name of the breakout group the participant is assigned to (if any)                                                                                                                                                                                                              |

## RoomJoinedEvent: <mark style="color:green;">\<Object></mark>

| Property            | Description                               |
| ------------------- | ----------------------------------------- |
| `isLocked: boolean` | True if room is locked                    |
| `selfId: string`    | Id of the participant who joined the room |

## RoomJoinedSuccess: <mark style="color:green;">\<Object></mark>

| Property                        | Description                                                                                                      |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `room: object`                  | Room state data on room joined                                                                                   |
| `selfId: string`                | Id of the participant who joined the room                                                                        |
| `breakoutGroup: string \| null` | Name of breakout group that the participant joined, or `null` if user is not in a breakout group on room joined. |
