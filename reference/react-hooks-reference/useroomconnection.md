---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# useRoomConnection

The `useRoomConnection` hook provides the ability to connect participants in a given room, subscribe to state updates, and perform actions on the connection like toggling the camera or microphone.

`useRoomConnection(roomUrl: string, roomConnectionOptions: RoomConnectionOptions): Object | null`

| Parameter             | Required | Type                                                                 | Description                                                                                                                                                           |
| --------------------- | -------- | -------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| roomUrl               | ✅        | `string`                                                             | The URL of the Whereby room. Refer to our <a href="https://docs.whereby.com/whereby-rest-api-reference#meetings">REST api reference</a> to learn how to create these. |
| roomConnectionOptions | ✅        | <a href="types.md#roomconnectionoptions">`RoomConnectionOptions`</a> | Additional options for the room connection                                                                                                                            |

## Return type

The hook returns a `RoomConnectionReference` object with the following properties.

| Property   | Type                                      | Description                                                         |
| ---------- | ----------------------------------------- | ------------------------------------------------------------------- |
| state      | [`RoomConnectionState`](#state)           | Object representing the state of the room                           |
| actions    | [`RoomConnectionActions`](#actions)       | Object representing the available actions in the room               |
| components | [`RoomConnectionComponents`](#components) | Object representing the pre-bound components available for the room |

### state

The current state of the room. Use this state to render your custom video experience.

| Property               | Type                                                                                                                            | Description                                                          |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| chatMessages           | [`ChatMessage[]`](./types.md#chatmessage)                                                                                       | The chat messages which have been sent in the room since connecting. |
| cloudRecording         | [`CloudRecordingState`](./types.md#cloudrecordingstate)                                                                         | Indicates whether cloud recording is active in the room.             |
| connectionStatus       | [`RoomConnectionStatus`](./types.md#roomconnectionstatus)                                                                       | Overall status of the room connection                                |
| localScreenshareStatus | [<mark style="color:blue;">`LocalScreenshareStatus`</mark>](./types.md#localscreensharestatus-less-than-string-greater-than)`?` | Status of your local screen share                                    |
| localParticipant       | [`LocalParticipant`](./types.md#localparticipant)`?`                                                                            | A representation of the local participant in the call (you)          |
| remoteParticipants     | [`RemoteParticipant`](broken-reference)`[]`                                                                                     | A list of the remote participants in the room                        |
| screenshares           | [`Screenshare`](./types.md#screenshare)`[]`                                                                                     | List of active screenshares in the room                              |
| liveStream             | [<mark style="color:blue;">`LiveStreamState`</mark>](./types.md#livestreamstate-less-than-object-greater-than)`?`               | Set if live stream is enabled for the room                           |
| waitingParticipants    | [`WaitingParticipant`](./types.md#waitingparticipant)`[]`                                                                       | A list of participants waiting to enter a locked room.               |

### actions

The actions property contains a map of functions which can be invoked to perform an action in the room. All these functions are sync and return `void`, and you should rely on the state to render the effect of their invocation.

| Property                                                    | Type                              | Description                                                                    |
| ----------------------------------------------------------- | --------------------------------- | ------------------------------------------------------------------------------ |
| sendChatMessage                                             | `(text: string) => void`          | Send a chat message to the room                                                |
| <a data-footnote-ref href="#user-content-fn-1">knock</a>    | `() => void`                      | Let the room host know that the local participant is waiting and wants to join |
| setDisplayName                                              | `(displayName: string) => void`   | Change your display name                                                       |  |  |
| toggleCamera                                                | `(enabled?: boolean) => void`     | Change the state of your camera                                                |
| toggleMicrophone                                            | `(enabled?: boolean) => void`     | Change the state of your microphone                                            |
| acceptWaitingParticipant                                    | `(participantId: string) => void` | Accept waiting participant                                                     |
| rejectWaitingParticipant                                    | `(participantId: string) => void` | Reject waiting participant                                                     |
| startScreenshare                                            | `() => void`                      | Start screen share                                                             |
| stopScreenshare                                             | `() => void`                      | Stop screen share                                                              |
| <a data-footnote-ref href="#user-content-fn-2">lockRoom</a> | `(locked: boolean) => void`       | **[Host only]** Lock (`true`) or unlock (`false`) the current room             |

### components

Components are JSX elements bound to the room connection to make it easier for you to render&#x20;

| Property  | Type                        | Description                                                                 |
| --------- | --------------------------- | --------------------------------------------------------------------------- |
| VideoView | [`VideoView`](videoview.md) | A React component to be used for rendering the participant video and audio. |

## Usage

```tsx
import { useRoomConnection } from "@whereby.com/browser-sdk/react";

function MyCallUX( { roomUrl, localStream }) {
    const { state, actions, components } = useRoomConnection(
        "<room_url>"
        {
            localMediaOptions: {
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
[^2]: Only allowed if the participant joined the room using a valid host `roomKey`
