---
description: >-
  The following code snippets show a basic example of using the camera effects
  SDK to create an effect stream and use it in the core SDK.
---

# Quick Start

Camera effects ship with `@whereby.com/core` and `@whereby.com/browser-sdk` — there's nothing extra to install. The effect code is loaded on demand the first time you use it.

1. Get the usable effect presets for the current browser

{% tabs %}
{% tab title="React (browser-sdk)" %}
```jsx
import { getUsableCameraEffectPresets } from "@whereby.com/browser-sdk/react";

const [effectPresets, setEffectPresets] = React.useState<Array<string>>([]);

// Lazy-loaded; can be called when needed
async function loadBackgroundEffects() {
    const usablePresets = await getUsableCameraEffectPresets();
    setEffectPresets(usablePresets);
}
```
{% endtab %}

{% tab title="Core" %}
```jsx
import { getUsableCameraEffectPresets } from "@whereby.com/core";

// Lazy-loaded; can be called when needed
const effectPresets = await getUsableCameraEffectPresets();
```
{% endtab %}
{% endtabs %}

2. Switch to one of the presets using the room connection



```jsx
import { useRoomConnection } from "@whereby.com/browser-sdk/react";

const { actions } = useRoomConnection(roomUrl, { localMediaOptions: { audio: true, video: true } });
const { switchCameraEffect, clearCameraEffect } = actions;

// Apply a preset
await switchCameraEffect(effectPresets[0]);

// ...or remove the active effect
await clearCameraEffect();
```
