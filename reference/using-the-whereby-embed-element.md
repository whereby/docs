---
description: >-
  Our web component allows you to embed Whereby's pre-built UI on any webpage.
  It provides a simple readable integration and exposes local client events and
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
If you aren't using a bundler or a library containing a bundler you can access the component code directly from our CDN using a [simple Script tag](../whereby-101/create-your-video/in-a-web-page/using-the-whereby-embed-element/script-tags.md) in your site
{% endhint %}

### Attributes of the component

The web component has a variety of attributes available to customize the meeting experience for your users. It’s possible for each participant in a meeting to have different attribute combinations.

<table><thead><tr><th width="215.33333333333331">Attribute</th><th>Value</th><th>Description</th></tr></thead><tbody><tr><td><code>aec</code></td><td>Nothing (on) or <code>"off"</code></td><td>Turn off acoustic echo cancellation on audio</td></tr><tr><td><code>agc</code></td><td>Nothing (on) or <code>"off"</code></td><td>Turn off acoustic echo cancellation on audio</td></tr><tr><td><code>audio</code></td><td><code>"off"</code></td><td>Enter meeting with audio off</td></tr><tr><td><code>audioDenoiser</code></td><td>Nothing (on) or <code>"off"</code></td><td>Enables/Disables the noise cancelation feature</td></tr><tr><td><code>autoHideSelfView</code></td><td>Nothing (off) or <code>"on"</code></td><td>Automatically hide the self view in the bottom right</td></tr><tr><td><code>autoSpotlight</code></td><td>Nothing (on) or <code>"off"</code></td><td>Automatically spotlight the local "<a href="../whereby-101/user-roles-and-privileges.md#hosts">host</a>" on room join</td></tr><tr><td><code>avatarUrl</code></td><td>Image URL - Must be HTTPS and cannot contain query params</td><td>Set the avatar picture of the participant. The image must be a <code>.png</code> or <code>.jpeg</code>, and a square maximum of 64x64.</td></tr><tr><td><code>background</code></td><td><code>"off"</code></td><td>Render without background to let embedding app render its own</td></tr><tr><td><code>bottomToolbar</code></td><td><code>"off"</code></td><td>Hide the entire bottom tool bar. Useful if creating buttons using <a href="using-the-whereby-embed-element.md#sending-commands">commands</a></td></tr><tr><td><code>breakout</code></td><td>Nothing (on) or <code>"off"</code></td><td>Show/hide the <a href="https://docs.whereby.com/whereby-101/customizing-rooms/breakout-groups-with-embedded">Breakout Groups feature</a> for the meeting host.</td></tr><tr><td><code>cameraAccess</code></td><td>Nothing (on) or <code>"off"</code></td><td>Disable camera access for the local participant, allowing for only audio input</td></tr><tr><td><code>cameraEffect</code></td><td><code>slight-blur</code>, <code>blur</code>, <code>image-cabin</code>, <code>image-concrete</code>, <code>image-brick</code>, <code>image-sunrise</code>, <code>image-day</code>, <code>image-night</code></td><td>Set the default camera effect to be used.</td></tr><tr><td><code>chat</code></td><td>Nothing (on) or <code>"off"</code></td><td>Enable/disable chat</td></tr><tr><td><code>callQualityMonitoring</code></td><td>Nothing(off) or <code>"on"</code></td><td>Enable/disable Whereby's call quality monitoring interface</td></tr><tr><td><code>displayName</code></td><td>100 character string - Excluding <strong><code>$!&#x3C;>:;</code></strong></td><td>Set displayname of participant</td></tr><tr><td><code>externalId</code></td><td>Up to 36 alphanumeric characters (<a href="../whereby-101/customizing-rooms/using-url-parameters.md#externalid-less-than-id-greater-than">details</a>)</td><td>Custom identifier for the participant. Can be searched in insights endpoints</td></tr><tr><td><code>floatSelf</code></td><td>Nothing (on) or <code>"off"</code></td><td>Float the self view to the bottom right</td></tr><tr><td><code>groups</code></td><td>"Group Name(s)" - EX:<br><br>"orange, banana, coconut"</td><td>Predefine up to 20 groups for the breakout groups function.</td></tr><tr><td><code>lang</code></td><td>Nothing (en) or <code>fr | it | de | nb | da | nl | pt | pl | es | hi | cs | zh-hant | jp</code></td><td>Set the meeting UI language to match your product or service. Select from either English en, French fr, Italian it, German de, Norwegian nb, Danish da, Dutch nl, Portuguese pt, Polish pl, Spanish es, Hindi hi, Czech cs, Chinese (Complex) zh-hant or Japanese jp.</td></tr><tr><td><code>leaveButton</code></td><td>Nothing (on) or <code>"off"</code></td><td>Enable/disable the leave button</td></tr><tr><td><code>localization</code></td><td>Nothing (on) or <code>"off"</code></td><td>Show/hide the language picker</td></tr><tr><td><code>locking</code></td><td>Nothing (on) or <code>"off"</code></td><td>Show/hide the room lock button and settings toggle</td></tr><tr><td><code>logo</code></td><td>Nothing (on) or <code>"off"</code></td><td>Show/hide the logo in the room header</td></tr><tr><td><code>lowData</code></td><td>Nothing (on) or <code>"off"</code></td><td>Use a lower resolution by default</td></tr><tr><td><code>metadata</code></td><td><code>&#x3C;string></code></td><td><code>&#x3C;string></code> gets passed on to the corresponding webhooks</td></tr><tr><td><code>minimal</code></td><td>Nothing (on) or <code>"off"</code></td><td>Apply minimal UI for embed scenarios</td></tr><tr><td><code>moreButton</code></td><td>Nothing (on) or <code>"off"</code></td><td>Show/hide the “…” button</td></tr><tr><td><code>participantCount</code></td><td>Nothing (on) or <code>"off"</code></td><td>Show/hide the participant counter</td></tr><tr><td><code>people</code></td><td>Nothing (on) or <code>"off"</code></td><td>Show/hide the participant list</td></tr><tr><td><code>pipButton</code></td><td>Nothing (on) or <code>"off"</code></td><td>Show/hide the Picture in Picture button</td></tr><tr><td><code>precallCeremony</code></td><td>Nothing (on) or <code>"off"</code></td><td>Determines if users see the pre-call device and connectivity test</td></tr><tr><td><code>precallCeremonyCanSkip</code></td><td>Nothing (off) or <code>"on"</code></td><td>Adds functionality for participants to skip the connectivity test</td></tr><tr><td><code>precallPermissionHelpLink</code></td><td><code>&#x3C;url></code></td><td>Specify custom help link in pre-call review step pointing users to additional support pages</td></tr><tr><td><code>room</code></td><td>A room URL</td><td>The full URL of the room you want to embed (required)</td></tr><tr><td><code>roomIntegrations</code></td><td>Nothing (off) or <code>"on"</code></td><td>Enables YouTube and Miro integrations in the meeting</td></tr><tr><td><code>screenshare</code></td><td>Nothing (on) or <code>"off"</code></td><td>Enable/disable screenshare</td></tr><tr><td><code>skipMediaPermissionPrompt</code></td><td>Nothing (on) or <code>"off"</code></td><td>Skips request permissions UI and asks for devices directly. Required for links used in <a href="../whereby-101/create-your-video/in-a-mobile-app/in-android-apps/">Android Apps</a></td></tr><tr><td><code>subgridLabels</code></td><td>Nothing (off) or <code>"on"</code></td><td>Show/hide name labels in the subgrid</td></tr><tr><td><code>timer</code></td><td>Nothing (on) or <code>"off"</code></td><td>Show/hide the meeting timer</td></tr><tr><td><code>toolbarDarkText</code></td><td>Nothing (on) or <code>"off"</code></td><td>Sets button icon labels color to black.</td></tr><tr><td><code>topToolbar</code></td><td>Nothing (on) or <code>"off"</code></td><td>Used to toggle the entire top toolbar on/off. Top toolbar is hidden by default if using <code>minimal</code></td></tr><tr><td><code>video</code></td><td><code>"off"</code></td><td>Enter meeting with video off</td></tr></tbody></table>

