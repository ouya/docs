[Back to OUYA-Everywhere overview](../ouya-everywhere.md)


OUYA-Everywhere INPUT Documentation for the Unity Game Engine

# Audience #

This document is for Unity developers that want to import the OUYA SDK to access API functions and publish to the OUYA Android Console.

# Releases #

Download the OUYA Core package from from the [ouya-sdk-examples Unity releases](https://github.com/ouya/ouya-sdk-examples/releases/).

```
Note: Before importing ALWAYS make a backup of your game!
``` 

**Note:** Make a backup of your `signing key` in `Assets/Plugins/Android/assets/key.der` before importing the `OuyaSDK-Core.unitypackage`.

**Note:** Make a backup of your `icons` in `Assets/Plugins/Android/res/drawable/app_icon.png` and `Assets/Plugins/Android/res/drawable-xhdpi/ouya_icon.png` before importing the `OuyaSDK-Core.unitypackage`.

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

## Other Player Settings ##

In the Android `Player Settings` and within the `Other Settings` subgroup, here you can enter your package identifier from the [developer portal](http://devs.ouya.tv) into the `bundle id` field. Make sure the `minimum API level` field to 16.

![image alt text](image_28.png)

# Dependencies #

The OUYA Plugin has dependencies on the Android SDK for packaging and deploying Android applications.

The OUYA Plugin includes prebuilt Java and Native plugins, recompiling the Java and Native plugin is no longer required.

Android SDK - [http://developer.android.com/sdk/index.html?hl=sk](http://developer.android.com/sdk/index.html?hl=sk)

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

OuyaSDK.OuyaInput.GetAxis(playerNum, OuyaController.AXIS_LS_X)
OuyaSDK.OuyaInput.GetAxis(playerNum, OuyaController.AXIS_LS_Y)
OuyaSDK.OuyaInput.GetAxis(playerNum, OuyaController.AXIS_RS_X)
OuyaSDK.OuyaInput.GetAxis(playerNum, OuyaController.AXIS_RS_Y)
OuyaSDK.OuyaInput.GetAxis(playerNum, OuyaController.AXIS_L2)
OuyaSDK.OuyaInput.GetAxis(playerNum, OuyaController.AXIS_R2)

#endif

// @result - Returns the value of the axis without smoothing
float OuyaSDK.OuyaInput.GetAxisRaw(int playerNum, int axis);

#if UNITY_ANDROID && !UNITY_EDITOR

OuyaSDK.OuyaInput.GetAxisRaw(playerNum, OuyaController.AXIS_LS_X)
OuyaSDK.OuyaInput.GetAxisRaw(playerNum, OuyaController.AXIS_LS_Y)
OuyaSDK.OuyaInput.GetAxisRaw(playerNum, OuyaController.AXIS_RS_X)
OuyaSDK.OuyaInput.GetAxisRaw(playerNum, OuyaController.AXIS_RS_Y)
OuyaSDK.OuyaInput.GetAxisRaw(playerNum, OuyaController.AXIS_L2)
OuyaSDK.OuyaInput.GetAxisRaw(playerNum, OuyaController.AXIS_R2)

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

OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_O)
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_U)
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_Y)
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_A)
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_L1)
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_R1)
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_L3)
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_R3)
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_DPAD_UP)
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_DPAD_DOWN)
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_DPAD_RIGHT)
OuyaSDK.OuyaInput.GetButton(playerNum, OuyaController.BUTTON_DPAD_LEFT)

#endif

// @result - true if the button was in the DOWN position in the last frame
bool OuyaSDK.OuyaInput.GetButtonDown(int playerNum, int button);

#if UNITY_ANDROID && !UNITY_EDITOR

OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_O)
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_U)
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_Y)
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_A)
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_L1)
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_R1)
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_L3)
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_R3)
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_DPAD_UP)
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_DPAD_DOWN)
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_DPAD_RIGHT)
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_DPAD_LEFT)
OuyaSDK.OuyaInput.GetButtonDown(playerNum, OuyaController.BUTTON_MENU)

#endif

// @result - true if the button was in the UP position in the last frame
bool OuyaSDK.OuyaInput.GetButtonUp(int playerNum, int button);

#if UNITY_ANDROID && !UNITY_EDITOR

OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_O)
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_U)
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_Y)
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_A)
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_L1)
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_R1)
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_L3)
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_R3)
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_DPAD_UP)
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_DPAD_DOWN)
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_DPAD_RIGHT)
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_DPAD_LEFT)
OuyaSDK.OuyaInput.GetButtonUp(playerNum, OuyaController.BUTTON_MENU)

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
bool OuyaSDK.OuyaInput.IsControllerConnected(int playerNum);

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

## OUYA Panel ##

Open the `OUYA Panel` from the `Window->Open OUYA Panel` menu item.

![image alt text](image_7.png)

The `OUYA Panel` provides a quick way to switch between examples.

![image alt text](image_27.png)

Use the drop down to select the example and then click the `Switch to Example` button which updates the icons and package name.

# Virtual Controller Example #

The virtual controller example exercises the new OUYA-Everywhere input. The button names and images are now accessible from the API. And the virtual controller buttons highlight for multiple controllers for supported controllers. The right-hand JOY buttons toggle input for specific player numbers.

![image alt text](image_18.png)

The [Virtual Controller](https://github.com/ouya/ouya-sdk-examples/blob/master/Unity/OuyaSDK/Assets/Ouya/Examples/Scripts/VirtualController.cs) script displays a 2D controller with axis and buttons that highlight when the physical controller is used.

## Input Test ##

The [Input Test](https://github.com/ouya/ouya-sdk-examples/blob/master/Unity/OuyaSDK/Assets/Ouya/Examples/Scripts/OuyaInputTest.cs) script displays all the axis, button up, and button down states to verify that input is correctly setup.

![image alt text](image_26.png)

## In App Purchase Example ##

The `ShowProducts` scene is an in-app-purchase example that uses the `OuyaSDK` to access gamer info, purchasing, and receipts.

![image alt text](image_25.png)

## Safe Area Example ##

The Safe Area example uses the DPAD left and right to invoke `OuyaSDK.setSafeArea(float amount)`. Using 0.0 for the amount uses full border padding. Using 1.0 for the amount uses no border padding.

![image alt text](image_24.png)

<hr>

# Customization #

The OUYA Plugin remains customizable and if you want to extend the Java and Native plugins there are dependencies on the Android SDK, Android NDK, and Java JDK.

Android NDK - [https://developer.android.com/tools/sdk/ndk/index.html](https://developer.android.com/tools/sdk/ndk/index.html)

Java6 (32-bit/64-bit) - [http://www.oracle.com/technetwork/java/javasebusiness/downloads/java-archive-downloads-javase6-419409.html](http://www.oracle.com/technetwork/java/javasebusiness/downloads/java-archive-downloads-javase6-419409.html)

If you are on Windows if you install the [[Tegra Android Developer Pack]](https://developer.nvidia.com/tegra-resources), it comes with all of the needed dependencies and is the easiest way to set things up. Be sure to register for "NVIDIA GameWorks™ Registered Developer Program" to get access to the Tegra Android Developer Pack downloads.[ [Tegra Registered Developer Program]](https://developer.nvidia.com/registered-developer-programs)

<hr>

# Legacy (old files no longer needed) #

If you used previous versions of the OUYA Unity Plugin many of the old files are no longer needed and can be considered legacy.

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
