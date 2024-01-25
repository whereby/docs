---
description: >-
  If you want to embed video rooms in a website using HTML or a framework that
  supports HTML elements like React.js, a great option for doing this is by
  using a simple iframe.
---

# With Low Code

{% embed url="https://youtu.be/kNhhpA_RXg4?feature=shared" %}

Use a `roomUrl` you've created as the `src` attribute as in the example below.

```html
<iframe
  src="https://subdomain.whereby.com/room?minimal"
  allow="camera; microphone; fullscreen; speaker; display-capture; autoplay; compute-pressure; media-capture"
></iframe>
```

{% hint style="success" %}
If you'd like more programmatic control around the meeting experience with things like browser events and commands, we recommend reviewing our documentation about the [Web Component](broken-reference) from our SDK
{% endhint %}

The above code can be used for in website builders like [Squarespace or wordpress](embedding-in-squarespace-or-wordpress.md). When embedding, you can customize the room by [toggling features and behaviors via URL parameters](../../../../customizing-rooms/using-url-parameters.md).&#x20;

You can restrict which domains are allowed to embed your rooms via the Allowed domains feature on your dashboard.
