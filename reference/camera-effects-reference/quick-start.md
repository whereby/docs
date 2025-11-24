---
description: >-
  The following code snippets show a basic example of using the camera effects
  SDK to create an effect stream and use it in the core SDK.
---

# Quick Start

1. Get the effect presets from the library

```jsx
   const [effectPresets, setEffectPresets] = React.useState<Array<string>>([]);
   
   // Lazy-loaded and can be called when needed
    async function loadBackgroundEffects() {
        const { getUsablePresets } = await import("@whereby.com/camera-effects");
        const usablePresets = getUsablePresets();
        setEffectPresets(usablePresets);
    }
```

2. Use the effect in the Core SDK

```jsx
  import { WherebyClient } from "@whereby.com/core";

    const client = new WherebyClient();
    const roomConnection = client.getRoomConnection();
    
    async function setCameraEffect(effectPreset: string) {
        await roomConnection.switchCameraEffect(effectPreset);
    }
```

