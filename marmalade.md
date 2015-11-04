## Marmalade Engine

### Downloads

[Example Source](https://github.com/ouya/ouya-sdk-examples/tree/master/Marmalade)

### Forums

[Forge TV on Razer Forums](https://insider.razerzone.com/index.php?forums/razer-forge-tv.126/)

[Marmalade on OUYA Forums](http://forums.ouya.tv/categories/marmalade-on-ouya)

[Marmalade on MadeWithMarmalade Forums](https://developer.madewithmarmalade.com/develop/announce-and-discuss)

# Getting Started #

<table border=1>
 <tr>
 <td>Razer Marmalade Controller and IAP Support (8:09)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=HvU-eoLyLYg" target="_blank">
<img src="http://img.youtube.com/vi/HvU-eoLyLYg/0.jpg" alt="Razer Marmalade Controller and IAP Support" width="240" height="180" border="10" /></a>
 </td>
 <td></td>
 </tr>
</table>

### Resources

Marmalade - https://www.madewithmarmalade.com

Learn Marmalade - http://developer.madewithmarmalade.com/learn

Marmalade Docs - http://docs.madewithmarmalade.com/

Project Options - http://docs.madewithmarmalade.com/display/MD/MKB+build+and+project+options

Resource Management System - http://www.drmop.com/index.php/2011/10/01/marmalade-sdk-tutorial-marmalades-resource-management-system/

### Setup

Install <a target=_blank href="https://www.madewithmarmalade.com/free-trial">[Marmalade]</a>.

Activate the Marmalade HUB by entering your license data, which also sets up your development/build environment.

```
C:\Marmalade\7.8\tools\hub\hub.exe
```

Be sure to checkout the <a target=_blank href="https://github.com/ouya/ouya-sdk-examples/tree/master/Marmalade">[Marmalade ouya-sdk-examples]</a> from github.

#### Windows

Switch to your preferred Marmalade SDK version by running 's3eConfig.exe' found in the desired version 'C:\Marmalade\7.8\s3e\bin' folder.

```
C:\Marmalade\7.8\s3e\bin\s3eConfig.exe
```

Be sure to set the 'NDK_ROOT' in the My Computer->Properties->Advanced System Settings->Environment Variables.

Create a system variable to your NDK path:

```
NDK_ROOT=C:\NVPACK\android-ndk-r8e
```

To support compiling in Visual Studio, install the <a target=_blank href="https://developer.nvidia.com/tegra-resources">[Tegra Android Development Pack]</a> and join the <a target=_blank href="https://developer.nvidia.com/registered-developer-programs">[Tegra Registered Developer Program]</a> to get access to the download.

You may need to reboot for the environment changes to take effect.

* Note: In Marmalade projects if you ever see the compile error 'The builds tools for "v110_wp80" cannot be found', set the 'Platform Toolset' to 'Visual Studio 2012 (v110)'.

```
Open the project properties for the configurations

 GCC ARM Release/Debug and change under
 
 Configuration Properties->General->Platform Toolset
 
 to "Visual Studio 2012 (v110)"
```

* Note: If you find your binary resources not being compiled, set the target platform to 'GCC ARM Debug' and start the debugger. The 'data-ram/data-gles1' binary resources will be generated during the debug session. [Details](https://answers.madewithmarmalade.com/questions/16743/marmalade-bin-files-are-not-compiling.html)

* Note: If you have Visual Studio set to launch 'As Administrator', Marmalade won't be able to auto-launch Visual Studio.

* Note: Marmalade requires Android NDK 10 or better.

* Note: Marmalade requires JDK7 or better. Be sure to set the `JAVA_HOME` environment variable. A reboot will be required.

The examples can be compiled in Visual Studio.

### Marmalade ODK Extension

The Marmalade ODK Extension is a native wrapper around the ODK which makes the Java library accessible to Marmalade application code.

#### Building the Marmalade Extension

Eventually the Marmalade Extension will be part of the Marmalade SDK and building will not be necessary.

To build the extension from source switch to the extension folder 'Marmalade\MarmaladeODK' in the Marmalade examples.

##### Windows

In Windows explorer, right-click 'ODK.s4e' and click 'Build Android Extension' in the context menu.

Double-click 'ODK_android_java.mkb' to build the Java source.

Double-click 'ODK_android.mkb' to build the Native source.

### Using the Marmalade ODK Extension in an Application

Files with the MKB extension define the build and deploy behaviour for Marmalade applications.

Within your Marmalade application's MKB file, add a subproject for the Marmalade ODK extension.

To reference your local build of the application, specify the relative path the to the extension.

```
subprojects
{
	../MarmaladeODK/ODK
}
```

Your application needs to include the Marmalade ODK header to interface with the extension.

```
#include "ODK.h"
```

#### Initialization

Before calling the Marmalade ODK extension, an application should check if the extension is available.

```
	if (!ODKAvailable())
	{
		IwTrace(DEFAULT, ("Not running on OUYA, exit!"));
		return 0;
	}
```

If the Marmalade ODK extension is available, [initialize](#ouyaplugin_initouyaplugin) it with the developer id found in the [Cortex TV developer portal](http://devs.ouya.tv).

c++
```
void Application::InitOuyaPlugin()
{
	int jsonArray = OuyaPlugin_JSONArray_Construct();
	int index = 0;
	int jsonObject = OuyaPlugin_JSONObject_Construct();
	OuyaPlugin_JSONObject_Put(jsonObject, "key", "tv.ouya.developer_id");
	OuyaPlugin_JSONObject_Put(jsonObject, "value", "310a8f51-4d6e-4ae5-bda0-b93878e5f5d0");
	OuyaPlugin_JSONArray_Put(jsonArray, index, jsonObject);
	std::string jsonData = OuyaPlugin_JSONArray_ToString(jsonArray);
	OuyaPlugin_initOuyaPlugin(jsonData.c_str(),
	Application::m_ui.m_callbacksInitOuyaPlugin->GetSuccessEvent(),
	Application::m_ui.m_callbacksInitOuyaPlugin->GetFailureEvent());
}
```

## Xiaomi Initialization

[Back to general info](enable_xiaomi_support.md#xiaomi-initialization)

`OuyaPlugin_initOuyaPlugin` supports additional strings to make the game compatible with `Cortex TV` Everywhere devices.

* `tv.ouya.developer_id` - The developer UUID can be found in the [developer portal](http://devs.ouya.tv) after logging in.

* `com.xiaomi.app_id` - The Xiaomi App Id is provided by the content team, email `officehours@ouya.tv` to obtain your key.

* `com.xiaomi.app_key` - The Xiaomi App Key is provided by the content team, email `officehours@ouya.tv` to obtain your key.

* `tv.ouya.product_id_list` - The product id list is a comma separated list of product ids that can be purchased in the game.

c++
```
void Application::InitOuyaPlugin()
{
	int jsonArray = OuyaPlugin_JSONArray_Construct();
	

	int index = 0;
	
	int jsonObject = OuyaPlugin_JSONObject_Construct();
	OuyaPlugin_JSONObject_Put(jsonObject, "key", "tv.ouya.developer_id");
	OuyaPlugin_JSONObject_Put(jsonObject, "value", "00000000-0000-0000-0000-000000000000");
	OuyaPlugin_JSONArray_Put(jsonArray, index, jsonObject);
	++index;

	jsonObject = OuyaPlugin_JSONObject_Construct();
	OuyaPlugin_JSONObject_Put(jsonObject, "key", "com.xiaomi.app_id");
	OuyaPlugin_JSONObject_Put(jsonObject, "value", "0000000000000");
	OuyaPlugin_JSONArray_Put(jsonArray, index, jsonObject);
	++index;

	jsonObject = OuyaPlugin_JSONObject_Construct();
	OuyaPlugin_JSONObject_Put(jsonObject, "key", "com.xiaomi.app_key");
	OuyaPlugin_JSONObject_Put(jsonObject, "value", "000000000000000000");
	OuyaPlugin_JSONArray_Put(jsonArray, index, jsonObject);
	++index;

	jsonObject = OuyaPlugin_JSONObject_Construct();
	OuyaPlugin_JSONObject_Put(jsonObject, "key", "tv.ouya.product_id_list");
	OuyaPlugin_JSONObject_Put(jsonObject, "value", "long_sword,sharp_axe,cool_level,awesome_sauce");
	OuyaPlugin_JSONArray_Put(jsonArray, index, jsonObject);
	++index;

	std::string jsonData = OuyaPlugin_JSONArray_ToString(jsonArray);
	OuyaPlugin_initOuyaPlugin(jsonData.c_str(),
	Application::m_ui.m_callbacksInitOuyaPlugin->GetSuccessEvent(),
	Application::m_ui.m_callbacksInitOuyaPlugin->GetFailureEvent());
}
```

## Disable Xiaomi Screensaver

[Back to general info](enable_xiaomi_support.md#disable-xiaomi-screensaver)

* The screensaver should be disabled while your game is running. By using the OUYA-Marmalade plugin, the screensaver will automatically be disabled.

### Input ###

Input comes from the Marmalade ODK Extension. Include the ODK header to get access to the extension.

```
#include "ODK.h"
```

Since the application will be polling the Marmalade ODK extension for input, make sure your main loop is not yielding infinitely.

```
	// loop while application has not quit
	while (!s3eDeviceCheckQuitRequest())
	{	
		// handle drawing the ui and invoking in-app-purchases
		render();

		// handle polling the extension for controller input
		handleInput();

		// yield in the main loop to avoid killing the processor
		s3eDeviceYield(0);
	}
```

When handling input detecting button down and button released events, make sure you detect in a tight loop and then clear the button state afterward. You want to avoid taking time like doing rendering when detecting button up and down events as they might be missed.

```
void handleInput()
{
	for (int index = 0; index < OuyaController_MAX_CONTROLLERS; ++index)
	{
		m_controllers[index].HandleInput();
	}

	OuyaPlugin_clearButtonStates();
}
```

I.e. in the HandleInput method, here is a check for the Menu Button press.

The menu button sprite is highlighted for a second when detected.

```
void VirtualControllerSprite::HandleInput()
{
	if (GetButtonUp(OuyaController_BUTTON_MENU))
	{
		_timerMenuDetected = std::clock();
	}
}
```

## `OuyaPlugin_isPressed` ##

OuyaPlugin_isPressed returns if the button is either up or down and can be called at any time.

```
bool GetButton(int playerNum, int button)
{
	bool val = OuyaPlugin_isPressed(playerNum, button);
	return val;
}
```

## `OuyaPlugin_isPressedDown` ##

OuyaPlugin_isPressedDown returns true if a button was just pressed. Make sure to group pressed logic together and invoke `OuyaPlugin_clearButtonStates` afterward to avoid missing pressed events.

```
bool GetButtonDown(int playerNum, int button)
{
	bool val = OuyaPlugin_isPressedDown(playerNum, button);
	return val;
}
```

## `OuyaPlugin_isPressedUp` ##

OuyaPlugin_isPressedUp returns true if a button was just released. Make sure to group released logic together and invoke `OuyaPlugin_clearButtonStates` afterward to avoid missing released events.

```
bool GetButtonUp(int playerNum, int button)
{
	bool val = OuyaPlugin_isPressedUp(playerNum, button);
	return val;
}
```

## `OuyaPlugin_clearButtonStates` ##

After button down and up detection logic completes, clear the button states to avoid missing any pressed and released events. If any delays occur between checking pressed and released states before clearing the state, a window will open where input could be lost.

```
OuyaPlugin_clearButtonStates();
```

## `OuyaPlugin_getAxis` ##

Axis values return as an int and need to be cast to a float to get the expected in the -1 to 1 range.

```
float GetAxis(int playerNum, int axis)
{
	int val = OuyaPlugin_getAxis(playerNum, axis);
	float result = *(reinterpret_cast<float*>(&val));
	return result;
}
```

Axis constants

```
	  static const int AXIS_LS_X = 0;
	  static const int AXIS_LS_Y = 1;
	  static const int AXIS_RS_X = 11;
	  static const int AXIS_RS_Y = 14;
	  static const int AXIS_L2 = 17;
	  static const int AXIS_R2 = 18;
```

Button constants

```
	  static const int BUTTON_O = 96;
	  static const int BUTTON_U = 99;
	  static const int BUTTON_Y = 100;
	  static const int BUTTON_A = 97;
	  static const int BUTTON_L1 = 102;
	  static const int BUTTON_R1 = 103;
	  static const int BUTTON_MENU = 82;
	  static const int BUTTON_DPAD_UP = 19;
	  static const int BUTTON_DPAD_RIGHT = 22;
	  static const int BUTTON_DPAD_DOWN = 20;
	  static const int BUTTON_DPAD_LEFT = 21;
	  static const int BUTTON_R3 = 107;
	  static const int BUTTON_L3 = 106;
```

#### Callbacks

The in-app-purchase methods use callbacks to get data back from the Marmalade ODK extension which are compatible with the s3eCallback delegate.

The callbacks use an accessor to get the s3eCallback to make the design more obvious.

```
s3eCallback ClassName::GetAssessorEvent()
{
	return theDelegate;
}
```

The s3eCallback method has a specific interface that gets event data from the Marmalade ODK extension. 

The fields on the event contain data passed from the extension.

Special detail needs to be paid to the fields on the event.

The event field memory is allocated in the extension space, so be especially careful with any pointer data being sent.

The best practice is to copy the event data immediately after receiving the event.

```
	void MethodOnSuccess(dataForSuccessEvent* event)
	void MethodOnFailure(dataForFailureEvent* event)
	void MethodOnCancel(cancelEvent* event)
```

#### Callback Process

The callback process begins by first invoking methods in the Marmalade ODK extension.

Methods that retrieve information asynchronously need callbacks to return the data.

The supplied callbacks are invoked upon completion of the extension operation.

The success callback will be receiving the data being requested.

The failure callback will return if an error was detected.

The cancel callback will invoke if the user cancelled the operation.

## `OuyaPlugin_initOuyaPlugin` ##

The developer id from the [developer portal](http://devs.ouya.tv) needs to be prepared in a `JSONObject` to initialize the OUYA Plugin.

Avoid invoking IAP methods until the OuyaPlugin has been initialized.

The success event is invoked after IAP has been initialized.

The failure event happens if there was a problem. 

c++
```
	// construct the JSON Array
	int jsonArray = OuyaPlugin_JSONArray_Construct();

	// prepare the first JSON Object index
	int index = 0;

	// construct the JSON Object
	int jsonObject = OuyaPlugin_JSONObject_Construct();

	// add the developer id from the developer portal
	OuyaPlugin_JSONObject_Put(jsonObject, "key", "tv.ouya.developer_id");
	OuyaPlugin_JSONObject_Put(jsonObject, "value", "310a8f51-4d6e-4ae5-bda0-b93878e5f5d0");

	// assign the JSON Object to the first array element
	OuyaPlugin_JSONArray_Put(jsonArray, index, jsonObject);

	// convert the JSON Object to a string to send with the initialization plugin values
	std::string jsonData = OuyaPlugin_JSONArray_ToString(jsonArray);

	// initialize the OUYA plugin and wait for the callbacks before using the IAP API
	OuyaPlugin_initOuyaPlugin(jsonData.c_str(),
		Application::m_ui.m_callbacksInitOuyaPlugin->GetSuccessEvent(),
		Application::m_ui.m_callbacksInitOuyaPlugin->GetFailureEvent());
```

## `OuyaPlugin_asyncOuyaRequestGamerInfo` ##

GamerInfo provides access to the gamer username and uuid.

```
	class GamerInfo
	{
	public:
		std::string Username;
		std::string Uuid;
	};
```

The success event indicates success which also provides the gamer info.

The failure event indicates a problem with error code and error message.

The cancel event indicates the user cancelled the request.

Invoking request gamer info takes the success, failure, and cancel event callbacks.

```
	OuyaPlugin_asyncOuyaRequestGamerInfo(
		Application::m_ui.m_callbacksRequestGamerInfo->GetSuccessEvent(),
		Application::m_ui.m_callbacksRequestGamerInfo->GetFailureEvent(),
		Application::m_ui.m_callbacksRequestGamerInfo->GetCancelEvent());
```

When the Marmalade ODK has completed FetchGamerInfo, it invokes the Application callback for onSuccess, onFailure, or onCancel.

```
	void ApplicationCallbacksRequestGamerInfo::OnSuccess(const OuyaSDK::GamerInfo& gamerInfo)
	void ApplicationCallbacksRequestGamerInfo::OnFailure(int errorCode, const std::string& errorMessage)
	void ApplicationCallbacksRequestGamerInfo::OnCancel()
```

## `OuyaPlugin_asyncOuyaRequestProducts` ##

Pass an array of JSON to the Marmalade ODK Extension to get the details of the product list and invoke the callbacks upon completion.

```
	OuyaPlugin_asyncOuyaRequestProducts(productsJson.c_str(),
		Application::m_ui.m_callbacksRequestProducts->GetSuccessEvent(),
		Application::m_ui.m_callbacksRequestProducts->GetFailureEvent(),
		Application::m_ui.m_callbacksRequestProducts->GetCancelEvent());
```

The success callback passes the retrieved product list, which is passed to the UI.

```
void ApplicationCallbacksRequestProducts::OnSuccess(const std::vector<ApplicationProduct>& products)
```

The failure callback passes an error code and string message about the failure, which is passed to the UI.

```
void ApplicationCallbacksRequestProducts::OnFailure(int errorCode, const std::string& errorMessage)
```

The error callback indicates the user aborted the operation and the event name is passed to the UI.

```
void ApplicationCallbacksRequestProducts::OnCancel()
```

## `OuyaPlugin_asyncOuyaRequestPurchase` ##

Invoke the Marmalade ODK Extension to request purchase for the identifier and invoke the callbacks upon completion.

```
	OuyaPlugin_asyncOuyaRequestPurchase(product->Identifier.c_str(),
		Application::m_ui.m_callbacksRequestPurchase->GetSuccessEvent(),
		Application::m_ui.m_callbacksRequestPurchase->GetFailureEvent(),
		Application::m_ui.m_callbacksRequestPurchase->GetCancelEvent());
```

The success callback passes the purchased product details, which is passed to the UI.

```
void ApplicationCallbacksRequestPurchase::OnSuccess(const ApplicationProduct& product)
```

The failure callback passes an error code and string message about the failure, which is passed to the UI.

```
void ApplicationCallbacksRequestPurchase::OnFailure(int errorCode, const std::string& errorMessage)
```

The error callback indicates the user aborted the operation and the event name is passed to the UI.

```
void ApplicationCallbacksRequestPurchase::OnCancel()
```

## `OuyaPlugin_asyncOuyaRequestReceipts` ##

Invoke the Marmalade ODK Extension to request receipts and invoke the callbacks upon completion.

```
	OuyaPlugin_asyncOuyaRequestReceipts(
		Application::m_ui.m_callbacksRequestReceipts->GetSuccessEvent(),
		Application::m_ui.m_callbacksRequestReceipts->GetFailureEvent(),
		Application::m_ui.m_callbacksRequestReceipts->GetCancelEvent());
```

The success callback passes the retrieved receipts list, which is passed to the UI.

```
void ApplicationCallbacksRequestReceipts::OnSuccess(const std::vector<ApplicationReceipt>& receipts)
```

The failure callback passes an error code and string message about the failure, which is passed to the UI.

```
void ApplicationCallbacksRequestReceipts::OnFailure(int errorCode, const std::string& errorMessage)
```

The error callback indicates the user aborted the operation and the event name is passed to the UI.

```
void ApplicationCallbacksRequestReceipts::OnCancel()
```

### Examples

<a target=_blank href="https://github.com/ouya/ouya-sdk-examples/tree/master/Marmalade/MarmaladeODK"><b>Marmalade ODK Extension</b></a> - `Cortex TV` Controller and In-App-Purchase extension for Marmalade

### Virtual Controller ###

The [Virtual Controller](https://github.com/ouya/ouya-sdk-examples/tree/master/Marmalade/VirtualController) example shows 4 images of the OUYA Controller which moves axises and highlights buttons when the physical controller is manipulated. The Virtual Controller example includes support for [OUYA-Everywhere](ouya-everywhere.md).

![Virtual Controller](https://raw.githubusercontent.com/ouya/docs/master/marmalade/image_1.png)

### In-App-Purchases ###

The [In-App-Purchase](https://github.com/ouya/ouya-sdk-examples/tree/master/Marmalade/InAppPurchases) example uses the `Cortex TV` ODK to access gamer info, purchasing, and receipts.

![In-App-Purchases](https://raw.githubusercontent.com/ouya/docs/master/marmalade/image_2.png)

### In App Purchase Example

To build the example switch to the folder 'Marmalade\InAppPurchases' in the Marmalade examples.

##### Windows

Double-click 'InAppPurchases.mkb' which will open the project in Visual Studio.

In Visual Studio, set the build target to 'GCC ARM Debug' and build the project.

In Visual Studio, set the build target to 'GCC X86 ANDROID Release' and build and run which will launch the deploy tool.

The the debug dialog select build to deploy 'ARM GCC Debug' or 'ARM GCC Release' and hit 'Next Stage &gt;'.

The 'Default' configuration should be selected and hit 'Next Stage &gt;'.

Select the 'Android' platform and hit 'Next Stage &gt;'.

For the 'Do Action' pick 'Package, Install, and Run'.

Hit 'Deploy All' to build the Android package.

The deploy tool will generate an Android package.

If you want to manually deploy the Android package with `ADB` commands you'll be able to install on the `Forge TV`.

```
adb install -r the.apk
```

#### Project Files

InAppPurchases.mkb - The build and deployment configuration, double-click to open Visual Studio

app.icf - Override the default memory limits

Application.h/cpp - Holds an instance of the UI, meant to hold application variables

ApplicationCallbacksInitOuyaPlugin.h/cpp - Handles callbacks for success or failure events when initializing the `Cortex TV` plugin

ApplicationCallbacksRequestGamerInfo.h/cpp - Handles callbacks coming from extension to the application with the result of RequestGamerInfo

ApplicationCallbacksRequestProducts.h/cpp - Handles callbacks coming from extension to the application with the result of RequestProducts

ApplicationCallbacksRequestPurchase.h/cpp - Handles callbacks coming from extension to the application with the result of RequestPurchase

ApplicationCallbacksRequestReceipts.h/cpp - Handles callbacks coming from extension to the application with the result of RequestReceipts

ApplicationProduct.h/cpp - A container to hold the ODK products that's safe to access in the Application

ApplicationReceipt.h/cpp - A container to hold the ODK receipts that's safe to access in the Application

Main.cpp - The Application main loop

TextButton.h/cpp - A UI control for displaying text that pressing the 'O' button will fire an event

TextLabel.h/cpp - A UI control for displaying text

UI.h/cpp - Displays the user interface and handles invoking the IAP events here
