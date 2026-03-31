---
description: Follow these steps below to set up your Whereby Embedded account!
---

# Initial setup

{% hint style="success" %}
These steps are essential for anyone using Whereby Embedded, whether you are using the [web component and prebuilt UI](../../whereby-for-web-browser/web-component-and-pre-built-ui/), the [Whereby Browser SDK](../../whereby-for-web-browser/react-based-browser-sdk/), or another solution.
{% endhint %}

### 1. Sign up for an Embedded account

To start with, sign up for a Whereby Embedded account:

1. Go to the ['Choose your Embedded' plan](https://whereby.com/information/embedded/select-plan) page.
2. Choose the Free plan for now. This gives you everything you need to test out the product; you can always upgrade your plan later on when you start implementing a production app.
3. Follow the steps to create an account.

{% hint style="info" %}
Note that Whereby has another product — [Whereby Meetings](https://whereby.com/information/meetings) — which provides you with static meeting rooms accessed through the Whereby website that you can use to meet friends, colleagues, and customers. Whereby Meetings has no access to our API or webhooks.
{% endhint %}

### 2. Generate an API key

From your account dashboard, go to “**Configure**” → “**API**” section of your account dashboard. Under the **API keys** portion, click the **Generate key** button to create a new key.

Your API key is only displayed once. Copy the key and save it to your secrets manager. You'll need it to send requests to our REST API.

### 3. Create a meeting room

Before you can create an embedded Whereby app, you need to create a meeting room. This is a key entity in the Whereby architecture — an app is passed a specific meeting room URL to join that meeting, and you can configure each meeting as required. Meeting room options include setting the number of participants, the end date of the meeting, whether it will be recorded or streamed, and more.

There are a couple of different ways to create a meeting room:

* **Via your account dashboard**: Go to the **Rooms** screen, click the **Create room** button, and follow the steps to create your room. You can choose the defaults on most screens, but you’ll need to choose an end date for your meeting on the final options screen.
* **Via our /**[**meetings**](../../reference/whereby-rest-api-reference/meetings.md) **API endpoint**: The request has the following requirements:
  * It must be sent to `https://api.whereby.dev/v1/meetings`.
  * It must be authenticated using Bearer Token authentication. Send your API key as part of the [`Authorization`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Authorization) header.
  * It must be a [`POST`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/POST) request with a body containing a valid JSON string ([`Content-Type`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Content-Type)`: application/json`). There are many [options](../../reference/whereby-rest-api-reference/meetings.md) you can include in the JSON, but at the very least, it must contain an `endDate` field.

{% hint style="info" %}
**Which creation method should I use?**

At this point in your journey, it doesn’t matter. In our quickstarts and starter guides, we’ll assume you are providing a static room URL, and your app will join the same room every time.

However, in a production app, you will likely want to allow different users or customers to create rooms dynamically. They probably don’t want to all share the same room. For this reason, we’d recommend getting used to using the API call method.
{% endhint %}

The following cURL example shows a minimal meeting room request (replace `YOUR-API-KEY` with the API key you generated in the previous step), which you can enter into your terminal/command line to create a meeting room:

```
curl https://api.whereby.dev/v1/meetings \
  --header "Authorization: Bearer YOUR-API-KEY" \
  --header "Content-Type: application/json" \
  --request POST \
  --data '{
    "endDate": "2099-07-01T10:01:32.041Z",
    "roomMode": "group",
    "fields": [
      "hostRoomUrl"
    ]
  }'
```

{% hint style="info" %}
**Note**: If you don’t have cURL available on your machine, you can generate the HTTP request using a different app, such as [Postman](https://docs.whereby.com/whereby-101/using-the-rest-api/using-postman).
{% endhint %}

{% hint style="info" %}
**Note**: While not mandatory for a valid API request, we've included the following data fields, as they are very useful to become familiar with early on:

* `roomMode` allows you to specify the capacity of the room you are creating. The default value, `normal`, allows up to 4 people to join the meeting. Setting `roomMode` to `group` allows more than 4 people to join.
* `fields.hostRoomUrl` causes a host URL to be provided in the response. Joining via this URL allows you to join the room as a host. See [User roles & Meeting Permissions](../../whereby-product-features/user-roles-and-privileges.md) for more details of host privileges.&#x20;
{% endhint %}

#### Getting the meeting room URL

Getting the meeting room URL to provide to your app depends on how you created the meeting room.

* If you created it via your account dashboard, you’ll see a list of the currently active rooms on the **Rooms** screen. To get the URL, click on the “link” icon on your room row, then click **Copy participant link**.
*   If you create it via an API call, the response will contain a JSON string along the following lines:<br>

    ```json
    {
      "startDate":"2025-08-06T19:37:21.658Z",
      "endDate":"2099-07-01T10:01:32.041Z",
      "roomName":"/50041254-faca-4e6b-b551-d36e79ea4987",
      "roomUrl":"https://my-company.whereby.com/50041254-faca-4e6b-b551-d36e79ea4987",
      "meetingId":"107585562",
      "hostRoomUrl":"https://my-company.whereby.com/50041254-faca-4e6b-b551-d36e79ea4987?roomKey=..."
    }
    ```

    \
    You need to copy the `roomUrl` field value.

#### Which creation method should I use?

At this point in your journey, it doesn’t matter. In our quickstarts and starter guides, we’ll assume you are providing a static room URL, and your app will join the same room every time.

However, in a production app, you will likely want to allow different users or customers to create rooms dynamically. They probably don’t want to all share the same room. For this reason, we’d recommend getting used to using the API call method.

### Next steps

Now that you’re set up, we'd recommend creating an embedded Whereby app using the [prebuilt web component](../../whereby-for-web-browser/web-component-and-pre-built-ui/web-app-quickstart.md).
