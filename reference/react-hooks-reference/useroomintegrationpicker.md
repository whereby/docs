# useRoomIntegrationPicker

```tsx
const { iframeProps } = useRoomIntegrationPicker({ integration, onPicked, onCancel });
```

`useRoomIntegrationPicker` loads an integration's own picker, for example YouTube search or Miro's board browser, into an `<iframe>` that you render. When the user picks something, the hook calls `onPicked` with the content to pass to [`startRoomIntegration`](useroomconnection/room-integrations.md).

The hook is headless. Whereby serves the picker page from the integration's origin, so everything inside the frame is the integration's UI. You decide where the frame goes, how big it is, and when it closes. The hook builds the URL and accepts the answer only when it comes from that frame.

{% hint style="info" %}
If you already have a YouTube link or a Miro embed link, you don't need a picker. Use [`roomIntegrationContent`](useroomconnection/room-integrations.md#building-content-without-the-picker) instead.
{% endhint %}

## Options

| Option          | Required | Type                                                                                   | Description |
| --------------- | -------- | -------------------------------------------------------------------------------------- | ----------- |
| `integration`   | ✅       | [`RoomIntegration`](types.md#roomintegration)                                          | The integration to pick content for. Take it from `state.roomIntegrations.embeddable`. |
| `onPicked`      | ✅       | `(content: RoomIntegrationPickerResult) => void`                                       | Called once with `{ tagName, shareUrl, props }` when the user picks something. |
| `onCancel`      |          | `() => void`                                                                           | Called when the user closes the picker from inside the frame. |
| `onError`       |          | `(error: Error) => void`                                                               | Called when the picker page fails to load. `error.message` comes from the picker. |
| `featureSource` |          | `string`                                                                               | Passed to the picker as `featuresource`. The picker page defaults to `"sdk"`. |
| `allow`         |          | `string`                                                                               | The iframe's `allow` attribute. Defaults to `"clipboard-read; clipboard-write; fullscreen"`, so users can paste links into the picker. |
| `title`         |          | `string`                                                                               | The iframe's `title`, for screen readers. Defaults to `"<integration title> picker"`, for example `"YouTube picker"`. |

The hook stores the latest callbacks in refs, so you can pass inline functions without restarting the picker.

## Return value

| Property      | Type                                                          | Description |
| ------------- | ------------------------------------------------------------- | ----------- |
| `iframeProps` | [`RoomIntegrationIframeProps`](types.md#roomintegrationiframeprops)` \| null` | Spread onto the `<iframe>` you render. `null` if the integration's `webview` URL can't be parsed. Render nothing in that case. |

`iframeProps` is `{ ref, src, title, allow }`. Spread all of it, and add your own `className` or `style`:

```tsx
return iframeProps ? <iframe {...iframeProps} className="picker" /> : null;
```

### Why the `ref` matters

The hook only accepts a message when `event.origin` is the picker's origin and `event.source` is the `contentWindow` of the iframe it gave you the `ref` for. The origin check stops other sites from answering, and the source check stops a second frame from the same origin, such as a running YouTube integration, from answering for the picker.

If you build your own `<iframe src={iframeProps.src}>` and drop the `ref`, the hook can't match the window, and `onPicked` never fires. In development the hook logs a warning when the `ref` isn't attached.

### Don't sandbox the frame

Some providers open a window of their own to sign the user in. A `sandbox` attribute without `allow-popups` blocks that window without any error. In development the hook warns if the iframe has a `sandbox` attribute.

## One answer per mount

The hook listens for a single outcome, then stops listening. After `onPicked`, `onCancel` or `onError`, it ignores further messages from the frame. To let the user pick again, unmount the component that calls the hook and mount it again. Rendering the picker only while the user is picking, as in the example below, does this for you.

## Usage

```tsx
import * as React from "react";
import {
    useRoomConnection,
    useRoomIntegrationPicker,
    type RoomIntegration,
    type UseRoomIntegrationPickerOptions,
} from "@whereby.com/browser-sdk/react";

function Picker(props: UseRoomIntegrationPickerOptions) {
    const { iframeProps } = useRoomIntegrationPicker(props);
    return iframeProps ? <iframe {...iframeProps} style={{ border: "none", width: 600, height: 600 }} /> : null;
}

function ShareMenu({ roomUrl }: { roomUrl: string }) {
    const { state, actions } = useRoomConnection(roomUrl, { localMediaOptions: { audio: true, video: true } });
    const [picking, setPicking] = React.useState<RoomIntegration | null>(null);

    return (
        <>
            {state.roomIntegrations.embeddable.map((integration) => (
                <button key={integration.roomIntegrationId} onClick={() => setPicking(integration)}>
                    Share {integration.title}
                </button>
            ))}

            {picking && (
                <Picker
                    integration={picking}
                    onPicked={(content) => {
                        actions.startRoomIntegration({ roomIntegrationId: picking.roomIntegrationId, ...content });
                        setPicking(null);
                    }}
                    onCancel={() => setPicking(null)}
                    onError={() => setPicking(null)}
                />
            )}
        </>
    );
}
```

## Outside React

`subscribeToRoomIntegrationPicker` is the listener this hook uses, exported for apps that don't use React. See [Room integrations without React](../../whereby-for-web-browser/react-based-browser-sdk/room-integrations-without-react.md).
