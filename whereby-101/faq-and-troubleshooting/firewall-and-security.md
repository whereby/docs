# Firewall & Security

{% hint style="success" %}
Connections are initiated in the **outbound** direction. There is no requirement for ports to be open inbound and there are no port forwarding requirements. The following information can be set as outbound rules only.
{% endhint %}

<table><thead><tr><th width="174">Service</th><th width="152">Source</th><th width="158" align="center">Destination port</th><th>Protocol</th></tr></thead><tbody><tr><td><a href="firewall-and-security.md#signaling-wss">Whereby signaling (wss)</a></td><td>browser-device</td><td align="center">443</td><td>TCP</td></tr><tr><td>Whereby TURN relay (TCP/TLS)</td><td>browser-device</td><td align="center">443</td><td>TCP</td></tr><tr><td>Whereby TURN relay (UDP)</td><td>browser-device</td><td align="center">443</td><td>UDP</td></tr><tr><td><a href="firewall-and-security.md#turn-sfu-media">Whereby TURN/SFU highport</a></td><td>browser-device</td><td align="center">1024-65535</td><td>UDP</td></tr></tbody></table>

### Domain Whitelisting

If your firewall or proxy requires or allows whitelisting via domain, the following are relevant domains:

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

### IP Whitelisting

Whereby servers don't currently have a static range of IP addresses. We can provide a list of current server IPs to our annual an enterprise customers upon request. Partner with your dedicated Success Manager or Solutions Engineer to obtain relevant information for your clients and discuss eligibility.

### Additional Info

#### Signaling (wss)

Control messages between the clients and Whereby servers when in a call ("signaling") is transmitted over secure websockets (wss). These utilize the same ports as HTTPS, but will set up persistent two-way connections. Proxies and firewalls that intercept HTTPS traffic should be configured to allow websocket traffic towards these hosts/domains:

* `*.sfu.whereby.com`
* `*.sfu.svc.whereby.com`
* `*.srv.whereby.com`
* `*.svc.whereby.com`
* `*.appearin.net`

#### TURN/SFU (media)

In order to transmit video and audio, participants must be allowed to send and receive packets containing media content. The optimal path for these packets is directly between participants, but where this is not possible or allowed Whereby provides a network of TURN servers that act as relays.

These servers are placed across the globe and participants will connect to the closest ones. Participants will connect to port 443 on these servers, using either UDP or TCP. For call quality and experience, UDP is the preferred protocol. The TURN servers are identified by the hostname patterns:

* `*.turn.whereby.com`
* `*.turn.svc.whereby.com`
* `turnserver.appearin.net`
