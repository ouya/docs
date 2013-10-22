## Marmalade

### Downloads
Open source, clone https://github.com/ouya/ouya-sdk-examples/tree/master/Marmalade

### Forums

@OUYA - (Marmalade on OUYA Forums) - http://forums.ouya.tv/categories/marmalade-on-ouya<br/>

## Guide

### Examples

<a target=_blank href="https://github.com/ouya/ouya-sdk-examples/tree/master/Marmalade/VirtualController"><b>Virtual Controller</b></a> - Maps OUYA controllers to multiple virtual controllers

<a target=_blank href="https://github.com/ouya/ouya-sdk-examples/tree/master/Marmalade/MarmaladeODK"><b>Marmalade ODK Extension</b></a> - OUYA Controller and In-App-Purchase extension for Marmalade

<a target=_blank href="https://github.com/ouya/ouya-sdk-examples/tree/master/Marmalade/InAppPurchases"><b>InAppPurchases</b></a> - In-App-Purchase uses the ODK extension

### Resources

@Marmalade - (Forums) - https://developer.madewithmarmalade.com/develop/announce-and-discuss

Marmalade - https://www.madewithmarmalade.com

Learn Marmalade - http://developer.madewithmarmalade.com/learn

Marmalade Docs - http://docs.madewithmarmalade.com/

Project Options - http://docs.madewithmarmalade.com/display/MD/MKB+build+and+project+options

Resource Management System - http://www.drmop.com/index.php/2011/10/01/marmalade-sdk-tutorial-marmalades-resource-management-system/

### Setup

Install <a target=_blank href="https://www.madewithmarmalade.com/free-trial">[Marmalade]</a>.

Activate the Marmalade HUB by entering your license data, which also sets up your development/build environment.

Be sure to checkout the <a target=_blank href="https://github.com/ouya/ouya-sdk-examples/tree/master/Marmalade">[Marmalade ouya-sdk-examples]</a> from github.

#### Windows

Switch to your preferred Marmalade SDK version by running 's3eConfig.exe' found in the desired version 'C:\Marmalade\7.0\s3e\bin' folder.

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

If the Marmalade ODK extension is available, initialize it with the developer id found in the OUYA developer portal.

```
OuyaPlugin_asyncSetDeveloperId("310a8f51-4d6e-4ae5-bda0-b93878e5f5d0");
```

Since the application will be polling the Marmalade ODK extension for input, make sure your main loop is not yielding infinitely.

```
	// loop while application has not quit
	while (!s3eDeviceCheckQuitRequest())
	{
		// handle polling the extension for controller input
		handleInput();
		
		// handle drawing the ui and invoking in-app-purchases
		render();

		// yield in the main loop to avoid killing the processor
		s3eDeviceYield(0);
	}
```

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

If you want to manually deploy the Android package with adb commands you'll be able to install on the OUYA.

```
adb install -r the.apk
```

#### Project Files

InAppPurchases.mkb - The build and deployment configuration, double-click to open Visual Studio

app.icf - Override the default memory limits

Application.h/cpp - Holds an instance of the UI, meant to hold application variables

ApplicationCallbacksFetchGamerUUID.h/cpp - Handles callbacks coming from extension to the application with the result of FetchGamerUUID

ApplicationCallbacksRequestProducts.h/cpp - Handles callbacks coming from extension to the application with the result of RequestProducts

ApplicationCallbacksRequestPurchase.h/cpp - Handles callbacks coming from extension to the application with the result of RequestPurchase

ApplicationCallbacksRequestReceipts.h/cpp - Handles callbacks coming from extension to the application with the result of RequestReceipts

ApplicationProduct.h/cpp - A container to hold the ODK products that's safe to access in the Application

ApplicationReceipt.h/cpp - A container to hold the ODK receipts that's safe to access in the Application

Main.cpp - The Application main loop

TextButton.h/cpp - A UI control for displaying text that pressing the 'O' button will fire an event

TextLabel.h/cpp - A UI control for displaying text

UI.h/cpp - Displays the user interface and handles invoking the IAP events here

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

Methods that retrieve information asyncronously need callbacks to return the data.

The supplied callbacks are invoked upon completion of the extension operation.

The success callback will be receiving the data being requested.

The failure callback will return if an error was detected.

The cancel callback will invoke if the user cancelled the operation.

'OuyaPlugin_asyncOuyaFetchGamerUUID' invokes in the extension in a separate memory space.

```
OuyaPlugin_asyncOuyaFetchGamerUUID(
	Application::m_ui.m_callbacksFetchGamerUUID->GetSuccessEvent(),
	Application::m_ui.m_callbacksFetchGamerUUID->GetFailureEvent(),
	Application::m_ui.m_callbacksFetchGamerUUID->GetCancelEvent());
```

The success, failure, and cancel events have accessors for the s3eCallback callbacks.

```
s3eCallback ApplicationCallbacksFetchGamerUUID::GetSuccessEvent()
s3eCallback ApplicationCallbacksFetchGamerUUID::GetFailureEvent()
s3eCallback ApplicationCallbacksFetchGamerUUID::GetCancelEvent()
```

When the Marmalade ODK has completed FetchGamerUUID, it invokes the Application callback for onSuccess, onFailure, or onCancel.

The callback method receives an event which holds data that needs to be copied to application memory.

```
void FetchGamerUuidOnSuccess(s3eFetchGamerUuidSuccessEvent* event)
void FetchGamerUuidOnFailure(s3eFetchGamerUuidFailureEvent* event)
void FetchGamerUuidOnCancel(s3eFetchGamerUuidCancelEvent* event)
```

The original call to Invoke Fetch GamerUUID passed 3 potential callbacks. The resulting callback is invoked after the data is copied.

```
void ApplicationCallbacksFetchGamerUUID::OnSuccess(const std::string& gamerUUID)
void ApplicationCallbacksFetchGamerUUID::OnFailure(int errorCode, const std::string& errorMessage)
void ApplicationCallbacksFetchGamerUUID::OnCancel()
```

When the gamerUUID string is received, it's then passed to the UI to display.

The UI also has a message field which displays the callback details.

```
Application::m_ui.SetMessage("ApplicationCallbacksFetchGamerUUID::OnCancel");
```

#### Fetch Gamer UUID

Invokes the Marmalade ODK Extension to get the Gamer UUID and invokes the respective callback on completion.

```
OuyaPlugin_asyncOuyaFetchGamerUUID(
	Application::m_ui.m_callbacksFetchGamerUUID->GetSuccessEvent(),
	Application::m_ui.m_callbacksFetchGamerUUID->GetFailureEvent(),
	Application::m_ui.m_callbacksFetchGamerUUID->GetCancelEvent());
```

The success callback passes the retrieved gamerUUID, which is passed to the UI.

```
void ApplicationCallbacksFetchGamerUUID::OnSuccess(const std::string& gamerUUID)
```

The failure callback passes an error code and string message about the failure, which is passed to the UI.

```
void ApplicationCallbacksFetchGamerUUID::OnFailure(int errorCode, const std::string& errorMessage)
```

The error callback indicates the user aborted the operation and the event name is passed to the UI.

```
void ApplicationCallbacksFetchGamerUUID::OnCancel()
```

#### Request Products

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

#### Request Purchase

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

#### Request Receipts

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
