---
description: >-
  This guide walks you through implementing essential features for your
  telehealth platform using Whereby's video API and SDK.
---

# Building secure telehealth applications: implementation guide

Whether you're building a new telehealth application or enhancing an existing platform, this guide covers the critical components needed to deliver secure, accessible, and user-friendly video consultations.

{% hint style="success" %}
**Did you know?** Healthcare organizations lose an estimated 40% of potential patients due to technical barriers during video consultations. Poor video quality, accessibility issues, and complex user flows directly impact patient satisfaction scores, provider efficiency, and ultimately, clinical outcomes.&#x20;
{% endhint %}

The guide focuses on the four most important components identified across successful telehealth implementations: reliable video infrastructure, accessibility compliance, user experience optimization, and integration capabilities that support clinical workflows.

### Prerequisites

Before implementing the features covered in this guide, ensure you have:

* A Whereby Embedded account with API access
* Basic knowledge of JavaScript/React development
* Understanding of HIPAA compliance requirements for healthcare applications
* Familiarity with web accessibility standards (WCAG 2.1 Level AA)
* Development environment set up with Node.js and npm/yarn

**Required Setup:**

* Node.js 14+ installed
* A text editor or IDE
* Web browser for testing
* HTTPS-enabled development environment (required for video features)

### Getting Started

This guide assumes that you’ve already created a Whereby embedded account and a meeting room to join with your web app. If you’ve not already taken these steps, you can do so by following the [Whereby Embedded initial setup guide](../getting-started/overview-of-whereby-embedded/initial-setup.md).

### Implementing Core Telehealth Features

#### Accessibility-First Design Implementation

Improving accessibility in telehealth design enhances usability for all users. Telehealth platforms must be inclusive of patients with disabilities, and Whereby is WCAG 2.1 Level AA compliant. This section highlights the implementation of WCAG 2.1 Level AA compliance features to boost universal usability.

**Core Accessibility Features:**

