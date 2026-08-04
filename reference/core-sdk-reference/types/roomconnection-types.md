# RoomConnection Types

### MediaStreamConstraints

{% embed url="https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia#constraints" %}

### MediaStream

{% embed url="https://developer.mozilla.org/en-US/docs/Web/API/MediaStream" %}

### MediaDeviceInfo

{% embed url="https://developer.mozilla.org/en-US/docs/Web/API/MediaDeviceInfo" %}

## LocalMediaOptions: <mark style="color:green;">\<Object></mark>

| Property        | Description  |
| --------------- | ------------ |
| `audio:boolean` | Enable audio |
| `video:boolean` | Enable video |

## WherebyClientOptions: <mark style="color:green;">\<Object></mark>

| Property                               | Description                                                                                                                 |
| -------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `localMediaOptions: localMediaOptions` | Options for starting local media                                                                                            |
| `displayName?: string`                 | Display name of the client                                                                                                  |
| `roomUrl?: string`                     | Room url of the session                                                                                                     |
| `assistantKey?: string \| null`        | If initializing [`Assistant`](../../assistant-sdk-reference/api-reference/assistant.md) the `assistantKey` is required      |
| `roomKey?: string \| null`             | URL of the room to join                                                                                                     |
| `externalId?: string \| null`          | A custom identifier for the participant. Gets saved in Insights data. Supports any **English alphabet characters** `(A-Z)`. |
| `isNodeSDK?: boolean`                  | True if initializing in a Node environment                                                                                  |
| `isAssistant`                          | True if initializing an `Assistant`                                                                                         |

## RemoteParticipantState: <mark style="color:green;">\<Object></mark>

| Property                                  | Description                                                                                                                                                                                                                                                                         |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `displayName: string`                     | Display name of the remote participant                                                                                                                                                                                                                                              |
| `id: string`                              | <p>Id of the remote</p><p>participant</p>                                                                                                                                                                                                                                           |
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

| Property                       | Description                                         |
| ------------------------------ | --------------------------------------------------- |
| `id: string`                   | Id of the participant in the waiting room           |
| `displayName?: string \| null` | Display name of the participant in the waiting room |

## ChatMessage: <mark style="color:green;">\<Object></mark> <a href="#chatmessage" id="chatmessage"></a>

