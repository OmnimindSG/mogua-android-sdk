[//]: # (redundant)
# Android Integration

The MoguaSDK is compatible with apps supporting Android API level 19 or above.

---

## 1. Install the SDK

1. Make sure inside the `repositories` block, `mavenCentral()` is added：

[//]: # (language="Groovy", target="project/build.gradle")
```groovy
allprojects {
    repositories {
        // ...
        mavenCentral()
    }
}
```
[//]: # (language="Kotlin", target="project/settings.gradle.kts")
```kotlin
pluginManagement {
    repositories {
        // ...
        mavenCentral()
    }
}
dependencyResolutionManagement {
    repositories {
        // ...
        mavenCentral()
    }
}
```


2. In the `dependencies` block, add:

[//]: # (language="Groovy", target="app/build.gradle")
```groovy
dependencies {
    // ...
    implementation 'sg.omnimind.mogua:mogua-android-sdk:0.4.0'
}
```
[//]: # (language="Kotlin", target="app/build.gradle.kts")
```kotlin
dependencies {
  // ...
  implementation("sg.omnimind.mogua:mogua-android-sdk:0.4.0")
}
```

---

## 2. Initialize the SDK

You need to initialize the SDK before any usage.

Assuming that the entry point of your application is a subclass of `Activity`, you can initialize the SDK by overriding the `onCreate` method:

[//]: # (language="Java", target="MainActivity.java")
```java
public class MainActivity extends Activity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        // ...
        Mogua.init(this, "mogua-sdk-app-key", true);
    }
}
```
[//]: # (language="Kotlin", target="MainActivity.kt")
```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // ...
        Mogua.init(this, "mogua-sdk-app-key", true)
    }
}
```

The `Mogua.init` method description:

[//]: # (language="Java")
```java
interface Mogua {
    /**
     * Initialize the MoguaSDK.
     * @param activity The activity of current context.
     * @param appKey The App Key associated with this application, you can find it on the mogua.io dashboard.
     * @param allowClipboardAccess Whether to allow access to the clipboard. Enabling this feature can enhance accuracy, but may trigger permission warnings on certain systems.
     */
    static void init(Activity activity, String appKey, boolean allowClipboardAccess);
}
```

[//]: # (Placeholder: Privacy Policy for Clipboard Authorization)

---

## 3. Retrieve the parameters

After initialization, you can asynchronously retrieve the parameters carried during installation (eg. Submit from landing pages).

[//]: # (language="Java", target="Example")
```java
Mogua.getData(new MoguaCallback() {
    
    /**
     * Callback when data fetch is completed.
     * @param data Key-value pair parameters passed to the app from webpage. If no parameters are provided, an empty HashMap object is returned.
     */
    @Override
    public void onData(HashMap<String, Object> data) {
        // Use data to retrieve the user's channel or referrer, etc.
    }

    /**
     * Callback when an exception occurs.
     * @param e The exception.
     */
    @Override
    public void onError(Exception e) {
        // ...
    }
})
```
[//]: # (language="Kotlin")
```kotlin
Mogua.getData(object: MoguaCallback {

    /**
     * Callback when data fetch is completed.
     * @param data Key-value pair parameters passed to the app from webpage. If no parameters are provided, an empty HashMap object is returned.
     */
    override fun onData(data: HashMap<String, Any>) {
        // Use data to retrieve the user's channel or referrer, etc.
    }

    /**
     * Callback when an exception occurs.
     * @param e The exception.
     */
    override fun onError(e: Exception) {
        // ...
    }
})
```

> Parameters are cached after passing.
> The `getData` method will always retrieve the same parameters unless the application is reinstalled.