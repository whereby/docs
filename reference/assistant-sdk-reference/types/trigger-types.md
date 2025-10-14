# Trigger Types

## WherebyWebhookTriggers: <mark style="color:green;">\<Object></mark>

<table><thead><tr><th width="390.96484375">Property</th><th width="173.77734375">Default</th><th>Description</th></tr></thead><tbody><tr><td><p><code>"room.client.joined": (</code></p><p> <code>webhookData:</code> <a href="trigger-types.md#wherebywebhookroomclientjoined-less-than-object-greater-than"><code>WherebyWebhookRoomClientJoined</code></a>   <code>) => Promise&#x3C;boolean> | boolean</code></p></td><td><code>() => false</code>  </td><td>Custom matcher on incoming <a href="../../../meeting-content-and-quality/insights-suite-and-api/webhooks.md#in-room-data-properties">"room.client.joined" webhook events</a>. Return <code>true</code> if a match should be triggered or `false`</td></tr><tr><td><p><code>"room.client.left": (</code></p><p><code>webhookData:</code> <a href="trigger-types.md#wherebywebhookroomclientleft-less-than-object-greater-than"><code>WherebyWebhookRoomClientLeft</code></a>   <code>) => Promise&#x3C;boolean> | boolean</code></p></td><td><code>() => false</code>  </td><td>Custom matcher on incoming <a href="../../../meeting-content-and-quality/insights-suite-and-api/webhooks.md#in-room-data-properties">"room.client.left" webhook events</a>. Return <code>true</code> if a match should be triggered or `false`</td></tr><tr><td><p><code>"room.session.started": (</code></p><p><code>webhookData:</code> <a href="trigger-types.md#wherebywebhookroomsessionstarted-less-than-object-greater-than"><code>WherebyWebhookRoomSessionStarted</code></a>   <code>) => Promise&#x3C;boolean> | boolean</code></p></td><td><code>() => false</code>  </td><td>Custom matcher on incoming <a href="../../../meeting-content-and-quality/insights-suite-and-api/webhooks.md#event-objects">"room.session.started" webhook events.</a> Return <code>true</code> if a match should be triggered or `false`</td></tr><tr><td><p><code>"room.session.ended": (</code></p><p><code>webhookData:</code> <a href="trigger-types.md#wherebywebhookroomsessionended-less-than-object-greater-than"><code>WherebyWebhookRoomSessionEnded</code></a>   <code>) => Promise&#x3C;boolean> | boolean</code></p></td><td><code>() => false</code>  </td><td>Custom matcher on incoming <a href="../../../meeting-content-and-quality/insights-suite-and-api/webhooks.md#event-objects">"room.session.ended" webhook events.</a> Return <code>true</code> if a match should be triggered or `false`</td></tr></tbody></table>

## WherebyWebhookBase: <mark style="color:green;">\<Object></mark>

| Property            | Description                    |
| ------------------- | ------------------------------ |
| `type: WebhookType` | Type of webhook                |
| `apiVersion: 1.0`   | API version                    |
| `id: string`        | Webhook ID                     |
| `createdAt: string` | Time the webhook was generated |

## WherebyWebhookInRoom: <mark style="color:green;">\<Object></mark>

<table><thead><tr><th width="300.578125">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>meetingId?: string</code></td><td>Meeting identifier from the meeting that the webhook was fired from</td></tr><tr><td><code>roomName: string</code></td><td>Name of the room from which the webhook was fired from</td></tr><tr><td><code>roomSessionId: string | null</code></td><td>Session ID of the meeting that the webhook was fired from (if any)</td></tr><tr><td><code>subdoman: string</code></td><td>The subdomain identifier for your organization</td></tr></tbody></table>

## WherebyWebhookDataClient: <mark style="color:green;">\<Object></mark>

<table><thead><tr><th width="302.49609375">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>displayName: string</code></td><td>Display name of the participant</td></tr><tr><td><code>participantId: string</code></td><td>Unique identifier for the participant</td></tr><tr><td><code>metadata: string | null</code></td><td>Custom metadata attached to the webhook</td></tr><tr><td><code>externalId: string |  null</code></td><td>Custom identifier attached to the webhook</td></tr></tbody></table>

## WherebyWebhookDataClientJoinLeave: <mark style="color:green;">\<Object></mark>

<table><thead><tr><th width="303.4140625">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>roleName:</code> <a href="trigger-types.md#wherebyrolename-less-than-string-greater-than"><code>WherebyRoleName</code></a></td><td>Role name of the client</td></tr><tr><td><code>numClients: number</code></td><td>Number of clients</td></tr><tr><td><code>numClientsByRoleName: Record&#x3C;</code><a href="trigger-types.md#wherebyrolename-less-than-string-greater-than"><code>WherebyRoleName</code></a><code>, number></code></td><td>Number of clients grouped by role name</td></tr></tbody></table>

## WherebyWebhookRoomClientJoined: <mark style="color:green;">\<Object></mark>

Extends [WherebyWebhookBase](trigger-types.md#wherebywebhookbase-less-than-object-greater-than)

| Property                                                                                                                                                                                                                                                                                                                                    | Description           |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| `data:` [`WherebyWebhookInRoom`](trigger-types.md#wherebywebhookinroom-less-than-object-greater-than) `&` [`WherebyWebhookDataClientJoinLeave`](trigger-types.md#wherebywebhookdataclientjoinleave-less-than-object-greater-than) `&` [`WherebyWebhookDataClient`](trigger-types.md#wherebywebhookdataclient-less-than-object-greater-than) | Payload of this event |

## WherebyWebhookRoomClientLeft: <mark style="color:green;">\<Object></mark>

Extends [WherebyWebhookBase](trigger-types.md#wherebywebhookbase-less-than-object-greater-than)

| Property                                                                                                                                                                                                                                                                                                                                    | Description           |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| `data:` [`WherebyWebhookInRoom`](trigger-types.md#wherebywebhookinroom-less-than-object-greater-than) `&` [`WherebyWebhookDataClientJoinLeave`](trigger-types.md#wherebywebhookdataclientjoinleave-less-than-object-greater-than) `&` [`WherebyWebhookDataClient`](trigger-types.md#wherebywebhookdataclient-less-than-object-greater-than) | Payload of this event |

## WherebyWebhookRoomSessionStarted: <mark style="color:green;">\<Object></mark>

Extends [WherebyWebhookBase](trigger-types.md#wherebywebhookbase-less-than-object-greater-than)

| Property                                                                                              | Description           |
| ----------------------------------------------------------------------------------------------------- | --------------------- |
| `data:` [`WherebyWebhookInRoom`](trigger-types.md#wherebywebhookinroom-less-than-object-greater-than) | Payload of this event |

## WherebyWebhookRoomSessionEnded: <mark style="color:green;">\<Object></mark>

Extends [WherebyWebhookBase](trigger-types.md#wherebywebhookbase-less-than-object-greater-than)

| Property                                                                                               | Description           |
| ------------------------------------------------------------------------------------------------------ | --------------------- |
| `data:` [`WherebyWebhookInRoom`](trigger-types.md#wherebywebhookinroom-less-than-object-greater-than)  | Payload of this event |

## WherebyRoleName: <mark style="color:green;">\<string></mark>

| Type                |
| ------------------- |
| "`owner"`           |
| `"member"`          |
| `"host"`            |
| `"granted_visitor"` |
| `"viewer"`          |
| `"granted_viewer"`  |
| `"recorder"`        |
| `"streamer"`        |
| `"captioner"`       |
| `"assistant"`       |
