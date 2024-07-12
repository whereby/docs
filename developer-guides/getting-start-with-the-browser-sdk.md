---
description: >-
  The Whereby Browser SDK lets you add video calling to your web application,
  customizing layout and features to fit your use case and brand.
---

# Getting start with the Browser SDK

## **What you'll need**

The Whereby Browser SDK takes advantage of React. This tutorial assumes that you're familiar with JavaScript, React and its JSX syntax. You should also be familiar with either [npm](https://www.npmjs.com/) or [yarn](https://yarnpkg.com/), JavaScript's primary package managers.

To follow along with this tutorial, you'll need to do two things.

* Sign up for a [Whereby Embedded](https://whereby.com/org/signup/embedded?signupFlowPlanType=embedded\_free) account. You can get started with Whereby's free plan. It provides you with 2,000 participant minutes each month.
* Create a Whereby meeting room. You'll need the meeting room's URL. Create your room using the wizard on your account dashboard or with Whereby's [REST API](https://docs.whereby.com/whereby-101/creating-and-deleting-rooms). For this tutorial, leave the room unlocked.

You should also have a basic React project set up. [Vite](https://vitejs.dev/guide/) or [Parcel](https://parceljs.org/recipes/react/) will get you up-and-running quickly.

### **Install the SDK**

First, add the Whereby Browser SDK to your React project. We'll need to install version 2.0.0-beta3 or later. Open a terminal window and type (or copy-and-paste) the following command.

```bash
npm i @whereby.com/browser-sdk
```

{% hint style="info" %}
If you're using yarn as your package manager, replace `npm i` with `yarn` add (e.g. `yarn add @whereby.com/browser-sdk`).
{% endhint %}

### **Connecting to your room**

Next, connect to your room. The Whereby Browser SDK exposes a custom React hook — [`useRoomConnection`](../reference/react-hooks-reference/api-reference/useroomconnection.md) — that connects participants in a meeting room. Import both React and `useRoomConnection` into your project file as shown below.

```jsx
import React from "react";
import { useRoomConnection } from "@whereby.com/browser-sdk/react";
```

The `useRoomConnection` hook requires two parameters:

* A Whereby room URL
* An object that contains a `localMediaOptions` field. `localMediaOptions` determines whether the browser should request access to a participant's camera, microphone, or both.

Use the `useRoomConnection` hook inside of a component function.&#x20;

{% code overflow="wrap" %}
```jsx
import React from "react";
import { useRoomConnection } from "@whereby.com/browser-sdk/react";

const MEETING_URL = 'https://<example>.whereby.com/b9538dfc-e52c-4f24-a509-xxxxxxxxxxxx';

const Meeting = () => {
  const mediaOptions = {
      localMediaOptions: {
          audio: true,
          video: true,
      }
  }

  const connection = useRoomConnection(MEETING_URL, mediaOptions);

  // Return the UI here
  return (
     <></>
  )
}

export default Meeting;
```
{% endcode %}

`useRoomConnection` returns a [`RoomConnectionReference`](../reference/react-hooks-reference/api-reference/useroomconnection.md#return-type) object that contains three properties:

* `state`, an object that reflects the status of the current room, including the call's participants;
* `components`, an object containing the pre-bound components available in the room; and
* `actions`, an object representing the available actions in the room.

This tutorial will introduce you to all three.

### **See yourself on screen**

[`RoomConnectionReference.state`](../reference/react-hooks-reference/api-reference/useroomconnection.md#state) includes two properties that represent participants in the meeting:

* `localParticipant`, an object that represents the individual meeting attendee (you, in this case); and
* `remoteParticipants`, an array of objects representing other meeting participants.

Each `localParticipant` and `remoteParticipants` object contains a `stream` property, that you'll use with the `VideoView` component to display video for each participant.

To see your own video stream, use the `localParticipant` property. Check whether `localParticipant` has an active stream, then conditionally render the [`VideoView`](../reference/react-hooks-reference/api-reference/videoview.md) component as shown below.

{% hint style="info" %}
**Note**: The examples in this tutorial use [destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring\_assignment), a syntax that allows you to extract Object properties into variables.&#x20;

If you prefer, you can use [dot-syntax](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Basics) (e.g. connection.components.VideoView or connection.state.localParticipant) to access these values.
{% endhint %}

```jsx
//…
const connection = useRoomConnection(MEETING_URL, mediaOptions);

const {state, components} = connection;
const {localParticipant} = state;
const {VideoView} = components;

return (
  <>
    {localParticipant?.stream ? (
      <div id="you" style={{width: '400px', height: 'auto'}}>
        <VideoView stream={localParticipant?.stream} muted />
      </div>
     ) : null
    }
  </>
)
```

`VideoView` requires a `stream` prop. Set its value to `localParticipant.stream`. The `muted` prop is optional, but good to use when rendering `VideoView`. Using `muted` for the `localParticipant` view prevents audio feedback from your own microphone.

Any attribute supported by HTML's [\<video> element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video) can be added as a `VideoView` prop. Remember, however, to use `className` instead of `class` when adding CSS class names to React components.

### **Add microphone and camera controls**

In the preceding example, the camera and microphone are enabled for the local participant — that's you. You probably want the ability to turn your camera and microphone off and on, though. You can accomplish that using the `toggleCamera` and `toggleMicrophone` methods of the `actions` property.

```jsx
//…
// Update the following line to define the `actions` variable.
const {state, components, actions} = connection;

// Extract toggleCamera and toggleMicrophone from the actions property.
const {toggleCamera, toggleMicrophone} = actions;
```

Set an initial state for the camera and microphone with [`React.useState()`](https://react.dev/reference/react/useState).

```jsx
const [isCameraActive, setIsCameraActive] = React.useState(true);
const [isMicrophoneActive, setIsMicrophoneActive] = React.useState(true);
```

Then update your markup to add a `button` for each control.

```jsx
//…
return (
  <>
  {localParticipant?.stream ? (
    <div id="you" style={{width: '400px', height: 'auto'}}>
      <VideoView stream={localParticipant?.stream} muted />
        <p className="controls">

          <button
            type="button"
            onClick={() => {
              setIsCameraActive((previous) => !previous);
              toggleCamera();
            }
          }>Toggle Camera</button>

          <button
            type="button"
            onClick={() => {
              setIsMicrophoneActive((previous) => !previous);
              toggleMicrophone();
            }}
          >Toggle Microphone</button>
        </p>
      </div>
		) : null
  }
  </>
);
```

In the click handler for each button, you'll first need to update state for your camera (`setIsCameraActive`) or microphone (`setIsMicrophoneActive`), before invoking the appropriate toggle method. Each meeting participant will have these controls available in their meeting room view.

### **Add friends and colleagues**

Talking to yourself is normal, but it's not a video call until you're talking to someone else. Use the `remoteParticipants` property and [`Array.prototype.map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Array/map) to display the other participants' video streams.

```jsx
//…

<div id="participants">
{remoteParticipants?.length ?
  remoteParticipants.map((friend) => {
      return (
        <div className="participant">
          <VideoView stream={friend?.stream} />
        </div>
      )
  }) : null
}
</div>
```

As with `localParticipant`, each `remoteParticipant` object contains a `stream` property. Set `remoteParticipant.stream` as the value of the `stream` prop for each `VideoView` instance. Don’t add the `muted` property to the `VideoView` component here. Doing so would mute audio for all attendees in the room.

Altogether, your code should now resemble the example below.

{% code overflow="wrap" %}
```jsx
import React from "react";
import { useRoomConnection } from "@whereby.com/browser-sdk/react";

const MEETING_URL = 'https://<example>.whereby.com/b9538dfc-e52c-4f24-a509-xxxxxxxxxxxx';

const Meeting = () => {
  const connection = useRoomConnection(MEETING_URL, {
      localMediaOptions: {
          audio: true,
          video: true,
      }
  });

  const {state, components, actions} = connection;
  const {localParticipant, remoteParticipants} = state;
  const {VideoView} = components;
  const {toggleCamera, toggleMicrophone} = actions;

  const [isCameraActive, setIsCameraActive] = React.useState(true);
  const [isMicrophoneActive, setIsMicrophoneActive] = React.useState(true);

  return (
    <>
    {localParticipant?.stream ? (
      <div className="participant">
        <VideoView
          stream={localParticipant?.stream}
          muted
          className="you" />

        <p className="controls">
          <button type="button" onClick={() => {
              setIsCameraActive((previous) => !previous);
              toggleCamera();
            }}
           >Toggle Camera</button>

           <button type="button" onClick={() => {
              setIsMicrophoneActive((previous) => !previous);
              toggleMicrophone();
              }}
	          >Toggle Microphone</button>
	        </p>

	      </div> 
			) : null
    }
    <div id="participants">
      {remoteParticipants?.length ?
        remoteParticipants.map((friend, index) => {
            return (
              <div className="participant" key={friend?.id}>
              {friend?.stream &&
                <VideoView stream={friend?.stream} className="friend" />
              }
              </div>
            )
        }) : null
      }
     </div>
    </>
  );
}

export default Meeting;
```
{% endcode %}

From here, you're ready to add CSS to change your meeting layout. Or, you can replace the default browser buttons with custom icon components.

Whereby's Browser SDK is a truly ["white-label" solution for video calling](https://whereby.com/information/embedded). It decouples the video stream from the standard Whereby UI so that you can create rich video conferencing, [virtual classrooms](https://whereby.com/information/embedded/elearning), or [telehealth](https://whereby.com/information/embedded/healthcare) experiences that reflect you or your brand.
