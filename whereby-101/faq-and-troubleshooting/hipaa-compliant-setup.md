---
description: >-
  A HIPAA compliant setup comes at no additional cost for customers on an annual
  plan. This page describes how to set up your meeting rooms in a compliant
  manner.
---

# HIPAA compliant setup

{% hint style="info" %}
Our HIPAA compliant setup information was updated in May 2023 to include rooms of all sizes (_including our "group" roomMode_), as well as [cloud recording](hipaa-compliant-setup.md#hipaa-compliant-recording).
{% endhint %}

## Pricing and packaging&#x20;

The HIPAA compliant on-demand capability for Whereby is available only on our [Grow plans](https://whereby.com/information/embedded/pricing). For any questions about pricing options or enabling the HIPAA add-on, reach out to your Whereby contact or [contact sales](https://whereby.com/information/contact-sales).&#x20;

## HIPAA compliance prerequisites

The following information describes what it means to use the HIPAA compliant capabilities of Whereby. Follow the steps below to get started:

1. Review all the information on this page to gain an understanding of our HIPAA capabilities and how can they be used.
2. Sign our [Business Associate Agreement](hipaa-compliant-setup.md#business-associate-agreement) by [reaching out to your Whereby contact](https://whereby.com/information/contact-sales/).
3. Follow the instructions to [create HIPAA compliant rooms](hipaa-compliant-setup.md#creating-hipaa-compliant-rooms).

![](../../.gitbook/assets/hipaa-compliant.jpg)

#### Business Associate Agreement&#x20;

Whereby has prepared and will sign a standard Business Associate Agreement that is adequate for all of our HIPAA compliant customers, at no extra charge. Should there be a need to sign a different BAA than what we offer, we are also open to having our legal counsel review it internally and proceed with it provided there are no issues.

To ensure a Business Associate Agreement is signed, [reach out to your Whereby contact](https://whereby.com/information/contact-sales/) and they will provide the ready to be signed agreement.&#x20;

## Creating HIPAA compliant rooms

Follow the steps below to verify you've created rooms that are HIPAA compliant. Additional information for some points is linked for further clarification.

1. In the API request set `isLocked: true`&#x20;
2. Set the [room name](hipaa-compliant-setup.md#room-names) pattern in the API request `roomNamePattern = uuid`&#x20;
3.  Disable [RTMP streaming](hipaa-compliant-setup.md#features-you-cannot-use-with-a-hipaa-compliant-setup) and room integrations by including the following in your POST creation requests:

    ```
        "roomPreferences": {
          "streaming": false,
          "roomIntegrations": false
      }
    ```
4. Consult our team for any additional security preferences or parameters

{% hint style="success" %}
**Note:** As of May 2023, all room sizes on Whereby are HIPAA compliant. You no longer need to specify "normal" mode to remain compliant.
{% endhint %}

### HIPAA Compliant Recording&#x20;

The recording capability means that the meeting, including audio, video and screen-share content will be captured and saved depending on the options chosen.

{% hint style="success" %}
**Update:** Whereby is proud to now offer HIPAA compliant [cloud recording](../../meeting-content-and-quality/recording-with-embedded/cloud-recording.md), in addition to our previously available HIPAA compliant [local recording](../../meeting-content-and-quality/recording-with-embedded/local-recording.md). To remain HIPPA compliant while using cloud recording, you need to store the recordings in an S3 storage managed by your organisation.
{% endhint %}

The reason our recording options are considered compliant is that Whereby does not, in any way store the recordings.

To ensure that recording is enabled, refer to the [Whereby REST API documentation](../../reference/whereby-rest-api-reference.md) to specify your desired `recording.type` equal to `local` or `cloud`. If you choose `cloud` recording type, make sure to setup your Amazon S3 account and configure the `provider` equal to `s3` as the recordings destination for the meeting room.&#x20;

<details>

<summary>Cloud Recording</summary>

Cloud recording is considered HIPAA compliant as long as the recording files are saved to a customer owned and controlled S3 storage buckets. \
\
We also request that you create a bucket with a specific [bucket policy](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-policies.html) to ensure compliance:

```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "AllowListAndPutRecordings",
			"Effect": "Allow",
			"Principal": {
				"AWS": "arn:aws:iam::<aws_account_id>:user/<aws_user>"
			},
			"Action": [
				"s3:ListBucket",
				"s3:PutObject"
			],
			"Resource": [
				"arn:aws:s3:::<recording_bucket>",
				"arn:aws:s3:::<recording_bucket>/*"
			]
		}
	]
}
```

</details>

<details>

<summary>Local Recording</summary>

It is the responsibility of the person that initiated the recording and saved it locally to adhere to HIPAA requirements. Be sure to assess internally if this is something that can be accept per your HIPAA compliant policies.

Whereby cannot control what happens to a local recording.&#x20;

[Local recording setup and instruction](../../meeting-content-and-quality/recording-with-embedded/local-recording.md#enabling-local-recording)

</details>

The use of recording is not required and if preferred can be disabled entirely. Please refer to the [Whereby REST API documentation](../../reference/whereby-rest-api-reference.md) and use `recording.type = none`&#x20;

### Room names&#x20;

For our HIPAA compliant customers, we recommend the use of random names to avoid accidental usage of [PHI ](https://www.hhs.gov/hipaa/for-professionals/privacy/laws-regulations/index.html)(protected health information) or PII (personal identifiable information). This also ensures there is no pattern in place that can be used to identify the purpose of a meeting.

To do this, refer to the [Whereby REST API documentation](../../reference/whereby-rest-api-reference.md) and use `roomNamePattern = uuid`&#x20;

### Display name&#x20;

The client device communicates the display name when it joins the room and then there is a custom `send_client_metadata` event which is broadcast to all the participants in the room by the Signal server.

To ensure HIPAA compliance, the display name can simply be used as is.

For an added measure of pseudo-anonymization or as a way of more easily integrating Whereby Embedded with your own app, the display name for a participant can be preset. This can be useful in several scenarios:

* **Scenario 1:** if you want to use various identifiers for a user rather than the actual name, e.g. instead of `Jane Doe`, it will be `participant 1` , thus adding a pseudo-anonymization measure to fit in with your current setup.
* **Scenario 2:** if you want a seamless integration with with your user handling flow, where e.g. one user is logged into your web app and you would like to pick up their `userId` or `userName` and have it as the display name in Whereby.

To do either of the above, you can refer to the "[Using URL parameters](../customizing-rooms/using-url-parameters.md)" section of the Whereby Developer documentation and use the URL parameter `?displayName=` &#x20;

## Additional information

### In-room Chat text&#x20;

The Whereby Embedded chat is HIPAA compliant by default, as information is not stored and is only available for the participants for the duration of the call.

Customers can choose to either rely on the Whereby chat knowing that the information will not be stored nor accessible after the call OR they can build their own elsewhere in their platform.

There is no need to do anything to enable the HIPAA compliant in-room chat as it is already enabled and available for all customers, regardless of using the Whereby Embedded HIPAA compliant package or not.

### File sharing

[Sharing files through Whereby chat ](hipaa-compliant-setup.md#file-sharing)is considered to be HIPAA compliant, as files are securely stored and only available to the participants for the duration of the session. All files are permanently deleted within 1 minute from the end of the session or from the moment when there is only 1 participant left in the room. They are not backed up and cannot be retrieved after the session.

There is no need to do anything to enable the HIPAA compliant file sharing as it is already enabled and available for all customers, regardless of using the Whereby Embedded HIPAA compliant package or not.

### Encryption in transit&#x20;

By default and through enforcement of our infrastructure's encryption capabilities, Whereby is accessible only via TLS 1.2 with specific ciphers enabled, [as described in our advisories](https://whereby.helpscoutdocs.com/article/710-whereby-tls-cipher-update) and as it can be seen from our [A+ grading on SSL Labs](https://www.ssllabs.com/ssltest/analyze.html?d=whereby.com\&latest).

There is no action required to enable encryption in transit as it is already enabled and available for all customers, regardless of whether they are using the Whereby Embedded HIPAA compliant package or not.&#x20;

### Audit information

As a covered entities, Whereby Embedded customers may require to audit their suppliers based on their internal policies but also to showcase that they have ensured they are using HIPAA compliant products and services.

To support the HIPAA compliance of our customers, we will gladly provide our ISO27001 certificate and our HIPAA compliance checklist, which documents how Whereby complies with specific HIPAA rules . For further information, [reach out to your Whereby contact](https://whereby.com/information/contact-sales/).

### Features you cannot use with a HIPAA compliant setup

Whereby Embedded can be used in a HIPAA compliant setup by our customers, however this comes with limitations that need to be in place to have adequate security and compliance with the requirements of the law.&#x20;

* **Streaming** - Whereby meetings cannot be live streamed (using RTMP) and we also do not envision scenarios where a private, health related discussion will need to be live-streamed. Make sure you don't include any streaming config when you create rooms.
* **Integrations -** We have integrations built with Miro and YouTube that can be used in Whereby meeting rooms. Because these providers are not HIPAA compliant, you must make sure to disable integrations with [roomIntegrations=off](../customizing-rooms/using-url-parameters.md#roomintegrations) unless you're using another parameter that hides them like ?minimal.
* **Transcriptions and Summaries** - Whereby session transcriptions and summaries are stored in Whereby-provided storage, which is currently not considered to be HIPAA compliant. Avoid using session transcriptions and session summaries to maintain HIPAA compliance of your usage of Whereby.

Return to [creating HIPAA compliant rooms](hipaa-compliant-setup.md#creating-hipaa-compliant-rooms)

