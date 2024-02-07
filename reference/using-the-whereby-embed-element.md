---
description: >-
  Our web component allows you to embed a Whereby room in any webpage. It
  provides a simple readable integration and exposes local client events and
  commands between our platform and yours.
---

# Web Component Reference

## Installation

When using React or a bundler like Webpack, Rollup, Parcel, etc

```bash
npm install @whereby.com/browser-sdk
```

```javascript
import "@whereby.com/browser-sdk/embed"
```

{% tabs %}
{% tab title="HTML" %}
{% code overflow="wrap" %}
```html
<whereby-embed room="https://subdomain.whereby.com/your_room?roomKey=3fe345a"></whereby-embed>
```
{% endcode %}

You can use a `hostRoomUrl` instead of the `roomUrl`, if you want to give the user [host privileges](../whereby-101/user-roles-and-privileges.md#hosts)
{% endtab %}

{% tab title="JSX" %}
```jsx
const MyComponent = ({ roomUrl }) => {
    return <whereby-embed room={roomUrl} />
}

export default MyComponent
```

You can use a `hostRoomUrl` instead of the `roomUrl`, if you want to give the user [host privileges](../whereby-101/user-roles-and-privileges.md#hosts)
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If you aren't using a bundler or a library containing a bundler you can access the component code directly from our CDN using a [simple Script tag](../whereby-101/readme/in-a-web-page/using-the-whereby-embed-element/script-tags.md) in your site
{% endhint %}

### Attributes of the component

<table><thead><tr><th width="215.33333333333331">Attribute</th><th>Value</th><th>Description</th></tr></thead><tbody><tr><td><code>room</code></td><td>A room URL</td><td>The full URL of the room you want to embed (required)</td></tr><tr><td><code>minimal</code></td><td>Nothing (on) or <code>"off"</code></td><td>Apply minimal UI for embed scenarios</td></tr><tr><td><code>displayName</code></td><td>Any string</td><td>Set displayname of participant</td></tr><tr><td><code>externalId</code></td><td>Up to 36 alphanumeric characters (<a href="../whereby-101/customizing-rooms/using-url-parameters.md#externalid-less-than-id-greater-than">details</a>)</td><td>Custom identifier for the participant.</td></tr><tr><td><code>audio=off</code></td><td><code>"off"</code></td><td>Enter meeting with audio off</td></tr><tr><td><code>video=off</code></td><td><code>"off"</code></td><td>Enter meeting with video off</td></tr><tr><td><code>background=off</code></td><td><code>"off"</code></td><td>Render without background to let embedding app render its own</td></tr><tr><td><code>chat</code></td><td>Nothing (on) or <code>"off"</code></td><td>Enable/disable chat</td></tr><tr><td><code>people</code></td><td><code>"off"</code></td><td>Disable the participant list</td></tr><tr><td><code>leaveButton</code></td><td>Nothing (on) or <code>"off"</code></td><td>Enable/disable the leave button</td></tr><tr><td><code>screenshare</code></td><td>Nothing (on) or <code>"off"</code></td><td>Enable/disable screenshare</td></tr><tr><td><code>subgridLabels</code></td><td>Nothing (off) or <code>"on"</code></td><td>Enable/disable name labels in the subgrid</td></tr><tr><td><code>floatSelf</code></td><td>Nothing (on) or <code>"off"</code></td><td>Float the self view to the bottom right</td></tr><tr><td><code>lowData</code></td><td>Nothing (on) or <code>"off"</code></td><td>Use a lower resolution by default</td></tr><tr><td><code>cameraAccess</code></td><td>Nothing (on) or <code>"off"</code></td><td>Disable camera access for the local participant, allowing for only audio input</td></tr></tbody></table>

There are additional room customizations and options that can be found in the [URL parameters](../whereby-101/customizing-rooms/using-url-parameters.md) section. These options must be added as parameters on the `room` source URL, and are not currently supported as attributes directly on the component.

#### Usage examples

{% tabs %}
{% tab title="Basic room embed" %}
```html
<whereby-embed room="https://subdomain.whereby.com/your_room" />
```
{% endtab %}

{% tab title="Room w/ minimal UI" %}
{% code overflow="wrap" %}
```html
<whereby-embed room="https://subdomain.whereby.com/your_room" minimal />
```
{% endcode %}
{% endtab %}

{% tab title="Room w/ multiple customizations" %}
{% code overflow="wrap" %}
```html
<whereby-embed room="https://subdomain.whereby.com/your_room" displayName="John Smith" audio=off video=off background=off chat=off />
```
{% endcode %}
{% endtab %}
{% endtabs %}



### Listening to events

You can listen for events on the `<whereby-embed>` component. The following events are supported:

<table><thead><tr><th width="289.3333333333333">Event type</th><th width="237">Description</th><th>Properties (from "detail")</th></tr></thead><tbody><tr><td><code>ready</code></td><td>Basic dependencies have loaded and the room is ready to be used</td><td>–</td></tr><tr><td><code>knock</code></td><td>The local user knocks to get into the room</td><td>–</td></tr><tr><td><code>participantupdate</code></td><td>A new participant join/leave</td><td>{ count: Number }</td></tr><tr><td><code>join</code></td><td>The local user joins</td><td>–</td></tr><tr><td><code>leave</code></td><td>The local user leaves</td><td>{ removed: Boolean }</td></tr><tr><td><code>participant_join</code></td><td>A new participant joins the room</td><td>{ participant: { metadata: String } }</td></tr><tr><td><code>participant_leave</code></td><td>A participant leaves the room</td><td>{ participant: { metadata: String } }</td></tr><tr><td><code>microphone_toggle</code></td><td>The local user toggles the microphone</td><td>{ enabled: Boolean }</td></tr><tr><td><code>camera_toggle</code></td><td>The local user toggles the camera</td><td>{ enabled: Boolean }</td></tr><tr><td><code>chat_toggle</code></td><td>The local user toggles the chat</td><td>{ open: Boolean }</td></tr><tr><td><code>people_toggle</code></td><td>The local user toggles the people pane</td><td>{ open: Boolean }</td></tr><tr><td><code>pip_toggle</code></td><td>The local user toggles Picture-in-Picture mode</td><td>{ open: Boolean }</td></tr><tr><td><code>deny_device_permission</code></td><td>The local user denies permission to camera and microphone in the pre-call screen</td><td>{ denied: Boolean }</td></tr><tr><td><code>screenshare_toggle</code></td><td>The local user toggles the screenshare</td><td>{ enabled: Boolean }</td></tr><tr><td><code>streaming_status_change</code></td><td>Streaming status changes. Possible values: " | requested | starting | streaming | stopping | stopped"</td><td>{ status: String }</td></tr><tr><td><code>connection_status_change</code></td><td>User connection status changes. Possible values: "stable | unstable"</td><td>{ status: String }</td></tr></tbody></table>



You can use standard JavaScript to listen to the events. Here's an example:

```javascript
  const elm = document.querySelector("whereby-embed");
  const output = document.querySelector("output");

  function logEvent(event) {
    output.innerText += `got event ${JSON.stringify({ type: event.type, detail: event.detail })}\n`;
  }
  elm.addEventListener("ready", logEvent)
  elm.addEventListener("knock", logEvent)
  elm.addEventListener("participantupdate", logEvent)
  elm.addEventListener("join", logEvent)
  elm.addEventListener("leave", logEvent)
  elm.addEventListener("participant_join", logEvent)
  elm.addEventListener("participant_leave", logEvent)
  elm.addEventListener("microphone_toggle", logEvent)
  elm.addEventListener("camera_toggle", logEvent)
  elm.addEventListener("chat_toggle", logEvent)
  elm.addEventListener("people_toggle", logEvent)
  elm.addEventListener("pip_toggle", logEvent)
  elm.addEventListener("deny_device_permission", logEvent)
  elm.addEventListener("screenshare_toggle", logEvent)
  elm.addEventListener("streaming_status_change", logEvent)
  elm.addEventListener("connection_status_change", logEvent)
```

Example output:

```javascript
got event {"type":"participantupdate","detail":{"count":1}}
got event {"type":"join","detail":null}
got event {"type":"leave","detail":{"removed":false}}
got event {"type":"participant_join","detail":{"metadata":"userId"}}
got event {"type":"participant_leave","detail":{"metadata":"userId"}}
got event {"type":"deny_device_permission","detail":{"denied":false}}
got event {"type":"streaming_status_change","detail":{"status":"starting"}}
got event {"type":"connection_status_change","detail":{"status":"stable"}}
```

### Sending commands

{% hint style="warning" %}
For this feature to work, you must add the origin of your application to the "[Allowed domains](broken-reference)" section in your Whereby account. If not present, the following methods will not do anything.
{% endhint %}

The `<whereby-embed>` component exposes a set of methods your application can invoke to perform actions in the room. Currently, the following methods are available:

| Method                               | Description                                                                                                  |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| `startRecording()`                   | Start cloud recording on behalf of the local user, who needs to be a host in the room.                       |
| `stopRecording()`                    | Stop cloud recording on behalf of the local user, who needs to be a host in the room.                        |
| `startStreaming()`                   | Start streaming on behalf of the local user, who needs to be a host in the room.                             |
| `stopStreaming()`                    | Stop streaming on behalf of the local user, who needs to be a host in the room.                              |
| `toggleCamera([true \| false])`      | Toggle the local user's camera on or off. Without any arguments, it toggles depending on current state.      |
| `toggleMicrophone([true \| false])`  | Toggle the local user's microphone on or off. Without any arguments, it toggles depending on current state.  |
| `toggleScreenshare([true \| false])` | Toggle the local user's screenshare on or off. Without any arguments, it toggles depending on current state. |
| `toggleChat([true \| false])`        | Toggle the local user's chat open or closed. Without any arguments, it toggles depending on current state.   |

Here is a small example showing uses of these methods:

```javascript
const room = document.querySelector("whereby-embed");
room.startRecording(); // Start cloud recording
room.stopRecording(); // Stop cloud recording
room.startStreaming(); // Start streaming
room.stopStreaming(); // Stop streaming
room.toggleCamera(); // Camera will be turned on if off, and off if on
room.toggleMicrophone(); // Microphone will be turned on if off, and off if on
room.toggleScreenshare(); // Screenshare will be turned on if off, and off if on
room.toggleChat(); // Chat will be opened if on, and closed if off
room.toggleCamera(true); // Turn camera on
room.toggleMicrophone(true); // Turn microphone on
room.toggleScreenshare(true); // Turn screenshare on
room.toggleCamera(false); // Turn camera off
room.toggleMicrophone(false); // Turn microphone off
room.toggleScreenshare(false); // Turn screenshare off
```
