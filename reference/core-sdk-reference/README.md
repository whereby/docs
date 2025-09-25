# Core SDK Reference

The **Whereby Core SDK** is the low-level foundation of the Whereby SDK. It exposes a set of containers, actions, and client classes for working with local media and room connections. It's used to power the [Whereby Browser SDK,](../react-hooks-reference/quick-start/getting-started-with-the-browser-sdk.md) [Whereby Assistants](../assistant-sdk-reference/api-reference/assistant.md),[ React Native](../react-native-reference/) and other forms of Whereby meetings. It also contains utils which may be useful in custom experiences.&#x20;

### Audience

Core is designed for developers who need fine-grained control over the Whereby experience. It is best suited if you:

* Are building a custom video UI from scratch (not using React or our higher level components)
* Need to integrate Whereby into a non-React environment or a framework of your choice
* Want direct access to connection, media and layout state
* Are working on advanced use cases such as automation or headless integrations where you want to orchestrate media yourself at a lower level

For most web applications, we recommend starting with our higher level [`@whereby.com/browser-sdk`](../../whereby-101/create-your-video/in-a-web-page/using-whereby-react-hooks-build-a-telehealth-app.md) , or if you can [`@whereby.com/assistant`](../assistant-sdk-reference/api-reference/assistant.md) to create custom bots that might work for your use case. Reach for `@whereby.com/core` when you need deeper controls.&#x20;

### Use Cases

* Implementing a **custom meeting UI** that renders participants with your own layout engine
* Running **automated tests** or **headless monitoring** clients with fine control over media state
* Building **framework integrations** (e.g. Vue, Svelte, Angular) on top of Whereby's primitives
* Experimenting with **advanced media workflows** like custom device selection

### Requirements

The Core SDK is intended for **browser environments** and requires access to browser APIs:

* `navigator.mediaDevices` (to request camera/microphone)
* `MediaStream` and `MediaTrack`
* `RTCPeerConnection` (WebRTC signaling and transport).&#x20;

It can also be used in Node.js if you provide compatible polyfills for the necessary Media and WebRTC APIs. However, if you're considering using Node.js we would recommend checking out our [Assistant SDK](../assistant-sdk-reference/api-reference/assistant.md) that handles this for you!&#x20;

## Getting started

### Installation

Install the `@whereby.com/core` package from the public[ npm registry.](https://www.npmjs.com/package/@whereby.com/core)

{% tabs %}
{% tab title="npm" %}
```bash
npm install @whereby.com/core
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @whereby.com/core
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @whereby.com/core
```
{% endtab %}
{% endtabs %}
