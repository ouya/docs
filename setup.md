## Setup Instructions for the OUYA ODK

### Videos

<table border=1>

 <tr>

 <td>Windows Driver Setup (10:48)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=454JFvFTxww" target="_blank">
<img src="http://img.youtube.com/vi/454JFvFTxww/0.jpg" alt="Windows Driver Setup" width="240" height="180" border="10" /></a>
 </td>
 
  <td>Mac Driver Setup (5:57)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=5LSBiNfMq8A" target="_blank">
<img src="http://img.youtube.com/vi/5LSBiNfMq8A/0.jpg" alt="Mac Driver Setup" width="240" height="180" border="10" /></a>
 </td>
 
 </tr>
 
 <tr>

 <td>ElGato Capture Setup (4:03)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=-Igb4IcpbDY" target="_blank">
<img src="http://img.youtube.com/vi/-Igb4IcpbDY/0.jpg" alt="ElGato Capture Setup" width="240" height="180" border="10" /></a>
 </td>
 
  <td>Mac Overscan Setup (1:07)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=fwd2a26LOys" target="_blank">
<img src="http://img.youtube.com/vi/fwd2a26LOys/0.jpg" alt="Mac Overscan Setup" width="240" height="180" border="10" /></a>
 </td>
 
 </tr>
 
 <tr>

 <td>Video Editing in MS Movie (3:45)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=HI_ekZKfrd4" target="_blank">
<img src="http://img.youtube.com/vi/HI_ekZKfrd4/0.jpg" alt="Video Editing in MS Movie" width="240" height="180" border="10" /></a>
 </td>
 
  <td>Adding Video Screenshots (3:09)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=l8JRYvy-Gtc" target="_blank">
<img src="http://img.youtube.com/vi/l8JRYvy-Gtc/0.jpg" alt="Add Screenshots to Your Trailer" width="240" height="180" border="10" /></a>
 </td>
 
 </tr>
 
</table>

**Direct download links**:

**Known Issue**: Some game engines <a target=_blank href="http://forum.unity3d.com/threads/176737-unknown-error-when-building-simple-scene">[post]</a> still depend on an old version of the Android SDK Tools, so here is a direct link to the older version. In most cases you can install the latest version of the Android SDK. These are convienence links.<br/>
Android SDK Latest - http://developer.android.com/sdk/index.html<br/>
Android SDK Archived Rev 21 on Windows - http://dl.google.com/android/android-sdk_r21-windows.zip<br/>
Android SDK Archived Rev 21 on Mac - http://dl.google.com/android/android-sdk_r21-macosx.zip<br/>

ElGato Capture Software - http://www.elgato.com/gaming/game-capture-hd/support/update

##### Preliminary Instructions

To fully complete the setup, you will need:
* an OUYA
* a computer running Mac OS X or Windows
* a Micro USB to USB cable

Before continuing, make sure your OUYA is powered and **not connected to your computer**.

##### Mac OS X

