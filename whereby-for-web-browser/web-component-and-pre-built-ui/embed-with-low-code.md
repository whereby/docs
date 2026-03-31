---
description: >-
  If you want to embed video rooms in a website using HTML or a framework that
  supports HTML elements like React.js, a great option for doing this is by
  using a simple iframe.
---

# Embed with low code

{% embed url="https://youtu.be/kNhhpA_RXg4?feature=shared" %}

Use a `roomUrl` you've created as the `src` attribute as in the example below.

```html
<iframe
  src="https://subdomain.whereby.com/room?minimal"
  allow="camera; microphone; fullscreen; display-capture; autoplay; compute-pressure"
></iframe>
```

{% hint style="danger" %}
Using an \<iframe> is the only option for low code builders, but that we generally do not recommend embedding via an iframe. Instead, we suggest using our [Web Component](../../reference/using-the-whereby-embed-element.md), as it provides more options and flexibility.&#x20;
{% endhint %}

### Additional details

* If you'd like more programmatic control around the meeting experience, this is where browser [events](using-whereby-events.md) and [commands](using-commands.md) come in!&#x20;
* The above code can be used for in website builders like [Squarespace or Wordpress](../../integrating-whereby-in-specific-environments/embedding-in-squarespace-or-wordpress.md).&#x20;
* When embedding, you can customize the room by [toggling features and behaviors via URL parameters](../../reference/using-the-whereby-embed-element.md).&#x20;
* You can restrict which domains are allowed to embed your rooms via the [Allowed domains feature](../../further-resources/faq-and-troubleshooting/allowed-domains-and-localhost.md) on your dashboard.
