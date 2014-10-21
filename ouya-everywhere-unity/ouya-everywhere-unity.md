[Back to OUYA-Everywhere overview](../ouya-everywhere.md)


OUYA-Everywhere INPUT Documentation for the Unity Game Engine

# Audience #

This document is for Unity developers that want to import the OUYA SDK to access API functions and publish to the OUYA Android Console.

# Releases #

Download the OUYA Core package from from the [ouya-sdk-examples Unity releases](https://github.com/ouya/ouya-sdk-examples/releases/).

```
Note: Before importing ALWAYS make a backup of your game!
``` 

# Source #

The source code for OUYA-Everywhere with Unity can be found within the [ouya-sdk-examples for Unity](https://github.com/ouya/ouya-sdk-examples/tree/master/Unity/OuyaSDK).

# Overview #

This document covers importing the core package, installing dependencies, building, and publishing your game to the OUYA.

# Intro #

The ouya-core.unitypackage contains a static access class for accessing input and the OUYA SDK API for the Unity game engine. The input API makes it possible to build your game and without needing to rebuild will automatically add future support for new controllers and devices while still correctly mapping for your game. The input API also adds new features like being able to consistently know which controller maps to a player number. And if a controller disconnects and reconnects it will maintain the same player number. The input API makes it possible to detect if a controller has been disconnected.

This input API is targeted at the OUYA Android Console and associated devices and is not maintained as a cross-platform input system.

# Setup #

Open your game or a new project.

Note: Make sure that your project path does not contain spaces in order to be compatible with the NDK compiler.

![image alt text](image_0.png)

Import the Core package. From the menu item Assets->Import Package->Custom Package.

![image alt text](image_1.png)

## `OuyaSDK-Core.unitypackage` ##

Import the OuyaSDK-Core.unitypackage.

`Ouya\SDK\Editor\OuyaMenuAdmin.cs` - Adds OUYA menu items for exporting packages for release

`Ouya\SDK\Editor\OuyaPanel.cs` - Provides example switcher to auto change package name and icons

`Ouya\SDK\Prefabs\OuyaGameObject.prefab` - Add the prefab to the initial scene for apps/games to enable the OUYA Plugin

`Ouya\SDK\Scripts\OuyaGameObject.cs` - Handles communication with the OUYA Plugin between C#, C++, and Java

![image alt text](image_21.png)

### Android Customization ###

`Plugins\Android\AndroidManifest.xml` - Defines the package identifier and start activity.

`Plugins\Android\assets\key.der` - The signing key from the developer portal

`Plugins\Android\libs\armeabi-v7a\lib-ouya-ndk.so` - Prebuilt native library for the OUYA Plugin

`Plugins\Android\libs\armeabi\lib-ouya-ndk.so` - Prebuilt native library for the OUYA Plugin

`Plugins\Android\libs\x86\lib-ouya-ndk.so` - Prebuilt native library for the OUYA Plugin

`Plugins\Android\libs\ouya-sdk.jar` - The OUYA SDK Java library

`Plugins\Android\OuyaUnityPlugin.jar` - Prebuit OUYA Unity Plugin Java library

`Plugins\Android\res\raw\drawable-xhdpi\ouya_icon.png` - The 732x412 OUYA Store icon

`Plugins\Android\res\raw\drawable\app_icon.png` - The 96x96 settings icon

![image alt text](image_22.png)

### Plugin Scripts ###

Files within `Plugins` make scripts available to `C#` and `JavaScript` developers.

`Plugins\JSONArray.cs` - JNI hooks for using Android JSON Array parsing

`Plugins\JSONObject.cs` - JNI hooks for using Android JSON Object parsing

`Plugins\OuyaController.cs` - JNI hooks for interacting with the OUYA Controller

`Plugins\OuyaSDK.cs` - The OUYA Plugin SDK methods for input and in-app-purchases

![image alt text](image_23.png)

## Icons ##

On the first import you'll get the sample icons.
```
Assets/Plugins/Android/res/drawable-xhdpi/ouya_icon.png (732x412).
Assets/Plugins/Android/res/drawable/app_icon.png (96x96).
```

If the icons have already been customized, there's no need to import the icons and replace with the sample icons.

## Signing key ##

Importing the signing key is a placeholder for where to place the signing key downloaded from the developer portal. The signing key is used in in-app-purchase encryption and decryption. The in-app-purchase API will not work until you [create a game in the developer portal](https://devs.ouya.tv/developers/games) to download the game's signing key. Be cautious when importing updates to not replace this file.

Note (ODK 1.0.14.1): The signing key was moved to be compatible with 3rd party plugins.

```
Assets/Plugins/Android/assets/key.der
```

## Orientation ##

Within the Player Settings, Android Tab, set the default orientation to Landscape Left.

![image alt text](image_19.png)

## OuyaGameObject.cs ##

Add the OuyaGameObject to your initial loading scene. It uses DontDestroyOnLoad so you only want one instance of the OuyaGameObject. It handles communication between Java to C#. Using the inspector set your developer id.

![image alt text](image_20.png)

# Dependencies #

The OUYA Plugin has dependencies on the Android SDK, Android NDK, and Java JDK.

Android SDK - [http://developer.android.com/sdk/index.html?hl=sk](http://developer.android.com/sdk/index.html?hl=sk)

Android NDK - [https://developer.android.com/tools/sdk/ndk/index.html](https://developer.android.com/tools/sdk/ndk/index.html)

Java6 (32-bit) - [http://www.oracle.com/technetwork/java/javasebusiness/downloads/java-archive-downloads-javase6-419409.html](http://www.oracle.com/technetwork/java/javasebusiness/downloads/java-archive-downloads-javase6-419409.html)

If you are on Windows if you install the [[Tegra Android Developer Pack]](https://developer.nvidia.com/tegra-resources), it comes with all of the needed dependencies and is the easiest way to set things up. Be sure to register for "NVIDIA GameWorks™ Registered Developer Program" to get access to the Tegra Android Developer Pack downloads.[ [Tegra Registered Developer Program]](https://developer.nvidia.com/registered-developer-programs)

Launch the SDK Manager within the Android SDK folder.

![image alt text](image_2.png)

If you upgrade your Android SDK Platform-tools to 19 also be sure to install the Android SDK Build-tools as some tools have moved around.

![image alt text](image_3.png)

Install the SDK Platform for API 16.

![image alt text](image_4.png)

Any updates to the Android SDK will require to re-edit the adb_usb.ini to add 0x2836 so that the OUYA can be recognized over micro-usb cable.

![image alt text](image_5.png)

Make sure there’s no white space or empty lines after the last entry in the adb_usb.ini file.

![image alt text](image_6.png)

The OUYA Panel via the menu item Window->Open OUYA Panel.

![image alt text](image_7.png)

On the Unity tab make sure that the Unity JAR is found and nothing is greyed out. Since you are running the Unity editor to access this panel it’s highly unlikely things would be grayed out unless there was a problem during install. In which case, reinstall your version of Unity.

![image alt text](image_8.png)

On the Java tab make sure the path to your Java JDK6 1.6 (32-bit) is found. Nothing should be grayed out. You may need to Select the SDK Path and browse to where you’ve installing the JDK. The tools may appear gray if the JDK cannot be found or if you are not pointed at the 32-bit version of JDK 6.

![image alt text](image_9.png)

If you don’t have the JDK there’s a button that will take you to download the Oracle website. Sorry you have to create a login to download Java. Make sure it’s Java 6 in the older versions and that it is 32-bit.

On the Android SDK tab, make sure that nothing is grayed out. You can set the Android API-16 level in the player settings. You may need to browse to where you’ve downloaded the Android SDK. You may need to run the Android SDK in order to install the SDK platform-16.

![image alt text](image_10.png)

On the Android NDK tab, make sure nothing is grayed out. NDK is required this time to be setup. Click the download link if you don’t have the Android NDK.

![image alt text](image_11.png)

The OUYA menu still has the export options from the previous version. And Core is the only package that you need to get your game up and running.

![image alt text](image_12.png)

This time when you import core, it’s optional to import the ProjectSettings\InputManager.asset as the new input completely bypasses the Unity Input API. You might want these settings for the non-Android platforms.

FAQ: All the other ProjectSettings that are part of the original project that was opened are just the defaults. There’s no need to copy these to your game project and you probably shouldn’t.

FAQ: Which input system should I use. The legacy input system is still useful for non-OUYA platforms. [https://github.com/ouya/ouya-unity-plugin](https://github.com/ouya/ouya-unity-plugin) Where the new OUYA-Everywhere input system is great for the feature that you don’t need to rebuild your game to support new devices and controllers for the Android platform. The new OUYA-Everywhere input system keeps your game compatible with all officially supported OUYA devices in the future. The new OUYA-Everywhere input system is cleaner too. There’s no longer the need to have a massive C# script with tons of switch blocks and controller mappings. OUYA support handles adding support for new controllers so you don’t have to.

FAQ: Is there more info on non-OUYA platforms? Talking about Mac, Windows, Linux, XBOX, etc, you are free to use whatever input system you want to use. The new OuyaSDK.OuyaInput APIs are great for the Android platform and making everything appear as an OUYA controller. On the non-OUYA side there are projects dedicated to supporting hundreds of controllers on Mac, Windows, Linux and those are worth checking out. I say that because OUYA support is our primary focus and so that’s what this library is targeted at.

Okay back on the OUYA-Everywhere input.

## Compile NDK ##

First you want to click the Compile NDK button.

* Note: Avoid spaces in the project path as it can cause issues with building NDK.

If everything is working okay your console output in the editor should look like this.

![image alt text](image_13.png)

The expected output in the console log should read:

```
[Results] elapsedTime: 1.995114 errors: 
output: [armeabi-v7a] Compile++ arm  : -ouya-ndk <= jni.cpp
[armeabi-v7a] SharedLibrary  : lib-ouya-ndk.so
[armeabi-v7a] Install        : lib-ouya-ndk.so => libs/armeabi-v7a/lib-ouya-ndk.so

UnityEngine.Debug:Log(Object)
OuyaPanel:RunProcess(List`1, String, String, String, String&, String&, String) (at Assets/Ouya/SDK/Editor/OuyaPanel.cs:2801)
OuyaPanel:RunProcess(List`1, String, String, String, String&, String&) (at Assets/Ouya/SDK/Editor/OuyaPanel.cs:2760)
OuyaPanel:RunProcess(List`1, String, String) (at Assets/Ouya/SDK/Editor/OuyaPanel.cs:2723)
OuyaPanel:CompileNDK() (at Assets/Ouya/SDK/Editor/OuyaPanel.cs:507)
OuyaPanel:Update() (at Assets/Ouya/SDK/Editor/OuyaPanel.cs:883)
UnityEditor.EditorApplication:Internal_CallUpdateFunctions()
```

## Main Activity ##

The next key things are in the OUYA Tab, make sure you’ve set a unique bundle identifier. And your main activity should be "MainActivity".

![image alt text](image_14.png)

## Sync Bundle ID ##

When you change the bundle identifier, you’ll see a popup. Where you just hit the "Sync Bundle Id" to make the android manifest and package name match. Keep in mind this is case sensitive and needs to match the bundle id that you will use in the developer portal when submitting your game. Case matters.

![image alt text](image_15.png)

After syncing the bundle id, now the error warning should disappear.

## Compile Plugin ##

Click the Compile Plugin button and that will compile the Java plugin. If successful you should see the editor console log print signatures for the compiled Java classes.

![image alt text](image_16.png)

The expected output in the console log should show the main activity signature:

```
MainActivity
[Results] elapsedTime: 0.1450083 errors: 
output: Compiled from "MainActivity.java"
public class tv.ouya.demo.SceneShowUnityInput.MainActivity extends tv.ouya.sdk.OuyaUnityActivity{
public tv.ouya.demo.SceneShowUnityInput.MainActivity();
  Signature: ()V
protected void onCreate(android.os.Bundle);
  Signature: (Landroid/os/Bundle;)V
public void onStart();
  Signature: ()V
public void onStop();
  Signature: ()V
protected void onActivityResult(int, int, android.content.Intent);
  Signature: (IILandroid/content/Intent;)V
protected void onSaveInstanceState(android.os.Bundle);
  Signature: (Landroid/os/Bundle;)V
protected void onDestroy();
  Signature: ()V
public void onResume();
  Signature: ()V
public void onPause();
  Signature: ()V
public void onConfigurationChanged(android.content.res.Configuration);
  Signature: (Landroid/content/res/Configuration;)V
public void onWindowFocusChanged(boolean);
  Signature: (Z)V
static java.lang.Boolean access$000(tv.ouya.demo.SceneShowUnityInput.MainActivity);
  Signature: (Ltv/ouya/demo/SceneShowUnityInput/MainActivity;)Ljava/lang/Boolean;
}


UnityEngine.Debug:Log(Object)
OuyaPanel:RunProcess(List`1, String, String, String, String&, String&, String) (at Assets/Ouya/SDK/Editor/OuyaPanel.cs:2807)
OuyaPanel:RunProcess(List`1, String, String, String, String) (at Assets/Ouya/SDK/Editor/OuyaPanel.cs:2749)
OuyaPanel:RunProcess(String, String, String, String) (at Assets/Ouya/SDK/Editor/OuyaPanel.cs:2742)
OuyaMenuAdmin:BuildApplicationJar() (at Assets/Ouya/SDK/Editor/OuyaMenuAdmin.cs:338)
OuyaMenuAdmin:MenuGeneratePluginJar() (at Assets/Ouya/SDK/Editor/OuyaMenuAdmin.cs:127)
OuyaPanel:Update() (at Assets/Ouya/SDK/Editor/OuyaPanel.cs:894)
UnityEditor.EditorApplication:Internal_CallUpdateFunctions()
```

# Legacy #

Some legacy files are not required to be imported or can be removed.

## Legacy InputManager.asset (Removed) ##

The InputManager Mappings file is no longer used by the plugin. So you may choose to keep your existing mapping file which is used for non-OUYA platforms.

```
ProjectSettings/InputManager.asset
```

## Legacy OuyaExampleCommon.cs (Removed) ##

The legacy OuyaExampleCommon.cs script can been removed or replaced with another input system.

```
Assets/Plugins/OuyaExampleCommon.cs
```

There are a number of related legacy input C# scripts that should be removed.

```
Assets/Plugins/IOuyaController.cs
Assets/Plugins/OuyaControllerCommon.cs
Assets/Plugins/OuyaExampleCommon.cs
Assets/Plugins/OuyaKeyCodes.cs
Assets/Plugins/PS2Controller.cs
Assets/Plugins/XBox360Controller.cs
```

## Legacy OuyaPostProcessor.cs (Removed) ##

The OuyaPostProcessor would auto compile C++ and Java source after a detected change. However, changes to the plugin are infrequent enough making this feature not used and so it was removed. Typically you only need to compile NDK and the Java Plugin after importing an update of the plugin.

```
Assets/Ouya/SDK/Editor/OuyaPostProcessor.cs
```

## Legacy OuyaUnityApplication.jar (Removed) ##

If you have the legacy OuyaUnityApplication.jar file make sure that it’s removed. If you forget this step, you’ll get an DEX error when building the game.

```
Assets\Plugins\Android\OuyaUnityApplication.jar
```

## Legacy OuyaUnityApplication.java (Removed) ##

If you have the legacy OuyaUnityApplication.java file make sure that it’s removed. If you forget this step, you’ll get a Java compile error.

```
Assets\Plugins\Android\src\OuyaUnityApplication.java
```

## Legacy OuyaNativeActivity.java (Removed) ##

If you have the legacy OuyaNativeActivity.java file make sure that it’s removed. If you forget this step, you’ll get a Java compile error.

```
Assets\Plugins\Android\src\OuyaNativeActivity.java
```

## Legacy R.java (Removed) ##

The Core Unity Package may include example R.java files that should not be imported into your game. You may want to delete these extra files if they were imported.

Note (ODK 1.0.14.1): R.java and resources were moved to be compatible with other 3rd party plugins that also supplied this generated file.  

```
Assets\Plugins\Android\src\tv\...\R.java
```

## Legacy Litjson (Removed) ##

Litjson is a public domain 3rd party library for parsing JSON data. Android already has classes for handling JSONObject parsing and so the legacy Litjson was replaced.

```
Assets\Litjson
```

# OUYA Panel #

Unity Free or Unity Pro

If you have the free version you’ll have to use the menu File->Build Settings or File->Build and Run.

If you have the pro version, you can hit the Build and Run Application button in the OUYA Panel.

FAQ: If you have the Android free version you won’t be able to use pro features like render textures.

The Run Application and Stop Application are convenient for relaunching or exiting your application while it’s running on the OUYA.

# OUYA Everywhere API #

Be sure to be on the Android platform before invoking the OUYA Everywhere API.

```
#if UNITY_ANDROID && !UNITY_EDITOR

	... make OUYA Everywhere API calls ...

#else

	... make calls to your preferred non-Android input system... (Linux/Mac/Windows/etc) 

#endif
```

## Initialization ##

Make sure that before invoking other OuyaSDK methods than isIAPInitComplete returns true. This gives time for the Java to initialize before accessing the controller, button names, button images, products, purchase, receipts, and toggling cursor visibility.

```
// returns true when the Java in-app-purchase system has initialized.
// return false when in-app-purchase calls should not be invoked 
// bool OuyaSDK.isIAPInitComplete();

#if UNITY_ANDROID && !UNITY_EDITOR

void Start()
{
	while (!OuyaSDK.isIAPInitComplete())
	{
		yield return null;
	}
}

#endif
```

# Accessing OuyaController #

The namespace has to be added to find the OuyaController.

C#
```
#if UNITY_ANDROID && !UNITY_EDITOR
using tv.ouya.console.api;
#endif
```

# Accessing Button Names #

OuyaController has a static method to retrieve button names. A null button name means the button was not found.  

C#
```
#if UNITY_ANDROID && !UNITY_EDITOR

OuyaController.ButtonData buttonData;
buttonData = OuyaController.getButtonData(OuyaController.BUTTON_O);
if (null == buttonData)
{
	return;
}
if (null == buttonData.buttonName)
{
	return;
}
string buttonName = buttonData.buttonName;

#endif
```

# Accessing Button Images #

OuyaController has a static method to retrieve button images as Texture2D images. A null Texture2D image means the button was not found.

C#
```
#if UNITY_ANDROID && !UNITY_EDITOR

Texture2D buttonTexture = null;
OuyaController.ButtonData buttonData;
buttonData = OuyaController.getButtonData(OuyaController.BUTTON_O);
if (null == buttonData)
{
	return;
}
if (null == buttonData.buttonDrawable)
{
	return;
}
BitmapDrawable drawable = (BitmapDrawable)buttonData.buttonDrawable;
if (null == drawable)
{
	return;
}
Bitmap bitmap = drawable.getBitmap();
if (null == bitmap)
{
	return;
}
ByteArrayOutputStream stream = new ByteArrayOutputStream();
bitmap.compress(Bitmap.CompressFormat.PNG, 100, stream);
if (stream.size() > 0)
{
	buttonTexture = new Texture2D(0, 0);
	buttonTexture.LoadImage(stream.toByteArray());
}
stream.close();

#endif
```

# Accessing Axis Values #

C#
```
// PlayerNm is zero based and must be less than OuyaController.MAX_CONTROLLERS.
// GetAxis, GetAxisRaw expects the following axis values:
OuyaController.AXIS_LS_X
OuyaController.AXIS_LS_Y
OuyaController.AXIS_RS_X
OuyaController.AXIS_RS_Y
OuyaController.AXIS_L2
OuyaController.AXIS_R2

int playerNum = 0; //zero based

// @result - Returns the value of the axis with smoothing
float OuyaSDK.OuyaInput.GetAxis(int playerNum, int axis);

#if UNITY_ANDROID && !UNITY_EDITOR

OuyaSDK.OuyaInput.GetAxis(playerNum, OuyaController.AXIS_LS_X);
OuyaSDK.OuyaInput.GetAxis(playerNum, OuyaController.AXIS_LS_Y);
OuyaSDK.OuyaInput.GetAxis(playerNum, OuyaController.AXIS_RS_X);
OuyaSDK.OuyaInput.GetAxis(playerNum, OuyaController.AXIS_RS_Y);
OuyaSDK.OuyaInput.GetAxis(playerNum, OuyaController.AXIS_L2);
OuyaSDK.OuyaInput.GetAxis(playerNum, OuyaController.AXIS_R2);

#endif

// @result - Returns the value of the axis without smoothing
float OuyaSDK.OuyaInput.GetAxisRaw(int playerNum, int axis);

#if UNITY_ANDROID && !UNITY_EDITOR

OuyaSDK.OuyaInput.GetAxisRaw(playerNum, OuyaController.AXIS_LS_X);
OuyaSDK.OuyaInput.GetAxisRaw(playerNum, OuyaController.AXIS_LS_Y);
OuyaSDK.OuyaInput.GetAxisRaw(playerNum, OuyaController.AXIS_RS_X);
OuyaSDK.OuyaInput.GetAxisRaw(playerNum, OuyaController.AXIS_RS_Y);
OuyaSDK.OuyaInput.GetAxisRaw(playerNum, OuyaController.AXIS_L2);
OuyaSDK.OuyaInput.GetAxisRaw(playerNum, OuyaController.AXIS_R2);

#endif
```

# Accessing Button States #

C#
```
// GetButton, GetButtonDown, GetButtonUp expect the following button values:
OuyaController.BUTTON_O
OuyaController.BUTTON_U
OuyaController.BUTTON_Y
OuyaController.BUTTON_A
OuyaController.BUTTON_L1
OuyaController.BUTTON_R1
OuyaController.BUTTON_L3
OuyaController.BUTTON_R3
OuyaController.BUTTON_DPAD_UP
OuyaController.BUTTON_DPAD_DOWN
OuyaController.BUTTON_DPAD_RIGHT
OuyaController.BUTTON_DPAD_LEFT
OuyaController.BUTTON_MENU

// @result - true when the button is in the DOWN position
// @result - false when the button is in the UP position
bool OuyaSDK.OuyaInput.GetButton(int playerNum, int keyCode);

#if UNITY_ANDROID && !UNITY_EDITOR

OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_O);
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_U);
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_Y);
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_A);
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_L1);
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_R1);
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_L3);
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_R3);
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_DPAD_UP);
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_DPAD_DOWN);
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_DPAD_RIGHT);
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_DPAD_LEFT);

