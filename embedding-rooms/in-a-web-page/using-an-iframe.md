---
description: >-
  If you want to embed video rooms in a website using HTML or a framework that
  supports HTML elements like React.js, a great option for doing this is by
  using a simple iframe.
---

# Using an iframe

{% embed url="https://youtu.be/kNhhpA_RXg4" %}

Use a `roomUrl` you've created as the `src` attribute as in the example below.

```html
<iframe
  src="https://subdomain.whereby.com/room?minimal"
  allow="camera; microphone; fullscreen; speaker; display-capture; autoplay; compute-pressure"
></iframe>
```

{% hint style="success" %}
If you'd like more programmatic control around the meeting experience with things like browser events and commands, we recommend reviewing our documentation about the [Embed Element](using-the-whereby-embed-element.md) from our [SDK](../../whereby-embedded-sdk-beta/)
{% endhint %}

When embedding, you can customize the room by [toggling features and behaviors via URL parameters](../../customizing-rooms/using-url-parameters.md).&#x20;

To learn how to restrict which domains are allowed to embed your rooms, read the [Allowed domains section](../allowed-domains.md).
