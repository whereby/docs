# getUsablePresets

Camera effects are controlled through helpers and room-connection actions exposed by `@whereby.com/core` and `@whereby.com/browser-sdk/react`. You don't need to import `@whereby.com/camera-effects` directly.

### getUsableCameraEffectPresets

```jsx
import { getUsableCameraEffectPresets } from "@whereby.com/core";
// or: from "@whereby.com/browser-sdk/react"

const presets = await getUsableCameraEffectPresets();
```

`getUsableCameraEffectPresets(): Promise<string[]>`

Returns the list of camera effect preset ids that are usable in the current browser. The camera effects code is loaded on demand the first time this is called. Pass one of the returned ids to the `switchCameraEffect` action to apply it.

### Room connection actions

The following actions are available on the `RoomConnectionClient` (Core) and in the `actions` object returned by `useRoomConnection`:

| Action                     | Parameters           | Returns         | Description                                                                             |
| -------------------------- | -------------------- | --------------- | --------------------------------------------------------------------------------------- |
| `switchCameraEffect`       | `(effectId: string)` | `Promise<void>` | Enable a camera effect. Use `getUsableCameraEffectPresets()` to get a valid `effectId`. |
| `switchCameraEffectCustom` | `(imageUrl: string)` | `Promise<void>` | Enable a camera effect that uses a custom image (from the given URL) as the background. |
| `clearCameraEffect`        |                      | `Promise<void>` | Disable the active camera effect.                                                       |

{% hint style="info" %}
For advanced, low-level use you can still install and import `@whereby.com/camera-effects` directly. Its `getUsablePresets()` function is what `getUsableCameraEffectPresets()` wraps.
{% endhint %}
