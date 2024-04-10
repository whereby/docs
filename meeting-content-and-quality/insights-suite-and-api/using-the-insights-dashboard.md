# Using the Insights dashboard

## Rooms and sessions

On the Rooms page, you can see a list of all the rooms that currently exist and have been created in your account. They are listed by most to least recently used. You can easily filter the list by searching a room name.&#x20;

You can also access details of all the sessions that have happened within a specific room. A session starts once there are two or more people in a call.&#x20;

## Participants

Within a session, you can find the participants that attended the session. Each time your user reconnects to the room, they will receive a new, unique participant ID from us. You will be able to match participant IDs based on the display name they enter the room with, which is currently available in the event logs.&#x20;

{% hint style="success" %}
#### Participant Call Ratings

Whereby has a call ratings feature in **beta** that you can ask us to turn on for your account.

If you have this feature turned on, each participant will be asked at the end of their call to rate the call quality, and you can track these ratings in a dashboard will will provide to you. Contact Whereby to get this feature enabled.
{% endhint %}

## Display name

The display name of the participant is the name they enter the call with. Your users can either enter this name themselves or you can pass in for them using the [displayName parameter](https://docs.whereby.com/customizing-rooms/using-url-parameters#displayname-less-than-name-greater-than).&#x20;

Some customers choose to use generic names, such as `Doctor`  or `Patient` for added anonymity. If you would like for names not to appear in your Insights, please reach out and we can turn this feature off for you.&#x20;

Only room sessions after July 20, 2023 will include display name in the session details pages.&#x20;

## Browser and operating system

You can use this information to determine if there were any compatibility issues, since not all devices or browsers fully support WebRTC.&#x20;

Check out our list of [supported browsers and devices](../../end-user/end-user-support-guides/supported-browsers-and-devices.md)

## Reading the charts

{% hint style="success" %}
All dates and times in the Insights UI and API are in UTC.&#x20;

We capture values for each chart every 2 seconds. We believe this frequency helps capture the spontaneous nature of call issues.
{% endhint %}

#### Sending & receiving

We've separated packet loss and bitrate out into separate charts for sending and receiving. Each participant is both sending data in the form of audio and video to others on the call and receiving audio and video data in return.

{% tabs %}
{% tab title="Packet Loss" %}
### Packet loss

Packet loss is a great indicator if a user had poor network. It is a measure of how many data packets sent over a network are lost in transit. Packets are small chunks of data that are used to transmit information over a network.&#x20;

Packet loss can have a significant impact on the quality of video calls. When packets are lost, the video can become choppy or pixelated. In some cases, the video call may even drop altogether.

Packet loss can occur for a variety of reasons, including:

* **Network congestion:** When there is too much traffic on a network, it can cause packets to be dropped.
* **Hardware failure:** A faulty network device, such as a router or switch, can cause packets to be lost.
* **Interference:** Electromagnetic interference from other devices, such as microwaves or cell phones, can also cause packet loss.

### How to interpret packet loss values

The amount of packet loss is measured in percentage. For example, 1% packet loss means that 1 out of every 100 packets were lost. 0% packet loss is ideal for video calls, but some amount of packet loss between 0% - 2.5% is tolerable and expected.&#x20;

{% hint style="info" %}
We consider 3% packet loss to be our threshold for quality. Any value at or exceeding 3% for a sustained period of time means that the participant likely experienced reduced call quality.\
\
Very short periods (2 to 4s) of high packet loss are normal and expected. They will show up on the charts as sharp spikes. It is unlikely the participant noticed a dip in video quality during this short period.
{% endhint %}
{% endtab %}

{% tab title="Bitrate" %}
### Bitrate

Bitrate is a measure of the amount of data that is transmitted over a network per unit of time. In the context of video calls, bitrate refers to the amount of data that is used to transmit audio and video.

In general, a higher bitrate will result in a better quality video call. However, a higher bitrate also requires more bandwidth. If your user's internet connection does not have enough bandwidth, they may experience choppy or pixelated video.

### How to interpret bitrate values

We measure bitrate in bits per second (bps), and a "good" value can vary depending on whether participants in the call had turned on HD video, if the call was in `normal` or `group` mode, how many participants joined the call, and whether someone was screensharing.&#x20;

Generally, we expect to see at least 500kbps sending bitrate if the participant has their video turned on. If the bitrate is below this value and their video was on, they may have sent blurry or pixelated video, choppy audio, or dropped frames. Our first suspicion would be that the participant has a slow internet connection or a poor quality webcam.

If the participant is using HD video, we expect to see 1 - 1.5Mbps send bitrate.
{% endtab %}
{% endtabs %}

#### **Group and normal rooms**

We calculate the sum of all of the streams for _bitrate sending_, and we display the max packet loss for any stream in _packet loss sending_. This is important to be aware of if you are using `"roomMode": "normal"` rooms.

If you use `normal` rooms, each participant on the call is sending a video stream to every other participant. This means if you have 4 participants in a `normal` room, each participant is sending 3 video streams. Then the _bitrate sending_ chart will be calculated as the sum of those 3 streams and _packet loss sending_ chart will display the stream with the maximum packet loss during the call.&#x20;

Every room created with  `"roomMode": "group"` is using our selective forwarding unit (SFU) mesh for data transfer. This means that every participant is sending only one single video stream to our SFU which then forwards the stream on to every other participant. This is one of [many reasons](https://docs.whereby.com/monitoring-usage/insights-suite-and-api/improving-call-quality#use-group-rooms) why we recommend using `"roomMode": "group"` for better call quality.

## Running into issues?

If you are having difficulty troubleshooting or have questions about the content of the insights data provided, please feel free to engage with us in our [Community Discord](https://docs.whereby.com/#joining-our-developer-community). Diagnosing call quality issues is not the most straightforward endeavor and we are happy to get your feedback so we can improve these features.&#x20;

We also have some information and recommendations in our document about [improving call quality](improving-call-quality.md).

