---
description: >-
  It can be helpful to have an understanding of how much usage your rooms are
  experiencing. Get the essentials with Insights Dashboard, or set up custom
  tracking using our webhooks or other mechanisms.
---

# Monitoring usage

### Insights suite & API

The Insights suite is available from the Embedded dashboard, and is the easiest way to access your usage data at a glance. You can also access insights data through the [Whereby REST API](../whereby-rest-api-reference.md#get-meetings). This way, you can discover trends and report on customer usage easier.

[Read more](insights-suite-and-api/)

### Webhooks

With Webhooks you can set up user-defined callbacks that will be triggered by meeting events. A very common way that this is used is to keep track of the number of users that are joining your meetings, or to verify that your hosts are joining on time.

[Read more](webhooks.md)

### Name prefixes

When [creating meetings](../whereby-rest-api-reference.md#create-meeting) using the REST API, you can send an extra identifier as part of your API request (`roomNamePrefix`).This identifier will become part of the URL for the room, which you can then use for generating your own usage metrics.

[Read more](name-prefixes.md)
