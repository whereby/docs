---
description: >-
  Our Streaming feature allows for you to share your meeting feed to an external
  service like YouTube or Twitch via RTMP.
---

# Streaming RTMP

{% hint style="info" %}
Streaming is a complementary feature of our paid Whereby Embedded plans. You can review the pricing and options [on our site](https://whereby.com/information/embedded/pricing/).
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=wAEwDL6JMMg" %}
How to set up streaming in Whereby Embedded
{% endembed %}

## Create a streaming meeting with the API

Create a meeting with streaming set up for others to use, such as a teacher or an event organizer. The stream is fixed and can not be changed inside the room afterward.

\
When creating a meeting [via our API](../reference/whereby-rest-api-reference.md#create-meeting), you can pass your platforms `RTMP URL` and include the `stream key` separated by a forward slash. Then specify how you'd like the stream to begin. "**Automatic**", "**Manual**", or "**Prompt**" by including an in room message to the host.

You'll have these same options when creating a meeting [via our dashboard](../whereby-101/using-the-rest-api/using-create-a-room.md).

## Obtaining Streaming info

{% tabs %}
{% tab title="YouTube" %}
{% hint style="info" %}
If you haven't streamed before, in order to access your stream key on YouTube you must go through the initial stream setup which includes a 24 hour account processing period.
{% endhint %}

Your stream key and URL can then be located under the "Stream Settings" once you've completed the initial setup. Google has documentation on this topic in their article "[Manage live stream settings](https://support.google.com/youtube/answer/9854503)".

![](<../.gitbook/assets/YouTube Settings.png>)
{% endtab %}

{% tab title="Twitch" %}
You can obtain a stream key from the "Stream" section of the Twitch creator dashboard. For more information you can visit Twitch's [Support Center](https://help.twitch.tv/s/article/creator-dashboard).

1. Log into your Twitch account
2. Click on your profile icon in the top right and navigate to "Creator Dashboard"
3. On the left side of the screen select **Settings->Stream**
4. The stream key is located at the very top of the page

![](<../.gitbook/assets/Twitch Settings.png>)

You can enter your stream key as part of the [broadcast URL](https://dev.twitch.tv/docs/video-broadcast). Twitch provides additional insight on choosing a broadcast URL in their [support documentation](https://help.twitch.tv/s/article/guide-to-broadcast-health-and-using-twitch-inspector?language=en\_US#HowtoChooseaTwitchIngestServer).
{% endtab %}

{% tab title="Facebook" %}
For in depth instructions on how to do live via Facebook, we recommend reviewing their support article "[How do I go live on Facebook](https://www.facebook.com/help/587160588142067)".

You'll get started by [creating a live stream](https://www.facebook.com/live/create), create a key, then copy and paste the server URL and key into Whereby.

![](<../.gitbook/assets/Facebook Settings.png>)
{% endtab %}

{% tab title="Vimeo" %}
Going live on Vimeo does require a paid Vimeo Premium or Vimeo Enterprise membership. You can view their plans and pricing on [their website](https://vimeo.zendesk.com/hc/en-us/articles/115012811168#h\_60c83788-04d4-4353-8a3a-cd0d0bb8b6bc).

Vimeo provides an in depth guide on creating a live event and setting up RTMP connections in their support document "[Create and go live to an event](https://vimeo.zendesk.com/hc/en-us/articles/115012811168#h\_60c83788-04d4-4353-8a3a-cd0d0bb8b6bc)". During the process they provide a link and stream key that you can copy and use in Whereby.

![](<../.gitbook/assets/Vimeo settings.png>)
{% endtab %}
{% endtabs %}