1. Download and install the [Android SDK and tools](http://developer.android.com/sdk/index.html) to your Mac following the included instructions.

2. You will need to add some paths to PATH. Assuming you have put the SDK folder in the location `~/Development/adt-bundle-mac-x86_64`, open a terminal and add the following three lines to your `~/.bash_profile`:

        export PATH=$PATH:~/Development/adt-bundle-mac-x86_64/sdk/tools
        export PATH=$PATH:~/Development/adt-bundle-mac-x86_64/sdk/platform-tools
        export ANDROID_HOME=~/Development/adt-bundle-mac-x86_64/sdk

    You will need to adjust the above paths to match the name of your bundle directory.

    Make sure to `source ~/.bash_profile` once you've made these changes.

3. Launch the Android SDK Manager by running ([detailed instructions](http://developer.android.com/sdk/installing/adding-packages.html)):

        android sdk

    Install the following packages:

    *Note: Some of these packages may be pre-installed with the Android SDK and tools bundle.*  
    **Tools**: Android SDK and Android SDK Platform tools  
    **Android 4.1.2 (API 16)**: SDK Platform (without Google APIs)  
    **Extras**: Android Support Library  

    Install the Java runtime if prompted.  

4. Add the following line to `~/.android/adb_usb.ini` (create it if it doesn't exist) for your OUYA console to be recognised:

        0x2836

5. Connect your OUYA to your Mac (Micro USB to USB) then run the following commands:

        adb kill-server
        adb devices

    Your console should be shown in the list of available devices.

##### Windows

*Windows 8 users:* You will need to disable driver signature verification to install the unsigned Android driver. This involves restarting your PC so do this before you start. See [here](windows8.md) for step-by-step instructions.

1. Download and install the [Android SDK and tools](http://developer.android.com/sdk/index.html) to your PC following the included instructions.

2. Launch **SDK Manager.exe** in the ADT Bundle directory. ([detailed instructions](http://developer.android.com/sdk/installing/adding-packages.html)):

    Install the following packages:

    *Note: Some of these packages may be pre-installed with the Android SDK and tools bundle.*  
    **Tools**: Android SDK and Android SDK Platform tools  
    **Android 4.1.2 (API 16)**: SDK Platform (without Google APIs)  
    **Extras**: Android Support Library  

    Install the Java runtime if prompted.  
    
3. You will need to add some paths to PATH. Assuming you put the ADT bundle at `C:/Development/adt-bundle-windows-x86_64`,

    Open **My Computer**.  
    In the left panel, right-click on **Computer** and choose **Properties**.  
    In the left panel of the new window, choose **Advanced system settings**.  
    Click the button **Environment Variables...** in the new window.
    In the first table (User variables), highlight the row for the **Path** variable.  
    Click the **Edit...** button immediately below the User Variables table.  
    Append the following to the end of the **Variable value** field:  

        ;C:/Development/adt-bundle-windows-x86_64/sdk/tools;C:/Development/adt-bundle-windows-x86_64/sdk/platform-tools
        
    *Note #1: If the Path variable didn't already exist, remove the semi-colon at the beginning.*  
    *Note #2: You will need to adjust the above paths to match the name of your bundle directory.*  
    
    Press OK to save your changes.  
    Press OK to exit the Environment Variables window.  
    Press OK to exit the Advanced System Settings window.

4. Open the `ADT Bundle\sdk\extras\google\usb_driver\android_winusb.inf` file.

5. Add the following lines to the `[Google.NTx86]` and `[Google.NTamd64]` sections:  

        ;OUYA Console  
        %SingleAdbInterface% = USB_Install, USB\VID_2836&PID_0010  
        %CompositeAdbInterface% = USB_Install, USB\VID_2836&PID_0010&MI_01  

6. Connect your OUYA to your PC (Micro USB to USB), open Command Prompt (Win+R then type **cmd**), and run the following commands

        adb kill-server  
        echo 0x2836 >> "%USERPROFILE%\.android\adb_usb.ini"  
        adb start-server  

7. Open the Device Manager (Right-click My Computer->Properties->Device Manager)

8. In Device Manager, find **Portable Devices\OUYA Console**. 

9. Right-click and choose **Update Driver Software...**

10. Choose **Browse my computer for driver software** 

11. Choose **Let me pick from a list of device drivers on my computer**

12. On Windows 8, select **All devices** and click **Next**

13. Click **Have Disk** and browse to **Android Bundle\sdk\extras\google\usb_driver**

14. Choose ADB Composite Device
    *The Google device driver is not signed which prevents automatic from finding it.*

15. Accept the unsigned driver

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

In order to use the OUYA API you will need to include `ouya-sdk.jar` in your project libraries.  This can be found in the `libs` directory.

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

- **Resolution**: 1920x1080
- **Hardware Back/Home keys**: yes (you will need to add this to the hardware parameters)
- **D-Pad support**: yes (you will need to add this to the hardware parameters)
- **Target**: Android 4.1 - API Level 16
- **CPU/ABI**: Intel Atom x86
- **Device RAM size**: 1024

**Note**: If the **OK** button is disabled, you may need to install a system image for your AVD. The Intel x86 Atom System Image and the Intel x86 Emulator Accelerator (HAXM) are recommended. You can install it via the Android SDK Manager.

We recommend the use of the Intel Atom x86 CPU/ABI and Intel's HAXM extensions to ensure the emulator performance is adequate for game development. If you are developing low-level code, you should note that the device is ARM based. Therefore, you should develop for the ARM architecture and use an emulator AVD with the CPU/ABI set to an ARM architecture.

The OUYA console does not have hardware buttons for back or menu, so your games should not rely on the presence of these. Setting the hardware keys property of the emulator hides the Android navigation bar, enabling the emulator to fill a 1080p screen in the same way the OUYA console does.

**Note**: When developing with the emulator, it is not possible to fully emulate the OUYA controller buttons and features.

##### Android Tablet

If using a standard Android tablet, we recommend using a tablet with a usable display resolution as close as possible to 1920x1080.

**Note**: The Android navigation bar will consume some of the screen on standard tablets. An OUYA does not show a navigation bar on screen, so it is unlikely a tablet will give you a perfect replica of a running OUYA, but it will be enough to get most of your code running. You may still need to perform UI tweaks to use the 1920x1080 display buffer made available to apps by default on an OUYA.

The OUYA game controller combines a standard controller (two joysticks, a D-Pad, four game buttons, two shoulder buttons, and two triggers) with a touchpad. For testing, we recommend using the Xbox 360 wired USB controller combined with a mouse or touchpad for testing joystick and game button interaction.

##### Screen Resolution Handling

All applications are provided with a 1080p virtual display for their output. This virtual display is then rendered to the screen in an appropriate manner. This may include downscaling the 1080p display to 720p where the screen does not support 1080p, or to 480p if a screen does not report it supports 720p or 1080p via an HDMI connection.

For OpenGL-based games, we recommend creating a render buffer of 1920x1080 (i.e. 1080p) or 1280x720 (720p). If this does not match the display resolution of your device it will be up or downscaled accordingly by the OUYA. 

Other Android devices may handle resolutions differently, but our store QA process will always test on an OUYA, so it is advisable to test any applications on an OUYA before submitting them to the OUYA store.

For games using the Android UI Framework, the Android documentation from Google at [developer.android.com](http://developer.android.com) provides some useful tips when working with 1080p displays.

##### Video Capture

For capturing video of your game, we recommend using [Elgato](http://www.elgato.com/gaming/game-capture-hd/support/update). Here is a setup and tutorial [video](http://www.youtube.com/watch?v=-Igb4IcpbDY).