{% hint style="success" %}
There are additional room customizations and options that can be found in the [URL parameters](../whereby-101/customizing-rooms/using-url-parameters.md) section. These options must be added as parameters on the `room` source URL, and are not currently supported as attributes directly on the component.
{% endhint %}

#### Usage examples

{% tabs %}
{% tab title="Room w/ multiple customizations" %}
{% code overflow="wrap" %}
```html
<whereby-embed room="https://subdomain.whereby.com/your_room" displayName="John Smith" audio=off video=off background=off chat=off />
```
{% endcode %}
{% endtab %}

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
{% endtabs %}



### Listening to events

You can listen for events on the `<whereby-embed>` component. The following events are supported:

<table><thead><tr><th width="289.3333333333333">Event type</th><th width="190">Description</th><th>Properties (from "detail")</th></tr></thead><tbody><tr><td><code>ready</code></td><td>Basic dependencies have loaded and the room is ready to be used</td><td>–</td></tr><tr><td><code>grant_device_permission</code></td><td>The local user grants permission to camera and microphone in the pre-call screen</td><td>{ granted: Boolean }</td></tr><tr><td><code>deny_device_permission</code></td><td>The local user denies permission to camera and microphone in the pre-call screen</td><td>{ denied: Boolean }</td></tr><tr><td><code>precall_check_completed</code></td><td>The local user <em>completed</em> the presented pre-call check steps. <br><br>Possible values for all <code>status</code> properties: "passed | failed".</td><td><p>{ </p><p>  status: String, </p><p>  steps: { </p><p>    camera: { status: String; },</p><p>    speaker: { status: String; },</p><p>    microphone: { status: String; },</p><p>    bandwidth: { status: String; },</p><p>  }</p><p>}</p></td></tr><tr><td><code>precall_check_skipped</code></td><td>The local user <em>skipped</em> the presented pre-call check steps</td><td>-</td></tr><tr><td><code>knock</code></td><td>The local user knocks to get into the room</td><td>–</td></tr><tr><td><code>cancel_knock</code></td><td>The local user cancels the knock</td><td>-</td></tr><tr><td><code>join</code></td><td>The local user joins</td><td>–</td></tr><tr><td><code>leave</code></td><td>The local user leaves</td><td>{ removed: Boolean }</td></tr><tr><td><code>meeting_end</code></td><td>A host has ended the meeting via "end meeting for all"</td><td>–</td></tr><tr><td><code>participantupdate</code></td><td>A new participant join/leave</td><td>{ count: Number }</td></tr><tr><td><code>participant_join</code></td><td>A new participant joins the room</td><td><p>{ </p><p>  participant: { </p><p>    metadata: String </p><p>  }</p><p>}</p></td></tr><tr><td><code>participant_leave</code></td><td>A participant leaves the room</td><td><p>{ </p><p>  participant: { </p><p>    metadata: String </p><p>  }</p><p>}</p></td></tr><tr><td><code>microphone_toggle</code></td><td>The local user toggles the microphone</td><td>{ enabled: Boolean }</td></tr><tr><td><code>camera_toggle</code></td><td>The local user toggles the camera</td><td>{ enabled: Boolean }</td></tr><tr><td><code>chat_toggle</code></td><td>The local user toggles the chat</td><td>{ open: Boolean }</td></tr><tr><td><code>people_toggle</code></td><td>The local user toggles the people pane</td><td>{ open: Boolean }</td></tr><tr><td><code>pip_toggle</code></td><td>The local user toggles Picture-in-Picture mode</td><td>{ open: Boolean }</td></tr><tr><td><code>screenshare_toggle</code></td><td>The local user toggles the screenshare</td><td>{ enabled: Boolean }</td></tr><tr><td><code>streaming_status_change</code></td><td>Streaming status changes. <br><br>Possible values: " | requested | starting | streaming | stopping | stopped"</td><td>{ status: String }</td></tr><tr><td><code>connection_status_change</code></td><td>User connection status changes. <br><br>Possible values: "stable | unstable"</td><td>{ status: String }</td></tr></tbody></table>



