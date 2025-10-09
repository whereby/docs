# Assistant

The `Assistant` class is the main entry point of the Assistants SDK. It runs in Node.js and provides everything that you need to connect an AI agent into a Whereby room - to access in-room audio and video streams, manage in-room actions and integrate with external services of your choice.&#x20;

## Constructor

```tsx
const assistant = new Assistant(options: AssistantOptions)
```

### AssistantOptions

| Parameter      | Type     | Default | Description                                        |
| -------------- | -------- | ------- | -------------------------------------------------- |
| `assistantKey` | `string` |  -      | Assistant key generated from the Whereby Dashboard |

## Methods

### Room Lifecycle

To join a room or to then subsequenty perform any in-room actions, the following methods are available.

<table><thead><tr><th width="176.14453125">Method</th><th width="155.6328125">Parameters</th><th width="256.57293701171875">Returns</th><th>Description</th></tr></thead><tbody><tr><td><code>joinRoom</code></td><td><code>roomUrl: string</code></td><td><code>Promise&#x3C;</code><a href="../../core-sdk-reference/types/roomconnection-types.md#roomjoinedsuccess-less-than-object-greater-than"><code>RoomJoinedSuccess</code></a><code>></code></td><td>Connects the assistant to the specified room. In case the room can not be joined the Promise will be rejected with a room joined error code.</td></tr><tr><td><code>getRoomConnection</code></td><td></td><td><a href="../../core-sdk-reference/api-reference/roomconnectionclient.md"><code>RoomConnectionClient</code></a></td><td>Returns the underlying room connection controller.</td></tr></tbody></table>

{% hint style="info" %}
With a [RoomConnectionClient](../../core-sdk-reference/api-reference/roomconnectionclient.md) object the Whereby Assistant can then perform host-level in-room actions including:&#x20;

* controlling room access for knocking participants,&#x20;
* control participant spotlighting and audio/video muting/unmuting
* sending chat messages,
* starting and stopping cloud recording (if enabled in the room)
* subscribing to room state changes

For a full overview of available in-room functionality refer to the [RoomConnectionClient reference pages](../../core-sdk-reference/api-reference/roomconnectionclient.md).
{% endhint %}

### Media

To obtain audio and/or media streams from the room or if you want to inject audio and/or video back in to the room from the Assistant then you can use the following media APIs to do so.

```jsx
import "@whereby.com/assistant-sdk/polyfills";

import { 
  Assistant,
  ASSISTANT_LEFT_ROOM,
} from "@whereby.com/assistant-sdk";

const assistant = new Assistant({
  assistantKey: process.env.ASSISTANT_KEY
});

assistant
  .joinRoom("https://your-subdomain.whereby.com/your-room-name")
  .then(({ roomUrl }) => {
    console.log("Assistant has joined the room: ", roomUrl);
  
    // To receive all audio data from the call:
    const combinedAudioSink = assistant.getCombinedAudioSink();
    // recieve raw PCM audio data of all audio in the room with e.g.:
    const unsubscribeCombinedAudioSink = combinedAudioSink.subscribe(({ samples, sampleRate }) => { 
      console.log(`${samples.length} samples received @ ${sampleRate}Hz`)
      // <your audio samples processing here>
    });
  
    // To inject audio and/or video data in to the call:
    assistant.startLocalMedia({
      audio: true,
      video: true,
    }).then(({ audioSource, videoSource }) => {
      setInterval(() => {
        console.log("Injecting media in to room");
    
        // continuously push raw PCM audio data in to the room with e.g.:
        audioSource.onData({ sampleRate: 8000, samples: new Int16Array(8000 / 100) });
    
        // continuously push raw I420 video data in to the room with e.g.:
        videoSource.onFrame({ width: 320, height: 240, data: new Uint8ClampedArray(320 * 240 * 1.5) });
      });
    }).catch((error) => {
      console.error("An error occurred starting local media", error);
      return;
    });
  
    assistant.on(ASSISTANT_LEFT_ROOM, ({ roomUrl }) => {
      console.log("Assistant has left the room: ", roomUrl);
    
      // Clean up any subscribers created above:
      unsubscribeCombinedAudioSink();
    })
  })
  .catch((error) => {
    console.error("An error occurred joining the room", error);
  });

```



