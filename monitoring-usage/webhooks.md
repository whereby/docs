---
description: >-
  With Webhooks you can set up user-defined callbacks triggered by meeting
  events. Use it to keep track of the number of users joining your meetings, or
  to verify that your hosts are joining on time.
---

# Webhooks

{% embed url="https://youtu.be/GGMcPqXdZ8I" %}

Webhooks are managed from the "Configure" section of your account, so they need to be set up by an admin in your organization.

![Visit the "Configure" section of your Whereby Embedded account dashboard and scroll down to find the Webhooks setup.](../.gitbook/assets/webhooks-dashboard.png)

At the moment the following event types are supported:

* `room.client.joined` - this is sent when a user joins the meeting room
* `room.client.left` - this is send when a user leaves the meeting room (this could be via the leave button or by closing the browser tab
* `room.session.started`: Sent when a room session starts, which is when there are at least 2 users in a room.
* `room.session.ended`: Sent when a room session ends. Currently, a session will end when the number of participants has been less than 2 for some time. This heuristic could change in the future to better determine that a session has ended.

Please note that webhook events are sent for interactions that happen between the creation of the room and an hour after the `endDate` of a room. Also consider that a particular event can be sent more than once, and that you could receive events in non-chronological order.

## Event objects

Events are delivered to their corresponding webhook endpoint in JSON format, as the body of an HTTP request. The table below describes their top-level attributes.

| Property     | Description                                              |
| ------------ | -------------------------------------------------------- |
| `id`         | String that uniquely identifies the event.               |
| `apiVersion` | The Whereby API version used to populate data.           |
| `createdAt`  | ISO representation of the creation date of the event.    |
| `type`       | The event’s type identifier, e.g. `room.client.joined`   |
| `data`       | Object containing information associated with the event. |

## Data properties

Properties in `data` that are common to all events:

| Property    | Description                                                                                               |
| ----------- | --------------------------------------------------------------------------------------------------------- |
| `meetingId` | The identifier of the meeting that the user has joined/left or where the session has started/ended.       |
| `roomName`  | The string that identifies the room assigned to the meeting. It’s the last path parameter of the roomUrl. |



Additional properties in `data` for both `room.client.joined` and `room.client.left`:

| Property                                                                                        | Description                                                                                                                                                       |
| ----------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| roleName                                                                                        | The client’s role depending on what URL they use to access the meeting.                                                                                           |
| numClients                                                                                      | Number of clients connected to the meeting after the event.                                                                                                       |
| numClientsByRoleName                                                                            | Number of clients connected to the meeting after the event, grouped by the roleName.                                                                              |
| [metadata](../customizing-rooms/using-url-parameters.md#metadata-less-than-string-greater-than) | String that matches the "[metadata](../customizing-rooms/using-url-parameters.md#metadata-less-than-string-greater-than)" query parameter passed to the room URL. |



The property `roleName` will have one of the following values:

* `host`: A user joined using the `hostRoomUrl`.
* `visitor`: A user joined using the regular `roomUrl`.
* `granted_visitor`: Same as a `visitor` but can join without knocking if the room is locked.
* `member`: A user with an account in your Embedded organization.
* `owner`: A user with an admin account in your Embedded organization.
* `recorder`: A cloud recording instance has started or stopped.

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
        "meetingId": "134",
        "roomName": "/af0b7b66-c738-4981-887a-ad416754f32d",
        "numClients": 8,
        "numClientsByRoleName": {
          "host": 1,
          "visitor": 7
        },
        "metadata": "<custom-metadata>"
    }
}
```
{% endtab %}
{% endtabs %}

## Validating events

To prevent from [man-in-the-middle attacks ↗](https://en.wikipedia.org/wiki/Man-in-the-middle\_attack) , webhook requests to your endpoint contain a signature in the `Whereby-Signature` header. This string is generated with a unique secret that only you can view when creating or editing a webhook in the Embedded dashboard. Only Whereby and you have access to this secret, and no third party can send forged events to your endpoint. On top of that the header also includes a timestamp to help you prevent replay attacks. The header is composed of a timestamp and the signature itself, for example:

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
