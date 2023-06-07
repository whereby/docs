---
description: >-
  The Insights suite is available from the Embedded dashboard, and is the
  easiest way to access your usage data at a glance. You can also access
  insights data through the Whereby REST API.
---

# Insights suite & API

You can quickly get a clear understanding of what's happening over time and see the big picture, or get a more granular view for data points like:

* 📈 How many Participant, Recording and Streaming minutes have been used
* 👨‍💻 The number of sessions/meetings happening over time
* 👥 How many participants have joined video calls
* :woman\_technologist:Each participant's call quality experience

![Visit the "Insights" section of your Whereby Embedded account to access these reports. ](../../.gitbook/assets/insights\_suite\_1920x1250.webp)

To access the Insights suite, log in to your Whereby Embedded account and visit the "Insights" section.

You can also query the Insights suite data through the [Whereby REST API](../../whereby-rest-api-reference.md), which allows you to programmatically use it for things like billing, operations, or even to create a dashboard of your own.

{% hint style="info" %}
Insights can take 10-15 minutes to update accordingly on the dashboard and via the GET endpoints
{% endhint %}

## Rooms and sessions

On the Rooms page, you can see a list of all the rooms that currently exist and have been created in your account. Your admins can easily filter the list by searching a room name or choosing a date range to find the insights you’re looking for in seconds.

You can also access details of all the sessions that have happened within a specific room and quickly filter them by date for a more detailed view.&#x20;

## Participants

Within a session, you can find the participants that attended the session. Each time your user reconnects to the room, they will receive a new, unique participant ID from us.

