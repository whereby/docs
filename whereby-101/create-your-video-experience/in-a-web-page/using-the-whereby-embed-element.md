---
description: >-
  Our web component allows you to embed a Whereby room in any webpage. It
  provides a simple readable integration and exposes local client events that
  are sent from the component to your platform.
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: false
---

# Using Whereby's Web Component

The Web Component offering in our SDK allows you to leverage the thoughtfully designed, pre-built Whereby interface and user experience we've developed over the last 10+ years.&#x20;

Below are a few examples of how you can use some of the client events and commands that can be sent to a room. We have information on how to get started and install, but for a full document about events, attributes, and commands you can view our Web Component Reference documentation.

{% content-ref url="../../../reference/using-the-whereby-embed-element/" %}
[using-the-whereby-embed-element](../../../reference/using-the-whereby-embed-element/)
{% endcontent-ref %}

## Installation

When using React or a bundler like Webpack, Rollup, Parcel, etc

```bash
npm install @whereby.com/browser-sdk
```

```javascript
import "@whereby.com/browser-sdk"
```

{% tabs %}
{% tab title="HTML" %}
{% code overflow="wrap" %}
```html
<whereby-embed room="https://subdomain.whereby.com/your_room?roomKey=3fe345a"></whereby-embed>
```
{% endcode %}

You can use a `hostRoomUrl` instead of the `roomUrl`, if you want to give the user [host privileges](../../user-roles-and-privileges.md#hosts)
{% endtab %}

{% tab title="JSX" %}
```jsx
const MyComponent = ({ roomUrl }) => {
    return <whereby-embed room={roomUrl} />
}

export default MyComponent
```

You can use a `hostRoomUrl` instead of the `roomUrl`, if you want to give the user [host privileges](../../user-roles-and-privileges.md#hosts)
{% endtab %}
{% endtabs %}

### Listening to events

You can listen for events on the `<whereby-embed>` component. Here are a few examples how the events might be useful, but there are lots of creative ways these might be useful! Let us know in an email or our discord how you've implemented these.

#### leave

When a user leaves the room, you can redirect them to a page of your choosing

{% code overflow="wrap" %}
```javascript
const room = document.querySelector("whereby-embed");

room.addEventListener("leave", () => location.href = "https://whereby.com")
```
{% endcode %}

#### participantupdate

Keep track of the number of participants in a room. Once a room has reached it's capacity (200), you can swap the room source URL to a [viewerRoomUrl](../../user-roles-and-privileges.md#viewers) for additional capacity.

{% code overflow="wrap" %}
```javascript
const whereby = document.querySelector("whereby-embed");
let participantCount = 0

function logEvent(event) {
    participantCount = event.detail.count
  }
  
room.addEventListener("participantupdate", logEvent)

if(participantCount <= 200){
  whereby.room = "https://subdomain.whereby.com/roomname-123"
} else {
  whereby.room = "https://subdomain.whereby.com/roomname-123?roomKey=xyz..."
}
```
{% endcode %}

A few other ideas:

* Display an in app greeting banner whenever there is a new `participant_join` event
* Display an in app message directing users to certain support documentation whenever they accidentally `deny_device_permission`
* Make a log for support or success teams if a user frequently experiences `connection_status_change` errors so they can proactively reach out and investigate connection issues

### Sending commands

{% hint style="warning" %}
For this feature to work, you must add the origin of your application to the "Allowed domains" section on your Whereby dashboard. If not present, the following methods will not function.
{% endhint %}

The `<whereby-embed>` component exposes a set of methods your application can invoke to perform actions in the room.&#x20;

For example, using a [URL parameter](../../customizing-rooms/using-url-parameters.md#bottomtoolbar-less-than-on-or-off-greater-than) you can hide the bottom toolbar and create your own buttons for taking actions in the room:

```javascript
const room = document.querySelector("whereby-embed");
```

{% code overflow="wrap" %}
```markup
<whereby-embed floatSelf background=off displayName="Rick Astley" room="https://subdomain.whereby.com/roomname-123-xyz?bottomToolbar=off"></whereby-embed>

<button onclick="room.toggleCamera()">CAMERA</button>
<button onclick="room.toggleMicrophone()">MIC</button>   
<button onclick="room.toggleScreenshare()">SCREENSHARE</button>
<button onclick="room.toggleChat()">CHAT</button>
```
{% endcode %}
