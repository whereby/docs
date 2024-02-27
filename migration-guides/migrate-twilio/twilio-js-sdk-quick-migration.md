# Twilio JS SDK Quick Migration

Welcome to Whereby! We're excited to showcase what it can look and feel like partnering with a team that is _completely_ focused on video. We've spent the last 10 years building our product to be the best the web has to offer for WebRTC calls and we're confident our delightful design, approachable API, and global infrastructure will prove to make you and (maybe more importantly) your customers lives better.

## Introduction

For this migration, you'll be utilizing our [Web Component](https://docs.whereby.com/reference/using-the-whereby-embed-element) to integrate Whereby rooms into your platform. While this is likely different than your existing Twilio setup, it requires **significantly less code and design time** as it leverages Whereby's existing PWA and our UI/UX.

It may not offer the complete customization you'd initially be looking for, but we've collaborated with customers that have made this migration in a matter of days instead of weeks. After you've got a 1.0 launch with Whereby under your belt, you can start considering a more custom integration method utilizing our [React Hooks](https://docs.whereby.com/reference/react-hooks-reference)

### Requirements

1. Install SDK via NPM or CDN&#x20;
2. Create a [Free Whereby account](https://whereby.com/org/signup/embedded?signupFlowPlanType=embedded\_free)
3. Create an API key in the "configure" section of your dashboard
4. Create a room with API or dashboard

#### Installation

When using React or a bundler like Webpack, Rollup, Parcel, etc

```bash
npm install @whereby.com/browser-sdk
```

```javascript
import "@whereby.com/browser-sdk/embed"
```

#### CDN

Or access the code via our CDN and add a `script` tag in your sites head.

{% code overflow="wrap" %}
```markup
<script src="https://cdn.srv.whereby.com/embed/v2-embed.js" type="module"></script>
```
{% endcode %}

#### Usage

{% tabs %}
{% tab title="JSX" %}
```jsx
const MyComponent = ({ roomUrl }) => {
    return <whereby-embed room={roomUrl} />
}

export default MyComponent
```

Create roomUrl's via [POST request to our REST API](../../reference/whereby-rest-api-reference.md#meetings)

You can use a `hostRoomUrl` instead of the `roomUrl`, if you want to give the user host privileges
{% endtab %}

{% tab title="HTML" %}
{% code overflow="wrap" %}
```html
<whereby-embed room="https://subdomain.whereby.com/your_room?roomKey=3fe345a"></whereby-embed>
```
{% endcode %}

Create roomUrl's via <mark style="background-color:orange;">POST request to our REST API</mark>

You can use a `hostRoomUrl` instead of the `roomUrl`, if you want to give the user host privileges.&#x20;
{% endtab %}
{% endtabs %}

### Account and API key creation

1. Sign up for a [Free Whereby Embedded](https://whereby.com/org/signup/embedded?signupFlowPlanType=embedded\_free) account
2.  From the **Configure** section of the dashboard, select **Generate Key**\


    <figure><img src="../../.gitbook/assets/Twilio JS SDK Quick migration screenshot.jpg" alt=""><figcaption></figcaption></figure>

### Create Rooms

Once you have created your API key, you can create a room by sending an HTTP request with the necessary properties in the body. Available properties and formats can be found in the [API reference](../../reference/whereby-rest-api-reference.md). Some features like the URL pattern of the room name and room size (`roomMode`) can only be set during the meetings creation.

`endDate` is the only required property and is interpreted as UTC by default, but other time zones are supported by including an offset in hours and minutes. For example, Eastern Standard Time (EST) would be expressed as `2099-08-11T07:56:01-05:00`.

Rooms are fully functional from the time they are created.

**Request Examples**

{% tabs %}
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

### Customization

After you've installed the Web Component and created your rooms, you can begin customizing the room experience to your liking.&#x20;

When leveraging Whereby's pre-built experience, there are multiple portions of Twilio's SDK that you receive out of the box as part of the rooms being generated:

* [Set up local media](https://www.twilio.com/docs/video/javascript-getting-started#set-up-local-media) -> Built in as part of our pre-call waiting room experience. Users are prompted to provide cam and mic access, and can manage I/O devices as they see fit. Enable pre-call review globally via the features section of your dashboard, or on a per room basis with the [?precallReview](https://docs.whereby.com/whereby-101/customizing-rooms/using-url-parameters#precallreview-less-than-on-or-off-greater-than)\

* [Connect to a room](https://www.twilio.com/docs/video/javascript-getting-started#connect-to-a-room) & [Join a room](https://www.twilio.com/docs/video/javascript-getting-started#join-a-room) -> This logic is handled for you in our PWA and pre-call experience. Based on the lock status of the room, they will either have the option to "join meeting" or "knock". Set `"isLocked": true` in the body of your API request to require your participants to knock. Then provide your [hosts](../../whereby-101/user-roles-and-privileges.md), speakers, doctors, etc with a corresponding `hostRoomUrl` so they can join a locked room to provide others access!
* [Working with remote participants](https://www.twilio.com/docs/video/javascript-getting-started#working-with-remote-participants) events, media, etc -> All participants follow the above mentioned pre-call experience and connect in the rooms via the underlying Whereby `roomUrl`. No rendering, grid, or connection logic is required from your engineering team!

