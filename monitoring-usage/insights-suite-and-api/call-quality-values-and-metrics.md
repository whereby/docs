# Call quality values and metrics

## User agent string

We receive the UA string from each participant's browser. This includes the browser and operating system they were using during the call. You can use this information to determine if there were any compatibility issues, since not all devices or browsers fully support WebRTC.&#x20;

You can find our list of supported browsers and devices here: [https://whereby.helpscoutdocs.com/article/415-supported-devices](https://whereby.helpscoutdocs.com/article/415-supported-devices)

Note that every browser decides how to pass the UA and values can be frozen. For example, your user might be on MacOS Ventura (13.2), but the Safari browser will pass `Intel Mac OS X 10_15_7` as part of the UA. Because of this, we plan to derive browser and OS differently in the future in order to improve the accuracy of the data. Providing the UA to you in the meantime can be helpful as a hint for determining call quality issues.&#x20;

## Reading the charts

### **Date and time**

All dates and times in the Insights UI and API are in UTC.

### Data intervals

We are capturing values for each chart every 2 seconds. We believe this frequency helps  capture the "spiky" nature of call issues.

### Sending and receiving

We have separated packet loss and bitrate out into separate charts for sending and receiving. Each participant is both sending data in the form of audio and video to others on the call and receiving receiving audio and video data in return.

## **Packet loss**

Packet loss is a measure of how many data packets sent over a network are lost in transit. Packets are small chunks of data that are used to transmit information over a network. When a packet is lost, it means that it did not arrive at its destination.

Packet loss can occur for a variety of reasons, including:

* **Network congestion:** When there is too much traffic on a network, it can cause packets to be dropped.
* **Hardware failure:** A faulty network device, such as a router or switch, can cause packets to be lost.
* **Interference:** Electromagnetic interference from other devices, such as microwaves or cell phones, can also cause packet loss.

Packet loss can have a significant impact on the quality of video calls. When packets are lost, the video stream can become choppy or pixelated. In some cases, the video call may even drop altogether.

### How to interpret packet loss values

The amount of packet loss is measured in percentage. For example, 1% packet loss means that 1 out of every 100 packets are lost. 0% packet loss is ideal for video calls, but some amount of packet loss is tolerable and expected.&#x20;

{% hint style="info" %}
We consider 3% packet loss to be our threshold for quality. Any value at or exceeding 3% means that the participant likely experienced reduced call quality.
{% endhint %}

## Bitrate

Bitrate is a measure of the amount of data that is transmitted over a network per unit of time. In the context of video calls, bitrate refers to the amount of data that is used to transmit the video and audio streams.

In general, a higher bitrate will result in a better quality video call. However, a higher bitrate also requires more bandwidth. If your user's internet connection does not have enough bandwidth, they may experience choppy or pixelated video.

### How to interpret bitrate values

We measure bitrate in bits per second (bps), and a "good" value can vary depending on whether participants in the call had turned on HD video, if the call was in `normal` or `group` mode, how many participants joined the call, and whether someone was screensharing.&#x20;

Generally, we expect to see at least 500 thousand bps for a _bitrate sending stream_ if the participant has their video turned on. If the bitrate is below this value and their video was on, they may have sent blurry or pixelated video, choppy audio, or dropped frames. Our first suspicion would be that the participant has a slow internet connection or a poor quality webcam.

If the participant is using HD video, we expect to see the sending stream around 1 - 1.5 million bps.

## How to improve call quality

**Use `group` rooms**

When creating rooms, we recommend creating rooms that use our selective forwarding mesh (SFU) for data transfer instead of peer-to-peer rooms. This is done by setting `"roomMode": "group"` instead of `"normal"` on room creation.&#x20;

* Our SFU rooms are generally less intensive on network usage, especially when there are more than 2 people in a call.&#x20;
* The mesh can make changes on the fly to participants' calls in order to manage the bandwidth usage of different resources and prioritize audio transmission.
* You will have more ability to troubleshoot and Whereby can better support you when you do run into issues.
* Your calls will benefit from the continuous improvements we make to our SFU mesh.

[**Precall ceremony**](https://docs.whereby.com/customizing-rooms/using-url-parameters#precallceremony-less-than-on-or-off-greater-than)

Turning on the precall ceremony will conduct a live device and connectivity test for the participant before they join the call.

[**Low data mode**](https://docs.whereby.com/customizing-rooms/using-url-parameters#lowdata-less-than-on-or-off-greater-than)

Turning on lowdata mode will set the participant's resolution lower by default.

If you have the precall ceremony turned on and we detect poor connectivity, we also surface an option to the participant to toggle this mode on themselves.

**Troubleshooting best practices for your users**

* Disable any ad-blockers, VPN, or privacy extensions. Occasionally browser extensions can interfere with how video calls load, which can manifest as video, audio, or room joining issues.
* Close any unnecessary applications when on the video call. This will free up resources on the computer and improve the quality of the video call.
* Ensure the browser is up to date. Outdated software may have bugs that can cause packet loss and other problems.
* Move closer to the Wifi router or use a wired connection if one is available, as it is less prone to interference than wireless networks.
* Try joining the call with video turned off.

Our support team has also provided some additional suggestions in our [Troubleshooting guide](../../faq-and-troubleshooting/end-user-documentation.md).

## Running into issues?

If you are having difficulty troubleshooting or have questions about the content of the Insights data provided, please feel free to engage with us in our [Community Discord](https://docs.whereby.com/#joining-our-developer-community). Diagnosing call quality issues is not the most straightforward endeavor and we are very happy to get your feedback so we can improve these features.&#x20;

&#x20;\
