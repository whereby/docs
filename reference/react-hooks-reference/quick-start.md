# Quick Start

This code snippet shows a basic example of using the browser-sdk to create a simple video app.

```tsx
import * as React from "react";
import { WherebyProvider, useRoomConnection, VideoView } from "@whereby.com/browser-sdk/react";

function App() {
  return (
    // Wrap your app in the provider
    <WherebyProvider>
      <MyVideoApp />
    </WherebyProvider>
  )
}

function MyVideoApp( { roomUrl, localStream }) {
    const { state, actions } = useRoomConnection(
        "<room_url>"
        {
            localMediaOptions: {
                audio: true,
                video: true,
            }
        }
    );

    const { connectionState, remoteParticipants } = state;
    const { joinRoom, leaveRoom } = actions;
    
    React.useEffect(() => {
        joinRoom();
        return () => leaveRoom();
    }, []);

    return <div className="videoGrid">
        { /* Render any UI, making use of state */ }
        { remoteParticipants.map((p) => (
            <VideoView key={p.id} stream={p.stream} />
        )) }
    </div>;
}
```

This is the most basic functionality of the library. Each of the concepts and more advanced usage is explained in the next sections of the documentation:

* [Guides and Concepts](guides-and-concepts/)
* [API Reference](api-reference/)
* [Types](types.md)

