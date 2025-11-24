# useRoomConnection

The `useRoomConnection` hook provides the ability to connect participants in a given room, subscribe to state updates, and perform actions on the connection like toggling the camera or microphone.

`useRoomConnection(roomUrl: string, roomConnectionOptions: RoomConnectionOptions): Object | null`

<table><thead><tr><th width="234">Parameter</th><th width="103">Required</th><th width="275">Type</th><th>Description</th></tr></thead><tbody><tr><td>roomUrl</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td><code>string</code></td><td>The URL of the Whereby room. Refer to our <a href="https://docs.whereby.com/whereby-rest-api-reference#meetings">REST api reference</a> to learn how to create these. </td></tr><tr><td>roomConnectionOptions</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td><a href="../types.md#roomconnectionoptions"><code>RoomConnectionOptions</code></a></td><td>Additional options for the room connection</td></tr></tbody></table>

## Return type

The hook returns a `RoomConnectionReference` object with the following properties.

<table><thead><tr><th>Property</th><th width="279.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>state</td><td><a href="useroomconnection.md#state"><code>RoomConnectionState</code></a></td><td>Object representing the state of the room</td></tr><tr><td>actions</td><td><a href="useroomconnection.md#actions"><code>RoomConnectionActions</code></a></td><td>Object representing the available actions in the room</td></tr><tr><td>events</td><td><a href="useroomconnection.md#events">RoomConnectionEvents</a></td><td>Event emitter that emits in-room events as they are happening</td></tr></tbody></table>

### state

The current state of the room. Use this state to render your custom video experience.

