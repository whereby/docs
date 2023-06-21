---
description: >-
  Local Recording is handled completely locally in the host's browser and
  browser cache.
---

# Local Recording

{% hint style="warning" %}
Our Local Recording feature has not been built specifically for Embedded solutions, and as such is limited in functionality. We recommend that this isn't used in a live production environment without significant internal testing. Recordings are only created and stored locally. Whereby cannot recover any lost or corrupted recordings.
{% endhint %}

### Enabling Local Recording

With the Recording feature you'll need to ensure that several different conditions are met so your room hosts are able to see and use the feature.

1. You're generating and the host is using a [`hostUrl`](../../user-roles-and-privileges.md)
   * Only a room host will be able to Record in the room so you need to provide this type of link to the host.
   * When generating this be sure to note that a `hostUrl` is only valid between the meeting creation and [`endDate`](../../creating-and-deleting-rooms/) that you set during your request. These times are in UTC and not your local time zone, unless a timezone was specified in the request.
2. Your host is using Chrome or Chromium based browser with the [Whereby Chrome Extension](https://chrome.google.com/webstore/detail/whereby/bbpjcfkgapecndkanjcojnldopjlnmjk) installed.
3. You've enabled Local Recording from the “Configure” → “Recording” section of your dashboard and are using `?recording=on` in your room URLs. Or you've set the recording type to <mark style="color:blue;">`local`</mark> during the meeting creation [API request](../../whereby-rest-api-reference.md#create-meeting).
4. Your hosts are selecting **only the tab** your meeting is embedded on. <mark style="color:red;">If users select the entire screen or app window it can cause files to be corrupted and unable to be downloaded.</mark>

### Guides for hosts recording meetings

* [Recording Best Practices](https://whereby.helpscoutdocs.com/article/480-recording-best-practices)
* [Record a Meeting](https://whereby.helpscoutdocs.com/article/479-how-to-record)
* [Download your Recordings](https://whereby.helpscoutdocs.com/article/481-download-your-recording)
* [Share your Recordings with others](https://whereby.helpscoutdocs.com/article/592-how-to-share-your-recordings)
