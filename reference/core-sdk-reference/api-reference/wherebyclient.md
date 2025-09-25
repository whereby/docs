# WherebyClient

```jsx
import { WherebyClient } from "@whereby.com/core";

const client = new WherebyClient();
```

The WherebyClient is the main entry point for the Core SDK. It acts as a wrapper around the underlying state store and service layer, and gives you access to the specialized client classes.

Creating a WherebyClient automatically sets up the internal store and services. From there, you can retrieve the specific clients you need:

* LocalMediaClient → manage camera and microphone
* RoomConnectionClient → join/leave rooms, subscribe to remote participants, any in room actions

### Methods

| Method                | Returns                | Description                                                                                                             |
| --------------------- | ---------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `new WherebyClient()` | `WherebyClient`        | Creates a new client instance with it’s own store and sub-clients. Construct once per session.                          |
| `getLocalMedia()`     | `LocalMediaClient`     | Manages local media streams. Use to start/stop local media and toggle tracks.                                           |
| `getRoomConnection()` | `RoomConnectionClient` | Manages and observes the room connection lifecycle. Use to initialize, join room and subscribe to remote participants.  |
| `destroy()`           | `void`                 | Destroy all sub-clients and release resources. Always ccll at teardown to unsubscribe and clean up.                     |

