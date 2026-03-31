# Using commands

Now that you've mastered attributes, let's move onto using custom commands!

The `<whereby-embed>` web component exposes a set of methods your application can invoke to perform actions in the room. This is ideal for implementing custom controls for your Whereby component-based video call functionality. In this article, we'll show you how to recreate the bottom toolbar, which we hid at the end of the [Configuring with attributes](configuring-with-attributes.md) guide.

### Prerequisites

This guide assumes that you’ve already created a Whereby Embedded account and a meeting room, and have joined it by adding the `<whereby-embed>` component to a web app.

* If you’ve not created a Whereby Embedded account and a meeting room, you can do so by following the [Whereby Embedded initial setup guide](../../getting-started/overview-of-whereby-embedded/initial-setup.md).
* If you've not created a video call web app using `<whereby-embed>`, you can clone our [finished web component quickstart web app with attributes](https://github.com/whereby/demos/tree/main/web-component-with-attributes) on GitHub.

### Getting started

1. Open your example web app in your browser and your code editor.
2. If you're using your own example, make sure you've got the `bottomToolbar="off"` attribute set on your `<whereby-embed>` attribute.
3. From your account dashboard, go to "**Configure**" > "**API**". Under the **Allowed domains** panel, add the domain where your web app is running. In general, this must be `https`, but `http://localhost` domains are allowed for testing purposes. **Your app domain&#x20;**_**must**_**&#x20;be present here; otherwise, the Whereby commands will not work.**
4. Press the `Save` button that appears under the text field.

### Adding control buttons

In this article, we are going to re-implement our own custom versions of the Camera, Microphone, Chat, People, and Leave controls. The first thing we need to achieve this is an HTML button bar.

Add the following HTML snippet to your document, above the `<whereby-embed>` element:

```html
<section id="button-bar">
  <button id="camera">Mute video</button>
  <button id="mic">Mute audio</button>
  <button id="chat">Show chat</button>
  <button id="people">Show people</button>
  <button id="leave">Leave meeting</button>
</section>
```

### Styling the button bar

Let's give our buttons some style.

1.  Add the following CSS to your stylesheet to make sure the `<body>` element spans the full height of the screen, but the `<whereby-embed>` element leaves a gap at the bottom to make space for the button bar. If you are starting from our finished web component quickstart web app with attributes example, you'll also want to remove the current CSS rule that targets `body` and\
    `whereby-embed`.<br>

    ```css
    body {
      height: inherit;
      margin: inherit;
    }

    whereby-embed {
      height: calc(100% - 30px);
      margin: inherit;
    }
    ```
2.  Next, add the following rule to place the button bar in the gap at the bottom of the screen, center the buttons, and get things looking neat:<br>

    ```css
    #button-bar {
      position: absolute;
      bottom: 0;
      width: 100%;
      background-color: black;
      padding: 10px 0;
      display: flex;
      justify-content: center;
      gap: 20px;
    }
    ```

### Adding button functionality

Now we'll get our buttons working.

1.  Add the following line to the bottom of your HTML `<head>`:<br>

    ```html
    <script defer src="index.js"></script>
    ```
2.  Create an `index.js` file in your app directory and add the following statements to grab references to your control buttons and the `<whereby-embed>` element:<br>

    ```javascript
    const cameraBtn = document.getElementById("camera");
    const micBtn = document.getElementById("mic");
    const chatBtn = document.getElementById("chat");
    const peopleBtn = document.getElementById("people");
    const leaveBtn = document.getElementById("leave");
    const whereby = document.querySelector("whereby-embed");
    ```
3. Add the following event handler functions to perform the expected actions when you press those buttons:

{% code overflow="wrap" %}
```javascript
cameraBtn.addEventListener("click", () => {
  whereby.toggleCamera();
  cameraBtn.textContent === "Mute video" ? cameraBtn.textContent = "Unmute video" : cameraBtn.textContent = "Mute video";
});

micBtn.addEventListener("click", () => {
  whereby.toggleMicrophone();
  micBtn.textContent === "Mute audio" ? micBtn.textContent = "Unmute audio" : micBtn.textContent = "Mute audio";
});

chatBtn.addEventListener("click", () => {
  whereby.toggleChat();
  chatBtn.textContent === "Show chat" ? chatBtn.textContent = "Hide chat" : chatBtn.textContent = "Show chat";
});

peopleBtn.addEventListener("click", () => {
  whereby.togglePeople();
  peopleBtn.textContent === "Show people" ? peopleBtn.textContent = "Hide people" : peopleBtn.textContent = "Show people";
});

leaveBtn.addEventListener("click", () => {
  whereby.leaveRoom();
  window.location.href = "https://whereby.com";
});
```
{% endcode %}

{% hint style="info" %}
**Note**: You can find our [finished web component quickstart web app with commands](https://github.com/whereby/demos/tree/main/web-component-with-commands) example on GitHub.
{% endhint %}

### See also

* [Command reference](../../reference/using-the-whereby-embed-element.md#sending-commands): contains a table detailing every available event.
