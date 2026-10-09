---
description: >-
  Share YouTube videos and Miro boards from an app built on @whereby.com/core,
  without React, or start them from Node.
---

# Room integrations without React

This guide covers room integrations for apps that use `@whereby.com/core` directly: Vue, Svelte, Angular, plain JavaScript, or a Node process. If you use React, follow [Sharing YouTube and Miro](sharing-youtube-and-miro.md) instead. The hooks there do most of what this page does by hand.

{% hint style="info" %}
**TODO:** add the `@whereby.com/core` and `@whereby.com/browser-sdk` versions that ship room integrations before publishing.
{% endhint %}

Read the [How it works](sharing-youtube-and-miro.md#how-it-works) section of the React guide first. The model is the same. Each integration runs in an iframe that Whereby serves from the integration's origin. Your page talks to it with `postMessage`. The room connection state is the source of truth.

Without React you need three pieces:

1. **A share menu**, built from `getState().roomIntegrations.embeddable`.
2. **A picker frame.** `@whereby.com/core` builds the URL, and `@whereby.com/browser-sdk` has a listener you can use without React.
3. **A content frame** for each running session. Core has no helper for this, so you implement the [content frame protocol](../../reference/core-sdk-reference/api-reference/room-integration-iframe-protocol.md#content-frame) yourself. The code below is under 100 lines.

The examples assume you've joined a room as in the [Core SDK quick start](../../reference/core-sdk-reference/quick-start.md):

```ts
import { WherebyClient } from "@whereby.com/core";

const client = new WherebyClient();
const roomConnection = client.getRoomConnection();

roomConnection.initialize({ roomUrl, localMediaOptions: { audio: true, video: true } });
await roomConnection.joinRoom();
```

## Watch the room integration state

Room integration state lives in `getState().roomIntegrations` (see [RoomConnectionClient: Room Integrations](../../reference/core-sdk-reference/api-reference/roomconnectionclient/room-integrations.md)). There's no dedicated subscription, so use `subscribe` and skip updates where `roomIntegrations` hasn't changed. `subscribe` doesn't call you straight away, so render once yourself as well:

```ts
let previous = null;

function onState(state) {
    if (state.roomIntegrations === previous) {
        return;
    }
    previous = state.roomIntegrations;

    renderShareMenu(state.roomIntegrations.embeddable);
    syncIntegrationFrames(state.roomIntegrations.running); // defined below
    renderError(state.roomIntegrations.error);
}

roomConnection.subscribe(onState);
onState(roomConnection.getState());
```

The list of integrations loads shortly after you join. `embeddable` is empty until `roomIntegrations.hasFetched` is `true`.

## Pick something to share

The picker is a page on the integration's origin. Load it with `roomIntegrationPickerUrl` and wait for one answer with `subscribeToRoomIntegrationPicker`:

```ts
import { roomIntegrationPickerUrl } from "@whereby.com/core";
import { subscribeToRoomIntegrationPicker } from "@whereby.com/browser-sdk/react";

function openPicker(integration, container) {
    const iframe = document.createElement("iframe");
    iframe.src = roomIntegrationPickerUrl({ integration, parentOrigin: window.location.origin });
    iframe.title = `${integration.title} picker`;
    iframe.allow = "clipboard-read; clipboard-write; fullscreen";
    iframe.style.cssText = "border: none; width: 600px; height: 600px";

    const unsubscribe = subscribeToRoomIntegrationPicker({
        pickerOrigin: new URL(integration.webview).origin,
        getSource: () => iframe.contentWindow,
        onOutcome: (outcome) => {
            iframe.remove();

            if (outcome.type === "submitted") {
                const { tagName, shareUrl, props } = outcome.content;
                roomConnection.startRoomIntegration({
                    roomIntegrationId: integration.roomIntegrationId,
                    tagName,
                    shareUrl,
                    props,
                });
            } else if (outcome.type === "error") {
                console.warn("Picker failed", outcome.error.message);
            }
            // outcome.type === "cancelled": the user closed the picker
        },
    });

    container.appendChild(iframe);

    // Call this if your own UI closes the picker first
    return () => {
        unsubscribe();
        iframe.remove();
    };
}
```

`subscribeToRoomIntegrationPicker` checks both the origin and the source window of every message, then reports the first `submitted`, `cancelled` or `error` outcome and stops listening. `getSource` is a function because `contentWindow` is `null` until the frame attaches.

Don't add a `sandbox` attribute. Some providers open a window to sign the user in, and a sandbox blocks it without an error.

{% hint style="info" %}
`subscribeToRoomIntegrationPicker` is exported from the `@whereby.com/browser-sdk/react` entry point, which needs `react` and `react-dom` installed even though the function doesn't use them. If you'd rather not install React, write the listener yourself with `ROOM_INTEGRATION_PICKER_MESSAGES` from `@whereby.com/core`:

```ts
import { ROOM_INTEGRATION_PICKER_MESSAGES } from "@whereby.com/core";

function onPickerMessage(event) {
    if (event.origin !== pickerOrigin || event.source !== iframe.contentWindow) {
        return;
    }
    const { type, payload } = event.data || {};
    if (type === ROOM_INTEGRATION_PICKER_MESSAGES.FORM_SUBMIT) {
        // payload: { tagName, shareUrl, props }
    } else if (type === ROOM_INTEGRATION_PICKER_MESSAGES.FORM_CLOSE) {
        // cancelled
    } else if (type === ROOM_INTEGRATION_PICKER_MESSAGES.ERROR) {
        // payload: { message }
    } else {
        return;
    }
    window.removeEventListener("message", onPickerMessage);
}
window.addEventListener("message", onPickerMessage);
```
{% endhint %}

### Or share a link directly

`roomIntegrationContent` builds the content from a YouTube link or a Miro embed link, with no picker:

```ts
import { roomIntegrationContent } from "@whereby.com/core";

const { embeddable } = roomConnection.getState().roomIntegrations;
const youtube = embeddable.find((integration) => integration.name === "youtube");
const content = roomIntegrationContent.youtube({ url: "https://youtu.be/dQw4w9WgXcQ?t=30" });

if (youtube && content) {
    roomConnection.startRoomIntegration({ roomIntegrationId: youtube.roomIntegrationId, ...content });
}
```

`startRoomIntegration` doesn't change local state. The session shows up in `roomIntegrations.running` when the server broadcasts it, for you and everyone else at about the same time.

## Show running integrations

Each running session needs an iframe that speaks the content frame protocol. `mountIntegrationFrame` creates one, handles its messages, and returns `update` and `destroy` functions:

```ts
const PROPS = "whereby:props";

function outboundProps(session) {
    return {
        ...session.props,
        ispresenter: session.isPresenter,
        presenterdisplayname: session.presenterDisplayName,
        roomintegrationsessionid: session.roomIntegrationSessionId,
        breakoutgroupid: session.breakoutGroupId,
    };
}

function mountIntegrationFrame(initialSession, container) {
    let session = initialSession;
    let lastSent = null; // null until the frame says it's ready

    const frameOrigin = new URL(session.integration.webview).origin;
    const src = new URL(session.integration.webview);
    src.searchParams.set("parentOrigin", window.location.origin);
    src.searchParams.set("roomintegrationsessionid", session.roomIntegrationSessionId);

    const iframe = document.createElement("iframe");
    iframe.src = src.href;
    iframe.title = session.integration.title;
    iframe.allow = "autoplay; fullscreen; encrypted-media; picture-in-picture";
    iframe.allowFullscreen = true;
    iframe.style.cssText = "border: none; width: 100%; height: 100%";

    // Always the exact origin, never "*"
    const post = (type, payload) => iframe.contentWindow?.postMessage({ type, payload }, frameOrigin);

    function onMessage(event) {
        // Both checks: other frames from the same integration share this origin
        if (event.origin !== frameOrigin || event.source !== iframe.contentWindow) {
            return;
        }
        const { type, payload } = event.data || {};
        const { roomIntegrationSessionId } = session;

        switch (type) {
            case "whereby:frameReady":
                // First load or a reload: send everything
                lastSent = outboundProps(session);
                post(PROPS, lastSent);
                break;
            case "whereby:updateProps":
                // Send to the room. The change comes back through the state and update() below.
                if (payload?.props) {
                    roomConnection.updateRoomIntegrationProps({ roomIntegrationSessionId, props: payload.props });
                }
                break;
            case "whereby:close":
                if (session.canStop) {
                    roomConnection.stopRoomIntegration({ roomIntegrationSessionId, intent: "end" });
                }
                break;
        }
    }

    window.addEventListener("message", onMessage);
    container.appendChild(iframe);

    return {
        update(nextSession) {
            session = nextSession;
            if (!lastSent) {
                return; // whereby:frameReady will send the full props
            }
            const next = outboundProps(session);
            const patch = {};
            for (const key of Object.keys(next)) {
                if (next[key] !== lastSent[key]) {
                    patch[key] = next[key];
                }
            }
            lastSent = next;
            if (Object.keys(patch).length) {
                post(PROPS, patch);
            }
        },
        destroy() {
            window.removeEventListener("message", onMessage);
            iframe.remove();
        },
    };
}
```

Then keep one frame per running session. Mount new sessions, update existing ones, and destroy the ones that stopped:

```ts
const frames = new Map(); // roomIntegrationSessionId -> { update, destroy }
const stage = document.querySelector("#stage");

function syncIntegrationFrames(running) {
    const ids = new Set(running.map((session) => session.roomIntegrationSessionId));

    for (const [id, frame] of frames) {
        if (!ids.has(id)) {
            frame.destroy();
            frames.delete(id);
        }
    }

    for (const session of running) {
        const existing = frames.get(session.roomIntegrationSessionId);
        if (existing) {
            existing.update(session);
        } else {
            frames.set(session.roomIntegrationSessionId, mountIntegrationFrame(session, stage));
        }
    }
}
```

`running` already only contains sessions for where you are, the main room or your breakout group. Late joiners get the running sessions when they join, so `syncIntegrationFrames` mounts them on the first state update.

### Optional messages

`mountIntegrationFrame` leaves out the messages you don't strictly need. Add them to the `switch` if you want them:

* `whereby:contentReady` tells you the content has loaded, so you can hide a spinner.
* `whereby:audioOverride` with `{ enabled: false }` asks you to turn the local microphone off, and `{ enabled: null }` drops that request.
* `whereby:setVolume` and `whereby:getVolume` control the volume on this device. YouTube takes `0` to `1`.

The [iframe protocol reference](../../reference/core-sdk-reference/api-reference/room-integration-iframe-protocol.md) has every payload.

### Don't move the iframe

Moving an iframe to another parent, or removing and re-adding it, reloads it. The frame posts `whereby:frameReady` again and the code above resends the full props, so nothing breaks, but a YouTube video restarts its player. Mount each frame once and change its size and position with CSS when your layout changes.

## Stop sharing

```ts
for (const session of roomConnection.getState().roomIntegrations.running) {
    if (session.canStop) {
        roomConnection.stopRoomIntegration({ roomIntegrationSessionId: session.roomIntegrationSessionId });
    }
}
```

`canStop` is `true` for the participant who started the session and for hosts. For anyone else, `stopRoomIntegration` sets the `not_allowed_to_stop` error and sends nothing.

## Start from Node

`roomIntegrationContent` doesn't need a browser, so a Node process in the room can start a share. An [Assistant](../../reference/assistant-sdk-reference/README.md) can queue up a video when a session starts, for example. The Assistant SDK installs `@whereby.com/core`, so you can import from both:

```ts
import "@whereby.com/assistant-sdk/polyfills";
import { Assistant } from "@whereby.com/assistant-sdk";
import { roomIntegrationContent, type RoomIntegrationsState } from "@whereby.com/core";

const assistant = new Assistant({ assistantKey: "my-assistant-key" });
await assistant.joinRoom("https://your-subdomain.whereby.com/your-room-name");

const roomConnection = assistant.getRoomConnection();

// The list of integrations loads after joining. Wait for it.
const roomIntegrations = await new Promise<RoomIntegrationsState>((resolve) => {
    const check = (state) => {
        if (state.roomIntegrations.hasFetched) {
            unsubscribe();
            resolve(state.roomIntegrations);
        }
    };
    const unsubscribe = roomConnection.subscribe(check);
    check(roomConnection.getState());
});

const youtube = roomIntegrations.embeddable.find((integration) => integration.name === "youtube");
const content = roomIntegrationContent.youtube({ url: "https://www.youtube.com/watch?v=dQw4w9WgXcQ" });

if (youtube && content) {
    roomConnection.startRoomIntegration({ roomIntegrationId: youtube.roomIntegrationId, ...content });
}
```

The Assistant is the presenter for that session, so it can stop it later. Everyone in the room still needs a browser to see the content. The Assistant only starts and stops sessions, it doesn't render anything.

Check `roomConnection.getState().roomIntegrations.error` after starting, in case the server refuses the request.

## Limitations

The [limitations in the React guide](sharing-youtube-and-miro.md#limitations) apply here too. Only YouTube and Miro render in SDK apps, Miro needs an embed link, and there's no core helper for the content frame.
