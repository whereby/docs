---
description: >-
  With Webhooks you can set up user-defined callbacks triggered by meeting
  events. These are a great way to programmatically keep track of some of the
  events and actions happening around your meetings.
---

# Tracking Room Events with Webhooks

## Configuration

Webhooks in Whereby Embedded allow you to track key room events in real-time. You can configure webhooks from the **Configure** section of your Whereby Embedded account dashboard. Only an admin in your organization can set up and manage webhooks.

![Visit the "Configure" section of your Whereby Embedded account dashboard and scroll down to find the Webhooks setup.](../../.gitbook/assets/webhooks-dashboard.png)

### Supported Events

Whereby webhooks support the following event types:

| Event Type | Description |
|------------|-------------|
| `room.client.joined` | Triggered when a user joins the meeting room. |
| `room.client.left` | Triggered when a user leaves the meeting room. This can be via the leave button or by closing the browser tab. |
| `room.client.knocked` | Triggered when a visitor knocks to join a meeting from the waiting room. |
| `room.client.knockCancelled` | Triggered when a visitor cancels their knock request. This can happen via the cancel button or due to network issues. |
| `room.session.started` | Triggered when a room session starts, meaning at least two users are in the room. |
| `room.session.ended` | Triggered when a session ends, determined by a drop in participant count to less than two. |
| `transcription.started` | Triggered when a transcription starts. |
| `transcription.finished` | Triggered when a transcription is processed successfully. |
| `transcription.failed` | Triggered when a transcription fails to process. |
| `recording.finished` | Triggered when a cloud recording is completed and uploaded successfully. |

## Webhook Event Structure

Webhook events are delivered as JSON payloads in HTTP requests to the configured endpoint. Below are the common properties of all webhook events:

| Property | Description |
|------------|-------------|
| `id` | Unique identifier for the event. |
| `apiVersion` | Version of the Whereby API that generated the event. |
| `createdAt` | Timestamp (ISO format) when the event was created. |
| `type` | Event type identifier, such as `room.client.joined`. |
| `data` | Object containing additional event-related information. |

### Example Webhook Event
```json
{
  "id": "d7c4df48b85318352b47d2df45872bf9be87595af379e2a8ad8f1ad28b2a482e",
  "apiVersion": "1.0",
  "createdAt": "2021-01-21T16:29:59.681Z",
  "type": "room.client.joined",
  "data": {
    "roleName": "host",
    "displayName": "Joe Bloggs",
    "meetingId": "134",
    "roomName": "/af0b7b66-c738-4981-887a-ad416754f32d",
    "numClients": 8,
    "numClientsByRoleName": {
      "host": 1,
      "visitor": 7
    },
    "metadata": "<custom-metadata>",
    "externalId": "<custom-id>"
  }
}
```

## Validating Webhook Events

To ensure security and prevent man-in-the-middle attacks, Whereby includes a `Whereby-Signature` header in webhook requests. This header contains a timestamp and a signature generated using a secret key known only to you.

### Signature Format
```
Whereby-Signature:
t=1606227791,v1=94a23dc9d73e8e6abdf9d4095aee954697e9317e9649e742361b35707edd45a3
```

### Validating the Signature in Node.js
```javascript
import crypto from "crypto";

const WEBHOOK_SECRET = "<WEBHOOK_SECRET>";
const MAX_ELAPSED_TIME = 1000 * 60; // 1 minute in milliseconds

function isWebhookEventValid({ body, headers }) {
  const wbSignature = headers["whereby-signature"];
  const matches = wbSignature.matchAll(/t=(.*),v1=(.*)/gm);
  
  let timestamp, signature;
  for (const match of matches) {
    timestamp = match[1];
    signature = match[2];
  }

  const current = new Date();
  const diffTime = current - parseInt(timestamp * 1000, 10);

  const signedPayload = timestamp + "." + JSON.stringify(body);

  const sha256Hasher = crypto.createHmac("sha256", WEBHOOK_SECRET);
  const hash = sha256Hasher.update(signedPayload);
  const hashStr = hash.digest("hex");

  return (
    crypto.timingSafeEqual(
      Buffer.from(hashStr, "utf-8"),
      Buffer.from(signature, "utf-8")
    ) && diffTime < MAX_ELAPSED_TIME
  );
}
```

## Testing Webhooks

You can use webhook testing tools to inspect, debug, and validate webhook events before integrating them into your system.

### Using Beeceptor
[Beeceptor](https://beeceptor.com/) allows you to create a temporary endpoint to receive and inspect webhook events without deploying a backend. Simply create an endpoint and configure it in the Whereby dashboard.

### Alternative: Request Inspector
[Request Inspector](https://requestinspector.com/) provides similar capabilities, allowing you to inspect and debug incoming webhooks with detailed logs.

## Webhook Retries

If a webhook delivery fails with a `5xx` HTTP response, Whereby retries the request up to two times with an exponential backoff. A webhook request also times out if it takes longer than 5 seconds, triggering a retry.

---
This enhanced guide makes it easier for developers to integrate and test Whereby webhooks while ensuring security and reliability.
