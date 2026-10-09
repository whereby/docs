# useRoomIntegrationView

```tsx
const { iframeProps, getVolume, setVolume } = useRoomIntegrationView({ session });
```

`useRoomIntegrationView` shows a running room integration, such as a shared YouTube video, in an `<iframe>` that you render. `VideoGrid` already uses it to show integrations, so you only need it in two cases: to draw the grid's integration cell yourself with [`renderIntegration`](videogrid.md#rendering-room-integrations), or to show integrations in a layout of your own.

The frame loads a page that Whereby serves from the integration's origin. The hook passes messages between that page and the room:

* When the frame says it's ready, the hook sends it the session's props. After that, it sends only the props that changed.
* When the content changes something, for example the presenter pauses the video, the hook sends the change to the room with `updateRoomIntegrationProps`. The change comes back to everyone, the sender included, through `state.roomIntegrations.running`, and the hook passes it on to each frame.
* When the content asks to close, the hook stops the session, but only if you're allowed to (`session.canStop`).

The store is the source of truth for the content's state, not the iframe. When a frame reloads, it gets the session's current props, not the ones it had when the session started.

The hook must be rendered inside a `WherebyProvider`, in the same tree as `useRoomConnection`.

## Options

| Option            | Required | Type                                                                          | Description |
| ----------------- | -------- | ----------------------------------------------------------------------------- | ----------- |
| `session`         | ✅       | [`RoomIntegrationSessionView`](types.md#roomintegrationsessionview)           | A session from `state.roomIntegrations.running`, or the `session` that `VideoGrid` passes to `renderIntegration`. Pass the latest object on every render so prop changes reach the frame. |
| `onContentReady`  |          | `() => void`                                                                  | Called when the content has loaded, for example when the YouTube player is ready. Use it to hide a loading state. |
| `onAudioOverride` |          | `(enabled: boolean \| null) => void`                                          | Called when the content asks you to change the local microphone. YouTube sends `false` to ask you to turn audio input off, and `null` to drop that request. The SDK doesn't act on this by itself. |
| `allow`           |          | `string`                                                                      | The iframe's `allow` attribute. Defaults to `"autoplay; fullscreen; encrypted-media; picture-in-picture"`. Without `autoplay`, browsers may refuse to start the video. |
| `title`           |          | `string`                                                                      | The iframe's `title`. Defaults to the integration's title, for example `"YouTube"`. |

## Return value

| Property      | Type                                                          | Description |
| ------------- | ------------------------------------------------------------- | ----------- |
| `iframeProps` | [`RoomIntegrationIframeProps`](types.md#roomintegrationiframeprops)` \| null` | Spread onto the `<iframe>` you render. `null` if the integration's `webview` URL can't be parsed. Render nothing in that case. |
| `getVolume`   | `() => Promise<number>`                                       | Ask the content for its current volume. YouTube answers with a number from `0` to `1`, and `0` when muted. Rejects if the frame doesn't answer within 2 seconds. |
| `setVolume`   | `(volume: number) => void`                                    | Set the content's volume on this device only. For YouTube, pass a number from `0` to `1`. |

Volume is local. Changing it doesn't affect anyone else in the room.

`iframeProps` is `{ ref, src, title, allow }`. Spread all of it. The frame needs a size, so give it one with `style` or `className`, and add `allowFullScreen` if you want the content's fullscreen button to work:

```tsx
return iframeProps ? (
    <iframe {...iframeProps} allowFullScreen style={{ border: "none", width: "100%", height: "100%" }} />
) : null;
```

### Why the `ref` matters

The hook ignores any message whose `event.origin` isn't the integration's origin or whose `event.source` isn't the `contentWindow` of the iframe the `ref` is attached to. Two frames from the same integration, such as two YouTube sessions, share an origin, so the origin alone can't tell them apart. Without the `ref` the hook never hears `whereby:frameReady`, never sends props, and the content doesn't start. In development the hook logs a warning when the `ref` isn't attached.

### Keep the iframe mounted

Moving an `<iframe>` to a different place in the DOM reloads it, and so does unmounting and mounting it again. For YouTube that means the video loads from scratch and then seeks to the room's position. To avoid it:

* Give the component that renders the frame a stable `key`, such as `session.roomIntegrationSessionId`.
* Don't move the frame between parents when your layout changes. Change its size and position with CSS instead.
* Don't make `src` change. It only depends on the integration's `webview` and the session id.

`VideoGrid` already does this for the cells it renders.

## Usage

```tsx
import * as React from "react";
import { useRoomConnection, useRoomIntegrationView, type RoomIntegrationSessionView } from "@whereby.com/browser-sdk/react";

function IntegrationFrame({ session }: { session: RoomIntegrationSessionView }) {
    const [loading, setLoading] = React.useState(true);
    const { iframeProps, setVolume } = useRoomIntegrationView({
        session,
        onContentReady: () => setLoading(false),
    });

    if (!iframeProps) {
        return null;
    }

    return (
        <div className="integration">
            {loading && <div className="spinner" />}
            <iframe {...iframeProps} allowFullScreen style={{ border: "none", width: "100%", height: "100%" }} />
            <input type="range" min={0} max={1} step={0.1} onChange={(e) => setVolume(Number(e.target.value))} />
        </div>
    );
}

function Stage({ roomUrl }: { roomUrl: string }) {
    const { state } = useRoomConnection(roomUrl, { localMediaOptions: { audio: true, video: true } });

    return (
        <>
            {state.roomIntegrations.running.map((session) => (
                <IntegrationFrame key={session.roomIntegrationSessionId} session={session} />
            ))}
        </>
    );
}
```

## Without React

There's no equivalent of this hook in `@whereby.com/core`. To show a running integration without React, implement the content frame messages yourself. The [iframe protocol reference](../core-sdk-reference/api-reference/room-integration-iframe-protocol.md) lists them, and [Room integrations without React](../../whereby-for-web-browser/react-based-browser-sdk/room-integrations-without-react.md) has a full example.
