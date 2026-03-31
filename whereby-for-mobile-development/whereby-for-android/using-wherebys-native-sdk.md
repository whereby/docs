# Whereby Android SDK

The Whereby Android SDK is [available through Jitpack](https://jitpack.io/#whereby/android-sdk) and can also be found on [GitHub](https://github.com/whereby/android-sdk). It loads the Whereby web component through a WebView and integrates with your editor to make it easier to customize the Whereby Embedded experience. You can also hook into powerful features, such as listening to room events and sending commands to the room from the host application. This gives you the possibility to implement your own controls. Media permissions are already handled in the SDK.&#x20;

## Quickstart guide

This guide shows you how to embed a Whereby room in a native Android app using the Whereby Android SDK. For a complete running example, checkout our [Android SDK demo app](https://github.com/whereby/android-sdk-demo) repository.&#x20;

### Prerequisites

This guide assumes that you’ve already created a Whereby Embedded account and a meeting room. If you’ve not already taken these steps, you can do so by following the [Whereby Embedded initial setup guide](../../getting-started/overview-of-whereby-embedded/initial-setup.md).

### Setting up the app

To start, open the latest version of Android Studio and create a new project with an empty Activity, keeping the recommended settings.&#x20;

### Installing the Whereby SDK

The framework is distributed using [Jitpack](https://docs.jitpack.io/). If it is not already the case, add the Maven and Jitpack repositories to your Android project. This can be done either in the project **build.gradle** file (not to be confused with the _module_ build.gradle file) or in the **settings.gradle** file:

{% tabs %}
{% tab title="Groovy DSL" %}
```groovy
repositories {
    ...
    mavenCentral()
    maven { url 'https://jitpack.io' }  
}
```
{% endtab %}

{% tab title="Kotlin DSL" %}
<pre class="language-kotlin"><code class="lang-kotlin">repositories {
    ...
    mavenCentral()
    maven("https://jitpack.io")
<strong>}
</strong></code></pre>
{% endtab %}
{% endtabs %}

Then, add the Whereby SDK dependency to your module **build.gradle** file. You can specify either a full name `X.X.X` version (see the [list of released versions](https://github.com/whereby/android-sdk/releases)), or simply use a range format `X.+`:

{% tabs %}
{% tab title="Groovy DSL" %}
```groovy
dependencies {
    ...
    def WHEREBY_SDK_VERSION = '1.+'
    implementation("com.github.whereby:android-sdk:$WHEREBY_SDK_VERSION@aar") { transitive = true }
}
```
{% endtab %}

{% tab title="Kotlin DSL" %}
```kotlin
dependencies {
    val wherebySdkVersion = "1.+"
    implementation("com.github.whereby:android-sdk:$wherebySdkVersion@aar") {
        isTransitive = true
    }
}
```
{% endtab %}
{% endtabs %}

_Remember to sync your project after updating the gradle files._

### Implementing the main activity

Unlike the WebView approach, there is no need to manually set up permissions — these are already handled by the SDK.

Replace the app's main Activity with the code below, and update the URL in the example to your own Whereby room URL.

{% tabs %}
{% tab title="Java" %}
```java
package com.example.myapplication;

import android.os.Bundle;
import androidx.fragment.app.FragmentActivity;

import com.whereby.sdk.*;

public class MainActivity extends FragmentActivity {

    // Replace with your own
    public static final String ROOM_URL_STRING = "https://your-whereby-room-URL";

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        if (savedInstanceState != null) return;

        // 1. Setup the WherebyRoomConfig
        WherebyRoomConfig config = new WherebyRoomConfig(ROOM_URL_STRING);

        // 2. Instantiate the WherebyRoomFragment to be presented
        WherebyRoomFragment fragment = WherebyRoomFragment.newInstance(config);

        // 3. Replace the fragment with the WherebyRoomFragment
        getSupportFragmentManager()
                .beginTransaction()
                .replace(android.R.id.content, fragment, "tag")
                .commit();
    }
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
package com.example.myapplication

import android.os.Bundle
import androidx.fragment.app.FragmentActivity
import com.whereby.sdk.*

class MainActivity : FragmentActivity() {

    companion object {
        // Replace with your own
        const val ROOM_URL_STRING = "https://your-whereby-room-URL"
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        if (savedInstanceState != null) return

        // 1. Setup the WherebyRoomConfig
        val config = WherebyRoomConfig(ROOM_URL_STRING)

        // 2. Instantiate the WherebyRoomFragment to be presented
        val fragment = WherebyRoomFragment.newInstance(config)

        // 3. Replace the fragment with the WherebyRoomFragment
        supportFragmentManager
            .beginTransaction()
            .replace(android.R.id.content, fragment, "tag")
            .commit()
    }
}
```
{% endtab %}
{% endtabs %}

### Testing the app

Run the app in Android Studio, and you should see your Whereby instance running, allowing you to join the room with both camera and microphone enabled.

{% hint style="info" %}
This is a minimal implementation. It is recommended to extend the configuration to improve the user experience. For example, you can:

* Send commands to the `WherebyRoomFragment` to toggle the camera or microphone, allowing you to implement your own controls.
* Listen for room events, such as when the user leaves the room, to redirect them to the previous screen.

For a complete example that support both Activity and Fragment, check out our public repository: [android-sdk](https://github.com/whereby/android-sdk-demo)[-demo](https://github.com/whereby/android-sdk-demo).
{% endhint %}

***
