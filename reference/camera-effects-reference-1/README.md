# Audio Denoiser Reference

The **Whereby Audio Denoiser SDK** is the low-level foundation for applying real-time noise suppression to a microphone stream within the Whereby ecosystem. It removes background noise — keyboard typing, fans, street noise — from the local participant's audio before it is sent to the room. It's meant to be used alongside Whereby's other SDKs, like the React Hooks browser SDK, or Whereby Core.

#### Use Case

Adding microphone noise suppression for use with either `@whereby.com/core` or `@whereby.com/browser-sdk/react`.

#### Requirements

The Audio Denoiser SDK is intended for browser environments and requires access to modern browser APIs, specifically `AudioContext` and `AudioWorkletNode`. Use the `isAudioDenoiserSupported()` helper to check support before exposing the feature in your UI.

{% hint style="info" %}
**You don't need to install `@whereby.com/audio-denoiser` separately.** As of `@whereby.com/core` v1.13 and `@whereby.com/browser-sdk` v3.24 it is bundled as a dependency of both packages. The denoiser code (and its WebAssembly assets) is loaded on demand the first time noise suppression is enabled, so it has no impact on your initial bundle size if you don't use it.
{% endhint %}

### Getting started

#### Installation

The audio denoiser ships with `@whereby.com/core` and `@whereby.com/browser-sdk`, so installing either of those is all you need:

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

#### Usage

Use the `isAudioDenoiserSupported()` helper to check browser support, then toggle noise suppression with the `enableAudioDenoiser` / `disableAudioDenoiser` actions. Both the helper and the actions are exposed by `@whereby.com/core` and `@whereby.com/browser-sdk/react`, so you never import `@whereby.com/audio-denoiser` directly. Once enabled, the denoiser is automatically re-applied if the microphone device is later switched.

{% tabs %}
{% tab title="React (browser-sdk)" %}
```tsx
import { useEffect, useState } from "react";
import { useRoomConnection, isAudioDenoiserSupported } from "@whereby.com/browser-sdk/react";

function NoiseSuppression({ roomUrl }) {
    const { actions } = useRoomConnection(roomUrl, { localMediaOptions: { audio: true, video: true } });
    const { enableAudioDenoiser, disableAudioDenoiser } = actions;

    const [supported, setSupported] = useState(false);
    const [enabled, setEnabled] = useState(false);

    // Loaded on demand; the denoiser code is only fetched when this runs.
    useEffect(() => {
        isAudioDenoiserSupported().then(setSupported);
    }, []);

    if (!supported) return null;

    return (
        <button
            onClick={async () => {
                if (enabled) {
                    await disableAudioDenoiser();
                } else {
                    await enableAudioDenoiser();
                }
                setEnabled(!enabled);
            }}
        >
            {enabled ? "Turn off noise suppression" : "Turn on noise suppression"}
        </button>
    );
}
```
{% endtab %}

{% tab title="Core" %}
```javascript
import { WherebyClient, isAudioDenoiserSupported } from "@whereby.com/core";

const client = new WherebyClient();
const roomConnection = client.getRoomConnection();

// Join the room as usual

// Check browser support (loaded on demand)
if (await isAudioDenoiserSupported()) {
    // Enable noise suppression on the local microphone
    await roomConnection.enableAudioDenoiser();

    // ...later
    await roomConnection.disableAudioDenoiser();
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
`@whereby.com/audio-denoiser` is still published as a standalone package and can be installed and imported directly for advanced use cases. For most integrations the helpers above are the recommended path.
{% endhint %}
