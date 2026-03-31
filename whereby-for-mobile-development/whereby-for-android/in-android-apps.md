# Using a WebView

When embedding Whereby in a WebView, you will have access to all features available on the web, directly within your Android app. While it is relatively simple to build a basic app this way, please note that it comes with certain limitations, such as restricted background mode support, additional logic required for file sharing, etc.

{% hint style="info" %}
We offer an [Android SDK](using-wherebys-native-sdk.md) that is a wrapper around a WebView and allows you to tap into powerful features such as listening to room events and using custom buttons to send commands to the room from your application. It also already handles the media permissions.
{% endhint %}

## Quickstart guide

This guide shows you how to embed a Whereby room in a native Android app using a WebView. After that, it also introduces some customization options to give you an idea of what’s available. For a complete running example, check out our [Android WebView demo app](https://github.com/whereby/android-webview-demo) repository.&#x20;

### Prerequisites

This guide assumes that you’ve already created a Whereby Embedded account and a meeting room. If you’ve not already taken these steps, you can do so by following the [Whereby Embedded initial setup guide](../../getting-started/overview-of-whereby-embedded/initial-setup.md).

When embedding Whereby in a WebView, depending on your implementation, you can use the following types of URLs:

* A Whereby room URL (which takes the user directly to the Whereby pre-call screen).
* A custom web app that embeds Whereby using the Embedded element.
* A React app that embeds Whereby with the React Hooks SDK.

### Setting up the app

To start, open the latest version of Android Studio and create a new project with an empty Activity, keeping the recommended settings.&#x20;

### Setting the required permissions

To run a Whereby instance on Android, you must grant a set of permissions — including access to the device’s camera, internet connectivity, audio settings, and recording capabilities. Additional permissions may be required if, for example, you need to store shared files; however, these are not covered in this guide.

Add the following permissions to your app's manifest file:

{% tabs %}
{% tab title="XML" %}
```xml
<manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    ...
</manifest>
```
{% endtab %}
{% endtabs %}

### Implementing the main activity

Replace the app's main Activity with the code below and update the URL in the example to your own Whereby room URL or web app URL.

{% tabs %}
{% tab title="Java" %}
```java
package com.example.androidwebview5;

import android.Manifest;
import android.app.Activity;
import android.content.pm.PackageManager;
import android.os.Bundle;
import android.webkit.PermissionRequest;
import android.webkit.WebChromeClient;
import android.webkit.WebSettings;
import android.webkit.WebView;
import android.widget.Toast;

import androidx.annotation.NonNull;

public class MainActivity extends Activity {

    public static final String ROOM_URL = "https://your-whereby-room-URL";
    private static final String ROOM_PARAMS = "?skipMediaPermissionPrompt"; // Permissions are handled manually in code
    private static final int PERMISSION_REQUEST_CODE = 1;

    private static final String[] RUNTIME_PERMISSIONS = new String[] {
            Manifest.permission.CAMERA,
            Manifest.permission.RECORD_AUDIO
    };

    private WebView webView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // 1. Configure the WebView settings
        webView = new WebView(this);
        WebSettings settings = webView.getSettings();
        settings.setJavaScriptEnabled(true);
        settings.setDomStorageEnabled(true);
        settings.setDatabaseEnabled(true);
        settings.setMediaPlaybackRequiresUserGesture(false);

        // 2. Set a custom WebChromeClient to handle the media permissions
        webView.setWebChromeClient(new WebChromeClient() {
            @Override
            public void onPermissionRequest(final PermissionRequest request) {
                runOnUiThread(() -> {
                    request.grant(request.getResources());
                });
            }
        });

        // 3. Setup the activity content view with the webView
        setContentView(webView);
        
        // 4. Request the permisssions
        requestPermissions(RUNTIME_PERMISSIONS, PERMISSION_REQUEST_CODE);
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);

        if (requestCode == PERMISSION_REQUEST_CODE) {
            boolean allGranted = true;
            for (int r : grantResults) {
                if (r != PackageManager.PERMISSION_GRANTED) { allGranted = false; break; }
            }
            if (allGranted) {
                webView.loadUrl(ROOM_URL + ROOM_PARAMS);
            } else {
                Toast.makeText(this, "Camera/mic permission denied.", Toast.LENGTH_LONG).show();
            }
        }
    }
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
package com.example.androidwebview5

import android.Manifest
import android.app.Activity
import android.content.pm.PackageManager
import android.os.Bundle
import android.webkit.PermissionRequest
import android.webkit.WebChromeClient
import android.webkit.WebSettings
import android.webkit.WebView
import android.widget.Toast

class MainActivity : Activity() {

    companion object {
        private const val ROOM_URL = "https://your-whereby-room-URL"
        private const val ROOM_PARAMS = "?skipMediaPermissionPrompt" // Permissions are handled manually in code
        private const val PERMISSION_REQUEST_CODE = 1

        private val RUNTIME_PERMISSIONS = arrayOf(
            Manifest.permission.CAMERA,
            Manifest.permission.RECORD_AUDIO
        )
    }

    private lateinit var webView: WebView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // 1. Configure the WebView settings
        webView = WebView(this)
        val settings: WebSettings = webView.settings
        settings.javaScriptEnabled = true
        settings.domStorageEnabled = true
        settings.databaseEnabled = true
        settings.mediaPlaybackRequiresUserGesture = false

        // 2. Set a custom WebChromeClient to handle the media permissions
        webView.webChromeClient = object : WebChromeClient() {
            override fun onPermissionRequest(request: PermissionRequest) {
                runOnUiThread {
                    request.grant(request.resources)
                }
            }
        }

        // 3. Setup the activity content view with the webView
        setContentView(webView)

        // 4. Request the permissions
        requestPermissions(RUNTIME_PERMISSIONS, PERMISSION_REQUEST_CODE)
    }

    override fun onRequestPermissionsResult(
        requestCode: Int,
        permissions: Array<out String>,
        grantResults: IntArray
    ) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults)

        if (requestCode == PERMISSION_REQUEST_CODE) {
            val allGranted = grantResults.all { it == PackageManager.PERMISSION_GRANTED }
            if (allGranted) {
                webView.loadUrl(ROOM_URL + ROOM_PARAMS)
            } else {
                Toast.makeText(this, "Camera/mic permission denied.", Toast.LENGTH_LONG).show()
            }
        }
    }
}

```
{% endtab %}
{% endtabs %}

### Testing the app

Run the app in Android Studio, and you should see your Whereby instance running, allowing you to join the room with both camera and microphone enabled.

{% hint style="info" %}
This is a minimal implementation that sticks to the default behavior of WebView. It is recommended to extend the configuration to improve the user experience. For example, you can:

* Enable and manage file sharing.
* Handle external link redirections.

For a complete example that support both Activity and Fragment, check out our public repository: [android-webview-demo](https://github.com/whereby/android-webview-demo).
{% endhint %}

***

### Customizing your app

Now that you’ve got a basic app running, you’ll want to customize it to your liking. Whereby has many options for customizing your app:

* Global settings via your account dashboard.
* Per-room settings via component [attributes](../../whereby-for-web-browser/web-component-and-pre-built-ui/configuring-with-attributes.md) and [room creation settings](../../whereby-product-features/using-the-rest-api/).

In this section, we’ll go through a couple of options to give you an idea of what’s available.

#### Setting custom brand colors

1. In your account dashboard: Go to the **Configure** screen and access the **Appearance** panel.
2. Set new primary, secondary, and focus highlight colors.
3. Set a new background color, and upload a background image.
4. Upload a company logo.
5. Re-run your app to see how these settings change the look and feel.

#### Setting web component attributes

There are many different attributes you can set on the web component to configure various aspects of the video call experience. Let's add some to the room URL:

```java
ROOM_URL = "https://your-whereby-room-URL?skipMediaPermissionPrompt&minimal"
```

The effect of the attributes is as follows:

* `skipMediaPermissionPrompt`: Skip the screen prompting the user to grant permission, as this can is already handled in the code.
* `minimal`: Uses the minimal Whereby UI.

Save your code and re-run your app to see the effect of the attributes.

### See also

* [Meeting controls](https://whereby.helpscoutdocs.com/article/458-meeting-controls)
