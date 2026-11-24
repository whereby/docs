# Grid logic

The `<VideoGrid />` component is simple to use, but it contains a set of complex rules that decide how the various video cells are rendered.

## Layouts

The `<VideoGrid />` component consists of three specific grid layouts:\
`Presentationgrid`, `Videogrid` and `Subgrid`. &#x20;

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

The placement of the grids will change depending on container size, number of presentations and number of participants in the video and subgrid. For example, if there are no active presentations, the video grid will fill the presentation space. The same rule applies to the subgrid.

## Participant distribution

The selection of which participants reside in which grid section is decided based on a set of rules:

### Presentationgrid

The presentation grid consists of all screen shares and spotlighted participants.&#x20;

### Videogrid

The video grid consists of all participants that have enabled video, are not spotlighted, and are not in the subgrid. By default, the `Videogrid` can have 12 active video cells. However, this is can be customised.

### Subgrid

The general rule is that all participants with their video off, will move to the subgrid. Any participant joining after the video limit in the `Videogrid` is reached, will also be automatically moved into the subgrid.

## Room integrations

When someone shares a YouTube video or a Miro board, `<VideoGrid />` places it like a cell and shows it in an iframe. You can draw the cell yourself with [`renderIntegration`](../../reference/react-hooks-reference/videogrid.md#rendering-room-integrations), or turn this off with `enableIntegrations={false}`.

* The first running integration takes the `Presentationgrid`. While it's there, spotlighted participants move to the `Videogrid` instead of sharing the stage with it.
* Any further running integrations go to the `Subgrid`.
* Maximizing a participant gives them the stage. The integration stays mounted in a hidden cell, so a video keeps playing, and it returns to the stage when the participant is restored. The same happens to extra integrations when the subgrid is turned off.
* An integration cell takes the content's aspect ratio, 16:9 by default.

An iframe reloads when React moves it in the DOM, so the grid always renders integration cells first in its list, with a key built from the session id. Participants joining, leaving or getting spotlighted don't move them. See [Sharing YouTube and Miro](sharing-youtube-and-miro.md) for the full feature.

## Tile order

By default your own tile comes first, and the others follow in the order they joined. Spotlighted participants are in the order they were spotlighted.

You can let people reorder tiles with [`moveCell`](../../reference/react-hooks-reference/videogrid.md#reordering-tiles), which follows the same rules as dragging a tile in a Whereby room:

* Two tiles in the `Videogrid` swap places for you only.
* Two spotlighted tiles in the `Presentationgrid` swap places for everyone, if you can spotlight.
* A tile can't move between the `Presentationgrid` and the `Videogrid`.
* Tiles in the `Subgrid` and the floating tile can't be reordered.
* Swapping a tile with the maximized tile moves the maximize to it.
* Dropping a tile on the room integration on the stage maximizes the tile.

A screenshare is its own tile, separate from the camera of the participant sharing it.
