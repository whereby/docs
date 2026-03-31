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

You can use a `hostRoomUrl` instead of the `roomUrl`, if you want to give the user [host privileges](../whereby-product-features/user-roles-and-privileges.md#hosts)
{% endtab %}

{% tab title="JSX" %}
```jsx
const MyComponent = ({ roomUrl }) => {
    return <whereby-embed room={roomUrl} />
}

export default MyComponent
```

You can use a `hostRoomUrl` instead of the `roomUrl`, if you want to give the user [host privileges](../whereby-product-features/user-roles-and-privileges.md#hosts)
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If you aren't using a bundler or a library containing a bundler you can access the component code directly from our CDN using a [simple Script tag](../whereby-for-web-browser/web-component-and-pre-built-ui/web-app-quickstart.md#using-a-less-than-script-greater-than-element) in your site
{% endhint %}

### Attributes of the component

The web component has a variety of attributes available to customize the meeting experience for your users. It’s possible for each participant in a meeting to have different attribute combinations.

<table data-full-width="true"><thead><tr><th width="301.33333333333337">Attribute</th><th>Description</th></tr></thead><tbody><tr><td><code>aec=off</code></td><td><p>Turn off acoustic echo cancellation on audio, useful in situations where higher quality audio is important like in music lessons.</p><p></p><p>AEC must be on in order to use the in room "Noise Reduction" feature.</p></td></tr><tr><td><code>agc=off</code></td><td><p>Turn off automatic gain control (or volume level) on audio.</p><p></p><p>AGC must be on in order to use the in room "Noise Reduction" feature.</p></td></tr><tr><td><code>audio=off</code></td><td>Participant joins the room with microphone turned off.</td></tr><tr><td><p><br><code>audioDenoiser=on</code></p><p><code>audioDenoiser=off</code></p></td><td><p>Enables/Disables the noise cancellation feature.</p><p></p><p>Can increase CPU load, particularly for older/slower devices.</p></td></tr><tr><td><code>autoHideSelfView</code></td><td><p>Automatically hide the self view in the bottom right.</p><p></p><p>Floating the self view to the bottom right is required in order to hide the self view. Must be used with <strong>floatSelf</strong> for this feature to work.</p></td></tr><tr><td><code>autoSpotlight</code></td><td><p>Automatically spotlight the local participant on room join.</p><p></p><p>Only works when the participant joining has host privileges.</p></td></tr><tr><td><code>avatarUrl=&#x3C;url></code></td><td><p>Set the profile avatar of participant.</p><p></p><p>Images must be a square .png or .jpg, maximum of 64pxx64px<br><br>The image URL must be listed in the <a href="../whereby-101/customizing-rooms/broken-reference/">allowed domains</a> section of the dashboard. The image URL must be https and cannot contain query params.</p></td></tr><tr><td><code>background=off</code></td><td>Hide the room background.</td></tr><tr><td><p><code>bottomToolbar=on</code></p><p><code>bottomToolbar=off</code></p></td><td>Show/hide the entire bottom toolbar.</td></tr><tr><td><p><code>breakout=on</code></p><p><code>breakout=off</code></p></td><td>Show/hide the breakout room feature for the meeting host.</td></tr><tr><td><p><code>callQualityMonitoring=on</code></p><p><code>callQualityMonitoring=off</code></p></td><td><p>Show/hide the <a href="../whereby-product-features/insights-suite-and-api/real-time-troubleshooting.md#meeting-diagnostics">meeting diagnostics</a> button, sidebar, and indicators.</p><p></p><p>The rooms top toolbar is required to use the meeting diagnostics feature. </p></td></tr><tr><td><p><code>cameraAccess=on</code></p><p><code>cameraAccess=off</code></p></td><td>Camera permissions are not requested or used at all. On by default.</td></tr><tr><td><code>cameraEffect=&#x3C;effect></code></td><td><p>Set default camera effect to be used. Still user changeable.</p><p></p><p><strong>Possible values:</strong> <code>slight-blur</code>, <code>blur</code>, <code>image-cabin</code>, <code>image-concrete</code>, <code>image-brick</code>, <code>image-sunrise</code>, <code>image-day</code>, <code>image-night</code></p></td></tr><tr><td><p><code>chat=on</code></p><p><code>chat=off</code></p></td><td>Show/hide the chat button.</td></tr><tr><td><p><code>chatDownload=on</code></p><p><code>chatDownload=off</code></p></td><td><p>Allows you to download the chat transcript from a session once it's finished.</p><p></p><p>This will only enable the chat download functionality for the user that has this parameter included in their URL.</p></td></tr><tr><td><code>displayName=&#x3C;name></code></td><td><p>Set display name of participant.</p><p></p><p>Supports <strong>any characters except code characters <code>$!&#x3C;>:;</code></strong> and can be <strong>up to 100 characters long</strong>. Strings not in this format will be rejected and the user will be prompted to enter a different display name instead.</p></td></tr><tr><td><code>externalId=&#x3C;id></code></td><td><p>A custom identifier for the participant. Gets saved in Insights data.</p><p></p><p>Supports <strong>alphanumeric characters</strong> (<code>A-Za-z0-9-_</code>) and can be <strong>up to 36 characters long</strong>. Strings not in this format will be rejected and return an error. We recommend the UUID v4 format.</p></td></tr><tr><td><code>floatSelf</code></td><td>Float the self view to the bottom right.</td></tr><tr><td><p><code>groups=&#x3C;group-names></code><br> </p><p><code>e.g. Orange,Banana,Coconut</code></p></td><td>Predefine up to 20 groups for the breakout groups function.</td></tr><tr><td><p><code>highlightActiveSpeaker=on</code></p><p><code>highlightActiveSpeaker=off</code></p></td><td>Show/hide active speaker highlighting in real-time.</td></tr><tr><td><code>lang=&#x3C;code></code></td><td><p>Set the room UI language.</p><p></p><p><strong>Possible values</strong>: Czech <code>cs</code>, Chinese (Traditional) <code>zh-hant</code>, Danish <code>da</code>, Dutch <code>nl</code>, English <code>en</code>, French <code>fr</code>, German <code>de</code>, Greek <code>el</code>, Hindi <code>hi</code>, Hungarian <code>hu</code>, Italian <code>it</code>, Japanese <code>jp</code>, Norwegian <code>nb</code>,  Portuguese <code>pt</code>, Polish <code>pl</code>, Spanish <code>es</code>, Swedish <code>sv</code>, or Ukrainian <code>uk</code></p></td></tr><tr><td><p><code>leaveButton=on</code></p><p><code>leaveButton=off</code></p></td><td>Show/hide the leave button.</td></tr><tr><td><p><code>localization=on</code></p><p><code>localization=off</code></p></td><td><p>Show/hide the language picker.</p><p></p><p>Hiding the localization options means the user will not be able to change their language preference.</p></td></tr><tr><td><code>locking=off</code></td><td>Hide the room lock button.</td></tr><tr><td><code>logo=off</code></td><td>Hide the logo in the room header.</td></tr><tr><td><p><code>lowData=on</code></p><p><code>lowData=off</code></p></td><td>Use a lower resolution by default.</td></tr><tr><td><code>metadata=&#x3C;string></code></td><td>Gets passed on to the corresponding webhooks.</td></tr><tr><td><code>minimal</code></td><td><p>Applies a minimal UI. Turns off all controls except for cam and mic.</p><p></p><p><strong>Hidden items:</strong> top bar, chat, screensharing, leave button, and Whereby’s default branding.<br></p><p><strong>Shown items:</strong> Video, audio, and people buttons.</p></td></tr><tr><td><code>moreButton=off</code></td><td>Hide the more [...] button.</td></tr><tr><td><code>participantCount=off</code></td><td>Hide the participant counter.</td></tr><tr><td><code>people=off</code></td><td>Hide the people button.</td></tr><tr><td><code>pipButton=off</code></td><td>Hide the Picture in Picture button.</td></tr><tr><td><p><code>precallCeremony=on</code></p><p><code>precallCeremony=off</code></p></td><td><p>Determines if users see the pre-call device and connectivity test.</p><p></p><p>This feature is a subset of the Pre-call review. This means in order for the pre-call tests to appear, pre-call cannot be turned off.</p></td></tr><tr><td><code>precallCeremonyCanSkip=on</code></td><td><p>Adds functionality for participants to skip the connectivity test.</p><p></p><p>This feature is a subset of the Pre-call review. This means in order for the pre-call tests to appear, pre-call cannot be turned off.</p></td></tr><tr><td><code>precallPermissionHelpLink=&#x3C;url></code></td><td>Specify custom help link in pre-call review step pointing users to additional support pages.</td></tr><tr><td><p><code>precallReview=on</code></p><p><code>precallReview=off</code></p></td><td>Determines if users see the pre-call review step.</td></tr><tr><td><code>roomIntegrations=on</code></td><td><p>Enables YouTube and Miro integrations in the meeting.</p><p></p><p>It is recommended you do testing to verify these integrations behave as expected before releasing to your users.</p></td></tr><tr><td><p><code>screenshare=on</code></p><p><code>screenshare=off</code></p></td><td>Show/hide the screenshare button.</td></tr><tr><td><code>settingsButton=off</code></td><td>Hide the settings button.</td></tr><tr><td><code>skipMediaPermissionPrompt</code></td><td>Skip the 'request permissions' screen and immediately trigger the browser’s camera/microphone prompt</td></tr><tr><td><p><code>subgridLabels=on</code></p><p><code>subgridLabels=off</code></p></td><td>Enable name labels for participants in the subgrid.</td></tr><tr><td><p><code>timer=on</code></p><p><code>timer=off</code></p></td><td>Show/hide the meeting timer.</td></tr><tr><td><code>toolbarDarkText</code></td><td>Sets button icon labels color to black.</td></tr><tr><td><code>toolbarText=on</code><br><code>toolbarText=off</code></td><td>Removes text from labels in the room toolbar.</td></tr><tr><td><p><code>topToolbar=on</code></p><p><code>topToolbar=off</code></p></td><td>Show/hide the entire top toolbar.</td></tr><tr><td><code>video=off</code></td><td>Participant joins the room with camera turned off.</td></tr><tr><td><code>virtualBackgroundUrl=&#x3C;url></code></td><td><p>Specify custom virtual background which should be applied to the local participant.</p><p></p><p>The image must be provided in <strong>1280x720</strong> resolution in either <code>image/jpeg</code> or <code>image/png</code> format. In addition, the image should be retrievable using CORS.</p></td></tr></tbody></table>

{% hint style="success" %}
There are additional room customizations and options that can be found in the [URL parameters](../whereby-for-web-browser/web-component-and-pre-built-ui/configuring-with-attributes.md) section. These options must be added as parameters on the `room` source URL, and are not currently supported as attributes directly on the component.
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

<table><thead><tr><th width="289.3333333333333">Event type</th><th width="190">Description</th><th>Properties (from "detail")</th></tr></thead><tbody><tr><td><code>ready</code></td><td>Basic dependencies have loaded and the room is ready to be used</td><td>–</td></tr><tr><td><code>grant_device_permission</code></td><td>The local user grants permission to camera and microphone in the pre-call screen</td><td>{ granted: Boolean }</td></tr><tr><td><code>deny_device_permission</code></td><td>The local user denies permission to camera and microphone in the pre-call screen</td><td>{ denied: Boolean }</td></tr><tr><td><code>precall_check_completed</code></td><td>The local user <em>completed</em> the presented pre-call check steps. <br><br>Possible values for all <code>status</code> properties: <code>passed</code> | <code>failed</code>.</td><td><p>{ </p><p>  status: String, </p><p>  steps: { </p><p>    camera: { status: String; },</p><p>    speaker: { status: String; },</p><p>    microphone: { status: String; },</p><p>    bandwidth: { status: String; },</p><p>  }</p><p>}</p></td></tr><tr><td><code>precall_check_skipped</code></td><td>The local user <em>skipped</em> the presented pre-call check steps</td><td>-</td></tr><tr><td><code>knock</code></td><td>The local user knocks to get into the room</td><td>–</td></tr><tr><td><code>cancel_knock</code></td><td>The local user cancels the knock</td><td>-</td></tr><tr><td><code>join</code></td><td>The local user joins</td><td>–</td></tr><tr><td><code>leave</code></td><td>The local user leaves</td><td>{ removed: Boolean }</td></tr><tr><td><code>meeting_end</code></td><td>A host has ended the meeting via "end meeting for all"</td><td>–</td></tr><tr><td><code>participantupdate</code></td><td>A new participant join/leave</td><td>{ count: Number }</td></tr><tr><td><code>participant_join</code></td><td>A new participant joins the room</td><td><p>{ </p><p>  participant: { </p><p>    metadata: String </p><p>  }</p><p>}</p></td></tr><tr><td><code>participant_leave</code></td><td>A participant leaves the room</td><td><p>{ </p><p>  participant: { </p><p>    metadata: String </p><p>  }</p><p>}</p></td></tr><tr><td><code>microphone_toggle</code></td><td>The local user toggles the microphone</td><td>{ enabled: Boolean }</td></tr><tr><td><code>camera_toggle</code></td><td>The local user toggles the camera</td><td>{ enabled: Boolean }</td></tr><tr><td><code>chat_toggle</code></td><td>The local user toggles the chat</td><td>{ open: Boolean }</td></tr><tr><td><code>people_toggle</code></td><td>The local user toggles the people pane</td><td>{ open: Boolean }</td></tr><tr><td><code>pip_toggle</code></td><td>The local user toggles Picture-in-Picture mode</td><td>{ open: Boolean }</td></tr><tr><td><code>screenshare_toggle</code></td><td>The local user toggles the screenshare</td><td>{ enabled: Boolean }</td></tr><tr><td><code>streaming_status_change</code></td><td>Streaming status changes. <br><br>Possible values: <code>requested</code> | <code>starting</code> | <code>streaming</code> | <code>stopping</code> | <code>stopped</code></td><td>{ status: String }</td></tr><tr><td><code>recording_status_change</code></td><td>Recording status changes. <br><br>Possible values: <code>starting</code> | <code>started</code> |  <code>stopped</code></td><td>{ status: String }</td></tr><tr><td><code>transcription_status_change</code></td><td>Transcription status changes. <br><br>Possible values: <code>starting</code> | <code>started</code> |  <code>stopped</code></td><td>{ status: String }</td></tr><tr><td><code>connection_status_change</code></td><td>User connection status changes. <br><br>Possible values: <code>stable</code> | <code>unstable</code></td><td>{ status: String }</td></tr></tbody></table>



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

### Sending commands <a href="#sending-commands" id="sending-commands"></a>

{% hint style="warning" %}
For this feature to work, you must add the origin of your application to the "[Allowed domains](../further-resources/faq-and-troubleshooting/allowed-domains-and-localhost.md)" section in your Whereby account. If not present, the following methods will not do anything.
{% endhint %}

The `<whereby-embed>` component exposes a set of methods your application can invoke to perform actions in the room. Currently, the following methods are available:

| Method                               | Description                                                                                                  |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| `endMeeting()`                       | End meeting for all on behalf of the local user, who needs to be a host in the room.                         |
| `knock()`                            | Knock on a locker room, on behalf of the local user.                                                         |
| `cancelKnock()`                      | Cancel the knock, on behalf of the local user.                                                               |
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
