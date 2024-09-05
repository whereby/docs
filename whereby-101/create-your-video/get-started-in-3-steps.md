---
description: >-
  With Whereby Embedded and our browser SDK, you can add video calls and video
  conferencing to your website in three easy steps.
---

# Get started in 3 steps

## Introduction

[WebRTC](https://www.w3.org/TR/webrtc/) is a protocol and collection of browser APIs that enable real-time communications. With WebRTC, you can send audio, video, images, and text without the need for a server — that is, as long as both parties are on the same network.

For most real-world uses of WebRTC, you'll need a server. In fact, you'll need a few servers.

* A **signaling** server to manage connections between users.
* A **STUN** server to discover the public IP addresses of user devices, and/or a **TURN** server to relay data between users.
* A **media server** to distribute audio, video, and other media for multi-party communications.

Or, you can use [**Whereby Embedded**](https://whereby.com/information/embedded). Whereby has built a network of signaling, STUN and media servers that you can use as a service. With Whereby Embedded and our browser SDK, you can add video calls and video conferencing to your website in three easy steps.&#x20;

{% hint style="success" %}
Get [started for free](https://whereby.com/information/embedded/pricing)!
{% endhint %}

### **Step 1: Generate an API key**

From your account dashboard, go to the _Configure_ screen. Under the API keys panel, click the **Generate key** button to create a new key.

Your API key is only displayed once. Copy the key and save it to your secrets manager. You'll need it to send requests to our REST API.

### **Step 2: Create a meeting room using the REST API**

Although you can create rooms from your account dashboard, using the [REST API](https://docs.whereby.com/reference/whereby-rest-api-reference) lets you automate the process.

To create a meeting room, send a POST request to the [`/meetings`](../../reference/whereby-rest-api-reference/#meetings) end point. Whereby's REST API uses [Bearer Token authentication](https://datatracker.ietf.org/doc/html/rfc6750). Send your API key as part of the `Authorization` header.

The request body should include a JSON-formatted string, with an [\[endDate\] field.](https://docs.whereby.com/reference/whereby-rest-api-reference#meetings-1) `endDate` should be a valid date string. It tells Whereby when the meeting room should expire. Here's an example using [cURL](https://curl.se/).

```bash
curl https://api.whereby.dev/v1/meetings \
    --header "Authorization: Bearer $YOUR_API_KEY" \
    --header "Content-Type: application/json" \
    --request POST \
```

Response:

{% code overflow="wrap" %}
```json
{
    "startDate": "2024-02-25T05:57:42.323Z",
    "endDate": "2099-02-18T14:23:00.000Z",
    "roomName": "/0000-1234-5678-yyyy-xxxxxx",
    "roomUrl": "https://example.whereby.com/0000-1234-5678-yyyy-xxxxxx"
}
```
{% endcode %}

### **Step 3: Add your video conference to your application**

Whereby Embedded offers a couple of options for integrating video calling with your application. You'll need the `roomURL` from the API response in Step 2.

#### **Add video calling using Whereby's web component**

For simple projects that don't use a bundler, try Whereby's [web component](https://docs.whereby.com/reference/using-the-whereby-embed-element). Load it from our CDN, using a `script` tag. Use the value of `roomURL` for the web component's `room` attribute value.

```html
<html>
  <head>
     {" "}
    <script
      src="https://cdn.srv.whereby.com/embed/v2-embed.js"
      type="module"
    ></script>
  </head>
  <body>
     {" "}
    <div class="container">
           {" "}
      <whereby-embed room="https://example.whereby.com/0000-1234-5678-yyyy-xxxxxx" />
       {" "}
    </div>
  </body>
</html>;
```

`whereby-embed` is a custom element, which mean you can use it as a CSS selector shown below

```
:root {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
  overflow: hidden;
}

body,
whereby-embed {
  width: inherit;
  height: inherit;
  padding: inherit;
  margin: inherit;
}
```

Both `iframe` and `whereby-embed` options use the pre-built Whereby UI. The pre-built UI gives you control over things like background and foreground colors. You can also add your company's logo, and choose which features you'd like to show to or hide from users.

Perhaps you'd like to customize your button icons instead. Maybe you'd like to change the shape of your video tiles, or how they're arranged. For fine-grained control over your video call layouts, try the [Whereby Browser SDK](https://www.npmjs.com/package/@whereby.com/browser-sdk) with React hooks.

#### **Adding customized video calls using the Whereby Browser SDK and React hooks**

Use the Whereby Browser SDK to create video conferencing UIs that fully reflect your brand. You'll need [Node](https://nodejs.org/) and a package manager such as [npm](https://www.npmjs.com/), [Yarn](https://yarnpkg.com/), or [pnpm](https://pnpm.io/) for this option. You'll also need a build tool such as [Vite](https://vitejs.dev/), [Parcel](https://parceljs.org/), or [Webpack](https://webpack.js.org/)

Install the SDK using the `npm install`, `yarn add`, or `pnpm install` command.

```bash
npm install @whereby.com/browser-sdk
```

Then import the SDK into your React project, and connect to your room.

```tsx
import React from "react";
import { useRoomConnection } from "@whereby.com/browser-sdk/react";

const MEETING_URL = 'https://example.whereby.com/0000-1234-5678-yyyy-xxxxxx';

const Meeting = () => {

  const mediaOptions = {
    localMediaOptions: {
        audio: true,
        video: true,
    }
  }

  const connection = useRoomConnection(MEETING_URL, mediaOptions);

  const {state, components} = connection;
  const {localParticipant, remoteParticipants} = state;
  const {VideoView} = components;

  return (
    <>
      {localParticipant?.stream ? (
        <div id="you">
          <VideoView stream={localParticipant?.stream} muted />
        </div>
       ) : null
      }

      {remoteParticipants[0]?.stream ? (
        <div className="participant">
          <VideoView stream={remoteParticipants[0]?.stream} className="friend" />
        </div>
      ) : null
      }
    </>
  )
}

export default Meeting;
```

> Written by [Tiffany Brown](https://whereby.com/blog/author-tiffany-brown/)
