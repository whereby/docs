---
description: >-
  More than ever, workshops, lectures and conferences are happening online. So
  it's never been more important to combine bigger (often one-way) meetings with
  smaller, collaborative sessions.
---

# Breakout Groups with Embedded

## Implementing Breakout Groups

With Breakout Groups in Whereby Embedded you can easily implement collaborative sessions directly into your app or website.

To use Breakout Groups you'll need to ensure the following is true. All of these conditions must be met for Breakout Groups to function properly, so if you're having issues implementing Breakout Groups these are good things to double check:

* The meeting is set to the `group` roomMode during [meeting creation](../creating-and-deleting-rooms/)&#x20;
* You have the top toolbar enabled. Normally this is enabled by default, but if you're using the [`?minimal`](../customizing-rooms/using-url-parameters.md#minimal) parameter it will need to be manually re-enabled with [`?topToolbar=on`](../customizing-rooms/using-url-parameters.md#toptoolbar-less-than-on-or-off-greater-than)``
* You are using the [`?breakout=on`](../customizing-rooms/using-url-parameters.md#breakout-less-than-on-or-off-greater-than) parameter
* The meeting will happen between the meeting creation and `endDate` that was defined in the API request to create the room
* You are using [Host URLs](../user-roles-and-privileges.md) and will have a Host in the meeting

## Using Breakout Groups

Once you have Breakout Groups implemented, it will be important that Hosts know how to use them. For this, we have a series of Whereby Meetings support guides that all Hosts should review and they can get started [here](https://whereby.helpscoutdocs.com/article/612-breakout-groups).

\




\
