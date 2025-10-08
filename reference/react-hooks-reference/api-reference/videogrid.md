# VideoGrid

```jsx
<VideoGrid />
```

The `VideoGrid` component renders a grid with all participants in the room rendered in one of 3 layouts: `PresentationGrid`, `VideoGrid` and `Subgrid`. The grid behaves like a normal Whereby room, with rules for which participants are rendered in which grid layout at any point in time. To see how the grid logic behaves, please see the [grid logic page](../guides-and-concepts/grid-logic.md).

## Properties

<table><thead><tr><th width="237">Property</th><th width="110">Required</th><th width="156">Type</th><th>Description</th></tr></thead><tbody><tr><td>videoGridGap</td><td></td><td><code>number</code></td><td>The gap between each video cell in pixels. Defaults to 8.</td></tr></tbody></table>



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
