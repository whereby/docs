# Room Integrations

Share a YouTube video or a Miro board with everyone in the room. One participant starts the share, the server broadcasts it, and it appears in `state.roomIntegrations.running` for everyone in the room, or in the current breakout group.

{% hint style="info" %}
**TODO:** add the `@whereby.com/core` version that ships room integrations before publishing.
{% endhint %}

The methods on this page start, stop and update sessions. To show a running session, render an iframe and speak the [room integration iframe protocol](../room-integration-iframe-protocol.md). For a full example without React, see [Room integrations without React](../../../../whereby-for-web-browser/react-based-browser-sdk/room-integrations-without-react.md).

### Methods

All three methods need a connected room. Called before that, they log a warning and do nothing. None of them return anything. Watch `getState().roomIntegrations` for the result.

#### `startRoomIntegration(options): void`

```ts
startRoomIntegration(options: {
    roomIntegrationId: string;
    tagName: string;
    shareUrl: string;
    props?: RoomIntegrationProps;
}): void
```

Validate the content and ask the server to share it with the room, or with your breakout group if you're in one. Get `tagName`, `shareUrl` and `props` from the picker (`whereby:formSubmit`) or from [`roomIntegrationContent`](room-integrations.md#building-content-without-the-picker).

Local state doesn't change straight away. The server broadcasts the new session to everyone, including you, and then it appears in `roomIntegrations.running`.

```ts
import { roomIntegrationContent } from "@whereby.com/core";

const roomConnection = wherebyClient.getRoomConnection();
const { roomIntegrations } = roomConnection.getState();

const youtube = roomIntegrations.embeddable.find((integration) => integration.name === "youtube");
const content = roomIntegrationContent.youtube({ url: "https://www.youtube.com/watch?v=dQw4w9WgXcQ" });

if (youtube && content) {
    roomConnection.startRoomIntegration({ roomIntegrationId: youtube.roomIntegrationId, ...content });
}
```

#### `stopRoomIntegration(options): void`

```ts
stopRoomIntegration(options: { roomIntegrationSessionId: string; intent?: "stop" | "end" }): void
```

Stop a running session for everyone. Only the participant who started it, or a host, can stop it. Check `session.canStop` first. `intent` defaults to `"stop"`. Pass `"end"` when the user deliberately closed the content, for example after a `whereby:close` message from the content frame.

#### `updateRoomIntegrationProps(options): void`

```ts
updateRoomIntegrationProps(options: { roomIntegrationSessionId: string; props: RoomIntegrationProps }): void
```

Merge `props` into a running session's props and send them to everyone in the room. Call it when the content frame posts `whereby:updateProps`. You rarely need it otherwise.

### State

