# Assistant

The `Assistant` class is the main entry point of the Assistants SDK. It runs in Node.js and provides everything that you need to connect an AI agent into a Whereby room - to access audio, manage in room actions and integrate with external services of your choice.&#x20;

## Constructor

```jsx
const assistant = new Assistant(options: AssistantOptions)
```

### AssistantOptions

| Parameter                   | Type      | Default | Description                                                                                                                                                                                                  |
| --------------------------- | --------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `assistantKey`              | `string`  |  -      | Assistant key generated from the Whereby Dashboard                                                                                                                                                           |
| `startCombinedAudioStream?` | `boolean` | false   | <p>If true, creates a mixed audio stream of all remote participants<br><span data-gb-custom-inline data-tag="emoji" data-code="2757">❗</span><strong>FFmpeg is required if using this feature.</strong> </p> |
| `startLocalMedia?`          | `boolean` | false   | If true, initializes  local media for the assistant                                                                                                                                                          |

## Methods

### Room Lifecycle

<table><thead><tr><th>Method</th><th>Parameters</th><th width="226.72918701171875">Returns</th><th>Description</th></tr></thead><tbody><tr><td><code>joinRoom</code></td><td><code>roomUrl: string</code></td><td><code>Promise&#x3C;void></code></td><td>Connects the assistant to the specified room</td></tr><tr><td><code>getRoomConnection</code></td><td></td><td><a href="../../core-sdk-reference/api-reference/roomconnectionclient.md"><code>RoomConnectionClient</code></a></td><td>Returns the underlying room connection</td></tr></tbody></table>

### Media

| Method                   | Parameters | Returns                                                            | Description                                                   |
| ------------------------ | ---------- | ------------------------------------------------------------------ | ------------------------------------------------------------- |
| `startLocalMedia`        |            | `void`                                                             | Creates and starts local media for the assistant              |
| `getLocalMediaStream`    |            | [`MediaStream`](../types/assistant-types.md#mediastream) `\| null` | Returns the assistant's local media stream if started         |
| `getLocalAudioSource`    |            | `RTCAudioSource \| null`                                           | Returns the raw Node WebRTC audio source                      |
| `getCombinedAudioStream` |            | [`MediaStream`](../types/assistant-types.md#mediastream) `\| null` | Returns the mixed audio stream of all the remote participants |

### Remote Participants

| Method                     | Parameters                    | Returns                                                                   | Description                                                  |
| -------------------------- | ----------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------ |
| `getRemoteParticipants`    |                               | [`RemoteParticipantState`](../types/assistant-types.md#remoteparticipant) | List of current participants in the room                     |
| `spotlightParticipant`     | `id: string`                  | `void`                                                                    | Spotlights the specified participant in the room             |
| `` removeSpotlight` ``     |                               | `void`                                                                    | Removes spotlight from a participant in the room             |
| `requestAudioEnable`       | `id: string, enable: boolean` | `void`                                                                    | Requests that the specified participant enables their audio  |
| `requestVideoEnabled`      | `id: string, enable: boolean` | `void`                                                                    | Requests that the specified participant enables their video  |
| `acceptWaitingParticipant` | `id string`                   | `void`                                                                    | Accepts a participant from the waiting room                  |
| `rejectWaitingParticipant` |                               | `void`                                                                    | Rejects a participant in the waiting room                    |

### Chat

| Method            | Parameters        | Returns | Description                       |
| ----------------- | ----------------- | ------- | --------------------------------- |
| `sendChatMessage` | `message: string` | `void`  | Send a chat message into the room |

### Recording

| Method                | Parameters | Returns | Description                                                                                                                                     |
| --------------------- | ---------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| `startCloudRecording` |            | `void`  | Starts a [cloud recording](../../../meeting-content-and-quality/recording-with-embedded/cloud-recording.md). Must be enabled to be successful.  |
| `stopCloudRecording`  |            | `void`  | Stops the active cloud recording.                                                                                                               |

## State Subscriptions

All subscribe methods follow this format:

* Call signature: `subscribeX(callback: (payload: T) => void: () => void`
* Contract: Invokes a callback on initial subscription and whenever the state changes.
* Returns: an unsubscribe function

<table><thead><tr><th width="256.01348876953125">Method</th><th>Payload Type</th><th>Description</th></tr></thead><tbody><tr><td><code>subscribeToChatMessages</code></td><td><code>messages:</code> <a href="../types/assistant-types.md#chatmessage"><code>ChatMessage</code></a><code>[]</code> </td><td>Emits chat messages from remote participants</td></tr><tr><td><code>subscribeToRemoteParticipants</code></td><td><code>participants:</code> <a href="../types/assistant-types.md#remoteparticipantstate-less-than-object-greater-than"><code>RemoteParticipantState</code></a><code>[]</code> </td><td>Emits the remote participant state</td></tr></tbody></table>

## Events

`Assistant` extends `EventEmitter` . You can listen to lifecycle and state change events directly:&#x20;

```jsx
import { Assistant } from "@whereby.com/assistant";

const assistant = new Assistant({
  assistantKey: process.env.ASSISTANT_KEY,
  startCombinedAudioStream: true,
});

assistant.on(AUDIO_STREAM_READY, ({ stream }) => {
  console.log("Combined audio stream available", stream.id);
});

assistant.on(ASSISTANT_LEFT_ROOM, ({ roomUrl }) => {
  console.log("Assistant has left the room: ", roomUrl);
});

```

<table><thead><tr><th>Event </th><th width="257.26788330078125">Payload</th><th>Emitted when</th></tr></thead><tbody><tr><td><code>AUDIO_STREAM_READY</code></td><td><code>{stream:</code> <a href="../types/assistant-types.md#mediastream"><code>MediaStream</code></a><code>; track:</code> <a href="../types/assistant-types.md#mediastreamtrack"><code>MediaStreamTrack</code></a><code>}</code>    </td><td>Combined audio stream is available</td></tr><tr><td><code>ASSISTANT_JOINED_ROOM</code></td><td><code>{ roomUrl: string }</code></td><td>Assistant has joined the room</td></tr><tr><td><code>ASSISTANT_LEFT_ROOM</code></td><td><code>{ roomUrl: string }</code></td><td>Assistant has left the room</td></tr><tr><td><code>PARTICIPANT_VIDEO_TRACK_ADDED</code></td><td><code>{ participantId: string; stream: MediaStream; track: MediaStreamTrack }</code></td><td>A remote participant has added or changed a video track</td></tr><tr><td><code>PARTICIPANT_VIDEO_TRACK_REMOVED</code></td><td><code>{ participantId: string; stream: MediaStream; track: MediaStreamTrack }</code></td><td>A remote participant has removed a video track</td></tr><tr><td><code>PARTICIPANT_AUDIO_TRACK_ADDED</code></td><td><code>{ participantId: string; stream: MediaStream; track: MediaStreamTrack }</code></td><td>A remote participant has added or changed an audio track</td></tr><tr><td><code>PARTICIPANT_AUDIO_TRACK_REMOVED</code></td><td><code>{ participantId: string; stream: MediaStream; track: MediaStreamTrack }</code></td><td>A remote participant has removed an audio track</td></tr></tbody></table>
