---
description: >-
  Whereby Embedded allows your participants to join a video meeting without
  creating accounts or logins. If you want some users to have altered privileges
  in the meeting, you can use different URL types
---

# User roles and privileges

### Hosts

{% embed url="https://www.youtube.com/watch?v=RicAdlE93cc" %}

Users joining via the hostRoomURL become meeting hosts and have the following features available:

* Lock and unlock the room.
* Remove, mute, and spotlight meeting participants.
* Enter locked rooms without knocking.
* Start and stop [recordings](customizing-rooms/recording-with-embedded/), if available.

{% hint style="info" %}
Host privileges are valid immediately on room creation and are disabled one hour after the `endDate`.
{% endhint %}

A hostRoomURL is only available if the `hostRoomURL` is included in the `fields` parameter when [creating the room via the API](creating-and-deleting-rooms/using-the-rest-api.md).

**Example request body:**

```json
{
    "endDate": "2099-03-25T13:30:00.000Z",
    "fields": ["hostRoomUrl"]
}
```

**Response:**

```json
{
    "meetingId": "1234",
    "startDate": "2022-03-09T13:50:00.000Z",
    "endDate": "2099-03-25T13:30:00.000Z",
    "roomUrl": "https://example.whereby.com/room",
    "hostRoomUrl": "https://example.whereby.com/room?roomKey=eFhcG...i00ZG"
}
```

Now that you have both a `roomUrl` and a `hostRoomUrl`, you can ensure that both your guests and hosts get the right privileges by using the corresponding URL when [embedding rooms](embedding-rooms/) in your own webpage or app.

### Participants

Users joining via the standard `roomUrl` will have a more typical meeting experience. They will only have access to the features you've set via [URL parameters](customizing-rooms/using-url-parameters.md) or the [dashboard](customizing-rooms/dashboard-preferences.md), and they will need to knock to gain entry to locked rooms.

A `roomUrl` will be included with any POST [meeting creation](https://whereby.dev/http-api/#/paths/\~1meetings/post) requests.



**Example request body:**

```javascript
{ 
    "endDate": "2099-03-25T13:30:00.000Z", 
}
```

#### Response:

```javascript
{
    "meetingId": "1234",
    "startDate": "2022-03-09T13:50:00.000Z",
    "endDate": "2099-03-25T13:30:00.000Z",
    "roomUrl": "https://example.whereby.com/room",
}
```

### Viewers

In Viewer Mode, participants joining as Viewers have their camera and microphone disabled by default. Viewers can only interact with Hosts and Speakers by using the Chat.

This is a <mark style="color:orange;">**beta**</mark> feature and might change in the future.

{% hint style="success" %}
Meetings created in "[group](creating-and-deleting-rooms/#using-the-rest-api)" mode can support up to 200 hosts or participants. Viewer mode can support an additional 400 viewers for a total of 600 viewers, hosts, and participants.&#x20;
{% endhint %}



**Example request body:**

```json
{
    "endDate": "2099-03-25T13:30:00.000Z",
    "templateType": "viewerMode"
}
```

**Response:**

```json
{
    "meetingId": "1234",
    "startDate": "2022-03-09T13:50:00.000Z",
    "endDate": "2099-03-25T13:30:00.000Z",
    "roomUrl": "https://example.whereby.com/room",
    "hostRoomUrl": "https://example.whereby.com/room?roomKey=eFhcG...i00ZG"
    "viewerRoomUrl": "https://example.whereby.com/room?roomKey=eFhcG...VTQIE"
}
```

[Explore more in the API reference](https://whereby.dev/http-api/).