| Property            | Description                                                                                                                                                                                                                                                                                 |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id: string`        | Unique identifier for the chat message                                                                                                                                                                                                                                                      |
| `senderId: string`  | Id of the participant who sent the chat message                                                                                                                                                                                                                                             |
| `parentId?: string` | If present, indicates that the chat message is a reply to a chat message whose `id` matches the given `parentId` value here. If not present, then this chat message is not a reply to any other chat message(s) previously received                                                         |
| `timestamp: string` | Timestamp of when the message was sent                                                                                                                                                                                                                                                      |
| `sig?: string`      | If the current participant is the owner of the chat message (i.e. they sent this chat message) then the service will return this property that can be used to e.g. remove this chat message at a later time (see [`removeChatMessage` API)](../api-reference/roomconnectionclient/#actions) |
| `text: string`      | Text content of the chat message to be displayed                                                                                                                                                                                                                                            |
| `removed: boolean`  | If `true` then this chat message should be considered removed from any ongoing chat display                                                                                                                                                                                                 |

## LocalScreenshareStatus: <mark style="color:green;">\<string></mark>

<table><thead><tr><th width="210">Value</th><th>Description</th></tr></thead><tbody><tr><td><code>"inactive"</code></td><td>Local screenshare is inactive</td></tr><tr><td><code>"starting"</code></td><td>Local screenshare is currently starting, eg the local user is selecting what to share</td></tr><tr><td><code>"active"</code></td><td>Local screenshare is active</td></tr></tbody></table>

## LiveCaption: <mark style="color:green;">\<Object></mark> <a href="#clientview" id="clientview"></a>

<table><thead><tr><th width="380.4270833333333">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>resultId: string</code></td><td>Unique identifier for the current live caption result</td></tr><tr><td><code>participantId: string</code></td><td>Identifier of the participant</td></tr><tr><td><code>text: string</code></td><td>The text content of the live caption result (can be updated until the live caption is finalized)</td></tr><tr><td><code>timestamp: number</code></td><td>Timestamp of when the live caption was received</td></tr></tbody></table>

## ClientView: <mark style="color:green;">\<Object></mark> <a href="#clientview" id="clientview"></a>

A client view can be either a participant or a screenshare.

<table><thead><tr><th width="380.4270833333333">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>id: string</code></td><td>Internal id</td></tr><tr><td><code>clientId: string</code></td><td>Id of the participant</td></tr><tr><td><code>displayName?: string</code></td><td>Display name of the participant or screenshare</td></tr><tr><td><code>stream?: MediaStream</code></td><td>Media stream of the client view</td></tr><tr><td><code>isLocalCliend: boolean</code></td><td>Is the client view owned by the local participant?</td></tr><tr><td><code>isAudioEnabled: boolean</code></td><td>The client view has their microphone enabled</td></tr><tr><td><code>isVideoEnabled: boolean</code></td><td>The client view has their camera enabled</td></tr><tr><td><code>isPresentation: boolean</code></td><td>The client view is a presentation (screenshare)</td></tr></tbody></table>

## CloudRecordingState: <mark style="color:green;">\<Object></mark> <a href="#screenshare" id="screenshare"></a>

<table><thead><tr><th width="224.390625">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>status: "requested" | "recording" | "error"</code></td><td>Cloud recording status</td></tr><tr><td><code>startedAt?: string</code></td><td>If <code>status</code> is <code>"recording"</code> then this field will show when the cloud recording started</td></tr><tr><td><code>error?: string</code></td><td>If <code>status</code> is <code>"error"</code> then this field will show the error message received from the service</td></tr></tbody></table>

## LiveTranscriptionState: <mark style="color:green;">\<Object></mark> <a href="#screenshare" id="screenshare"></a>

<table><thead><tr><th width="220.73828125">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>status: "requested" | "transcribing" | "error"</code></td><td>Live transcription status</td></tr><tr><td><code>startedAt?: string</code></td><td>If <code>status</code> is <code>"transcribing"</code> then this field will show when the live transcription started. Otherwise this field will be <code>undefined</code>.</td></tr><tr><td><code>error?: string</code></td><td>If <code>status</code> is <code>"error"</code> then this field will show the error message received from the service. Otherwise this field will be <code>undefined</code>.</td></tr></tbody></table>

## LiveCaptionsState: <mark style="color:$primary;">\<Object></mark> <a href="#screenshare" id="screenshare"></a>

<table><thead><tr><th width="220.73828125">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>status: "requested" | "captioning" | "error"</code></td><td>Live captions status</td></tr><tr><td><code>startedAt?: string</code></td><td>If <code>status</code> is <code>"captioning"</code> then this field will show when the live captions started. Otherwise this field will be <code>undefined</code>.</td></tr><tr><td><code>error?: string</code></td><td>If <code>status</code> is <code>"error"</code> then this field will show the error message received from the service. Otherwise this field will be <code>undefined</code>.</td></tr><tr><td><code>captionLog: Array&#x3C;</code><a href="roomconnection-types.md#clientview"><code>LiveCaption</code></a><code>></code></td><td>Zero or more live captions data objects received from the Whereby Live Captions service. Entries in this array are removed after 5 seconds of inactivity since their last update.</td></tr></tbody></table>

## ScreenshareState: <mark style="color:green;">\<Object></mark> <a href="#screenshare" id="screenshare"></a>

<table><thead><tr><th width="294.3333333333333">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>participantId: string</code></td><td>Id of the participant owning the screenshare</td></tr><tr><td><code>id: string</code></td><td>Id of the screenshare</td></tr><tr><td><code>hasAudioTrack: boolean</code></td><td></td></tr><tr><td><code>stream?</code>:<a href="roomconnection-types.md#mediastream"><code>MediaStream</code></a></td><td>Media stream of the screenshare</td></tr><tr><td><code>isLocal: boolean</code></td><td>Is the screenshare owned by the local participant?</td></tr></tbody></table>

## LiveStreamState: <mark style="color:green;">\<Object></mark> <a href="#livestreamstate" id="livestreamstate"></a>

| Property              | Description                   |
| --------------------- | ----------------------------- |
| `status: "streaming"` | Live streaming is in progress |
| `startedAt: number`   | When the live stream started  |

## BreakoutState: <mark style="color:green;">\<Object></mark> <a href="#breakout" id="breakout"></a>

Timestamp fields are epoch milliseconds unless noted, so they can be compared directly against `Date.now()` to drive countdowns.

| Type                                                                                            | Description                                                                                                                                                           |
| ----------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `isAvailable: boolean`                                                                          | Breakout groups require a group (SFU) room. `false` in peer-to-peer rooms, where breakout actions are refused                                                         |
| `error: string \| null`                                                                         | Set when a breakout action was refused, for example starting a session in a peer-to-peer room                                                                         |
| `isActive: boolean`                                                                             | The state of the breakout session. If it's true, a breakout session is currently ongoing in the room                                                                  |
| `currentGroup: { id: string, name: string } \| null`                                            | An object containing the information of the breakout group that the participant is currently in. `null` if the SDK participant is not in a group.                     |
| `groups: { [groupId: string]: string } \| null`                                                 | The configured groups, as a map of group id to group name                                                                                                             |
| `enforceAssignment: boolean`                                                                    | When `true`, participants are meant to stay in the group they are assigned to. The SDK surfaces the flag but does not block `joinBreakoutGroup`, honour it in your UI |
| `autoMoveToGroup: boolean`                                                                      | When `true`, assigned participants are moved into their group automatically when the session starts                                                                   |
| `moveToGroupGracePeriod: number \| null`                                                        | Seconds between the session starting and that automatic move. Defaults to 10                                                                                          |
| `autoMoveToMain: boolean`                                                                       | When `true`, participants are moved back to the main room automatically when the session ends                                                                         |
| `moveToMainGracePeriod: number \| null`                                                         | Seconds between the session ending and that automatic move. Defaults to 30                                                                                            |
| `breakoutTimerSetting: boolean`                                                                 | Whether the session runs on a timer                                                                                                                                   |
| `breakoutTimerDuration: number`                                                                 | Length of the timer in seconds. Defaults to 1800 (30 minutes)                                                                                                         |
| `startedAt: Date \| null`                                                                       | When the breakout session was started                                                                                                                                 |
| `endTime: number \| null`                                                                       | When the timer runs out, as a timestamp                                                                                                                               |
| `moveToGroupAt: number \| null`                                                                 | Timestamp of the upcoming automatic move into the group, or `null` when `autoMoveToGroup` is off                                                                      |
| `moveToMainAt: number \| null`                                                                  | Timestamp of the upcoming automatic move back to the main room, or `null` when `autoMoveToMain` is off                                                                |
| `groupedParticipants: { clients: ClientView[], group: { id: string, name: string } \| null }[]` | List of the groups in the current breakout session, including the participants in each group.                                                                         |
| `participantsInCurrentGroup: ClientView[]`                                                      | Participants in the current breakout group. Empty list if the SDK participants is not currently in a group.                                                           |
| `broadcastingParticipants: ClientView[]`                                                        | Main-room participants currently being broadcast into every group. Only populated for participants who are inside a group                                             |

## BreakoutSessionSettings: <mark style="color:green;">\<Object></mark>

The settings shared by `startBreakoutSession` and `updateBreakoutSession`. All fields are optional, anything you leave out keeps its current value.

| Type                                      | Description                                                                                                   |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `enforceAssignment?: boolean`             | Signal that participants should stay in their assigned group instead of picking one                           |
| `autoMoveToGroup?: boolean`               | Move assigned participants into their group automatically when the session starts                             |
| `moveToGroupGracePeriod?: number \| null` | Seconds to wait before that automatic move. Defaults to 10 if you enable `autoMoveToGroup` without setting it |
| `autoMoveToMain?: boolean`                | Move participants back to the main room automatically when the session ends                                   |
| `moveToMainGracePeriod?: number \| null`  | Seconds to wait before that automatic move. Defaults to 30 if you enable `autoMoveToMain` without setting it  |
| `breakoutTimerSetting?: boolean`          | Run the session on a timer                                                                                    |
| `breakoutTimerDuration?: number`          | Timer length in seconds. Defaults to 1800 (30 minutes) if you enable the timer without setting it             |

## StartBreakoutSessionOptions: <mark style="color:green;">\<Object></mark>

`BreakoutSessionSettings`, plus:

| Type                                           | Description                                                            |
| ---------------------------------------------- | ---------------------------------------------------------------------- |
| `groups: { [groupId: string]: string }`        | **Required.** The groups to create, as a map of group id to group name |
| `assignments?: { [clientId: string]: string }` | Which participants go where, as a map of `clientId → groupId`          |

## UpdateBreakoutSessionOptions: <mark style="color:green;">\<Object></mark>

Same shape as `StartBreakoutSessionOptions`, except that `groups` is optional too. Every field you omit is left as it is.

## Breakout group helpers

Exported from `@whereby.com/core` for building the `groups` map:

| Export                              | Description                                                                            |
| ----------------------------------- | -------------------------------------------------------------------------------------- |
| `createBreakoutGroups(count?)`      | Returns `count` groups (`{ a: "Group A", … }`), clamped to the supported range         |
| `defaultBreakoutGroupName(groupId)` | The default display name for a group id, e.g. `"Group A"`                              |
| `BREAKOUT_GROUPS_MIN_MAX`           | `[2, 20]`, the minimum and maximum number of groups                                    |
| `DEFAULT_BREAKOUT_TIMER_DURATION`   | `1800`, the timer duration used when the timer is enabled without an explicit duration |

## ConnectionStatus: <mark style="color:green;">\<string></mark>

<table><thead><tr><th width="222">Value</th><th>Description</th></tr></thead><tbody><tr><td><code>"ready"</code></td><td>Ready to join the room</td></tr><tr><td><code>"connecting"</code></td><td>Currently in the process of doing the initial connection in the room</td></tr><tr><td><code>"connected"</code></td><td>Connected in the room, this is the "stable" state</td></tr><tr><td><code>"room_locked"</code></td><td>Connection failed due to the room being locked, a knock is required to proceed</td></tr><tr><td><code>"knocking"</code></td><td>Waiting for the room host to respond to the knock request</td></tr><tr><td><code>"knock_on_hold"</code></td><td>The host put your knock request on hold. You are still in the waiting room and the host can accept or reject you later, so keep showing a waiting state rather than sending the participant away.<br><br>If the host included a message, it is available as <code>knockResponse</code> on the room connection state.</td></tr><tr><td><code>"knock_rejected"</code></td><td>The host rejected your knock request. If the host included a message, it is available as <code>knockResponse</code></td></tr><tr><td><code>"kicked"</code></td><td><p>The current participant was kicked from the room.</p><p>This can happen in the following scenarios:</p><ul><li>when a meeting host ends the meeting for all participants in the room</li><li>when a meeting host kicks the current participant individually from the room</li></ul></td></tr><tr><td><code>"leaving"</code></td><td>The current participant has invoked the <code>leaveRoom</code> action to exit the room.</td></tr><tr><td><code>"left"</code></td><td>The current participant has now successfully left the room and all other room participants have been notified.</td></tr><tr><td><code>"disconnected"</code></td><td><p>The current participant has been disconnected from the room due to an unplanned loss of network connection.</p><p>This can happen during temporary network outage (e.g. loss of network or switching networks). If/when the network connection returns the SDK will change the room connection status to <code>reconnecting</code>automatically.</p></td></tr><tr><td><code>"reconnecting"</code></td><td><p>A lost internet connection has been re-established.</p><p>The SDK will now automatically re-connect the current participant to the previous room and the room connection state will change to either: a.) <code>knocking</code> if the room is locked, or; b.) <code>connected</code> if the room is unlocked.</p></td></tr></tbody></table>

## LocalParticipantState: <mark style="color:green;">\<Object></mark>

| Property                                                       | Description                                                                                                                                                                                                                                                                         |
| -------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `displayName: string`                                          | Display name of the local participant (you)                                                                                                                                                                                                                                         |
| `id:string`                                                    | Local participant id                                                                                                                                                                                                                                                                |
| `roleName:string`                                              | <p>The role of the local participant. This will be one of the following values:<br><br><code>none</code>, <code>visitor</code>, <code>granted_visitor</code>, <code>viewer</code>, <code>granted_viewer</code>, <code>host</code>, <code>recorder</code>, <code>streamer</code></p> |
| `stream?:`[`MediaStream`](roomconnection-types.md#mediastream) | <p>When set, the media stream (audio &#x26; video) of</p><p>the local participant</p>                                                                                                                                                                                               |
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

## KnockResponse: <mark style="color:green;">\<Object></mark>

The host's response to your knock, delivered when they put you on hold or reject you. Available as `knockResponse` on the room connection state. It is `null` until a response arrives, and a host who responds without writing anything produces a response with no `message`, so always guard on `knockResponse?.message` rather than on `knockResponse` alone.

| Type                           | Description                                                                                                                                      |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| `message?: string`             | The message the host wrote when calling `holdWaitingParticipant` or `rejectWaitingParticipant`. Absent when the host responded without a message |
| `sender?: KnockResponseSender` | Who the message came from. Populated with the host's display name for on-hold responses; rejections are delivered without sender details         |

## KnockResponseSender: <mark style="color:green;">\<Object></mark>

| Type                           | Description                                      |
| ------------------------------ | ------------------------------------------------ |
| `displayName?: string \| null` | Display name of the host who responded           |
| `avatarUrl?: string \| null`   | Avatar of the host who responded, when available |