| Property                | Type                                                                                               | Description                                                          |
| ----------------------- | -------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| chatMessages            | [`ChatMessage[]`](../types.md#chatmessage)                                                         | The chat messages which have been sent in the room since connecting. |
| cloudRecording          | [`CloudRecordingState`](../types.md#cloudrecordingstate)                                           | Indicates whether cloud recording is active in the room.             |
| connectionStatus        | [`RoomConnectionStatus`](../types.md#roomconnectionstatus)                                         | Overall status of the room connection                                |
| localScreenshareStatus  | [<mark style="color:blue;">`LocalScreenshareStatus`</mark>](../types.md#localscreensharestatus)`?` | Status of your local screen share                                    |
| localParticipant        | [`LocalParticipant`](../types.md#localparticipant)`?`                                              | A representation of the local participant in the call (you)          |
| remoteParticipants      | [`RemoteParticipant`](../types.md#remoteparticipant)`[]`                                           | A list of the remote participants in the room                        |
| screenshares            | [`Screenshare`](../types.md#screenshare)`[]`                                                       | List of active screenshares in the room                              |
| liveStream              | [<mark style="color:blue;">`LiveStreamState`</mark>](../types.md#livestreamstate)`?`               | Set if live stream is enabled for the room                           |
| waitingParticipants     | [`WaitingParticipant`](../types.md#waitingparticipant)`[]`                                         | A list of participants waiting to enter a locked room.               |
| spotlightedParticipants | [ClientView](../types.md#clientview)\[]                                                            | A list of spotlighted participants                                   |
| breakout                | [Breakout](../types.md#breakout)                                                                   | The breakout group state of the room                                 |

### actions

The actions property contains a map of functions which can be invoked to perform an action in the room. All these functions are sync and return `void`, and you should rely on the state to render the effect of their invocation.

<table><thead><tr><th width="187.55208333333331">Property</th><th width="324.72265625">Type</th><th>Description</th></tr></thead><tbody><tr><td>joinRoom</td><td><code>() => Promise&#x3C;</code><a href="../../core-sdk-reference/types/roomconnection-types.md#roomjoinedsuccess-less-than-object-greater-than"><code>RoomJoinedSuccess</code></a><code>></code></td><td><p>Join the room configured in the <a href="useroomconnection.md"><code>useRoomConnection</code></a> config.</p><p></p></td></tr><tr><td><a data-footnote-ref href="#user-content-fn-1">knock</a></td><td><code>() => void</code></td><td>Let the room host know that the local participant is waiting and wants to join</td></tr><tr><td>setDisplayName</td><td><code>(displayName: string) => void</code></td><td>Change your display name</td></tr><tr><td>sendChatMessage</td><td><code>(text: string) => void</code></td><td>Send a chat message to the room</td></tr><tr><td>toggleCamera</td><td><code>(enabled?: boolean) => void</code></td><td>Change the state of your camera</td></tr><tr><td>toggleMicrophone</td><td><code>(enabled?: boolean) => void</code></td><td>Change the state of your microphone</td></tr><tr><td></td><td></td><td></td></tr><tr><td>toggleLowDataMode</td><td><code>(enabled?: boolean) => void</code></td><td>Change the state of low data mode</td></tr><tr><td>toggleRaiseHand</td><td><code>(enabled?: boolean) => void</code></td><td>Toggle raising and lowering your hand in the meeting. Any host in the meeting can acknowledge your request with the <code>askToSpeak</code> host action.</td></tr><tr><td>acceptWaitingParticipant</td><td><code>(participantId: string) => void</code></td><td>Accept waiting participant</td></tr><tr><td>rejectWaitingParticipant</td><td><code>(participantId: string) => void</code></td><td>Reject waiting participant</td></tr><tr><td>startScreenshare</td><td><code>() => void</code></td><td>Start screen share</td></tr><tr><td>stopScreenshare</td><td><code>() => void</code></td><td>Stop screen share</td></tr><tr><td>leaveRoom</td><td><code>() => void</code></td><td>Leave the room</td></tr><tr><td>joinBreakoutGroup</td><td><code>(group: string) ⇒ void</code></td><td>Join a breakout group. </td></tr><tr><td>joinBreakoutMainRoom</td><td><code>() ⇒ void</code></td><td>Join the main room in a breakout session.</td></tr><tr><td>switchCameraEffect</td><td><code>(effectId: string) => void</code>        </td><td>Enable a camera effect.<code>@whereby.com/camera-effects</code> needs to be installed for this to work</td></tr><tr><td>clearCameraEffect</td><td><code>() ⇒ void</code></td><td>Disable a camera effect.<code>@whereby.com/camera-effects</code> needs to be installed for this to work</td></tr></tbody></table>

#### Host-only actions

When a participant provides a valid "host" `roomKey` in the [`RoomConnectionOptions`](../types.md#roomconnectionoptions-less-than-object-greater-than) when the [useRoomConnection](useroomconnection.md) hook was created, they will have access to a number of addition host-only actions in rooms:

<table><thead><tr><th width="239.33333333333331">Property</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td>lockRoom</td><td><code>(locked: boolean) => void</code></td><td><strong>[Host only]</strong> Lock (<code>true</code>) or unlock (<code>false</code>) the current room</td></tr><tr><td>muteParticipants</td><td><code>(participantIds: string[]) => void</code></td><td><strong>[Host only]</strong> Mute the specified remote participants</td></tr><tr><td>askToSpeak</td><td><code>(participantId: string) => void</code></td><td><p><strong>[Host only]</strong> Ask the specified remote participant to unmute their microphone and speak in the meeting. </p><p></p><p>This is typically useful in response to a <code>toggleRaiseHand</code> request from a participant in the meeting.</p></td></tr><tr><td>kickParticipant</td><td><code>(participantId: string) => void</code></td><td><strong>[Host only]</strong> Remove the specified remote participant from the meeting </td></tr><tr><td>endMeeting</td><td><code>(stayBehind?: boolean) => void</code></td><td><p><strong>[Host only]</strong> End the meeting for <em>all</em> remote participants. </p><p></p><p>If <code>stayBehind</code> is not provided or is not <code>true</code>, then the local participant will also leave the room</p></td></tr><tr><td>spotlightParticipant</td><td><code>(participantId: string) => void</code></td><td><strong>[Host only]</strong> Put a spotlight on a participant. <br><br>When used in combination with the video grid, the spotlighted participant will move to the presentation stage, and their video cell will be bigger.</td></tr><tr><td>removeSpotlight</td><td><code>(participantId: string) => void</code></td><td><strong>[Host only]</strong> Remove spotlight on a participant. </td></tr></tbody></table>

### events

Event emitter which emits notification events as they are happening inside of the room.

It's possible to subscribe and unsubscribe to events using the `events.on` and `events.off` methods.

| Event               | Payload                                                                                                | Description                                                                                                                                                                                                                                                          |
| ------------------- | ------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \*                  | [NotificationEvent](../types.md#notificationevent)                                                     | Listen for all events                                                                                                                                                                                                                                                |
| requestAudioEnable  | [NotificationEvent](../types.md#notificationevent)<[RequestAudioEvent](../types.md#requestaudioevent)> | <p>A host is asking for the local participant to speak in the meeting. </p><p></p><p>The local participant should be notified when this event is received and prompted to trigger <code>actions.toggleMicrophone(true)</code> if or when they are ready to speak</p> |
| requestAudioDisable | [NotificationEvent](../types.md#notificationevent)<[RequestAudioEvent](../types.md#requestaudioevent)> |  A host has forcibly muted your microphone                                                                                                                                                                                                                           |
| signalTrouble       | [NotificationEvent](../types.md#notificationevent)<[SignalStatusEvent](../types.md#signalstatusevent)> | There is a problem with the internet connection and a connection to our signal server can not be established                                                                                                                                                         |
| signalOk            | [NotificationEvent](../types.md#notificationevent)<[SignalStatusEvent](../types.md#signalstatusevent)> | Internet connectivity is present or it has been restored after `signalTrouble`                                                                                                                                                                                       |
| chatMessageReceived | [NotificationEvent](../types.md#notificationevent)<[ChatMessageEvent](../types.md#chatmessageevent)>   | A chat message was sent by a remote participant                                                                                                                                                                                                                      |

#### Host-only events

When a participant provides a valid "host" `roomKey` in the [`RoomConnectionOptions`](../types.md#roomconnectionoptions-less-than-object-greater-than) when the [useRoomConnection](useroomconnection.md) hook was created, they will have access to a number of addition host-only events in rooms:

| Event             | Payload                                                                                                    | Description                                                                                                                                                                                                                                                                                                                        |
| ----------------- | ---------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| remoteHandRaised  | [NotificationEvent](../types.md#notificationevent)<[StickyReactionEvent](../types.md#stickyreactionevent)> | <p>A remote participant has raised their hand to request to speak in the meeting.</p><p></p><p>The local host participant should be notified when this event is received and prompted to trigger <code>actions.askToSpeak(participantId)</code> if or when they want to invite the remote participant to speak in the meeting.</p> |
| remoteHandLowered | [NotificationEvent](../types.md#notificationevent)<[StickyReactionEvent](../types.md#stickyreactionevent)> | <p>A remote participant who previously had their hand raised has now lowered their hand.</p><p></p><p>Any previous raised hand notifications shown for this remote participant should be cancelled and no further action is needed from the local host participant.</p>                                                            |

## Usage

```tsx
import * as React from "react";
import { useRoomConnection, VideoView } from "@whereby.com/browser-sdk/react";

function MyCallUX( { roomUrl, localStream }) {
    const { state, actions, events } = useRoomConnection(
        "<room_url>"
        {
            localMediaOptions: {
                audio: true,
                video: true,
            }
        }
    );

    const { connectionState, remoteParticipants } = state;
    const { joinRoom, leaveRoom, toggleCamera, toggleMicrophone } = actions;
    
    React.useEffect(() => {
        joinRoom().catch(error => console.error("Could not join room", error));
        return () => leaveRoom();
    }, []);

    // listen to all notification events on mount and unlisten on unmount
    React.useEffect(() => {
        if(!events) return;
        const handleEvents = (e) => console.log(e);
        events.on("*", handleEvents);
        return () => events && events.off("*", handleEvents);
    }, []);

    return <div className="videoGrid">
        { /* Render any UI, making use of state */ }
        { remoteParticipants.map((p) => (
            <VideoView key={p.id} stream={p.stream} />
        )) }
    </div>;
}
```

[^1]: Only needed when the roomConnectionStatus is `room_locked`
