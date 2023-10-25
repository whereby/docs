# useRoomConnection

<table><thead><tr><th width="234">Parameter</th><th width="103">Required</th><th width="256">Type</th><th>Description</th></tr></thead><tbody><tr><td>roomUrl</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td><code>string</code></td><td>The URL of the Whereby room. Refer to our <a href="https://docs.whereby.com/whereby-rest-api-reference#meetings">REST api reference</a> to learn how to create these. </td></tr><tr><td>roomConnectionOptions</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td><a href="broken-reference"><code>RoomConnectionOptions</code></a></td><td>Additional options for the room connection</td></tr></tbody></table>

## Return type

The hook returns a RoomConnectionReference object with the following properties.

<table><thead><tr><th>Property</th><th width="279.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>state</td><td><a href="useroomconnection.md#state"><code>RoomConnectionState</code></a></td><td>Object representing the state of the room</td></tr><tr><td>actions</td><td><a href="useroomconnection.md#actions"><code>RoomConnectionActions</code></a></td><td>Object representing the available actions in the room</td></tr><tr><td>components</td><td><a href="useroomconnection.md#components"><code>RoomConnectionComponents</code></a></td><td>Object representing the pre-bound components available for the room</td></tr></tbody></table>

### state

The current state of the room. Use this state to render your custom video experience.

| Property              | Type                                         | Description                                                          |
| --------------------- | -------------------------------------------- | -------------------------------------------------------------------- |
| chatMessages          | [`ChatMessage`](broken-reference)`[]`        | The chat messages which have been sent in the room since connecting. |
| cloudRecording        | [`CloudRecordingState`](broken-reference)    | Indicates whether cloud recording is active in the room.             |
| isJoining             | `boolean`                                    |                                                                      |
| isStartingScreenshare | `boolean`                                    |                                                                      |
| joinError             | `unknown`                                    |                                                                      |
| startScreenshareError | `unknown`                                    |                                                                      |
| localParticipant      | `LocalParticipant \| null`                   | A representation of the local participant in the call (you)          |
| mostRecentChatMessage | [`ChatMessage`](broken-reference)`\| null`   | The most recent chat message sent in the room                        |
| remoteParticipants    | [`RemoteParticipant`](broken-reference)`[]`  | A list of the remote participants in the room                        |
| screenshares          | [`Screenshare`](broken-reference)`[]`        | List of active screenshares in the room                              |
| roomConnectionStatus  | [`RoomConnectionStatus`](broken-reference)   | Overall status of the room connection                                |
| streaming             | [`StreamingState`](broken-reference)         | The [Broken link](broken-reference "mention")of the room             |
| waitingParticipants   | [`WaitingParticipant`](broken-reference)`[]` | A list of participants waiting to enter a locked room.               |

### actions

The actions property contains a map of functions which can be invoked to perform an action in the room. All these functions are sync and return `void`, and you should rely on the state to render the effect of their invocation.

<table><thead><tr><th width="239.33333333333331">Property</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td>sendChatMessage</td><td><code>(text: string) => void</code></td><td>Send a chat message to the room</td></tr><tr><td><a data-footnote-ref href="#user-content-fn-1">knock</a></td><td><code>() => void</code></td><td>Let the room host know that the local participant is waiting and wants to join</td></tr><tr><td>setDisplayName</td><td><code>(displayName: string) => void</code></td><td>Change your display name</td></tr><tr><td>toggleCamera</td><td><code>(enabled?: boolean) => void</code></td><td>Change the state of your camera</td></tr><tr><td>toggleMicrophone</td><td><code>(enabled?: boolean) => void</code></td><td>Change the state of your microphone</td></tr><tr><td>acceptWaitingParticipant</td><td><code>(participantId: string) => void</code></td><td>Accept waiting participant</td></tr><tr><td>rejectWaitingParticipant</td><td><code>(participantId: string) => void</code></td><td>Reject waiting participant</td></tr><tr><td>startScreenshare</td><td><code>() => void</code></td><td>Start screen share</td></tr><tr><td>stopScreenshare</td><td><code>() => void</code></td><td>Stop screen share</td></tr></tbody></table>

### components

Components are JSX elements bound to the room connection to make it easier for you to render&#x20;

| Property  | Type                            | Description                                                                 |
| --------- | ------------------------------- | --------------------------------------------------------------------------- |
| VideoView | [`VideoView`](broken-reference) | A React component to be used for rendering the participant video and audio. |

## Usage

```tsx
import { useRoomConnection } from "@whereby.com/browser-sdk";

function MyCallUX( { roomUrl, localStream }) {
    const { state, actions, components } = useRoomConnection(
        "<room_url>"
        {
            localMediaConstraints: {
                audio: true,
                video: true,
            }
        }
    );

    const { connectionState, remoteParticipants } = state;
    const { toggleCamera, toggleMicrophone } = actions;
    const { VideoView } = components;

    return <div className="videoGrid">
        { /* Render any UI, making use of state */ }
        { remoteParticipants.map((p) => (
            <VideoView key={p.id} stream={p.stream} />
        )) }
    </div>;
}
```

[^1]: Only needed when the roomConnectionStatus is `room_locked`
