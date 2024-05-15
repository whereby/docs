# Grid logic

The `<VideoGrid />` component is simple to use, but it contains a set of complex rules that decide how the various video cells are rendered.

## Layouts

The `<VideoGrid />` component consists of three specific grid layouts:\
`Presentationgrid`, `Videogrid` and `Subgrid`. &#x20;

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

The placement of the grids will change depending on container size, number of presentations and number of participants in the video and subgrid. For example,  if there are no active presentations, the video grid will fill the presentation space. The same rule applies to the subgrid.

## Participant distribution

The selection of which participants reside in which grid section is decided based on a set of rules:

### Presentationgrid

The presentation grid consists of all screen shares and spotlighted participants.&#x20;

### Videogrid

The video grid consists of all participants that have enabled video, are not spotlighted, and are not in the subgrid. By default, the `Videogrid` can have 12 active video cells. However, this is can be customised.

### Subgrid

The general rule is that all participants with their video off, will move to the subgrid. Any participant joining after the video limit in the `Videogrid` is reached, will also be automatically moved into the subgrid.

