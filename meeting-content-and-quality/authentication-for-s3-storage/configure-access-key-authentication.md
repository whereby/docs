# Configure Access Key Authentication

## Setup and information in S3

{% hint style="warning" %}
This feature requires the use of Amazon S3 storage. You can review their plans and pricing on the [AWS site](https://aws.amazon.com/s3/pricing/). Amazon also offers some [free tier](https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank\&all-free-tier.sort-order=asc\&awsf.Free%20Tier%20Types=*all\&awsf.Free%20Tier%20Categories=*all) options to explore. Note that if you're creating an AWS account from scratch it can sometimes take up to 24 hours.
{% endhint %}

{% embed url="https://youtu.be/iLRCdQNK7FY" %}
How to setup Amazon S3 bucket for Cloud Recording storage
{% endembed %}

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

**Key & secret rotation:**

If you wish to rotate the access key at regular intervals, Whereby supports setting new keys on the organization level using our admin UI, or on a per room level using our [REST api](../../reference/whereby-rest-api-reference/).

Please ensure however, that any recording started before updating the key is allowed to finish before invalidating the old key. The following steps should ensure continuous operation:\
\
(One day before invalidation of old key)

1. Create a new access key & secret with identical permissions to the existing one
2. Update cloud recording settings in the admin UI with the new key / update code which uses our REST api to include new key
3. Wait until all existing recordings have completed (max recording duration is 24 hours)
4. Delete / invalidate old key
{% endtab %}
{% endtabs %}
