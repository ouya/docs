# App Setup

## Requirements

For your Android application to be compatible with the OUYA, it must target platform **16** (Android 4.1.2) or later.

**Example:**

```xml
<uses-sdk android:minSdkVersion="16" />
```

## Step 1: Add OUYA SDK jar to build path

To access OUYA-specific functions, you'll need to [download the ODK](https://devs.ouya.tv/developers/odk) and add `ouya-sdk.jar` to the build-path of your app.  
This jar can be found in the `libs/` folder of the ODK.

### Gradle

If you're using gradle, you can compile against our latest SDK using the following in your `build.gradle`:

```Gradle
repositories {
    maven {
        url "http://maven.ouya.tv/"
    }
}
dependencies {
    compile 'tv.ouya:sdk:+' // You can replace "+" with a specific version number
}
```

## Step 2: Add intent-filter

For your OUYA app to show up in the **PLAY** section, it must have an activity that contains a filter for either `tv.ouya.intent.category.GAME` or `tv.ouya.intent.category.APP`.

**Example:**

```xml
<activity android:name=".GameActivity" android:label="@string/app_name">
  <intent-filter>
    <action android:name="android.intent.action.MAIN"/>
    <category android:name="android.intent.category.LAUNCHER"/>
    <category android:name="tv.ouya.intent.category.GAME"/>
  </intent-filter>  
</activity>
```

## Step 3: Add an icon

All OUYA apps have an icon which is displayed in various places on the system. The icon is expected to be located in `res/drawable-xhdpi/ouya_icon.png` with the dimensions 732px x 412px

## Step 4: Create and secure your keystore

When releasing an APK, it must be signed by a keystore to verify its source. You can create a keystore using built-in wizards in [Eclipse](http://developer.android.com/tools/publishing/app-signing.html#adt) and [Android Studio/IntelliJ](http://developer.android.com/tools/publishing/app-signing.html#studio), or via the command line:

```
keytool -genkey -v -keystore my-release-key.keystore -alias alias_name -keyalg RSA -keysize 2048 -validity 10000
```

Additionally, it is **CRITICAL** that you keep this keystore in a secure place. You will not be able to update your app if you lose your key. Updates must be signed with the exact same key; there is no way to re-create it if lost.

Detailed information on Android app signing is available [here](http://developer.android.com/tools/publishing/app-signing.html)
