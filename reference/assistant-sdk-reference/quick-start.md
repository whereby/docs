---
description: >-
  The following example shows a basic example of creating an assistant to join a
  room and send a chat message
---

# Quick Start

{% hint style="info" %}
Assistants are most powerful when they are **started automatically in response to events**.

In almost all production use cases you will need a **trigger mechanism** to decide _when_ an Assistant should join a room. This can be done with the built-in [**Trigger API**](quick-start.md#using-trigger-api), or by hosting your own service that listens for Whereby webhooks and creates an Assistant instance based on your own custom logic.

Without a trigger, an Assistant can only be started manually, which limits its ability to react to user behavior or integrate seamlessly with in-room events such as participants joining/leaving or room sessions starting/ending.
{% endhint %}

### Getting Started

1. Create your assistant in the Whereby dashboard
   1. Visit Configure > Assistant and create an assistant
   2. Grab your assistant key

<figure><img src="../../.gitbook/assets/Screenshot 2025-09-25 at 09.56.31 (1).png" alt="The Assistant Dashboard"><figcaption></figcaption></figure>

2. Import polyfills and initialize assistant

```javascript
import "@whereby.com/assistant-sdk/polyfills";
```

```jsx
import { Assistant } from "@whereby.com/assistant-sdk";

const assistant = new Assistant({ assistantKey: "my-assistant-key" });
```

3. Have the assistant start to join the room and wait until the room is connected. We should also wrap this call in a try/catch block in case any errors happen during the join room flow:  &#x20;

```jsx
try { 
  await assistant.joinRoom("https://your-subdomain.whereby.com/your-room-name");
} catch(error => {
  console.error("An error occurred joining the room", error);
}); 
```

4. Once the room is connected start using your assistant!&#x20;

```jsx
const roomConnection = assistant.getRoomConnection();
roomConnection.sendChatMessage("Hello! I'm your assistant, how can I help you?");
```

The above steps can be written in with promise-formatting as follows:

```jsx
const assistant = new Assistant({ assistantKey: "my-assistant-key" });
assistant
  .joinRoom("https://your-subdomain.whereby.com/your-room-name")
  .then(() => {
    const roomConnection = assistant.getRoomConnection();
    roomConnection.sendChatMessage("Hello! I'm your assistant, how can I help you?");
  })
  .catch((error) => {
    console.error("An error has occurred", error);
  });
```

### Using Trigger API

If you'd like the assistant to join the room automatically, you can make some small tweaks to our first example to use our webhook trigger API.  This way, you can listen for certain webhook events and create your assistant whenever those webhooks are received! You should still follow step 1 in the above tutorial to obtain the assistant key.&#x20;

1. Create your assistant in the Whereby dashboard
   1. Visit Configure > Assistant and create an assistant
   2. Grab your assistant key
   3. :exclamation:**Make sure to set the endpoint URL in the configuration steps to receive webhooks**
2. Set up start [trigger](api-reference/trigger.md)&#x20;

```jsx
import { Trigger } from "@whereby.com/assistant-sdk";

let hasAssistantJoined = false;

const trigger = new Trigger({
  webhookTriggers: {
    "room.client.joined": () => !hasAssistantJoined,
  },
  port: 3000,
});
```

3. Start the trigger server

```jsx
trigger.start();
```

4. Listen for the  [`TRIGGER_EVENT_SUCCESS`](types/trigger-types.md#triggerevents) event - this indicates that the trigger predicate has been a match, and that your assistant should now join the room. Here, you can create the assistant and join the room before sending a chat message in to the connected room.

```jsx
trigger.on(TRIGGER_EVENT_SUCCESS, async ({ roomUrl }) => {
  console.log("Webhook trigger has been matched!");
  const assistant = new Assistant({ assistantKey: "my-assistant-key" });
  
  try { 
    await assistant.joinRoom("https://your-subdomain.whereby.com/your-room-name");
    console.log("Connected to room");
    hasAssistantJoined = true;
  } catch((error) => {
    console.error("An error occurred joining the room", error);
  });

  const roomConnection = assistant.getRoomConnection();
  roomConnection.sendChatMessage("Hello! I'm your assistant, how can I help you?");
});
```

5. Finally, clean up any open listeners when the assistant eventually leaves the room (e.g. reset the webhook trigger for this room).

```jsx
assistant.on(ASSISTANT_LEFT_ROOM, () => {
  console.log("Disconnected from room");
  hasAssistantJoined = false;
});
```

5. Start building! Here’s a minimal example, using the trigger API and creating an assistant that will send a chat message into the room session and subscribe to the room session's combined audio stream.&#x20;

```jsx
import "@whereby.com/assistant-sdk/polyfills";

import {
    Trigger,
    Assistant,
    TRIGGER_EVENT_SUCCESS,
    ASSISTANT_LEFT_ROOM,
} from "@whereby.com/assistant-sdk";

let hasAssistantJoined = false;

const trigger = new Trigger({
  webhookTriggers: {
    "room.client.joined": () => !hasAssistantJoined,
  },
  port: 3000,
});

trigger.start();

trigger.on(TRIGGER_EVENT_SUCCESS, async ({ roomUrl }) => {
  console.log("Webhook trigger has been matched!");
  const assistant = new Assistant({ assistantKey: "my-assistant-key" });
  
  try { 
    await assistant.joinRoom("https://your-subdomain.whereby.com/your-room-name");
    console.log("Connected to room");
    hasAssistantJoined = true;
  } catch((error) => {
    console.error("An error occurred joining the room", error);
  });
    
  const roomConnection = assistant.getRoomConnection();
  roomConnection.sendChatMessage("Hello! I'm your assistant, how can I help you?");
    
  assistant.on(ASSISTANT_LEFT_ROOM, () => {
    console.log("Disconnected from room");
    hasAssistantJoined = false;
  });
});
```

This demonstrates the most basic functionality of the library. From this example, you can go on to implement more advanced functionality that is explained in more detail in the next sections of the documentation: [API Reference](api-reference/).

Additionally, you can take a look at the source code for an example Whereby Assistant that is published on Github here: [https://github.com/whereby/whereby-assistant-audio-recorder](https://github.com/whereby/whereby-assistant-audio-recorder).
