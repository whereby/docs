---
description: >-
  Learn how to embed Whereby rooms in your native Android app using the WebView
  class.
---

# In Android apps

In order to embed a room in Android, add these permissions to the manifest:

{% tabs %}
{% tab title="XML" %}
```xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
```
{% endtab %}
{% endtabs %}

You will need to use the [WebView](https://developer.android.com/reference/android/webkit/WebView) class. Start by creating a custom `WebChromeClient` class and override the `onPermissionRequest` method:

{% tabs %}
{% tab title="Java" %}
```java
import android.annotation.TargetApi;
import android.app.Activity;
import android.os.Build;
import android.webkit.PermissionRequest;
import android.webkit.WebChromeClient;

public class CustomWebChromeClient extends WebChromeClient {
    private Activity activity;

    public CustomWebChromeClient(Activity parentActivity) {
        super();
        activity = parentActivity;
    }

    @Override
    public void onPermissionRequest(final PermissionRequest request) {
        activity.runOnUiThread(new Runnable() {
            @TargetApi(Build.VERSION_CODES.LOLLIPOP)
            @Override
            public void run() {
                request.grant(request.getResources());
            }
        });
    }
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
import android.app.Activity
import android.webkit.PermissionRequest
import android.webkit.WebChromeClient

class CustomWebChromeClient(private val activity: Activity) : WebChromeClient() {
    override fun onPermissionRequest(request: PermissionRequest) {
        activity.runOnUiThread { request.grant(request.resources) }
    }
}
```
{% endtab %}
{% endtabs %}

Then, create a `WebViewUtils` helper class to configure the WebView. Here is a possible configuration:

{% tabs %}
{% tab title="Java" %}
```java
import android.annotation.SuppressLint;
import android.webkit.CookieManager;
import android.webkit.WebSettings;
import android.webkit.WebView;

public class WebViewUtils {

    @SuppressLint("SetJavaScriptEnabled")
    public static void configureWebView(WebView webView) {
        WebSettings webSettings = webView.getSettings();
        webSettings.setJavaScriptEnabled(true);
        webSettings.setDomStorageEnabled(true);
        webSettings.setDatabaseEnabled(true);
        webSettings.setMediaPlaybackRequiresUserGesture(false);
        CookieManager.getInstance().setAcceptCookie(true);
        CookieManager.getInstance().setAcceptThirdPartyCookies(webView, true);
    }
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
import android.annotation.SuppressLint
import android.webkit.CookieManager
import android.webkit.WebView

object WebViewUtils {
    @SuppressLint("SetJavaScriptEnabled")
    fun configureWebView(webView: WebView) {
        val webSettings = webView.settings
        webSettings.javaScriptEnabled = true
        webSettings.domStorageEnabled = true
        webSettings.databaseEnabled = true
        webSettings.mediaPlaybackRequiresUserGesture = false
        CookieManager.getInstance().setAcceptCookie(true)
        CookieManager.getInstance().setAcceptThirdPartyCookies(webView, true)
    }
}
```
{% endtab %}
{% endtabs %}

Finally, set up your activity following these steps:

1. Configure the webView.
2. Request the permissions if needed.
3. Add the `?skipMediaPermissionPrompt` parameter to the room URL and load it.

Here is an example:

{% tabs %}
{% tab title="Java" %}
```java
import androidx.annotation.NonNull;
import androidx.annotation.RequiresApi;
import androidx.appcompat.app.AppCompatActivity;
import android.Manifest;
import android.content.pm.PackageManager;
import android.os.Build;
import android.os.Bundle;
import android.webkit.WebView;
import android.webkit.WebViewClient;

import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {

    public String roomUrlString = ""; // Replace by your own
    private String roomParameters = "?skipMediaPermissionPrompt";

    private static final int PERMISSION_REQUEST_CODE = 1234;
    private String[] requiredDangerousPermissions = {
            Manifest.permission.CAMERA,
            Manifest.permission.MODIFY_AUDIO_SETTINGS,
            Manifest.permission.RECORD_AUDIO
    };

    private WebView webView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        this.webView = findViewById(R.id.webView);
        WebViewUtils.configureWebView(this.webView);
        this.webView.setWebChromeClient(new CustomWebChromeClient(this));
        this.webView.setWebViewClient(new WebViewClient());
    }

    @Override
    protected void onResume() {
        super.onResume();
        if (this.webView.getUrl() == null) {
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M && this.isPendingPermissions()) {
                // This explicitly requests the camera and audio permissions.
                // It's fine for a demo app but should probably be called earlier in the flow,
                // on a user interaction instead of onResume.
                this.requestCameraAndAudioPermissions();
            } else {
                this.loadEmbeddedRoomUrl();
            }
        }
    }

    private void loadEmbeddedRoomUrl() {
        this.webView.loadUrl(roomUrlString + roomParameters);
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        switch (requestCode) {
            case PERMISSION_REQUEST_CODE:
                if (this.grantResultsContainsDenials(grantResults)) {
                    // Show some permissions required dialog.
                } else {
                    // All necessary permissions granted, continue loading.
                    this.loadEmbeddedRoomUrl();
                }
                break;
            default:
                super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        }
    }

    @RequiresApi(api = Build.VERSION_CODES.M)
    private void requestCameraAndAudioPermissions() {
        this.requestPermissions(this.getPendingPermissions(), PERMISSION_REQUEST_CODE);
    }

    @RequiresApi(api = Build.VERSION_CODES.M)
    private String[] getPendingPermissions() {
        List<String> pendingPermissions = new ArrayList<>();
        for (String permission : this.requiredDangerousPermissions) {
            if (this.checkSelfPermission(permission) == PackageManager.PERMISSION_DENIED) {
                pendingPermissions.add(permission);
            }
        }
        return pendingPermissions.toArray(new String[pendingPermissions.size()]);
    }

    private boolean isPendingPermissions() {
        if (Build.VERSION.SDK_INT < Build.VERSION_CODES.M) {
            return false;
        }
        return this.getPendingPermissions().length > 0;
    }

    private boolean grantResultsContainsDenials(int[] grantResults) {
        for (int result : grantResults) {
            if (result == PackageManager.PERMISSION_DENIED) {
                return true;
            }
        }
        return false;
    }
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
import android.Manifest
import android.content.pm.PackageManager
import android.os.Build
import android.os.Bundle
import android.webkit.WebView
import android.webkit.WebViewClient
import androidx.annotation.RequiresApi
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {

    var roomUrlString = "" // Replace by your own
    private val roomParameters = "?skipMediaPermissionPrompt"

    companion object {
        private const val PERMISSION_REQUEST_CODE = 1234
    }

    private val requiredDangerousPermissions = arrayOf(
        Manifest.permission.CAMERA,
        Manifest.permission.MODIFY_AUDIO_SETTINGS,
        Manifest.permission.RECORD_AUDIO
    )

    private var webView: WebView? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        webView = findViewById(R.id.webView)
        WebViewUtils.configureWebView(webView!!)
        webView!!.setWebChromeClient(CustomWebChromeClient(this))
        webView!!.setWebViewClient(WebViewClient())
    }

    override fun onResume() {
        super.onResume()
        if (webView!!.url == null) {
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M && isPendingPermissions()) {
                // This explicitly requests the camera and audio permissions.
                // It's fine for a demo app but should probably be called earlier in the flow,
                // on a user interaction instead of onResume.
                requestCameraAndAudioPermissions()
            } else {
                loadEmbeddedRoomUrl()
            }
        }
    }

    private fun loadEmbeddedRoomUrl() {
        webView!!.loadUrl(roomUrlString + roomParameters)
    }

    override fun onRequestPermissionsResult(
        requestCode: Int,
        permissions: Array<String>,
        grantResults: IntArray
    ) {
        when (requestCode) {
            PERMISSION_REQUEST_CODE -> if (grantResultsContainsDenials(grantResults)) {
                // Show some permissions required dialog.
            } else {
                // All necessary permissions granted, continue loading.
                loadEmbeddedRoomUrl()
            }
            else -> super.onRequestPermissionsResult(requestCode, permissions, grantResults)
        }
    }

    @RequiresApi(api = Build.VERSION_CODES.M)
    private fun requestCameraAndAudioPermissions() {
        requestPermissions(pendingPermissions, PERMISSION_REQUEST_CODE)
    }

    @get:RequiresApi(api = Build.VERSION_CODES.M)
    private val pendingPermissions: Array<String>
        private get() {
            val pendingPermissions: MutableList<String> = ArrayList()
            for (permission in requiredDangerousPermissions) {
                if (checkSelfPermission(permission) == PackageManager.PERMISSION_DENIED) {
                    pendingPermissions.add(permission)
                }
            }
            return pendingPermissions.toTypedArray()
        }

    private fun isPendingPermissions(): Boolean {
        return if (Build.VERSION.SDK_INT < Build.VERSION_CODES.M) {
            false
        } else pendingPermissions.isNotEmpty()
    }

    private fun grantResultsContainsDenials(grantResults: IntArray): Boolean {
        for (result in grantResults) {
            if (result == PackageManager.PERMISSION_DENIED) {
                return true
            }
        }
        return false
    }

}
```
{% endtab %}
{% endtabs %}

