# VideoGrid

```jsx
<VideoGrid />
```

The `VideoGrid` component renders a grid with all participants in the room rendered in one of 3 layouts: `PresentationGrid`, `VideoGrid` and `Subgrid`. The grid behaves like a normal Whereby room, with rules for which participants are rendered in which grid layout at any point in time. To see how the grid logic behaves, please see the [grid logic page](../../whereby-for-web-browser/react-based-browser-sdk/grid-logic.md).

## Properties

<table><thead><tr><th width="237">Property</th><th width="110">Required</th><th width="156">Type</th><th>Description</th></tr></thead><tbody><tr><td>videoGridGap</td><td></td><td><code>number</code></td><td>The gap between each video cell in pixels. Defaults to 8.</td></tr><tr><td>renderParticipant</td><td></td><td><code>({ participant }: { participant: ClientView }) => React.ReactNode</code></td><td>Render your own video cell for participants in the presentation grid and the video grid. When omitted, the default Whereby video cell is rendered.</td></tr><tr><td>renderSubgridParticipant</td><td></td><td><code>({ participant }: { participant: ClientView }) => React.ReactNode</code></td><td>Render your own video cell for participants in the <a href="../../whereby-for-web-browser/react-based-browser-sdk/grid-logic.md">subgrid</a>. When omitted, subgrid participants fall back to the default Whereby video cell — <em>not</em> to <code>renderParticipant</code>.</td></tr><tr><td>renderFloatingParticipant</td><td></td><td><code>({ participant }: { participant: ClientView }) => React.ReactNode</code></td><td>Render your own video cell for the participant that is currently floating on top of the grid, if any.</td></tr><tr><td>renderIntegration</td><td></td><td><code>({ session }: { session: RoomIntegrationSessionView }) => React.ReactNode</code></td><td>Render a running <a href="useroomconnection/room-integrations.md">room integration</a>, such as a shared YouTube video, in the cell the grid gives it. Usually a component that calls <a href="useroomintegrationview.md"><code>useRoomIntegrationView</code></a>. When omitted, the grid renders the integration in a plain iframe that fills the cell.</td></tr><tr><td>enableIntegrations</td><td></td><td><code>boolean</code></td><td>Show running room integrations in the grid. Defaults to <code>true</code>. Set to <code>false</code> to lay out the grid as if none were running, for example when you show integrations outside the grid.</td></tr></tbody></table>

## Rendering the subgrid

The subgrid holds the participants that are not on the stage — typically those with their camera off, and any participant that joins after the video grid limit is reached. See grid logic for the full set of rules.

Subgrid cells are much smaller than stage cells, so a tile design that works on the stage is rarely the right fit there. `renderSubgridParticipant` lets you render a separate, compact cell for those participants while keeping your stage cell untouched:

```tsx
<VideoGrid
    renderParticipant={({ participant }) => (
        <GridCell className="gridCell" participant={participant}>
            <GridVideoView className="videoView" />
            <div className="participantName">{participant.displayName}</div>
            <div className="participantControls">{/* mute, spotlight, ... */}</div>
        </GridCell>
    )}
    renderSubgridParticipant={({ participant }) => (
        <GridCell className="subgridCell" participant={participant}>
            <GridVideoView className="videoView" />
            {participant.displayName}
        </GridCell>
    )}
/>
```

{% hint style="info" %}
`renderParticipant` and `renderSubgridParticipant` are independent. If you only pass `renderParticipant`, participants moving into the subgrid will switch to the default Whereby cell. Pass both if you want a fully custom grid.
{% endhint %}

## Rendering room integrations

When someone in the room shares a YouTube video or a Miro board, `VideoGrid` makes room for it and shows it in an iframe. You don't need any props for this.

To draw the cell yourself, pass `renderIntegration`. It receives the session. Render an iframe from `useRoomIntegrationView` that fills the cell:

```tsx
function IntegrationFrame({ session }: { session: RoomIntegrationSessionView }) {
    const { iframeProps } = useRoomIntegrationView({ session });
    return iframeProps ? (
        <iframe {...iframeProps} allowFullScreen style={{ border: "none", width: "100%", height: "100%" }} />
    ) : null;
}

<VideoGrid renderIntegration={({ session }) => <IntegrationFrame session={session} />} />;
```

To leave integrations out of the grid, pass `enableIntegrations={false}`.

The first running integration takes the presentation stage, and spotlighted participants move to the video grid while it's there. Other running integrations go to the subgrid. If a participant is maximized, the integration leaves the stage but stays mounted in a hidden cell, so a video keeps playing. See [Grid logic](../../whereby-for-web-browser/react-based-browser-sdk/grid-logic.md#room-integrations) and the [room integrations guide](../../whereby-for-web-browser/react-based-browser-sdk/sharing-youtube-and-miro.md).

## `useGrid`

`useGrid` is the layout hook that `VideoGrid` uses. It's exported for building your own grid renderer with the same rules. Most apps don't need it. Use `VideoGrid` with `renderParticipant` and `renderIntegration` unless you need to place cells yourself.

