---
description: >-
  Let participants share a YouTube video or a Miro board with everyone in the
  room, using room integrations in the React-based Browser SDK.
---

# Sharing YouTube and Miro

Room integrations let a participant share a YouTube video or a Miro board with the whole room. One person starts it and everyone sees the same thing. When the presenter plays, pauses or seeks a YouTube video, everyone's player follows.

This guide builds the feature in four steps: pick, start, show, stop. It assumes you already have a room working with `useRoomConnection`. If you don't, start with the [Browser SDK quickstart](quick-start.md).

Room integrations are available in every room, on every plan.

## How it works

The Whereby app loads integrations straight into its own page. Your app runs on your domain, so the SDK can't do that. Instead, each integration runs in an iframe that Whereby serves from the integration's own origin, and the SDK talks to it with `postMessage`.

The hooks are headless. They give you `iframeProps` for an `<iframe>` that you render, place and size. `VideoGrid` uses the same hook to render integrations for you. Nothing inside the frame is yours to style. It's the integration's UI.

A share goes through these stages:

1. **Pick.** The user chooses a video or a board in the integration's picker, which runs in an iframe from [`useRoomIntegrationPicker`](../../reference/react-hooks-reference/useroomintegrationpicker.md). If you already have a link, skip the picker and use `roomIntegrationContent`.
2. **Start.** `actions.startRoomIntegration` checks the content and sends it to the server. Your state doesn't change yet.
3. **Share.** The server records the session and broadcasts it to everyone in the room, or in the current breakout group, including the sender.
4. **Show.** The session appears in `state.roomIntegrations.running`. `VideoGrid` shows it by default. In your own layout, [`useRoomIntegrationView`](../../reference/react-hooks-reference/useroomintegrationview.md) gives you the iframe that shows it.
5. **Sync.** When the content changes, for example the presenter pauses, the change goes to the server and comes back to everyone through the state. The hook sends each frame only the props that changed.
6. **Stop.** The person who started it, or a host, stops it with `actions.stopRoomIntegration` or the close control inside the content.

The room connection state is the source of truth. An iframe that reloads asks for the session's current props and catches up.

## Step 1: Offer what can be shared

`state.roomIntegrations.embeddable` lists the integrations your app can run. Today that's YouTube and Miro. The SDK fetches the list after you join, so it's empty until `hasFetched` is `true`.

```tsx
const { state, actions } = useRoomConnection(roomUrl, { localMediaOptions: { audio: true, video: true } });
const { roomIntegrations, connectionStatus } = state;
const [picking, setPicking] = React.useState<RoomIntegration | null>(null);

{roomIntegrations.embeddable.map((integration) => (
    <button
        key={integration.roomIntegrationId}
        disabled={connectionStatus !== "connected"}
        onClick={() => setPicking(integration)}
    >
        <img src={integration.icons.small} alt="" /> Share {integration.title}
    </button>
))}
```

Build the menu from `embeddable`, not `enabled`. `enabled` also contains integrations such as Google Docs and Trello that can't run in your app (see [Limitations](sharing-youtube-and-miro.md#limitations)).

## Step 2: Pick something to share

`useRoomIntegrationPicker` loads the integration's picker into an iframe you render. When the user picks something, `onPicked` receives `{ tagName, shareUrl, props }`.

```tsx
import { useRoomIntegrationPicker, type UseRoomIntegrationPickerOptions } from "@whereby.com/browser-sdk/react";

function Picker(props: UseRoomIntegrationPickerOptions) {
    const { iframeProps } = useRoomIntegrationPicker(props);
    return iframeProps ? <iframe {...iframeProps} className="picker" /> : null;
}
```

Render it while the user is picking, and remove it when they're done:

```tsx
{picking && (
    <div className="picker-dialog">
        <button onClick={() => setPicking(null)}>Close</button>
        <Picker
            integration={picking}
            onPicked={(content) => {
                actions.startRoomIntegration({ roomIntegrationId: picking.roomIntegrationId, ...content });
                setPicking(null);
            }}
            onCancel={() => setPicking(null)}
            onError={(error) => {
                console.warn("Picker failed", error.message);
                setPicking(null);
            }}
        />
    </div>
)}
```

A few rules for the picker frame:

* **Spread all of `iframeProps`, including `ref`.** The hook uses the ref to check that answers come from this frame. Without it, `onPicked` never fires. In development the hook warns you.
* **Give it room.** The picker is a full page. Something like 600 × 600 pixels works on desktop.
* **Don't add `sandbox`.** Some providers open a window to sign the user in, and a sandbox blocks it without an error.
* **One answer per mount.** The hook stops listening after the first answer. Unmounting the picker when the user is done, as above, gets you a fresh one next time.

### Share a link without the picker

