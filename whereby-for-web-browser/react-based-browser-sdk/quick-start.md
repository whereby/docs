# Browser SDK quickstart

This guide shows you how to use the Whereby Browser SDK to create a custom React video calling application with full control over the user interface. Unlike the web component approach, the Browser SDK provides granular access to video streams, participants, and room actions, enabling you to build completely tailored video experiences.

{% hint style="info" %}
The Browser SDK is ideal when you need to integrate video calling seamlessly into existing applications, implement custom layouts, or create specialized video experiences like telehealth platforms, virtual classrooms, or branded conference solutions.
{% endhint %}

### Prerequisites

This guide assumes you have a React development environment set up and are familiar with React hooks and JSX syntax. You'll also need:

* A Whereby Embedded account with an active subscription
* A created meeting room with its URL
* Node.js and npm or yarn installed
* A React project (create one with [Vite](https://vitejs.dev/guide/), [Parcel](https://parceljs.org/recipes/react/), Create React App, or similar)

If you haven't created a Whereby embedded account and meeting room yet, follow the [Whereby Embedded initial setup guide](../../getting-started/overview-of-whereby-embedded/initial-setup.md).

### Getting Started

#### Install the SDK

Add the Whereby Browser SDK to your React project:

```bash
npm install @whereby.com/browser-sdk
```

For yarn users:

```bash
yarn add @whereby.com/browser-sdk
```

#### Basic Implementation

The Browser SDK centers around the `useRoomConnection` hook, which manages the connection to your Whereby room and provides access to participants, actions, and UI components.

1. **Import the necessary components**:

```javascript
import React, { useEffect } from "react";
import { useRoomConnection } from "@whereby.com/browser-sdk/react";
```

2. **Set up the room connection**:

```javascript
const ROOM_URL = "https://your-subdomain.whereby.com/your-room-id";

function MyVideoApp() {
  const { state, actions, components } = useRoomConnection(ROOM_URL, {
    localMediaOptions: {
      audio: true,
      video: true,
    }
  });

  const { connectionState, localParticipant, remoteParticipants } = state;
  const { joinRoom, leaveRoom } = actions;
  const { VideoView } = components;

  useEffect(() => {
    joinRoom();
    return () => leaveRoom();
  }, [joinRoom, leaveRoom]);

  return (
    <div className="video-app">
      {/* Your custom UI here */}
    </div>
  );
}
```

3. **Render video streams**:

```javascript
return (
  <div className="video-app">
    {/* Local participant video */}
    {localParticipant?.stream && (
      <div className="local-video">
        <VideoView stream={localParticipant.stream} muted />
      </div>
    )}
    
    {/* Remote participants */}
    {remoteParticipants.map((participant) => (
      <div key={participant.id} className="remote-video">
        <VideoView stream={participant.stream} />
      </div>
    ))}
  </div>
);
```

4. **Add basic styles** to make your video elements visible:

```css
.video-app {
  display: flex;
  gap: 20px;
  padding: 20px;
}

.local-video, .remote-video {
  width: 300px;
  height: 200px;
}

.local-video video, .remote-video video {
  width: 100%;
  height: 100%;
  border-radius: 8px;
}
```

### Adding Controls

Enhance your video app with camera and microphone controls:

```javascript
function MyVideoApp() {
  const [isCameraOn, setIsCameraOn] = React.useState(true);
  const [isMicrophoneOn, setIsMicrophoneOn] = React.useState(true);
  
  const { state, actions, components } = useRoomConnection(ROOM_URL, {
    localMediaOptions: {
      audio: true,
      video: true,
    }
  });

  const { localParticipant, remoteParticipants } = state;
  const { joinRoom, leaveRoom, toggleCamera, toggleMicrophone } = actions;
  const { VideoView } = components;

  useEffect(() => {
    joinRoom();
    return () => leaveRoom();
  }, [joinRoom, leaveRoom]);

  return (
    <div className="video-app">
      {/* Video streams */}
      {localParticipant?.stream && (
        <VideoView stream={localParticipant.stream} muted />
      )}
      
      {remoteParticipants.map((participant) => (
        <VideoView key={participant.id} stream={participant.stream} />
      ))}
      
      {/* Controls */}
      <div className="controls">
        <button 
          onClick={() => {
            toggleCamera();
            setIsCameraOn(prev => !prev);
          }}
        >
          {isCameraOn ? 'Turn Camera Off' : 'Turn Camera On'}
        </button>
        
        <button 
          onClick={() => {
            toggleMicrophone();
            setIsMicrophoneOn(prev => !prev);
          }}
        >
          {isMicrophoneOn ? 'Mute' : 'Unmute'}
        </button>
      </div>
    </div>
  );
}
```

### Complete Example

Here's a fully functional video app that demonstrates the core Browser SDK features:

```javascript
import React, { useEffect, useState } from "react";
import { useRoomConnection } from "@whereby.com/browser-sdk/react";

const ROOM_URL = "https://your-subdomain.whereby.com/your-room-id";

function App() {
  return (
    <div className="app">
      <h1>My Custom Video App</h1>
      <MyVideoApp />
    </div>
  );
}

function MyVideoApp() {
  const [isCameraOn, setIsCameraOn] = useState(true);
  const [isMicrophoneOn, setIsMicrophoneOn] = useState(true);
  
  const { state, actions, components } = useRoomConnection(ROOM_URL, {
    localMediaOptions: {
      audio: true,
      video: true,
    }
  });

  const { localParticipant, remoteParticipants, connectionState } = state;
  const { joinRoom, leaveRoom, toggleCamera, toggleMicrophone } = actions;
  const { VideoView } = components;

  useEffect(() => {
    joinRoom();
    return () => leaveRoom();
  }, [joinRoom, leaveRoom]);

  if (connectionState === "connecting") {
    return <div>Connecting to room...</div>;
  }

  return (
    <div className="video-container">
      <div className="participants">
        {localParticipant?.stream && (
          <div className="participant local">
            <VideoView stream={localParticipant.stream} muted />
            <span>You</span>
          </div>
        )}
        
        {remoteParticipants.map((participant) => (
          <div key={participant.id} className="participant remote">
            <VideoView stream={participant.stream} />
            <span>{participant.displayName || "Guest"}</span>
          </div>
        ))}
      </div>
      
      <div className="controls">
        <button 
          className={isCameraOn ? "active" : "inactive"}
          onClick={() => {
            toggleCamera();
            setIsCameraOn(prev => !prev);
          }}
        >
          📹 Camera
        </button>
        
        <button 
          className={isMicrophoneOn ? "active" : "inactive"}
          onClick={() => {
            toggleMicrophone();
            setIsMicrophoneOn(prev => !prev);
          }}
        >
          🎤 Microphone
        </button>
      </div>
    </div>
  );
}

export default App;
```

**Note**: Replace `"https://your-subdomain.whereby.com/your-room-id"` with your actual room URL from the Whereby dashboard.

### Customizing Your App

The Browser SDK provides extensive customization options:

**Advanced Features**: Add screen sharing with `startScreenshare()` and `stopScreenshare()` actions, implement chat functionality using the `chatMessages` state and `sendChatMessage()` action, or handle room locks with the `knock()` action.

**Custom Layouts**: Create grid layouts, spotlight views, or presentation modes by manipulating how you render the `VideoView` components and applying custom CSS.

**Event Handling**: Access detailed room state including participant count, connection status, and media device states to build sophisticated UI logic.

**Branding**: Since you control the entire UI, you can implement your brand colors, logos, and styling without limitations.

### See Also

* [Browser SDK React Hooks Tutorial](../../telehealth-industry-solutions/using-whereby-react-hooks-build-a-telehealth-app.md): Build a complete telehealth-style video app with advanced features
* [Browser SDK Reference](../../reference/react-hooks-reference/): Complete documentation of all available hooks, actions, and components
