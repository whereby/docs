# Web app quickstart

## Prerequisites

This guide assumes that you’ve already created a Whereby Embedded account and a meeting room to join with your web app. If you’ve not already taken these steps, you can do so by following the [Whereby Embedded initial setup guide](../../getting-started/overview-of-whereby-embedded/initial-setup.md).

## Setting up the app

Our sample app uses vanilla HTML, CSS, and JavaScript. You can create your own sample files to follow along with the tutorial or download our [web app starter template](https://github.com/whereby/demos/tree/main/webapp-template) to use as a starting point.

{% hint style="info" %}
**Note**: You can also find our [finished web component quickstart web app](https://github.com/whereby/demos/tree/main/web-component-quickstart) on GitHub.
{% endhint %}

## Using the component

The prebuilt web component is available as part of the [Whereby Browser SDK](https://www.npmjs.com/package/@whereby.com/browser-sdk), published on npm. You can include it in your app via a package manager, such as npm or yarn, and install instructions can be found [here](../react-based-browser-sdk/quick-start.md).

### Install methods

To install the web component, there are different methods that can be used. You can either use a `<script>` element, import via the package manager, or by importing the SDK.&#x20;

#### Using a \<script> element

For simple apps like this one, however, a package manager can be overkill. You can also include it via a `<script>` element with the source set to point to our CDN.

Add the following to the `<head>` of your HTML document:

```html
<script src="https://cdn.srv.whereby.com/embed/v3-embed.js" type="module">
</script>
```

#### Using a package manager

When using React or a bundler like Webpack, Rollup, Parcel, etc., you can install the Whereby Browser SDK in your project using npm:

```bash
npm install @whereby.com/browser-sdk
```

#### Importing the SDK

With the browser SDK included in your project, you can then import it as follows:

```javascript
import "@whereby.com/browser-sdk/embed"
```

### How to use

After installation, add the following to your HTML document’s `<body>`, substituting `YOUR-ROOM-URL` for the room URL you obtained in the initial setup guide:

```html
<whereby-embed
  room="YOUR-ROOM-URL">
</whereby-embed>
```

{% hint style="info" %}
**Note**: You can use a `hostRoomUrl` instead of the `roomUrl`, if you want to give the user [host privileges](../../whereby-product-features/user-roles-and-privileges.md#hosts).
{% endhint %}

To ensure the component fits the page, we’ll set it to fill the entire viewport for now. Add the following rules to your web app’s CSS:

```css
:root {
  height: 100%;
  margin: 0;
  overflow: hidden;
}

body,
whereby-embed {
  height: inherit;
  margin: inherit;
}
```

Load your sample app in a web browser. You should see the Whereby interface appear, allowing you to enter your name and join the meeting room.

## Customizing your app

Now that you’ve got a basic app running, you’ll want to customize it to your liking. Whereby’s web component has many options for customizing your app:

* [Global settings](../../whereby-product-features/dashboard-preferences/) via your account dashboard.
* Per-room settings via component [attributes](configuring-with-attributes.md) and [room creation settings](../../whereby-product-features/using-the-rest-api/).
* Creating custom behavior and controls using [commands](using-commands.md) and [events](using-whereby-events.md).

Our next steps are to explore a couple of the customization options that Whereby has to offer. Subsequent sections will explain all the options in full.&#x20;

### Setting custom brand colors

1. In your account dashboard: Go to the **Configure** screen and access the **Appearance** section.
2. Set new primary, secondary, and focus highlight colors.
3. Set a new background color, and upload a background image.
4. Upload a company logo.
5. Reload your sample app to see how these settings change the look and feel.

### Setting web component attributes

There are many different attributes you can set on the web component to configure various aspects of the video call experience. Let's set a couple now. Update the component as follows:

```html
<whereby-embed
  room="YOUR-ROOM-URL"
  cameraEffect="blur"
  minimal="on"
  logo="off">
</whereby-embed>
```

The effect of the [attributes](../../reference/using-the-whereby-embed-element.md#attributes-of-the-component) is as follows:

* `cameraEffect="blur"`: Sets a blur effect on the camera background by default.
* `minimal="on"`: Uses the minimal Whereby UI.
* `logo="off"`: Hides the company logo.

Save your code and reload your app to see the effect of the attributes.

You’ve just built a working video calling app with Whereby Embedded and customized it with your own branding and layout. Next, we'll explore advanced options like event handling and custom controls.

## See also

* [Meeting controls](https://whereby.helpscoutdocs.com/article/458-meeting-controls)
