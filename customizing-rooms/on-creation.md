---
description: >-
  Some key properties of your rooms should be set when the room is created using
  the API.
---

# On creation

Some features of the meeting rooms can be set at the time of creation, within the API requests. This includes (but isn't limited to):

* **Lock State** (`isLocked`) - This set the initial lock status of the rooms to create a type of waiting room for your participants. [Hosts](../user-roles-and-privileges.md#hosts) are able to gain access to locked room and are able to adjust the lock status, if the setting is [exposed](using-url-parameters.md#locking-off).&#x20;
* **Pattern of the room name** (`roomNamePattern`) and **Prefix** (`roomNamePrefix`) - Adjust the random string used for room names. You can also include a prefix for easier link searching and management within [insights](../monitoring-usage/insights-suite-and-api/), [webhooks](../monitoring-usage/webhooks.md), or your own database.
* **Room size** (roomMode) - Adjust the capacity (4 vs 200) and underlying connection type (P2P vs SFU) of your rooms being generated. Review our [blog post](https://whereby.com/blog/p2p-vs-sfu-video-calls-which-is-best/) for further clarifications and information.

Have a look at our [API reference](../whereby-rest-api-reference/) for a complete overview of properties that can be set on meeting creation.

Further customizations can be done after the room has been created, either by using [URL parameters](using-url-parameters.md) when embedding the room, [managing preferences via the dashboard](dashboard-preferences.md), or by customizing [branding elements](branding-elements.md) like background and logo.