`useGrid` leaves room integrations out unless you pass `includeIntegrations: true`. Only turn it on once you render integration cells. Otherwise a running integration takes the stage and shows nothing.

```tsx
import { useGrid } from "@whereby.com/browser-sdk/react";

const { cellViewsVideoGrid, cellViewsInPresentationGrid, cellViewsInSubgrid, videoStage, setContainerBounds } = useGrid({
    stageParticipantLimit: 12,
});
```

Call it inside a `WherebyProvider` with a room connection. It throws otherwise.

### Options

All options are optional.

| Option                       | Type      | Default | Description |
| ---------------------------- | --------- | ------- | ----------- |
| `activeVideosSubgridTrigger` | `number`  | `12`    | How many participants with video can be on the stage before participants with muted audio move to the subgrid. |
| `forceSubgrid`               | `boolean` | `true`  | Use the subgrid even when the number of participants is below `stageParticipantLimit`. |
| `stageParticipantLimit`      | `number`  | `12`    | Participant count above which the subgrid is used when `forceSubgrid` is `false`. |
| `gridGap`                    | `number`  | `8`     | Gap between the grid areas, in pixels. |
| `videoGridGap`               | `number`  | `8`     | Gap between cells, in pixels. |
| `enableSubgrid`              | `boolean` | `true`  | Set to `false` to keep everyone on the stage. |
| `enableConstrainedGrid`      | `boolean` | `true`  | Switch to the compact layout when the container is under 500 pixels wide or tall. |
| `includeIntegrations`        | `boolean` | `false` | Lay out running room integrations as cells with `type: "integration"`. When `false`, the layout ignores them. |

### Return value

| Property                      | Type                                                  | Description |
| ----------------------------- | ----------------------------------------------------- | ----------- |
| `cellViewsInPresentationGrid` | [`CellView`](types.md#cellview)`[]`                   | Cells on the presentation stage. Integration cells come first when `includeIntegrations` is on. |
| `cellViewsVideoGrid`          | [`CellView`](types.md#cellview)`[]`                   | Cells in the video grid. |
| `cellViewsInSubgrid`          | [`CellView`](types.md#cellview)`[]`                   | Cells in the subgrid. |
| `cellViewsHidden`             | [`CellView`](types.md#cellview)`[]`                   | Integration cells with no place in the current layout, for example while a participant is maximized. Keep rendering them, hidden, so their iframes don't reload. |
| `cellViewsFloating`           | [`CellView`](types.md#cellview)`[]`                   | The floating participant's cell, if any. |
| `videoStage`                  | `object`                                              | The computed layout: the bounds of each area and of each cell in it, in the same order as the cell arrays. |
| `containerFrame`              | `object`                                              | The container size the layout was computed for. |
| `setContainerBounds`          | `({ width, height }) => void`                         | Tell the hook the size of your container. Call it on mount and on resize. |
| `clientAspectRatios`          | `{ [clientId: string]: number }`                      | Aspect ratios reported for each participant's video. |
| `setClientAspectRatios`       | `React.Dispatch<...>`                                 | Update those aspect ratios, for example when a video's size changes. |
| `maximizedCellId`             | `string \| null`                                      | The cell that's maximized, if any. |
| `setMaximizedCellId`          | `React.Dispatch<React.SetStateAction<string \| null>>` | Maximize a cell by its `cellId`, or pass `null` to restore. Only participant cells can be maximized. |
| `maximizedParticipant`        | `ClientView \| null`                                  | The participant in the maximized cell. |
| `floatingCellId`              | `string \| null`                                      | The cell that's floating, if any. |
| `setFloatingCellId`           | `React.Dispatch<React.SetStateAction<string \| null>>` | Float a participant's cell by its `cellId`, or pass `null` to stop. |
| `floatingParticipant`         | `ClientView \| null`                                  | The participant in the floating cell. |
| `isConstrained`               | `boolean`                                             | `true` when the compact layout is active. |

Each cell has a `cellId`. For a participant it's the participant's id. For an integration it's `"room-integration:<roomIntegrationSessionId>"`. Use it as the React `key`. Render integration cells first and keep their keys stable, or React will move their iframes, and moving an iframe reloads it.

## Usage

```tsx
    const [isLocalScreenshareActive, setIsLocalScreenshareActive] = useState(false);

    const { actions } = useRoomConnection(roomUrl, { localMediaOptions: { audio: false, video: true } });
    const { toggleCamera, toggleMicrophone, startScreenshare, stopScreenshare } = actions;

    return (
        <>
            <div className="controls">
                <button onClick={() => toggleCamera()}>Toggle camera</button>
                <button onClick={() => toggleMicrophone()}>Toggle microphone</button>
                <button
                    onClick={() => {
                        if (isLocalScreenshareActive) {
                            stopScreenshare();
                        } else {
                            startScreenshare();
                        }
                        setIsLocalScreenshareActive((prev) => !prev);
                    }}
                >
                    Toggle screenshare
                </button>
            </div>
            <div style={{ height: "500px", width: "100%" }}>
                <VideoGrid videoGridGap={10} />
            </div>
        </>
    );
};
```