You can use standard JavaScript to listen to the events. Here's an example:

```javascript
  const elm = document.querySelector("whereby-embed");
  let output = "";

  function logEvent(event) {
    output = `got event ${JSON.stringify({ type: event.type, detail: event.detail })}\n`;
    console.log(output)
  }
  elm.addEventListener("ready", logEvent)
  elm.addEventListener("precall_check_skipped", logEvent)
  elm.addEventListener("precall_check_completed", logEvent)
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
got event {"type":"ready","detail":null}
got event {"type":"deny_device_permission","detail":{"denied":false}}
got event {"type":"precall_check_completed","detail":{"status":"failed",steps: {"camera":"passed","speaker":"failed","microphone":"failed","bandwidth":"passed"}}}
got event {"type":"join","detail":null}
got event {"type":"participant_join","detail":{"metadata":"userId"}}
got event {"type":"participantupdate","detail":{"count":2}}
got event {"type":"participant_leave","detail":{"metadata":"userId"}}
got event {"type":"participantupdate","detail":{"count":1}}
got event {"type":"streaming_status_change","detail":{"status":"starting"}}
got event {"type":"connection_status_change","detail":{"status":"unstable"}}
got event {"type":"leave","detail":{"removed":false}}
```

### Sending commands

{% hint style="warning" %}
For this feature to work, you must add the origin of your application to the "[Allowed domains](../whereby-101/faq-and-troubleshooting/allowed-domains-and-localhost.md)" section in your Whereby account. If not present, the following methods will not do anything.
{% endhint %}

The `<whereby-embed>` component exposes a set of methods your application can invoke to perform actions in the room. Currently, the following methods are available:

| Method                               | Description                                                                                                  |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| `endMeeting()`                       | End meeting for all on behalf of the local user, who needs to be a host in the room.                         |
| `knock()`                            | Knock on a locker room, on behalf of the local user.                                                         |
| `leaveRoom()`                        | Allows local user to leave the room.                                                                         |
| `startRecording()`                   | Start cloud recording on behalf of the local user, who needs to be a host in the room.                       |
| `stopRecording()`                    | Stop cloud recording on behalf of the local user, who needs to be a host in the room.                        |
| `startStreaming()`                   | Start streaming on behalf of the local user, who needs to be a host in the room.                             |
| `stopStreaming()`                    | Stop streaming on behalf of the local user, who needs to be a host in the room.                              |
| `startLiveTranscription()`           | Start live transcription on behalf of the local user, who needs to be a host in the room.                    |
| `stopLiveTranscription()`            | Stop live transcription on behalf of the local user, who needs to be a host in the room.                     |
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
room.startLiveTranscription(); // Start live transcription
room.stopLiveTranscription(); // Stop live transcription
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
