# Configuring with attributes

This article takes an in-depth look at the attributes available to the `<whereby-embed>` element. These allow you to customize the meeting experience for each user after the room has been created, for example, by enabling and disabling Whereby functionality, setting user avatars and room backgrounds, and more.&#x20;

{% hint style="info" %}
Note that the attributes are _only_ compatible with Whereby's web component and pre-built UI and will not work with the React Hooks SDK.
{% endhint %}

### Prerequisites

This guide assumes that you’ve already created a Whereby Embedded account and a meeting room, and have joined it by adding the `<whereby-embed>` component to a web app.

* If you’ve not created a Whereby Embedded account and a meeting room, you can do so by following the [Whereby Embedded initial setup guide](../../getting-started/overview-of-whereby-embedded/initial-setup.md).
* If you've not created a video call web app using `<whereby-embed>`, you can clone our [finished pre-built component quickstart web app](https://github.com/whereby/demos/tree/main/web-component-quickstart) from GitHub, or you can build the app yourself by following our [Web app quickstart](web-app-quickstart.md).

### Getting started

1. To begin with, open your web app in your web browser and your code editor.
2.  Have a look at the following `<whereby-embed>` example, which features several attributes:<br>

    ```html
    <whereby-embed
      room="YOUR-ROOM-URL"
      cameraEffect="blur"
      minimal="on"
      logo="off">
    </whereby-embed>
    ```

    \
    These attributes respectively specify which room you want to connect to (`YOUR-ROOM-URL` should be your room URL), automatically add the "blur" camera effect to the user's video, enable the minimal pre-built UI, and hide the user's company logo.
3. If you're using the pre-built component quickstart web app, the last three attributes should already be present. If not, you'll have to add them yourself.
4. Add and remove them a few times, refreshing the browser tab where the app is loaded each time so you can see the difference the attributes make.

### Adding an avatar image

To add an avatar image for a user, follow these steps:

1. Upload an appropriate avatar image to an online location. The URL must be `https` and contain no URL parameters.&#x20;
2. From your account dashboard, go to "**Configure**" > "**API**". Under the **Allowed domains** panel, add both the domain where you uploaded the avatar image, and the domain where your web app is running, separated by a space. In general, these must be `https`, but `http://localhost` domains are allowed for testing purposes.
3. Press the `Save` button that appears under the text field.
4.  Add the following attribute to your `<whereby-embed>` element, replacing `YOUR-AVATAR-URL` with the URL of your avatar image:<br>

    ```html
    avatarUrl="YOUR-AVATAR-URL"
    ```
5. Refresh your app to see your avatar in action. You should be able to see it next to your name in the **People** panel within the meeting room.

### Enabling some features

The `minimal` setting has hidden too many features. Let's re-enable the **Leave** and **Chat** controls.

1.  Add the following attributes to your `<whereby-embed>` element:<br>

    ```html
    leaveButton="on"
    chat="on"
    ```
2. Refresh your app: you should see that the **Leave** and **Chat** controls have been added back in.

### Hiding the toolbar altogether

For the final customization, we'll take a step in the opposite direction and hide the bottom toolbar completely.

1.  Add the following attributes to your `<whereby-embed>` element:<br>

    ```
    bottomToolbar="off"
    ```
2. Refresh your app: The bottom toolbar should have disappeared, and the rest of the UI should have expanded to fill the space.

We have done this to set the scene for our [Using commands](using-commands.md) article, in which we use commands to create our own custom toolbar.

{% hint style="info" %}
**Note**: You can find our [finished web component quickstart web app](https://github.com/whereby/demos/tree/main/web-component-quickstart) with attributes example on GitHub.
{% endhint %}

### See also

[Attribute reference](../../reference/using-the-whereby-embed-element.md#attributes-of-the-component): contains a table detailing every available attribute.
