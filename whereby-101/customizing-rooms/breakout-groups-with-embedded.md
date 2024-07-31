---
description: >-
  More than ever, workshops, lectures and conferences are happening online. So
  it's never been more important to combine bigger (often one-way) meetings with
  smaller, collaborative sessions.
---

# Breakout Groups with Embedded

{% hint style="warning" %}
Cloud recording does not support our [Breakout Groups](breakout-groups-with-embedded.md) feature. When using Breakout Groups, the recorder will remain in the main room and will not capture any of the meeting content from individual groups.
{% endhint %}

## Breakout Groups

With Breakout Groups in Whereby Embedded you can easily implement collaborative sessions directly into your app or website.

To use Breakout Groups you'll need to ensure the following is true. All of these conditions must be met for Breakout Groups to function properly, so if you're having issues implementing Breakout Groups these are good things to double check:

* The meeting is set to the `group` roomMode during [meeting creation](../../reference/whereby-rest-api-reference/#create-meeting)&#x20;
* You are using the [`?breakout=on`](using-url-parameters.md#breakout-less-than-on-or-off-greater-than) parameter/attribute if you have `?minimal` enabled
* The meeting will happen between the meeting creation and `endDate` that was defined in the API request to create the room
* You are using [Host URLs](../user-roles-and-privileges.md) and will have a Host in the meeting to start the breakout session

You can also programmatically predefine the number of groups and their names before the meeting using our [?groups](using-url-parameters.md#groups-orange-banana-coconut) parameter in the URL or[ Web Component attribute](../../reference/using-the-whereby-embed-element.md#attributes-of-the-component).

After you've implemented, we have a [series of guides](../../end-user/end-user-support-guides/using-breakout-groups.md) you can use to train your hosts on using the Breakout Groups feature.

\
