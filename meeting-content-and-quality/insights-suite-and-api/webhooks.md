---
description: >-
  With Webhooks you can set up user-defined callbacks triggered by meeting
  events. These are a great way to programmatically keep track of some of the
  events and actions happening around your meetings.
---

# Tracking room events with Webhooks

## Configuration

Webhooks are managed from the "Configure" section of your account, so they need to be set up by an admin in your organization.

![Visit the "Configure" section of your Whereby Embedded account dashboard and scroll down to find the Webhooks setup.](../../.gitbook/assets/webhooks-dashboard.png)

At the moment the following event types are supported:

* `room.client.joined` - this is sent when a user joins the meeting room
* `room.client.left` - this is sent when a user leaves the meeting room (this could be via the leave button or by closing the browser tab)
* `room.client.knocked`- this is sent when a visitor knocks the meeting room from the waiting room
* `room.client.knockCancelled`- this is sent when a visitor cancels their knock from the waiting room (this could be via the cancel button or a result of network issues)
* `room.session.started`: Sent when a room session starts, which is when there are at least 2 users in a room.
* `room.session.ended`: Sent when a room session ends. Currently, a session will end when the number of participants has been less than 2 for some time. This heuristic could change in the future to better determine that a session has ended.
* `transcription.started`: Sent when a transcription has started.
* `transcription.finished`: Sent when a transcription has finished processing.
* `transcription.failed`: Sent when a transcription has failed to process.
* `recording.finished`: Sent when a cloud recording has finished and the recording has uploaded successfully.

## Event objects

Events are delivered to their corresponding webhook endpoint in JSON format, as the body of an HTTP request. The table below describes their top-level attributes.

