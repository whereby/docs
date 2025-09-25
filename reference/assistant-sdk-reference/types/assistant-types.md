# Assistant Types

## MediaStream

{% embed url="https://developer.mozilla.org/en-US/docs/Web/API/MediaStream" %}

## MediaStreamTrack

{% embed url="https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamTrack" %}

## RemoteParticipantState:<<mark style="color:green;">Object</mark>>

| Property                                  | Description                                                                                                                                                                                                                                                                         |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `displayName: string`                     | Display name of the remote participant                                                                                                                                                                                                                                              |
| `id: string`                              | <p>Id of the remote </p><p>participant</p>                                                                                                                                                                                                                                          |
| `roleName: string`                        | <p>The role of the local participant. This will be one of the following values:<br><br><code>none</code>, <code>visitor</code>, <code>granted_visitor</code>, <code>viewer</code>, <code>granted_viewer</code>, <code>host</code>, <code>recorder</code>, <code>streamer</code></p> |
| `stream?:MediaStream`                     | Stream of the remote participant                                                                                                                                                                                                                                                    |
| `isAudioEnabled:boolean`                  | The remote participant has their camera enabled                                                                                                                                                                                                                                     |
| `isVideoEnabled:boolean`                  | The remote participant has their microphone enabled                                                                                                                                                                                                                                 |
| `isLocalParticipant:boolean`              | For remote participants, this value is always false                                                                                                                                                                                                                                 |
| `presentationStream: MediaStream \| null` | If the participant is sharing their screen                                                                                                                                                                                                                                          |
| `externalId: string \| null`              | A custom identifier for the participant.                                                                                                                                                                                                                                            |
| `stickyReaction?: StickyReaction`         | Whether the participant has hand raised                                                                                                                                                                                                                                             |
| `isDialIn: boolean`                       | True if participant is a dial in agent                                                                                                                                                                                                                                              |

## ChatMessage: <mark style="color:green;">\<Object></mark> <a href="#chatmessage" id="chatmessage"></a>

| Property             | Description                                     |
| -------------------- | ----------------------------------------------- |
| `senderId: string`   | Id of the participant who sent the chat message |
| `text: string`       | Content of the chat message                     |
| `timestamp: string`  | Timestamp of when the message was sent          |

## AssistantEvents:<<mark style="color:green;">Object</mark>>

| Property                                                                                                                                                      | Description                                 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| `[AUDIO_STREAM_READY]: [ { stream:` [`MediaStream`](assistant-types.md#mediastream)`; track:` [`MediaStreamTrack`](assistant-types.md#mediastreamtrack)`} ];` | Emitted when combined audio stream is ready |
| `[ASSISTANT_JOINED_ROOM]: [{ roomUrl: string }]+`                                                                                                             | Emitted when the assistant joins the room   |
| `[ASSISTANT_LEFT_ROOM]: [{ roomUrl: string }]`                                                                                                                | Emitted when the assistant leaves the room  |
