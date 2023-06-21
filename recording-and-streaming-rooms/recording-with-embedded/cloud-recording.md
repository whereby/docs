---
description: >-
  Cloud recording allows you to record your meetings and store those files in a
  storage bucket owned by you and your organization.
---

# Cloud Recording

{% embed url="https://youtu.be/iLRCdQNK7FY" %}

{% hint style="warning" %}
This feature requires the use of Amazon S3 storage. You can review their plans and pricing on the [AWS site](https://aws.amazon.com/s3/pricing/). Amazon also offers some [free tier](https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank\&all-free-tier.sort-order=asc\&awsf.Free%20Tier%20Types=\*all\&awsf.Free%20Tier%20Categories=\*all) options to explore. Note that if you're creating an AWS account from scratch it can sometimes take up to 24 hours.
{% endhint %}

## Setup in Whereby

You can access recording settings and options from the “Configure” → “Recording” section of your organization's dashboard. You can also specify recording preferences via the API during a [room creation](../../whereby-rest-api-reference.md#create-meeting) request.

{% hint style="info" %}
When configuring cloud recording options via the dashboard, it will apply these as default settings for all rooms created. However, you can override the defaults by specifying different preferences within the POST requests used to create meetings.
{% endhint %}

Select "Cloud Recording" and input your S3 information.

![](<../../.gitbook/assets/recording dashboard.png>)

After setting up the appropriate information, select how you would like to trigger your recording in rooms.&#x20;

* **Auto-start (1 person)**\
  Recordings will start when the first person joins and end when the last person leaves
* **Auto-start (2 people)**\
  Recordings will start when 2 people join a room and end when the last person leaves
* **Prompt host** \
  Display a prompt to the host to manually start recording
* **Manual**\
  Host will manually start by clicking "Record" in the toolbar

Then select the format that you'd like the recordings to be saved as, we currently offer <mark style="color:blue;">`.mkv`</mark> and <mark style="color:blue;">`.mp4`</mark>. Recording quality is set to 720p, however individual video feeds may appear in lower quality depending on their network connection, device, and preferences.

![](<../../.gitbook/assets/Recording format 2.png>)

{% hint style="info" %}
File names will be automatically set to <mark style="color:red;">`[room name]-[start time in ISO format].mkv`</mark> and is the time when the recording started.
{% endhint %}

### Managing recording activity

You can use our [webhook events](../../monitoring-usage/webhooks.md#data-properties) to track when a recording has started and stopped. The roleName will be set to recorder, and will use the `room.client.joined` and `room.client.left` events accordingly.

<figure><img src="../../.gitbook/assets/recorder webhook.png" alt=""><figcaption></figcaption></figure>

In addition to tracking recordings, we offer [browser methods](https://docs.whereby.com/embedding-rooms/in-a-web-page/using-the-whereby-embed-element#sending-commands) to start and stop recordings at your leisure via our SDK's Embed Element.&#x20;

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
You can locate or create your Access Key and Access secret in the "Identity and Access Management" (IAM) section of your console.&#x20;

{% hint style="info" %}
It's recommended and considered best practice to create IAM users with the appropriate permissions for your AWS Access Keys. Sharing the credentials of a Root user can allow unrestricted access to all resources in your AWS account, including billing information.\
\
For more information, check out Amazon's support article:\
[Best practices for managing AWS access keys](https://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html#root-password)
{% endhint %}



![Where to create Access Keys](<../../.gitbook/assets/access key s3.png>)
{% endtab %}
{% endtabs %}
