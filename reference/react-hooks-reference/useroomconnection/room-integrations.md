# Room Integrations

Participants can share a YouTube video or a Miro board with everyone in the room. One person starts the share, everyone sees it, and playback stays in sync. The Whereby app calls these room integrations, and so does the SDK.

{% hint style="info" %}
**TODO:** add the `@whereby.com/browser-sdk` and `@whereby.com/core` versions that ship room integrations before publishing.
{% endhint %}

This page lists the state and actions on `useRoomConnection`. Two more hooks render the shared content: [`useRoomIntegrationPicker`](../useroomintegrationpicker.md) for choosing what to share and [`useRoomIntegrationView`](../useroomintegrationview.md) for showing it. For a walkthrough that puts them together, see [Sharing YouTube and Miro](../../../whereby-for-web-browser/react-based-browser-sdk/sharing-youtube-and-miro.md).

### Actions

| Action                       | Signature                                                                                                        | Description                                                                                                                                                                                       |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `startRoomIntegration`       | `(options: { roomIntegrationId: string; tagName: string; shareUrl: string; props?: RoomIntegrationProps }) => void` | Validate the content and ask the server to start sharing it with the room, or with your breakout group if you're in one. Local state doesn't change until the server confirms (see below).        |
| `stopRoomIntegration`        | `(options: { roomIntegrationSessionId: string; intent?: "stop" \| "end" }) => void`                               | Stop a running integration for everyone. Only the participant who started it, or a host, can stop it. `intent` defaults to `"stop"`. Pass `"end"` when the user deliberately closed the content. |
| `updateRoomIntegrationProps` | `(options: { roomIntegrationSessionId: string; props: RoomIntegrationProps }) => void`                            | Merge `props` into a running session's props and send them to everyone in the room. `useRoomIntegrationView` calls this for you, so you only need it if you drive the content frame yourself.     |

All three actions need a connected room. Called before that, they log a warning and do nothing.

The arguments to `startRoomIntegration` come from one of two places:

* The picker. `useRoomIntegrationPicker` calls `onPicked` with `{ tagName, shareUrl, props }`, which you spread into the call along with the integration's `roomIntegrationId`.
* `roomIntegrationContent`, when you already have a link and don't need a picker (see [Building content without the picker](room-integrations.md#building-content-without-the-picker)).

```tsx
const { state, actions } = useRoomConnection(roomUrl, { localMediaOptions: { audio: true, video: true } });

const youtube = state.roomIntegrations.embeddable.find((integration) => integration.name === "youtube");
const content = roomIntegrationContent.youtube({ url: "https://www.youtube.com/watch?v=dQw4w9WgXcQ" });

if (youtube && content) {
    actions.startRoomIntegration({ roomIntegrationId: youtube.roomIntegrationId, ...content });
}

// Later, from a stop button. Disable it when the session's canStop is false.
const [session] = state.roomIntegrations.running;
if (session?.canStop) {
    actions.stopRoomIntegration({ roomIntegrationSessionId: session.roomIntegrationSessionId });
}
```

#### What happens after `startRoomIntegration`

`startRoomIntegration` checks the content locally, then sends it to the server. Nothing appears in your state yet. The server records the session and broadcasts it to everyone in the room, or in your breakout group, and that includes you. When the broadcast arrives, the session shows up in `state.roomIntegrations.running` for every participant at about the same time.

If a local check fails, the action sets `state.roomIntegrations.error` and sends nothing. If the server refuses, the error arrives the same way, a moment later.

### State

