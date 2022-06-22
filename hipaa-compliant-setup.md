---
description: >-
  A HIPAA compliant setup comes at no additional cost for customers on an annual
  plan. This page describes how to set up your account in a compliant manner.
---

# HIPAA compliant setup

## How to configure Whereby in a HIPAA compliant manner

The information below should be enough to give you an understanding of what it means to use the HIPAA compliant capabilities of Whereby Embedded and helps you getting started. Follow the steps below, and we'll guide you through the process:

1. Review all the information posted on this page to get an understanding of our HIPAA capabilities and how can they be used.
2. Sign the [Business Associate Agreement](hipaa-compliant-setup.md#business-associate-agreement) by [reaching out to your Whereby contact](https://whereby.com/information/contact-sales/).
3. Finalize the HIPAA compliant setup by following the guidance from your Whereby contact after finishing step 2.&#x20;

![](.gitbook/assets/hipaa-compliant.jpg)

## Pricing and packaging&#x20;

The HIPAA compliant on-demand capability for Whereby Embedded is available only on our [annual plan](https://whereby.com/information/embedded/pricing/). After purchasing an annual plan, make sure to reach out to your Whereby contact for enabling the HIPAA add-on.

## Business Associate Agreement&#x20;

Whereby has prepared and will sign a standard Business Associate Agreement that is adequate for all of our HIPAA compliant customers, at no extra charge. Should there be a need to sign a different BAA than what we offer, we are also open to having our legal counsel review it internally and proceed with it provided there are no issues.

To ensure a Business Associate Agreement is signed, [reach out to your Whereby contact](https://whereby.com/information/contact-sales/) and they will provide the ready to be signed agreement.&#x20;

## End-to-End encryption&#x20;

End-to-End encryption ensures nobody, not even Whereby, can access your call content, thus enabling our customers and their participants to benefit from a fully secured connection.

To ensure End-to-End encryption is active, verify you are using small rooms only, where up to 4 participants are allowed.

Even though this is default behavior, it is specified here to ensure customers have the right settings in place.

To do this, refer to the [Whereby REST API documentation](https://whereby.dev/http-api/#/paths/\~1meetings/post) and set `roomMode = normal`&#x20;

## Encryption in transit&#x20;

By default and through enforcement of our infrastructure's encryption capabilities, Whereby is accessible only via TLS 1.2 with specific ciphers enabled, [as described in our advisories](https://whereby.helpscoutdocs.com/article/710-whereby-tls-cipher-update) and as it can be seen from our [A+ grading on SSL Labs](https://www.ssllabs.com/ssltest/analyze.html?d=whereby.com\&latest).

There is no action required to enable encryption in transit as it is already enabled and available for all customers, regardless of whether they are using the Whereby Embedded HIPAA compliant package or not.&#x20;

## Room names&#x20;

For our HIPAA compliant customers, we recommend the use of random names to avoid accidental usage of [PHI ](https://www.hhs.gov/hipaa/for-professionals/privacy/laws-regulations/index.html)(protected health information) or PII (personal identifiable information). This also ensures there is no pattern in place that can be used to identify the purpose of a meeting.

To do this, refer to the [Whereby REST API documentation](https://whereby.dev/http-api/#/paths/\~1meetings/post) and use `roomNamePattern = uuid`&#x20;

## In-room Chat text&#x20;

The Whereby Embedded chat is HIPAA compliant by default, as information is not stored and is only available for the participants for the duration of the call.

Customers can choose to either rely on the Whereby chat knowing that the information will not be stored nor accessible after the call OR they can build their own on top.

There is no need to do anything to enable the HIPAA compliant in-room chat as it is already enabled and available for all customers, regardless of using the Whereby Embedded HIPAA compliant package or not.

## Recording&#x20;

The recording capability means that the meeting, including audio, video and screen-share content will be captured and saved depending on the options chosen.

Whereby offers the HIPAA compliant capability of [local recording](faq-and-troubleshooting/recording-with-embedded/local-recording.md), where the recording can be initiated by a participant of the call and can be stored locally. The reason it is considered compliant is that Whereby does not, in any way, store or process the recording.

{% hint style="info" %}
Note: _It is the responsibility of the person that initiated the recording and saved it locally to adhere to HIPAA requirements and Whereby cannot control what happens to a local recording._
{% endhint %}

To ensure that recordings are enabled and are only locally processed, refer to the [Whereby REST API documentation](https://whereby.dev/http-api/#/paths/\~1meetings/post) and use `recording.type = local`  and make sure to internally assess if the recording stored locally for those that initiate the recording and save is something that can be accepted as per your HIPAA compliant policies.

Another HIPAA compliant option is to simply **disable the recording capability**, thus removing any kind of concern about the recording and how it is handled afterwards.

To ensure recordings are disabled, refer to the [Whereby REST API documentation](https://whereby.dev/http-api/#/paths/\~1meetings/post) and use `recording.type = none`&#x20;

## Display name&#x20;

The display name for a guest participant is not stored in the Whereby infrastructure, but it is handled temporarily as part of a call, similar to the in-room chat text. The client device communicates the display name when it joins the room and then there is a custom `send_client_metadata` event which is broadcast to all the participants in the room by the Signal server.

To ensure HIPAA compliance, the display name can simply be used as is.

For an added measure of pseudo-anonymization or as a way of more easily integrating Whereby Embedded with your own app the display name for a participant can be preset. This can be useful in several scenarios:

* **Scenario 1:** if you want to use various identifiers for a user rather than the actual name, e.g. instead of `Jane Doe`, it will be `participant 1` , thus adding a pseudo-anonymization measure to fit in with your current setup.
* **Scenario 2:** if you want a seamless integration with with your user handling flow, where e.g. one user is logged into your web app and you would like to pick up their `userId` or `userName` and have it as the display name in Whereby.

To do either of the above, you can refer to the "[Using URL parameters](customizing-rooms/using-url-parameters.md)" section of the Whereby Developer documentation and use the URL parameter `?displayName=` &#x20;

## Audit information

As a covered entities, Whereby Embedded customers may require to audit their suppliers based on their internal policies but also to showcase that they have ensured they are using HIPAA compliant products and services.

To support the HIPAA compliance of our customers, we will gladly provide our ISO27001 certificate and our HIPAA compliance checklist, which documents how Whereby complies with specific HIPAA rules . For further information, [reach out to your Whereby contact](https://whereby.com/information/contact-sales/).

## Whereby features you cannot use when going for a HIPAA compliant setup

There is always a balance between security and usability and this case is no different.&#x20;

If services were to be 100% secure, they would not be usable anymore. Whereby Embedded can be used in a HIPAA compliant setup by our customers, however this comes with clear limitations that need to be in place to have adequate security and compliance with the requirements of the law.&#x20;

* **A maximum of 4 participants**, based on the limitation of the small room. For big rooms, where more than 4 participants can join, the setup will not be HIPAA compliant&#x20;
* **Insights information** - We will stop tracking usage data from meetings, allowing you to stay compliant at the cost of us and yourself having less information when debugging issues. Tracking will be disabled as soon as the on-demand HIPAA compliant setup is activated.
* **Streaming** - Whereby meetings cannot be live streamed (using RTMP) and we also do not envision scenarios where a private, health related discussion will need to be live-streamed. Make sure you don't include any streaming config when you create rooms.