| Field              | Type                                                                          | Description |
| ------------------ | ----------------------------------------------------------------------------- | ----------- |
| `roomIntegrations` | [`RoomIntegrationsState`](../../types/roomconnection-types.md#roomintegrationsstate) | Which integrations the room offers, which are running, and the last error. |

There's no dedicated subscription for room integrations. Use `subscribe` and compare `state.roomIntegrations` with the previous value:

```ts
let previous = roomConnection.getState().roomIntegrations;

roomConnection.subscribe((state) => {
    if (state.roomIntegrations === previous) {
        return;
    }
    previous = state.roomIntegrations;
    renderShareMenu(state.roomIntegrations.embeddable);
    renderRunningIntegrations(state.roomIntegrations.running);
});
```

`GridClient` also has [`subscribeRunningRoomIntegrations`](../gridclient.md) if you only need the running sessions.

#### `RoomIntegrationsState`

```ts
type RoomIntegrationsState = {
    hasFetched: boolean;
    isFetching: boolean;
    error: RoomIntegrationErrorDetail | null;
    enabled: RoomIntegration[];
    embeddable: RoomIntegration[];
    running: RoomIntegrationSessionView[];
};
```

| Field        | Description |
| ------------ | ----------- |
| `hasFetched` | `true` once the room's list of integrations has loaded. The client fetches it after the room connects. Until then `enabled` and `embeddable` are empty, and `startRoomIntegration` fails with `unknown_integration`. |
| `isFetching` | `true` while that request is in flight. |
| `error`      | The last error from a fetch, start, stop or props update. Cleared by the next successful fetch and by the next start or stop that passes the local checks. |
| `enabled`    | Integrations switched on for this room, including ones that can't run in an SDK app. |
| `embeddable` | The subset of `enabled` that can run in an SDK app. Today that's YouTube and Miro. Offer only these. `startRoomIntegration` doesn't check this flag. |
| `running`    | Sessions running in your part of the room. In a breakout group you only see that group's sessions. People who join late receive the running sessions when they join. |

See [RoomConnection Types](../../types/roomconnection-types.md#roomintegration) for `RoomIntegration`, `RoomIntegrationSessionView` and `RoomIntegrationErrorDetail`.

### Errors

`state.roomIntegrations.error` is `{ code, message }`. Switch on `code`. `message` is for developers.

| `code`                    | Raised by             | Meaning |
| ------------------------- | --------------------- | ------- |
| `unknown_integration`     | SDK, start and stop   | Start: no integration with that `roomIntegrationId`, often because the list hasn't loaded yet. Stop: no running session with that `roomIntegrationSessionId`. |
| `integration_not_enabled` | SDK, start            | The integration exists but isn't switched on for this room. |
| `invalid_content`         | SDK, start and update | `tagName` isn't a custom element name, `shareUrl` isn't `https`, or `props` isn't a flat object of strings, numbers, booleans and `null`. |
| `not_allowed_to_stop`     | SDK, stop             | You didn't start this session and you aren't a host. |
| `not_in_a_room`           | Server                | The server doesn't consider you a participant in the room. |
| `forbidden`               | Server                | The server refused the request. |
| `missing_parameters`      | Server                | The request was missing a field. |
| `invalid_parameters`      | Server                | The request had a field the server didn't accept. |
| `internal_server_error`   | Server                | The server failed to carry out the request. |
| `unknown`                 | SDK or server         | The integration list failed to load, or the server sent an error code the SDK doesn't recognise. |

### Building content without the picker

`roomIntegrationContent` builds `{ tagName, shareUrl, props }` from a link you already have. It doesn't need a browser, so it also works from Node, for example in an [Assistant](../../../assistant-sdk-reference/README.md).

```ts
import { roomIntegrationContent } from "@whereby.com/core";
```

Both functions return `null` when they can't use the input.

#### `roomIntegrationContent.youtube({ url, startAt?, metadata? }): RoomIntegrationContent | null`

| Option     | Type                                                          | Description |
| ---------- | ------------------------------------------------------------- | ----------- |
| `url`      | `string`                                                      | **Required.** A YouTube link or a bare 11-character video id. Accepts `youtube.com/watch?v=`, `youtu.be/`, `/embed/`, `/shorts/` and `/live/` links, and `youtube-nocookie.com`. |
| `startAt`  | `number?`                                                     | Where to start, in seconds. Defaults to the `t=` or `start=` value in the link, or `0`. |
| `metadata` | `{ aspectRatio?: number; isLive?: boolean; title?: string }?` | `aspectRatio` defaults to `16 / 9`. Set `isLive` for live streams. |

`youtube()` schedules playback to start four seconds after you call it, which gives each participant's frame time to load. Build the content right before you start it.

#### `roomIntegrationContent.miro({ accessLink }): RoomIntegrationContent | null`

`accessLink` must be a Miro embed link, `https://miro.com/app/live-embed/...` from **Share → Embed** in Miro, or `https://miro.com/app/access-link/...` from the Miro board picker. A normal board URL returns `null`, because Miro only renders links its own API generates.

#### `RoomIntegrationContent`

```ts
interface RoomIntegrationContent {
    tagName: string;
    shareUrl: string;
    props: RoomIntegrationProps;
}
```

### Picker helpers

Exported from `@whereby.com/core` for building the picker yourself. The [iframe protocol reference](../room-integration-iframe-protocol.md#picker-frame) explains how they fit together.

| Export                                                            | Description |
| ----------------------------------------------------------------- | ----------- |
| `roomIntegrationPickerUrl({ integration, parentOrigin, featureSource? })` | Returns the URL of the integration's picker page. Only `integration.webview` is read. |
| `ROOM_INTEGRATION_PICKER_MESSAGES`                                | `{ FORM_SUBMIT: "whereby:formSubmit", FORM_CLOSE: "whereby:formClose", ERROR: "whereby:bootstrapError" }` |
| `RoomIntegrationPickerResult` (type)                              | `{ tagName: string; shareUrl: string; props: RoomIntegrationProps }`, the payload of `whereby:formSubmit`. |
| `roomIntegrationContentTagName(name)`                             | The `tagName` for an integration name, for example `"youtube-integration-contentframe"`. |
