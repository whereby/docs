---
description: >-
  The following code snippets show a basic example of using the core sdk to join
  and leave a room.
---

# Quick Start

1. Create client

```jsx
import { WherebyClient } from "@whereby.com/core";

const client = new WherebyClient();
const roomConnection = client.getRoomConnection();
const localMedia = client.getLocalMedia();
```

2. Join room

```jsx
 type JoinRoomOptions = {
	 roomUrl: string;
	 audio?: boolean;
	 video?: boolean;
 }
 
 async function joinRoom({roomUrl, audio = false, video = false}: JoinRoomOptions) {
 
 /** If sending media, you need to start local media before initialising, for example: 
        async function startLocalMedia(stream: MediaStream) {
	await localMedia.startMedia(stream);
		}
 **/
        roomConnection.initialize({
            localMediaOptions: {
                audio,
                video,
            },
            roomUrl,
        });
        
        await roomConnection.joinRoom();
   }
```

3. Subscribe to remote participants

```jsx
import { RemoteParticipantState } from "@whereby.com/core"

public subscribeToRemoteParticipants(callback: (participants: RemoteParticpantState) => void): () => void {
        return roomConnection.subscribeToRemoteParticipants(callback);
    }
```

4. Minimal sample: Join for 10 seconds and then leave

```javascript
import { WherebyClient, RemoteParticipantState } from "@whereby.com/core";

const client = new WherebyClient();
const roomConnection = client.getRoomConnection();
const localMedia = client.getLocalMedia();

function subscribeToRemoteParticipants(
  callback: (participants: RemoteParticipantState[]) => void
) {
  return roomConnection.subscribeToRemoteParticipants(callback);
}

async function main(roomUrl: string) {

  // start media here if passing in a stream
  roomConnection.initialize({
    localMediaOptions: { audio: false, video: false },
    roomUrl,
  });
  await roomConnection.joinRoom();

  const unsubscribe = subscribeToRemoteParticipants((participants) => {
    console.log(
      "Participants",
      participants.map((p) => ({
        id: p.id,
        displayName: p.displayName,
      }))
    );
  });

  setTimeout(async () => {
    unsubscribe?.();
    await roomConnection.leaveRoom();
    await localMedia.stopMedia?.();
    console.log("Left room");
 
  }, 10000);
}
```

This demonstrates the most basic functionality of the library. From this example, you can go on to implement more advanced functionality that is explained in more detail in the next sections of the documentation:&#x20;

* [API Reference](api-reference/)
