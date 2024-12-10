# Usage with Next.js

Next.js is a popular framework for React and is rendered server-side. The Whereby browser-sdk relies on browser specific API’s to work and because of this, usage with Next.js has some challenges.&#x20;

The default way of building Next.js app currently uses an [app router architecture](https://nextjs.org/docs/app/building-your-application/routing#the-app-router). Conceptually, the in-room functionality of a Whereby call happens in a single route (there should be no reloads in the middle of a call). This means:

1. We need to dedicate a single page route for the call
2. Make sure the page is running on the `client,` in order have access to the necessary browser API’s.&#x20;

{% hint style="info" %}
In Next.js, every component is a server component by default. You need to opt-out to make use of browser specific API’s. This can be done by adding `“use client”` directive at the top of a file, above the imports.
{% endhint %}

Read more about client components in the Next.js documentation [here](https://nextjs.org/docs/app/building-your-application/rendering/client-components). Unfortunately, this is not enough for our use case. Client components in Next.js are still pre-rendered by default, and we need to opt-out of that as well, to be able to access all the browser API’s we need. We can do this with `next/dynamic`. See more info [here](https://nextjs.org/docs/app/building-your-application/optimizing/lazy-loading#nextdynamic).

### Provider

We need a “client” component that will setup our connection to Whereby for us, and provide us with the global Whereby state.

`components/provider.tsx`

```tsx
"use client";

import * as React from "react";

import { WherebyProvider } from "@whereby.com/browser-sdk/react";

function Provider({ children }: { children: React.ReactNode }) {
    return (
        <WherebyProvider>
            <>{children}</>
        </WherebyProvider>
    );
}

export default Provider;

```

The next step is to include this provider anywhere in the app, but it needs to be above (a parent of), any component that will utilize the Whereby SDK. The root level `layout.tsx` is a natural place to put it, and that gives all pages and components access to it. We need to import it using Next.js dynamic import, like this:

`app/layout.tsx`

```tsx
const Provider = dynamic(() => import("../components/provider"), { ssr: false });
```



### The room component

We can now create a room component, this is a minimal example:

`components/room.tsx`

```tsx
"use client";

import * as React from "react";

import { useRoomConnection, VideoGrid } from "@whereby.com/browser-sdk/react";

interface Props {
    roomUrl: string;
}

function Room({ roomUrl }: Props) {
    const { actions } = useRoomConnection(roomUrl, { localMediaOptions: { audio: true, video: true } });
    const { joinRoom, leaveRoom } = actions;

    React.useEffect(() => {
        joinRoom();
        return () => leaveRoom();
    }, [joinRoom, leaveRoom]);

    return (
        <div className={"w-full h-full"}>
            <VideoGrid />
        </div>
    );
}

export default Room;

```

As with the provider, this also needs to be a client component. To see more examples of how to customize the room layout,  you can see a more in-depth guide on that [here.](../../../whereby-101/create-your-video/in-a-web-page/using-whereby-react-hooks-build-a-telehealth-app.md)

### Using the room component

Now we can import and use the room component anywhere in our app. The important thing to remember is that we need to use Next.js’s `dynamic` import, as explained in the beginning of this guide.

`app/page.tsx`

```tsx
import * as React from "react";
import dynamic from "next/dynamic";

const Room = dynamic(() => import("../components/room"), { ssr: false });

// Replace this with your own Whereby room URL
const roomUrl = "";

export default function Home() {
    return (
        <main className="h-screen w-screen">
            <Room roomUrl={roomUrl} />
        </main>
    );
}

```

That should be all that’s needed to run the Whereby browser-sdk in Next.js. We also have an example implementation of this in our [repository](https://github.com/whereby/sdk/tree/main/apps/nextjs-example), where you can see a working example of a setup with Next.js and the Whereby browser-sdk.
