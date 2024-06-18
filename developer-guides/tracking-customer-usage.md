# Tracking Customer Usage

{% embed url="https://youtu.be/HKr836Exm1o" %}

## Getting Started

Whereby has a number of options you can use to track and report on the usage of your video meetings. The Insights Suite and API are a great starting point, but with a few simple changes to your meeting links you can enhance this data to enable segmentation by customer, or even by user.

{% hint style="info" %}
You can use either of the options below individually, but you can also implement both for maximum flexibility!
{% endhint %}

### roomNamePrefix

The roomNamePrefix option allows you to track usage on a per-room basis. This prepends a string of your choosing to the roomName, which allows for sorting based on that prefix when you're reporting on usage.

{% code overflow="wrap" %}
```json
// Example Request
{
  "roomNamePrefix": "test-prefix-",
  "endDate": "2099-06-13T19:22:05.168Z",
}

//Example Reponse
{
    "startDate": "2099-06-13T19:22:05.168Z",
    "endDate": "2099-06-27T19:22:04.807Z",
    "roomName": "/test-prefix-87e0497f-40e0-47f1-9a27-fb72fdbc1fda",
    "roomUrl": "https://yoursubdomain.whereby.com/test-prefix-87e0497f-40e0-47f1-9a27-fb72fdbc1fda",
    "meetingId": "87385993"
}
```
{% endcode %}

With this option you'll need to have some mechanism for detecting which of your customers is requesting the room, and then programmatically set the roomNamePrefix based on that result.&#x20;

For example if you have a function that logged in users can use to generate a meeting, you could assign the roomNamePrefix based on the company they work for.

### Attributes / URL Parameters

{% hint style="info" %}
[Attributes](https://docs.whereby.com/reference/using-the-whereby-embed-element#attributes-of-the-component) are available on our Web Component, and are generally what we recommend for implementing Whereby's pre-built experience. If you're using an iframe though you can also opt for [URL Parameters](https://docs.whereby.com/whereby-101/customizing-rooms/using-url-parameters).
{% endhint %}

Attributes and URL Parameters allow you to report on usage a specific user, or a grouping of users. Specifically, `externalId` and `metadata` are included in our webhook and insights data.

{% code overflow="wrap" %}
```html
Attributes Example

<whereby-embed room="https://subdomain.whereby.com/your_room" displayName="John Smith" metadata="some string" externalid="1234" />

```
{% endcode %}

{% code overflow="wrap" %}
```html
URL Parameters Example

<iframe
  src="https://yoursubdomain.whereby.com/test-prefix-87e0497f-40e0-47f1-9a27-fb72fdbc1fda?metadata=some_string&externalId=1234"
  allow="camera; microphone; fullscreen; display-capture; autoplay; compute-pressure"
></iframe>
```
{% endcode %}

In both of the above cases you could simply pass in the user's ID on your own platform as the value, which would make relating data between your back-end and Whereby very straightforward.

## Report on Tracked Data

### Insights Dashboard

<figure><img src="../.gitbook/assets/Screenshot 2024-06-13 at 10.26.16 AM.png" alt=""><figcaption></figcaption></figure>

The Insights Dashboard is great for quickly referencing insights data or getting a general overview of your usage. Simply log in to your Whereby account, and select **Insights** **>** **Rooms**.

From here we can search for a specific roomNamePrefix to see a list of rooms that contain that prefix. The PM used column shows the number of Participant Minutes that have been used in that meeting, and the Sessions column shows how many sessions (individual meetings) have taken place in that room.

#### Session and Participant Details

Click on a room name to see a list of sessions that have taken place in that room. From there click on the relevant session to see all of the details.

<figure><img src="../.gitbook/assets/Screenshot 2024-06-13 at 10.32.20 AM.png" alt=""><figcaption></figcaption></figure>

On the Session details page you can click on a Participant ID to see detailed information on that participant, including the externalId via the events section. You can also view quality related stats for that participant like any bandwidth, CPU issues, or packet loss. This can be a useful tool for troubleshooting if a participant reports issues during a specific meeting.

<figure><img src="../.gitbook/assets/Screenshot 2024-06-13 at 10.34.24 AM.png" alt=""><figcaption></figcaption></figure>

### Insights API

The Insights API includes all of the same information that you can access from the Dashboard, but it also allows for automating the retrieval and further processing of that data.&#x20;

With this method you'll need to have some experience querying APIs, but any method you normally use for API requests will be supported! If you don't have a lot of technical experience you can also opt for a tool like [Postman](https://docs.whereby.com/whereby-101/using-the-rest-api/using-postman), and Whereby has set up a collection of pre-formatted requests that you can use to quickly get started! &#x20;

For a complete overview of using the Insights API, check out our [REST API Reference](https://docs.whereby.com/reference/whereby-rest-api-reference#insights-rooms).

### Web Component / Webhook Events

[Web Component](https://docs.whereby.com/whereby-101/create-your-video/in-a-web-page/using-the-whereby-embed-element#listening-to-events) and/or [Webhook](https://docs.whereby.com/meeting-content-and-quality/insights-suite-and-api/webhooks) Events offer access to real-time event data, which can be helpful for tracking individual users. For example if you need to verify that your hosts/guests are arriving to meetings on-time, these events will give you real-time reports on join and leave times for the room!

These events include the `metadata` and `externalId` values that are passed in as Attributes or URL parameters. You'll also be able to capture the roomName with either method, since this is included on the Web Component and Webhooks have a roomName value in their payload!

Once you're capturing data, you can do your on processing on it, or even import the values into your own insights platform for even more powerful insights.

