# VideoView

```jsx
<VideoView stream={participant.stream} />
```

The `VideoView` component renders a `<video>` element within your application.

## Properties

<table><thead><tr><th width="237">Property</th><th width="110">Required</th><th width="156">Type</th><th>Description</th></tr></thead><tbody><tr><td>stream</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td><code>MediaStream</code></td><td>The stream</td></tr><tr><td>muted</td><td></td><td><code>boolean</code></td><td>The mute state of the video element. Defaults to <code>false</code></td></tr><tr><td>mirror</td><td></td><td><code>boolean</code></td><td>When set to <code>true</code>, the video is mirrored. Defaults to <code>false</code>.</td></tr><tr><td>style</td><td></td><td><code>Object</code></td><td>Inline styles to be applied to the <code>&#x3C;video></code> element.</td></tr><tr><td>onResize</td><td></td><td><code>Function</code></td><td>Callback that is triggered when the video element is resized. Reports the video's <code>height</code>, <code>width</code> and <code>stream</code>.</td></tr></tbody></table>

`VideoView` also accepts any other `React.VideoHTMLAttributes`.

## Usage

```tsx
import { useLocalMedia, VideoView } from "@whereby.com/browser-sdk/react";

function SelfView() {
    const localMedia = useLocalMedia({ audio: false, video: true });

    const { localStream } = localMedia.state;

    return (
        <VideoView muted stream={localStream} />
    );
}
```
