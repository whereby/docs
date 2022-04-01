---
description: >-
  Cloud recording allows you to record your meetings and store those files in a
  storage bucket owned by you and your organization.
---

# Cloud Recording

{% hint style="info" %}
This feature requires the use of Amazon S3 storage. You can review their plans and pricing on the [AWS site](https://aws.amazon.com/s3/pricing/). Amazon also offers some [free tier](https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank\&all-free-tier.sort-order=asc\&awsf.Free%20Tier%20Types=\*all\&awsf.Free%20Tier%20Categories=\*all) options to explore. Note that if you're creating an AWS account from scratch it can sometimes take up to 24 hours.
{% endhint %}

## Setup in Whereby

You can access recording settings and options from the “Configure” → “Recording” section of your organization's dashboard. Select "Cloud Recording" and input your S3 information.

![](<../../.gitbook/assets/recording dashboard.png>)

After setting up the appropriate information, select how you would like to trigger your recording in rooms.&#x20;

* **Auto-start** \
  Recordings will start when the first person joins and end when the last person leaves
* **Prompt host** \
  ****Display a prompt to the host to manually start recording
* **Manual**\
  ****Host will manually start by clicking "Record" in the toolbar

{% hint style="warning" %}
File names will be automatically set to <mark style="color:red;">`[room name]-[HHhMM].mkv`</mark> Currently, we only support the `.mkv` file format.
{% endhint %}

You can also specify recording preferences via the API during the [room creation](https://whereby.dev/http-api/#/paths/\~1meetings/post) request.

## Setup and information in S3

There are a few pieces of information to gather from your S3 instance to create and connect your bucket with Whereby. We'll outline each one briefly below and how to locate the information.

{% tabs %}
{% tab title="Creating a Bucket" %}
Creating an S3 bucket can be done from the [Amazon S3 console](https://console.aws.amazon.com/console/home) page. If S3 isn't presented as an option you can search for it within the services search at the top of the page.

![](<../../.gitbook/assets/S3 bucket.png>)

For in depth instructions about bucket naming conventions and settings, please follow the Amazon support guide "[Create your first S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/creating-bucket.html)".
{% endtab %}

{% tab title="S3 info for Whereby" %}
**Host:**\
This is also known as your Bucket name. No URL is required, you can simply copy and paste your bucket name found in your S3 console.

![](<../../.gitbook/assets/Bucket name.png>)

**Access Key ID & Access Secret:**\
You can locate or create your Access Key and Access secret in the "Identity and Access Management" (IAM) section of your console. You can search for that in the services search, or select "My Security Credentials" from the account dropdown menu.

![](<../../.gitbook/assets/access key s3.png>)
{% endtab %}
{% endtabs %}
