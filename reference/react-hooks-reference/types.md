# Types

### MediaStreamConstraints

{% embed url="https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia#constraints" %}

### MediaStream

{% embed url="https://developer.mozilla.org/en-US/docs/Web/API/MediaStream" %}

### MediaDeviceInfo

{% embed url="https://developer.mozilla.org/en-US/docs/Web/API/MediaDeviceInfo" %}

### RoomConnectionOptions: <mark style="color:green;">\<Object></mark> <a href="#roomconnectionoptions" id="roomconnectionoptions"></a>

| Property                                                                              | Description                                                                                                                                                                   |
| ------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `displayName?: string`                                                                | <p>The name to use for the local participant</p><p>(you) in the call</p>                                                                                                      |
| `localMediaConstraints?:` [`MediaStreamConstraints`](types.md#mediastreamconstraints) | Constraints to use for the local participant media (camera and microphone)                                                                                                    |
| `localMedia: LocalMedia`                                                              | Existing local media to use, as provided by the [`useLocalMedia`](uselocalmedia.md) hook                                                                                      |
| `roomKey?: string`                                                                    | Room key to use if the local participant should assume a non-standard role in the room, such as [host or viewer](../../whereby-product-features/user-roles-and-privileges.md) |

### ChatMessage: <mark style="color:green;">\<Object></mark> <a href="#chatmessage" id="chatmessage"></a>

<table><thead><tr><th>Property</th><th width="286.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td><code>string</code></td><td>Unique identifier for the chat message </td></tr><tr><td>senderId</td><td><code>string</code></td><td>Id of the participant who sent the chat message</td></tr><tr><td>parentId</td><td><code>string?</code></td><td>If present, indicates that the chat message is a reply to a chat message whose <code>id</code> matches the given <code>parentId</code> value here. If not present, then this chat message is not a reply to any other chat message(s) previously received</td></tr><tr><td>sig</td><td><code>string?</code></td><td>If the current participant is the owner of the chat message (i.e. they sent this chat message) then the service will return this property that can be used to e.g. remove this chat message at a later time (see <a href="../core-sdk-reference/api-reference/roomconnectionclient/#actions"><code>removeChatMessage</code> API)</a></td></tr><tr><td>text</td><td><code>string</code></td><td>Content of the chat message</td></tr><tr><td>timestamp</td><td><code>string</code></td><td>Timestamp of when the message was sent (TODO: format)</td></tr><tr><td>removed</td><td><code>boolean</code></td><td>If <code>true</code> then this chat message should be considered removed from any ongoing chat display</td></tr></tbody></table>

### LiveCaption: <mark style="color:green;">\<Object></mark>

<table><thead><tr><th width="234.69270833333331">Property</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td>resultId</td><td><code>string</code></td><td>Unique identifier for the current live caption result</td></tr><tr><td>participantId</td><td><code>string</code></td><td>Identifier of the participant</td></tr><tr><td>text</td><td><code>string</code></td><td>The text content of the live caption result (can be updated until the live caption is finalized)</td></tr><tr><td>timestamp</td><td><code>number</code></td><td>Timestamp of when the live caption was received</td></tr></tbody></table>

### CloudRecordingState: <mark style="color:green;">\<Object></mark> <a href="#cloudrecordingstate" id="cloudrecordingstate"></a>

<table><thead><tr><th>Property</th><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>status</td><td><code>"requested" | "recording" | "error"</code></td><td>Cloud recording status</td></tr><tr><td>startedAt</td><td><code>number</code></td><td>If <code>status</code> is <code>"recording"</code> then this field will show when the cloud recording started</td></tr><tr><td>error</td><td><code>string</code></td><td>If <code>status</code> is <code>"error"</code> then this field will show the error message received from the service</td></tr></tbody></table>

### LiveTranscriptionState: <mark style="color:green;">\<Object></mark> <a href="#localscreensharestatus" id="localscreensharestatus"></a>

<table><thead><tr><th>Property</th><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>status</td><td><code>null | "requested" | "transcribing" | "error"</code></td><td>Live transcription status</td></tr><tr><td>startedAt</td><td><code>number</code></td><td>If <code>status</code> is <code>"transcribing"</code> then this field will show when the live transcription started</td></tr><tr><td>error</td><td><code>string</code></td><td>If <code>status</code> is <code>"error"</code> then this field will show the error message received from the service</td></tr></tbody></table>

### LiveCaptionsState: <mark style="color:green;">\<Object></mark> <a href="#localscreensharestatus" id="localscreensharestatus"></a>

<table><thead><tr><th>Property</th><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>status</td><td><code>null | "requested" | "captioning" | "error"</code></td><td>Live captions status</td></tr><tr><td>startedAt</td><td><code>number</code></td><td>If <code>status</code> is <code>"captioning"</code> then this field will show when the live captions started for the local client</td></tr><tr><td>error</td><td><code>string</code></td><td>If <code>status</code> is <code>"error"</code> then this field will show the error message received from the service</td></tr><tr><td>captionLog</td><td><code>Array&#x3C;</code><a href="types.md#livecaption-less-than-object-greater-than"><code>LiveCaption</code></a><code>></code></td><td>Zero or more live captions data objects received from the Whereby Live Captions service. Entries in this array are removed after 5 seconds of inactivity since their last update.</td></tr></tbody></table>

### LocalScreenshareStatus: <mark style="color:green;">\<string></mark> <a href="#localscreensharestatus" id="localscreensharestatus"></a>

<table><thead><tr><th width="210">Value</th><th>Description</th></tr></thead><tbody><tr><td><code>"inactive"</code></td><td>Local screenshare is inactive</td></tr><tr><td><code>"starting"</code></td><td>Local screenshare is currently starting, eg the local user is selecting what to share</td></tr><tr><td><code>"active"</code></td><td>Local screenshare is active</td></tr></tbody></table>

### LocalParticipant: <mark style="color:green;">\<Object></mark> <a href="#localparticipant" id="localparticipant"></a>

<table><thead><tr><th>Property</th><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>displayName</td><td><code>string</code></td><td>Display name of the local participant (you)</td></tr><tr><td>id</td><td><code>string</code></td><td>Local participant id</td></tr><tr><td>roleName</td><td><code>string</code></td><td>The role of the local participant. This will be one of the following values:<br><br><code>none</code>, <code>visitor</code>, <code>granted_visitor</code>, <code>viewer</code>, <code>granted_viewer</code>, <code>host</code>, <code>recorder</code>, <code>streamer</code></td></tr><tr><td>stream</td><td><a href="types.md#mediastream"><code>MediaStream</code></a><code>?</code></td><td><p>When set, the media stream (audio &#x26; video) of</p><p>the local participant</p></td></tr><tr><td>isAudioEnabled</td><td><code>boolean</code></td><td>The local participant has their microphone enabled</td></tr><tr><td>isVideoEnabled</td><td><code>boolean</code></td><td>The local participant has their camera enabled</td></tr><tr><td>isLocalParticipant</td><td><code>true</code></td><td>Always set to true. Can be used to easily identify the local participant if in an array with eg remote participants</td></tr><tr><td>breakoutGroup</td><td><code>string | null</code></td><td>Id of the breakout group the local participant is currently <em>in</em>. <code>null</code> or empty when they are in the main room</td></tr><tr><td>breakoutGroupAssigned</td><td><code>string</code></td><td>Id of the breakout group the local participant has been <em>assigned</em> to by a host. Empty string when unassigned. Compare with <code>breakoutGroup</code> to tell whether they still need to join</td></tr></tbody></table>

### RemoteParticipant: <mark style="color:green;">\<Object></mark> <a href="#remoteparticipant" id="remoteparticipant"></a>

<table><thead><tr><th>Property</th><th width="294.3333333333333">Types</th><th>Description</th></tr></thead><tbody><tr><td>displayName</td><td><code>string</code></td><td>Display name of the remote participant</td></tr><tr><td>id</td><td><code>string</code></td><td><p>Id of the remote</p><p>participant</p></td></tr><tr><td>roleName</td><td><code>string</code></td><td>The role of the local participant. This will be one of the following values:<br><br><code>none</code>, <code>visitor</code>, <code>granted_visitor</code>, <code>viewer</code>, <code>granted_viewer</code>, <code>host</code>, <code>recorder</code>, <code>streamer</code></td></tr><tr><td>stream</td><td><a href="types.md#mediastream"><code>MediaStream</code></a><code>?</code></td><td>Stream of the remote participant</td></tr><tr><td>isAudioEnabled</td><td><code>boolean</code></td><td>The remote participant has their camera enabled</td></tr><tr><td>isVideoEnabled</td><td><code>boolean</code></td><td>The remote participant has their microphone enabled</td></tr><tr><td>breakoutGroup</td><td><code>string | null</code></td><td>Id of the breakout group the participant is currently in. <code>null</code> or empty when they are in the main room</td></tr><tr><td>breakoutGroupAssigned</td><td><code>string</code></td><td>Id of the breakout group the participant has been assigned to. Empty string when unassigned, use it to render assignment state in a host's breakout panel</td></tr></tbody></table>

### ClientView: <mark style="color:green;">\<Object></mark> <a href="#clientview" id="clientview"></a>

A client view can be either a participant or a screenshare.

<table><thead><tr><th>Property</th><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td><code>string</code></td><td>Internal id</td></tr><tr><td>clientId</td><td><code>string</code></td><td>Id of the participant</td></tr><tr><td>displayName</td><td><code>string</code></td><td>Display name of the participant or screenshare</td></tr><tr><td>stream</td><td><a href="types.md#mediastream"><code>MediaStream</code></a><code>?</code></td><td></td></tr><tr><td>isLocalClient</td><td><code>boolean</code></td><td>Is the client view owned by the local participant?</td></tr><tr><td>isAudioEnabled</td><td><code>boolean</code></td><td>The client view has their microphone enabled</td></tr><tr><td>isVideoEnabled</td><td><code>boolean</code></td><td>The client view has their camera enabled</td></tr><tr><td>isPresentation</td><td><code>boolean</code></td><td>The client view is a presentation (screenshare)</td></tr></tbody></table>

### Screenshare: <mark style="color:green;">\<Object></mark> <a href="#screenshare" id="screenshare"></a>

<table><thead><tr><th>Property</th><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>participantId</td><td><code>string</code></td><td>Id of the participant owning the screenshare</td></tr><tr><td>id</td><td><code>string</code></td><td>Id of the screenshare</td></tr><tr><td>hasAudioTrack</td><td><code>boolean</code></td><td></td></tr><tr><td>stream</td><td><a href="types.md#mediastream"><code>MediaStream</code></a><code>?</code></td><td></td></tr><tr><td>isLocal</td><td><code>boolean</code></td><td>Is the screenshare owned by the local participant?</td></tr></tbody></table>

### RoomConnectionStatus: <mark style="color:green;">\<string></mark> <a href="#roomconnectionstatus" id="roomconnectionstatus"></a>

<table><thead><tr><th width="222">Value</th><th>Description</th></tr></thead><tbody><tr><td><code>"ready"</code></td><td>Ready to join the room</td></tr><tr><td><code>"connecting"</code></td><td>Currently in the process of doing the initial connection in the room</td></tr><tr><td><code>"connected"</code></td><td>Connected in the room, this is the "stable" state</td></tr><tr><td><code>"room_locked"</code></td><td>Connection failed due to the room being locked, a knock is required to proceed</td></tr><tr><td><code>"knocking"</code></td><td>Waiting for the room host to respond to the knock request</td></tr><tr><td><code>"knock_on_hold"</code></td><td>The host put your knock request on hold. You are still in the waiting room and the host can accept or reject you later, so keep showing a waiting state rather than sending the participant away.<br><br>If the host included a message, it is available in <code>knockResponse</code>.</td></tr><tr><td><code>"knock_rejected"</code></td><td>The host rejected your knock request. If the host included a message, it is available in <code>knockResponse</code>.</td></tr><tr><td><code>"kicked"</code></td><td><p>The current participant was kicked from the room.</p><p>This can happen in the following scenarios:</p><ul><li>when a meeting host ends the meeting for all participants in the room</li><li>when a meeting host kicks the current participant individually from the room</li></ul></td></tr><tr><td><code>"leaving"</code></td><td>The current participant has invoked the <code>leaveRoom</code> action to exit the room.</td></tr><tr><td><code>"left"</code></td><td>The current participant has now successfully left the room and all other room participants have been notified.</td></tr><tr><td><code>"disconnected"</code></td><td><p>The current participant has been disconnected from the room due to an unplanned loss of network connection.</p><p>This can happen during temporary network outage (e.g. loss of network or switching networks). If/when the network connection returns the SDK will change the room connection status to <code>reconnecting</code>automatically.</p></td></tr><tr><td><code>"reconnecting"</code></td><td><p>A lost internet connection has been re-established.</p><p>The SDK will now automatically re-connect the current participant to the previous room and the room connection state will change to either: a.) <code>knocking</code> if the room is locked, or; b.) <code>connected</code> if the room is unlocked.</p></td></tr></tbody></table>