| Property     | Description                                                                                                                                                                  |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`         | String that uniquely identifies the event.                                                                                                                                   |
| `apiVersion` | The Whereby API version used to populate data. Refer to the details on [Versioning](../../reference/whereby-rest-api-reference/#versioning) for how webhook data may change. |
| `createdAt`  | ISO representation of the creation date of the event.                                                                                                                        |
| `type`       | The event’s type identifier, e.g. `room.client.joined`                                                                                                                       |
| `data`       | Object containing information associated with the event.                                                                                                                     |

## In-Room Data properties

Please note that in-room webhook events are sent for interactions that happen between the creation of the room and an hour after the `endDate` of a room. Also consider that a particular event can be sent more than once, and that you could receive events in non-chronological order.

Properties in `data` that are common to all in-room webhook events:

| Property    | Description                                                                                                 |
| ----------- | ----------------------------------------------------------------------------------------------------------- |
| `meetingId` | The identifier of the meeting that the user has joined/left or where the session has started/ended.         |
| `roomName`  | The string that identifies the room assigned to the meeting. It’s the last path parameter of the `roomUrl`. |

Additional properties in `data` for `room.client.joined` , `room.client.left room.client.knocked` and `room.client.knockCancelled`:

<table><thead><tr><th width="374">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>displayName</code></td><td>The visible name displayed to others in the meeting.</td></tr><tr><td><code>roomSessionId</code></td><td>The <code>roomSessionId</code> for the meeting.</td></tr><tr><td><code>participantId</code></td><td>The current user's <code>participantId</code>. Can be used for insights data.</td></tr><tr><td><a href="../../whereby-101/customizing-rooms/using-url-parameters.md#metadata-less-than-string-greater-than"><code>metadata</code></a></td><td>String that matches the "<a href="../../whereby-101/customizing-rooms/using-url-parameters.md#metadata-less-than-string-greater-than">metadata</a>" query parameter passed to the room URL.</td></tr><tr><td><a href="../../whereby-101/customizing-rooms/using-url-parameters.md#externalid-less-than-id-greater-than"><code>externalId</code></a></td><td>String that matches the "<a href="../../whereby-101/customizing-rooms/using-url-parameters.md#externalid-less-than-id-greater-than">externalId</a>" query parameter passed to the room URL.</td></tr></tbody></table>

Additional properties in data for just `room.client.joined` and `room.client.left`:

| Property               | Description                                                                                                                                                                 |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `roleName`             | The client’s role depending on what URL they use to access the meeting.                                                                                                     |
| `numClients`           | Number of clients connected to the meeting after the event.                                                                                                                 |
| `numClientsByRoleName` | Number of clients connected to the meeting after the event, grouped by the `roleName`.                                                                                      |
| `isDialIn`             | `true`when a client has called into the room from a telephone with our [dial-in](https://docs.whereby.com/whereby-101/customizing-rooms/dial-in) feature, `false`otherwise. |

The property `roleName` will have one of the following values:

* `owner`: A user with an admin account in your Embedded organization.
* `member`: A user with an account in your Embedded organization.
* `host`: A user joined using the `hostRoomUrl`.
* `visitor`: A user joined using the regular `roomUrl`.
* `granted_visitor`: The `roleName` that is assigned to a Participant after they have knocked and been let in by a Host.
* `viewer`: A user joined using the `viewerRoomUrl`.
* `granted_viewer`: The `roleName` that is assigned to a Participant if they are queued before a Host joins.
* `recorder`: A cloud recording instance has started or stopped.
* `streamer`: A streaming instance has started or stopped.
* `captioner`: The meeting has been captioned or transcribed.

An example of a webhook event object:

{% tabs %}
{% tab title="JSON" %}
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
{% endtab %}
{% endtabs %}

## Transcription Data properties

Properties in `data` that are common to all transcription webhook events:

| Property          | Description                                                                                                                                                                                                                                        |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `transcriptionId` | The identifier of the transcription that has finished processing or failed to process.                                                                                                                                                             |
| `type`            | The type of transcription, `LIVE_TRANSCRIPTION` for Session Transcription or `RECORDING_TRANSCRIPTION` for recording based transcription                                                                                                           |
| status            | The status of the transcription. Starts in `in_progress` and transitions to `ready` once the transcript has been successfully written to storage. If any errors occurred in the processing of the transcription then this will be set to `failed`. |

Additional properties in data for just `transcription.finished`:

| Property            | Description                                                                                                                                                                         |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `filename`          | The file name and extension of the transcript.                                                                                                                                      |
| `storageType`       | The storage type of the transcription, `WHEREBY_HOSTED` for transcript files that are stored with Whereby or `SELF_HOSTED` for transcript files that were written to other storage. |
| `durationInSeconds` | The total billable time used for the transcription in seconds.                                                                                                                      |
| `recordingId`       | The identifier of the recording that the transcription is associated with. Only set if `type` is `RECORDING_TRANSCRIPTION`.                                                         |

Additional properties in data for just `transcription.failed`:

| Property | Description                                                               |
| -------- | ------------------------------------------------------------------------- |
| `error`  | The error message that describes why the transcription failed to process. |

## Cloud Recording Data properties

Properties in `data` for the `recording.finished` webhook event:

| Property      | Description                                                            |
| ------------- | ---------------------------------------------------------------------- |
| `filename`    | The file name and extension of the recording.                          |
| `recordingId` | The identifier of the recording.                                       |
| `roomName`    | The string that identifies the room assigned to the meeting.           |
| `status`      | The final status of the recording. It can be only `completed` for now. |

## Validating events

To prevent from [man-in-the-middle attacks ↗](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) , webhook requests to your endpoint contain a signature in the `Whereby-Signature` header. This string is generated with a unique secret that only you can view when creating or editing a webhook in the Embedded dashboard. Only Whereby and you have access to this secret, and no third party can send forged events to your endpoint. On top of that the header also includes a timestamp to help you prevent replay attacks. The header is composed of a timestamp and the signature itself, for example:

{% tabs %}
{% tab title="Text" %}
```
Whereby-Signature:
t=1606227791,v1=94a23dc9d73e8e6abdf9d4095aee954697e9317e9649e742361b35707edd45a3

```
{% endtab %}
{% endtabs %}

To verify an event’s signature, follow these steps:

1. Extract the timestamp and signature by splitting the string on `,` then removing both `t=` and `v1=` from the resulting strings.
2. Prepare the `signedPayload` string by concatenating the timestamp (as a string), the character `.` and the JSON event object (the request body).
3. Calculate the HMAC: It is the SHA256 hash of `signedPayload`, using the endpoint’s signing secret (the one you get when creating the webhook) as the key.
4. Compare the signature from the header to the one you just generated. To protect yourself from timing attacks consider using a constant-time equality function instead of the default equality operator of the language you’re using. Finally, to prevent replay attacks, compare the header’s timestamp with the current one and decide if the elapsed time is within your allowed threshold.

An example of a webhook event validation:

{% tabs %}
{% tab title="Node" %}
```javascript
import crypto from "crypto";

const WEBHOOK_SECRET = "<WEBHOOK_SECRET>";
const MAX_ELAPSED_TIME = 1000 * 60; // 1 minutes in milliseconds

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
    ) && diffTime < Whereby.MAX_ELAPSED_TIME
  );
}
```
{% endtab %}
{% endtabs %}

## Failed delivery

Any `5xx` response to the webhook delivery request will trigger a retry, for a total of 2 retries. A short exponential backoff will be used. We also have a 5 seconds timeout on webhook requests, if the response takes longer than that the same retry mechanism will be used.
