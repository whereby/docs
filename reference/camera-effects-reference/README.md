# Camera Effects Reference

The **Whereby Camera Effects SDK** is the low-level foundation for applying real-time visual effects to camera streams within the Whereby ecosystem. It provides a set of predefined images and videos that can be used as background effects on an existing video stream. It's meant to be used alongside Whereby's other SDK's, like the React Hooks browser SDK, or Whereby Core.

### Use Case

Implementing camera effects such as background blur, background images, or video backgrounds for use with either `@whereby.com/core` or `@whereby.com/browser-sdk/react`

### Requirements

The Camera Effects SDK is intended for browser environments and requires access to modern browser APIs.

{% hint style="info" %}
**You no longer need to install `@whereby.com/camera-effects` separately.** As of `@whereby.com/core` v1.13 and `@whereby.com/browser-sdk` v3.24 it is bundled as a dependency of both packages. The effect code (and its assets) is still loaded on demand the first time an effect is used, so it has no impact on your initial bundle size if you don't use camera effects.
{% endhint %}

## Getting started

### Installation

Camera effects ship with `@whereby.com/core` and `@whereby.com/browser-sdk`, so installing either of those is all you need:

{% tabs %}
{% tab title="npm" %}
```bash
npm install @whereby.com/browser-sdk
# or, for the low-level core SDK
npm install @whereby.com/core
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @whereby.com/browser-sdk
# or, for the low-level core SDK
yarn add @whereby.com/core
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @whereby.com/browser-sdk
# or, for the low-level core SDK
pnpm add @whereby.com/core
```
{% endtab %}
{% endtabs %}

### Usage

Use the `getUsableCameraEffectPresets()` helper to list the presets that are supported in the current browser, then apply one with the `switchCameraEffect` action. Both are exposed by `@whereby.com/core` and `@whereby.com/browser-sdk/react`, so you never import `@whereby.com/camera-effects` directly.

{% tabs %}
{% tab title="React (browser-sdk)" %}
```tsx
import { useEffect, useState } from "react";
import { useRoomConnection, getUsableCameraEffectPresets } from "@whereby.com/browser-sdk/react";

function CameraEffects({ roomUrl }) {
    const { actions } = useRoomConnection(roomUrl, { localMediaOptions: { audio: true, video: true } });
    const { switchCameraEffect, clearCameraEffect } = actions;

    const [presets, setPresets] = useState<string[]>([]);

    // Loaded on demand; the camera effects code is only fetched when this runs.
    useEffect(() => {
        getUsableCameraEffectPresets().then(setPresets);
    }, []);

    return (
        <div>
            {presets.map((preset) => (
                <button key={preset} onClick={() => switchCameraEffect(preset)}>
                    {preset}
                </button>
            ))}
            <button onClick={() => clearCameraEffect()}>Remove effect</button>
        </div>
    );
}
```
{% endtab %}

{% tab title="Core" %}
```javascript
import { WherebyClient, getUsableCameraEffectPresets } from "@whereby.com/core";

const client = new WherebyClient();
const roomConnection = client.getRoomConnection();

// Join the room as usual

// List of available effects (loaded on demand)
const presets = await getUsableCameraEffectPresets();

// Switch to the selected effect
await roomConnection.switchCameraEffect(presets[0]);

// You can also apply a custom image background
await roomConnection.switchCameraEffectCustom("https://example.com/background.jpg");

// Remove the effect
await roomConnection.clearCameraEffect();
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
`@whereby.com/camera-effects` is still published as a standalone package and can be installed and imported directly for advanced use cases. For most integrations the helpers above are the recommended path.
{% endhint %}
