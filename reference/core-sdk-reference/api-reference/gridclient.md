---
hidden: true
---

# GridClient

```jsx
const grid = client.getGrid()
```

RoomConnectionClient manages entire room connection.  It exposes state subscriptions and provides actions to manage and observe remoteParticipants and in room actions. Room connection s retrieved from the WherebyClient.

## Utilities

| Method     | Returns     | Description                                 |
| ---------- | ----------- | ------------------------------------------- |
| `getState` | `GridState` | Returns the entire grid state               |
| `destroy`  | `void`      | Destroy the RoomConnectionClient instance.  |

## State Subscriptions

All subscribe methods follow this format:

* Call signature: `subscribeX(callback: (payload: T) => void: () => void`
* Contract: Invokes a callback on initial subscription and whenever the state changes.
* Returns: an unsubscribe function

### Methods

<table><thead><tr><th width="278.92578125">Method</th><th>Payload Type</th><th>Description</th></tr></thead><tbody><tr><td><code>subscribeClientViews</code></td><td><code>clientViews: ClientView[]</code> </td><td>Emits client views</td></tr><tr><td><code>subscribeSpotlightedParticipants</code></td><td><code>spotlighted:ClientView[]</code> </td><td>Emits spotlighted client views</td></tr><tr><td><code>subscribeNumberOfClientViews</code></td><td><code>num:number</code> </td><td>Emits number of client views</td></tr><tr><td><code>subscribeRunningRoomIntegrations</code></td><td><code>sessions: RoomIntegrationSessionView[]</code></td><td>Emits the running <a href="roomconnectionclient/room-integrations.md">room integrations</a> in the main room or your current breakout group. Only called on changes, so read <code>getState().runningRoomIntegrations</code> for the current value</td></tr></tbody></table>

## Actions

<table><thead><tr><th width="186.70442708333331">Method</th><th width="144.67578125">Parameters</th><th>Returns</th><th>Description</th></tr></thead><tbody><tr><td><code>spotlightParticipant</code></td><td><code>id: string</code></td><td> <code>void</code></td><td>Spotlights the specified participant</td></tr><tr><td><code>removeSpotlight</code></td><td><code>id: string</code></td><td> <code>void</code></td><td>Removes spotlight on specified participant (if spotlighted)</td></tr><tr><td><code>reorderClientViews</code></td><td><code>{ id: string, targetId: string }</code></td><td> <code>void</code></td><td>Swaps two client views in <code>allClientViews</code>, for this participant only. See <a href="#reordering">Reordering</a></td></tr><tr><td><code>setClientViewOrder</code></td><td><code>ids: string[]</code></td><td> <code>void</code></td><td>Puts the listed client views first in <code>allClientViews</code>, for this participant only. The rest keep their current order</td></tr><tr><td><code>swapSpotlightOrder</code></td><td><code>{ id: string, targetId: string }</code></td><td> <code>void</code></td><td>Swaps two spotlighted client views for everyone in the room. Needs <code>canSpotlight</code></td></tr></tbody></table>

### Reordering

All ids are client view ids (`ClientView.id`). A screenshare is its own client view with its own id, so it moves separately from the camera of the participant sharing it.

`reorderClientViews` and `setClientViewOrder` change the order of `allClientViews` in the grid state, and of what `subscribeClientViews` and `CLIENT_VIEW_CHANGED` emit, for this participant only. Other participants keep their own order. The participant lists on the room connection keep join order. Client views keep their ids when they move, so video elements don't remount.

```javascript
const grid = client.getGrid();

// Swap two tiles
grid.reorderClientViews({ id: "a", targetId: "b" });

// Move a participant who raised a hand to the front
grid.setClientViewOrder([raisedHandId]);
```

With `setClientViewOrder`, the client views you list go first, in that order. The ones you leave out keep their current order after them, and participants who join later go at the end. There's no way to reset to join order: list the client views in the order you want instead. `reorderClientViews` does nothing when either id isn't a client view in the room.

`swapSpotlightOrder` swaps two spotlighted client views for everyone in the room. Only participants who can spotlight, such as hosts, can call it: check `canSpotlight` in the grid state. It does nothing when either client view isn't spotlighted. The order of `spotlightedParticipants` changes when the server confirms the swap, so it doesn't change straight away.

In a React app, [`moveCell`](../../react-hooks-reference/videogrid.md#reordering-tiles) on `VideoGrid` picks the right one of these for a dropped tile.


## Events

`GridClient` extends `EventEmitter` . You can listen to lifecycle and state change events directly:&#x20;

```jsx
import { NUMBER_OF_CLIENT_VIEWS_CHANGED } from "@whereby.com/core";

const grid = client.getGrid();

function numberOfClientsChanged(numClients: number) {
    console.log("Num of clients changed", numClients);
}

grid.on(NUMBER_OF_CLIENT_VIEWS_CHANGED, numberOfClientsChanged);
```

<table><thead><tr><th>Event (constant)</th><th>Event name (string)</th><th width="205.98529052734375">Payload</th><th>Emitted when</th></tr></thead><tbody><tr><td><code>CLIENT_VIEW_CHANGED</code></td><td><code>grid:client-view-changed</code></td><td><code>clientViews: ClientView[]</code></td><td>Client view changes</td></tr><tr><td><code>CLIENT_VIEW_SPOTLIGHTS_CHANGED</code></td><td><code>grid:client-view-spotlights-changed</code></td><td><code>clientViews: ClientView[]</code></td><td>Client is spotlighted</td></tr><tr><td><code>NUMBER_OF_CLIENT_VIEWS_CHANGED</code></td><td><code>grid:number-of-client-views-changed</code></td><td><code>numClients: number</code></td><td>Number of client views change</td></tr><tr><td><code>RUNNING_ROOM_INTEGRATIONS_CHANGED</code></td><td><code>grid:running-room-integrations-changed</code></td><td><code>sessions: RoomIntegrationSessionView[]</code></td><td>A room integration starts or stops, or anything about a running one changes, such as its props</td></tr></tbody></table>



