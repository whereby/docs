# Room Integration iframe Protocol

Room integrations run in iframes that Whereby serves from the integration's origin. Your page talks to them with `window.postMessage`. In React, [`useRoomIntegrationPicker`](../../react-hooks-reference/useroomintegrationpicker.md) and [`useRoomIntegrationView`](../../react-hooks-reference/useroomintegrationview.md) handle all of this for you. Read this page if you use `@whereby.com/core` without React, or want to know what the hooks do.

There are two kinds of frame:

* The **picker frame** lets the user choose what to share. It sends back one answer.
* The **content frame** shows a running session. It stays open and keeps sending and receiving messages until the session stops.

Every message, in both directions, has this shape:

```ts
{ type: string; payload?: unknown }
```

## Security rules

Your page and the frames trust each other only through these checks. Skip one and another page or frame can impersonate the integration.

* **Post to the exact origin.** Compute the frame's origin with `new URL(integration.webview).origin` and pass it as the `targetOrigin` of every `postMessage`. Never use `"*"`. The props you send include the session id and the presenter's name.
* **Check `event.origin`.** Ignore any message whose origin isn't the frame's origin.
* **Check `event.source`.** Ignore any message whose `event.source` isn't your iframe's `contentWindow`. Two frames from the same integration share an origin, so the origin check alone can't tell a picker from a content frame, or one YouTube session from another. Read `iframe.contentWindow` when the message arrives rather than saving it earlier. It's `null` until the frame attaches and changes when the frame reloads.
* **Tell the frame your origin.** Both URLs carry a `parentOrigin` query parameter set to `window.location.origin`. The frame posts only to that origin and only accepts messages from it.
* **Don't add `sandbox`.** Some providers open a window to sign the user in, and a sandbox without `allow-popups` blocks it without any error.

## Picker frame

### URL

Build it with `roomIntegrationPickerUrl` from `@whereby.com/core`:

```ts
import { roomIntegrationPickerUrl } from "@whereby.com/core";

const src = roomIntegrationPickerUrl({
    integration, // a RoomIntegration; only `webview` is read
    parentOrigin: window.location.origin,
    featureSource: "my-app", // optional
});
// => <webview>/bootstrap.html?parentOrigin=https%3A%2F%2Fexample.com&featuresource=my-app
```

Use `allow="clipboard-read; clipboard-write; fullscreen"` so users can paste links into it.

### Messages

The picker only sends. It doesn't expect any message from your page. The message types are exported from `@whereby.com/core` as `ROOM_INTEGRATION_PICKER_MESSAGES`:

| Constant      | `type`                   | Payload                                       | Sent when |
| ------------- | ------------------------ | --------------------------------------------- | --------- |
| `FORM_SUBMIT` | `whereby:formSubmit`     | `{ tagName, shareUrl, props, featureSource? }` | The user picked something. Pass `tagName`, `shareUrl` and `props` to `startRoomIntegration`. You can ignore `featureSource`. |
| `FORM_CLOSE`  | `whereby:formClose`      | none                                          | The user closed the picker without choosing. |
| `ERROR`       | `whereby:bootstrapError` | `{ message: string }`                         | The picker page failed to load. |

Treat the first of these as the answer and stop listening. `subscribeToRoomIntegrationPicker` in `@whereby.com/browser-sdk/react` does exactly that, and you can use it without React (see [Room integrations without React](../../../whereby-for-web-browser/react-based-browser-sdk/room-integrations-without-react.md)).

## Content frame

### URL

Start from the integration's `webview` URL and add two query parameters:

```ts
const url = new URL(session.integration.webview);
url.searchParams.set("parentOrigin", window.location.origin);
url.searchParams.set("roomintegrationsessionid", session.roomIntegrationSessionId);
const src = url.href;
```

Use `allow="autoplay; fullscreen; encrypted-media; picture-in-picture"`, and add `allowfullscreen`.

