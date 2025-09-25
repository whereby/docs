# Trigger Types

## WebhookType:<<mark style="color:$success;">string</mark>>

| Value                  | Description                                                                                                                                                                                                             |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `room.client.joined`   | This is sent when a user joins the meeting room                                                                                                                                                                         |
| `room.client.left`     | This is sent when a user leaves the meeting room (this could be via the leave button or by closing the browser tab)                                                                                                     |
| `room.session.started` | Sent when a room session starts, which is when there are at least 2 users in a room.                                                                                                                                    |
| `room.session.ended`   | Sent when a room session ends. Currently, a session will end when the number of participants has been less than 2 for some time. This heuristic could change in the future to better determine that a session has ended |

## TriggerOptions<<mark style="color:$success;">Object</mark>>

| Property                                                                                | Description                                                      |
| --------------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| `webhookTriggers:` [`WherebyWebhookTriggers`](trigger-types.md#wherebywebhooktriggers)  | Webhook trigger that should be listened for to trigger room join |
| `port?: number`                                                                         | The port that this service should run on - default is `8080`     |

## TriggerEvents

| Property                                                                                                                           | Description                                          |
| ---------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| `[TRIGGER_EVENT_SUCCESS]: [ { roomUrl: string; triggerWebhook:` [`WherebyWebhookType`](trigger-types.md#wherebywebhooktype)`,  ];` | Emitted when trigger conditions are successfully met |

## WherebyWebhookTriggers

| Property                                                                                                          |                                                                                                                                                                                                                                 |
| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `keyof` [`WherebyWebhookTriggerTypes`](trigger-types.md#wherebywebhooktriggertypes-less-than-object-greater-than) | A function that receives the payload defined for this webhook type (`WherebyWebhookTriggerTypes[Key]`) and returns a `boolean` or `Promise<boolean>`. Returning `true` indicates that the trigger should start an Assistant. \| |

## WherebyWebhookTriggerTypes<<mark style="color:$success;">Object</mark>>

| Property                                                                                                                                        | Description                |
| ----------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------- |
| `"room.client.joined":` [`WherebyWebhookRoomClientJoined`](trigger-types.md#wherebywebhookroomclientjoined-less-than-object-greater-than)       | Room client joined event   |
| `"room.client.left":`[`WherebyWebhookRoomClientLeft`](trigger-types.md#wherebywebhookroomclientleft-less-than-object-greater-than)              | Room client left event     |
| `"room.session.started":` [`WherebyWebhookRoomSessionStarted`](trigger-types.md#wherebywebhookroomsessionstarted-less-than-object-greater-than) | Room session started event |
| `"room.session.ended":` [`WherebyWebhookRoomSessionEnded`](trigger-types.md#wherebywebhookroomsessionended-less-than-object-greater-than)       | Room session ended event   |

## WherebyWebhookBase<<mark style="color:$success;">Object</mark>>

| Property            | Description                     |
| ------------------- | ------------------------------- |
| `type: WebhookType` | Type of webhook                 |
| `apiVersion: 1.0`   | Api version                     |
| `id: string`        | Webhook ID                      |
| `createdAt: string` | Time the webhook was created at |

## WherebyWebhookType

| Type                                                                                                                  |
| --------------------------------------------------------------------------------------------------------------------- |
| [`WherebyWebhookRoomClientJoined`](trigger-types.md#wherebywebhookroomclientjoined-less-than-object-greater-than)     |
| [`WherebyWebhookRoomClientLeft`](trigger-types.md#wherebywebhookroomclientleft-less-than-object-greater-than)         |
| [`WherebyWebhookRoomSessionStarted`](trigger-types.md#wherebywebhookroomsessionstarted-less-than-object-greater-than) |
| [`WherebyWebhookRoomSessionEnded`](trigger-types.md#wherebywebhookroomsessionended-less-than-object-greater-than)     |

## WherebyRoleName<<mark style="color:$success;">string</mark>>

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



## WherebyWebhookInRoom<<mark style="color:$success;">Object</mark>>

| Property                        | Description                                                         |
| ------------------------------- | ------------------------------------------------------------------- |
| `meetingId?: string`            | Meeting identifier from the meeting that the webhook was fired from |
| `roomName: string`              | Name of the room from which the webhook was fired from              |
| `roomSessionId: string \| null` | Session ID of the meeting that the webhook was fired from           |
| `subdoman: string`              | The identifier for your organization                                |

## WherebyWebhookDataClient<<mark style="color:$success;">Object</mark>>

| Property                      | Description                               |
| ----------------------------- | ----------------------------------------- |
| `displayName: string`         | Display name of the participant           |
| `participantId: string`       | Unique identifier for the participant     |
| `metadata: string \| null`    | Custom metadata attached to the webhook   |
| `externalId: string \|  null` | Custom identifier attached to the webhook |

## WherebyWebhookDataClientJoinLeave<<mark style="color:$success;">Object</mark>>

| Property                                                                                                                      | Description                            |
| ----------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- |
| `roleName:` [`WherebyRoleName`](trigger-types.md#wherebyrolename-less-than-string-greater-than)                               | Role name of the client                |
| `numClients: number`                                                                                                          | Number of clients                      |
| `numClientsByRoleName: Record<`[`WherebyRoleName`](trigger-types.md#wherebyrolename-less-than-string-greater-than)`, number>` | Number of clients grouped by role name |

## WherebyWebhookRoomClientJoined<<mark style="color:$success;">Object</mark>>

Extends [WherebyWebhookBase](trigger-types.md#wherebywebhookbase-less-than-object-greater-than)

| Property                                                                                                                                                                                                                                                                                                                                    | Description           |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| `data:` [`WherebyWebhookInRoom`](trigger-types.md#wherebywebhookinroom-less-than-object-greater-than) `&` [`WherebyWebhookDataClientJoinLeave`](trigger-types.md#wherebywebhookdataclientjoinleave-less-than-object-greater-than) `&` [`WherebyWebhookDataClient`](trigger-types.md#wherebywebhookdataclient-less-than-object-greater-than) | Payload of this event |

## WherebyWebhookRoomClientLeft<<mark style="color:$success;">Object</mark>>

Extends [WherebyWebhookBase](trigger-types.md#wherebywebhookbase-less-than-object-greater-than)

| Property                                                                                                                                                                                                                                                                                                                                    | Description           |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| `data:` [`WherebyWebhookInRoom`](trigger-types.md#wherebywebhookinroom-less-than-object-greater-than) `&` [`WherebyWebhookDataClientJoinLeave`](trigger-types.md#wherebywebhookdataclientjoinleave-less-than-object-greater-than) `&` [`WherebyWebhookDataClient`](trigger-types.md#wherebywebhookdataclient-less-than-object-greater-than) | Payload of this event |

## WherebyWebhookRoomSessionStarted<<mark style="color:$success;">Object</mark>>

Extends [WherebyWebhookBase](trigger-types.md#wherebywebhookbase-less-than-object-greater-than)

| Property                                                                                              | Description           |
| ----------------------------------------------------------------------------------------------------- | --------------------- |
| `data:` [`WherebyWebhookInRoom`](trigger-types.md#wherebywebhookinroom-less-than-object-greater-than) | Payload of this event |

## WherebyWebhookRoomSessionEnded<<mark style="color:$success;">Object</mark>>

Extends WherebyWebhookBase

| Property                                                                                               | Description           |
| ------------------------------------------------------------------------------------------------------ | --------------------- |
| `data:` [`WherebyWebhookInRoom`](trigger-types.md#wherebywebhookinroom-less-than-object-greater-than)  | Payload of this event |
