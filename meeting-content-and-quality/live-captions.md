---
description: >-
  Live captions provide AI-powered, real-time text representation of spoken
  content during events within a meeting room, such as presentations, meetings,
  or videos.
---

# Live Captions

{% hint style="warning" %}
Live captions are currently in Public Beta.
{% endhint %}

Live captions automatically generate real-time text on the screen of a meeting room during the meeting itself. It usually focuses on key spoken content and may not include every single word, like filler words.&#x20;

Primarily used for real-time accessibility, helping participants follow along as the meeting progresses without needing to listen to the audio.

{% hint style="info" %}
Live captions is a supplementary feature of our paid Whereby Embedded plans. The price will depend on your **Embedded plan** type and the **language** chosen.

**Primary languages\*:**

* **Build (monthly)** plan: $0.0065 per unmuted participant minute.
* **Enterprise (annual)** plan:
  * $0.0055 per unmuted participant minute (for pre-purchased minutes).\*\*
  * $0.0065 per unmuted participant minute (for over usage).

**Secondary languages\***:

* Both plan types: $0.024 per unmuted participant minute.

**\*** Currently, our primary languages include: English, Spanish, Italian, French, Portuguese, and German. All other languages are secondary languages at this time. \
\*\* To pre-purchase minutes, please reach out to your CSM.
{% endhint %}

{% hint style="info" %}
_What is an unmuted participant minute?_ This is calculated using the number of participants who are unmuted during a call. For example, a 60 minute meeting containing 2 people who are unmuted for the whole meeting would use 120 unmuted participant minutes. Alternatively, a 60 minute meeting with 3 participants, where only 2 participants were unmuted and the third participant was muted for the whole meeting, would also use 120 unmuted participant minutes. From the moment a participant is unmuted, this usage counts towards the number of unmuted participant minutes, even if they do not actively talk or engage on the call.
{% endhint %}

## Setup <a href="#setup" id="setup"></a>

You can enable and configure Live Captions globally for your account, or individually for each room.&#x20;

### Global configuration <a href="#global-configuration" id="global-configuration"></a>

When you enable Live Captions globally through the dashboard, these settings become the default for all rooms and sessions. Enabling Live Captions globally will result in all meetings being captioned. You can override these global settings by specifying the caption on a per room basis.

If you want to use Live Captions for all of your meetings, you can enable it globally for your account. Go to “Configure” → “Transcription” section of your customer portal and scroll down to the "Live Captions" section. Then, toggle the Live Captions on:

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

### Per room configuration <a href="#per-room-configuration" id="per-room-configuration"></a>

If you want to use Live Captions for some of your meetings, or if you need a different configuration for some of them, you can configure Live Captions individually for the room. Room parameters will override the global Live Captions settings.

{% hint style="warning" %}
The default language set on your organization’s dashboard can be overridden on a room level. This could result in the price charged for your use of live captions/session transcriptions being impacted. Please refer to the price for different languages at the top of the page for further details.
{% endhint %}

To do so, create the room with [POST /meetings](https://docs.whereby.com/reference/whereby-rest-api-reference/meetings) request and specify the live caption configuration. It's a sub-property of the `liveTranscription` object.

For example, if you want a meeting with just live captions, and not live transcriptions, you can specify the `liveCaptions` option in the `liveTranscription` property, but set the `startTrigger` to `none`.

```json
    "liveTranscription": {
        "startTrigger": "none",
        "liveCaptions": true
    }
```

### Supported languages

Live captions generate real-time speech to text in the specified language. You need to declare the language used by your session participants in advance - in the [global configuration](live-captions.md#global-configuration) or [per room](live-captions.md#per-room-configuration).&#x20;

<details>

<summary>Live caption supported languages</summary>

* Catalan (ca)&#x20;
* Chinese (Mandarin, Simplified) (zh)
* Chinese (Mandarin, Traditional) (zh-TW)
* Czech (cs)
* Danish (da)
* Dutch (nl)
* English (en)
* Finnish (fi)&#x20;
* French (fr)
* German (de)
* German - Switzerland (de-CH)&#x20;
* Greek (el)
* Hindi (hi)
* Indonesian (id)&#x20;
* Italian (it)&#x20;
* Japanese (ja)&#x20;
* Korean (ko)&#x20;
* Latvian (lv)
* Malay (ms)
* Norwegian (no)
* Polish (pl)
* Portuguese (pt)&#x20;
* Brazilian Portuguese (pt-BR)&#x20;
* Romanian (ro)&#x20;
* Russian (ru)&#x20;
* Slovak (sk)&#x20;
* Spanish (es)&#x20;
* Swedish (sv)&#x20;
* Thai (th)&#x20;
* Turkish (tr)&#x20;
* Ukrainian (uk)&#x20;
* Vietnamese (vi)

</details>

### Known limitations

1. Currently, the language spoken cannot be auto-detected. For example, if the meeting is configured for English but French is spoken in the meeting, the language output will likely be incoherent. In this case, you will still be charged per unmuted participant minute for the configured language.
2. We don't show data related to Live captions in the Insights Dashboard.
3. Live captions are not available in the Whereby iOS app.&#x20;