### LiveStreamState: <mark style="color:green;">\<Object></mark> <a href="#livestreamstate" id="livestreamstate"></a>

<table><thead><tr><th>Property</th><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>status</td><td><code>"streaming"</code></td><td>Live streaming is in progress</td></tr><tr><td>startedAt</td><td><code>number</code></td><td>When the live stream started</td></tr></tbody></table>

### WaitingParticipant: <mark style="color:green;">\<Object></mark> <a href="#waitingparticipant" id="waitingparticipant"></a>

<table><thead><tr><th>Property</th><th width="294.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td><code>string</code></td><td>Id of the participant waiting to be let in</td></tr><tr><td>displayName</td><td><code>string</code></td><td></td></tr></tbody></table>

### WaitingParticipant on hold

A participant that a host has put on hold stays in the `waitingParticipants` list, they are still in the waiting room, just told to wait. The list entry itself is unchanged; the on-hold state lives on the waiting participant's own client, as the `knock_on_hold` connection status. Track locally which participants you put on hold if you want to reflect that in the host UI.

### KnockResponse: <mark style="color:green;">\<Object></mark>

The host's response to your knock, delivered when they put you on hold or reject you. Available as `state.knockResponse`. It is `null` until a response arrives, and a host who responds without writing anything produces a response with no `message` so always guard on `knockResponse?.message` rather than on `knockResponse` alone.