| Field              | Type                                                   | Description |
| ------------------ | ------------------------------------------------------ | ----------- |
| `roomIntegrations` | [`RoomIntegrations`](../types.md#roomintegrations)      | Which integrations the room offers, which are running, and the last error. |

#### `RoomIntegrations`

| Field         | Type                                                                          | Description |
| ------------- | ----------------------------------------------------------------------------- | ----------- |
| `hasFetched`  | `boolean`                                                                     | `true` once the list of integrations for the room has loaded. The SDK fetches it after you join. |
| `isFetching`  | `boolean`                                                                     | `true` while that request is in flight. |
| `error`       | [`RoomIntegrationErrorDetail`](../types.md#roomintegrationerrordetail)` \| null` | The last error from a fetch, start, stop or props update. Cleared by the next successful fetch and by the next start or stop that passes the local checks. |
| `enabled`     | [`RoomIntegration`](../types.md#roomintegration)`[]`                            | Integrations switched on for this room, including ones the SDK can't run. |
| `embeddable`  | [`RoomIntegration`](../types.md#roomintegration)`[]`                            | The subset of `enabled` that can run inside your app. Today that's YouTube and Miro. Build your "Share" menu from this list. |
| `running`     | [`RoomIntegrationSessionView`](../types.md#roomintegrationsessionview)`[]`      | Integrations running in your part of the room. In a breakout group you only see the group's sessions, and in the main room you only see the main room's. |

The list arrives shortly after the room connects. Until `hasFetched` is `true`, `enabled` and `embeddable` are empty and `startRoomIntegration` fails with `unknown_integration`.

`running` comes from the server, not from the list fetch. People who join late receive the sessions that are already running as part of joining, so a late joiner's `running` is filled in from the start.

{% hint style="warning" %}
`startRoomIntegration` doesn't check `isEmbeddable`. It will start Google Docs or Trello if they're enabled for the room, but neither will render in an SDK app (see [Limitations](../../../whereby-for-web-browser/react-based-browser-sdk/sharing-youtube-and-miro.md#limitations)). Only offer integrations from `embeddable`.
{% endhint %}

#### `RoomIntegrationSessionView`

Each entry in `running` combines the session the server sent with what the SDK knows about you:

```ts
interface RoomIntegrationSessionView {
    roomIntegrationSessionId: string;
    roomIntegrationId: string;
    breakoutGroupId: string; // "" in the main room
    tagName: string;
    shareUrl: string;
    props: RoomIntegrationProps;
    clientId: string; // the participant who started it
    roomIntegrationSessionStartedAt: number | null; // epoch milliseconds
    integration: RoomIntegration;
    isPresenter: boolean; // you started it
    presenterDisplayName: string | null; // null when isPresenter is true, or if the starter has left
    canStop: boolean; // isPresenter, or you're a host
}
```

See [Types](../types.md#roomintegration) for `RoomIntegration` and the other types used here.

### Errors

`state.roomIntegrations.error` is `{ code, message }`. `message` is written for developers, so show your own text to users and switch on `code`.

| `code`                    | Raised by           | Meaning |
| ------------------------- | ------------------- | ------- |
| `unknown_integration`     | SDK, start and stop | Start: no integration with that `roomIntegrationId`, often because the list hasn't loaded yet. Stop: no running session with that `roomIntegrationSessionId`. |
| `integration_not_enabled` | SDK, start          | The integration exists but isn't switched on for this room. |
| `invalid_content`         | SDK, start and update | `tagName` isn't a custom element name, `shareUrl` isn't `https`, or `props` isn't a flat object of strings, numbers, booleans and `null`. |
| `not_allowed_to_stop`     | SDK, stop           | You didn't start this session and you aren't a host. |
| `not_in_a_room`           | Server              | The server doesn't consider you a participant in the room. |
| `forbidden`               | Server              | The server refused the request. |
| `missing_parameters`      | Server              | The request was missing a field. |
| `invalid_parameters`      | Server              | The request had a field the server didn't accept. |
| `internal_server_error`   | Server              | The server failed to carry out the request. |
| `unknown`                 | SDK or server       | The integration list failed to load, or the server sent an error code the SDK doesn't recognise. |

### Building content without the picker

`roomIntegrationContent` builds the `{ tagName, shareUrl, props }` that `startRoomIntegration` needs from a link you already have. Use it when your app chooses what to share, for example a video attached to a lesson.

```ts
import { roomIntegrationContent } from "@whereby.com/browser-sdk/react";
```

Both functions return `null` if they can't use the input. Check for `null` before you start.

#### `roomIntegrationContent.youtube(options): RoomIntegrationContent | null`

| Option     | Type                                                       | Description |
| ---------- | ---------------------------------------------------------- | ----------- |
| `url`      | `string`                                                   | **Required.** A YouTube link or a bare 11-character video id. Accepts `youtube.com/watch?v=`, `youtu.be/`, `/embed/`, `/shorts/` and `/live/` links, and `youtube-nocookie.com`. |
| `startAt`  | `number?`                                                  | Where to start, in seconds. Defaults to the `t=` or `start=` value in the link, or `0`. |
| `metadata` | `{ aspectRatio?: number; isLive?: boolean; title?: string }?` | `aspectRatio` defaults to `16 / 9` and sets the shape of the integration's cell in `VideoGrid`. Set `isLive` for live streams. |

`youtube()` schedules playback to start four seconds after you call it, which gives each participant's frame time to load. Build the content right before you start it, not ahead of time.

```ts
roomIntegrationContent.youtube({ url: "https://youtu.be/dQw4w9WgXcQ?t=42" });
roomIntegrationContent.youtube({ url: "dQw4w9WgXcQ", startAt: 42 });
roomIntegrationContent.youtube({ url: "https://www.youtube.com/shorts/abc", metadata: { aspectRatio: 9 / 16 } });
```

#### `roomIntegrationContent.miro(options): RoomIntegrationContent | null`

| Option       | Type     | Description |
| ------------ | -------- | ----------- |
| `accessLink` | `string` | **Required.** A Miro embed link: `https://miro.com/app/live-embed/...` (from **Share → Embed** in Miro) or `https://miro.com/app/access-link/...` (from the Miro board picker). |

A normal board URL (`https://miro.com/app/board/...`) returns `null`. Miro only renders links that its own API generates. To let users browse their own boards, use the picker.

#### `RoomIntegrationContent`

```ts
interface RoomIntegrationContent {
    tagName: string;
    shareUrl: string;
    props: RoomIntegrationProps;
}
```

`roomIntegrationContentTagName(name)` is also exported. It returns the `tagName` for an integration name, for example `"youtube-integration-contentframe"` for `"youtube"`.
