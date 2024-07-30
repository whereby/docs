---
description: >-
  Session Transcription allows you to produce a transcript directly from the
  live session, without the need to record it. Full transcript is available
  right after the session is finished.
---

# Session Transcription

{% hint style="info" %}
Session Transcription for Grow plan customers is currently available on demand (terms and conditions apply). Email us at embedded@whereby.com if you want to start using Session Transcription.
{% endhint %}

{% hint style="info" %}
Session Transcription is a supplementary feature of our paid Whereby Embedded plans. You can review the pricing and options [on our site](https://whereby.com/information/embedded/pricing/).&#x20;
{% endhint %}

Session transcripts are created from live streaming of Whereby session audio in real time, and once the session is finished they are saved as text files accessible through the customer portal or via the API. You can use the transcripts as a standalone resource (eg. for compliance purposes) or send to an external service for post processing (eg. to derive key topics or create a session summary).&#x20;

## Setup

You can enable and configure Session Transcription globally for your account, or individually for each room. All transcripts can be downloaded manually through the customer portal, or programatically with the API requests.

{% hint style="info" %}
When you enable Session Transcription globally through the customer portal, these settings become the default for all rooms and sessions. Specifically, enabling Session Transcription globally will result in all sessions being transcribed, including sessions in rooms created previously. You can override these global settings by specifying the transcription options for each room individually in the [POST /meetings](https://www.notion.so/o/UqLY7vLb01EgY68ZG0GF/s/c7hN8ZKHNZris5300KEl/\~/changes/551/reference/whereby-rest-api-reference#meetings-1) requests.
{% endhint %}

If you want to use Session Transcription for all your sessions, you can enable it globally for your account. Go to “Configure” → “Transcription” section of your customer portal and choose "Session Transcription" option. Then choose the trigger and the main language of your sessions.

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption><p>Global configuration of transcriptions for all your sessions</p></figcaption></figure>

You can choose between the following transcription triggers:

* **Auto-start (1 person)** \
  Transcription will start when the first person joins and end when the last person leaves.&#x20;
* **Auto-start (2 people)**\
  Transcription will start when 2 people join a room and end when the last person leaves.

If you want to use Session Transcription for some of your sessions, or if you need a different configuration for some of the sessions, you can configure Session Transcription individually for the room. Room parameters will override the global Session Transcription settings.&#x20;

To do so, create the room with [POST /meetings](../../reference/whereby-rest-api-reference.md#meetings-1) request and specify the transcription options (with the `"startTrigger"` and language of your choice):&#x20;

```json
"liveTranscription": { 
    "language": "en", 
    "startTrigger": "automatic" 
    },
```

You can choose between `"automatic"` and `"automatic-2nd-participant"` triggers, and below you will find the [list of supported languages](session-transcription.md#supported-languages).

When the session is transcribed, the participants see a notification circle in the top-left meeting status bar:

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption><p>Red circle in the top-left panel indicates session transcription in progress.</p></figcaption></figure>

## Download and delete transcripts

Transcripts are saved in Whereby-provided storage, and they are available for download soon after the session is finished.&#x20;

In order to download the transcript manually go to “Transcriptions” section of your customer portal. The transcript is downloaded as an .md file. From there you can also [create a session summary](../transcribing-sessions-1.md#manual-session-summaries) or delete the transcript.

If you want to automate your transcription process, you can do so programatically with a combination  of API requests and webhook events.

Once the transcript is ready, Whereby sends a `transcription.finished` [webhook](../insights-suite-and-api/webhooks.md#transcription-data-properties) event. Hook onto that event to fetch the `transcriptionId` of the session that you want to transcribe.&#x20;

Send a [GET /transcriptions/{transcriptionId}/access-link](../../reference/whereby-rest-api-reference.md#transcriptions-transcriptionid-access-link) request to get the download link of the transcription file. Transcripts are downloaded as .md files.&#x20;

All transcripts will be stored in the Whereby-provided storage until you delete them. If you want to  minimise the time when your sessions' content is stored in the Whereby-provided storage, you can delete the transcript with [DELETE /transcriptions/{transcriptionId}](../../reference/whereby-rest-api-reference.md#transcriptions-transcriptionid-1) request.

## Supported languages

Session Transcription generates a transcript in the specified language. You need to declare the language used by your session participants in advance - in the global configuration or individually for each room with [POST /meetings](../../reference/whereby-rest-api-reference.md#meetings-1) request. Once the room is created, you cannot change the language of the Session Transcription.&#x20;

The following languages are supported by Session Transcription: Bulgarian (bg), Catalan (ca), Chinese (Mandarin, Simplified) (zh), Chinese (Mandarin, Traditional) (zh-TW), Czech (cs), Danish (da), Dutch (nl), English (en), Estonian (et), Finnish (fi), Flemish (nl-BE), French (fr), German (de), Greek (el), Hindi (hi), Hungarian (hu), Indonesian (id), Italian (it), Japanese (ja), Korean (ko), Latvian (lv), Lithuanian (lt), Malay (ms), Norwegian (no), Polish (pl), Portuguese (pt), Brazilian Portuguese (pt-BR), Romanian (ro), Russian (ru), Slovak (sk), Spanish (es), Swedish (sv), Thai (th), Turkish (tr), Ukrainian (uk), Vietnamese (vi).

## Known limitations

{% hint style="warning" %}
Session Transcriptions are not compatible with [Breakout Groups](../../whereby-101/customizing-rooms/breakout-groups-with-embedded.md) feature. When using Breakout Groups, the transcript will cover the conversation from the main room, but the audio from individual groups will not be transcribed.&#x20;
{% endhint %}

{% hint style="warning" %}
Session Transcriptions are currently not considered to be HIPAA compliant. Avoid using Session Transcriptions in order to maintain HIPAA compliance of your Whereby sessions. [Learn more about Whereby HIPAA compliant setup](../../whereby-101/faq-and-troubleshooting/hipaa-compliant-setup.md).
{% endhint %}

{% hint style="info" %}
Session Transcription is available for sessions up to 12 hours long.
{% endhint %}

## Coming soon

We’re excited about the future of API-assisted content processing and wanted to give you a sneak peek at what’s on the horizon. Here’s a quick look at the features and improvements we’re actively working on to enhance Session Transcriptions of Whereby sessions:

* Manual trigger, so that the host can start and stop transcribing the session.
* `<whereby-embed>` methods to start and stop transcribing programatically.
* Abiliy to save the transcript into customer-managed AWS S3 bucket.
* Live preview of the transcript, visible to all session participants.
* Ability to download the transcript by the host or participants.
* Integration point to plug into the live transcript in real-time (eg. to send it into 3rd party processing tool like a chatbot).