| Property | Type                   | Description                                                                                                                                      |
| -------- | ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| message  | `string?`              | The message the host wrote when calling `holdWaitingParticipant` or `rejectWaitingParticipant`. Absent when the host responded without a message |
| sender   | `KnockResponseSender?` | Who the message came from. Populated with the host's display name for on-hold responses; rejections are delivered without sender details         |

### KnockResponseSender: <mark style="color:green;">\<Object></mark> <a href="#breakout" id="breakout"></a>

| Property    | Type              | Description                                      |
| ----------- | ----------------- | ------------------------------------------------ |
| displayName | `string \| null?` | Display name of the host who responded           |
| avatarUrl   | `string \| null?` | Avatar of the host who responded, when available |

### Breakout: <mark style="color:green;">\<Object></mark> <a href="#breakout" id="breakout"></a>

The full breakout state of the room, as `state.breakout`. Timestamp fields are epoch milliseconds unless noted, so they can be compared directly against `Date.now()` to drive countdowns.

| Property                   | Type                                                                       | Description                                                                                                                                                                                                            |
| -------------------------- | -------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| isAvailable                | `boolean`                                                                  | Breakout groups require a group (SFU) room. `false` in peer-to-peer rooms, where breakout actions are refused                                                                                                          |
| error                      | `string \| null`                                                           | Set when a breakout action was refused, for example starting a session in a peer-to-peer room                                                                                                                          |
| isActive                   | `boolean`                                                                  | The state of the breakout session. If it's true, a breakout session is currently ongoing in the room                                                                                                                   |
| currentGroup               | `{ id: string, name: string } \| null`                                     | The breakout group that the local participant is currently in. `null` if they are not in a group                                                                                                                       |
| groups                     | `{ [groupId: string]: string } \| null`                                    | The configured groups, as a map of group id to group name                                                                                                                                                              |
| enforceAssignment          | `boolean`                                                                  | When `true`, participants are meant to stay in the group they are assigned to. The SDK surfaces the flag but does not block `joinBreakoutGroup`, honour it in your UI, for example by only offering the assigned group |
| autoMoveToGroup            | `boolean`                                                                  | When `true`, assigned participants are moved into their group automatically when the session starts                                                                                                                    |
| moveToGroupGracePeriod     | `number \| null`                                                           | Seconds between the session starting and the automatic move into the group. Gives you a window to show a countdown and a "join now" button. Defaults to 10 when `autoMoveToGroup` is enabled                           |
| autoMoveToMain             | `boolean`                                                                  | When `true`, participants are moved back to the main room automatically when the session ends                                                                                                                          |
| moveToMainGracePeriod      | `number \| null`                                                           | Seconds between the session ending and the automatic move back to the main room. Defaults to 30 when `autoMoveToMain` is enabled                                                                                       |
| breakoutTimerSetting       | `boolean`                                                                  | Whether the session runs on a timer                                                                                                                                                                                    |
| breakoutTimerDuration      | `number`                                                                   | Length of the timer in seconds. Defaults to 1800 (30 minutes) when the timer is enabled without a duration                                                                                                             |
| startedAt                  | `Date \| null`                                                             | When the breakout session was started                                                                                                                                                                                  |
| endTime                    | `number \| null`                                                           | When the timer runs out, as a timestamp. Use it to render the remaining time                                                                                                                                           |
| moveToGroupAt              | `number \| null`                                                           | Timestamp of the upcoming automatic move into the group, or `null` when `autoMoveToGroup` is off                                                                                                                       |
| moveToMainAt               | `number \| null`                                                           | Timestamp of the upcoming automatic move back to the main room, or `null` when `autoMoveToMain` is off                                                                                                                 |
| groupedParticipants        | `{ clients: ClientView[], group: { id: string, name: string } \| null }[]` | List of the groups in the current breakout session, including the participants in each group                                                                                                                           |
| participantsInCurrentGroup | `ClientView[]`                                                             | Participants in the local participant's current group. Empty list when they are not in a group                                                                                                                         |
| broadcastingParticipants   | `ClientView[]`                                                             | Main-room participants currently being broadcast into every group. Only populated for participants who are inside a group                                                                                              |