#endif

// @result - true if the button was in the DOWN position in the last frame
bool OuyaSDK.OuyaInput.GetButtonDown(int playerNum, int button);

#if UNITY_ANDROID && !UNITY_EDITOR

OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_O);
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_U);
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_Y);
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_A);
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_L1);
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_R1);
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_L3);
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_R3);
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_DPAD_UP);
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_DPAD_DOWN);
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_DPAD_RIGHT);
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_DPAD_LEFT);
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_MENU);

#endif

// @result - true if the button was in the UP position in the last frame
bool OuyaSDK.OuyaInput.GetButtonUp(int playerNum, int button);

#if UNITY_ANDROID && !UNITY_EDITOR

OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_O);
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_U);
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_Y);
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_A);
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_L1);
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_R1);
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_L3);
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_R3);
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_DPAD_UP);
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_DPAD_DOWN);
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_DPAD_RIGHT);
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_DPAD_LEFT);
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_MENU);

#endif
```

## OuyaController.BUTTON_MENU ##

Note: Be sure to check for button states with GetButtonDown and GetButtonUp. Just checking for the GetButton state will never catch the event in time because the down and up event fire within the same frame.

# Check if controller is connected #

OuyaInput exposes a static method to check if the controller is connected.

C#
```
#if UNITY_ANDROID && !UNITY_EDITOR

