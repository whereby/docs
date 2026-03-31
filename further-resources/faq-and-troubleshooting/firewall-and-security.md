---
description: Configure allowed domains and manage access to Whereby services
---

# Firewall & Security

{% hint style="success" %}
Connections are initiated in the **outbound** direction. There is no requirement for ports to be open inbound and there are no port forwarding requirements. The following information can be set as outbound rules only.
{% endhint %}

<table><thead><tr><th width="174">Service</th><th width="152">Source</th><th width="158" align="center">Destination port</th><th>Protocol</th></tr></thead><tbody><tr><td><a href="firewall-and-security.md#signaling-wss">Whereby signaling (wss)</a></td><td>browser-device</td><td align="center">443</td><td>TCP</td></tr><tr><td>Whereby TURN relay (TCP/TLS)</td><td>browser-device</td><td align="center">443</td><td>TCP</td></tr><tr><td>Whereby TURN relay (UDP)</td><td>browser-device</td><td align="center">443</td><td>UDP</td></tr><tr><td><a href="firewall-and-security.md#turn-sfu-media">Whereby TURN/SFU highport</a></td><td>browser-device</td><td align="center">1024-65535</td><td>UDP</td></tr></tbody></table>

{% hint style="warning" %}
**Note:** For **UDP** entries above, some firewalls cannot enforce rules using domain names; allowlisting may need to be done by **port + IP** rather than FQDN.
{% endhint %}

## Domain Whitelisting (HTTPS/WSS + TURN over TCP/TLS)

If your firewall or proxy supports domain-based rules, you can use the domains below as an allowlist for HTTPS/WSS signaling and TURN over TCP/TLS on port 443. These flows use TLS and typically include hostname information that proxies/firewalls can evaluate.

* `*.sfu.whereby.com`
* `*.sfu.svc.whereby.com`
* `*.srv.whereby.com`
* `*.svc.whereby.com`
* `*.appearin.net`
* `*.turn.whereby.com`
* `*.turn.svc.whereby.com`
* `*.posthog.com`
* `*.sentry.io`
* `*.helpscout.net`
* `*.amazonaws.net` (for user avatars)
* `*.cloudfront.net`

{% hint style="warning" %}
**Important:** Domain/DNS allowlisting is not a reliable trust anchor for ICE media over UDP. ICE connectivity checks and UDP media are generally evaluated by firewalls using IP/port only (no HTTP Host header and typically no hostname information that can be validated against DNS), so a “FQDN-only” egress policy may still block audio/video, even when these domains are allowlisted.
{% endhint %}

## IP Whitelisting

Whereby servers don't currently have a static range of IP addresses. We can provide a list of current server IPs to our annual an enterprise customers upon request. Partner with your dedicated Success Manager or Solutions Engineer to obtain relevant information for your clients and discuss eligibility.

## Additional Info

### Signaling (wss)

Control messages between the clients and Whereby servers when in a call ("signaling") is transmitted over secure websockets (wss). These utilize the same ports as HTTPS, but will set up persistent two-way connections. Proxies and firewalls that intercept HTTPS traffic should be configured to allow websocket traffic towards these hosts/domains:

* `*.sfu.whereby.com`
* `*.sfu.svc.whereby.com`
* `*.srv.whereby.com`
* `*.svc.whereby.com`
* `*.appearin.net`

{% hint style="info" %}
Because signaling uses WSS (TLS over TCP/443), **domain-based allowlisting is typically sufficient** here (firewalls or proxies can evaluate the destination hostname for this traffic).
{% endhint %}

### TURN/SFU (media)

In order to transmit video and audio, participants must be allowed to send and receive packets containing media content. The optimal path for these packets is directly between participants, but where this is not possible or allowed Whereby provides a network of TURN servers that act as relays.

These servers are placed across the globe and participants will connect to the closest ones. Participants will connect to port 443 on these servers, using either UDP or TCP (TLS). For call quality and experience, UDP is the preferred protocol.&#x20;

#### **Firewall note for ICE / UDP media**

For ICE media (especially over UDP), many firewalls **cannot validate the destination by FQDN/DNS** because the traffic is not HTTP and typically does not carry a hostname that can be used as a trust anchor. In practice, this means you must allow **outbound UDP** to the relevant ports and the **resolved IP addresses** of our TURN/SFU infrastructure (or allow broader UDP egress on those ports).

The TURN servers are identified by the following hostname patterns:

* `*.turn.whereby.com`
* `*.turn.svc.whereby.com`
* `turnserver.appearin.net`

{% hint style="warning" %}
**If your network only supports FQDN-based allowlisting:**\
Whereby may still work using **TURN over TCP/TLS (443)** (where hostname-based allowlisting is more feasible), but call quality may be reduced compared to UDP.
{% endhint %}