### BreakoutSessionSettings: <mark style="color:green;">\<Object></mark>

The settings shared by `startBreakoutSession` and `updateBreakoutSession`. All fields are optional, anything you leave out keeps its current value.

| Property               | Type              | Description                                                                                                                                               |
| ---------------------- | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| enforceAssignment      | `boolean?`        | Signal that participants should stay in their assigned group instead of picking one. Surfaced as `state.breakout.enforceAssignment` for your UI to honour |
| autoMoveToGroup        | `boolean?`        | Move assigned participants into their group automatically when the session starts                                                                         |
| moveToGroupGracePeriod | `number \| null?` | Seconds to wait before that automatic move. Defaults to 10 if you enable `autoMoveToGroup` without setting it                                             |
| autoMoveToMain         | `boolean?`        | Move participants back to the main room automatically when the session ends                                                                               |
| moveToMainGracePeriod  | `number \| null?` | Seconds to wait before that automatic move. Defaults to 30 if you enable `autoMoveToMain` without setting it                                              |
| breakoutTimerSetting   | `boolean?`        | Run the session on a timer                                                                                                                                |
| breakoutTimerDuration  | `number?`         | Timer length in seconds. Defaults to 1800 (30 minutes) if you enable the timer without setting it                                                         |

## StartBreakoutSessionOptions: <mark style="color:green;">\<Object></mark>

