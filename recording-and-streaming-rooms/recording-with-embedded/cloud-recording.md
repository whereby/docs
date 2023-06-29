---
description: >-
  Cloud recording allows your users to record their meetings and save the
  recordings to a storage bucket accessible by you.
---

# Cloud Recording

## Storage options

We offer two storage options for our cloud recording feature.&#x20;

### Whereby-provided storage

By choosing Whereby-provided storage for recordings, you can can take full advantage of the flexibility of cloud recording without the need to configure and maintain your own Amazon S3 bucket for media storage.

Depending on your plan, you will have some Whereby-provided storage quota available for free. For example, Whereby Build customers have 30 GB days of storage included in their plan. They can store 1 GB of recordings for 30 days, 2 GB of recordings for 15 days or 30 GB of recordings for 1 day each month for free, at no additional cost.

There is an additional storage fee applied to recordings saved in Whereby-provided storage above the free storage quota available in your plan. You can check the current price of Whereby-provided storage as well as available free storage quota on [Whereby's pricing page](https://whereby.com/information/embedded/pricing) or in the "Subscription" section of your customer portal.  &#x20;

Whereby would not delete your media files automatically, so we recommend that you periodically delete the obsolete recordings from Whereby storage or download them to the storage of your choice.

You can delete or download the recordings either through the "Recordings" page in Whereby customer portal or using our [Recordings API](https://docs.whereby.com/whereby-rest-api-reference#recordings).

### Your Amazon S3 bucket

You may also choose to store cloud recordings in an Amazon S3 bucket owned and managed by you. This is more technical to set up, but may suite your business needs better if you already own and manage Amazon S3 storage.&#x20;

## Setup

You can access recording settings and options from the “Configure” → “Recording” section of your customer portal. You can also specify recording preferences via the API during a [room creation](../../whereby-rest-api-reference/#create-meeting) request.

{% hint style="info" %}
When configuring cloud recording options via the dashboard, it will apply these as default settings for all rooms created. However, you can override the defaults by specifying different preferences within the POST requests used to create meetings.
{% endhint %}

![](<../../.gitbook/assets/Screenshot 2023-06-26 at 13.17.15 (1).png>)

After setting up the appropriate information, select how you would like to trigger your recording in rooms.&#x20;

* **Auto-start (1 person)**\
  Recordings will start when the first person joins and end when the last person leaves
* **Auto-start (2 people)**\
  Recordings will start when 2 people join a room and end when the last person leaves
* **Prompt host** \
  Display a prompt to the host to manually start recording
* **Manual**\
  Host will manually start by clicking "Record" in the toolbar

### Recording format

You can choose the format in which you want to save the recordings. Currently you can select either <mark style="color:blue;">`.mkv`</mark> or <mark style="color:blue;">`.mp4`</mark> file formats.

Recording quality is set to 720p, however individual video feeds may appear in lower quality depending on their network connection, device, and preferences.

![](<../../.gitbook/assets/Recording format 2.png>)

{% hint style="info" %}
File names will be automatically set to <mark style="color:red;">`[room name]-[start time in ISO format].mkv`</mark> and is the time when the recording started.
{% endhint %}

### Managing recording activity

You can use our [webhook events](../../monitoring-usage/webhooks.md#data-properties) to track when a recording has started and stopped. The roleName will be set to recorder, and will use the `room.client.joined` and `room.client.left` events accordingly.

<figure><img src="../../.gitbook/assets/recorder webhook.png" alt=""><figcaption></figcaption></figure>

In addition to tracking recordings, we offer [browser methods](https://docs.whereby.com/embedding-rooms/in-a-web-page/using-the-whereby-embed-element#sending-commands) to start and stop recordings at your leisure via our SDK's Embed Element.&#x20;

## Downloading from Whereby-provided storage

If you choose to save your cloud recordings in Whereby-provided storage you can delete or download them manually through you Whereby customer portal or using our [Recordings API](https://docs.whereby.com/whereby-rest-api-reference#recordings).

You can access all recordings saved in the Whereby-provided storage from the “Recordings” page of your customer portal.

<figure><img src="../../.gitbook/assets/Screenshot 2023-06-27 at 13.09.27.png" alt=""><figcaption></figcaption></figure>



## Setup and information in S3

{% hint style="warning" %}
This feature requires the use of Amazon S3 storage. You can review their plans and pricing on the [AWS site](https://aws.amazon.com/s3/pricing/). Amazon also offers some [free tier](https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank\&all-free-tier.sort-order=asc\&awsf.Free%20Tier%20Types=\*all\&awsf.Free%20Tier%20Categories=\*all) options to explore. Note that if you're creating an AWS account from scratch it can sometimes take up to 24 hours.
{% endhint %}

{% embed url="https://youtu.be/iLRCdQNK7FY" %}

If you choose to use your own storage, there are a few pieces of information to gather from your S3 instance to create and connect your bucket with Whereby. We'll outline each one briefly below and how to locate the information.

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
