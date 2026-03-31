---
description: >-
  More than ever, workshops, lectures, and conferences are happening online. So
  it's never been more important to combine bigger (often one-way) meetings with
  smaller, collaborative sessions.
---

# Breakout groups

With Breakout Groups, you can easily implement collaborative sessions directly into your app or website.

## Setup

To use Breakout Groups, you'll need to ensure the following is true. **All of these conditions must be met** for Breakout Groups to function properly, so if you're having issues implementing Breakout Groups, these are good things to double-check:

1. The meeting is set to the `group` roomMode during [meeting creation](../reference/whereby-rest-api-reference/meetings.md)&#x20;
2. You are using the [`?breakout=on`](../whereby-for-web-browser/web-component-and-pre-built-ui/configuring-with-attributes.md#breakout-less-than-on-or-off-greater-than) parameter/attribute if you have `?minimal` enabled
3. The meeting will happen between the meeting creation and `endDate` that was defined in the API request to create the room
4. You are using [Host URLs](user-roles-and-privileges.md) and will have a Host in the meeting to start the breakout session

## Additional information

You can also programmatically predefine the number of groups and their names before the meeting using our [?groups](../whereby-for-web-browser/web-component-and-pre-built-ui/configuring-with-attributes.md#groups-orange-banana-coconut) parameter in the URL or[ Web Component attribute](../reference/using-the-whereby-embed-element.md).

After you've implemented, we have a [series of guides](../end-user/end-user-support-guides/using-breakout-groups.md) you can use to train your hosts on using the Breakout Groups feature.

## Known limitations

* Cloud recording does not support our Breakout Groups feature. When using Breakout Groups, the recorder will remain in the main room and will not capture any of the meeting content from individual groups.
