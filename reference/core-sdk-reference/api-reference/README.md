# API Reference

The WherebyClient is the entry point for this API. Construct it once, and then you can retrieve each of the sub-clients.&#x20;

```jsx
import { WherebyClient } from "@whereby.com/core";

const client = new WherebyClient();
const localMedia = client.getLocalMedia();
const roomConnection = client.getRoomConnection();
```

### Clients

{% content-ref url="wherebyclient.md" %}
[wherebyclient.md](wherebyclient.md)
{% endcontent-ref %}

{% content-ref url="localmediaclient.md" %}
[localmediaclient.md](localmediaclient.md)
{% endcontent-ref %}

{% content-ref url="roomconnectionclient.md" %}
[roomconnectionclient.md](roomconnectionclient.md)
{% endcontent-ref %}

