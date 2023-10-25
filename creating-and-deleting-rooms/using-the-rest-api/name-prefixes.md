---
description: >-
  When creating rooms using the REST API, you can send an extra identifier as
  part of your API request. This identifier will become part of the room URL,
  acting as a tag to enable further analysis.
---

# Name prefixes

Rooms created using the API get a unique name/URL based on your Whereby domain and a randomly generated ID.&#x20;

Example URL:

`https://mydomain.whereby.com/8d9c4219-483e-44fe-c826-c70ce1439018`

If for some reason you need to attach your own identifier to the room URL, you can do so by using the `roomNamePrefix` parameter in the body of the POST request to include a string which will then be used as a room name prefix.

The string can be up to 39 characters long, and can contain alphanumerical characters.

Example request body:

```json
{
    "roomNamePrefix": "Customer0348-",
    "endDate": "2099-03-25T13:30:00.000Z",
    "fields": []
}
```

Response:

```json
{
    "startDate": "2022-03-11T13:56:20.593Z",
    "endDate": "2099-03-25T13:30:00.000Z"",
    "roomUrl": "https://mydomain.whereby.com/customer0348-72b919c2-17dc-4007-ac7b-56f38b8e29ba",
    "meetingId": "52755922"
}
```

{% hint style="info" %}
Note that any upper-case letters in the roomNamePrefix will be transformed to lower-case, and any whitespace will be stripped away.
{% endhint %}

Read more about [creating rooms using the API](./), and refer to our [API reference](../../whereby-rest-api-reference/) for further details.
