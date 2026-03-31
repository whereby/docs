# Configure OIDC Authentication

If you choose S3 storage with OIDC authentication, there are a few things you’ll need to set up in AWS before configuring your storage in Whereby.

At a high level, this involves:

* Creating an S3 bucket to store recordings and transcriptions
* Creating an IAM role that Whereby can assume using OIDC
* Granting that role permission to access your S3 bucket

{% tabs fullWidth="true" %}
{% tab title="Creating a Bucket" %}
Creating an S3 bucket can be done from the [Amazon S3 console](https://console.aws.amazon.com/console/home) page. If S3 isn't presented as an option you can search for it within the services search at the top of the page.

![](<../../.gitbook/assets/S3 bucket.png>)

For in depth instructions about bucket naming conventions and settings, please follow the Amazon support guide "[Create your first S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/creating-bucket.html)".

**Host:**\
This is also known as your Bucket name. No URL is required, you can simply copy and paste your bucket name found in your S3 console.

![](<../../.gitbook/assets/Bucket name.png>)
{% endtab %}

{% tab title="Create an OIDC identity provider in IAM" %}
You’ll need an IAM OpenID Connect provider that matches your OIDC issuer.

1. Go to IAM.
2. In the left sidebar, select Identity providers.
3. Select Add provider (or Create provider).
4. For Provider type, choose OpenID Connect.
5. Enter the following details:
   * Provider URL: **`https://cognito-idp.eu-west-1.amazonaws.com/eu-west-1_tMvcyDdW4`**&#x20;
   * Audience: **`427ijsob8u270j7sbil5rbnjj1`**

<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

6. Follow the prompts to create the provider.

{% hint style="info" %}
For more in depth information on creating an OIDC identity provider, see this AWS support guide: [Create an OpenID Connect (OIDC) identity provider in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html)
{% endhint %}
{% endtab %}

{% tab title="Create an IAM role" %}
Next you create an IAM role with a trust policy for your OIDC provider, and permissions to your S3 bucket.

1. In IAM, go to Roles.
2. Select Create role.
3. Under Trusted entity type, choose Web identity.

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

4. Configure:
   1. _Identity provid&#x65;_&#x72;: select the OIDC provider you created
   2. _Audience_: select/enter the audience used above
5. Attach permissions (you can do this either by selecting an existing policy or creating a new one). You'll need the following:&#x20;
   1. `s3:PutObject`,&#x20;
   2. `s3:GetObject,`
   3. `s3:ListObject`
6. Select **Next**.
7. Enter:
   1. Role name: e.g. `WherebyS3StorageRole`
8. Select Create role - and copy this Role ARN from the summary page for later.&#x20;

For more in depth information on how to create an IAM role for OIDC authentication, see this AWS support: [Create a role for OpenID Connect federation (console)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp_oidc.html)
{% endtab %}

{% tab title="Configure your storage in Whereby" %}
The final step is to input these values into your Whereby dashboard:&#x20;

1. Visit your organization dashboard
2. Navigate to the Configure section for either Recordings or Transcriptions
3. Select "Self-hosted cloud recording", and change "Connection Method" to **role-based.**&#x20;

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>



4. Enter your bucket name (from step 1) and your Role ARN (from step 3)
5. From here, you can test your connection and then save these credentials to be used during your next Whereby meeting!&#x20;
{% endtab %}

{% tab title="Restrict the role to Whereby's OIDC Sub ID" %}
Once you have added your configuration to the dashboard, we can provide you with the\
exact `sub` value used in our OIDC token for your organization. You can update the IAM role trust policy **to allow only this subject** to assume the role.

1. Get the `sub` value from your Whereby dashboard
   1. Copy the exact `sub` string we provide for your organization (hidden behind the Show Sub ID button).

<figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

2. Open the IAM role
   1. In the AWS Console, go to IAM
   2. In the left sidebar, click Roles
   3. Search for and select the role you created for Whereby
3. Edit the trust policy
   1. Go to the Trust relationships tab
   2. Click Edit trust policy
4. Add the `sub` condition
   1. In the JSON editor, find the statement with:
   2. Under `"Condition"`, extend the `StringEquals` condition for the subject claim:

```json
"Condition": {
  "StringEquals": {
    "<your-oidc-provider-host>:aud": "<your-audience>",
    "<your-oidc-provider-host>:sub": "<whereby-sub-value>"
  }
}
```
{% endtab %}
{% endtabs %}
