---
description: >-
  This guide will walk you through migrating from Twilio's Programmable Video
  platform to Whereby's Browser SDK.
---

# Twilio JS SDK Direct Migration

{% embed url="https://www.twilio.com/docs/video/javascript-getting-started#contents" %}

Whereby's Browser SDK is a suite of powerful integration options that allows developers to set up WebRTC-based video experiences in minutes! We offer a pre-built experience via our Web Component as well as a complete set of React Hooks, so there are options for teams of all sizes and capabilities.

This guide will mainly focus on our React library as this offers developers complete control over the video experience, and closely replicates the features that were offered in Programmable Video. If you're looking for a solution that can be quickly implemented check out our Twilio JS SDK Quick Migration guide.

## Where to start

Whereby and Twilio both have methods to manage connections to the room, control local media, and handle events within the room. The easiest place to start your migration is to swap these methods within your application.

Whereby and Twilio have structured these methods a bit different though, so there will need to be some refactoring necessary to compile the old Twilio requests.

### Key Differences

Twilio manages their SDK in a very piecemeal way, with small snippets being responsible for things like connecting to a room, managing local media, rendering remote participant streams, etc.

Whereby on the other hand has two React hooks that break the SDK into two key components: Managing the Video Experience, and Managing Local Media. Below you'll find an overview of both of these Hooks, and guidance on what Twilio snippets feed in to these larger hooks.

## Managing the Video Experience

### The useRoomConnection Hook

The primary hook in the React SDK is `useRoomConnection`. This hook contains three main constants that handle parts of your application: state, actions, and components. Additionally, this hook returns all of your video feeds (`localParticipants` and `remoteParticipants`), so this is where you will do any UI customizations that you're building yourself. Basically this hook contains everything you need to build a basic video app in one simple to manage place!

Below are some examples of the various data types that are available on these constants, as well as a basic demonstration of using this hook. These also link to our Type Definitions, which break out each type in greater detail

<table data-view="cards"><thead><tr><th align="center"></th><th></th><th></th><th></th><th></th><th></th></tr></thead><tbody><tr><td align="center"><strong>State</strong></td><td><a href="https://docs.whereby.com/reference/react-hooks-reference/types#remoteparticipant-less-than-object-greater-than"><code>localParticipant</code></a></td><td><a href="https://docs.whereby.com/reference/react-hooks-reference/types#remoteparticipant-less-than-object-greater-than"><code>remoteParticipants</code></a></td><td><a href="https://docs.whereby.com/reference/react-hooks-reference/types#screenshare-less-than-object-greater-than"><code>screenshares</code></a></td><td><a href="https://docs.whereby.com/reference/react-hooks-reference/types#chatmessage-less-than-object-greater-than"><code>chatMessages</code></a></td><td><a href="https://docs.whereby.com/reference/react-hooks-reference/types#roomconnectionoptions-less-than-object-greater-than"><code>RoomConnectionOptions</code></a></td></tr><tr><td align="center"><strong>Actions</strong></td><td><code>toggleCamera</code></td><td><code>toggleMicrophone</code></td><td><code>startScreenshare</code></td><td><code>stopScreenshare</code></td><td><a href="https://docs.whereby.com/reference/react-hooks-reference/types#chatmessage-less-than-object-greater-than"><code>sendChatMessage</code></a></td></tr><tr><td align="center"><strong>Components</strong></td><td><code>VideoView</code></td><td></td><td></td><td></td><td></td></tr></tbody></table>

#### Example useRoomConnection Implementation

