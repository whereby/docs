# useLocalMedia

The `useLocalMedia` hook enables preview and selection of local devices (camera & microphone) prior to establishing a connection within a Whereby room. Use this hook to build rich pre-call and waiting room experiences, allowing end users to confirm their device selection up-front. This hook works seamlessly with the [useroomconnection.md](useroomconnection.md "mention") hook.

`useLocalMedia(mediaStreamConstraints: MediaStreamConstraints): Object | null`

## Parameters

<table><thead><tr><th width="237">Parameter</th><th width="97">Required</th><th width="149">Type</th><th>Description</th></tr></thead><tbody><tr><td>mediaStreamConstraints</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td><a href="types.md#mediastreamconstraints"><code>MediaStreamConstraints</code></a></td><td>Options to use for initializing local media</td></tr></tbody></table>

## Return type

The hook returns a LocalMediaReference object with the following properties.

<table><thead><tr><th>Property</th><th width="279.3333333333333">Type</th><th>Description</th></tr></thead><tbody><tr><td>state</td><td><a href="uselocalmedia.md#state"><code>LocalMediaState</code></a></td><td>Object representing the state of local media</td></tr><tr><td>actions</td><td><a href="uselocalmedia.md#actions"><code>LocalMediaActions</code></a></td><td>Object representing the available actions on local media</td></tr></tbody></table>

### state

The state of local media

<table><thead><tr><th width="263.3333333333333">Property</th><th width="264">Type</th><th>Description</th></tr></thead><tbody><tr><td>currentCameraDeviceId</td><td><code>string | undefined</code></td><td>The Id of the current camera device</td></tr><tr><td>currentMicrophoneDeviceId</td><td><code>string | undefined</code></td><td>The id of the current microphone device</td></tr><tr><td>cameraDeviceError</td><td><code>unknown</code></td><td></td></tr><tr><td>cameraDevices</td><td><a href="types.md#mediadeviceinfo"><code>MediaDeviceInfo</code></a><code>[]</code></td><td>List of the available camera devices</td></tr><tr><td>isSettingCameraDevice</td><td><code>boolean</code></td><td></td></tr><tr><td>isSettingMicrophoneDevice</td><td><code>boolean</code></td><td></td></tr><tr><td>isStarting</td><td><code>boolean</code></td><td></td></tr><tr><td>localStream</td><td><a href="types.md#mediastream"><code>MediaStream</code></a><code>| undefined</code></td><td>The stream of local media (camera &#x26; microphone)</td></tr><tr><td>lowDataMode</td><td>boolean</td><td>State of low data mode</td></tr><tr><td>microphoneDeviceError</td><td><code>unknown</code></td><td></td></tr><tr><td>microphoneDevices</td><td><a href="types.md#mediadeviceinfo"><code>MediaDeviceInfo</code></a><code>[]</code></td><td>List of the available microphone devices</td></tr><tr><td>speakerDevices</td><td><a href="types.md#mediadeviceinfo"><code>MediaDeviceInfo</code></a><code>[]</code></td><td>List of the available speaker devices</td></tr><tr><td>startError</td><td><code>unknown</code></td><td></td></tr></tbody></table>

### actions

The actions property contains a map of functions which can be invoked to perform an action on local media. All these functions are sync and return `void`, and you should rely on the [state](uselocalmedia.md#state) to render the effect of their invocation.

<table><thead><tr><th width="267.97265625">Name</th><th>Parameters</th><th>Description</th></tr></thead><tbody><tr><td>setCameraDevice</td><td><code>(deviceId: string) => void</code></td><td>Change the current camera device</td></tr><tr><td>setMicrophoneDevice</td><td><code>(deviceId: string) => void</code></td><td>Change the current microphone device</td></tr><tr><td>setSpeakerDevice</td><td><code>(deviceId: string) => void</code></td><td>Change the current speaker device</td></tr><tr><td>toggleCameraEnabled</td><td><code>(enabled?: boolean) => void</code></td><td>Change the state of your camera. Omitting the <code>enabled</code> parameter toggles the current value</td></tr><tr><td>toggleMicrophoneEnabled</td><td><code>(enabled?: boolean) => void</code></td><td>Change the state of your microphone. Omitting the <code>enabled</code> parameter toggles the current value</td></tr><tr><td>toggleHdModeEnabled</td><td><code>(enabled?: boolean) => void</code></td><td>Change the state of video quality between high-definition and standard-definition. Default <code>true</code></td></tr><tr><td>toggleLowDataModeEnabled</td><td><code>(enabled?: boolean) => void</code></td><td>Change the state of low data mode. Omitting the <code>enabled</code> parameter toggles the current value</td></tr><tr><td>toggleWidescreenModeEnabled</td><td><code>(enabled?: boolean) => void</code></td><td>Change the state of video width/height between 16:9 (widescreen) and 4:3 (standard). Default <code>true</code></td></tr></tbody></table>

## Usage

```tsx
import { useLocalMedia, VideoView } from "@whereby.com/browser-sdk/react";

function MyPreCallUX() {
    const localMedia = useLocalMedia({ audio: false, video: true });

    const { currentCameraDeviceId, cameraDevices, localStream } = localMedia.state;
    const { setCameraDevice, toggleCameraEnabled } = localMedia.actions;

    return <div className="preCallView">
        { /* Render any UI, making use of state */ }
        { cameraDevices.map((d) => (
            <p
                key={d.deviceId}
                onClick={() => {
                    if (d.deviceId !== currentCameraDeviceId) {
                        setCameraDevice(d.deviceId);
                    }
                }}
            >
                {d.label}
            </p>
        )) }
        <VideoView muted stream={localStream} />
    </div>;
}
```
