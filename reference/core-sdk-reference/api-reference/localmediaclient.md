# LocalMediaClient

<pre class="language-jsx"><code class="lang-jsx"><strong>const localMedia = client.getLocalMedia();
</strong></code></pre>

LocalMediaClient manages the local audio/video streams and state of a client. It exposes state subscriptions and provides actions to manage media and devices. If you want the client to join the session with media, you should start local media before joining the room. Local media is retrieved from the WherebyClient.

## Utilities

| Method     | Returns                                                           | Description                             |
| ---------- | ----------------------------------------------------------------- | --------------------------------------- |
| `getState` | [`LocalMediaState`](../types/localmedia-types.md#localmediastate) | Returns the entire local media state    |
| `destroy`  | `void`                                                            | Destroy the LocalMediaClient instance.  |

## State Subscriptions

All subscribe methods follow this format:

* Call signature: `subscribeX(callback: (payload: T) => void: () => void`
* Contract: Invokes a callback on initial subscription and whenever the state changes.
* Returns: an unsubscribe function

### Methods

<table><thead><tr><th width="278.92578125">Method</th><th width="214.03643798828125">Payload Type</th><th>Description</th></tr></thead><tbody><tr><td><code>subscribeCameraDeviceError</code></td><td><code>error: unknown | null</code></td><td>Emits local camera device error if detected</td></tr><tr><td><code>subscribeCameraDevices</code></td><td><code>cameraDevices:</code> <a href="../types/roomconnection-types.md#mediadeviceinfo"><code>MediaDeviceInfoa</code></a></td><td>Emits the local camera devices</td></tr><tr><td><code>subscribeIsSettingCameraDevice</code></td><td><code>isSetting: boolean</code></td><td>Emits true or false when the local camera device is being set </td></tr><tr><td><code>subscribeIsSettingMicrophoneDevice</code></td><td><code>isSetting: boolean</code></td><td>Emits true or false when the local microphone device is being set </td></tr><tr><td><code>subscribeMicrophoneDeviceError</code></td><td><code>error: unknown | null</code></td><td>Emits local microphone device error if detected</td></tr><tr><td><code>subscribeMicrophoneDevices</code></td><td><code>microphoneDevices:</code> <a href="../types/roomconnection-types.md#mediadeviceinfo"><code>MediaDeviceInfo</code></a></td><td>Emits the local microphone devices</td></tr><tr><td><code>subscribeSpeakerDevices</code></td><td><code>speakerDevices:</code> <a href="../types/roomconnection-types.md#mediadeviceinfo"><code>MediaDeviceInfo</code></a></td><td>Emits the local speaker devices</td></tr><tr><td><code>subscribeCurrentCamera</code></td><td><code>cameraId?: string</code></td><td>Emits the cameraId of the currently selected camera device</td></tr><tr><td><code>subscribeCurrentMicrophone</code></td><td><code>microphoneId?: string</code></td><td>Emits the cameraId of the currently selected microphone device</td></tr><tr><td><code>subscribeCurrentSpeaker</code></td><td><code>speakerId?: string</code></td><td>Emits the cameraId of the currently selected speaker device</td></tr><tr><td><code>subscribeLocalMediaStarting</code></td><td><code>starting: boolean</code></td><td>Emits true or false whether the localMedia is starting</td></tr><tr><td><code>subscribeLocalStream</code></td><td><code>stream?:</code> <a href="../types/roomconnection-types.md#mediastream"><code>MediaStream</code></a></td><td>Emits the local media stream</td></tr><tr><td><code>subscribeLocalMediaStartError</code></td><td><code>error: unknown | null</code></td><td>Emits any local media starting errors</td></tr></tbody></table>

## Actions

<table><thead><tr><th>Method</th><th width="199.00909423828125">Parameters</th><th>Returns</th><th>Description</th></tr></thead><tbody><tr><td><code>startMedia</code></td><td><code>options:</code> <a href="../types/roomconnection-types.md#localmediaoptions-less-than-object-greater-than"><code>LocalMediaOptions</code></a> <code>| MediaStream</code><br><em><code>(default { audio: true, video: true })</code></em></td><td><code>Promise&#x3C;void></code></td><td>Initializes local media (mic/camera). Pass constraints or an existing <a href="../types/roomconnection-types.md#mediastream"><code>MediaStream</code></a>.</td></tr><tr><td><code>toggleCamera</code></td><td><code>(enabled?: boolean)</code></td><td><code>void</code></td><td>Enables / disabled camera</td></tr><tr><td><code>toggleMicrophone</code></td><td><code>(enabled?: boolean)</code></td><td><code>void</code></td><td>Enables / disables microphone</td></tr><tr><td><code>toggleLowDataMode</code></td><td><code>(enabled?: boolean)</code></td><td><code>void</code></td><td>Enables / disables low data mode</td></tr><tr><td><code>setCameraDevice</code></td><td><code>(deviceId: string)</code></td><td><code>void</code></td><td>Set the camera device to the specified deviceId</td></tr><tr><td><code>setMicrophoneDevice</code></td><td><code>(deviceId: string)</code></td><td><code>void</code></td><td>Set the microphone device to the specified deviceId</td></tr><tr><td><code>setSpeakerDevice</code></td><td><code>(deviceId: string)</code></td><td><code>void</code></td><td>Set the speaker device to the specified deviceId</td></tr><tr><td><code>stopMedia</code></td><td></td><td><code>void</code></td><td>Stops local media</td></tr></tbody></table>

## Events

`LocalMediaClient` extends `EventEmitter` . You can listen to lifecycle and state change events directly:&#x20;

```jsx
import { LOCAL_STREAM_CHANGED } from "@whereby.com/core";

const localMedia = client.getLocalMedia();

function localMediaChanged(stream: MediaStream) {
    console.log("Local media has changed", stream);
}

localMedia.on(LOCAL_STREAM_CHANGED, localMediaChanged);
```

#### LocalMediaEvents

<table data-full-width="false"><thead><tr><th width="262.82122802734375">Event (constant)</th><th>Event name (string)</th><th width="208.276123046875">Payload</th><th>Emitted when</th></tr></thead><tbody><tr><td><code>CAMERA_DEVICE_ERROR_CHANGED</code></td><td><code>local-media:camera-device-error-changed</code></td><td><code>error: unknown | null</code></td><td>A camera error occurs</td></tr><tr><td><code>CAMERA_DEVICES_CHANGED</code></td><td><code>ocal-media:camera-devices-changed</code></td><td><code>cameraDevices:</code> <a href="../types/roomconnection-types.md#mediadeviceinfo"><code>MediaDeviceInfo</code></a></td><td>There's a change to camera devices</td></tr><tr><td><code>IS_SETTING_CAMERA_DEVICE</code></td><td><code>local-media:is-setting-camera-device</code></td><td><code>isSetting: boolean</code></td><td>Camera device is being set</td></tr><tr><td><code>IS_SETTING_MICROPHONE_DEVICE</code></td><td><code>local-media:is-setting-microphone-device</code></td><td><code>isSetting: boolean</code></td><td>Microphone device is being set</td></tr><tr><td><code>MICROPHONE_DEVICE_ERROR_CHANGED</code></td><td><code>ocal-media:microphone-device-error-changed</code></td><td><code>error: unknown | null</code></td><td>A microphone error occurs</td></tr><tr><td><code>MICROPHONE_DEVICES_CHANGED</code></td><td><code>ocal-media:microphone-devices-changed</code></td><td><code>microphoneDevices:</code> <a href="../types/roomconnection-types.md#mediadeviceinfo"><code>MediaDeviceInfo</code></a>  </td><td>There is a change to microphone devices</td></tr><tr><td><code>SPEAKER_DEVICES_CHANGED</code></td><td><code>local-media:speaker-devices-changed</code></td><td><code>speakerDevices:</code> <a href="../types/roomconnection-types.md#mediadeviceinfo"><code>MediaDeviceInfo</code></a></td><td>When there is a change to speaker devices</td></tr><tr><td><code>CURRENT_CAMERA_CHANGED</code></td><td><code>ocal-media:current-camera-changed</code></td><td><code>cameraId?: string</code></td><td>When selected camera is changed</td></tr><tr><td><code>CURRENT_MICROPHONE_CHANGED</code></td><td><code>local-media:current-microphone-changed</code></td><td><code>microphoneId?: string</code></td><td>When selected microphone is changed</td></tr><tr><td><code>CURRENT_SPEAKER_CHANGED</code></td><td><code>local-media:current-speaker-changed</code></td><td><code>speakerId?: string</code></td><td>When selected speaker is changed</td></tr><tr><td><code>LOCAL_MEDIA_STARTING</code></td><td><code>local-media:starting</code></td><td><code>starting: boolean</code></td><td>When local media is starting</td></tr><tr><td><code>LOCAL_STREAM_CHANGED</code></td><td><code>local-media:local-stream-changed</code></td><td><code>stream?: MediaStream</code></td><td>When media stream is changed</td></tr><tr><td><code>LOCAL_MEDIA_START_ERROR_CHANGED</code></td><td><code>ocal-media:start-error-changed</code></td><td><code>error: unknown | null</code></td><td>An error occus when starting local media</td></tr></tbody></table>