1.  **Enable** [**live captions**](../whereby-product-features/live-captions.md) **and** [**transcriptions**](../whereby-product-features/transcribing/):\
    \
    If you want to use Live Captions or Transcriptions for _all_ of your meetings, you can enable it globally for your account. Go to “Configure” → “Transcription” section of your Dashboard and enable accordingly.\
    \
    You can enable these on a per-room basis by making a [POST /meetings](../reference/whereby-rest-api-reference/meetings.md#post-meetings) request. Here is an example for enabling Live Captions:<br>

    ```
    "liveTranscription": {
        "startTrigger": "none",
        "liveCaptions": true
    }
    ```
2. **Implement keyboard navigation**:\
   \
   Keyboard shortcuts can be enabled (or disabled) within the settings of a Whereby room. Enter the room, click the Settings cogwheel icon, navigate to the Advanced settings tab, and toggle on. Users can also disable our single-key shortcuts if they prefer, as these might interfere with other shortcuts users have set.\
   \
   You can find our list of keyboard shortcuts in our [Accessibility guide](../further-resources/faq-and-troubleshooting/accessibility.md).&#x20;
3. **Configure highlight speaker features**:\
   \
   The highlight active speaker feature is especially helpful for users who rely on visual cues. It visually identifies who’s speaking, making it easier to follow conversations.

```javascript
<whereby-embed
  room="YOUR-ROOM-URL"
  highlightActiveSpeaker="on"
</whereby-embed>
```

{% hint style="warning" %}
**Implementation Note:** Accessibility features should be enabled by default rather than opt-in to ensure inclusive healthcare delivery.
{% endhint %}

#### Advanced UI/UX Customization for Clinical Workflows

By default, Whereby is built with patient engagement in mind because we want our partners to experience ease when it comes to managing their clients’ virtual care.

{% hint style="success" %}
**Did you know?** The design of your telehealth interface directly impacts clinical effectiveness and patient trust. Studies show that patients form trust judgments about healthcare providers within the first 30 seconds of a video consultation, with interface design playing a crucial role.&#x20;
{% endhint %}

Effective telehealth interfaces require careful attention to visual hierarchy and user experience that supports clinical decision-making rather than hindering it.

**Customizing Video Layouts:**

1. **Implement minimalist clinical interface**:

```javascript
<whereby-embed
  room="YOUR-ROOM-URL"
  minimal="on"
  chat="on"
  leaveButton="on"
  screenshare="on"
  people="off"
  pipButton="off"
  settingsButton="off">
</whereby-embed>
```

2. **Configure video fatigue reduction**:

```javascript
<whereby-embed
  room="YOUR-ROOM-URL"
  autoHideSelfView="on"
  cameraEffect="blur">
</whereby-embed>
```

#### Waiting Room and Patient Flow Management

With Whereby's virtual Waiting Room feature, you can mirror the experience of traditional medical office waiting rooms.

**Setting Up Virtual Waiting Rooms:**

1. **Configure waiting room**:\
   You can create a waiting room experience for your participants by making sure the room is created as **locked** during the API request. You do this by setting the `isLocked` property to **true**:

```javascript
// Example Request
{
  "isLocked": true,
  "roomNamePrefix": "waiting-room-example-",
  "roomNamePattern": "uuid",
  "roomMode": "group",
  "endDate": "2024-07-05T18:10:34.695Z",
  "fields": [
    "hostRoomUrl"
  ]
}
```

```
// Example Response
{
  "startDate": "2024-06-21T18:10:35.056Z",
  "endDate": "2024-07-05T18:10:34.695Z",
  "roomName": "/waiting-room-example-546f6c91-74b2-4597-b32a-6aeefa9ed90f",
  "roomUrl": "https://subdomain.whereby.com/waiting-room-example-546f6c91-74b2-4597-b32a-6aeefa9ed90f",
  "meetingId": "87673375",
  "hostRoomUrl": "https://subdomain.whereby.com/waiting-room-example-546f6c91-74b2-4597-b32a-6aeefa9ed90f?roomKey=eyJhbGciOiJIU..."
}
```

{% hint style="warning" %}
Make sure to include a hostRoomUrl in the request. Users provided with a hostRoomUrl will have the ability to join a locked room, as well as accept and reject participants knocking at the room.
{% endhint %}

2. **Set the Waiting Room background**:\
   Make an API request to: [https://api.whereby.dev/v1/rooms/{roomName}/theme/room-knock-page-background](https://api.whereby.dev/v1/rooms/%7BroomName%7D/theme/room-knock-page-background)

```javascript
const response = await fetch('https://api.whereby.dev/v1/rooms/{roomName}/theme/room-knock-page-background', {
    method: 'PUT',
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify({
      "palette": "default",
      "theme": "default"
    })
});

const data = await response.json();
```

3. **Add meeting timers for clinical efficiency**:

```javascript
<whereby-embed
  room="YOUR-ROOM-URL"
  timer="on"
</whereby-embed>
```

4. **Utilize our webhooks to create a queuing system.**\
   See more here: [webhooks.md](../whereby-product-features/insights-suite-and-api/webhooks.md "mention")

### Recording and Transcription

Clinical documentation is a critical but time-consuming aspect of healthcare delivery. Manual transcription of telehealth consultations is not only labor-intensive but also prone to errors that can impact patient care continuity and regulatory compliance. Healthcare organizations implementing automated clinical documentation see an improvement in provider satisfaction and significant reduction in after-hours administrative work.<br>

1. **Set up Cloud Recording**:\
   To ensure that recording is enabled, refer to the [Whereby REST API documentation](https://docs.whereby.com/reference/whereby-rest-api-reference) to specify your desired `recording.type` equal to  `cloud`. If you choose `cloud` recording type, we recommend that you setup your Amazon S3 account and configure the `provider` equal to `s3` as the recordings destination for the meeting room.
2. **Set up Session Transcriptions:** \
   Create the room with [POST /meetings](https://docs.whereby.com/reference/whereby-rest-api-reference/meetings) request and specify the transcription options of your choice. In the `"destination.provider"` option, we recommend that you choose  `"s3"`. Please refer to the [POST /meetings](https://docs.whereby.com/reference/whereby-rest-api-reference/meetings) API reference docs for further `"destination"` configuration options.
3. **Handle recording consent and privacy**:\
   We recommend that the person who is starting the recording (i.e the host) to get consent from all participants before starting a recording. This can be a simple verbal consent upon the meeting starting.

### Security and Compliance Implementation

#### HIPAA Compliance Configuration

Ensure your telehealth platform meets healthcare privacy and security requirements while maintaining optimal user experience. HIPAA compliance isn't optional; it's a fundamental requirement that affects every aspect of your telehealth platform's design and operation. Security features also become competitive advantages when marketing to enterprise healthcare clients.

1. **Configure secure rooms:**
   1. In the API request set `isLocked: true`
   2. Set the [room name](https://docs.whereby.com/whereby-101/faq-and-troubleshooting/hipaa-compliant-setup#room-names) pattern in the API request `roomNamePattern = uuid`
   3.  Disable [RTMP live streaming](https://docs.whereby.com/whereby-101/faq-and-troubleshooting/hipaa-compliant-setup#features-you-cannot-use-with-a-hipaa-compliant-setup) and room integrations by including the following in your POST creation requests:



       ```
           "roomPreferences": {
             "streaming": false,
             "roomIntegrations": false
         }
       ```
2. **Encryption**:\
   There is no action required to enable encryption in transit as it is already enabled and available for all customers, regardless of whether they are using the Whereby Embedded HIPAA compliant package or not.
3.  **Audit information**:\
    Whereby Embedded customers may require to audit their suppliers based on their internal policies but also to showcase that they have ensured they are using HIPAA compliant products and services.<br>

    To support the HIPAA compliance of our customers, we will gladly provide our ISO27001 certificate and our HIPAA compliance checklist, which documents how Whereby complies with specific HIPAA rules . For further information, [reach out to your Whereby contact](https://whereby.com/information/contact-sales/).
4. **More HIPAA-compliancy:** \
   We have a more in-depth guide on having a HIPAA compliant setup in the next guide, [HIPAA compliant setup](hipaa-compliant-setup.md).

### Performance Optimization for Healthcare

Healthcare delivery requires consistent performance across diverse network conditions and geographic locations, ensuring that critical medical consultations aren't compromised by technical limitations. We have several features to ensure quality connections.&#x20;

1. **Optimize for low-bandwidth scenarios**:\
   Whereby has an **audio-only mode** so users can join sessions with their videos turned off to help minimize bandwidth usage during poor network conditions. This way, the appointment can still be facilitated.
2.  **Implement connection monitoring**:\
    Turning on the pre-call ceremony will conduct a live device and network connectivity test for the participant before they join the call.\
    \
    For example, the following component would ensure users see the pre-call review step and see the pre-call device and connectivity test: <br>

    ```
    <whereby-embed 
        room="https://subdomain.whereby.com/your_room?roomKey=3fe345a"
        precallReview=on
        precallCeremony=on
    />
    ```

    \
    To better understand issues _during_ ongoing calls, we also have an in-call diagnostics feature. With this, you get a panel that provides a detailed overview of every participant’s meeting quality during sessions.\
    \
    To use this feature, you can use the following component:\
    `callQualityMonitoring=on`
3. **Global mesh network utilization**:\
   Whereby has a global mesh network that connects users to their closest servers to ensure reliable connections, even in low-bandwidth scenarios. This is done by setting `"roomMode": "group"` instead of `"normal"` on room creation.

### See Also

* [Whereby Embedded API Reference](../reference/whereby-rest-api-reference/) - Complete API documentation
* [React Hooks SDK Guide](../reference/react-hooks-reference/) - Advanced React implementations
* [HIPAA Compliance Documentation](hipaa-compliant-setup.md) - Healthcare-specific compliance features
* [Accessibility Features Reference](../further-resources/faq-and-troubleshooting/accessibility.md) - Complete accessibility implementation guide
* [Session Transcriptions](../whereby-product-features/transcribing/session-transcription.md) - Guide to our Session Transcriptions feature
* [Telehealth Tutorial App](https://github.com/whereby/sdk/tree/main/apps/telehealth-tutorial-app) - Example app that demonstrates how to use the Whereby browser SDK to create a telehealth application
