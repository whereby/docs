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

<table><thead><tr><th width="278.92578125">Method</th><th>Payload Type</th><th>Description</th></tr></thead><tbody><tr><td><code>subscribeClientViews</code></td><td><code>clientViews: ClientView[]</code> </td><td>Emits client views</td></tr><tr><td><code>subscribeSpotlightedParticipants</code></td><td><code>spotlighted:ClientView[]</code> </td><td>Emits spotlighted client views</td></tr><tr><td><code>subscribeNumberOfClientViews</code></td><td><code>num:number</code> </td><td>Emits number of client views</td></tr></tbody></table>

## Actions

<table><thead><tr><th width="186.70442708333331">Method</th><th width="144.67578125">Parameters</th><th>Returns</th><th>Description</th></tr></thead><tbody><tr><td><code>spotlightParticipant</code></td><td><code>id: string</code></td><td> <code>void</code></td><td>Spotlights the specified participant</td></tr><tr><td><code>removeSpotlight</code></td><td><code>id: string</code></td><td> <code>void</code></td><td>Removes spotlight on specified participant (if spotlighted)</td></tr></tbody></table>

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

<table><thead><tr><th>Event (constant)</th><th>Event name (string)</th><th width="205.98529052734375">Payload</th><th>Emitted when</th></tr></thead><tbody><tr><td><code>CLIENT_VIEW_CHANGED</code></td><td><code>grid:client-view-changed</code></td><td><code>clientViews: ClientView[]</code></td><td>Client view changes</td></tr><tr><td><code>CLIENT_VIEW_SPOTLIGHTS_CHANGED</code></td><td><code>grid:client-view-spotlights-changed</code></td><td><code>clientViews: ClientView[]</code></td><td>Client is spotlighted</td></tr><tr><td><code>NUMBER_OF_CLIENT_VIEWS_CHANGED</code></td><td><code>grid:number-of-client-views-changed</code></td><td><code>numClients: number</code></td><td>Number of client views change</td></tr></tbody></table>

