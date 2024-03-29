# Screen Sharing Setup & Usage

Screen sharing is a great way to share content to other hosts/participants in the room.

To keep an optimal level of performance in the room, screen sharing will broadcast from 1-5 FPS (frames per second) by default, and up to 1080p based on network and CPU restraints.

Depending on the browser being used, there will be different options for what can be shared.

{% hint style="danger" %}
Screen sharing on Mobile (both phone and tablet) devices is not currently available.&#x20;
{% endhint %}

## Setup and Management

As the platform holder, there are few options you have for managing the available screen sharing feature.

* Enabling screen sharing for all rooms via a [dashboard preference](../whereby-101/customizing-rooms/dashboard-preferences.md)
* Enabling screen sharing on a per room/user basis with a [URL parameter](../whereby-101/customizing-rooms/using-url-parameters.md#screenshare-less-than-on-or-off-greater-than)
* Listening for the [screen sharing event](broken-reference) with our embed element to facilitate in app messages or notifications
* Using the [browser method](broken-reference) for stopping/starting screen sharing

## Browser sharing options

{% tabs %}
{% tab title="Chrome/Chromium" %}
You can share an image of your entire screen. If you have multiple monitors, you can also select which one you'd like to show.

If you select the "Window" section, you can select a specific application like Word or Chrome that you want to share.&#x20;

lastly, you can use the Chrome Tab option if you only want to share one tab from your Browser.

<figure><img src="../.gitbook/assets/Screenshot 2023-08-11 at 3.25.24 PM.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Firefox" %}
Firefox allows for the sharing of an entire screen, or of an app window. Options are not divided into sections.

<figure><img src="../.gitbook/assets/Screenshot 2023-08-11 at 3.32.26 PM.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Safari" %}
Safari allows for the sharing of an entire screen, or of an app window. When selecting the "Share Window" option, you'll hover your mouse over and click the window you'd like to share.

<figure><img src="../.gitbook/assets/Screenshot 2023-08-11 at 3.36.32 PM.png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

## Screen sharing additional features

### Screen sharing with audio

Sharing computer audio is possible while screen sharing and using Chrome or Chromium browsers.&#x20;

Depending on the sharing option selected and the operating system, a checkbox that says "Share Audio" will be present that you can check to share audio.

{% hint style="success" %}
**Tip:** Many audio and video files (_eg mp3, mp4, webm_) can be opened into Chrome for viewing and playback. Simply drag and drop the local files into a new tab, and share tab audio as previously mentioned
{% endhint %}

{% tabs %}
{% tab title="macOS" %}
**macOS**

On macOS, users are only able to share a specific Chrome tab. Sharing audio for the entire computer or a specific application window isn't possible at this time due to browser/operating system permissions.On Windows machines, you'll have the option to share audio from any Chrome tab, and you can also share the audio for your Entire Screen.

<figure><img src="../.gitbook/assets/file-KljwIMe85O.png" alt="" width="375"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Windows" %}
On Windows machines, users have the option to share audio from any Chrome tab or share the audio for the Entire Screen.

{% hint style="danger" %}
Sharing the audio of the entire screen that contains a Whereby room will cause an echo effect. To prevent this issue from happening, try sharing the audio of a Chrome tab or a separate monitor instead.
{% endhint %}

<figure><img src="../.gitbook/assets/file-KljwIMe85O (1).png" alt="" width="375"><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

### Picture in picture

Allowing the use of "[Picture in Picture](https://docs.whereby.com/customizing-rooms/using-url-parameters#pipbutton-off)" can be helpful for users that want to screen share content and still be able to view others within the room.

<figure><img src="../.gitbook/assets/file-I2HcA3GVNZ.png" alt="" width="480"><figcaption></figcaption></figure>

This activates a pop-out of the currently active videos within the meeting room.&#x20;

### Annotating Screen share

Users can extend the capability of screen sharing by installing extensions in Chrome or Chromium browsers. Tools like "[Draw on page](https://chrome.google.com/webstore/detail/draw-on-page/ngmfehckdahhmlbabjemcepfhgnoddlo?utm\_source=ext\_sidebar\&hl=en-US)" or "[Paint](https://chrome.google.com/webstore/detail/paint/ejllkedmklophclpgonojjkaliafeilj)" work well to highlight and indicate content in your browser while screen sharing.

<figure><img src="../.gitbook/assets/Screenshot 2023-08-14 at 2.29.27 PM.png" alt="" width="563"><figcaption></figcaption></figure>

