---
description: >-
  The Whereby browser SDK includes React hooks and components which allow
  integrating a fully custom video experience into your web application.
---

# React Hooks Reference

## Getting started

{% hint style="warning" %}
The browser SDK is currently in beta, meaning we are still correcting smaller issues before marking it as ready for production. While in beta, we will break our api surface, but cannot guarantee there won't be any breaking changes between the current version and the next.
{% endhint %}

### Installation

Install the `@whereby.com/browser-sdk` package from the public [npm registry](https://www.npmjs.com/package/@whereby.com/browser-sdk).

{% tabs %}
{% tab title="npm" %}
```bash
npm install @whereby.com/browser-sdk@2.0.0-beta3
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @whereby.com/browser-sdk@2.0.0-beta3
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @whereby.com/browser-sdk@2.0.0-beta3
```
{% endtab %}
{% endtabs %}

While in beta, you need to explicitly install new versions in order to get new features and fixes. If you want to stay up-to-date, please visit the [npm versions page](https://www.npmjs.com/package/@whereby.com/browser-sdk?activeTab=versions) regularly.

## Available hooks

{% content-ref url="useroomconnection.md" %}
[useroomconnection.md](useroomconnection.md)
{% endcontent-ref %}

{% content-ref url="uselocalmedia.md" %}
[uselocalmedia.md](uselocalmedia.md)
{% endcontent-ref %}

## Available components

{% content-ref url="videoview.md" %}
[videoview.md](videoview.md)
{% endcontent-ref %}
