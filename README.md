# Android Integration

The MoguaSDK is compatible with apps supporting Android API level 19 or above.

---

## 1. Install the SDK

1. Make sure inside the `repositories` block, `mavenCentral()` is added：

```Groovy
// project/build.gradle

allprojects {
    repositories {
        // ...
        mavenCentral()
    }
}
```
```Kotlin
// project/settings.gradle.kts

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

```Groovy
// app/build.gradle

dependencies {
    // ...
    implementation 'sg.omnimind.mogua:mogua-android-sdk:0.4.0'
}
```
```Kotlin
// app/build.gradle.kts

dependencies {
  // ...
  implementation("sg.omnimind.mogua:mogua-android-sdk:0.4.0")
}
```

---

## 2. Initialize the SDK

You need to initialize the SDK before any usage.

Assuming that the entry point of your application is a subclass of `Activity`, you can initialize the SDK by overriding the `onCreate` method:

```Java
public class MainActivity extends Activity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        // ...
        Mogua.init(this, "mogua-sdk-app-key", true);
    }
}
```
```Kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // ...
        Mogua.init(this, "mogua-sdk-app-key", true)
    }
}
```

The `Mogua.init` method description:

```Java
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

> Placeholder: *Privacy Policy for Clipboard Authorization*

---

## 3. Retrieve the parameters

After initialization, you can asynchronously retrieve the parameters carried during installation (eg. Submit from landing pages).

```Java
Mogua.getData(new MoguaCallback() {

    @Override
    public void onData(HashMap<String, Object> data) {
        // data: Parameters passed from the web to the app. If no parameters are provided, an empty HashMap object is returned.
    }

    @Override
    public void onError(Exception e) {
        // e: The exception that occurred.
    }
})
```
```Kotlin
Mogua.getData(object: MoguaCallback {

    override fun onData(data: HashMap<String, Any>) {
        // data: Parameters passed from the web to the app. If no parameters are provided, an empty HashMap object is returned.
    }

    override fun onError(e: Exception) {
        // e: The exception that occurred.
    }
})
```

> Parameters are cached after passing.
> The `getData` method will always retrieve the same parameters unless the application is reinstalled.