### Startup

1. The frame loads and posts `whereby:frameReady`.
2. You reply with `whereby:props`, carrying the session's full props (see [Props you send](room-integration-iframe-protocol.md#props-you-send)).
3. The frame applies them and renders the content. If it gets no props within 3 seconds, it logs a warning and renders anyway.
4. When the content has loaded, the frame posts `whereby:contentReady`.

The frame posts `whereby:frameReady` once per load. If it reloads, it posts it again, and you send the full props again.

### Frame to your page

| `type`                  | Payload                                                         | What to do |
| ----------------------- | --------------------------------------------------------------- | ---------- |
| `whereby:frameReady`    | none                                                            | Send `whereby:props` with the full props. |
| `whereby:updateProps`   | `{ props, roomIntegrationSessionId, breakoutGroupId }`          | Call `roomConnection.updateRoomIntegrationProps({ roomIntegrationSessionId: session.roomIntegrationSessionId, props: payload.props })`. Don't apply the props to the frame yourself. They come back from the room. |
| `whereby:contentReady`  | `{ roomIntegrationSessionId, breakoutGroupId }`                 | Optional. The content has loaded, so you can hide a loading state. |
| `whereby:close`         | none                                                            | If `session.canStop` is `true`, call `roomConnection.stopRoomIntegration({ roomIntegrationSessionId, intent: "end" })`. Otherwise ignore it. YouTube sends this when the video ends. |
| `whereby:audioOverride` | `{ enabled: boolean \| null }`                                  | Optional. The content asks you to change the local microphone. `false` means turn audio input off, `null` means the request is over. |
| `whereby:volume`        | `{ requestId: number, volume: number }`                         | The answer to a `whereby:getVolume` with the same `requestId`. |

Use the session id from your own session object, not the one in the payload.

### Your page to the frame

| `type`              | Payload                     | Effect |
| ------------------- | --------------------------- | ------ |
| `whereby:props`     | `RoomIntegrationProps`      | The frame sets each key as an attribute on the content element. It skips keys whose value is `null` or `undefined`. Send the full props after `whereby:frameReady`, then only the keys that changed. |
| `whereby:setVolume` | `{ volume: number }`        | Sets the volume on this device. YouTube takes `0` to `1`. |
| `whereby:getVolume` | `{ requestId: number }`     | Asks for the volume. The frame answers with `whereby:volume` and the same `requestId`. Time out if no answer comes. The React hook waits 2 seconds. |

### Props you send

Send the session's `props`, plus four keys that tell the content who it's running for:

| Key                        | Value |
| -------------------------- | ----- |
| `ispresenter`              | `session.isPresenter` |
| `presenterdisplayname`     | `session.presenterDisplayName` |
| `roomintegrationsessionid` | `session.roomIntegrationSessionId` |
| `breakoutgroupid`          | `session.breakoutGroupId` |

```ts
function outboundProps(session) {
    return {
        ...session.props,
        ispresenter: session.isPresenter,
        presenterdisplayname: session.presenterDisplayName,
        roomintegrationsessionid: session.roomIntegrationSessionId,
        breakoutgroupid: session.breakoutGroupId,
    };
}
```

YouTube uses `ispresenter` to decide whose play, pause and seek actions are sent to the room.

### Keeping the frame in sync

Whenever the session in the room connection state changes, compute `outboundProps(session)` again, compare it with what you last sent, and post `whereby:props` with only the keys that differ. Don't send anything before `whereby:frameReady`. Reset what you last sent when the frame reloads.

The loop goes like this. The content posts `whereby:updateProps`, you call `updateRoomIntegrationProps`, the server broadcasts the change, the new props show up in `getState().roomIntegrations.running`, and you post the difference to the frame. Every participant's page runs the same loop, so every frame gets the same props.

Moving the iframe element in the DOM, or removing and re-adding it, reloads it. Keep it in one place and change its size and position with CSS.