//@returns true if the player number is connected
//@returns false if the player number is disconnected
//bool OuyaSDK.OuyaInput.IsControllerConnected(int playerNum);

#endif
```

# Hide the mouse cursor #

In some cases you may want to hide or show the mouse cursor. The showCursor static method on OuyaController toggles cursor visibility.

C#
```
#if UNITY_ANDROID && !UNITY_EDITOR

// Hide the mouse cursor
OuyaController.showCursor(false);

// Show the mouse cursor
OuyaController.showCursor(true);

#endif
```

# Examples #

Download the Examples package from github releases…

Import the Examples package. From the menu item Assets->Import Package->Custom Package… and browse to the "ouya-examples.unitypackage".

![image alt text](image_17.png)

# Virtual Controller Example #

The virtual controller example exercises the new OUYA-Everywhere input. The button names and images are now accessible from the API. And the virtual controller buttons highlight for multiple controllers for supported controllers. The right-hand JOY buttons toggle input for specific player numbers.

![image alt text](image_18.png)

## Virtual Controller Example ##

The [Virtual Controller](https://github.com/ouya/ouya-sdk-examples/blob/master/Unity/OuyaSDK/Assets/Ouya/Examples/Scripts/VirtualController.cs) script displays a 2D controller with axis and buttons that highlight when the physical controller is used.

## Input Test ##

The [Input Test](https://github.com/ouya/ouya-sdk-examples/blob/master/Unity/OuyaSDK/Assets/Ouya/Examples/Scripts/OuyaInputTest.cs) script displays all the axis, button up, and button down states to verify that input is correctly setup.
