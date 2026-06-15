# isAudioDenoiserSupported

The audio denoiser ships with `@whereby.com/core` and `@whereby.com/browser-sdk` — there's nothing extra to install. The denoiser code is loaded on demand the first time you use it.

1. Check that noise suppression is supported in the current browser

{% tabs %}
{% tab title="React (browser-sdk)" %}
```jsx
import { isAudioDenoiserSupported } from "@whereby.com/browser-sdk/react";

const [supported, setSupported] = React.useState(false);

// Lazy-loaded; can be called when needed
async function checkAudioDenoiserSupport() {
    setSupported(await isAudioDenoiserSupported());
}
```
{% endtab %}

{% tab title="Core" %}
```jsx
import { isAudioDenoiserSupported } from "@whereby.com/core";

// Lazy-loaded; can be called when needed
const supported = await isAudioDenoiserSupported();
```
{% endtab %}
{% endtabs %}

2. Toggle noise suppression using the room connection

{% tabs %}
{% tab title="React (browser-sdk)" %}
```jsx
import { useRoomConnection } from "@whereby.com/browser-sdk/react";

const { actions } = useRoomConnection(roomUrl, { localMediaOptions: { audio: true, video: true } });
const { enableAudioDenoiser, disableAudioDenoiser } = actions;

// Turn noise suppression on
await enableAudioDenoiser();

// ...or off
await disableAudioDenoiser();
```
{% endtab %}

{% tab title="Core" %}
```jsx
import { WherebyClient } from "@whereby.com/core";

const client = new WherebyClient();
const roomConnection = client.getRoomConnection();

// Turn noise suppression on
await roomConnection.enableAudioDenoiser();

// ...or off
await roomConnection.disableAudioDenoiser();
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Once enabled, the denoiser is automatically re-applied if the microphone device is later switched.
{% endhint %}