`BreakoutSessionSettings`, plus:

| Property    | Type                              | Description                                                            |
| ----------- | --------------------------------- | ---------------------------------------------------------------------- |
| groups      | `{ [groupId: string]: string }`   | **Required.** The groups to create, as a map of group id to group name |
| assignments | `{ [clientId: string]: string }?` | Which participants go where, as a map of `clientId → groupId`          |

### BreakoutGroupAssignedEvent: <mark style="color:green;">\<Object></mark>

| Property  | Type     | Description                                           |
| --------- | -------- | ----------------------------------------------------- |
| group     | `string` | Id of the group the local participant was assigned to |
| groupName | `string` | Name of that group                                    |

### BreakoutTimerEvent: <mark style="color:green;">\<Object></mark>

Carries no props. The `message` on the NotificationEvent describes what happened, and the current timer state is available in `state.breakout`.

## Events

### NotificationEvent

All events are of type `NotificationEvent`, with

| Property | Type                               | Description |
| -------- | ---------------------------------- | ----------- |
| type     | `string`                           |             |
| message  | `string`                           |             |
| props    | [EventProps](types.md#event-props) |             |

## Event Props

### StickyReactionEvent

| Property       | Type                                              | Description |
| -------------- | ------------------------------------------------- | ----------- |
| client         | [RemoteParticipant](types.md#remoteparticipant)   |             |
| stickyReaction | `{ reaction: string, timestamp: string } \| null` |             |

### RequestAudioEvent

| Property | Type                                            | Description |
| -------- | ----------------------------------------------- | ----------- |
| client   | [RemoteParticipant](types.md#remoteparticipant) |             |
| enable   | `boolean`                                       |             |

### SignalStatusEvent

| Property | Type | Description |
| -------- | ---- | ----------- |
|          |      |             |
|          |      |             |

### ChatMessageEvent

| Property    | Type                                            | Description |
| ----------- | ----------------------------------------------- | ----------- |
| client      | [RemoteParticipant](types.md#remoteparticipant) |             |
| chatMessage | [ChatMessage](types.md#chatmessage)             |             |
