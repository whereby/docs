---
description: >-
  We've compiled some of the most common questions we receive about Whereby
  Embedded.
---

# FAQ & Troubleshooting

You can check for service outages and issues on our status page:

{% embed url="https://wherebystatus.com" %}

### Common Questions

<details>

<summary>URL parameters aren't working as expected</summary>

1. The most common error we see relating to URL parameters is incorrectly using "?" twice in the meeting room URL. Combining parameters can be achieved by using the ampersand symbol (&) for example:\
   `?minimal`**`&`**`screenshare=off`.
2. If the person using/testing the room is also logged into their Embedded account, it will override any parameters added to the room URL. Try accessing the room via a private or incognito window to verify your links are working as expected.

</details>

<details>

<summary>My hosts don't have the correct meeting privileges</summary>

1. Host privileges will only be available while the room is active and until 1 hour after the meeting's `endDate`
2. Check your [hostRoomUrl](../user-roles-and-privileges.md) to make sure you've properly separated the `roomkey` and `minimal` parameters with "&"

</details>

<details>

<summary>How do I enable integrations in my meetings?</summary>

Integrations are disabled for Whereby Embedded by default while using the [`?minimal`](../customizing-rooms/using-url-parameters.md#minimal) parameter. We set this by default because [Content Security Policy](https://en.wikipedia.org/wiki/Content\_Security\_Policy) restrictions can sometimes cause integrations to fail in unexpected ways. Currently only our YouTube and Miro integrations will work in an embedded setting.

If you would like to test an integration in your embedded meeting, you can do so by adding the `?roomIntegrations=on` parameter.

Please note, we offer limited support for integrations in an embedded meeting, even when enabled. It's highly recommended that you test before using it in a production context.

</details>

<details>

<summary>CORS Policy Error in the browser console</summary>

In some cases you may experience an error in your browsers developer console that looks something like this:

<mark style="color:red;">`Access to fetch at "https://api.whereby.dev/v1/meetings' from origin S?? nas WithOutLoginMult:1 been blocked by CORS policy...`</mark>

You need to access the Whereby API from a server environment or computer. You aren't allowed access to the API via the browser console because it would expose your secret API key.

</details>
