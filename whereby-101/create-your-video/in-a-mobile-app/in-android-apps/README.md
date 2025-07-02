---
description: >-
  Discover how to embed Whereby rooms into your native Android app using a
  WebView.
---

# Embedding Whereby in Android

This guide offers a high-level overview of the implementation. For a complete working example (including media permission handling, shared file upload and download, for both Activity and Fragment-based setups) we recommend checking out [public demo app](https://github.com/whereby/android-webview-demo).

{% hint style="info" %}
We also offer native SDKs that allow you to tap into powerful features such as listening to room events and use custom buttons to send commands to the room from your application.\
[Read more](using-wherebys-native-sdk.md)
{% endhint %}

In order to embed a room in an Android WebView, add these permissions to the Android Manifest:

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

Configure the WebView settings to enable media functionality. Here is an example:

{% tabs %}
{% tab title="Java" %}
```java
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
{% endtabs %}

Your app will need to handle media permissions, along with file upload and download functionality. Finally, set up your Activity or Fragment:

{% tabs %}
{% tab title="Java" %}
```java
public class MainActivity extends AppCompatActivity {

    private String roomUrlString = "https://yourWherebyRoomUrl"; // Replace by your own
    private WebView webView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        this.webView = findViewById(R.id.webView);
        WebViewUtils.configureWebView(this.webView);
        // Add extra configuration and hanlders for media permissions and shared file here
    }

    @Override
    protected void onResume() {
        super.onResume();
        this.webView.loadUrl(roomUrlString);
    }
}
```
{% endtab %}
{% endtabs %}

