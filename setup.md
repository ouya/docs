## Setup Instructions for the OUYA ODK

##### MacOS

Download and install the [Android SDK and tools](http://developer.android.com/sdk/index.html) to your Mac or PC, following the included instructions.

Launch the Android SDK Manager by running ([detailed instructions](http://developer.android.com/sdk/installing/adding-packages.html)):

    ./android sdk

Install these packages:

- **Tools**: Including both Android SDK and Android SDK Platform tools
- **Android 4.1 (API 16)**: SDK Platform
- **Android 4.0 (API 14)**: SDK Platform
- **Extras**: Android Support Library

Install the Java runtime if you are prompted to do so.

You will need to add some paths to PATH. Assuming you have put the SDK folder in the location `~/android/android-sdk-macosx`, open a terminal and add the following three lines to your `.bashrc` (or `.zshrc`):

```bash
export PATH=$PATH:~/android/android-sdk-macosx/tools
export PATH=$PATH:~/android/android-sdk-macosx/platform-tools
export ANDROID_HOME=~/android/android-sdk-macosx
```

You may need to adjust your `.bashrc` entries if you have used a custom SDK folder location.

Make sure to `source `~/.bashrc` once you've made these changes.

**New** : You will need to add the following line to `~/.android/adb_usb.ini` for your OUYA console to be recognised:

`0x2836`

Then, run the following commands:

    adb kill-server
    adb devices

Your console should be shown in the list of available devices.

##### Windows

You will need to modify the file `usb_driver\android_winusb.inf` within the Android SDK for your OUYA console to be recognised.

You should locate the section with the title `[Google.NTx86]` and add the following lines before connecting the console;

  ;OUYA Console
  %SingleAdbInterface% = USB_Install, USB\VID_2836&PID_0010
  %CompositeAdbInterface% = USB_Install, USB\VID_2836&PID_0010&MI_01

You should then connect your consoles micro USB port to the USB port of your computer using the appropriate cable. Once it has been connected you should run the following command:

  adb kill-server
  echo 0x2836 >> "%USERPROFILE%\.android\adb_usb.ini"
  adb start-server
  adb devices

Your console should be shown in the list of available devices.

##### Eclipse

If you're using Eclipse as your development environment, you can create Eclipse projects from the samples included in the ODK to get a working example running quickly.

- Open Eclipse
- From the menu, select **File** > **New** > **Project**
- In the new project dialog, select **Android** > **Android Project from Existing Code**. Click the **Next** button.
- In the **Import Projects** dialog, click the **Browse** button.
- From the file browser that appears, navigate to the root directory of the sample app you want to use. For example, `OUYA-ODK/Samples/game-sample`. Then click the **Open** button.
- In the **Import Projects** dialog, click the **Finish** button.

This will create a working Eclipse project. If your OUYA console is connected, you can run the sample application from the Eclipse menu **Run** > **Run**.

##### Developing with the ODK

Use the Android API Level 16 (Android 4.1 "Jelly Bean") when developing for the OUYA Console.

In order to use the OUYA API you will need to include `ouya-sdk.jar` in your project libraries, as well as `guava-r09.jar` and `commons-lang-2.6.jar`. These can be found in the `libs` directory.

For information on the API commands available, please consult the OUYA API reference documentation.

To run the sample code, open up the project in `iap-sample-app` and follow the instructions in the `README.txt` file.

For your application or game to be recognized as made for OUYA, you will need to include an OUYA intent category on the manifest entry of your main activity. 
Use “tv.ouya.intent.category.GAME” or “tv.ouya.intent.category.APP”.

```xml
<activity android:name=".GameActivity" android:label="@string/app_name">
  <intent-filter>
    <action android:name="android.intent.action.MAIN"/>
    <category android:name="android.intent.category.LAUNCHER"/>
    <category android:name="tv.ouya.intent.category.GAME"/>
  </intent-filter>
</activity>
```

The application image that is shown in the launcher is embedded inside of the APK itself.  The expected file is in res/drawable-xhdpi/ouya_icon.png and the image size must be 732x412.

##### Cleanly shutting down audio

When you exit the app and/or open the system menu, any ongoing game audio should be paused or stopped. Your app is responsible for managing its own audio. Failing to do so may be grounds for review process failure.

When the system overlay menu opens (e.g. when the system button is held or double tapped) the framework broadcasts a sticky intent with the action `tv.ouya.intent.action.OUYA_MENU_APPEARING` (also defined in OuyaIntent). 
You can set up a BroadcastReceiver for this either dynamically or statically.

Statically:

```xml
<application>
    ...
    <receiver android:name=".MyBroadcastReceiver">
        <intent-filter>
            <action android:name="tv.ouya.intent.action.OUYA_MENU_APPEARING" />
        </intent-filter>
    </receiver>
    ...
</application>
```

```java
public class MyBroadcastReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        if(intent.getAction().equals(OuyaIntent.ACTION_MENUAPPEARING)) {
            //pause music, free up resources, etc.
        }
    }
}
```

Or dynamically:

```java
mContext.registerReceiver(new BroadcastReceiver{
    @Override
    public void onReceive(Context context, Intent intent) {
        //pause music, free up resources, etc.
    }
}, new IntentFilter(OuyaIntent.ACTION_MENUAPPEARING));
```

#### Hardware Substitutes

In order to begin developing software before having access to an OUYA console, you may use the Android emulator or a standard Android tablet.

##### Software

The OUYA console hardware already includes the OUYA launcher, but when using an emulator or Android tablet, you will need to install the launcher manually. This file is included in the OUYA ODK package.

To install the launcher, run:

    adb install -r ouya-framework.apk
    adb install -r ouya-launcher.apk

**Note**: If the OUYA launcher is not installed, some ODK features will not work correctly.

##### Emulator

If using the emulator, configure the Android Virtual Device (AVD) as follows:

- **Resolution**: 1920x1080 or 1280x720, as desired
- **Hardware Back/Home keys**: yes (you will need to add this to the hardware parameters)
- **D-Pad support**: yes (you will need to add this to the hardware parameters)
- **Target**: Android 4.1 - API Level 16
- **CPU/ABI**: Intel Atom x86
- **Device RAM size**: 1024

**Note**: If the **OK** button is disabled, you may need to install a system image (such as the Intel x86 Atom System Image) for your AVD. You can install it via the Android SDK Manager.

We recommend the use of the Intel Atom x86 CPU/ABI and Intel's HAXM extensions to ensure the emulator performance is adequate for game development. If you are developing low-level code, you should note that the device is ARM based. Therefore, you should develop for the ARM architecture and use an emulator AVD with the CPU/ABI set to an ARM architecture.

The OUYA console does not have hardware buttons for back or menu, so your games should not rely on the presence of these. Setting the hardware keys emulator property hides the Android navigation bar, enabling the emulator to fill the entire 1920x1080 or 1280x720 screen in the same way the OUYA console does.

**Note**: When developing with the emulator, it is not possible to fully emulate the OUYA controller buttons and features.

##### Android Tablet

If using a standard Android tablet, we recommend using a tablet with a usable display resolution as close as possible to 1920x1080 or 1280x720.

**Note**: The Android navigation bar will consume some of the screen on standard tablets, which will not be the case for the OUYA console.

The OUYA game controller combines a standard controller (two joysticks, a D-Pad, four game buttons, two shoulder buttons, and two triggers) with a touchpad. For testing, we recommend using the Xbox 360 wired USB controller combined with a mouse or touchpad for testing joystick and game button interaction.

##### Screen Resolution Handling

The OUYA console supports 720p or 1080p output only.

For OpenGL-based games, we recommend creating a render buffer of 1920x1080 (if targeting 1080p) or 1280x720 (if targeting 720p). If this does not match the display resolution of your device, the game screen may be windowed or scaled depending on the behavior determined by the device manufacturer.

For games using the Android UI Framework, follow the Android best practices for developing applications for xhdpi-large and tvdpi-large displays. See [developer.android.com](http://developer.android.com) for more information.
