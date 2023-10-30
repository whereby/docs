---
description: >-
  Create or delete Whereby rooms programmatically using a simple API request.
  Rooms can be created on demand, or ahead of time for scheduled meetings.
---

# Programmatically with REST API

{% embed url="https://youtu.be/3-uORdwjlBc" %}

{% hint style="info" %}
To use the API, you’ll need to create an API key. A new key is generated from the “Configure” section in the Embedded dashboard. Your API key is secret and should only be used from your server.
{% endhint %}

### Creating rooms

Once you have secured your API key, you can create a room by sending an HTTP request with the necessary properties in the body. Available properties and formats can be found in the [API reference](../../../reference/whereby-rest-api-reference.md). Some features like the URL pattern of the room name and room size (`roomMode`) can only be set during the meetings creation.

`endDate` is interpreted as UTC by default, but other time zones are supported by including an offset in hours and minutes. For example, Eastern Standard Time (EST) would be expressed as `2099-08-11T07:56:01-05:00`.

Rooms are fully functional from the time they are created. They can then be used alongside [our SDK to embed](broken-reference) on your platform

{% tabs %}
{% tab title="cURL" %}
```bash
curl https://api.whereby.dev/v1/meetings \
  --header "Authorization: Bearer $YOUR_API_KEY" \
  --header "Content-Type: application/json" \
  --request POST \
  --data @- << EOF
{
  "endDate": "2099-02-18T14:23:00.000Z",
  "fields": ["hostRoomUrl"]
}
EOF
```
{% endtab %}

{% tab title="PHP" %}
```php
$api_key = "YOUR_API_KEY";
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, 'https://api.whereby.dev/v1/meetings');
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, '{
  "endDate": "2099-02-18T14:23:00.000Z",
  "fields": ["hostRoomUrl"]}'
);

$headers = [
  'Authorization: Bearer ' . $api_key,
  'Content-Type: application/json'
];

curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
$response = curl_exec($ch);
$httpcode = curl_getinfo($ch, CURLINFO_HTTP_CODE);

curl_close($ch);

echo "Status code: $httpcode\n";
$data = json_decode($response);
echo "Room URL: ", $data->{'roomUrl'}, "\n";
echo "Host room URL: ", $data->{'hostRoomUrl'}, "\n";
```
{% endtab %}

{% tab title="Node" %}
```javascript
const fetch = require("cross-fetch");

const API_KEY = "YOUR_API_KEY";

const data = {
  endDate: "2099-02-18T14:23:00.000Z",
  fields: ["hostRoomUrl"],
};

function getResponse() {
    return fetch("https://api.whereby.dev/v1/meetings", {
        method: "POST",
        headers: {
            Authorization: `Bearer ${API_KEY}`,
            "Content-Type": "application/json",
        },
        body: JSON.stringify(data),
    });
}

getResponse().then(async res => {
    console.log("Status code:", res.status);
    const data = await res.json();
    console.log("Room URL:", data.roomUrl);
    console.log("Host room URL:", data.hostRoomUrl);
});

```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json

API_KEY = "YOUR_API_KEY"

data = {
    "endDate": "2099-02-18T14:23:00.000Z",
    "fields": ["hostRoomUrl"],
}

headers = {
    "Authorization": f"Bearer {API_KEY}",
    "Content-Type": "application/json",
}

response = requests.post(
    "https://api.whereby.dev/v1/meetings",
    headers=headers,
    json=data
)

print("Status code:", response.status_code)
data = json.loads(response.text)
print("Room URL:", data["roomUrl"])
print("Host room URL:", data["hostRoomUrl"])
```
{% endtab %}

{% tab title="Java" %}
```java
import com.fasterxml.jackson.databind.ObjectMapper;

import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse.BodyHandlers;
import java.util.Collections;
import java.util.Map;

var apiKey = "YOUR_API_KEY";
var data = Map.of(
        "endDate", "2099-02-18T14:23:00.000Z",
        "fields", Collections.singletonList("hostRoomUrl")
);
var request = HttpRequest.newBuilder(
                URI.create("https://api.whereby.dev/v1/meetings"))
        .header("Authorization", "Bearer " + apiKey)
        .header("Content-Type", "application/json")
        .POST(HttpRequest.BodyPublishers.ofString(new ObjectMapper().writeValueAsString(data)))
        .build();

var response = HttpClient.newHttpClient().send(request, BodyHandlers.ofString());
System.out.println("Status code: " + response.statusCode());
System.out.println("Body: " + response.body());

```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="201 Response: Success" %}
```json
{
    "meetingId": "1",
    "startDate": "2022-02-17T14:24:00.000Z",
    "endDate": "2099-02-18T14:23:00.000Z",
    "roomUrl": "https://example.whereby.com/room",
    "hostRoomUrl": "https://example.whereby.com/room?roomKey=eFhcG...i00ZG"
} 
```
{% endtab %}
{% endtabs %}

### Deleting rooms and endDate

Creating rooms via the API, produces a room with a unique URL and a limited lifespan. There's no need to worry about meetings conflicting or rooms being used for other purposes after the intended session.

The `endDate` property is used to indicate the time at which the room will be marked for deactivation. It **does not** indicate when a meeting will end and remove participants.&#x20;

One hour after the `endDate` things like [Webhook](../../../reference/webhooks.md) events, [host](../../user-roles-and-privileges.md) privileges, new cloud recordings, and minutes consumption will no longer function. The room will then automatically be deleted within 24 hours of the `endDate` provided.

{% hint style="success" %}
If you'd like to limit the length of a meeting and verify a room is no longer being used, you can delete a room [via API request](../../../reference/whereby-rest-api-reference.md#meetings-meetingid-1). A deletion request will remove all participants and prevent any further use. You can also keep track of when a session start via [webhooks](../../../reference/webhooks.md) to limit a meeting by length.
{% endhint %}
