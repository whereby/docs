# Authentication for S3 Storage

### Overview

{% hint style="info" %}
If you're using **Whereby-hosted** storage, you can skip this guide. This only applies to **Self-hosted** cloud recordings and session transcriptions.&#x20;
{% endhint %}

When storing cloud recordings and session transcriptions in S3, the system must authenticate with your S3 bucket to upload and manage objects securely.

We support two authentication methods:

* Access Key authentication
* OIDC (OpenID Connect) authentication

Both methods allow access to the same S3 functionality, but they differ in how credentials are managed and how they are configured.

This documentation explains:

* What each authentication type is and how it works
* When to use each method
* How to configure both Access Key and OIDC authentication

### At a glance

* Access Key authentication uses long-lived AWS credentials (access key ID and secret access key).
* OIDC authentication uses short-lived credentials by assuming an IAM role via an OpenID Connect identity provider.

For most production environments, **OIDC authentication is recommended.**

{% content-ref url="understanding-authentication-types.md" %}
[understanding-authentication-types.md](understanding-authentication-types.md)
{% endcontent-ref %}

{% content-ref url="configure-access-key-authentication.md" %}
[configure-access-key-authentication.md](configure-access-key-authentication.md)
{% endcontent-ref %}

{% content-ref url="configure-oidc-authentication.md" %}
[configure-oidc-authentication.md](configure-oidc-authentication.md)
{% endcontent-ref %}
