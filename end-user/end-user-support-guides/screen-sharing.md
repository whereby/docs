# Screen Sharing Setup & Usage

Screen sharing is a great way to share content to other hosts/participants in the room.

To keep an optimal level of performance in the room, screen sharing will broadcast from 1-5 FPS (frames per second) by default, and up to 1080p based on network and CPU restraints.

Depending on the browser being used, there will be different options for what can be shared.

{% hint style="danger" %}
Screen sharing on Mobile (both phone and tablet) devices is not currently available.&#x20;
{% endhint %}

## Setup and Management

As the platform holder, there are few options you have for managing the available screen sharing feature.

* Enabling screen sharing for all rooms via a [dashboard preference](../../whereby-101/customizing-rooms/dashboard-preferences.md)
* Enabling screen sharing on a per room/user basis with a [URL parameter](../../whereby-101/customizing-rooms/using-url-parameters.md#screenshare-less-than-on-or-off-greater-than)
* Listening for the [screen sharing event](broken-reference) with our embed element to facilitate in app messages or notifications
* Using the [browser method](broken-reference) for stopping/starting screen sharing

## Browser sharing options

{% tabs %}
{% tab title="Chrome/Chromium" %}
You can share an image of your entire screen. If you have multiple monitors, you can also select which one you'd like to show.

If you select the "Window" section, you can select a specific application like Word or Chrome that you want to share.&#x20;

lastly, you can use the Chrome Tab option if you only want to share one tab from your Browser.

<figure><img src="../../.gitbook/assets/Screenshot 2023-08-11 at 3.25.24 PM.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Firefox" %}
Firefox allows for the sharing of an entire screen, or of an app window. Options are not divided into sections.

<figure><img src="../../.gitbook/assets/Screenshot 2023-08-11 at 3.32.26 PM.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Safari" %}
Safari allows for the sharing of an entire screen, or of an app window. When selecting the "Share Window" option, you'll hover your mouse over and click the window you'd like to share.

<figure><img src="../../.gitbook/assets/Screenshot 2023-08-11 at 3.36.32 PM.png" alt=""><figcaption></figcaption></figure>
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

<figure><img src="../../.gitbook/assets/file-KljwIMe85O.png" alt="" width="375"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Windows" %}
On Windows machines, users have the option to share audio from any Chrome tab or share the audio for the Entire Screen.

{% hint style="danger" %}
Sharing the audio of the entire screen that contains a Whereby room will cause an echo effect. To prevent this issue from happening, try sharing the audio of a Chrome tab or a separate monitor instead.
{% endhint %}

<figure><img src="../../.gitbook/assets/file-KljwIMe85O (1).png" alt="" width="375"><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

### Picture in picture

Allowing the use of "[Picture in Picture](https://docs.whereby.com/customizing-rooms/using-url-parameters#pipbutton-off)" can be helpful for users that want to screen share content and still be able to view others within the room.

<figure><img src="../../.gitbook/assets/file-I2HcA3GVNZ.png" alt="" width="480"><figcaption></figcaption></figure>

This activates a pop-out of the currently active videos within the meeting room.&#x20;

### Annotating Screen share

Users can extend the capability of screen sharing by installing extensions in Chrome or Chromium browsers. Tools like "[Draw on page](https://chrome.google.com/webstore/detail/draw-on-page/ngmfehckdahhmlbabjemcepfhgnoddlo?utm\_source=ext\_sidebar\&hl=en-US)" or "[Paint](https://chrome.google.com/webstore/detail/paint/ejllkedmklophclpgonojjkaliafeilj)" work well to highlight and indicate content in your browser while screen sharing.

<figure><img src="../../.gitbook/assets/Screenshot 2023-08-14 at 2.29.27 PM.png" alt="" width="563"><figcaption></figcaption></figure>

## Troubleshooting Screen Sharing

#### "Could not start screen share"

If you are seeing the above error message, it could mean a few things. Either the request was canceled or your browser doesn't have permission to access screen sharing.

{% hint style="info" %}
**Windows Permissions**:  At the moment Windows doesn't have any system-level permissions for screen access. if you're running into problems or getting an error, double-check your browser settings.

**Chrome and Chromium**: At the moment Chrome doesn't have any specific settings related to screen access.
{% endhint %}

{% tabs %}
{% tab title="macOS permissions" %}
In the more recent versions of macOS, Apple has introduced system-level permissions for applications like your browser for accessing screen sharing.&#x20;

If you've recently updated or upgraded your device and can't screen share anymore, you'll need to update the preferences by doing the following:

1. Open up **System Settings** on your Mac
2. Click on the "**Privacy & Security**" section
3. Scroll to the "**Screen & System Audio Recording**" section, and ensure that your browser is toggled on. You will likely need to restart your browser after enabling.

<figure><img src="../../.gitbook/assets/Screen share permissions.png" alt="" width="563"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Safari Permissions" %}
In some cases you may have accidentally blocked screen sharing for the site at a browser level. In Safari you can fix that with the following:

1. In your menu bar, select **Safari->Settings**
2. Select the "**Websites**" option
3. Scroll down to the "**Screen Sharing**" section
4. Find your website and verify it is set to "**Ask**"

<figure><img src="../../.gitbook/assets/safari screen share permissions.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="FireFox Permissions" %}
When using screen sharing in Firefox, you'll see a pop up that asks if you'd like to allow Whereby (or your site) to access your screen

<figure><img src="../../.gitbook/assets/Firefox screen share.png" alt="" width="375"><figcaption></figcaption></figure>

If you accidentally selected "Always block", Firefox won't show this prompt again. In this case, you'll need to manually reset the permissions for screen access by doing the following:

1. Click on the "Site information" area, just to the left of the website address
2. Locate the "Share the Screen" item in the list, and select the "**X**" next to Blocked

<figure><img src="../../.gitbook/assets/Firefox screen share 2.png" alt="" width="375"><figcaption></figcaption></figure>

3. Refresh the tab
{% endtab %}
{% endtabs %}

#### "Screen share is hard to read"

If you're having trouble reading smaller text on a screen share, try making the window or content that you are sharing smaller. That might mean instead of sharing your entire screen, you share a browser window/tab that you resize slightly. Or, if you have a large monitor, not using an app/tab that is full size for the monitor