<table><thead><tr><th width="187.4921875">Method</th><th width="180.984375">Parameters</th><th width="241.48046875">Returns</th><th>Description</th></tr></thead><tbody><tr><td><code>startLocalMedia</code></td><td><a href="../../core-sdk-reference/types/roomconnection-types.md#localmediaoptions-less-than-object-greater-than"><code>LocalMediaOptions</code></a></td><td><p><code>Promise&#x3C;{</code></p><p>  <code>audioSource:</code> <a href="../types/assistant-types.md#audiosource-less-than-object-greater-than"><code>AudioSource</code></a> <code>| null,</code></p><p><code>videoSource:</code> <a href="../types/assistant-types.md#videosource-less-than-object-greater-than"><code>VideoSource</code></a> <code>| null</code></p><p><code>}></code></p></td><td>Creates and starts local audio and/or video media for the assistant based on parameters and returns media objects that can be used to inject media data in to the room</td></tr><tr><td><code>stopLocalMedia</code></td><td></td><td><code>void</code></td><td>Stops local media for the assistant</td></tr><tr><td><code>getLocalMedia</code></td><td></td><td><a href="../../core-sdk-reference/api-reference/localmediaclient.md"><code>LocalMediaClient</code></a></td><td>Returns the underlying local media client controller. </td></tr><tr><td><code>getCombinedAudioSink</code></td><td></td><td> <a href="../types/assistant-types.md#audiosink-less-than-object-greater-than"><code>AudioSink</code></a> <code>| null</code></td><td>Returns a raw audio sink object containing all in-room audio combined on a single audio channel.</td></tr></tbody></table>

## Events

`Assistant` extends `EventEmitter` . You can listen to lifecycle and state change events directly:&#x20;

```jsx
import "@whereby.com/assistant-sdk/polyfills";

import { 
  Assistant,
  ASSISTANT_JOINED_ROOM,
  ASSISTANT_LEFT_ROOM,
  PARTICIPANT_VIDEO_TRACK_ADDED,
  PARTICIPANT_AUDIO_TRACK_ADDED,
} from "@whereby.com/assistant-sdk";

const mediaDataSubscriptions = [];

const assistant = new Assistant({
  assistantKey: process.env.ASSISTANT_KEY
});

assistant.on(ASSISTANT_JOINED_ROOM, ({ roomUrl }) => {
  console.log("Assistant has joined the room: ", roomUrl);
});

assistant.on(PARTICIPANT_AUDIO_TRACK_ADDED, ({ participantId, trackId, data }) => {
  console.log("Remote participant audio track available: ", participantId, trackId);
  
  mediaDataSubscriptions.push(
    // recieve raw PCM audio data of this audio track in the room with e.g.:
    data.subscribe(({ samples, sampleRate }) => { 
      console.log(`${samples.length} audio samples received @ ${sampleRate}Hz`);
    })
  );
});

assistant.on(PARTICIPANT_VIDEO_TRACK_ADDED, ({ participantId, trackId, data }) => {
  console.log("Remote participant video track available: ", participantId, trackId);
  
  mediaDataSubscriptions.push(
    // recieve raw I420 video data of this video track in the room with e.g.:
    data.subscribe(({ width, height, data }) => { 
      console.log(`${data.length} video samples received @ ${width}x${height} pixels`);
    })
  );
});

assistant.on(ASSISTANT_LEFT_ROOM, ({ roomUrl }) => {
  console.log("Assistant has left the room: ", roomUrl);
  
  // Clean-up all media data subscriptions on exit
  mediaDataSubscriptions.forEach(unsubscribeFn => unsubscribeFn());
});

try {
  void assistant.joinRoom("https://your-subdomain.whereby.com/your-room-name");
} catch(error) {
  console.error("An error occurred joining the room", error);
}; 

```

<table><thead><tr><th width="287.91796875">Event </th><th width="237.89678955078125">Payload</th><th>Emitted when</th></tr></thead><tbody><tr><td><code>ASSISTANT_JOINED_ROOM</code></td><td><code>{ roomUrl: string }</code></td><td>Assistant has joined the room</td></tr><tr><td><code>ASSISTANT_LEFT_ROOM</code></td><td><code>{ roomUrl: string }</code></td><td>Assistant has left the room</td></tr><tr><td><code>PARTICIPANT_VIDEO_TRACK_ADDED</code></td><td><p><code>{</code></p><p>   <code>participantId: string;</code> </p><p>   <code>trackId: string;</code> </p><p> <code>data:</code> <a href="../types/assistant-types.md#videosink-less-than-object-greater-than"><code>VideoSink</code></a> </p><p><code>}</code></p></td><td>A remote participant has added or changed a video track</td></tr><tr><td><code>PARTICIPANT_VIDEO_TRACK_REMOVED</code></td><td><p><code>{</code></p><p>   <code>participantId: string;</code> </p><p>   <code>trackId: string</code> </p><p><code>}</code></p></td><td>A remote participant has removed a video track</td></tr><tr><td><code>PARTICIPANT_AUDIO_TRACK_ADDED</code></td><td><p><code>{</code></p><p>   <code>participantId: string;</code> </p><p>   <code>trackId: string;</code> </p><p> <code>data:</code> <a href="../types/assistant-types.md#audiosink-less-than-object-greater-than"><code>AudioSink</code></a> </p><p><code>}</code></p></td><td>A remote participant has added or changed an audio track</td></tr><tr><td><code>PARTICIPANT_AUDIO_TRACK_REMOVED</code></td><td><p><code>{</code></p><p>   <code>participantId: string;</code> </p><p>   <code>trackId: string</code> </p><p><code>}</code></p></td><td>A remote participant has removed an audio track</td></tr></tbody></table>