```javascript
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

### High Level Overview

The table below is a summary of the various Twilio Methods that will migrate in to the `useRoomConnection` hook.

As you go through this guide you'll find that a lot of things that need to be handled manually in Twilio (for example participant join/leave events and media muting/unmuting) are automatically managed by our React hook. This means you have all the same capability and power afforded in Programmable Video with _significantly less_ code to maintain.

<table><thead><tr><th width="116">Use Case</th><th width="149.33333333333331">Twilio Methods</th><th>useRoomConnection Hook</th><th>Comments</th></tr></thead><tbody><tr><td>Connecting to the Room</td><td><code>connect({ name: 'existing-room'})</code></td><td><code>const { state, actions, components } = userRoomConnection("&#x3C;room_url>"</code></td><td>"existing-room" = "room_url"</td></tr><tr><td>Managing remote participants (other guests)</td><td><code>room.participants</code></td><td><a href="https://docs.whereby.com/reference/react-hooks-reference/types#remoteparticipant-less-than-object-greater-than"><code>remoteParticipants</code></a> </td><td>(<code>remoteParticipants</code> are part of <code>state</code>, and include an id and a stream [video/audio])</td></tr><tr><td>Managing the local participant</td><td><code>room.localParticipant</code></td><td><a href="https://docs.whereby.com/reference/react-hooks-reference/types#remoteparticipant-less-than-object-greater-than"><code>localParticipant</code></a></td><td><code>localParticipant</code> uses the default mic/cam unless specified.</td></tr><tr><td>Handling Mute Events for Remote Participants</td><td><a href="https://www.twilio.com/docs/video/javascript-getting-started#handle-remote-media-unmute-events">https://www.twilio.com/docs/video/javascript-getting-started#handle-remote-media-unmute-events</a></td><td>Automatically Managed</td><td>This is handled by room.participants as a part of state automatically</td></tr><tr><td>Muting/Unmuting Local Audio</td><td><code>room.localParticipant.audioTracks</code></td><td><code>toggleMicrophone</code></td><td>Used for muting and unmuting microphone for the local participant</td></tr><tr><td>Muting/Unmuting Local Video</td><td><code>room.localParticipant.videoTracks</code></td><td><code>toggleCamera</code></td><td>Used for muting and unmuting camera for the local participant</td></tr><tr><td>Handling Chat Messages</td><td>Not included in Programmable Video</td><td><code>chatMessages</code></td><td></td></tr></tbody></table>

***

## Managing Local Media

The `useLocalMedia` hook allows you to manage media devices with greater flexibility, but it's not required to have a successful implementation of Whereby. You can also use it to build a page that allows your users to test their device configurations outside of a Meeting Experience if you wish.

The `useRoomConnection` hook automatically selects the default mic/cam for your user's device, but it won't detect additional devices if they're connected. You might want this level of simplicity in your app, but if not you the `useLocalMedia` allows you to access all of the connected devices, and supply the desired device to `useRoomConnection`.

Below are some examples of the various data types that are available on this hook, as well as a basic demonstration of using this hook.

<table data-view="cards"><thead><tr><th align="center"></th><th></th><th></th><th></th></tr></thead><tbody><tr><td align="center"><strong>localMedia.state</strong></td><td><code>currentCameraDeviceId</code></td><td><code>cameraDevices</code></td><td><code>localStream</code></td></tr><tr><td align="center"><strong>localMedia.actions</strong></td><td><code>setCameraDevice</code></td><td><code>toggleCameraEnabled</code></td><td><code>toggleMicrophoneEnabled</code></td></tr></tbody></table>

## High Level Overview

<table><thead><tr><th>Use Case</th><th width="221">Twilio Methods</th><th width="207">useLocalMedia</th><th>Notes</th></tr></thead><tbody><tr><td>Access Local Devices</td><td><code>createLocalTracks</code></td><td><code>useLocalMedia</code></td><td></td></tr><tr><td>Mute Local Audio</td><td><code>room.localParticipant.audioTracks.forEach(publication => { publication.track.disable(); });</code></td><td><code>toggleCamera</code></td><td>These are actions that are included on <code>useRoomConnection</code></td></tr><tr><td>Unmute Local Audio</td><td><code>room.localParticipant.audioTracks.forEach(publication => { publication.track.enable(); });</code></td><td><code>toggleMicrophone</code></td><td><code>useRoomConnection</code></td></tr><tr><td>Mute Local Video</td><td><code>room.localParticipant.videoTracks.forEach(publication => { publication.track.disable(); });</code></td><td><code>toggleCamera</code></td><td><code>useRoomConnection</code></td></tr><tr><td>Unmute Local Video</td><td><code>room.localParticipant.videoTracks.forEach(publication => { publication.track.enable(); });</code></td><td><code>toggleMicrophone</code></td><td><code>useRoomConnection</code></td></tr></tbody></table>
