---
description: >-
  Local Recording is handled completely locally in the host's browser and
  browser cache.
---

# Local Recording

{% hint style="warning" %}
We recommend that this isn't used in a production environment without significant internal testing. Recordings are only created and stored locally. Whereby cannot recover any lost or corrupted recordings.
{% endhint %}

### Enabling Local Recording

With the Recording feature you'll need to ensure that several different conditions are met so your room hosts are able to see and use the feature.

1. You're generating and the host is using a [`hostUrl`](../../../whereby-101/user-roles-and-privileges.md)
   * Only a room host will be able to Record in the room so you need to provide this type of link to the host.
   * When generating this be sure to note that a `hostUrl` is only valid between the meeting creation and [`endDate`](../../../whereby-101/using-the-rest-api/#enddate-and-deleting-rooms) that you set during your request. These times are in UTC and not your local time zone, unless a timezone was specified in the request.
2. You've enabled Local Recording from the “Configure” → “Recording” section of your dashboard and are using `?recording=on` in your room URLs. \
   \
   Or, you've set the recording type to <mark style="color:blue;">`local`</mark> during the meeting creation [API request](../../../whereby-101/using-the-rest-api/#creating-rooms).



### Information for hosts recording meetings

{% tabs %}
{% tab title="Best Practices" %}
* Local Recording is only available in Chrome or Chromium browsers on a desktop or laptop. It is not available on mobile devices.
* When recording a meeting while using the [YouTube integration](../../../whereby-101/customizing-rooms/using-url-parameters.md#roomintegrations), any audio from the YouTube video will not be captured in the recording. If you need to capture audio from the video you're viewing in the local recording, we recommend [sharing your screen](../../../end-user/screen-sharing.md#screen-sharing-with-audio) with audio.
* Whereby recordings are stored locally in your browser until they are downloaded onto your device. We recommend clearing your browser's site data, cookies, and cache prior to making a recording, to help ensure that your browser does not run out of space.&#x20;
* Files are initially downloaded as .webm files, which can be used in many platforms like Slack, YouTube, Chrome, and more. However you can use free programs like [VLC](https://www.videolan.org/vlc/index.html) or [Handbrake](https://handbrake.fr/) to convert them to a file format you prefer
{% endtab %}

{% tab title="Troubleshooting" %}
#### Record button not appearing:

1. Verify you're accessing the meeting with a hostUrl
2. If you've generated the roomUrl via the dashboard, you must include the parameter of `&recording=on`&#x20;

#### Unable to download recordings

1. Recordings are stored locally in your browser cache, so they will only be available on the device and browser that they were originally recorded on
2. In rare cases Chrome extensions can interfere with the download process. Try temporarily disabling them and then attempt the download again
3. In some cases if your browser ran out of storage while creating the recording, the resulted file in unable to be completed and not able to be downloaded. Prior to your next recording, try clearing your cache, cookies, and site data
{% endtab %}
{% endtabs %}

