# VideoGrid

```jsx
<VideoGrid />
```

The `VideoGrid` component renders a grid with all participants in the room rendered in one of 3 layouts: `PresentationGrid`, `VideoGrid` and `Subgrid`. The grid behaves like a normal Whereby room, with rules for which participants are rendered in which grid layout at any point in time. To see how the grid logic behaves, please see the [grid logic page](../../whereby-for-web-browser/react-based-browser-sdk/grid-logic.md).

## Properties

<table><thead><tr><th width="237">Property</th><th width="110">Required</th><th width="156">Type</th><th>Description</th></tr></thead><tbody><tr><td>videoGridGap</td><td></td><td><code>number</code></td><td>The gap between each video cell in pixels. Defaults to 8.</td></tr><tr><td>renderParticipant</td><td></td><td><code>({ participant }: { participant: ClientView }) => React.ReactNode</code></td><td>Render your own video cell for participants in the presentation grid and the video grid. When omitted, the default Whereby video cell is rendered.</td></tr><tr><td>renderSubgridParticipant</td><td></td><td><code>({ participant }: { participant: ClientView }) => React.ReactNode</code></td><td>Render your own video cell for participants in the <a href="../../whereby-for-web-browser/react-based-browser-sdk/grid-logic.md">subgrid</a>. When omitted, subgrid participants fall back to the default Whereby video cell — <em>not</em> to <code>renderParticipant</code>.</td></tr><tr><td>renderFloatingParticipant</td><td></td><td><code>({ participant }: { participant: ClientView }) => React.ReactNode</code></td><td>Render your own video cell for the participant that is currently floating on top of the grid, if any.</td></tr></tbody></table>

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
