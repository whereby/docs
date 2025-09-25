# Trigger

The `Trigger` API lets you run an `express` server that listens for Whereby [webhooks](../../../meeting-content-and-quality/insights-suite-and-api/webhooks.md) and, based on your rules, returns a `TRIGGER_EVENT_SUCCESS` event if the conditions of your trigger are met.&#x20;

{% hint style="info" %}
When using Assistant's, i
{% endhint %}

## Constructor

```jsx
const trigger = new Trigger(options: TriggerOptions)
```

### TriggerOptions

<table><thead><tr><th>Parameter</th><th width="279.11981201171875">Type</th><th>Default</th><th>Description</th></tr></thead><tbody><tr><td><code>webhookTriggers</code></td><td><a href="../types/trigger-types.md#wherebywebhooktriggers"><code>WherebyWebhookTriggers</code></a></td><td></td><td></td></tr><tr><td><code>port</code></td><td><code>number</code></td><td><code>4999</code></td><td>Port for the HTTP server</td></tr></tbody></table>

## Methods

| Method  | Parameters | Returns | Description                                             |
| ------- | ---------- | ------- | ------------------------------------------------------- |
| `start` |            | `void`  | Starts the Express server and begins accepting webhooks |

## Events

The `Trigger` API extends `EventEmitter` and emits lifecycle events after your trigger predicate returns `true``.`    &#x20;

```jsx
import { TRIGGER_EVENT_SUCCESS, Trigger } from "@whereby.com/assistant";

let hasJoined = false;
const trigger = new Trigger({
  webhookTriggers: {
    "room.client.joined": () => !hasJoined,
  },
  port: 4999,
});


trigger.on(TRIGGER_EVENT_SUCCESS, ({ roomUrl }) => {
  console.log("Trigger successful for room: ", roomUrl);
});
```

<table><thead><tr><th>Event</th><th width="257.26788330078125">Payload</th><th>Emitted when</th></tr></thead><tbody><tr><td><code>TRIGGER_EVENT_SUCCESS</code></td><td><code>{ roomUrl:string; triggerWebhook:</code> <a href="../types/trigger-types.md#wherebywebhooktype-less-than-type-greater-than"><code>WherebyWebhookType</code></a>    </td><td>Trigger has met the required conditions</td></tr></tbody></table>