If your app already knows what to share, such as a video attached to a lesson, build the content with `roomIntegrationContent` and skip the picker:

```tsx
import { roomIntegrationContent } from "@whereby.com/browser-sdk/react";

function shareVideo(url: string) {
    const youtube = state.roomIntegrations.embeddable.find((integration) => integration.name === "youtube");
    const content = roomIntegrationContent.youtube({ url });

    if (!youtube || !content) {
        return; // YouTube isn't available, or the link isn't a YouTube link
    }

    actions.startRoomIntegration({ roomIntegrationId: youtube.roomIntegrationId, ...content });
}
```

`youtube()` accepts most YouTube link formats and bare video ids, and reads the start time from `t=`. `miro()` only accepts Miro embed links (`miro.com/app/live-embed/...` from **Share → Embed** in Miro), not board URLs. Both return `null` for input they can't use. See [Building content without the picker](../../reference/react-hooks-reference/useroomconnection/room-integrations.md#building-content-without-the-picker) for every option.

## Step 3: Start

`startRoomIntegration` doesn't change your state on its own. It checks the content and sends it to the server. A moment later the server's broadcast adds the session to `state.roomIntegrations.running` for everyone, you included. Don't show the integration optimistically. Render from `running`.

If the local checks fail, or the server refuses, `state.roomIntegrations.error` is set instead. See [Handling errors](sharing-youtube-and-miro.md#handling-errors).

## Step 4: Show it

### Inside `VideoGrid`

If you use `VideoGrid`, you don't need to do anything. The grid shows running integrations by default, in an iframe that fills the integration's cell:

```tsx
<VideoGrid />
```

Here's where the grid puts running integrations:

* The first running integration takes the presentation stage. Spotlighted participants move to the video grid while it's there.
* Any other running integrations go to the subgrid. With `enableSubgrid={false}` they stay mounted in hidden cells instead.
* If you maximize a participant, the integration leaves the stage but stays mounted in a hidden cell, so a video keeps playing. When you un-maximize, it comes back where it was.
* The cell takes its shape from the content's aspect ratio, 16:9 unless the content says otherwise.

The grid keeps integration cells first in its list, with a stable key, so React never moves the iframe when participants join, leave or get spotlighted. See [Grid logic](grid-logic.md#room-integrations) for the full rules.

To draw the cell yourself, for example to add a "Shared by" label or a volume control, pass `renderIntegration`. The grid still decides where the cell goes and how big it is. Build the frame with `useRoomIntegrationView`:

```tsx
import { useRoomIntegrationView, type RoomIntegrationSessionView } from "@whereby.com/browser-sdk/react";

function IntegrationFrame({ session }: { session: RoomIntegrationSessionView }) {
    const { iframeProps } = useRoomIntegrationView({ session });

    return iframeProps ? (
        <iframe {...iframeProps} allowFullScreen style={{ border: "none", width: "100%", height: "100%" }} />
    ) : null;
}

<VideoGrid renderIntegration={({ session }) => <IntegrationFrame session={session} />} />;
```

To keep integrations out of the grid, for example because you show them in your own layout, pass `enableIntegrations={false}`. The grid then lays out participants as if nothing were running.

### In your own layout

Render a frame for each entry in `running`, keyed by the session id, using the `IntegrationFrame` component from above. If the same page also has a `VideoGrid`, pass it `enableIntegrations={false}` so the content doesn't appear twice.

```tsx
<div className="stage">
    {state.roomIntegrations.running.map((session) => (
        <div key={session.roomIntegrationSessionId} className="integration-box">
            <IntegrationFrame session={session} />
        </div>
    ))}
</div>
```

The hook needs the latest `session` object on every render to send prop changes to the frame. Reading it from `state.roomIntegrations.running` each render does that.

### Keep the iframe mounted

Moving an iframe to a different parent element reloads it, and so does unmounting and remounting it. For YouTube, a reload means the player loads from scratch and then seeks back to the room's position. Users notice.

* Use `session.roomIntegrationSessionId` as the key.
* When your layout changes, for example switching between a stage view and a sidebar, keep the frame in the same parent and change its size and position with CSS.
* Don't put the frame inside something that unmounts, such as a tab panel that's removed when hidden. Hide it with CSS instead.

### Volume and loading state

The view hook also returns `getVolume` and `setVolume` for a per-device volume control, and accepts `onContentReady` to tell you when the content has loaded. See [useRoomIntegrationView](../../reference/react-hooks-reference/useroomintegrationview.md).

## Step 5: Stop

Two people can stop an integration: the participant who started it, and a host. The SDK works this out for you in `session.canStop`.

```tsx
<button
    disabled={!session.canStop}
    onClick={() => actions.stopRoomIntegration({ roomIntegrationSessionId: session.roomIntegrationSessionId })}
>
    Stop sharing
</button>
```

Stopping removes the session from `running` for everyone, and their frames unmount.

Integrations can also close themselves. When the content sends a close message, for example when a YouTube video ends, `useRoomIntegrationView` stops the session if `canStop` is `true` for you. Participants who can't stop it ignore the message, so the session stops once, from the presenter's or a host's page.

Use `session.isPresenter` and `session.presenterDisplayName` for labels such as "Shared by Alex".

## Late joiners and breakout groups

People who join while an integration is running get it as part of joining. It's in `running` as soon as the room connects, and the frame starts from the session's current props, not the ones it was started with.

Integrations are scoped to breakout groups:

* Starting an integration inside a breakout group shares it with that group only.
* `running` only contains sessions for where you are now. In a group you see that group's sessions. In the main room you see the main room's.
* When you move between the main room and a group, `running` changes to match, and the frames for the old location unmount.

## Handling errors

`state.roomIntegrations.error` is `{ code, message }` or `null`. Show your own text based on `code`, since `message` is written for developers.

```tsx
const ERROR_TEXT: Partial<Record<RoomIntegrationError, string>> = {
    not_allowed_to_stop: "Only the person who shared this, or a host, can stop it.",
    integration_not_enabled: "This integration isn't available in this room.",
    invalid_content: "That link can't be shared.",
};

{roomIntegrations.error && (
    <p role="alert">{ERROR_TEXT[roomIntegrations.error.code] ?? "Something went wrong. Please try again."}</p>
)}
```

The error stays until the next successful start, stop or list fetch replaces it, so clear your banner in your own UI when the user dismisses it. The [reference](../../reference/react-hooks-reference/useroomconnection/room-integrations.md#errors) lists every code.

Picker failures don't go into `state`. They arrive through the picker's `onError`.

## Limitations

* **Only YouTube and Miro work in SDK apps.** Google Docs, Trello and generic integrations check the embedding origin on the provider's side and refuse to load inside your domain. They can be switched on for the room and show up in `enabled`, but `isEmbeddable` is `false` and they aren't in `embeddable`. If someone shares one from the Whereby app, it still appears in `running`, but the frame won't render it. Check `session.integration.isEmbeddable` and show a placeholder instead, in your own layout or from `renderIntegration`. `VideoGrid`'s default view doesn't do this check.
* **Miro needs an embed link.** A normal board URL won't work with `roomIntegrationContent.miro()`. Use the picker, or a link from **Share → Embed** in Miro.
* **Picker sign-in may open a window.** Some providers sign the user in through a separate window. Don't sandbox the picker frame.
* **One integration on the stage.** `VideoGrid` stages the first running integration. Others go to the subgrid. The participant menu's maximize and float actions only apply to participants, not to integrations.
* **No content-frame helper in core.** Outside React you speak the iframe protocol yourself. See [Room integrations without React](room-integrations-without-react.md).

## Full example

```tsx
import * as React from "react";
import {
    VideoGrid,
    useRoomConnection,
    useRoomIntegrationPicker,
    type RoomIntegration,
    type UseRoomIntegrationPickerOptions,
} from "@whereby.com/browser-sdk/react";

function Picker(props: UseRoomIntegrationPickerOptions) {
    const { iframeProps } = useRoomIntegrationPicker(props);
    return iframeProps ? <iframe {...iframeProps} style={{ border: "none", width: 600, height: 600 }} /> : null;
}

export function Room({ roomUrl }: { roomUrl: string }) {
    const { state, actions } = useRoomConnection(roomUrl, { localMediaOptions: { audio: true, video: true } });
    const { roomIntegrations } = state;
    const [picking, setPicking] = React.useState<RoomIntegration | null>(null);

    React.useEffect(() => {
        actions.joinRoom();
        return () => actions.leaveRoom();
    }, []);

    return (
        <>
            <div className="toolbar">
                {roomIntegrations.embeddable.map((integration) => (
                    <button key={integration.roomIntegrationId} onClick={() => setPicking(integration)}>
                        Share {integration.title}
                    </button>
                ))}
                {roomIntegrations.running.map((session) => (
                    <button
                        key={session.roomIntegrationSessionId}
                        disabled={!session.canStop}
                        onClick={() =>
                            actions.stopRoomIntegration({ roomIntegrationSessionId: session.roomIntegrationSessionId })
                        }
                    >
                        Stop {session.integration.title}
                    </button>
                ))}
            </div>

            {roomIntegrations.error && <p role="alert">{roomIntegrations.error.message}</p>}

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

            <div style={{ height: "70vh", width: "100%" }}>
                <VideoGrid />
            </div>
        </>
    );
}
```

The `Examples/Room integrations` stories in the [SDK repository](https://github.com/whereby/sdk) show the same pieces, with a message log of everything passing between the page and the frames.
