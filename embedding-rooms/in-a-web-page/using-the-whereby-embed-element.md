---
description: >-
  This web component allows you to embed a Whereby room in any webpage. It
  provides a more readable integration, and we’ve also exposed local client
  events that are sent from the room to the component.
---

# Using the \<whereby-embed> element

You can try it out like this:

{% tabs %}
{% tab title="HTML" %}
```html
<script type=module src="https://cdn.srv.whereby.com/embed/v1.js"></script>
<style>whereby-embed { height: 700px; }</style>
<whereby-embed minimal room="https://subdomain.whereby.com/your_room"></whereby-embed>
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
This element changed location in October 2021, use the new URL\
`https://cdn.srv.whereby.com/embed/v1.js` \
to make sure you are on the latest version.
{% endhint %}

You can also use the `hostRoomUrl` instead of the `roomUrl`, if you want to give the user host privileges:

{% tabs %}
{% tab title="HTML" %}
```html
<whereby-embed room="https://subdomain.whereby.com/your_room?roomKey=3fe345a"></whereby-embed>
```
{% endtab %}
{% endtabs %}

### Attributes you can use

| Attribute        | Value                   | Description                                                   |
| ---------------- | ----------------------- | ------------------------------------------------------------- |
| `room`           | A room URL              | The full URL of the room you want to embed (required)         |
| `minimal`        | Nothing (on) or `"off"` | Apply minimal UI for embed scenarios                          |
| `displayName`    | Any string              | Set displayname of participant                                |
| `audio=off`      | `"off"`                 | Enter meeting with audio off                                  |
| `video=off`      | `"off"`                 | Enter meeting with video off                                  |
| `background=off` | `"off"`                 | Render without background to let embedding app render its own |
| `chat`           | Nothing (on) or `"off"` | Enable/disable chat                                           |
| `people`         | `"off"`                 | Disable the participant list                                  |
| `leaveButton`    | Nothing (on) or `"off"` | Enable/disable the leave button                               |
| `screenshare`    | Nothing (on) or `"off"` | Enable/disable screenshare                                    |
| `subgridLabels`  | Nothing (off) or `"on"` | Enable/disable name labels in the subgrid                     |
| `floatSelf`      | Nothing (on) or `"off"` | Float the self view to the bottom right                       |

The full list of supported attributes can be found in the [URL parameters](../../customizing-rooms/using-url-parameters.md) section.

#### Usage examples

Basic room embedding:

```html
<whereby-embed room="https://subdomain.whereby.com/your_room" />
```

Room embedding with minimal UI:

```html
<whereby-embed room="https://subdomain.whereby.com/your_room" minimal />
```

Setting the display name for the current user, starting with audio and video off:

```html
<whereby-embed room="https://subdomain.whereby.com/your_room" displayName="John Smith" audio=off video=off />
```

Removing the background to blend with the embedding app and turning off some buttons:

```html
<whereby-embed room="https://subdomain.whereby.com/your_room" background=off chat=off people=off leaveButton=off />
```

Use a minimal UI but with the leave and screenshare buttons:

```html
<whereby-embed room="https://subdomain.whereby.com/your_room" minimal leaveButton screenshare />
```

Disable the floating self view:

```html
<whereby-embed room="https://subdomain.whereby.com/your_room" floatSelf=off />
```

### Listening to events

You can listen for events on the `<whereby-embed>` element. The following events are supported:

| Event type          | Description                                                     | Properties (from "detail")            |
| ------------------- | --------------------------------------------------------------- | ------------------------------------- |
| `ready`             | Basic dependencies have loaded and the room is ready to be used | –                                     |
| `knock`             | The local user knocks to get into the room                      | –                                     |
| `participantupdate` | A new participant join/leave                                    | { count: Number }                     |
| `join`              | The local user joins                                            | –                                     |
| `leave`             | The local user leaves                                           | { removed: Boolean }                  |
| `participant_join`  | A new participant joins the room                                | { participant: { metadata: String } } |
| `participant_leave` | A participant leaves the room                                   | { participant: { metadata: String } } |
| `microphone_toggle` | The local user toggles the microphone                           | { enabled: Boolean }                  |
| `camera_toggle`     | The local user toggles the camera                               | { enabled: Boolean }                  |



You can use standard JavaScript to listen to the events. Here's a small demo:

{% tabs %}
{% tab title="Javascript" %}
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
```
{% endtab %}
{% endtabs %}

Example output:

```javascript
got event {"type":"participantupdate","detail":{"count":1}}
got event {"type":"join","detail":null}
got event {"type":"leave","detail":{"removed":false}}
got event {"type":"participant_join","detail":{"metadata":"userId"}}
got event {"type":"participant_leave","detail":{"metadata":"userId"}}
```

### Sending commands

The `<whereby-embed>` element exposes a set of methods your application can invoke in order to perform actions in the room. Currently, the following methods are available:

{% hint style="warning" %}
For this feature to work, you must add the origin of your application to the "[Allowed domains](../allowed-domains.md)" section in your Whereby account. If not present, the following methods will not do anything.
{% endhint %}

| Method                              | Description                                                                                                 |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| `toggleCamera([true \| false])`     | Toggle the local user's camera on or off. Without any arguments, it toggles depending on current state.     |
| `toggleMicrophone([true \| false])` | Toggle the local user's microphone on or off. Without any arguments, it toggles depending on current state. |

Here is a small example showing uses of these methods:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const room = document.querySelector("whereby-embed");
room.toggleCamera(); // Camera will be turned on if off, and off if on
room.toggleMicrophone(); // Microphone will be turned on if off, and off if on
room.toggleCamera(true); // Turn camera on
room.toggleMicrophone(true); // Turn microphone on
room.toggleCamera(false); // Turn camera off
room.toggleMicrophone(false); // Turn microphone off


```
{% endtab %}
{% endtabs %}
