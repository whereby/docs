---
description: >-
  Some key properties of your rooms should be set when the room is created using
  the API or Create a room flow.
---

# Customizing rooms during creation

Some features of the meeting rooms can be set at the time of creation, within API requests. This includes (but isn't limited to):

* **Lock State** (`isLocked`) - This set the initial lock status of the rooms to create a type of waiting room for your participants. [Hosts](../user-roles-and-privileges.md#hosts) are able to gain access to locked room and are able to adjust the lock status, if the setting is [exposed](../../whereby-for-web-browser/web-component-and-pre-built-ui/configuring-with-attributes.md#locking-off).&#x20;
* **Pattern of the room name** (`roomNamePattern`) and **Prefix** (`roomNamePrefix`) - Adjust the random string used for room names. You can also include a prefix for easier link searching and management within [insights](../insights-suite-and-api/), [webhooks](../insights-suite-and-api/webhooks.md), or your own database.
* **Room size** (`roomMode`) - Adjust the capacity (4 vs 200) and connection type (P2P vs SFU) of your rooms being generated. Specify `normal` for a capacity of 4 and P2P. Or, `group` for a capacity of 200 and SFU. Review our [blog post](https://whereby.com/blog/p2p-vs-sfu-video-calls-which-is-best/) for further clarifications and information.

Have a look at our [API reference](../../reference/whereby-rest-api-reference/) for a complete overview of properties that can be set on meeting creation.

{% hint style="warning" %}
If you're using Whereby in a telehealth use case, to be HIPAA compliant, set `isLocked: true` in your API request and use `roomNamePattern = uuid`&#x20;
{% endhint %}

Further customizations can be done after the room has been created, either by using [URL parameters](../../whereby-for-web-browser/web-component-and-pre-built-ui/configuring-with-attributes.md) when embedding the room, [managing preferences via the dashboard](../dashboard-preferences/), or by customizing [branding elements](../branding-elements.md) like background and logo.
