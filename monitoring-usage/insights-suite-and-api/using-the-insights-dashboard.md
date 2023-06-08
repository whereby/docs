# Using the Insights dashboard

## User agent string

We receive the UA string from each participant's browser. This includes the browser and operating system they were using during the call. You can use this information to determine if there were any compatibility issues, since not all devices or browsers fully support WebRTC.&#x20;

You can find our list of supported browsers and devices here: [https://whereby.helpscoutdocs.com/article/415-supported-devices](https://whereby.helpscoutdocs.com/article/415-supported-devices)

Note that every browser decides how to pass the UA and values can be frozen. For example, your user might be on MacOS Ventura (13.2), but the Safari browser will pass `Intel Mac OS X 10_15_7` as part of the UA. Because of this, we plan to derive browser and OS differently in the future in order to improve the accuracy of the data. Providing the UA to you in the meantime can be helpful as a hint for determining call quality issues.&#x20;

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

Packet loss is a measure of how many data packets sent over a network are lost in transit. Packets are small chunks of data that are used to transmit information over a network.&#x20;

Packet loss can have a significant impact on the quality of video calls. When packets are lost, the video can become choppy or pixelated. In some cases, the video call may even drop altogether.

Packet loss can occur for a variety of reasons, including:

* **Network congestion:** When there is too much traffic on a network, it can cause packets to be dropped.
* **Hardware failure:** A faulty network device, such as a router or switch, can cause packets to be lost.
* **Interference:** Electromagnetic interference from other devices, such as microwaves or cell phones, can also cause packet loss.

### How to interpret packet loss values

The amount of packet loss is measured in percentage. For example, 1% packet loss means that 1 out of every 100 packets were lost. 0% packet loss is ideal for video calls, but some amount of packet loss between 0% - 2% is tolerable and expected.&#x20;

{% hint style="info" %}
We consider 3% packet loss to be our threshold for quality. Any value at or exceeding 3% means that the participant likely experienced reduced call quality.
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

