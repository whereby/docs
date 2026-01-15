# Understanding authentication types

### Access Key authentication

#### Overview

Access Key authentication uses long-lived AWS credentials: an **access key ID** and a **secret access key**. These credentials belong to an IAM user or role in your AWS account and are used directly to authenticate requests to S3.

#### How it works

1. You create an IAM user or role in AWS.
2. That identity is granted permissions to access the target S3 bucket (for example, `s3:PutObject`, `s3:GetObject`).
3. You generate an access key ID and secret access key.
4. These credentials are stored in your configuration and used whenever recordings or transcriptions are uploaded.

#### When to use Access Key auth

Access Key authentication is a good fit when:

* You want a **simple and familiar setup**
* You are working in environments where OIDC is not available
* You already manage IAM users and access keys
* You are integrating with existing infrastructure that relies on static credentials

#### Considerations

* Access keys are **long-lived secrets** and must be stored securely
* Keys must be **rotated manually** to reduce security risk
* If a key is compromised, it must be revoked and replaced

### OIDC (OpenID Connect) authentication

{% hint style="warning" %}
OIDC Authentication is currently in **Early Access** only. Currently, we only support OIDC authentication with **AWS,** with hopes to expand this to other providers in the future. If you'd like to discuss early access or suggest alternate providers, please contact us at embedded@whereby.com.&#x20;
{% endhint %}

#### Overview

OIDC authentication allows the system to assume an AWS IAM role using **short-lived credentials**, without storing long-lived secrets. Authentication is based on trust between AWS and an OIDC identity provider.

#### How it works

1. You configure an OIDC identity provider in AWS.
2. You create an IAM role that trusts that identity provider.
3. The role is granted permissions to access the S3 bucket.
4. When access to S3 is needed, the system exchanges its OIDC identity for **temporary AWS credentials** by assuming the role.
5. These short-lived credentials are used to upload recordings and transcriptions.

#### When to use OIDC auth

OIDC authentication is recommended when:

* You want **improved security** with no long-lived secrets
* You are running in cloud or containerized environments
* You want **automatic credential rotation**
* You follow modern, least-privilege security practices

#### Considerations

* Initial setup is more complex than access keys
* Requires AWS IAM role and OIDC provider configuration
* Not all environments support OIDC out of the box
