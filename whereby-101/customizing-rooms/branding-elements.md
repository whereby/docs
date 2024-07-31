---
description: >-
  Make your Whereby rooms truly yours by setting a custom logo, background and
  other branding elements. You can do this using the branding tools in the
  Embedded dashboard, or via the API.
---

# Branding elements

{% embed url="https://youtu.be/SZkEOh3v6qA" %}

## Using the Branding tools in your Embedded Dashboard

The branding tools can be found under “Configure” → “Appearance”. Any configuration done here will be applied to all your rooms, unless overridden by individual room settings applied via the [Room Theme API](branding-elements.md#using-the-room-theme-api).

![](<../../.gitbook/assets/branding dashboard.png>)

## Using the Room Theme API

The Room Theme API lets you set up and manage branding settings for individual rooms. This could be used for tailoring the video call experience to your client's own branding, or simply to apply different visual styles for rooms used for different purposes.

Visit the [API reference docs](../../reference/whereby-rest-api-reference/#put-requests) for a closer look at how to use the Room Theme API.&#x20;

Note that branding elements configured via the API will override any "global" branding you may have applied via the Embedded Dashboard.

## Additional Branding Options

There are some supplementary branding options available via our [URL parameters](using-url-parameters.md) that may be useful in customizing your meetings look and feel:

* [`?background=off`](using-url-parameters.md#background-off) - This parameter allows you to hide the meeting room's background, allowing for your app or website's branding to shine through.
* [`?avatarUrl=<url>`](using-url-parameters.md#avatarurl-less-than-url-greater-than) - Set the avatar / profile picture of the participant instead of the automatically assigned name initials.&#x20;
* [`?virtualBackgroundUrl=<url>`](using-url-parameters.md#virtualbackgroundurl-less-than-url-greater-than) - Specify an image url to use as the virtual background for the participant.&#x20;
