# Improving call quality

## How to improve call quality

#### **Use `group` rooms**

When [creating rooms](../../whereby-rest-api-reference/#create-meeting), we recommend creating rooms that use our selective forwarding unit (SFU) mesh for data transfer instead of peer-to-peer rooms. This is done by setting `"roomMode": "group"` instead of `"normal"` on room creation.&#x20;

* Our SFU rooms are generally less intensive on network usage, especially when there are more than 2 people in a call.&#x20;
* The mesh can make changes on the fly to participants' calls in order to manage the bandwidth usage of different resources and prioritize audio transmission. For example, if a participant decreases the size of their browser window during a call, the SFU will adjust the resolution of the videos they are receiving in order to optimize the quality of the call. &#x20;
* You will have more ability to troubleshoot and Whereby can better support you when you do run into issues.
* Your calls will benefit from the continuous improvements we make to our SFU mesh.

#### **Turn on our device and connectivity test**

Turning on the [pre-call ceremony](https://docs.whereby.com/customizing-rooms/using-url-parameters#precallceremony-less-than-on-or-off-greater-than) will conduct a live device and network connectivity test for the participant before they join the call.&#x20;

Read more about this feature in our [release notes](https://whereby.launchnotes.io/announcements/pre-call-device-connectivity-tests).

#### **Turn on low data mode**

Turning on [low data mode](https://docs.whereby.com/customizing-rooms/using-url-parameters#lowdata-less-than-on-or-off-greater-than) will set the participant's video resolution lower by default. This means there is less data to send and receive, which is helpful with poor network conditions.

We always surface an option to the participant to toggle this mode on themselves before they join the call.&#x20;

## **Recommended troubleshooting steps for end users**

* Disable any ad-blockers, VPN, or privacy extensions. Occasionally browser extensions can interfere with how video calls load, which can manifest as video, audio, or room joining issues.
* Close any unnecessary applications when on the video call. This will free up resources on the computer and improve the quality of the video call.
* Ensure the browser is up to date. Outdated software may have bugs that can cause packet loss and other problems.
* Move closer to the Wifi router or use a wired connection if one is available, as it is less prone to interference than wireless networks.
* Try joining the call with video turned off.

Our support team has also provided some additional suggestions in our [Troubleshooting guide](../../faq-and-troubleshooting/end-user-documentation.md).
