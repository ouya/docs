## Setup Instructions for the OUYA ODK

##### MacOS
Download and install the [Android SDK and tools](http://developer.android.com/sdk/index.html) to your Mac or PC, following the included instructions. 

When you reach the portion of the install process where you run Android SDK, install the following:

- **Tools**: Including both Android SDK and Android SDK Platform tools
- **Android 4.1(API 16)**: SDK Platform
- **Android 4.0 (API 14)**: SDK Platform
- **Extras**: Android Support Library

**NOTE**: To launch the Android SDK, you may need to use the following command:
```bash 
./android sdk
```

After running the SDK Manager tool, install the Java runtime if you are prompted to do so.

You will need to add some paths to PATH. Assuming you have put the SDK folder in the location `~/android/android-sdk-macosx`, open a terminal and add the following three lines to your `.bashrc`:

```bash
export PATH=$PATH:~/android/android-sdk-macosx/tools
export PATH=$PATH:~/android/android-sdk-macosx/platform-tools
export ANDROID_HOME=~/android/android-sdk-macosx
```

You may need to adjust your `.bashrc` entries if you have used a custom SDK folder location.


##### Windows
TBD

##### Eclipse
TBD

##### Developing with the ODK
Use the Android API Level 16 (Android 4.1 "Jelly Bean") when developing for the OUYA Console.

In order to use the OUYA API you will need to include `ouya-sdk.jar` in your project libraries, as well as `guava-r09.jar` and `commons-lang-2.6.jar`. These can be found in the `libs` directory. 

For information on the API commands available, please consult the OUYA API reference documentation.

To run the sample code, open up the project in `iap-sample-app` and follow the instructions in the `README.txt` file.

For your application or game to be recognized as made for OUYA, you will need to include an OUYA intent category on the manifest entry of your main activity. 
Use “ouya.intent.category.GAME” or “ouya.intent.category.APP”.

```xml
<activity android:name=".GameActivity" android:label="@string/app_name">
  <intent-filter>
    <action android:name="android.intent.action.MAIN"/>
    <category android:name="android.intent.category.LAUNCHER"/>
    <category android:name="ouya.intent.category.GAME"/>
  </intent-filter>
</activity>
```

The application image that is shown in the launcher is embedded inside of the APK itself.  The expected file is in `res/drawable-xhdpi/ouya_icon.png` and the image size must be 732x412 for games or 412x412 for applications.

#### Hardware Substitutes

In order to begin developing software before having access to an OUYA console, you may use the Android emulator or a standard Android tablet.

##### Software

The OUYA console hardware already includes the OUYA launcher, but when using an emulator or Android tablet, you will need to install the launcher manually. This file is included in the OUYA ODK package.

To install the launcher, run:
```bash
adb install -r ouya-framework.apk
adb install -r ouya-launcher.apk
```

**Note**: If the OUYA launcher is not installed, some ODK features will not work correctly.

##### Emulator

If using the emulator, configure the Android virtual device as follows:

- **Resolution**: 1920x1080 or 1280x720, as desired
- **Hardware Back/Home keys**: yes (you will need to add this to the hardware parameters)
- **D-Pad support**: yes (you will need to add this to the hardware parameters)
- **Target**: Android 4.1 - API Level 16
- **CPU/ABI**: Intel Atom x86
- **Device RAM size**: 1024

We recommend the use of the Intel Atom x86 CPU/ABI and Intel's HAXM extensions to ensure the emulator performance is adequate for game development. If you are developing low-level code, you should note that the device is ARM based. Therefore, you should develop for the ARM architecture and use an emulator AVD with the CPU/ABI set to an ARM architecture.

The OUYA console does not have hardware buttons for back or menu, so your games should not rely on the presence of these. Setting the hardware keys emulator property hides the Android navigation bar, enabling the emulator to fill the entire 1920x1080 or 1280x720 screen in the same way the OUYA console does.

**Note**: When developing with the emulator, it is not possible to fully emulate the OUYA controller buttons and features. 

##### Android Tablet

If using a standard Android tablet, we recommend using a tablet with a usable display resolution as close as possible to 1920x1080 or 1280x720. 

**Note**: The Android navigation bar will consume some of the screen on standard tablets, which will not be the case for the OUYA console.

The OUYA game controller combines a standard controller (two joysticks, a D-Pad, four game buttons, two shoulder buttons, and two triggers) with a touchpad. For testing, we recommend using the Xbox 360 wired USB controller combined with a mouse or touchpad for testing joystick and game button interaction. 

##### Screen Resolution Handing

The OUYA console supports 720p or 1080p output only.

For OpenGL-based games, we recommend creating a render buffer of 1920x1080 (if targeting 1080p) or 1280x720 (if targeting 720p). If this does not match the display resolution of your device, the game screen may be windowed or scaled depending on the behavior determined by the device manufacturer.

For games using the Android UI Framework, follow the Android best practices for developing applications for xhdpi-large and tvdpi-large displays. See [developer.android.com](http://developer.android.com) for more information.
