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

{% hint style="info" %}
Note that any upper-case letters in the roomNamePrefix will be transformed to lower-case, and any whitespace will be stripped away.
{% endhint %}

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

### Searching insights for rooms with a prefix

After you've applied `roomNamePrefix` to your URLs, you can then use our [API](../../reference/whereby-rest-api-reference/#meetings-2) to search for rooms containing that prefix.

Example GET request URL:

```
https://api.whereby.dev/v1/meetings?roomName[startsWith]=/customer
```

Response

```json

{
    "startDate": "2024-02-08T16:57:01.017Z",
    "endDate": "2024-02-10T05:00:00.000Z",
    "roomName": "/customer123-172aa26e-802a-41a6-ba1f-142792c4f42",
    "roomUrl": "https://nickrolls.whereby.com/customer123-172aa26e-802a-41a6-ba1f-142792c4f42",
    "meetingId": "82490894"
},
{
    "startDate": "2024-02-08T16:57:18.847Z",
    "endDate": "2024-02-10T05:00:00.000Z",
    "roomName": "/customer456-b621d167-2cb1-4bb8-8aa4-46f8a58f0069",
    "roomUrl": "https://nickrolls.whereby.com/customer456-b621d167-2cb1-4bb8-8aa4-46f8a58f0069",
    "meetingId": "82490920"
}
```

Read more about [creating rooms using the API](./), and refer to our [API reference](../../reference/whereby-rest-api-reference/) for further details.
