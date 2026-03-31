# Using Whereby events

Now that you've mastered using custom commands, let's move onto events!

The `<whereby-embed>` web component fires custom events that allow your application to respond to real-time changes in the meeting room. This enables you to create dynamic user interfaces, provide helpful feedback, and implement custom behaviors based on what's happening during the video call.

In this article, we'll show you how to enhance your custom controls with event-driven features, like participant counters and connection status indicators.

### Prerequisites

This guide assumes that you've already created a Whereby Embedded account and a meeting room, and have joined it by adding the `<whereby-embed>` component to a web app.

* If you've not created a Whereby embedded account and a meeting room, you can do so by following the [Whereby Embedded initial setup guide](../../getting-started/overview-of-whereby-embedded/initial-setup.md).
* If you've not built custom controls for your video call web app, you can clone our [finished web component quickstart web app with commands](https://github.com/whereby/demos/tree/main/web-component-with-commands) on GitHub.

### Getting Started

Event listeners allow your application to react to changes in the meeting room without constantly checking the room's state. This is essential for creating responsive user interfaces that provide real-time feedback to users. For example, you can update participant counts as people join and leave, show connection quality indicators, or display helpful messages when users encounter issues.

Open your existing web app with custom controls in your browser and code editor. We'll be building on the button bar you created in the [commands](using-commands.md) tutorial to add dynamic status indicators and user feedback.

### Adding a Status Bar

First, let's add a status bar above your existing button bar to display real-time information about the meeting.

Add the following HTML snippet to your document, above the existing `#button-bar`:

```html
<section id="status-bar">
  <div id="participant-count">Participants: 0</div>
  <div id="connection-status">Connection: Good</div>
  <div id="notification"></div>
</section>
```

### Styling the Status Bar

Let's style the new status bar and adjust the existing layout to accommodate it.

First, update your existing CSS rules to make room for both bars:

{% code expandable="true" %}
```css
whereby-embed {
  height: calc(100% - 80px); /* Increased from 30px to accommodate both bars */
  margin: inherit;
}

#button-bar {
  position: absolute;
  bottom: 0;
  width: 100%;
  background-color: black;
  padding: 10px 0;
  display: flex;
  justify-content: center;
  gap: 20px;
}
```
{% endcode %}

Now, add styling for the status bar:

{% code expandable="true" %}
```css
#status-bar {
  position: absolute;
  bottom: 50px; /* Position above the button bar */
  width: 100%;
  background-color: #333;
  color: white;
  padding: 10px 0;
  display: flex;
  justify-content: space-around;
  align-items: center;
  font-size: 14px;
}

#notification {
  background-color: #007bff;
  padding: 5px 10px;
  border-radius: 4px;
  min-height: 20px;
  opacity: 0;
  transition: opacity 0.3s ease;
}

#notification.show {
  opacity: 1;
}

.connection-good { color: #28a745; }
.connection-unstable { color: #ffc107; }
.connection-offline { color: #dc3545; }
```
{% endcode %}

### Implementing Participant Counter

Now let's add JavaScript to track participants in real-time. Add the following code to your existing `index.js` file:

{% code expandable="true" %}
```javascript
// Add these references alongside your existing button references
const participantCounter = document.getElementById("participant-count");
const connectionStatus = document.getElementById("connection-status");
const notification = document.getElementById("notification");

// Track participant count
whereby.addEventListener("participantupdate", (event) => {
  const count = event.detail.count;
  participantCounter.textContent = `Participants: ${count}`;
  
  // Show notification when someone joins or leaves
  if (count > 1) {
    showNotification(`${count} people in meeting`);
  }
});

// Welcome new participants
whereby.addEventListener("participant_join", (event) => {
  showNotification("Someone joined the meeting!");
});

whereby.addEventListener("participant_leave", (event) => {
  showNotification("Someone left the meeting");
});
```
{% endcode %}

### Adding Connection Status Monitoring

Connection quality is crucial for video calls. Let's add a connection status indicator that updates in real-time:

{% code expandable="true" %}
```javascript
// Monitor connection status
whereby.addEventListener("connection_status_change", (event) => {
  const status = event.detail.status;
  
  // Remove existing connection classes
  connectionStatus.className = "";
  
  switch(status) {
    case "stable":
      connectionStatus.textContent = "Connection: Good";
      connectionStatus.classList.add("connection-good");
      break;
    case "unstable":
      connectionStatus.textContent = "Connection: Unstable";
      connectionStatus.classList.add("connection-unstable");
      showNotification("Connection is unstable - check your internet");
      break;
    case "offline":
      connectionStatus.textContent = "Connection: Offline";
      connectionStatus.classList.add("connection-offline");
      showNotification("You appear to be offline");
      break;
    default:
      connectionStatus.textContent = "Connection: Unknown";
  }
});
```
{% endcode %}

### Creating a Notification System

Add a helper function to display temporary notifications to users:

{% code expandable="true" %}
```javascript
// Helper function to show notifications
function showNotification(message, duration = 3000) {
  notification.textContent = message;
  notification.classList.add("show");
  
  setTimeout(() => {
    notification.classList.remove("show");
  }, duration);
}

// Show helpful messages for common issues
whereby.addEventListener("deny_device_permission", (event) => {
  if (event.detail.denied) {
    showNotification("Camera/microphone access denied. Check your browser settings.", 5000);
  }
});

// Provide feedback when users toggle controls
whereby.addEventListener("camera_toggle", (event) => {
  const isOn = event.detail.enabled;
  showNotification(isOn ? "Camera turned on" : "Camera turned off");
});

whereby.addEventListener("microphone_toggle", (event) => {
  const isOn = event.detail.enabled;
  showNotification(isOn ? "Microphone turned on" : "Microphone turned off");
});
```
{% endcode %}

### Room Lifecycle Management

Finally, let's add some room lifecycle events to provide a complete user experience:

{% code expandable="true" %}
```javascript
// Handle room ready state
whereby.addEventListener("ready", () => {
  showNotification("Room is ready!");
});

// Handle successful join
whereby.addEventListener("join", () => {
  showNotification("You joined the meeting");
});

// Handle pre-call check results
whereby.addEventListener("precall_check_completed", (event) => {
  const status = event.detail.status;
  if (status === "failed") {
    showNotification("Device check failed - you may experience issues", 5000);
  }
});
```
{% endcode %}

Your enhanced video call interface now provides real-time feedback to users about participant changes, connection quality, device issues, and room status. The event-driven approach ensures your UI stays synchronized with the actual state of the meeting without requiring manual polling or state management.

### See Also

* [Event reference](../../reference/using-the-whereby-embed-element.md#listening-to-events): contains a table detailing every available event
* [Using commands](using-commands.md): learn how to create custom controls for your Whereby component
