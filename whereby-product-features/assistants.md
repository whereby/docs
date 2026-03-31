---
description: >-
  Whereby Assistants let you connect AI agents to video rooms, stream and
  process audio, and automate real-time interactions.
---

# Assistants

{% hint style="warning" %}
⚠️ **Closed Beta**\
The Assistants feature is currently in Closed Beta and available to Enterprise plan customers on annual plans. If your organisation is on a Enterprise plan, you can request access by emailing [embedded@whereby.com](mailto:embedded@whereby.com). Assistants will be made generally available after the beta.
{% endhint %}

{% hint style="info" %}
Enabling assistants is not charged as an additional feature — they are treated as normal participants in a room. Your standard Whereby participant minute billing applies to this usage.
{% endhint %}

Assistants are headless participants that connect to a Whereby room to enhance your user experience.&#x20;

They can:&#x20;

* Access all participant audio and video streams in a connected room, including a combined audio stream of all participants
* Perform in-room actions like sending chat messages, starting cloud recording, letting participants into the room
* Alongside our [Trigger API](../reference/assistant-sdk-reference/api-reference/trigger.md) be triggered automatically via webhooks (e.g. when a first participant joins a room or when two or more participants join and a room session starts).

## Prerequisites

The Assistant SDK is intended for Node.js environments and depends on some external tooling:&#x20;

* **Node.js ≥ 20**
* [**FFmpeg**](https://ffmpeg.org/) must be installed on the host machine if you wish to make use of the **combined audio stream** functionality.&#x20;

{% hint style="warning" %}
Note: The SDK cannot be run in the browser - use [Core](../reference/core-sdk-reference/) or [Browser](../reference/react-hooks-reference/) SDK for creating user-interface based integrations.&#x20;
{% endhint %}

## Setup

There are a few steps required to set up Assistants to be used in your Whereby sessions.&#x20;

### Creating an Assistant

Before an Assistant will be granted access to your sessions, they must first be created in the Whereby dashboard. This will assign each individual assistant an `assistantKey`. This key uniquely identifies every assistant and will be later used in the implementation stage when joining a call as a Whereby Assistant.&#x20;

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

In this stage, you can assign some key details to your Assistant including:&#x20;

* Avatar
* Display Name
* Description
* If you are using the [Trigger API](../reference/assistant-sdk-reference/api-reference/trigger.md), you can also set the URL that you have listening for Whereby Webhook events.

Once you've assigned these details, you can save the Assistant and obtain the `assistantKey`.&#x20;

{% hint style="info" %}
You can always edit an Assistant's configuration, view its `assistantKey`, and enable or disable it in your organization after it has been created.&#x20;
{% endhint %}

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

Your Assistant will be created in a `disabled` state initially. Once you're ready to start using your Assistant, you can toggle it to the `enabled` state via the dashboard.&#x20;

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

Once enabled, any Whereby Assistant that now provides the `assistantKey` copied from this interface will self-identify as this assistant.

If you'd like room hosts to be able to request this assistant from within the room, you can enable **Manual** **Invite**. More on that below.

{% hint style="info" %}
You'll need an Endpoint URL configured before Manual Invite can be enabled.
{% endhint %}

That's it for the configuration steps!

## In Room

When an assistant now joins any room (using its `assistantKey`), an in-room notification will be shown to all participants.

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

A Whereby Assistant then sits in the room status bar for the duration that it is connected and until it leaves the room.&#x20;

Interacting with the assistant icon in the room status displays the assistants menu.&#x20;

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

### Manual Invite

When enabled, hosts will be able to invite your Whereby Assistant into their room with the **Invite** button in the assistants menu.

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

The **Invite** button will send a webhook event with the type `assistant.requested` endpoint URL configured for this assistant. You can configure your webhook server to start the requested assistant and have it join the room specified in the webhook event payload.

Once an assistant has joined the room, hosts have the ability to remove it through the assistants menu.

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

When the assistant leaves the room it will announce that it is leaving to all participants remaining in the room.

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

And that's it!

Now, you can view our [Quick Start](../reference/assistant-sdk-reference/quick-start.md) guide to set up and explore the possibilities for your Assistant.&#x20;

## Key Features

### Individual Streams

Access the individual audio and media streams of all participants in a session.&#x20;

**Use cases**: video analysis, video recording, per-participant live transcription and recording

### Combined Audio&#x20;

All remote participant audio mixed into a single audio stream.

**Use cases**: single track live transcription, AI models, sentiment analysis, audio only recording

### Trigger API&#x20;

Start assistants automatically when specific webhooks arrive (eg `room.client.joined` )

See the [Trigger API Reference](../reference/assistant-sdk-reference/api-reference/trigger.md) for more details

### In-Room Actions&#x20;

Assistants can perform a variety of actions inside a room — they are given the same action permissions as host users. These actions include:&#x20;

* Sending and receiving chat messages
* Starting or stopping cloud recordings
* Spotlighting participants&#x20;
* Request audio / video to be enabled for participants
* Admitting waiting participants

See the [Core API Reference](../reference/core-sdk-reference/) for the full list of in-room actions available.&#x20;

## See Also

* [Assistant SDK Reference](../reference/assistant-sdk-reference/): Complete documentation to get started building a Whereby Assistant.
