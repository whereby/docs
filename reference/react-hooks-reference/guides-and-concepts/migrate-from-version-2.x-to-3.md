# Migrate from version 2.x to 3

Version 3 of the browser-sdk contains three breaking changes:

* `WherebyProvider` is now **required** to be rendered, and all usage of browser-sdk needs to be in children of the provider.
* `useRoomConnection` does **not** automatically join the room any longer. It's required to manually call `joinRoom()` from `useRoomConnection.actions`
* `useRoomConnection.components` is **deprecated.**



## Provider

It's now mandatory to wrap your react app in a `WherebyProvider.`

This is typically done in the `index.tsx/js` file.

```tsx
import * as React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import { WherebyProvider } from '@whereby.com/browser-sdk/react'

ReactDOM.render(
  <WherebyProvider>
    <App />
  </WherebyProvider>,
  document.getElementById('root')
);
```



## joinRoom

Starting in version `3.0.0`, The `useRoomConnection` hook does not automatically join the room when initialized. It's required to manually call the `joinRoom()` function. This gives you as the developer more control of the lifecycle of the room connection. It's still possible to auto-join, but it requires some additional code. This code snippet mimicks the old behaviour, and auto-joins when the component is mounted.

```tsx
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



## useRoomConnection.components

From version `3`, the `useRoomConnection.components` field is deprecated. It's no longer needed to bind components to the room connection, since we have the provider. The only change required is to import components directly from the library, instead of getting them from the hook.\


This is an example of rendering a `VideoView` in version `2`.&#x20;

```tsx
import { useRoomConnection } from "@whereby.com/browser-sdk/react";

function MyVideoApp( { roomUrl, localStream }) {
    // The components field here is now deprecated.
    const { state, actions, components } = useRoomConnection(
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
    // This is the old way of getting the VideoView component
    const { VideoView } = components;
    
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



In version `3`, the code will look like this instead

```tsx
import { VideoView, useRoomConnection } from "@whereby.com/browser-sdk/react";

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
